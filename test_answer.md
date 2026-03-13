Вот версия **ультра-короткой шпаргалки в формате `README.md`** — можно прямо вставить в репозиторий GitHub.

```markdown
# 🧠 Frontend Architecture & Security Cheat Sheet

Краткая шпаргалка по:
- ООП
- паттернам проектирования
- чистой архитектуре
- алгоритмам
- безопасности
- REST API
- хранению данных в браузере

---

# 1. ООП

## Основные принципы

**Инкапсуляция**  
Скрытие внутреннего состояния объекта.

**Наследование**  
Создание нового класса на основе существующего.

**Полиморфизм**  
Одинаковый интерфейс — разная реализация.

**Абстракция**  
Выделение важных характеристик и скрытие деталей.

---

# 2. SOLID

| Принцип | Значение |
|---|---|
| **S** | Single Responsibility — 1 класс = 1 ответственность |
| **O** | Open/Closed — открыт для расширения, закрыт для изменения |
| **L** | Liskov Substitution — подкласс должен заменять родителя |
| **I** | Interface Segregation — много маленьких интерфейсов |
| **D** | Dependency Inversion — зависимость от абстракций |

---

# 3. Паттерны проектирования

## Порождающие

**Singleton**  
Один экземпляр объекта.

**Factory**  
Создание объектов через фабрику.

**Builder**  
Пошаговое создание сложного объекта.

---

## Структурные

**Adapter**  
Приводит интерфейс к нужному виду.

**Decorator**  
Добавляет функциональность объекту.

**Facade**  
Упрощает интерфейс сложной системы.

---

## Поведенческие

**Observer**  
Подписка на события.

**Strategy**  
Взаимозаменяемые алгоритмы.

---

# 4. Чистая архитектура (Clean Architecture)

Главная идея:

> Бизнес-логика не должна зависеть от фреймворков.

## Слои

```

Entities
Use Cases
Interface Adapters
Frameworks

```

## Правило зависимостей

```

Framework → UseCases → Entities

```

Зависимости направлены **внутрь**.

---

# 5. Big O

| Сложность | Значение |
|---|---|
| O(1) | константная |
| O(log n) | логарифмическая |
| O(n) | линейная |
| O(n²) | квадратичная |

---

# 6. Основные структуры данных

## Array
- доступ: `O(1)`
- поиск: `O(n)`

## Stack
LIFO

```

push
pop

```

## Queue
FIFO

```

enqueue
dequeue

```

## Hash Table

```

Map
Object

```

Средняя сложность операций:

```

O(1)

````

## Tree

Пример: **DOM**

---

# 7. Async / Await

❌ Ошибка

```javascript
array.forEach(async item => {
 await fetch(item)
})
````

`forEach` не ждёт `await`.

✅ Правильно

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

# 8. REST API

## HTTP методы

```
GET
POST
PUT
PATCH
DELETE
```

## Пример REST

```
GET /users
GET /users/1
POST /users
DELETE /users/1
```

---

## HTTP коды

| Код | Значение     |
| --- | ------------ |
| 200 | OK           |
| 201 | Created      |
| 400 | Bad Request  |
| 401 | Unauthorized |
| 403 | Forbidden    |
| 404 | Not Found    |
| 500 | Server Error |

---

# 9. HTTPS / TLS

HTTPS =

```
HTTP + TLS
```

TLS обеспечивает:

* шифрование
* аутентификацию
* целостность данных

## TLS Handshake

1. Client Hello
2. Server Certificate
3. Проверка CA
4. Обмен ключами
5. Session Key

---

# 10. Основные веб-атаки

## XSS

Вставка JS в страницу.

```html
<script>alert(1)</script>
```

Защита:

* escaping
* sanitization
* CSP

---

## CSRF

Поддельный запрос от имени пользователя.

Защита:

* CSRF token
* SameSite cookies

---

## SQL Injection

```
OR 1=1
```

Защита:

* prepared statements
* ORM

---

# 11. Хранение данных в браузере

## LocalStorage

* ~5MB
* синхронный API
* хранит строки

```javascript
localStorage.setItem("key", "value")
```

---

## SessionStorage

Живёт **пока открыта вкладка**.

---

## IndexedDB

Асинхронная база данных браузера.

Подходит для:

* PWA
* offline apps
* больших данных

Особенности:

* индексы
* транзакции

---

# 12. Микросервисы

Особенности:

* независимые сервисы
* отдельный deploy
* собственная БД

## Коммуникация

```
HTTP
gRPC
Message Queue
```

---

# ⚠️ Частые вопросы на тестах

1. Принципы **SOLID**
2. Паттерны **Observer / Factory / Singleton**
3. Слои **Clean Architecture**
4. **Big O**
5. `async/await` в циклах
6. Принципы **REST**
7. **TLS handshake**
8. **XSS vs CSRF**
9. **LocalStorage vs IndexedDB**
10. **Unit vs Integration тесты**

```

---

Если хочешь, я могу ещё сделать **улучшенную версию README**, которую реально любят на GitHub:

- с **диаграммой Clean Architecture**
- **таблицей паттернов**
- **таблицей атак безопасности**
- **алгоритмической шпаргалкой**

Она будет выглядеть **как профессиональный репозиторий для подготовки к собеседованиям**.
```
