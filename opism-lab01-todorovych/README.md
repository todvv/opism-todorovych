# Лабораторна робота. DNS

| Поле | Значення |
|---|---|
| ПІБ | Тодорович В. В. |
| Група | КБЗІ-2.02 |
| Ідентифікатор (`sNN`) | cs-202-25 |
| Дата виконання | 02.11.26 |
| Середовище | GitHub Codespaces |
| Версія `dig` | DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu |

## Параметри варіанта

| Параметр | Значення |
|---|---|
| `$TTL`, с | 90 |
| TTL запису SOA, с | 15 |
| Поле `minimum` SOA, с | 40 |
| Псевдонім (CNAME на `www`) | news |
| Другий хост і його TTL, с | preview, 35 |
| Пріоритет MX | 15 |
| Токен TXT | bc96aae3 |
| Ім'я для завдання 5 | new-ngsyx.cs-202-25.lab |

## Завдання 1. Складові URI

### 1.1. Розбір

| № | scheme | userinfo | host | port | path | query | fragment |
|---|---|---|---|---|---|---|---|
| 1 | http:// | — | www.cs-202-25.lab | :80 (за замовч.) | /users/archive | ?lang=20&from=uk | #summary |
| 2 | http:// | — | news.cs-202-25.lab | :80 | /archive/ | ?lang=%D0%BA%D0%B8%D1%97%D0%B2 | — |
| 3 | http:// | user:pass@ | preview.cs-202-25.lab | :80 (за замовч.) | /users | — | #top |
| 4 | https:// | — | www.cs-202-25.lab | :9443 | /users/../archive | — | — |
| 5 | mailto: | — | — | Відсутній? Оскільки не є транспортним протоколом | admin@cs-202-25.lab | ?subject=users | — |

| № | Ім'я, що запитується в DNS | Заголовок `Host` | Ціль запиту (request target) |
|---|---|---|---|
| 1 | www.cs-202-25.lab | www.cs-202-25.lab | /users/archive?lang=20&from=uk |
| 2 | news.cs-202-25.lab | news.cs-202-25.lab:80 | /archive/?lang=%D0%BA%D0%B8%D1%97%D0%B2 |
| 3 | preview.cs-202-25.lab | preview.cs-202-25.lab | /users |
| 4 | www.cs-202-25.lab | www.cs-202-25.lab:9443 | /users/../archive |
| 5 | — | — | — |

Декодоване значення параметра URI 2: %D0%BA = к %D0%B8 = и %D1%97 = ї %D0%B2 = в; Київ

Чому URI 5 не має складової host, хоча містить символ `@`: mailto не є звичайним посиланням на сторінку, це формат, що при натисканні переходить до дефолт застосунку електронної пошти, створює новий лист та в поле "кому" вводить отримувача (path), можливо встановлює інші значення, як назву листа (query, ?subject=...). В цьому URI @ є частиною адреси електроної пошти, а не роздільником між userinfo i host.

### 1.2. Перевірка (після завдання 3)

```
> GET /users/archive?lang=20 HTTP/1.1
> Host: www.cs-202-25.lab
> User-Agent: curl/8.5.0
> Accept: */*
>
server: web-a
Host: www.cs-202-25.lab
request URI: /users/archive?lang=20
path: /users/archive
query: lang=20
client: 10.53.1.1

> GET /archive/?lang=%D0%BA%D0%B8%D1%97%D0%B2 HTTP/1.1
> Host: news.cs-202-25.lab
> User-Agent: curl/8.5.0
> Accept: */*
>
server: web-a
Host: news.cs-202-25.lab
request URI: /archive/?lang=%D0%BA%D0%B8%D1%97%D0%B2
path: /archive/
query: lang=%D0%BA%D0%B8%D1%97%D0%B2
client: 10.53.1.1

> GET /users HTTP/1.1
> Host: preview.cs-202-25.lab
> Authorization: Basic dXNlcjpwYXNz
> User-Agent: curl/8.5.0
> Accept: */*
>
server: web-b
Host: preview.cs-202-25.lab
request URI: /users
path: /users
query: 
client: 10.53.1.1
```

Значення заголовка `Authorization` і результат декодування:

```
> Authorization: Basic dXNlcjpwYXNz

root@ns-cs-202-25:/# echo dXNlcjpwYXNz | base64 -d
user:pass
```

URI 4: етап, на якому запит не вдається (пошук імені / TCP / TLS), і рядки виводу, що це підтверджують:

```text
TCP

*   Trying 10.53.0.80:9443...
* connect to 10.53.0.80 port 9443 from 10.53.1.1 port 35850 failed: Connection refused
* Failed to connect to www.cs-202-25.lab port 9443 after 2 ms: Couldn't connect to server
```

| Запит | `request URI`, отриманий сервером | `path`, отриманий сервером |
|---|---|---|
| звичайний | /archive | /archive |
| `--path-as-is` | /users/../archive | /archive |

Розбіжності між URI та тим, що отримав сервер, і їх пояснення: у першому випадку утиліта самостійно перетворює шлях на стороні клієнта, ще до того як запит відправляється. У наступному випадку утиліта отримує чітку інструкцію не модифікувати, тоді поле request URI залишається у невідредактованому виді. Однак сам сервер вже не має цієї інструкції та перетворює шлях на простий /archive.

## Завдання 2. Проходження ієрархії вручну

```
root@ns-cs-202-25:/# dig @10.53.0.2 ns.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.2 ns.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29444
;; flags: qr; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 92a0b908ac141728010000006ae8cb04998bcec2410257af (good)
;; QUESTION SECTION:
;ns.cs-202-25.lab.              IN      A

;; AUTHORITY SECTION:
lab.                    172800  IN      NS      ns1.nic.lab.

;; ADDITIONAL SECTION:
ns1.nic.lab.            172800  IN      A       10.53.0.3

;; Query time: 81 msec
;; SERVER: 10.53.0.2#53(10.53.0.2) (UDP)
;; WHEN: Mon Nov 02 17:40:20 UTC 2026
;; MSG SIZE  rcvd: 117

root@ns-cs-202-25:/# dig @10.53.0.3 ns.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.3 ns.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2673
;; flags: qr; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 7372fc10981af727010000006ae8cb112079f03a6b078f9a (good)
;; QUESTION SECTION:
;ns.cs-202-25.lab.              IN      A

;; AUTHORITY SECTION:
cs-202-25.lab.          3600    IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       3600    IN      A       10.53.1.1

;; Query time: 81 msec
;; SERVER: 10.53.0.3#53(10.53.0.3) (UDP)
;; WHEN: Mon Nov 02 17:40:33 UTC 2026
;; MSG SIZE  rcvd: 103

root@ns-cs-202-25:/# dig @10.53.1.1 ns.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 ns.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53791
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 0facc4b37b5ec4ab010000006ae8cb2171edcb5f23b80874 (good)
;; QUESTION SECTION:
;ns.cs-202-25.lab.              IN      A

;; ANSWER SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; AUTHORITY SECTION:
cs-202-25.lab.          3600    IN      NS      ns.cs-202-25.lab.

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 17:40:49 UTC 2026
;; MSG SIZE  rcvd: 103

```

| Сервер | status | flags | ANSWER | AUTHORITY | ADDITIONAL |
|---|---|---|---|---|---|
| 10.53.0.2 | NOERROR | qr | 0 | 1 (lab.                    172800  IN      NS      ns1.nic.lab.) | 2 (ns1.nic.lab.            172800  IN      A       10.53.0.3) |
| 10.53.0.3 | NOERROR | qr | 0 | 1 (cs-202-25.lab.          3600    IN      NS      ns.cs-202-25.lab.) | 2 (ns.cs-202-25.lab.       3600    IN      A       10.53.1.1) |
| 10.53.1.1 | NOERROR | qr aa | 1 (ns.cs-202-25.lab.       90      IN      A       10.53.1.1) | 1 (cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.) | 1 |

Вивід `dig ns.sNN.lab A +trace`:

```
root@ns-cs-202-25:/# dig ns.cs-202-25.lab A +trace

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> ns.cs-202-25.lab A +trace
;; global options: +cmd
.                       86289   IN      NS      a.root-servers.net.
;; Received 59 bytes from 10.53.0.53#53(10.53.0.53) in 0 ms

lab.                    172800  IN      NS      ns1.nic.lab.
;; Received 117 bytes from 10.53.0.2#53(a.root-servers.net) in 81 ms

cs-202-25.lab.          3600    IN      NS      ns.cs-202-25.lab.
;; Received 103 bytes from 10.53.0.3#53(ns1.nic.lab) in 40 ms

ns.cs-202-25.lab.       90      IN      A       10.53.1.1
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.
;; Received 103 bytes from 10.53.1.1#53(ns.cs-202-25.lab) in 0 ms

```

| Запит | TTL запису NS | Прапорець `aa` | Секція, у якій міститься NS |
|---|---|---|---|
| `dig @10.53.0.3 sNN.lab NS +norec` | 3600 | - | QUESTION, AUTHORITY |
| `dig @10.53.1.1 sNN.lab NS +norec` | 90 | + | QUESTION, ANSWER |

1. Які відповіді є відсиланнями і за якими ознаками це визначено?<br>
Відповіді від root та TLD є відсиланнями. Це можна визначити за: відсутності прапорців aa (authoritative answer); відсутності відповіді та відповідної секції; секція ADDITIONAL містить адреси на наступні вузли.

2. Для чого у відповіді TLD присутній запис у секції ADDITIONAL і що сталося б, якби його не було?<br>
Запис допомагає рекурсивному резолверу одразу дізнатися наступний сервер. При відсутності запису резолвер дізнається назву серверів, не включаючи їх адреси, що змушує його виконувати додаткові запити. Більше запитів призводить до довшого отримання потрібної відповіді резолвером.

3. Яка з двох копій запису NS є авторитативною і чому?<br>
Друга, копія мого серверу. Оскільки присутній прапорець aa та відповідь разом з відповідною до неї секції.

## Завдання 3. Заповнення власної зони

Вивід `zone-reload`:

```
root@ns-cs-202-25:/# zone-reload
zone cs-202-25.lab/IN: loaded serial 2026110223
OK
zone reload queued
```

Вивід шости запитів:
```
root@ns-cs-202-25:/# dig @10.53.1.1 www.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 www.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50960
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 3137daee93fcd799010000006ae8e6548fcaba6479fa4367 (good)
;; QUESTION SECTION:
;www.cs-202-25.lab.             IN      A

;; ANSWER SECTION:
www.cs-202-25.lab.      90      IN      A       10.53.0.80

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:36:52 UTC 2026
;; MSG SIZE  rcvd: 123

root@ns-cs-202-25:/# dig @10.53.1.1 news.cs-202-25.lab CNAME +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 news.cs-202-25.lab CNAME +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 6498
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 0bb795e65ec01caf010000006ae8e6685f7c647abdb1286e (good)
;; QUESTION SECTION:
;news.cs-202-25.lab.            IN      CNAME

;; ANSWER SECTION:
news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:37:12 UTC 2026
;; MSG SIZE  rcvd: 126

root@ns-cs-202-25:/# dig @10.53.1.1 mail.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 mail.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36091
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 1f844feba4359570010000006ae8e6755179aefecc2e64bf (good)
;; QUESTION SECTION:
;mail.cs-202-25.lab.            IN      A

;; ANSWER SECTION:
mail.cs-202-25.lab.     90      IN      A       10.53.0.80

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 1 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:37:25 UTC 2026
;; MSG SIZE  rcvd: 124

root@ns-cs-202-25:/# dig @10.53.1.1 cs-202-25.lab MX +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 cs-202-25.lab MX +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12035
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: efbf69f5d0beb218010000006ae8e67f15f8b6ac25d4e38a (good)
;; QUESTION SECTION:
;cs-202-25.lab.                 IN      MX

;; ANSWER SECTION:
cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab.

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
mail.cs-202-25.lab.     90      IN      A       10.53.0.80
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 1 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:37:35 UTC 2026
;; MSG SIZE  rcvd: 140

root@ns-cs-202-25:/# dig @10.53.1.1 cs-202-25.lab TXT +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 cs-202-25.lab TXT +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12503
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: e7e8bcdd51ad55df010000006ae8e68b5e4c2240830d4c99 (good)
;; QUESTION SECTION:
;cs-202-25.lab.                 IN      TXT

;; ANSWER SECTION:
cs-202-25.lab.          90      IN      TXT     "lab-token=bc96aae3"

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:37:47 UTC 2026
;; MSG SIZE  rcvd: 134

root@ns-cs-202-25:/# dig @10.53.1.1 preview.cs-202-25.lab A +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 preview.cs-202-25.lab A +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2186
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: e4725d92362308bb010000006ae8e69b8bf33fe5682a72de (good)
;; QUESTION SECTION:
;preview.cs-202-25.lab.         IN      A

;; ANSWER SECTION:
preview.cs-202-25.lab.  35      IN      A       10.53.0.81

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:38:03 UTC 2026
;; MSG SIZE  rcvd: 127

```

| Запис | status | Прапорець `aa` | Значення в ANSWER | TTL | Відповідає картці (так/ні) |
|---|---|---|---|---|---|
| `www` A | NOERROR | + | www.cs-202-25.lab.      90      IN      A       10.53.0.80 | 90 | + |
| псевдонім CNAME | NOERROR | + | news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab. | 90 | + |
| `mail` A | NOERROR | + | mail.cs-202-25.lab.     90      IN      A       10.53.0.80 | 90 | + |
| `@` MX | NOERROR | + | cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab. | 90 | + |
| `@` TXT | NOERROR | + | cs-202-25.lab.          90      IN      TXT     "lab-token=bc96aae3" | 90 | + |
| другий хост A | NOERROR | + | preview.cs-202-25.lab.  35      IN      A       10.53.0.81 | 35 | + |

Порівняння відповідей резолвера та власного сервера:

```
root@ns-cs-202-25:/# dig news.cs-202-25.lab

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> news.cs-202-25.lab
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26521
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;news.cs-202-25.lab.            IN      A

;; ANSWER SECTION:
news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.
www.cs-202-25.lab.      90      IN      A       10.53.0.80

;; Query time: 40 msec
;; SERVER: 10.53.0.53#53(10.53.0.53) (UDP)
;; WHEN: Mon Nov 02 19:44:00 UTC 2026
;; MSG SIZE  rcvd: 81

root@ns-cs-202-25:/# dig @10.53.1.1 news.cs-202-25.lab +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 news.cs-202-25.lab +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 14821
;; flags: qr aa; QUERY: 1, ANSWER: 2, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 9c35cd4c7c636169010000006ae8e80fe2444cc132cad04c (good)
;; QUESTION SECTION:
;news.cs-202-25.lab.            IN      A

;; ANSWER SECTION:
news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.
www.cs-202-25.lab.      90      IN      A       10.53.0.80

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:44:15 UTC 2026
;; MSG SIZE  rcvd: 142

root@ns-cs-202-25:/# dig cs-202-25.lab MX

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> cs-202-25.lab MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 10366
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;cs-202-25.lab.                 IN      MX

;; ANSWER SECTION:
cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab.

;; Query time: 0 msec
;; SERVER: 10.53.0.53#53(10.53.0.53) (UDP)
;; WHEN: Mon Nov 02 19:44:28 UTC 2026
;; MSG SIZE  rcvd: 63

root@ns-cs-202-25:/# dig @10.53.1.1 cs-202-25.lab MX +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 cs-202-25.lab MX +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53892
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 549bc8cfe26d09ef010000006ae8e82c1c8f572cff70f714 (good)
;; QUESTION SECTION:
;cs-202-25.lab.                 IN      MX

;; ANSWER SECTION:
cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab.

;; AUTHORITY SECTION:
cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.

;; ADDITIONAL SECTION:
mail.cs-202-25.lab.     90      IN      A       10.53.0.80
ns.cs-202-25.lab.       90      IN      A       10.53.1.1

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 19:44:44 UTC 2026
;; MSG SIZE  rcvd: 140

```

| Запит | flags | ANSWER (кількість, типи) | AUTHORITY (кількість, типи) | ADDITIONAL (кількість, типи) | TTL |
|---|---|---|---|---|---|
| псевдонім через resolver-a | qr rd ra | 2 (news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.<br>www.cs-202-25.lab.      90      IN      A       10.53.0.80) | 0 | 1 | 90 |
| псевдонім через 10.53.1.1 | qr aa | 2 (news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.<br>www.cs-202-25.lab.      90      IN      A       10.53.0.80) | 1 (cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.) | 2 (ns.cs-202-25.lab.       90      IN      A       10.53.1.1) | 90 |
| MX через resolver-a | qr rd ra | 1 (cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab.) | 0 | 1 | 90 |
| MX через 10.53.1.1 | qr aa | 1 (cs-202-25.lab.          90      IN      MX      15 mail.cs-202-25.lab.) | 1 (cs-202-25.lab.          90      IN      NS      ns.cs-202-25.lab.) | 3 (mail.cs-202-25.lab.     90      IN      A       10.53.0.80<br>ns.cs-202-25.lab.       90      IN      A       10.53.1.1) | 90 |

Пояснення відмінностей: одразу можна помітити, що не всі відповіді резолверу мали флажок aa, коли всі відповіді власного сервера мали даний флажок. Рекурсивні відповіді мають декілька відповідей чи можливо додаткову інформацію з секції ADDITIONAL. Власний сервер надал інформацію про домен, що ближче до авторитетної, а резолвер надає менш службової інформації.

Повідомлення `zone-check` після тимчасового додавання `@ IN CNAME www`:

```
root@ns-cs-202-25:/# zone-check
dns_master_load: /lab/zone/cs-202-25.lab.zone:21: cs-202-25.lab: CNAME and other data
zone cs-202-25.lab/IN: loading from master file /lab/zone/cs-202-25.lab.zone failed: CNAME and other data
zone cs-202-25.lab/IN: not loaded due to errors.
```

1. Який TTL має кожен запис зони і звідки це значення взято?<br>
90 як TTL за замовченням для всіх записів, що не мають вказаного TTL. 35 для запису preview. Також є запис @ IN SOA, що містить TTL 15. Значення 90 та 15 знаходяться на початку зони, 35 разом з записом preview.

2. Скільки записів містить секція ANSWER у відповіді на запит псевдоніма і чому?<br>
Резолвер на обидва запитів з псевдоніму надає дві відповіді.
```
news.cs-202-25.lab.     90      IN      CNAME   www.cs-202-25.lab.
www.cs-202-25.lab.      90      IN      A       10.53.0.80
```
Перший надає псевдонім, та що він повертає. Другий запит допомогає одразу знайти IP адреси, яку перетворено з псевдоніму. Рекурсивний резолвер робить це для того, щоб користувач не виконував ще один DNS-запит, щоб знайти адресу www.cs-202-25.lab. Секція ANSWER у відпвіді на запит псевдоніма мого сервера надає лише один запис, оскільки це виконується без допомоги резолверу.

3. Чому запис CNAME неможливо розмістити для імені `@`?<br>
CNAME використовується лише для створення унікальних записів. @ використовується і на початку зони і для інших записів знизу, тому @ та CNAME не змогли співіснувати.

## Завдання 4. TTL і видимість змін

Два запити з інтервалом 5 с:

```
root@ns-cs-202-25:/# dig www.cs-202-25.lab +noall +answer
www.cs-202-25.lab.      90      IN      A       10.53.0.80
root@ns-cs-202-25:/# dig www.cs-202-25.lab +noall +answer
www.cs-202-25.lab.      83      IN      A       10.53.0.80
```

**Прогноз** (записати до вимірювання). Формула часу появи нової адреси через залишок R, найбільша можлива затримка:<br>T = R. Tmax = TTLпочатковий.

Перевірка, що сервер до `zone-reload` повертає стару адресу:

```
root@ns-cs-202-25:/# dig @10.53.1.1 www.cs-202-25.lab +norec +short
10.53.0.80
```

Залишок R і час (крок 4), час `zone-reload` (крок 5), перевірка на сервері (крок 6):

```
root@ns-cs-202-25:/# date +%T; dig www.cs-202-25.lab +noall +answer
22:32:47
www.cs-202-25.lab.      90      IN      A       10.53.0.80
root@ns-cs-202-25:/# zone-reload; date +%T
zone cs-202-25.lab/IN: loaded serial 2026110278
OK
zone reload queued
22:32:55
root@ns-cs-202-25:/# dig @10.53.1.1 www.cs-202-25.lab +norec +short
10.53.0.81
```

Очікуваний час появи нової адреси (підставити R у формулу): T = R = 90-8 = 82 c

Вивід циклу спостереження (перші два рядки, два останні рядки зі старою адресою, рядок з новою адресою):

```
root@ns-cs-202-25:/# while true; do echo "$(date +%T) $(dig +short www.cs-202-25.lab)"; sleep 2; done
22:33:12 10.53.0.80
22:33:14 10.53.0.80
...
22:34:19 10.53.0.81
22:34:21 10.53.0.81
```

Рядок `server:` з відповіді `curl`: web-b

| Час зміни | Очікуваний час | Спостережений час | Виміряна затримка, с | Прогноз, с |
|---|---|---|---|---|
| 22:32:55 | 22:34:17 | 22:34:19 | 84 | 82 |

1. Чим пояснюється розбіжність між прогнозом і виміряним значенням?<br>
Я б не сказала, що розбіжність настільки критична в даному випадку. На мою думку, розбіжність лише в дві секунди це добрий результат, та цю затримку можна пояснити, наприклад, швидкістю виконання curl в циклі while. Також можливо, що цикл просто не встиг чи поспішив перевірити зміну, та оскільки інтервал чітко дві секунди - виникла розбіжність.

2. Якою була б найбільша можлива затримка для цього запису і за яких умов?<br>
Найбільша можлива розбіжність є дефолт TTL, тобто 90 с. Ця затримка можлива, коли DNS-запис змінюється сразу після того, як клієнт отримує стару адресу повного (дефолт) TTL.

3. Як слід діяти, щоб перенести сервіс на нову адресу, якщо TTL запису становить 86 400 с?<br>
Поперше можна зменшити TTL, навіть якщо просто на цю одну подію. Тоді, йде підготовка нового сервера, зміна DNS-записів А на нову IP-адресу. Старий сервер потрібно залишити працездатним до завершення терміну життя записів, це зроблено для користувачів, що мають старий кеш.

## Завдання 5. Негативне кешування

**Прогноз** (записати до вимірювання). Формула за RFC 2308 і розраховане значення: T = min(TTLsoa, minimum); T = min(15, 40) = 15 c

Перевірка, що сервер до `zone-reload` повертає `NXDOMAIN`:

```
root@ns-cs-202-25:/# dig @10.53.1.1 new3-ngsyx.cs-202-25.lab +norec

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.1.1 new3-ngsyx.cs-202-25.lab +norec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 984
;; flags: qr aa; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 79369bd5fbb40644010000006ae920c18d3e5157c0cd6d0e (good)
;; QUESTION SECTION:
;new3-ngsyx.cs-202-25.lab.      IN      A

;; AUTHORITY SECTION:
cs-202-25.lab.          15      IN      SOA     ns.cs-202-25.lab. admin.cs-202-25.lab. 2026110211 3600 600 86400 40

;; Query time: 0 msec
;; SERVER: 10.53.1.1#53(10.53.1.1) (UDP)
;; WHEN: Mon Nov 02 23:46:09 UTC 2026
;; MSG SIZE  rcvd: 126

```

Запит через resolver-a (крок 3), час `zone-reload` (крок 4), перевірка на сервері (крок 5):

```
root@ns-cs-202-25:/# date +%T; dig new3-ngsyx.cs-202-25.lab
23:46:37

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> new3-ngsyx.cs-202-25.lab
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 33144
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;new3-ngsyx.cs-202-25.lab.      IN      A

;; AUTHORITY SECTION:
cs-202-25.lab.          15      IN      SOA     ns.cs-202-25.lab. admin.cs-202-25.lab. 2026110211 3600 600 86400 40

;; Query time: 0 msec
;; SERVER: 10.53.0.53#53(10.53.0.53) (UDP)
;; WHEN: Mon Nov 02 23:46:37 UTC 2026
;; MSG SIZE  rcvd: 98

root@ns-cs-202-25:/# zone-reload; date +%T
zone cs-202-25.lab/IN: loaded serial 2026110222
OK
zone reload queued
23:46:40
root@ns-cs-202-25:/# dig @10.53.1.1 new3-ngsyx.cs-202-25.lab +norec +short
10.53.0.80
```

Вивід циклу спостереження (перші два рядки, останній рядок з `NXDOMAIN`, перший рядок з `NOERROR`):

```
root@ns-cs-202-25:/# while true; do echo "$(date +%T) $(dig new3-ngsyx.cs-202-25.lab +noall +comments | grep -o 'status: [A-Z]*')"; sleep 2; done
23:46:51 status: NXDOMAIN
23:46:53 status: NOERROR
```

| Негативну відповідь закешовано | Запис створено | Ім'я резолвиться через resolver-a | Виміряно, с | Прогноз, с |
|---|---|---|---|---|
| 23:46:37 | 23:46:40 | 23:46:53 | 13 | 15 |

Завдання повторювалось двічі. Вперше в мене випав баг з консоллю, ввід неправильно відображався перед виконанням програми та це сильно збило мій таймінг в 15 секунд. Вдруге я попросту не встигла ввести команду до рефрешу. Вдруге в мене було:<br>
| resolver-a | zone-reload |  Перший запит у циклі | Прогноз |
|---|---|---|---|
| 23:41:40 | 23:41:47 | 23:41:57 | 23:41:55 |

1. Чому тривалість негативного кешування визначається записом SOA, а не записом, який запитували?<br>
Оскільки запису не існує, то й його TTL також не існує. Коли сервер повертає NXDOMAIN, він також повертає запис SOA та звідси береться час негативного кешування.

2. Чи збігається TTL запису SOA в секції AUTHORITY з TTL запису SOA у файлі зони? Поясніть.<br>
Так, збігаються. Як я розумію, TTL в секції також обирається за формулою min(TTLsoa, minimum). Та оскільки minimum більший за TTLsoa, то було обрано 15. Також можна зазначити, що сам вимір зайняв ще менше, ніж прогноз.

3. Скрипт розгортання перевіряє ім'я сервісу до створення запису, а потім одразу після. Що він побачить і як це виправити?<br>
Після першої перевірки скрипт отримає NXDOMAIN, який закешується резолвером на деякий період часу. Це означає, що навіть після створення DNS-запису, перевірка ще буде отримувати NXDOMAIN. Я не знаю, чи можна це повноцінно виправити, але саме метод спостерігання й можна вважати "виправленням". Одним методом може бути просто дочекатися закінченню строку кешу, інший - використанні в лабораторній роботі повторні перевірки з інтервалом. 

## Завдання 6. Затримка пошуку імені

**Прогноз** (записати до вимірювання). До яких серверів звернеться resolver-a:

| Ім'я | Перший запит | Повторний запит |
|---|---|---|
| A: нове ім'я у власній зоні | Власний сервер | Кеш резолвера |
| B: нове ім'я в зоні `lab.` | Кореневий сервер | Кеш |
| C: ім'я в неіснуючому TLD | Кореневий | Кеш |
| D: `www.sNN.lab` після перезапуску резолвера | Кореневий | Кеш |

Вивід вимірювань:

```text
root@ns-cs-202-25:/# A=y$RANDOM.cs-202-25.lab; dig $A | grep 'Query time'; dig $A |grep 'Query time'
;; Query time: 1 msec
;; Query time: 1 msec
root@ns-cs-202-25:/# B=x$RANDOM.lab; dig $B | grep 'Query time'; dig $B | grep 'Query time'
;; Query time: 40 msec
;; Query time: 0 msec
root@ns-cs-202-25:/# C=x$RANDOM.zz$RANDOM; dig $C | grep 'Query time'; dig $C | grep 'Query time'
;; Query time: 80 msec
;; Query time: 0 msec
@todvv ➜ /workspaces/opism-lab-DNS (master) $ docker compose -f stand/compose.yaml restart resolver-a
[+] restart 0/1
 ⠸ Container dnslab-... Restarting 0.4s
root@ns-cs-202-25:/# dig www.cs-202-25.lab | grep 'Query time'; dig www.cs-202-25.lab | grep 'Query time'
;; Query time: 405 msec
;; Query time: 1 msec
```

| Ім'я | Перший запит, мс | Повторний, мс | Сервери, до яких звернувся резолвер (за журналами) |
|---|---|---|---|
| A | 1 | 1 | Авторитативний 10.53.1.1 |
| B | 40 | 0 | TLD 10.53.0.3 |
| C | 80 | 0 | Root 10.53.0.2 |
| D | 405 | 1 | Root 10.53.0.2, TLD 10.53.0.3, авторитативний 10.53.1.1 |

Значення `time_namelookup` з `curl` після перезапуску резолвера: 0.000000

Фрагменти журналів запитів root, TLD і власного сервера для рядка D:

```
@todvv ➜ /workspaces/opism-lab-DNS (master) $ docker compose -f stand/compose.yaml exec root tail /var/log/named/queries.log
03-Nov-2026 00:19:33.590 client @0x70eb64004488 10.53.0.53#16592 (.): query: . IN NS -E(0)DC (10.53.0.2)
03-Nov-2026 00:19:33.672 client @0x70eb6c06ac88 10.53.0.53#65336 (a.root-servers.net): query: a.root-servers.net IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:19:33.752 client @0x70eb6c06ac88 10.53.0.53#48885 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:19:34.210 client @0x70eb64004488 10.53.0.54#11509 (.): query: . IN NS -E(0)DC (10.53.0.2)
03-Nov-2026 00:19:34.291 client @0x70eb64004488 10.53.0.54#8419 (a.root-servers.net): query: a.root-servers.net IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:19:34.371 client @0x70eb64004488 10.53.0.54#9317 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:21:46.507 client @0x70eb6c06ac88 10.53.0.53#44466 (x27205.zz16984): query: x27205.zz16984 IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:29:06.901 client @0x70eb6c06ac88 10.53.0.53#34084 (.): query: . IN NS -E(0)DC (10.53.0.2)
03-Nov-2026 00:29:06.982 client @0x70eb64004488 10.53.0.53#54670 (a.root-servers.net): query: a.root-servers.net IN A -E(0)DC (10.53.0.2)
03-Nov-2026 00:29:07.062 client @0x70eb6c06ac88 10.53.0.53#62616 (www.cs-202-25.lab): query: www.cs-202-25.lab IN A -E(0)DC (10.53.0.2)
@todvv ➜ /workspaces/opism-lab-DNS (master) $ docker compose -f stand/compose.yaml exec 
tld tail /var/log/named/queries.log
02-Nov-2026 22:27:46.488 client @0x70801c06ac88 10.53.0.53#59223 (www.cs-202-25.lab): query: www.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
02-Nov-2026 23:29:58.888 client @0x708014002f38 10.53.0.53#16631 (new-ngsyx.cs-202-25.lab): query: new-ngsyx.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
02-Nov-2026 23:36:19.800 client @0x70801c06ac88 10.53.0.54#10827 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
02-Nov-2026 23:41:40.944 client @0x70801c06ac88 10.53.0.53#21269 (new2-ngsyx.cs-202-25.lab): query: new2-ngsyx.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:19:33.873 client @0x7aeb94002f38 10.53.0.53#19027 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:19:34.492 client @0x7aeb94002f38 10.53.0.54#17922 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:21:08.183 client @0x7aeb9c06ac88 10.53.0.53#5115 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:21:08.467 client @0x7aeb94002f38 10.53.0.54#34392 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:21:42.901 client @0x7aeb94002f38 10.53.0.53#39277 (x11518.lab): query: x11518.lab IN A -E(0)DC (10.53.0.3)
03-Nov-2026 00:29:07.183 client @0x7aeb94002f38 10.53.0.53#58952 (www.cs-202-25.lab): query: www.cs-202-25.lab IN A -E(0)DC (10.53.0.3)
root@ns-cs-202-25:/# querylog
02-Nov-2026 23:46:53.432 client @0x7901d129abf8 10.53.0.53#33419 (new3-ngsyx.cs-202-25.lab): query: new3-ngsyx.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:19:33.285 client @0x79b0a0001ee8 10.53.0.2#41086 (cs-202-25.lab): query: cs-202-25.lab IN SOA -E(0)K (10.53.1.1)
03-Nov-2026 00:19:33.915 client @0x79b0a0001ee8 10.53.0.53#19292 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:19:34.532 client @0x79b0a806b088 10.53.0.54#15801 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:21:07.970 client @0x79b0a0001ee8 10.53.0.2#40569 (cs-202-25.lab): query: cs-202-25.lab IN SOA -E(0)K (10.53.1.1)
03-Nov-2026 00:21:08.225 client @0x79b0a806b088 10.53.0.53#53187 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:21:08.507 client @0x79b0a0001ee8 10.53.0.54#28746 (ns.cs-202-25.lab): query: ns.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:21:37.791 client @0x79b0a806b088 10.53.0.53#37452 (y2341.cs-202-25.lab): query: y2341.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:21:50.534 client @0x79b0a0001ee8 10.53.0.53#38237 (www.cs-202-25.lab): query: www.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
03-Nov-2026 00:29:07.225 client @0x79b0a0001ee8 10.53.0.53#11501 (www.cs-202-25.lab): query: www.cs-202-25.lab IN A -E(0)DC (10.53.1.1)
```

1. Яку затримку вносить кожен із серверів root, TLD і власний сервер? Покажіть, з яких рядків таблиці це випливає.<br>
Root 80 ms - C<br>
TLD 40 ms - B<br>
Власний 1 ms - A<br>
Це випливає з рядків таблиці про перший запит.

2. Чому в рядку A перший запит не проходить через root і TLD, хоча ім'я нове?<br>
Скоріш за все, всю потрібну інформацію вже давно було записано в кеш, тоді резолвер вже знає DNS-ієрархію, тобто складову зони. Оскільки новим є лише ім'я, резолвер одразу звертається до власного сервера.

3. Вебсторінка завантажує ресурси з п'яти різних доменів. Як це впливає на час першого відкриття сторінки?<br>
Кожен новий домен потребує окремого резолвінгу DNS, тому перше відкриття може бути довшим через додаткову нагрузку DNS-запитами. Якщо ці різні домені вже присутні в кеші, то відкриття сторінки буде швидше.

## Завдання 7. Інцидент: зупинка власного сервера

**Прогноз** (записати до вимірювання). Що поверне кожен резолвер після зупинки сервера та спливання TTL і скільки часу займе відповідь: resolver-a видасть таймаут. resolver-b можливо швидко надасть закешовану минулу/застарілу версію.

Кешування запису в обох резолверах (крок 2) і час зупинки сервера (крок 3):

```
root@ns-cs-202-25:/# dig @10.53.0.53 review.cs-202-25.lab +noall +answer
review.cs-202-25.lab.      35      IN      A       10.53.0.81
root@ns-cs-202-25:/# dig @10.53.0.54 review.cs-202-25.lab +noall +answer
review.cs-202-25.lab.      35      IN      A       10.53.0.81
```

Запити із заміром часу (крок 5):

```
root@ns-cs-202-25:/# named-stop; date +%T
01:37:17
root@ns-cs-202-25:/# sleep 45
root@ns-cs-202-25:/# time dig @10.53.0.53 preview.cs-202-25.lab +tries=1 +timeout=30
;; communications error to 10.53.0.53#53: timed out

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.53 preview.cs-202-25.lab +tries=1 +timeout=30
; (1 server found)
;; global options: +cmd
;; no servers could be reached

real    0m30.051s
user    0m0.005s
sys     0m0.006s
root@ns-cs-202-25:/# time dig @10.53.0.53 preview.cs-202-25.lab +tries=1 +timeout=30

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.53 preview.cs-202-25.lab +tries=1 +timeout=30
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 57807
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;preview.cs-202-25.lab.         IN      A

;; Query time: 0 msec
;; SERVER: 10.53.0.53#53(10.53.0.53) (UDP)
;; WHEN: Tue Nov 03 01:38:39 UTC 2026
;; MSG SIZE  rcvd: 50


real    0m0.021s
user    0m0.005s
sys     0m0.006s
root@ns-cs-202-25:/# time dig @10.53.0.54 preview.cs-202-25.lab +tries=1 +timeout=30
;; communications error to 10.53.0.54#53: timed out

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.54 preview.cs-202-25.lab +tries=1 +timeout=30
; (1 server found)
;; global options: +cmd
;; no servers could be reached

real    0m30.033s
user    0m0.003s
sys     0m0.007s
```

| Запит | status або тайм-аут | ANSWER, TTL | `real` |
|---|---|---|---|
| resolver-a, перший | timed out | - | 30.051s |
| resolver-a, другий | SERVFAIL | - | 0.021s |
| resolver-b | timed out | - | 30.033s |

Час `named-start` і вивід циклу спостереження до першої успішної відповіді (крок 6):

```text
root@ns-cs-202-25:/# named-start; date +%T
01:39:18
root@ns-cs-202-25:/# while true; do echo "$(date +%T) $(dig @10.53.0.53 preview.cs-202-25.lab +short +tries=1 +timeout=3)"; sleep 2; done
01:39:20 10.53.0.81
01:39:22 10.53.0.81
01:39:24 10.53.0.81
01:39:26 10.53.0.81
01:39:28 10.53.0.81
01:39:30 10.53.0.81
01:39:32 10.53.0.81
```

Перевірка після завдання (`dig @10.53.1.1 sNN.lab SOA +norec +short`):

```
ns.cs-202-25.lab. admin.cs-202-25.lab. 2026110222 3600 600 86400 40
```

### Постмортем

1. Що сталося: після виконання named-stop DNS-сервер припинив обслуговувати запити. Після очікування 45 секунд резолвери не змогли отримати актуальну відповідь від авторитетного сервера. Resolver-a спочатку повернув тайм-аут, потім SERVFAIL. Resolver-b також повернув тайм-аут.
2. Вплив на клієнтів resolver-a і resolver-b: resolver-a спочатку чекав 30 секунд і надав тайм-аут. Під час повторного запиту резолвер швидко повернув SERVFAIL за 0.021 секунди, можливо через згенерований кеш. IP-адресу та відповіді не отримано. resolver-b також чекав 30 секунд, після чого також не отримав відповіді. Нажаль, очікуваного повернення застарілого запису не відбулося.
3. Хронологія з виміряним часом: 01:37:17 server stop; 01:38:02 sleep period finish; 01:38:37 завершення першого запиту resolver-а; 01:38:39 другого запиту resolver-a; 01:39:10 resolver-b; 01:39:18 server start; 01:39:20 server responds.
4. Чому кеш допоміг або не допоміг: мені здається, в даній ситуації кеш потрібен допомогти. Можливо, я знов не встигла з таймінгом та кеш очистився настільки, що навіть serve-stale не зміг витягнути якусь версію. 
5. Що слід змінити: налаштувати TTL та механізм serve-stale так, щоб було гарантовано можливість отримання застарілої версії, коли сервер недоступний.

1. Який TTL мав запис у відповіді resolver-b і навіщо такий TTL?<br>
Ніякий? Знаємо лише те, що початковий TTL становив 35 с. Така швидкість потрібна, щоб швидше отримати/поширити зміну DNS-записів.
2. Порівняйте результат і тривалість двох запитів до resolver-a. Якщо вони різняться, сформулюйте гіпотезу щодо причини; якщо ні, поясніть, що робить резолвер протягом цього часу.<br>
Перший запит був тайм-аут після повних 30 секунд, другий запит зайняв менш секунди, щоб повернути SERVFAIL. Тут працює кеш, бо резолвер вже знає, що сервер недоступен, то він не витрачає час/ресурси та просто надає результат "недоступно", поки не буде очищено кеш. 
4. Чому після `named-start` ім'я через resolver-a зарезолвилося не одразу?<br>
Може я неправильно бачу, але саме в моєму дослідженні після включення сервера, сервер майже одразу відповідає - затримка в дві секунди, і то тому, що я не працюю моментально. Однак, якщо сервер не одразу резолвиться, це просто вказує на те, що серверу потрібен час на відновлення функціональності. Або резолвер звернувся до серверу, отримав негативний результат та закешував його.
6. У якій ситуації serve-stale шкодить, а не допомагає? Пов'яжіть відповідь із завданням 4.<br>
Якщо механіка допомогає, коли сервер "лежить", то вона буде заважати, коли сервер повністю працездатен та була зроблена зміна в DNS-записах. Це продемонстровано в завданні 4, де резолвер 2 секунди утримує інформацію від користувача. Мені повезло, що система швидко працює, що мені потрібно було переробляти тричі роботу, але для великих систем з немалою кількістю користувачів така техніка буде лише заважати, коли потрібний оперативний доступ до інформації.

## Фінальний файл зони

```
  GNU nano 7.2                        /lab/zone/cs-202-25.lab.zone                                  
; Zone cs-202-25.lab  ^`^t starting version.
; After every change: increase the serial, then run  zone-reload
$TTL 90
@     15 IN SOA ns.cs-202-25.lab. admin.cs-202-25.lab. (
            2026110222 ; serial
            3600       ; refresh
            600        ; retry
            86400      ; expire
            40         ; minimum
        )
@     IN NS ns.cs-202-25.lab.
ns    IN A  10.53.1.1

; ---- add records from your variant card below this line ----
www             IN A            10.53.0.81
news            IN CNAME        www
mail            IN A            10.53.0.80
@               IN MX           15 mail
@               IN TXT          "lab-token=bc96aae3"
preview   35    IN A            10.53.0.81
new-ngsyx       IN A    10.53.0.80
new2-ngsyx      IN A            10.53.0.80
new3-ngsyx      IN A            10.53.0.80
```

## Висновки

**Що виявилося несподіваним.** Результат, який не збігся з очікуванням, і рядок виводу, де це видно:
```
root@ns-cs-202-25:/# time dig @10.53.0.54 preview.cs-202-25.lab +tries=1 +timeout=30
;; communications error to 10.53.0.54#53: timed out

; <<>> DiG 9.18.39-0ubuntu0.24.04.8-Ubuntu <<>> @10.53.0.54 preview.cs-202-25.lab +tries=1 +timeout=30
; (1 server found)
;; global options: +cmd
;; no servers could be reached

real    0m30.033s
user    0m0.003s
sys     0m0.007s
```
Мені здалося, що resolver-b потрібен був отримати "застарілу" інформацію та було несподівано отримати зовсім нічого.

**Яке питання залишилося без відповіді.** Що саме не вдалося пояснити; до якого рядка виводу це стосується; що вже перевірено (RFC, документація, додатковий експеримент на стенді) і що з'ясувати не вдалося: нажаль, при піднятті стенда та відкритті вікна з логін-інформацією ніяк не вдалось увійти. Всі інші аспекти біль-менш інтуїтивно понятні, настільки, що можна просто пояснити своїими словами, та викладач поправить, якщо потрібно.

## Декларація використання ШІ

Використовувалися інструменти ШІ: так

| № | Інструмент | Промпт (дослівно) | Що використано у звіті і що змінено самостійно |
|---|---|---|---|
| 1 | GPT-5.6 Luna | Для чого у відповіді TLD присутній запис у секції ADDITIONAL і що сталося б, якби його не було? | Відповідь моделі була використана для самостійного формування відповіді на питання своїми словами |
