# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент | Тодорович В. В. |
| Група | КБЗІ-2.02 |
| Номер варіанта | 26 |
| Індивідуальний домен | iana.org |
| «Чужий» домен для завдання A.3.1 | nbuv.gov.ua |
| Середовище виконання | Killercoda Ubuntu Playground |
| Дата виконання | 06.10.26 |

---

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```
nc -C iana.org 80
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: iana.org
Connection: close

```

**Відповідь:**

```
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 16:10:49 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 16:10:49 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

---

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc iana.org 80
```

**Вивід:**

```
HTTP/1.1 400 Bad Request
Date: Tue, 06 Oct 2026 16:35:36 GMT
Server: Apache
Content-Length: 347
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
<p>Additionally, a 400 Bad Request
error was encountered while trying to use an ErrorDocument to handle the request.</p>
</body></html>
```

---

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: nbuv.gov.ua\r\nConnection: close\r\n\r\n' | nc iana.org 80
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 16:40:18 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 16:40:18 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc iana.org 80
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 17:09:38 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 17:09:38 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```
printf 'GET / HTTP/1.0\r\n\r\n' | nc iana.org 80
```

**Вивід:**

```
HTTP/1.1 302 Found
Date: Tue, 06 Oct 2026 17:15:43 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 17:15:43 GMT
Content-Length: 205
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

Зведення результатів наведено в **Додатку Д**.

---

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: iana.org\r\n\r\nGET / HTTP/1.1\r\nHost: iana.org\r\nConnection: close\r\n\r\n' | nc -C iana.org 80
```

**Вивід:**

```
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 17:21:31 GMT
Server: Apache
Location: https://www.iana.org/opism-pr02-12345
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 17:21:31 GMT
Content-Length: 245
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/opism-pr02-12345">here</a>.</p>
</body></html>
HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 17:21:31 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 17:21:31 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
```

**Кількість отриманих відповідей:** 2.

**Коди стану отриманих відповідей:** 301; 301.

---

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда:**

```
curl -v --http1.1 http://iana.org/ -o /dev/null
```

**Вивід:**

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host iana.org:80 was resolved.
* IPv6: 2001:500:88:200::8
* IPv4: 192.0.43.8
*   Trying 192.0.43.8:80...
* Connected to iana.org (192.0.43.8) port 80
> GET / HTTP/1.1
> Host: iana.org
> User-Agent: curl/8.5.0
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Date: Tue, 06 Oct 2026 18:03:39 GMT
< Server: Apache
< Location: https://www.iana.org/
< Cache-Control: max-age=345600
< Expires: Sat, 10 Oct 2026 18:03:39 GMT
< Content-Length: 229
< Content-Type: text/html; charset=iso-8859-1
< 
{ [229 bytes data]
100   229  100   229    0     0    890      0 --:--:-- --:--:-- --:--:--   891
* Connection #0 to host iana.org left intact
```

---

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** `iana.org`

**Команда:**

```
openssl s_client -connect iana.org:443 -servername iana.org -crlf -quiet
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: iana.org
Connection: close

```

**Вивід:**

```
depth=2 C = GB, O = Sectigo Limited, CN = Sectigo Public Server Authentication Root R46
verify return:1
depth=1 C = GB, O = Sectigo Limited, CN = Sectigo Public Server Authentication CA OV R36
verify return:1
depth=0 C = US, ST = California, O = Internet Corporation For Assigned Names and Numbers, CN = *.iana.org
verify return:1
GET / HTTP/1.1
Host: iana.org
Connection: close

HTTP/1.1 301 Moved Permanently
Date: Tue, 06 Oct 2026 18:10:51 GMT
Server: Apache
Location: https://www.iana.org/
Cache-Control: max-age=345600
Expires: Sat, 10 Oct 2026 18:10:51 GMT
Content-Length: 229
Connection: close
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>
</body></html>
40B7F88EF2710000:error:0A000126:SSL routines:ssl3_read_n:unexpected eof while reading:../ssl/record/rec_layer_s3.c:316:
```

---

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:**

| № | Поле заголовка | Значення | Призначення | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | Date | Tue, 06 Oct 2026 16:10:49 GMT | Час надання сервером відповіді | Сервер | Сервер формує відповідь та встановлює час її надання |
| 2 | Server | Apache | Який сервер (програма) відповіла | Сервер | Оскільки це сервер, що оброблює запит, заголовок був встановлений сервером |
| 3 | Location | `https://www.iana.org/` | Куди звертається клієнт | Сервер | Сервер знаходить сайт, в який потрібно перенаправити клієнта |
| 4 | Cache-Control | max-age=345600 | Скільки зберігається відповідь для повторного використання | Сервер | Сервер встановлює правило використання кеша клієнтом та проксі |
| 5 | Expires | Sat, 10 Oct 2026 16:10:49 GMT | Коли термін зберігання відповіді закінчується | Проксі | Проксі перетворює необроблені дані на читабельний варіант користувачу |
| 6 | Content-Length | 229 | Розмір тіла відповіді | Проксі | Проксі отримує тіло відповіді та підраховує його розмір |
| 7 | Connection | close | Інструкція, що робити з зв'язком | Проксі | Базуючись на інструкції клієнта проксі встановлює, що робити з зв'язком |
| 8 | Content-Type | text/html; charset=iso-8859-1 | Правила виводу тіла | Сервер | Сервер передає правильний формат виводу тіла |

---

## Частина D. Висновки

**D.1.** Що з поведінки сервера виявилося неочевидним або несподіваним. Конкретно, з посиланням на рядок виводу.

На мою думку, цей етап більш відноситься до поведінки клієнту, ніж серверу, але теж хочу занотувати даний момент та отримати на нього відповідь. В частині А.1 після отримання відповіді на запит не можливо було взаємодіяти з консоллю. Тобто, три останні рядки:
`<h1>Moved Permanently</h1>`<br>
`<p>The document has moved <a href="https://www.iana.org/">here</a>.</p>`<br>
`</body></html>`<br>
Та все. З минулої практичної я можу очікувати ще один рядок, що повідомить про статус з'єднання та завершення роботи. Примітка завдання А.1 цієї практичної також встановлює, без специфічного поля набраного запиту з'єднання залишається та його потрібно завершити комбінацією клавиш. В моєму запиті строка про з'єднання присутня, але для продовження використання консолі все ще потрібно було натиснути Ctrl+C. Далі, А.3.1:<br>
`HTTP/1.1 302 Found`<br>
Отримано відповідь, яку не було занотовано в примітці А.3. Нормальна відповідь встановлює, що ресурс повністю змігрував, тоді, як можна отримати відповідь, що ресурс змінився тільки тимчасово? Якщо вгадати, то це помилка цього неправильного вводу поля команди. Те ж саме з А.3.2 та А.3.3:<br>
`HTTP/1.1 302 Found`

**D.2.** Яке з полів заголовка викликало найбільше утруднення при визначенні походження (частина B) та з якої причини.

Поля заголовків, які я позначила "Проксі". Мені здається, що вони можуть бути як з боку сервера, як з боку проміжного вузла. Оскільки всі заголовки можуть походити від сервера, було важко виділити поля заголовків саме від проксі.

**D.3.** Яке питання залишилося без відповіді після виконання роботи.

Чому після виконання запиту А.1 не можливо було одразу звернутися до консолі. Чому некоректні запити видають статус код 302.

---

## Контрольні питання

**1.** У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

HTTP-запит закінчується порожнім рядком. В формі fprint це можна побачити як "\r\n\r\n".

**2.** Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3. За яких значень поля `Host` і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле `Host`? Відповідь має посилатися на конкретні рядки ваших виводів.

В моєму випадку, сервер не обслуговував запит лише тоді, коли запит був неправильно сформован, тобто було відсутнє поле `Host`. Лише для іншого протоколу HTTP (1.0 замість 1.1) поле `Host` не було потрібне для обслуговування запиту. Поле `Host` визначає хост, до якого адресований запит. Якщо правильно розумію, дане поле допомагає у випадку, коли декілька ресурсів розташовано на одному сервері. `Host` в такому випадку допоможе встановити, до кого саме адресовано запит.<br>
Випадки, коли обслуговано запити:<br>
<ul><ol>- A.1<br>
HTTP/1.1 301 Moved Permanently</ol>
<ol>- A.3.1<br>
HTTP/1.1 302 Found</ol>
<ol>- A.3.2<br>
HTTP/1.1 302 Found</ol>
<ol>- A.3.3<br>
HTTP/1.1 302 Found</ol></ul>
Випадки, коли запити не було обслуговано:<br>
<ul><ol>- A.2<br>
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc iana.org 80<br>
HTTP/1.1 400 Bad Request</ol></ul>

**3.** Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху — і що це говорить про роль цього сервера? Якщо надійшла одна відповідь, знайдіть у ній поле заголовка, яке це пояснює, або зазначте, що такого поля немає. Якщо надійшло дві — що це означає для клієнтської програми, яка завантажує сторінку з великою кількістю вкладених ресурсів?

Надійшло дві відповіді, обидві з кодами 301. Відповідь мого серверу на порту 80 не залежала від запитаного шляху, оскільки було отримано однаковий результат, окрем хедера `Location` та вмісту тіла, саме "де" знаходиться оновлений ресурс. Оскільки надійшло дві відповіді, це означає, що сервер підтримує послідовні HTTP-запити. Якщо сторінка містить велику кількість вкладених ресурсів, це означає, що для одного й того ж з'єднання не потрібно знову встановлювати з'єднання для перегляду ресурсів сторінки. Тобто клієнт може виконати декілька різних запитів без потреби перепідключатися.

**4.** Які поля заголовка програма `curl` додала самостійно (завдання A.5)? Ці поля не є обов'язковими — сервер відповів і без них у завданні A.1. З якою метою їх додано?

`User-Agent: curl/8.5.0`<br>
Повідомлення серверу, яка утиліта та її версія була використана в запиті. Додано з метою ведення статистики.<br>
`Accept: */*`<br>
Повідомлення серверу, що клієнт може отримати будь-який тип медіа. Додано з метою встановлення сервером, який формат даних клієнт може обробити.

**5.** За якими ознаками у вашому виводі виявляється присутність проміжного вузла? Якщо таких ознак не виявлено, поясніть, що з цього випливає.

Чітких ознак, як в минулій практичній (наприклад, `X-Content-Type-Options`), немає. Якщо ознак немає, це може вказати на відсутність проміжного вузла. За даним фактом також можна встановити, що в таблиці частини В замість "проксі" повинні стояти "сервер", або "не виявлено". Я більш схиляюся до "не виявлено", бо це виглядає, як інформація, оброблена клієнтом чітко для перегляду та засвоєння користувачем. Звісно, це лише непрофесійна думка.

**6.** Три рядки, про які не йшлося на лекції, наведено в **Додатку В**.

---

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело |
|---|---|---|
| 1 | `Cache-Control: max-age=345600` | A.1 |
| 2 | `User-Agent: curl/8.5.0` | A.5 |
| 3 | `depth=2 C = GB, O = Sectigo Limited, CN = Sectigo Public Server Authentication Root R46` | A.6 |

---

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання:** iana.org

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 |
|---|---|---|---|---|---|
| A.1 | `iana.org` | 1.1 | 301 | 229 | — |
| A.2 | поле відсутнє | 1.1 | 400 | 347 | ні |
| A.3.1 | `nbuv.gov.ua` | 1.1 | 302 | 205 | ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 | 302 | 205 | ні |
| A.3.3 | поле відсутнє | 1.0 | 302 | 205 | ні |

**Висновок за таблицею (2–4 речення):**

У пробах змінювались наявність та зміст поля `Host`, останнім змінилась версія протоколу HTTP. При відсутності `Host` сервер видає помилку поганого запиту, однакЮ якщо версія HTTP 1.0, дане поле не обов'язкове та запит оброблюється. При сторонньому `Host`, а також під час запиту з HTTP 1.0, замість 301 видає код 302, тобто запит оброблюється, але з інтересним результатом.

---

## Декларування використання технологій штучного інтелекту

Для цієї роботи встановлено **рівень Р3 — ШІ як співвиконавець**.

**Чи використовувалися технології ШІ під час виконання роботи:** Ні

**Підтвердження:** усі виводи команд, наведені в частині A, отримано внаслідок фактичного виконання команд на зазначеному індивідуальному домені.
