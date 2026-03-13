
```markdown
# 🧠 Frontend Developer Knowledge Cheat Sheet

Краткая шпаргалка по ключевым темам для frontend-разработчика:

- ООП и SOLID
- Паттерны проектирования
- Clean Architecture
- Структуры данных и алгоритмы
- Async / Await
- REST API
- Web Security
- Browser Storage
- Микросервисная архитектура

---

# 📦 Object-Oriented Programming (OOP)

## Основные принципы

| Принцип | Описание |
|---|---|
| **Инкапсуляция** | Скрытие состояния объекта |
| **Наследование** | Создание классов на основе других |
| **Полиморфизм** | Один интерфейс — разные реализации |
| **Абстракция** | Выделение важных характеристик |

---

# 🧩 SOLID

| Принцип | Значение |
|---|---|
| **S** | Single Responsibility — одна ответственность |
| **O** | Open/Closed — открыт для расширения |
| **L** | Liskov Substitution — подклассы заменяют родителя |
| **I** | Interface Segregation — маленькие интерфейсы |
| **D** | Dependency Inversion — зависимость от абстракций |

Пример зависимости:

```

UserService → UserRepository (interface)

```

---

# 🏗 Design Patterns

## Creational

| Pattern | Назначение |
|---|---|
| Singleton | один экземпляр |
| Factory | создание объектов |
| Builder | поэтапное создание |

---

## Structural

| Pattern | Назначение |
|---|---|
| Adapter | преобразование интерфейса |
| Decorator | расширение функциональности |
| Facade | упрощение сложной системы |

---

## Behavioral

| Pattern | Назначение |
|---|---|
| Observer | подписка на события |
| Strategy | взаимозаменяемые алгоритмы |

---

# 🧱 Clean Architecture

Главная идея:

> Бизнес-логика не зависит от фреймворков.

## Слои архитектуры

```

+-----------------------+
|    Frameworks         |
|  (React, NestJS)     |
+-----------------------+
|  Interface Adapters   |
|  Controllers, API     |
+-----------------------+
|      Use Cases        |
|    Business Logic     |
+-----------------------+
|       Entities        |
|     Domain Models     |
+-----------------------+

```

## Dependency Rule

Зависимости направлены **внутрь**.

```

Framework → Adapters → UseCases → Entities

````

---

# ⚙️ Algorithms & Big O

## Временная сложность

| Complexity | Meaning |
|---|---|
| O(1) | constant |
| O(log n) | logarithmic |
| O(n) | linear |
| O(n²) | quadratic |

---

# 📚 Data Structures

| Structure | Использование |
|---|---|
| Array | быстрый доступ |
| Stack | undo / recursion |
| Queue | task processing |
| Hash Table | быстрый поиск |
| Tree | DOM |
| Graph | маршруты |

---

# ⚡ Async / Await

## Ошибка

```javascript
array.forEach(async item => {
 await fetch(item)
})
````

`forEach` не ожидает `await`.

---

## Правильно

```javascript
for (const item of array) {
 await fetch(item)
}
```

или

```javascript
await Promise.all(array.map(fetch))
```

---

# 🌐 REST API

## HTTP Methods

| Method | Purpose              |
| ------ | -------------------- |
| GET    | получение данных     |
| POST   | создание             |
| PUT    | обновление           |
| PATCH  | частичное обновление |
| DELETE | удаление             |

---

## REST endpoints

```
GET /users
GET /users/1
POST /users
DELETE /users/1
```

---

## HTTP Status Codes

| Code | Meaning      |
| ---- | ------------ |
| 200  | OK           |
| 201  | Created      |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |
| 500  | Server Error |

---

# 🔐 Web Security

## XSS (Cross-Site Scripting)

Вставка JS в страницу.

```html
<script>alert("XSS")</script>
```

Защита:

* escaping
* sanitization
* CSP

---

## CSRF (Cross-Site Request Forgery)

Поддельный запрос от имени пользователя.

Защита:

* CSRF token
* SameSite cookies

---

## SQL Injection

```
SELECT * FROM users WHERE id = 1 OR 1=1
```

Защита:

* prepared statements
* ORM

---

# 🔑 HTTPS / TLS

HTTPS =

```
HTTP + TLS
```

TLS обеспечивает:

* encryption
* authentication
* integrity

---

## TLS Handshake

1. Client Hello
2. Server Certificate
3. CA verification
4. Key exchange
5. Session key

---

# 💾 Browser Storage

## LocalStorage

* ~5MB
* synchronous
* string only

```javascript
localStorage.setItem("key", "value")
```

---

## SessionStorage

Живёт **пока открыта вкладка**.

---

## IndexedDB

Асинхронная база данных.

Подходит для:

* PWA
* offline apps
* больших данных

Поддерживает:

* индексы
* транзакции

---

# 🧩 Microservices

Особенности:

* независимые сервисы
* отдельный deploy
* собственная база данных

---

## Communication

```
HTTP
gRPC
Message Queue
```

---

# 🧪 Testing

Типы тестов:

| Type        | Purpose             |
| ----------- | ------------------- |
| Unit        | тест функции        |
| Integration | тест взаимодействия |
| E2E         | тест приложения     |

---

# 🚀 Most Asked Interview / Test Questions

1. Принципы **SOLID**
2. Паттерны **Observer / Factory / Singleton**
3. **Clean Architecture layers**
4. **Big O**
5. `async/await` в циклах
6. REST принципы
7. TLS handshake
8. XSS vs CSRF
9. LocalStorage vs IndexedDB
10. Unit vs Integration тесты

---

⭐ Используй этот README как шпаргалку перед тестами и интервью.

```
```
