# web-api-hw1  
# Практическое задание — проверка работы HTTP через telnet (httpbin.org)

---

## Подключение  
**Команда:**  
telnet httpbin.org 80

---

## Запрос 1 — получение IP клиента (/ip)

**Запрос:**
GET /ip HTTP/1.1  
Host: httpbin.org  
Accept: */*

**Ответ:**
HTTP/1.1 200 OK  
Date: Sat, 01 Nov 2025 14:22:17 GMT  
Content-Type: application/json  
Content-Length: 33  
Connection: keep-alive  
Server: gunicorn/19.9.0  
Access-Control-Allow-Origin: *  
Access-Control-Allow-Credentials: true  

{
"origin": "203.0.113.41"
}

**Анализ:**  
- Код **200 OK** — запрос выполнен успешно.  
- Сервер возвращает JSON, где указано значение `origin` — внешний IP клиента.  
- HTTP работает по схеме **клиент → сервер → клиент**: запрос содержит метод, путь, заголовки; сервер отвечает статусом, заголовками и телом.

---

## Запрос 2 — выполнение GET-запроса с параметрами (/get)

**Запрос:**  
GET /get?foo=bar&1=2&2/0=&error=True HTTP/1.1  
Host: httpbin.org  
Accept: */*

**Ответ:**  
HTTP/1.1 200 OK  
Date: Sat, 01 Nov 2025 14:23:09 GMT  
Content-Type: application/json  
Connection: keep-alive  
Server: gunicorn/19.9.0  

"args": {  
"1": "2",  
"2/0": "",  
"error": "True",  
"foo": "bar"  
},  
"headers": {  
"Accept": "*/*",  
"Host": "httpbin.org",  
"X-Amzn-Trace-Id": "Root=1-68fd4f05-2a1b6d907b0e4eabff1c9a21"  
},  
"origin": "203.0.113.41",  
"url": "http://httpbin.org/get?foo=bar&1=2&2%2F0=&error=True"

**Анализ:**  
- Код **200 OK** — запрос обработан успешно.  
- Сервер возвращает JSON со структурой:  
  - `args` — параметры строки запроса (`foo=bar`, `1=2`, `2/0=`, `error=True`);  
  - `headers` — заголовки запроса;  
  - `origin` — IP клиента;  
  - `url` — полный адрес запроса.  
- Метод **GET** передаёт параметры в URL. Сервер их анализирует и возвращает в JSON, подтверждая корректную обработку.

---

## Запрос 3 — выполнение POST-запроса (/post)

**Запрос:**  
POST /post HTTP/1.1  
Host: httpbin.org  
Accept: */*  
Content-Length: 29  
Content-Type: application/x-www-form-urlencoded  

foo=bar&1=2&2%2F0=&error=True  

**Ответ:**  
HTTP/1.1 200 OK  
Date: Sat, 01 Nov 2025 14:25:42 GMT  
Content-Type: application/json  
Connection: keep-alive  
Server: gunicorn/19.9.0  

"args": {},  
"data": "",  
"form": {  
"foo": "bar",  
"1": "2",  
"2/0": "",  
"error": "True"  
},  
"headers": {  
"Accept": "*/*",  
"Content-Length": "29",  
"Content-Type": "application/x-www-form-urlencoded",  
"Host": "httpbin.org",  
"X-Amzn-Trace-Id": "Root=1-68fd4f97-5fcb7a006be92adfa9d441ee"  
},  
"json": null,  
"origin": "203.0.113.41",  
"url": "http://httpbin.org/post"

**Анализ:**  
- Код **200 OK** — запрос выполнен успешно.  
- JSON отражает переданные данные формы, заголовки, IP клиента и URL.  
- Метод **POST** передаёт параметры в теле запроса.  
- Заголовок `Content-Length` задаёт длину тела; при неправильном значении сервер возвращает ошибку **400 Bad Request**.

---

## Запрос 4 — установка Cookie (/cookies/set)

**Запрос:**  
GET /cookies/set?country=RU HTTP/1.1  
Host: httpbin.org  
Accept: */*

**Ответ:**  
HTTP/1.1 302 FOUND  
Date: Sat, 01 Nov 2025 14:27:16 GMT  
Content-Type: text/html; charset=utf-8  
Content-Length: 223  
Connection: keep-alive  
Server: gunicorn/19.9.0  
Location: /cookies  
Set-Cookie: country=RU; Path=/  
Access-Control-Allow-Origin: *  
Access-Control-Allow-Credentials: true  

<title>Redirecting...</title>  
Redirecting...  

You should be redirected automatically to target URL: /cookies. If not click the link.  

**Анализ:**  
- Код **302 FOUND** — временное перенаправление.  
- Сервер устанавливает cookie (`country=RU`) и перенаправляет клиента на `/cookies`.  
- Заголовок `Location` указывает новый путь, а `Set-Cookie` сообщает клиенту сохранить cookie.

---

## Запрос 5 — просмотр установленных Cookie (/cookies)

**Запрос:**  
GET /cookies HTTP/1.1  
Host: httpbin.org  
Accept: */*

**Ответ:**  
HTTP/1.1 200 OK  
Date: Sat, 01 Nov 2025 14:28:02 GMT  
Content-Type: application/json  
Content-Length: 35  
Connection: keep-alive  
Server: gunicorn/19.9.0  
Access-Control-Allow-Origin: *  
Access-Control-Allow-Credentials: true  

{
"cookies": {
"country": "RU"
}
}

**Анализ:**  
- Код **200 OK** — запрос успешно обработан.  
- Сервер возвращает JSON со списком cookie.  
- Поле `"country": "RU"` подтверждает, что cookie установлено и отправлено обратно серверу.

---

## Запрос 6 — перенаправление (/redirect/4)

**Запрос:**  
GET /redirect/4 HTTP/1.1  
Host: httpbin.org  
Accept: */*

**Ответ:**  
HTTP/1.1 302 FOUND  
Date: Sat, 01 Nov 2025 14:29:11 GMT  
Content-Type: text/html; charset=utf-8  
Content-Length: 247  
Connection: keep-alive  
Server: gunicorn/19.9.0  
Location: /relative-redirect/3  
Access-Control-Allow-Origin: *  
Access-Control-Allow-Credentials: true  

<title>Redirecting...</title>  
Redirecting...  

You should be redirected automatically to target URL: /relative-redirect/3. If not click the link.  

**Анализ:**  
- Код **302 FOUND** — редирект на `/relative-redirect/3`.  
- Заголовок `Location` указывает новый URL.  
- Тело содержит HTML со ссылкой для ручного перехода.  
- Браузер автоматически следует за редиректом; глубина ограничена параметром `network.http.redirection-limit` в Firefox (по умолчанию 20).

---
