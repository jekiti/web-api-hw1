
# web-api-hw1  
## Задание 4 — Отправка формы multipart/form-data на http://httpbin.org/post

---

### Ответ на форму
```json
{
  "args": {},
  "data": "",
  "files": {
    "myimg": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEA...<обрезано>"
  },
  "form": {
    "firstname": "Григорий",
    "lastname": "Воронцов",
    "group": "РИ-330943",
    "message": "Тестовая отправка данных через форму"
  },
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7",
    "Cache-Control": "no-cache",
    "Content-Length": "247182",
    "Content-Type": "multipart/form-data; boundary=----WebKitFormBoundaryA4dG7xZtY9v2K2H",
    "Host": "httpbin.org",
    "Origin": "null",
    "Pragma": "no-cache",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36",
    "X-Amzn-Trace-Id": "Root=1-68fd7f3e-2c1237bb4e19f4df6e9fa7a1"
  },
  "json": null,
  "origin": "203.0.113.41",
  "url": "http://httpbin.org/post"
}
```

---

### Анализ
- **args:** Пустой объект — метод POST передаёт данные в теле запроса, а не в URL.  
- **data:** Пусто, потому что multipart разбивает тело на части, а не хранит всё одной строкой.  
- **files:** Поле `myimg` содержит файл, закодированный в base64. Сервер корректно разобрал multipart-запрос.  
- **form:** Текстовые поля формы (`firstname`, `lastname`, `group`, `message`) пришли корректно.  
- **headers:**  
  - `Content-Type` указывает формат тела — `multipart/form-data` с boundary.  
  - `Content-Length` — длина тела запроса (включая файл).  
  - Остальные заголовки (`Host`, `User-Agent`, `Accept`) — стандартные для браузера.  
- **json:** `null`, потому что данные не отправлялись как JSON.  
- **origin:** IP клиента.  
- **url:** URL, на который был отправлен POST-запрос.  
- **Код 200 OK:** сервер успешно обработал запрос.

---

### Принцип работы HTTP
1. Клиент формирует **POST-запрос** на `http://httpbin.org/post`.  
2. Тело запроса кодируется в формате **multipart/form-data** — каждая часть (поле или файл) отделена границей (`boundary`).  
3. Сервер получает тело, парсит его на текстовые и бинарные части.  
4. Все данные возвращаются клиенту в виде JSON с полями `form`, `files`, `headers`.  
5. Клиент получает ответ `200 OK`, подтверждающий успешную обработку данных.

---
