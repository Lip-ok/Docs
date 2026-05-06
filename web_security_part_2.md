
---

### 11: CSRF — атака через “доверие браузера”

CSRF (Cross-Site Request Forgery) — это когда пользователь уже авторизован, а злоумышленник заставляет его браузер отправить запрос.

**Сценарий:**
Ты залогинен в банке. Открываешь вредоносный сайт — и он отправляет запрос:

```html
<form action="https://bank.com/transfer" method="POST">
  <input type="hidden" name="amount" value="1000">
</form>
<script>
  document.forms[0].submit();
</script>
```

➡️ Запрос уйдёт с твоими cookies.

**Защита:**

```js
import csrf from 'csurf';

app.use(csrf());
```

И на клиенте:

```html
<input type="hidden" name="_csrf" value="{{token}}">
```

**Вывод:** всегда используй CSRF-токены + SameSite cookies.

---

### 12: SSRF — когда сервер ходит “куда не надо”

SSRF (Server-Side Request Forgery) — атака, при которой сервер делает запрос по URL, заданному пользователем.

**Плохо:**

```js
app.get('/fetch', async (req, res) => {
  const data = await fetch(req.query.url);
  res.send(await data.text());
});
```

Пользователь может указать:

```id="8v9f8g"
http://localhost:3000/admin
```

или даже:

```id="o0jz6h"
http://169.254.169.254/latest/meta-data/
```

➡️ Получит доступ к внутренним ресурсам.

**Защита:**

* whitelist доменов
* запрет внутренних IP
* парсинг URL

```js
const allowedHosts = ['api.myapp.com'];
```

**Вывод:** никогда не доверяй URL от пользователя.

---

### 13: File Upload — загрузка, которая ломает сервер

Проблема: пользователь может загрузить что угодно.

**Плохо:**

```js
app.post('/upload', upload.single('file'), (req, res) => {
  res.send('ok');
});
```

➡️ Можно загрузить `.exe`, `.php` или огромный файл.

**Риски:**

* RCE (выполнение кода)
* переполнение диска
* XSS через SVG

**Хорошо:**

```js
const upload = multer({
  limits: { fileSize: 2 * 1024 * 1024 },
  fileFilter: (req, file, cb) => {
    if (!file.mimetype.startsWith('image/')) {
      return cb(new Error('Only images allowed'));
    }
    cb(null, true);
  }
});
```

**Дополнительно:**

* генерируй свои имена файлов
* храни вне публичной папки

**Вывод:** файл = потенциальная атака.

---

### 14: OAuth — вход через Google не делает тебя безопасным

Многие думают: “у меня OAuth — значит всё ок”.

Не совсем.

**Плохо:**

```js
const user = await getUserFromGoogle(token);
login(user.email);
```

➡️ Ты доверяешь email без проверки.

**Риски:**

* подмена токена
* отсутствие проверки `aud`, `iss`
* отсутствие state (CSRF)

**Хорошо:**

```js
const ticket = await client.verifyIdToken({
  idToken: token,
  audience: CLIENT_ID,
});

const payload = ticket.getPayload();
```

И обязательно:

* проверяй `aud`, `iss`
* используй `state`
* не доверяй просто email

**Вывод:** OAuth — это не “магия”, а ещё один слой, который нужно правильно реализовать.

---

