# web-api-hw1

# Задание 2

## Цель
Подключиться к веб-серверу по протоколу **HTTP/1.1** с помощью **telnet**, отправить запрос, получить ответ и проанализировать работу HTTP-протокола.

---

## Подключение

**Сайт:** `example.com`  
**Команда для подключения:**
```bash
telnet example.com 80
```
Запрос:

GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Connection: close

Ответ сервера
HTTP/1.1 200 OK
Content-Type: text/html
ETag: "bc2473a18e003bdb249eba5ce893033f:1760028122.592274"
Last-Modified: Thu, 09 Oct 2025 16:42:02 GMT
Cache-Control: max-age=86000
Date: Fri, 24 Oct 2025 09:42:44 GMT
Content-Length: 513
Connection: close
X-N: S

<!doctype html><title>Example Domain</title><style>body{background:#eee;width:60vw;margin:15vh auto;font-family:system-ui,sans-serif}h1{font-size:1.5em}div{opacity:0.8}a:link,a:visited{color:#348}</style>

Example Domain
This domain is for use in documentation examples without needing permission. Avoid use in operations.

Learn more

Анализ
1. Строка статуса

HTTP/1.1 — версия протокола;

200 OK — код состояния (успешный ответ).

2. Заголовки
| Заголовок        | Назначение                                       |
| ---------------- | ------------------------------------------------ |
| `Content-Type`   | Тип контента — HTML-документ                     |
| `Content-Length` | Длина тела ответа в байтах                       |
| `Date`           | Дата и время формирования ответа                 |
| `Last-Modified`  | Когда страница последний раз изменялась          |
| `ETag`           | Идентификатор версии ресурса                     |
| `Cache-Control`  | Время хранения кэша                              |
| `Connection`     | Политика соединения (в данном случае — закрытие) |

3. Тело ответа

После пустой строки начинается HTML-код страницы, который браузер затем визуализирует.
В данном случае это простая демонстрационная страница “Example Domain”.

Принцип работы HTTP
- Клиент устанавливает TCP-соединение с сервером (порт 80).

- Отправляет текстовый запрос (метод, путь, заголовки).

- Сервер обрабатывает запрос и возвращает:

- строку статуса;

- заголовки;

- тело ответа (HTML).

- После передачи контента сервер закрывает соединение.

Вывод

HTTP-протокол использует модель запрос → ответ поверх TCP.
Клиент формирует запрос, сервер отвечает кодом состояния, заголовками и данными.
В нашем случае код 200 OK подтверждает успешную обработку запроса,
а тело содержит HTML-страницу, переданную в открытом виде.
