# Лабораторна робота 2. TLS та сертифікати

| Поле | Значення |
|---|---|
| ПІБ | Тодорович В. В. |
| Група | КБЗІ-2.02 |
| Ідентифікатор | cs-202-25 |
| Дата виконання | 17.11.26 |
| Середовище | GitHub Codespaces |
| Версія OpenSSL | OpenSSL 3.0.13 30 Jan 2024 (Library: OpenSSL 3.0.13 30 Jan 2024) |

## Параметри варіанта

| Параметр | Значення |
|---|---|
| Віддалений сервер | `edge.cs-202-25.lab` |
| Імена для сертифіката | `www.cs-202-25.lab`, `files.cs-202-25.lab` |
| Ваш сервер (IP) | 10.53.1.1 |

## Завдання 1. HTTP і HTTPS у дампі трафіку

Вивід кроку 1.1:

```
[1] 43
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
server: web
Host: edge.cs-202-25.lab
SNI: 
protocol: HTTP/1.1 
client: 10.53.1.1
10 packets captured
10 packets received by filter
0 packets dropped by kernel
[1]+  Exit 124                timeout 8 tcpdump -ni eth0 -A -s0 -l 'host 10.53.0.80 and tcp port 80' > t1-http.txt
17:57:14.449502 IP 10.53.1.1.50494 > 10.53.0.80.80: Flags [P.], seq 1:82, ack 1, win 502, options [nop,nop,TS val 1534678845 ecr 172727694], length 81: HTTP: GET / HTTP/1.1
K..GET / HTTP/1.1
Host: edge.cs-202-25.lab
server: web
Host: edge.cs-202-25.lab
```

Вивід кроку 1.2:

```
[1] 49
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
server: web
Host: edge.cs-202-25.lab
SNI: edge.cs-202-25.lab
protocol: HTTP/2.0 TLSv1.3
client: 10.53.1.1
23 packets captured
23 packets received by filter
0 packets dropped by kernel
[1]+  Exit 124                timeout 8 tcpdump -ni eth0 -A -s0 -l 'host 10.53.0.80 and tcp port 443' > t1-https.txt
0
1
```

| Що шукали в дампі | HTTP | HTTPS |
|---|---|---|
| рядок запиту `GET /` | + | - |
| заголовок `Host` | + | - |
| тіло відповіді (`server: web`) | + | - |
| ім'я віддаленого сервера (кількість входжень) | 2 (`Host: edge.cs-202-25.lab` запит, тіло відповіді) | 1 (`SNI: edge.cs-202-25.lab`) |

1. У якому повідомленні і в якому полі передається ім'я сервера, яке видно в дампі HTTPS?
> Ім'я сервера передається в частині `ClientHello`, в рядку `SNI`. Оскільки шифрування ще не почалося, ім'я сервера можна вільно побачити.
2. Чому рядок `SNI:` у відповіді сервера порожній для HTTP і заповнений для HTTPS?
> SNI є частиною TLS. Оскільки HTTP по порту 80 не використовує TLS, то відповідно рядок `SNI` буде відсутній в частині `ClientHello` запиту HTTP.
3. Рядок `protocol:` показує HTTP/1.1 для запиту по HTTP і HTTP/2.0 для запиту по HTTPS, хоча обидва запити виконано однією командою `curl` без додаткових ключів. Що визначило версію HTTP у другому випадку?
> ALPN визначає версію HTTPS. `Application-Layer Protocol Negotiation` працює під час рукостискання, пропонуючи серверу версії протоколу. Знову ж, даний механізм є частиною TLS, тому воно не буде працювати в HTTP та визначати його версію.

## Завдання 2. Рукостискання по записах

Вивід кроку 2.1 (TLS 1.3):

```
Sent Record
  Content Type = Handshake (22)
    ClientHello, Length=329
Received Record
  Content Type = Handshake (22)
    ServerHello, Length=118
Received Record
  Content Type = ChangeCipherSpec (20)
Received Record
  Content Type = ApplicationData (23)
  Inner Content Type = Handshake (22)
    EncryptedExtensions, Length=15
Received Record
  Content Type = ApplicationData (23)
  Inner Content Type = Handshake (22)
    Certificate, Length=984
Received Record
  Content Type = ApplicationData (23)
  Inner Content Type = Handshake (22)
    CertificateVerify, Length=75
Received Record
  Content Type = ApplicationData (23)
  Inner Content Type = Handshake (22)
    Finished, Length=48
Sent Record
  Content Type = ChangeCipherSpec (20)
Sent Record
  Content Type = ApplicationData (23)
  Inner Content Type = Handshake (22)
    Finished, Length=48
Sent Record
  Content Type = ApplicationData (23)
  Inner Content Type = Alert (21)
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
ALPN protocol: h2
Verify return code: 0 (ok)
```

Вивід кроку 2.2 (TLS 1.2):

```
Sent Record
  Content Type = Handshake (22)
    ClientHello, Length=224
Received Record
  Content Type = Handshake (22)
    ServerHello, Length=74
Received Record
  Content Type = Handshake (22)
    Certificate, Length=979
Received Record
  Content Type = Handshake (22)
    ServerKeyExchange, Length=110
Received Record
  Content Type = Handshake (22)
    ServerHelloDone, Length=0
Sent Record
  Content Type = Handshake (22)
    ClientKeyExchange, Length=33
Sent Record
  Content Type = ChangeCipherSpec (20)
Sent Record
  Content Type = Handshake (22)
    Finished, Length=12
Received Record
  Content Type = Handshake (22)
    NewSessionTicket, Length=214
Received Record
  Content Type = ChangeCipherSpec (20)
Received Record
  Content Type = Handshake (22)
    Finished, Length=12
Received Record
  Content Type = ApplicationData (23)
Sent Record
  Content Type = Alert (21)
New, TLSv1.2, Cipher is ECDHE-ECDSA-AES256-GCM-SHA384
ALPN protocol: h2
    Verify return code: 0 (ok)
```

| № | Напрям | `Content Type` | `Inner Content Type` | Повідомлення | `Length` |
|---|---|---|---|---|---|
| 1 | К→С | Handshake (22) | - | ClientHello | 329 |
| 2 | С→К | Handshake (22) | - | ServerHello | 118 |
| 3 | С→К | ChangeCipherSpec (20) | - | ChangeCipherSpec | - |
| 4 | С→К | ApplicationData (23) | Handshake (22) | EncryptedExtensions | 15 |
| 5 | С→К | ApplicationData (23) | Handshake (22) | Certificate | 984 |
| 6 | С→К | ApplicationData (23) | Handshake (22) | CertificateVerify | 75 |
| 7 | С→К | ApplicationData (23) | Handshake (22) | Finished | 48 |
| 8 | К→С | ChangeCipherSpec (20) | - | ChangeCipherSpec | - |
| 9 | К→С | ApplicationData (23) | Handshake (22) | Finished | 48 |
| 10 | К→С | ApplicationData (23) | Alert (21) | Alert | - |

Вивід кроків 2.3 і 2.4:

```
        extension_type=server_name(0), length=23
          0000 - 00 15 00 00 12 65 64 67-65 2e 63 73 2d 32 30   .....edge.cs-20
          000f - 32 2d 32 35 2e 6c 61 62-                       2-25.lab
        extension_type=ec_point_formats(11), length=4
[1] 65
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
23 packets captured
23 packets received by filter
0 packets dropped by kernel
[1]+  Exit 124                timeout 6 tcpdump -ni eth0 -A -s0 -l 'host 10.53.0.80 and tcp port 443' > t2-dump-$v.txt
TLS 1.3: 0
[1] 71
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
21 packets captured
21 packets received by filter
0 packets dropped by kernel
[1]+  Exit 124                timeout 6 tcpdump -ni eth0 -A -s0 -l 'host 10.53.0.80 and tcp port 443' > t2-dump-$v.txt
TLS 1.2: 2
```

| Показник | TLS 1.3 | TLS 1.2 |
|---|---|---|
| Довжина ClientHello, байт | 329 | 224 |
| Номер першого запису, тип якого спостерігач не бачить | 4 | - |
| Кількість RTT від ClientHello до першого HTTP-запиту | 1 | 2 |
| Чи видно сертифікат у дампі (результат 2.4) | - (0) | + (2) |
| Обраний протокол ALPN | h2 | h2 |

1. За якими рядками виводу 2.1 визначено, з якого запису починається шифрування? Що бачить спостерігач замість типу кожного наступного запису?
> Початок шифрування можна одразу відстежити за повідомленням `EncryptedExtensions`.
> `Received Record`
> `  Content Type = ApplicationData (23)`
> `  Inner Content Type = Handshake (22)`
> `    EncryptedExtensions, Length=15`
> Спостерігач бачить лише зовнішній тип запису ApplicationData (23), довжину та повідомлення про рукостискання. Інші дані, як чіткий внутрішній тип повідомлення, зашифровані та недоступні без відповідних ключей.
2. Чому ClientHello у TLS 1.3 довший, ніж у TLS 1.2? Назвіть розширення, яке за рахунок цього скорочує рукостискання на один RTT.
> Він є довшим, бо TLS 1.3 містить розширення `key_share` для того, щоб одразу зробити обмін ключами. Оскільки клієнт вже надсилає відкриту частину ключа, сервер має змогу відповісти разом з спільним ключем - ця взаємодія скорочує RTT.
3. Довжина розширення `server_name` у виводі 2.3 становить 17 байт для імені з 12 символів. З яких частин складається це значення і яким воно буде для вашого імені?
> `server_name` має довжину 23 байти для `edge.cs-202-25.lab` з 18 символів. Складається з: типу імені (1 байт, host_name 00); довжини імені (2 байти, 00 12); довжни списку імен (2 байти, 00 16); власне ім'я (18 байтів).
4. Навіщо в TLS 1.3 передається запис ChangeCipherSpec, якщо в рукостисканні він нічого не змінює?
> Даний запис потрібен проксі обладнанню, яке очікує на подібну примітку для коректного продовження роботи. Проміжне мережеве обладнання може бути застарілим, тоді воно буде використовувати лише TLS 1.2. Та тоді, краще для такої апаратури імітувати TLS 1.2 замість прибирання запису та сподівання, що нічого не зламається.

## Завдання 3. Сертифікат для власного сервера

Вивід кроків 3.1 і 3.2:

```
збережено /lab/tls/acme-root.crt
subject=CN = Pebble Root CA 68ae98
notBefore=Nov 23 17:56:01 2026 GMT
notAfter=Nov 23 17:56:01 2056 GMT
curl: (60) SSL certificate problem: self-signed certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

Вивід кроку 3.3 (останні рядки `lego` і рядки журналу nginx):

```
2026/11/23 18:00:32 [INFO] [www.cs-202-25.lab, files.cs-202-25.lab] acme: Obtaining SAN certificate
2026/11/23 18:00:32 [INFO] [files.cs-202-25.lab] AuthURL: https://ca.lab:14000/authZ/USGtRJ-RCDxrtdv_DiLYSeRPMBNiB_66QueBQyc9ux8
2026/11/23 18:00:32 [INFO] [www.cs-202-25.lab] AuthURL: https://ca.lab:14000/authZ/3DOxeam8ijOc-d8Mvv-0DWyauZ0o83p_DR8GKFdvNXg
2026/11/23 18:00:32 [INFO] [files.cs-202-25.lab] acme: Could not find solver for: tls-alpn-01
2026/11/23 18:00:32 [INFO] [files.cs-202-25.lab] acme: use http-01 solver
2026/11/23 18:00:32 [INFO] [www.cs-202-25.lab] acme: Could not find solver for: tls-alpn-01
2026/11/23 18:00:32 [INFO] [www.cs-202-25.lab] acme: use http-01 solver
2026/11/23 18:00:32 [INFO] [files.cs-202-25.lab] acme: Trying to solve HTTP-01
2026/11/23 18:00:39 [INFO] [files.cs-202-25.lab] The server validated our request
2026/11/23 18:00:39 [INFO] [www.cs-202-25.lab] acme: Trying to solve HTTP-01
2026/11/23 18:00:46 [INFO] [www.cs-202-25.lab] The server validated our request
2026/11/23 18:00:46 [INFO] [www.cs-202-25.lab, files.cs-202-25.lab] acme: Validations succeeded; requesting certificates
2026/11/23 18:00:46 [INFO] Wait for certificate [timeout: 30s, interval: 500ms]
2026/11/23 18:00:46 [INFO] [www.cs-202-25.lab] Server responded with a certificate.
10.53.0.10 - - [23/Nov/2026:18:00:32 +0000] "GET /.well-known/acme-challenge/PjMtruPTNzCAsAv6kEUh-ZnYtV451DHmpuqviAO3hww HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"
10.53.0.10 - - [23/Nov/2026:18:00:32 +0000] "GET /.well-known/acme-challenge/PjMtruPTNzCAsAv6kEUh-ZnYtV451DHmpuqviAO3hww HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"
10.53.0.10 - - [23/Nov/2026:18:00:32 +0000] "GET /.well-known/acme-challenge/PjMtruPTNzCAsAv6kEUh-ZnYtV451DHmpuqviAO3hww HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"
10.53.0.10 - - [23/Nov/2026:18:00:40 +0000] "GET /.well-known/acme-challenge/IoMVvYpmF-9m2Finx5h_3va4LsHlJFOoZrltfLhJwMw HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"
10.53.0.10 - - [23/Nov/2026:18:00:40 +0000] "GET /.well-known/acme-challenge/IoMVvYpmF-9m2Finx5h_3va4LsHlJFOoZrltfLhJwMw HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"
10.53.0.10 - - [23/Nov/2026:18:00:40 +0000] "GET /.well-known/acme-challenge/IoMVvYpmF-9m2Finx5h_3va4LsHlJFOoZrltfLhJwMw HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"

```

Вивід кроку 3.4:

```
subject=CN = www.cs-202-25.lab
issuer=CN = Pebble Intermediate CA 56f347
notBefore=Nov 23 18:00:46 2026 GMT
notAfter=Nov 23 18:00:45 2031 GMT
serial=084A4FE6208ACF64
X509v3 Subject Alternative Name: 
    DNS:www.cs-202-25.lab, DNS:files.cs-202-25.lab
subject=CN = Pebble Intermediate CA 56f347
issuer=CN = Pebble Root CA 68ae98
```

Вивід кроку 3.5:

```
nginx: the configuration file /lab/nginx.conf syntax is ok
nginx: configuration file /lab/nginx.conf test is successful
2026/11/23 18:01:34 [notice] 94#94: signal process started
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
Certificate chain
 0 s:CN = www.cs-202-25.lab
   i:CN = Pebble Intermediate CA 56f347
   a:PKEY: id-ecPublicKey, 256 (bit); sigalg: RSA-SHA256
   v:NotBefore: Nov 23 18:00:46 2026 GMT; NotAfter: Nov 23 18:00:45 2031 GMT
---
Verify return code: 21 (unable to verify the first certificate)
```

Вивід кроку 3.6:

```
nginx: the configuration file /lab/nginx.conf syntax is ok
nginx: configuration file /lab/nginx.conf test is successful
2026/11/23 18:02:22 [notice] 102#102: signal process started
server: cs-202-25 (https)
Host: www.cs-202-25.lab
SNI: www.cs-202-25.lab
protocol: TLSv1.3
Certificate chain
 0 s:CN = www.cs-202-25.lab
   i:CN = Pebble Intermediate CA 56f347
   a:PKEY: id-ecPublicKey, 256 (bit); sigalg: RSA-SHA256
   v:NotBefore: Nov 23 18:00:46 2026 GMT; NotAfter: Nov 23 18:00:45 2031 GMT
 1 s:CN = Pebble Intermediate CA 56f347
   i:CN = Pebble Root CA 68ae98
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Nov 23 17:56:01 2026 GMT; NotAfter: Nov 23 17:56:01 2056 GMT
---
Verify return code: 0 (ok)
server: cs-202-25 (https)
Host: files.cs-202-25.lab
SNI: files.cs-202-25.lab
protocol: TLSv1.3
```

| Параметр | Значення |
|---|---|
| Видавець сертифіката сервера | Pebble Intermediate CA 56f347 |
| Видавець сертифіката видавця | Pebble Root CA 68ae98 |
| Імена в SAN | `www.cs-202-25.lab`, `files.cs-202-25.lab` |
| Строк дії (від – до) | 5 років (11.23.26 – 11.23.31) |
| Тип ключа сервера і алгоритм підпису | id-ecPublicKey, 256 (bit); RSA-SHA256 |
| Кількість сертифікатів, які надсилає сервер, у кроці 3.5 і в кроці 3.6 | 1; 2 |

1. Хто і як перевірив, що ви контролюєте обидва імена? Вкажіть рядки виводу, які це підтверджують.
> Центр сертифікації Pebble перевірив контроль імен протоколом ACME, використовуючи метод перевірки HTTP-01. Центр сертифікації надіслав HTTP-запити до обох доменів, щоб перевірити доступність спеціальних файлів підтвердження за адресою `/.well-known/acme-challenge/`. Це підтверджують рядки виводу:
> `[INFO] [files.cs-202-25.lab] acme: Trying to solve HTTP-01`
> `[INFO] [files.cs-202-25.lab] The server validated our request`
> `[INFO] [www.cs-202-25.lab] acme: Trying to solve HTTP-01`
> `[INFO] [www.cs-202-25.lab] The server validated our request`
> `[INFO] [www.cs-202-25.lab, files.cs-202-25.lab] acme: Validations succeeded; requesting certificates`
> А також журнал `Nginx`:
> `10.53.0.10 - - [23/Nov/2026:18:00:32 +0000] "GET /.well-known/acme-challenge/PjMtruPTNzCAsAv6kEUh-ZnYtV451DHmpuqviAO3hww HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"`
> `10.53.0.10 - - [23/Nov/2026:18:00:32 +0000] "GET /.well-known/acme-challenge/PjMtruPTNzCAsAv6kEUh-ZnYtV451DHmpuqviAO3hww HTTP/1.1" 200 87 "-" "LetsEncrypt-Pebble-VA (linux; amd64)"`
> `...`
2. Чому в кроці 3.5 перевірка не пройшла, хоча корінь центру сертифікації вказано в `--cacert`, а сертифікат виданий правильно?
> Перевірка не пройшла через те, що сервер надсилав клієнту лише власний сертифікат, не надсилаючи проміжний сертифікат. --cacert містить лише корінь Pebble, коли перевірка також очікує на проміжний сертифікат. Через це клієнт не може побудувати ланцюжок до кореня, тому перевірка завершується `return code: 21 (unable to verify the first certificate)`.
3. У виводі ланцюжка для сертифіката сервера вказано `id-ecPublicKey` і `RSA-SHA256`. Що означає кожне з цих значень і чому вони не суперечать одне одному?
> `id-ecPublicKey` є алгоритмом, що описує відкритий ключ сервера за допомогою еліптичних кривих. `RSA-SHA256` є алгоритмом цифрового підпису, яким видавець підписує сертифікати. Оба алгоритми мають різні функції: перший працює для сервера, визначає тип ключа; другий працює для центра сертифікації, визначає спосіб підписання сертифікату для підтвердження його автентичності. Також видно, що ці ключи належать різним сторонам, тому суперечностей зовсім немає.
4. Що станеться з перевіркою вашого сервера після перезапуску стенда і чому? Що в реальній інфраструктурі відповідає цій ситуації?
> Після перезапуску Pebble створить новий корінь та проміжний сертифікат. Це означає, що сертифікат потрібно заново видавати, оскільки новий корінь не знає застарілого проміжного центру сертифікації. У реальній інфраструктурі це може бути ситуація відкликання сертифікатів CA. Компаніям потрібно отримати нові сертифікати; центр сертифікації буде створювати та захищати нові ключи, підписувати нові сертифікати кореня чи проксі та публікувати їх. Також може бути ситуація оновлення даних (PKI), що також змусить пройти вже названу ситацію.

## Завдання 4. Діагностика несправностей

Вивід кроку 4.1:

```
=== f1.cs-202-25.lab
server: web, f1
Certificate chain
 0 s:CN = f1.cs-202-25.lab
   i:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Feb 21 17:56:00 2027 GMT
 1 s:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   i:O = OPISM Lab, CN = OPISM Stand Root CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Nov 22 17:56:00 2031 GMT
---
Verify return code: 0 (ok)
=== f2.cs-202-25.lab
curl: (60) SSL: no alternative certificate subject name matches target host name 'f2.cs-202-25.lab'
Certificate chain
 0 s:CN = intra.cs-202-25.lab
   i:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Feb 21 17:56:00 2027 GMT
 1 s:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   i:O = OPISM Lab, CN = OPISM Stand Root CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Nov 22 17:56:00 2031 GMT
---
Verify return code: 0 (ok)
=== f3.cs-202-25.lab
curl: (60) SSL certificate problem: self-signed certificate
Certificate chain
 0 s:CN = f3.cs-202-25.lab
   i:CN = f3.cs-202-25.lab
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Feb 21 17:56:00 2027 GMT
---
Verify return code: 18 (self-signed certificate)
=== f4.cs-202-25.lab
curl: (60) SSL certificate problem: unable to get local issuer certificate
Certificate chain
 0 s:CN = f4.cs-202-25.lab
   i:O = Foreign Org, CN = Foreign Intermediate CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Feb 21 17:56:00 2027 GMT
 1 s:O = Foreign Org, CN = Foreign Intermediate CA
   i:O = Foreign Org, CN = Foreign Root CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Nov 22 17:56:00 2031 GMT
---
Verify return code: 20 (unable to get local issuer certificate)
=== f5.cs-202-25.lab
curl: (60) SSL certificate problem: certificate has expired
Certificate chain
 0 s:CN = f5.cs-202-25.lab
   i:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   v:NotBefore: May  7 17:56:00 2026 GMT; NotAfter: Nov  3 17:56:00 2026 GMT
 1 s:O = OPISM Lab, CN = OPISM Stand Intermediate CA
   i:O = OPISM Lab, CN = OPISM Stand Root CA
   v:NotBefore: Nov 23 17:56:00 2026 GMT; NotAfter: Nov 22 17:56:00 2031 GMT
---
Verify return code: 10 (certificate has expired)
```

| Сервер | Помилка `curl` | `Verify return code` | Яка перевірка не пройшла | Рядок виводу, що це доводить | Що має змінити власник сервера |
|---|---|---|---|---|---|
| f1 | - | `0 (ok)` | - | Ланцюжок з двох сертифікатів без помилок | - |
| f2 | `no alternative certificate subject name matches target host name` | `0 (ok)` | Перевірка відповідності хоста сертифікату | `s:CN = intra.cs-202-25.lab` | Випустити сертифікат для `f2.cs-202-25.lab` |
| f3 | `self-signed certificate` | `18 (self-signed certificate)` | Перевірка на довіру центра сертифікації | `i:CN = f3.cs-202-25.lab` | Змінити центр сертифікації |
| f4 | `unable to get local issuer certificate` | `20 (unable to get local issuer certificate)` | Перевірка ланцюжка сертифікатів | `i:O = Foreign Org, CN = Foreign Root CA` | Змінити центр сертифікації або додати проміжний сертифікат |
| f5 | `certificate has expired` | `10 (certificate has expired)` | Перевірка терміну дії сертифіката | `NotAfter: Nov  3 17:56:00 2026 GMT` | Оновити сертифікат |

1. Для сервера з помилкою `unable to get local issuer certificate` визначте, яка це несправність: сервер не надсилає проміжний сертифікат чи сертифікат виданий центром, якому клієнт не довіряє. Чим ці дві несправності відрізняються у виводі `openssl s_client`?
> Сервер надсилає два сертифікати, але обидва належать `Foreign Org`, тобто це саме проблема недовіри до центра сертифікації. У виводі `openssl s_client` ці несправності б відрізнялись за інформацією про ланцюжок сертифікатів, `return code` чітко б вказав проблему. В моєму варіанті код 20, а для проблеми, де сервер не надсилає проміжний сертифікат, код, напевно, був б `21 (unable to verify the first certificate)`. Також для коду 20 можна побачити, що надіслано два сертифікати, тоді як для коду 21 буде надсилатися лише один.
2. Якщо серед ваших серверів є сервер з невідповідним ім'ям, чому `openssl s_client` повертає для нього `Verify return code: 0 (ok)`, а `curl` повідомляє про помилку? Якщо такого сервера немає, дайте відповідь для загального випадку.
> `openssl s_client` без відповідних ключей буде довіряти сертифікату та його ланцюжку. Тобто, перевірки відповідності імені сервера імені в сертифікаті немає. Тому й f2 повертає код 0, хоча сталася помилка, бо шлях до кореня успішно побудовано. `curl` повідомляє про помилку, бо утиліта додатково перевіряє відповідність ім'я сайту з SAN сертифіката, `intra.cs-202-25.lab`, а запит йде до `f2.cs-202-25.lab`.
3. Які з п'яти серверів відповіли б на запит `curl -k`? Що при цьому втрачається порівняно з `--cacert`?
> Якщо всі п'ять серверів повноцінно працюють (а рукостискання завершилось з кожним з них, то вони працюють), то запити будуть успішними до всіх вказаних серверів. Ключ -k вимикає перевірку сертифіката, тобто всі випадки перевірки на строк роботи сертифікату, відповідності імена хоста та довіри до центру сертифікації опущено. Тоді, порівняно з `--cacert`, втрачається безпека. Шифрування може залишитися, але немає гарантій справжності сервера.

## Завдання 5. Ціна нового з'єднання

**Прогноз** (записати до вимірювання). Скільки RTT мине до завершення кожного етапу:

| Етап | TLS 1.3, нове з'єднання | TLS 1.2, нове з'єднання | Повторне використання з'єднання |
|---|---|---|---|
| `time_connect` | 1 | 2 | 0 |
| `time_appconnect` | 2 | 3 | 0 |
| `time_starttransfer` | 3 | 5 | 0 |

Вивід кроків 5.2 і 5.3:

```
0.040980 0.061144 0.084858 0.105247 0.105313 2 1
0.020431 0.040574 0.064295 0.084611 0.084672 2 1
0.020420 0.040552 0.064430 0.084712 0.084772 2 1
0.020448 0.040596 0.084792 0.105205 0.105289 2 1
0.020481 0.040595 0.084642 0.104939 0.105002 2 1
0.020540 0.040671 0.084691 0.104986 0.105078 2 1
0.020506 0.040637 0.064683 0.085078 0.085138 2 1
0.000016 0.000000 0.000000 0.020416 0.020473 2 0
0.000019 0.000000 0.000000 0.020330 0.020490 2 0
```

| Запит | `namelookup` | `connect` | `appconnect` | `starttransfer` | `total` | `num_connects` |
|---|---|---|---|---|---|---|
| TLS 1.3, нове | 20.4 | 40.6 | 64.4 | 84.7 | 84.7 | 1 |
| TLS 1.2, нове | 20.5 | 40.7 | 84.7 | 105 | 105 | 1 |
| 5.3, перший запит | 21 | 40.6 | 64.7 | 85.1 | 85.1 | 1 |
| 5.3, другий запит | 0.02 | 0 | 0 | 20.4 | 20.5 | 0 |
| 5.3, третій запит | 0.02 | 0 | 0 | 20.3 | 20.5 | 0 |

RTT, визначений вимірюванням, і з якого значення його визначено: 20 мс. Визначено з namelookup (DNS) - 20 мс; TCP-з'єднання (connect - namelookup) - 20 мс; TLS 1.3 (appconnect - connect) - 20 мс; TLS 1.2 (appconnect - connect) - 40 мс (зазвичай на 1 RTT вище за TLS 1.3).

1. Виразіть кожне виміряне значення через RTT. Де вимірювання відрізняється від цілого числа RTT і чим це пояснюється?

| Етап | TLS 1.3, нове з'єднання | TLS 1.2, нове з'єднання | Повторне використання з'єднання |
|---|---|---|---|
| `time_namelookup` | 1 | 1 | 0 |
| `time_connect` | 2 | 2 | 0 |
| `time_appconnect` | 3 | 4 | 0 |
| `time_starttransfer` | 4 | 5 | 1 |

> Різниця від цілих значень RTT пояснюються витратами часу на обробку DNS-запиту, встановлення TCP-з'єднання, криптографічні операції TLS, обробку HTTP-запиту сервером. У першому рядку вимірювань TLS 1.3 namelookup становить 41 мс замість 20 мс. Мені здається, що додаткове RTT виникає через перше звернення до авторитетного DNS-сервера, що дублює затримку.
2. Чому `time_namelookup` не близький до нуля, хоча резолвер уже має ім'я в кеші? За яких умов він був би приблизно вдвічі більшим?
> Хоча кеш дуже допомагає з проблемою повторного пошуку адрес, кешування не може повністю стерти мережеву затримку. Запит до резолвера проходить через мережевий інтерфейс зі штучно встановленою затримкою, тоді навіть кешована відповідь потребує одного RTT. `time_namelookup` мог би бути вдвічі більше, якщо, наприклад, в кеші резолвера були б відсутні записи адрес. Тоді, він звертається до авторитетного DNS-сервера та стикається з тією ж штучною затримкою.
3. Сервіс виконує 200 послідовних запитів до цього сервера. Розрахуйте, скільки часу займе лише очікування мережі, якщо кожен запит встановлює нове з'єднання TLS 1.3, нове з'єднання TLS 1.2 і якщо всі запити йдуть через одне з'єднання.
> Для підрахувань штучну затримку на DNS буде враховано. Для TLS 1.3 - `84.7 * 200 = 16940 ms = 16.94 s`; TLS 1.2 - `105 * 200 = 21000 ms = 21 s`; одне з'єднання - `85.1 + (20.5 * 199) = 4164.6 ms = 4.2 s`

## Висновки

**Що виявилося несподіваним.** Результат, який не збігся з очікуванням, і рядок виводу, де це видно: Очікувала, що TLS 1.3 буде швидше за TLS 1.2 в прогнозі, оскільки TLS 1.3 буквально швидша та безпечніша TLS 1.2. Здивував однаковий час для namelookup та connect. Взагалі, спочатку зовсім здивувало, що namelookup зайняло 20 мс. Оскільки я очікувала, що так, резолвер вже має ім'я. Також спочатку здивувало, що повторні запити не були моментальними навіть для starttransfer, потім ще раз перечитала завдання з вказівкою про затримку. Все це можна побачити в таблицях завдання 5.

**Яке питання залишилося без відповіді.** Що саме не вдалося пояснити; до якого рядка виводу це стосується; що вже перевірено і що з'ясувати не вдалося: В процесі написання лабораторної роботи питань без відповідей не залишилося.

## Декларація використання ШІ

Використовувалися інструменти ШІ: ні
