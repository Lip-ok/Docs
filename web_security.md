
---

### Пост 1: SQL-инъекции — классика, которая всё ещё ломает сайты

SQL-инъекция возникает, когда пользовательский ввод напрямую вставляется в запрос.

**Плохо:**

```js
const query = `SELECT * FROM users WHERE email = '${email}'`;
```

Если пользователь введёт:

```
' OR 1=1 --
```

Запрос станет:

```sql
SELECT * FROM users WHERE email = '' OR 1=1 --'
```

➡️ Вернёт всех пользователей.

**Хорошо:**

```js
const query = 'SELECT * FROM users WHERE email = ?';
db.execute(query, [email]);
```

**Вывод:** всегда используйте параметризованные запросы или ORM.

---

### Пост 2: XSS — когда пользователь пишет JavaScript за тебя

Cross-Site Scripting — это внедрение JS-кода в страницу.

**Плохо:**

```js
res.send(`<h1>${userInput}</h1>`);
```

Пользователь вводит:

```html
<script>alert('hacked')</script>
```

➡️ Скрипт выполнится у всех.

**Хорошо:**

```js
import escape from 'escape-html';
res.send(`<h1>${escape(userInput)}</h1>`);
```

**Вывод:** экранируйте ввод и используйте безопасный рендеринг.

---

### Пост 3: Пароли — никогда не храни их как есть

**Плохо:**

```js
db.save({ email, password });
```

Если база утечёт — все аккаунты скомпрометированы.

**Хорошо:**

```js
import bcrypt from 'bcrypt';

const hash = await bcrypt.hash(password, 10);
db.save({ email, password: hash });
```

**Вывод:** всегда хэшируйте пароли (bcrypt/argon2).

---

### Пост 4: JWT — не доверяй клиенту слепо

**Плохо:**

```js
const user = jwt.decode(token);
```

➡️ decode не проверяет подпись!

**Хорошо:**

```js
const user = jwt.verify(token, SECRET);
```

**Вывод:** всегда проверяйте подпись токена.

---

### Пост 5: CORS — не открывай всё подряд

**Плохо:**

```js
app.use(cors({ origin: '*' }));
```

➡️ Любой сайт может делать запросы к твоему API.

**Хорошо:**

```js
app.use(cors({
  origin: 'https://myapp.com'
}));
```

**Вывод:** ограничивай доступ к API.

---

### Пост 6: Rate limiting — защита от брутфорса

Без ограничений можно перебирать пароли бесконечно.

**Решение:**

```js
import rateLimit from 'express-rate-limit';

app.use(rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
}));
```

**Вывод:** ограничивай количество запросов.

---

### Пост 7: HTTPS — не опция, а обязательство

Если сайт работает по HTTP:

➡️ Пароли и токены можно перехватить.

**Решение:**

* Используй HTTPS
* Включи HSTS

```js
app.use(helmet());
```

**Вывод:** всегда шифруй трафик.

---

### Пост 8: Environment variables — не храни секреты в коде

**Плохо:**

```js
const SECRET = "supersecret123";
```

➡️ Утечка кода = утечка ключей.

**Хорошо:**

```js
const SECRET = process.env.JWT_SECRET;
```

**Вывод:** все секреты — только в env.

---

### Пост 9: Проверка данных — доверяй, но проверяй

**Плохо:**

```js
app.post('/user', (req) => {
  db.save(req.body);
});
```

➡️ Можно отправить что угодно.

**Хорошо:**

```js
import Joi from 'joi';

const schema = Joi.object({
  email: Joi.string().email().required()
});
```

**Вывод:** валидируй входные данные.

---

### Пост 10: Ошибки — не раскрывай лишнего

**Плохо:**

```js
res.send(err.stack);
```

➡️ Показывает структуру сервера.

**Хорошо:**

```js
res.status(500).send('Something went wrong');
```

**Вывод:** логируй ошибки, но не показывай их пользователю.

---



* сделать продолжение (CSRF, SSRF, file upload, OAuth)
* или переписать это в формат для Telegram / LinkedIn / карусели в Instagram
