
# JavaScript / ECMAScript — стандарты ES5 → ES2026

> История развития стандарта ECMAScript и ключевые возможности JavaScript от ES5 до современных версий.

## Содержание

* [Что такое ECMAScript](#что-такое-ecmascript)
* [ES5 — 2009](#es5--2009)
* [ES2015 / ES6 — 2015](#es2015--es6--2015)
* [ES2016 / ES7 — 2016](#es2016--es7--2016)
* [ES2017 / ES8 — 2017](#es2017--es8--2017)
* [ES2018 / ES9 — 2018](#es2018--es9--2018)
* [ES2019 / ES10 — 2019](#es2019--es10--2019)
* [ES2020 / ES11 — 2020](#es2020--es11--2020)
* [ES2021 / ES12 — 2021](#es2021--es12--2021)
* [ES2022 / ES13 — 2022](#es2022--es13--2022)
* [ES2023 / ES14 — 2023](#es2023--es14--2023)
* [ES2024 / ES15 — 2024](#es2024--es15--2024)
* [ES2025 / ES16 — 2025](#es2025--es16--2025)
* [ES2026 / ES17 — 2026](#es2026--es17--2026)
* [Хронология](#хронология)
* [Что учить современному разработчику](#что-учить-современному-разработчику)
* [Полезные ссылки](#полезные-ссылки)

---

# Что такое ECMAScript

**ECMAScript (ES)** — стандартизированная спецификация языка программирования, на которой основан JavaScript.

JavaScript — это реализация ECMAScript плюс дополнительные возможности среды выполнения.

Например:

```js
console.log("Hello World");
```

Синтаксис `console.log()` связан не только со стандартом ECMAScript, но и с API конкретной среды выполнения.

В браузере доступны Web API:

```js
document
window
fetch
localStorage
setTimeout
```

В Node.js доступны API среды Node.js:

```js
process
fs
path
Buffer
```

При этом базовые конструкции языка определяются ECMAScript:

```js
const user = {
  name: "Alex",
  age: 25
};
```

---

# Почему версии называются ES5, ES6 и ES2015

До ES2015 версии ECMAScript обычно обозначались порядковыми номерами:

```text
ES3
ES5
ES6
ES7
ES8
```

Начиная с ES2015 стандарт получил ежегодную нумерацию по году выпуска:

```text
ES2015
ES2016
ES2017
ES2018
...
```

Поэтому:

```text
ES6  = ES2015
ES7  = ES2016
ES8  = ES2017
ES9  = ES2018
ES10 = ES2019
ES11 = ES2020
...
```

Сегодня корректнее говорить:

> ECMAScript 2020, ECMAScript 2021 и т. д.

---

# ES5 — 2009

ES5 — один из важнейших этапов развития JavaScript.

До ES5 язык уже существовал много лет, однако именно ES5 заложил значительную часть основы современного JavaScript.

## Основные возможности

* `"use strict"`
* JSON API
* `Object.keys()`
* getters/setters
* новые методы массивов
* `Function.prototype.bind()`
* дополнительные методы `Object`
* формализация поведения языка

---

## Strict Mode

Позволяет включить более строгий режим выполнения:

```js
"use strict";

x = 10;
```

В strict mode такой код вызовет ошибку, поскольку переменная `x` не была объявлена.

Правильно:

```js
"use strict";

const x = 10;
```

> `let` и `const` появились только в ES2015, поэтому в чистом ES5 обычно использовался `var`.

---

## JSON

ES5 стандартизировал встроенный JSON API.

### JSON.stringify()

Преобразование JavaScript-объекта в JSON:

```js
const user = {
  name: "Alex",
  age: 25
};

const json = JSON.stringify(user);

console.log(json);
```

Результат:

```json
{"name":"Alex","age":25}
```

### JSON.parse()

Преобразование JSON в JavaScript-объект:

```js
const json = '{"name":"Alex","age":25}';

const user = JSON.parse(json);

console.log(user.name);
```

---

## Методы массивов

ES5 добавил множество методов, которые стали фундаментальными для JavaScript.

### forEach()

```js
const numbers = [1, 2, 3];

numbers.forEach(function (number) {
  console.log(number);
});
```

### map()

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(function (number) {
  return number * 2;
});

console.log(doubled);
// [2, 4, 6]
```

### filter()

```js
const numbers = [1, 2, 3, 4, 5];

const even = numbers.filter(function (number) {
  return number % 2 === 0;
});

console.log(even);
// [2, 4]
```

### reduce()

```js
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce(function (total, number) {
  return total + number;
}, 0);

console.log(sum);
// 10
```

### some()

```js
const numbers = [1, 2, 3];

const hasEven = numbers.some(function (number) {
  return number % 2 === 0;
});
```

### every()

```js
const numbers = [2, 4, 6];

const allEven = numbers.every(function (number) {
  return number % 2 === 0;
});
```

### indexOf()

```js
const fruits = ["apple", "banana", "orange"];

console.log(fruits.indexOf("banana"));
// 1
```

---

## Object.keys()

```js
const user = {
  name: "Alex",
  age: 25
};

console.log(Object.keys(user));

// ["name", "age"]
```

---

## Getters и setters

```js
const user = {
  firstName: "Alex",
  lastName: "Smith",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
};

console.log(user.fullName);
```

Setter:

```js
const user = {
  firstName: "Alex",
  lastName: "Smith",

  set fullName(value) {
    const [firstName, lastName] = value.split(" ");

    this.firstName = firstName;
    this.lastName = lastName;
  }
};

user.fullName = "John Doe";
```

---

## bind()

Позволяет явно привязать значение `this`:

```js
const user = {
  name: "Alex",

  sayHello() {
    console.log(`Hello, ${this.name}`);
  }
};

const hello = user.sayHello.bind(user);

hello();
```

---

# ES2015 / ES6 — 2015

ES2015 — крупнейшее обновление ECMAScript.

Именно с него началась эпоха **современного JavaScript**.

## Основные возможности

* `let`
* `const`
* стрелочные функции
* классы
* модули
* `Promise`
* `Map`
* `Set`
* `WeakMap`
* `WeakSet`
* `Symbol`
* `for...of`
* деструктуризация
* rest parameters
* spread syntax
* template literals
* default parameters
* `import/export`
* `Proxy`
* `Reflect`
* итераторы
* генераторы

---

# let и const

До ES2015:

```js
var name = "Alex";
```

Современный JavaScript:

```js
let age = 25;
const name = "Alex";
```

`let` позволяет переопределять значение:

```js
let count = 0;

count = 1;
count = 2;
```

`const` запрещает переопределение переменной:

```js
const name = "Alex";

name = "John";
// TypeError
```

Однако объект, объявленный через `const`, можно изменять:

```js
const user = {
  name: "Alex"
};

user.name = "John";
```

---

# Стрелочные функции

ES5:

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(function (number) {
  return number * 2;
});
```

ES2015:

```js
const doubled = numbers.map(number => number * 2);
```

Несколько аргументов:

```js
const sum = (a, b) => a + b;
```

Тело функции:

```js
const sum = (a, b) => {
  return a + b;
};
```

### Важная особенность

Стрелочные функции не создают собственный `this`.

```js
const user = {
  name: "Alex",

  sayHello() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};
```

---

# Template Literals

Вместо:

```js
const name = "Alex";

const message = "Hello, " + name + "!";
```

Можно:

```js
const name = "Alex";

const message = `Hello, ${name}!`;
```

Поддерживается многострочный текст:

```js
const text = `
  First line
  Second line
  Third line
`;
```

---

# Деструктуризация

## Object destructuring

```js
const user = {
  name: "Alex",
  age: 25
};

const { name, age } = user;

console.log(name);
console.log(age);
```

Переименование:

```js
const {
  name: userName
} = user;
```

---

## Array destructuring

```js
const numbers = [10, 20, 30];

const [a, b, c] = numbers;

console.log(a);
// 10
```

Пропуск элемента:

```js
const [first, , third] = numbers;
```

---

# Default Parameters

```js
function greet(name = "Guest") {
  console.log(`Hello, ${name}`);
}

greet();
```

Результат:

```text
Hello, Guest
```

---

# Rest Parameters

```js
function sum(...numbers) {
  return numbers.reduce((total, number) => {
    return total + number;
  }, 0);
}

sum(1, 2, 3, 4);
```

---

# Spread Syntax

```js
const first = [1, 2, 3];
const second = [4, 5, 6];

const result = [...first, ...second];

console.log(result);
// [1, 2, 3, 4, 5, 6]
```

Для объектов:

```js
const user = {
  name: "Alex"
};

const updatedUser = {
  ...user,
  age: 25
};
```

---

# Classes

До ES2015 часто использовались constructor functions:

```js
function User(name) {
  this.name = name;
}

User.prototype.sayHello = function () {
  console.log(`Hello, ${this.name}`);
};
```

ES2015:

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    console.log(`Hello, ${this.name}`);
  }
}

const user = new User("Alex");

user.sayHello();
```

Наследование:

```js
class Admin extends User {
  deleteUser() {
    console.log("User deleted");
  }
}
```

---

# Modules

ES2015 добавил официальную систему модулей.

### export

```js
export function sum(a, b) {
  return a + b;
}
```

### import

```js
import { sum } from "./math.js";

console.log(sum(2, 3));
```

Default export:

```js
export default class User {
  // ...
}
```

Импорт:

```js
import User from "./User.js";
```

---

# Promise

До Promise асинхронный код часто строился на callback:

```js
getUser(function (user) {
  getPosts(user, function (posts) {
    // ...
  });
});
```

ES2015 добавил `Promise`.

```js
const promise = new Promise((resolve, reject) => {
  resolve("Success");
});

promise.then(result => {
  console.log(result);
});
```

Обработка ошибки:

```js
promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error);
  });
```

---

# Map

`Map` — коллекция ключ/значение.

```js
const users = new Map();

users.set(1, "Alex");
users.set(2, "John");

console.log(users.get(1));
// Alex
```

Проверка:

```js
users.has(1);
```

Размер:

```js
users.size;
```

---

# Set

`Set` хранит только уникальные значения.

```js
const numbers = new Set([
  1,
  2,
  2,
  3
]);

console.log(numbers);
// Set { 1, 2, 3 }
```

Получение уникального массива:

```js
const numbers = [1, 2, 2, 3, 3];

const unique = [...new Set(numbers)];

console.log(unique);
// [1, 2, 3]
```

---

# Symbol

`Symbol` создаёт уникальное значение:

```js
const id = Symbol("id");

const user = {
  name: "Alex",
  [id]: 123
};
```

Каждый Symbol уникален:

```js
Symbol("id") === Symbol("id");
// false
```

---

# for...of

```js
const numbers = [10, 20, 30];

for (const number of numbers) {
  console.log(number);
}
```

Работает с iterable-объектами:

```js
Array
String
Map
Set
```

---

# Generators

Генераторы позволяют приостанавливать выполнение функции.

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = numbers();

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
```

---

# Proxy

Позволяет перехватывать операции над объектом.

```js
const user = {
  name: "Alex"
};

const proxy = new Proxy(user, {
  get(target, property) {
    console.log(`Reading ${property}`);

    return target[property];
  }
});

console.log(proxy.name);
```

---

# ES2016 / ES7 — 2016

Небольшое обновление по сравнению с ES2015.

## Основные возможности

* `Array.prototype.includes()`
* оператор `**`

---

## includes()

```js
const numbers = [1, 2, 3];

numbers.includes(2);
// true

numbers.includes(10);
// false
```

---

## Exponentiation operator

Вместо:

```js
Math.pow(2, 3);
```

Можно:

```js
2 ** 3;
// 8
```

---

# ES2017 / ES8 — 2017

Главная возможность ES2017 — `async/await`.

## async

```js
async function getUser() {
  return {
    name: "Alex"
  };
}
```

## await

```js
async function loadUser() {
  const user = await getUser();

  console.log(user);
}
```

---

# async/await

До:

```js
fetch("/api/users")
  .then(response => response.json())
  .then(users => {
    console.log(users);
  });
```

Современный вариант:

```js
async function loadUsers() {
  const response = await fetch("/api/users");

  const users = await response.json();

  console.log(users);
}
```

Обработка ошибок:

```js
async function loadUsers() {
  try {
    const response = await fetch("/api/users");

    const users = await response.json();

    return users;
  } catch (error) {
    console.error(error);
  }
}
```

---

## Object.values()

```js
const user = {
  name: "Alex",
  age: 25
};

Object.values(user);
// ["Alex", 25]
```

---

## Object.entries()

```js
Object.entries(user);
```

Результат:

```js
[
  ["name", "Alex"],
  ["age", 25]
]
```

---

## String padding

```js
"5".padStart(2, "0");
// "05"
```

```js
"5".padEnd(3, "0");
// "500"
```

---

# ES2018 / ES9 — 2018

## Object Rest

```js
const user = {
  name: "Alex",
  age: 25,
  role: "admin"
};

const { name, ...rest } = user;

console.log(rest);

// {
//   age: 25,
//   role: "admin"
// }
```

---

## Object Spread

```js
const user = {
  name: "Alex"
};

const updated = {
  ...user,
  age: 25
};
```

---

## Promise.finally()

```js
fetch("/api/users")
  .then(response => response.json())
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log("Request finished");
  });
```

`finally()` выполняется независимо от результата Promise.

---

## for await...of

Позволяет работать с асинхронными итераторами:

```js
for await (const item of asyncIterable) {
  console.log(item);
}
```

---

# ES2019 / ES10 — 2019

## Array.flat()

```js
const numbers = [
  [1, 2],
  [3, 4]
];

numbers.flat();

// [1, 2, 3, 4]
```

Для нескольких уровней:

```js
const numbers = [
  [1, [2, [3]]]
];

numbers.flat(Infinity);
```

---

## Array.flatMap()

```js
const numbers = [1, 2, 3];

const result = numbers.flatMap(number => [
  number,
  number * 2
]);

console.log(result);

// [1, 2, 2, 4, 3, 6]
```

---

## Object.fromEntries()

```js
const entries = [
  ["name", "Alex"],
  ["age", 25]
];

const user = Object.fromEntries(entries);
```

---

## trimStart() / trimEnd()

```js
const text = "   Hello   ";

text.trimStart();
// "Hello   "

text.trimEnd();
// "   Hello"
```

---

## Optional catch binding

До:

```js
try {
  riskyOperation();
} catch (error) {
  handleError();
}
```

Если ошибка не нужна:

```js
try {
  riskyOperation();
} catch {
  handleError();
}
```

---

# ES2020 / ES11 — 2020

ES2020 принёс несколько очень важных возможностей современного JavaScript.

## Основные возможности

* Optional chaining `?.`
* Nullish coalescing `??`
* `BigInt`
* `Promise.allSettled()`
* `globalThis`
* dynamic `import()`
* `String.matchAll()`

---

# Optional Chaining — ?.

Без optional chaining:

```js
const city = user &&
  user.address &&
  user.address.city;
```

Современный вариант:

```js
const city = user?.address?.city;
```

Метод:

```js
user?.sayHello?.();
```

Массив:

```js
users?.[0]?.name;
```

---

# Nullish Coalescing — ??

Позволяет использовать значение по умолчанию, если значение равно `null` или `undefined`.

```js
const name = user.name ?? "Guest";
```

В отличие от `||`:

```js
const count = 0 || 10;
// 10
```

Но:

```js
const count = 0 ?? 10;
// 0
```

Поэтому `??` особенно полезен, когда `0`, `false` и `""` являются валидными значениями.

---

# BigInt

Для целых чисел, превышающих безопасный диапазон `Number`.

```js
const bigNumber = 123456789012345678901234567890n;
```

Операции:

```js
const a = 1000000000000000000n;
const b = 2000000000000000000n;

console.log(a + b);
```

---

# Promise.allSettled()

В отличие от `Promise.all()`, `allSettled()` ждёт завершения всех Promise независимо от того, завершились они успешно или с ошибкой.

```js
const results = await Promise.allSettled([
  fetch("/api/users"),
  fetch("/api/posts"),
  fetch("/api/comments")
]);
```

---

# Dynamic import()

Модуль можно загрузить динамически:

```js
const module = await import("./module.js");
```

Это особенно полезно для lazy loading и code splitting.

---

# String.matchAll()

```js
const text = "cat dog cat";

const matches = text.matchAll(/cat/g);

for (const match of matches) {
  console.log(match);
}
```

---

# ES2021 / ES12 — 2021

## Основные возможности

* `String.replaceAll()`
* `Promise.any()`
* logical assignment
* `WeakRef`
* `FinalizationRegistry`
* numeric separators

---

## replaceAll()

```js
const text = "foo foo foo";

const result = text.replaceAll("foo", "bar");

console.log(result);
// bar bar bar
```

---

# Promise.any()

Возвращает первый успешно завершившийся Promise.

```js
const result = await Promise.any([
  fetch("/api/server-1"),
  fetch("/api/server-2"),
  fetch("/api/server-3")
]);
```

Если все Promise завершились с ошибкой, возникает `AggregateError`.

---

# Logical Assignment

## ||=

```js
let name;

name ||= "Guest";
```

Эквивалентно:

```js
name = name || "Guest";
```

---

## &&=

```js
let user = {
  isAdmin: true
};

user.isAdmin &&= false;
```

---

## ??=

```js
let name;

name ??= "Guest";
```

---

# Numeric separators

Большие числа стало легче читать:

```js
const million = 1_000_000;

const billion = 1_000_000_000;
```

---

# ES2022 / ES13 — 2022

## Основные возможности

* class fields
* private fields
* static class blocks
* top-level `await`
* `Object.hasOwn()`
* `Error.cause`

---

# Public Class Fields

```js
class User {
  name = "Alex";
  age = 25;
}
```

---

# Private Class Fields

Приватное поле начинается с `#`.

```js
class User {
  #password;

  constructor(password) {
    this.#password = password;
  }

  checkPassword(password) {
    return this.#password === password;
  }
}
```

Попытка:

```js
user.#password;
```

вне класса является ошибкой.

---

# Static Fields

```js
class User {
  static count = 0;
}
```

Использование:

```js
console.log(User.count);
```

---

# Static Initialization Block

```js
class Config {
  static value;

  static {
    Config.value = "production";
  }
}
```

---

# Top-level await

В ES-модуле можно использовать `await` без дополнительной `async` функции:

```js
const response = await fetch("/api/users");

const users = await response.json();
```

---

# Object.hasOwn()

Вместо:

```js
Object.prototype.hasOwnProperty.call(
  user,
  "name"
);
```

Можно:

```js
Object.hasOwn(user, "name");
```

---

# Error Cause

Можно указать исходную причину ошибки:

```js
try {
  connectToDatabase();
} catch (error) {
  throw new Error("Database connection failed", {
    cause: error
  });
}
```

---

# ES2023 / ES14 — 2023

ES2023 продолжил развитие встроенных API.

## Основные возможности

* `findLast()`
* `findLastIndex()`
* `toReversed()`
* `toSorted()`
* `toSpliced()`
* `with()`
* улучшения WeakMap и WeakSet
* Hashbang Grammar

---

# findLast()

```js
const numbers = [1, 2, 3, 4, 5, 4];

const result = numbers.findLast(number => {
  return number === 4;
});

console.log(result);
// 4
```

---

# findLastIndex()

```js
const numbers = [1, 2, 3, 4, 5, 4];

const index = numbers.findLastIndex(number => {
  return number === 4;
});

console.log(index);
// 5
```

---

# toReversed()

Обычный `reverse()` изменяет исходный массив:

```js
const numbers = [1, 2, 3];

numbers.reverse();
```

`toReversed()` создаёт новый:

```js
const numbers = [1, 2, 3];

const reversed = numbers.toReversed();

console.log(numbers);
// [1, 2, 3]

console.log(reversed);
// [3, 2, 1]
```

---

# toSorted()

Обычный `sort()` мутирует массив:

```js
const numbers = [3, 1, 2];

numbers.sort();
```

`toSorted()` возвращает новый массив:

```js
const numbers = [3, 1, 2];

const sorted = numbers.toSorted();

console.log(numbers);
// [3, 1, 2]

console.log(sorted);
// [1, 2, 3]
```

---

# toSpliced()

```js
const numbers = [1, 2, 3, 4];

const result = numbers.toSpliced(1, 2);

console.log(numbers);
// [1, 2, 3, 4]

console.log(result);
// [1, 4]
```

---

# with()

Позволяет создать копию массива с изменённым элементом.

```js
const numbers = [1, 2, 3];

const result = numbers.with(1, 100);

console.log(numbers);
// [1, 2, 3]

console.log(result);
// [1, 100, 3]
```

---

# ES2024 / ES15 — 2024

ES2024 добавил ряд возможностей, особенно полезных при работе с коллекциями, Promise и регулярными выражениями.

## Основные возможности

* `Object.groupBy()`
* `Map.groupBy()`
* `Promise.withResolvers()`
* улучшения `ArrayBuffer`
* RegExp `/v`
* `String.isWellFormed()`
* `String.toWellFormed()`

---

# Object.groupBy()

Позволяет группировать элементы массива.

```js
const users = [
  {
    name: "Alex",
    role: "admin"
  },
  {
    name: "John",
    role: "user"
  },
  {
    name: "Kate",
    role: "admin"
  }
];

const groups = Object.groupBy(users, user => user.role);
```

Получается объект примерно такого вида:

```js
{
  admin: [
    { name: "Alex", role: "admin" },
    { name: "Kate", role: "admin" }
  ],
  user: [
    { name: "John", role: "user" }
  ]
}
```

---

# Map.groupBy()

Позволяет группировать элементы с использованием `Map`.

```js
const groups = Map.groupBy(
  users,
  user => user.role
);
```

---

# Promise.withResolvers()

Удобный способ получить Promise и его функции управления:

```js
const {
  promise,
  resolve,
  reject
} = Promise.withResolvers();

resolve("Success");

const result = await promise;
```

---

# ES2025 / ES16 — 2025

ES2025 продолжил ежегодное развитие ECMAScript.

Среди заметных возможностей:

* Iterator Helpers
* новые операции `Set`
* `Promise.try()`
* улучшения регулярных выражений
* import attributes
* новые возможности JSON и модулей

---

# Iterator Helpers

Итераторы получили методы, позволяющие выполнять операции непосредственно над iterator.

Например:

```js
const iterator = [1, 2, 3, 4, 5].values();

const result = iterator
  .filter(number => number % 2 === 0)
  .map(number => number * 10)
  .toArray();

console.log(result);

// [20, 40]
```

Это позволяет строить цепочки операций над итераторами без необходимости сразу создавать промежуточные массивы.

---

# Set Operations

Современный `Set` получил операции для работы с множествами.

Например:

```js
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);
```

Пересечение:

```js
a.intersection(b);
```

Результат:

```text
{2, 3}
```

Разность:

```js
a.difference(b);
```

Результат:

```text
{1}
```

Объединение:

```js
a.union(b);
```

Результат:

```text
{1, 2, 3, 4}
```

---

# Promise.try()

Позволяет унифицировать обработку синхронных и асинхронных ошибок.

```js
const result = Promise.try(() => {
  return JSON.parse(data);
});
```

---

# ES2026 / ES17 — 2026

ECMAScript развивается ежегодно.

Вместо одного большого релиза, как ES2015, современные версии содержат относительно небольшие наборы возможностей, которые прошли стандартизацию.

ES2026 продолжает эту модель.

При работе с конкретной версией среды выполнения всегда следует проверять поддержку интересующей возможности в:

* браузерах;
* Node.js;
* JavaScript runtime;
* TypeScript;
* Babel;
* других инструментах сборки.

---

# Хронология

| Версия            |  Год | Ключевые возможности                                                 |
| ----------------- | ---: | -------------------------------------------------------------------- |
| **ES5**           | 2009 | Strict Mode, JSON, Array methods, Object API, bind                   |
| **ES6 / ES2015**  | 2015 | `let`, `const`, classes, modules, Promise, Map, Set, arrow functions |
| **ES7 / ES2016**  | 2016 | `includes()`, `**`                                                   |
| **ES8 / ES2017**  | 2017 | `async/await`, Object.values, Object.entries                         |
| **ES9 / ES2018**  | 2018 | Object spread/rest, Promise.finally, async iteration                 |
| **ES10 / ES2019** | 2019 | `flat`, `flatMap`, `fromEntries`, trimStart/trimEnd                  |
| **ES11 / ES2020** | 2020 | `?.`, `??`, BigInt, Promise.allSettled, dynamic import               |
| **ES12 / ES2021** | 2021 | `replaceAll`, Promise.any, logical assignment, numeric separators    |
| **ES13 / ES2022** | 2022 | Class fields, private fields, top-level await, Object.hasOwn         |
| **ES14 / ES2023** | 2023 | findLast, toSorted, toReversed, toSpliced, with                      |
| **ES15 / ES2024** | 2024 | groupBy, Promise.withResolvers, RegExp `/v`                          |
| **ES16 / ES2025** | 2025 | Iterator Helpers, Set methods, Promise.try                           |
| **ES17 / ES2026** | 2026 | дальнейшее ежегодное развитие языка                                  |

---

# Что изменилось между ES5 и современным JavaScript

Разница очень большая.

## ES5

Типичный код:

```js
var numbers = [1, 2, 3];

var doubled = numbers.map(function (number) {
  return number * 2;
});
```

## ES2015+

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(
  number => number * 2
);
```

---

## ES5

```js
var user = {
  name: "Alex"
};

if (
  user &&
  user.address &&
  user.address.city
) {
  console.log(user.address.city);
}
```

## Современный JavaScript

```js
const city = user?.address?.city;
```

---

## ES5

```js
function loadUser(callback) {
  getUser(function (user) {
    callback(user);
  });
}
```

## Современный JavaScript

```js
async function loadUser() {
  return await getUser();
}
```

---

# Что изучать в первую очередь

Если цель — современная разработка, не нужно запоминать все версии ECMAScript как отдельные языки.

Рекомендуемый порядок:

## 1. Основы ES5

Изучить:

```text
var
function
scope
this
prototype
Object
Array
String
Number
JSON
try/catch
```

Это важно для понимания старого кода и самого устройства JavaScript.

---

## 2. ES2015

Это наиболее важная версия для понимания современного JS.

Изучить:

```text
let
const
arrow functions
template literals
destructuring
spread
rest
default parameters
classes
modules
Promise
Map
Set
Symbol
for...of
```

---

## 3. Асинхронность

Обязательно:

```text
Promise
async
await
Promise.all
Promise.allSettled
Promise.race
Promise.any
```

Например:

```js
async function loadData() {
  const [users, posts] = await Promise.all([
    getUsers(),
    getPosts()
  ]);

  return {
    users,
    posts
  };
}
```

---

## 4. Современный синтаксис

Обязательно знать:

```js
const user = data?.user;

const name = user?.name ?? "Guest";

count ??= 0;

isActive &&= true;
```

---

## 5. Современные классы

```js
class User {
  name;
  #password;

  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }

  checkPassword(password) {
    return this.#password === password;
  }
}
```

---

## 6. Иммутабельные методы массивов

Особенно полезны:

```js
toSorted()
toReversed()
toSpliced()
with()
```

Например:

```js
const users = [
  { name: "John", age: 30 },
  { name: "Alex", age: 20 }
];

const sortedUsers = users.toSorted(
  (a, b) => a.age - b.age
);
```

Исходный массив при этом не изменяется.

---

# ECMAScript vs JavaScript

Важно не смешивать понятия.

**ECMAScript** — спецификация языка.

**JavaScript** — конкретная реализация языка и среда выполнения.

Например:

```js
const result = await fetch("/api/users");
```

`const` и `await` относятся к ECMAScript.

А `fetch()` предоставляется средой выполнения, например браузером или современным runtime.

---

# ECMAScript vs TypeScript

TypeScript — не отдельная версия ECMAScript.

Например:

```ts
interface User {
  name: string;
  age: number;
}
```

`interface` — возможность TypeScript, а не ECMAScript.

После компиляции TypeScript преобразуется в JavaScript.

---

# ECMAScript и транспиляция

Не каждая среда сразу поддерживает все новые возможности.

Например, разработчик может писать:

```js
const userName = user?.profile?.name ?? "Guest";
```

А Babel или другой инструмент может преобразовать код для старых браузеров.

Это называется **transpilation**.

Типичная цепочка:

```text
Modern JavaScript
       ↓
Babel / TypeScript
       ↓
Compatible JavaScript
       ↓
Browser / Runtime
```

---

# Как определять поддержку возможностей

Для конкретной возможности важно смотреть не только на номер ES.

Например:

```js
Array.prototype.toSorted
```

может поддерживаться одной версией браузера и отсутствовать в другой.

Поэтому на практике используются:

* MDN;
* таблицы совместимости браузеров;
* документация Node.js;
* `browserslist`;
* Babel;
* TypeScript;
* тесты runtime.

---

# Краткая шпаргалка

## ES5

```js
var
function
JSON
Object.keys()
Array.map()
Array.filter()
Array.reduce()
bind()
"use strict"
```

## ES2015

```js
let
const
class
import/export
Promise
Map
Set
Symbol
=> 
...
destructuring
```

## ES2016

```js
includes()
**
```

## ES2017

```js
async
await
Object.entries()
Object.values()
```

## ES2018

```js
{ ...object }
{ ...rest }
Promise.finally()
for await...of
```

## ES2019

```js
flat()
flatMap()
Object.fromEntries()
trimStart()
trimEnd()
```

## ES2020

```js
?.
??
BigInt
Promise.allSettled()
import()
globalThis
```

## ES2021

```js
replaceAll()
Promise.any()
||=
&&=
??=
1_000_000
```

## ES2022

```js
class fields
#private
static {}
await
Object.hasOwn()
Error.cause
```

## ES2023

```js
findLast()
findLastIndex()
toSorted()
toReversed()
toSpliced()
with()
```

## ES2024

```js
Object.groupBy()
Map.groupBy()
Promise.withResolvers()
RegExp /v
```

## ES2025

```js
Iterator Helpers
Set methods
Promise.try()
```

## ES2026+

```text
Ежегодное развитие ECMAScript
```

---

# Итог

Развитие JavaScript можно условно разделить на несколько больших этапов:

```text
ES5
 │
 ├── основа современного языка
 │
 ▼
ES2015
 │
 ├── let / const
 ├── classes
 ├── modules
 ├── Promise
 ├── arrow functions
 ├── destructuring
 ├── Map / Set
 │
 ▼
ES2016–ES2019
 │
 ├── async/await
 ├── object spread
 ├── flat()
 ├── flatMap()
 └── другие улучшения
 │
 ▼
ES2020–ES2022
 │
 ├── optional chaining
 ├── nullish coalescing
 ├── BigInt
 ├── private fields
 ├── top-level await
 └── современные Promise API
 │
 ▼
ES2023–ES2026
 │
 ├── immutable Array methods
 ├── groupBy
 ├── Iterator Helpers
 ├── Set methods
 └── дальнейшее развитие языка
```

Главная версия, которую стоит особенно хорошо знать, — **ES2015 (ES6)**. Именно она изменила JavaScript настолько сильно, что большинство современного JS-кода строится на её концепциях.

При этом современный JavaScript — это не «ES2026 вместо ES5». Новые стандарты **добавляют возможности поверх уже существующего языка**, поэтому разработчику приходится понимать как современный синтаксис, так и фундаментальные механизмы JavaScript: `scope`, `this`, prototype chain, closures, objects, functions, event loop и асинхронность.

---

## Полезные ссылки

* [ECMAScript Language Specification](https://tc39.es/ecma262/)
* [TC39 — ECMAScript Proposals](https://github.com/tc39/proposals)
* [MDN — JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
* [MDN — JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
* [Node.js Documentation](https://nodejs.org/docs/)

