

---

## 1️⃣ Что такое MobX?

**MobX** — это библиотека для управления состоянием, которая делает реактивное программирование простым и предсказуемым.
В отличие от Redux, MobX:

* Не требует редьюсеров и экшенов в явном виде
* Работает с **обычными классами и свойствами**
* Автоматически отслеживает зависимости и обновляет компоненты

---

## 2️⃣ Установка

```bash
npm install mobx mobx-react-lite
```

> ⚠️ Используем `mobx-react-lite`, т.к. он предназначен для функциональных компонентов.

---

## 3️⃣ Создаём Store

Создадим папку `stores/` и файл `CounterStore.ts`:

```ts
// stores/CounterStore.ts
import { makeAutoObservable } from "mobx";

export class CounterStore {
  count = 0;

  constructor() {
    // делает все поля и методы реактивными
    makeAutoObservable(this);
  }

  increment() {
    this.count++;
  }

  decrement() {
    this.count--;
  }

  get double() {
    return this.count * 2;
  }
}

// создаём экземпляр
export const counterStore = new CounterStore();
```

---

## 4️⃣ Подключаем Store к React через Context

```tsx
// stores/StoreContext.tsx
import React from "react";
import { counterStore } from "./CounterStore";

export const StoreContext = React.createContext({
  counterStore,
});

export const useStores = () => React.useContext(StoreContext);
```

Теперь `useStores()` даст доступ к хранилищу из любого компонента.

---

## 5️⃣ Используем MobX в компоненте

```tsx
// components/Counter.tsx
import React from "react";
import { observer } from "mobx-react-lite";
import { useStores } from "../stores/StoreContext";

export const Counter = observer(() => {
  const { counterStore } = useStores();

  return (
    <div style={{ textAlign: "center", padding: 20 }}>
      <h2>Счётчик: {counterStore.count}</h2>
      <p>Удвоенное значение: {counterStore.double}</p>
      <button onClick={() => counterStore.increment()}>+</button>
      <button onClick={() => counterStore.decrement()}>−</button>
    </div>
  );
});
```

> 🧠 `observer()` — делает компонент реактивным: он будет перерисовываться только при изменении наблюдаемых свойств (`count`, `double` и т.д.)

---

## 6️⃣ Используем компонент в приложении

```tsx
// App.tsx
import React from "react";
import { Counter } from "./components/Counter";
import { StoreContext } from "./stores/StoreContext";
import { counterStore } from "./stores/CounterStore";

function App() {
  return (
    <StoreContext.Provider value={{ counterStore }}>
      <h1 style={{ textAlign: "center" }}>MobX + React пример</h1>
      <Counter />
    </StoreContext.Provider>
  );
}

export default App;
```

---

## 7️⃣ Реактивные вычисления (`computed`)

В `CounterStore` у нас есть геттер `double`.
MobX автоматически вычисляет его заново **только когда меняется `count`**.

```ts
get double() {
  console.log("Пересчёт...");
  return this.count * 2;
}
```

Открой консоль — пересчёт произойдёт только при изменении `count`.

---

## 8️⃣ Асинхронные действия

MobX поддерживает `async/await` прямо внутри `actions`:

```ts
async incrementLater() {
  await new Promise((r) => setTimeout(r, 1000));
  this.count++;
}
```

---

## 9️⃣ Наблюдение вне компонентов

MobX можно использовать и **вне React**, например:

```ts
import { autorun } from "mobx";
import { counterStore } from "./stores/CounterStore";

autorun(() => {
  console.log("Текущее значение:", counterStore.count);
});
```

---

## 🔍 Ключевые концепции MobX

| Понятие      | Что делает                                                 |
| ------------ | ---------------------------------------------------------- |
| `observable` | Делает объект реактивным                                   |
| `computed`   | Вычисляемые свойства, зависят от `observable`              |
| `action`     | Методы, изменяющие состояние                               |
| `observer`   | Делает React-компонент реактивным                          |
| `autorun`    | Автоматически запускает функцию при изменении зависимостей |

---

## 🧱 Структура проекта

```
src/
 ├─ components/
 │   └─ Counter.tsx
 ├─ stores/
 │   ├─ CounterStore.ts
 │   └─ StoreContext.tsx
 ├─ App.tsx
 └─ main.tsx
```

---

## ⚡ Пример 1. Список дел (ToDo List)

**Фишки:**

* Несколько `store`
* Работа со списками
* `computed` для фильтрации

```tsx
// stores/TodoStore.ts
import { makeAutoObservable } from "mobx";

export class TodoStore {
  todos = [
    { id: 1, text: "Изучить MobX", completed: false },
    { id: 2, text: "Создать ToDo App", completed: true },
  ];
  filter = "all";

  constructor() {
    makeAutoObservable(this);
  }

  addTodo(text: string) {
    this.todos.push({
      id: Date.now(),
      text,
      completed: false,
    });
  }

  toggleTodo(id: number) {
    const todo = this.todos.find((t) => t.id === id);
    if (todo) todo.completed = !todo.completed;
  }

  setFilter(filter: string) {
    this.filter = filter;
  }

  get filteredTodos() {
    switch (this.filter) {
      case "active":
        return this.todos.filter((t) => !t.completed);
      case "completed":
        return this.todos.filter((t) => t.completed);
      default:
        return this.todos;
    }
  }

  get total() {
    return this.todos.length;
  }

  get completedCount() {
    return this.todos.filter((t) => t.completed).length;
  }
}

export const todoStore = new TodoStore();
```

```tsx
// stores/StoreContext.tsx
import React from "react";
import { todoStore } from "./TodoStore";

export const StoreContext = React.createContext({ todoStore });
export const useStores = () => React.useContext(StoreContext);
```

```tsx
// components/TodoApp.tsx
import React, { useState } from "react";
import { observer } from "mobx-react-lite";
import { useStores } from "../stores/StoreContext";

export const TodoApp = observer(() => {
  const { todoStore } = useStores();
  const [input, setInput] = useState("");

  const addTodo = () => {
    if (input.trim()) {
      todoStore.addTodo(input.trim());
      setInput("");
    }
  };

  return (
    <div style={{ maxWidth: 400, margin: "0 auto", padding: 20 }}>
      <h2>📝 ToDo List</h2>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Новая задача..."
      />
      <button onClick={addTodo}>Добавить</button>

      <div style={{ marginTop: 10 }}>
        <button onClick={() => todoStore.setFilter("all")}>Все</button>
        <button onClick={() => todoStore.setFilter("active")}>Активные</button>
        <button onClick={() => todoStore.setFilter("completed")}>Выполненные</button>
      </div>

      <ul>
        {todoStore.filteredTodos.map((todo) => (
          <li
            key={todo.id}
            onClick={() => todoStore.toggleTodo(todo.id)}
            style={{
              textDecoration: todo.completed ? "line-through" : "none",
              cursor: "pointer",
            }}
          >
            {todo.text}
          </li>
        ))}
      </ul>

      <p>
        Выполнено: {todoStore.completedCount} / {todoStore.total}
      </p>
    </div>
  );
});
```

```tsx
// App.tsx
import React from "react";
import { TodoApp } from "./components/TodoApp";
import { StoreContext } from "./stores/StoreContext";
import { todoStore } from "./stores/TodoStore";

export default function App() {
  return (
    <StoreContext.Provider value={{ todoStore }}>
      <TodoApp />
    </StoreContext.Provider>
  );
}
```

---

## 🌐 Пример 2. Асинхронная загрузка данных (MobX + Fetch)

**Фишки:**

* `async actions`
* Состояние загрузки
* Отображение данных из API

```tsx
// stores/UserStore.ts
import { makeAutoObservable } from "mobx";

export class UserStore {
  users: any[] = [];
  loading = false;
  error: string | null = null;

  constructor() {
    makeAutoObservable(this);
  }

  async fetchUsers() {
    this.loading = true;
    this.error = null;
    try {
      const res = await fetch("https://jsonplaceholder.typicode.com/users");
      const data = await res.json();
      this.users = data;
    } catch (e) {
      this.error = "Ошибка загрузки пользователей";
    } finally {
      this.loading = false;
    }
  }
}

export const userStore = new UserStore();
```

```tsx
// stores/StoreContext.tsx
import React from "react";
import { userStore } from "./UserStore";
export const StoreContext = React.createContext({ userStore });
export const useStores = () => React.useContext(StoreContext);
```

```tsx
// components/UserList.tsx
import React, { useEffect } from "react";
import { observer } from "mobx-react-lite";
import { useStores } from "../stores/StoreContext";

export const UserList = observer(() => {
  const { userStore } = useStores();

  useEffect(() => {
    userStore.fetchUsers();
  }, [userStore]);

  if (userStore.loading) return <p>Загрузка...</p>;
  if (userStore.error) return <p>{userStore.error}</p>;

  return (
    <div style={{ padding: 20 }}>
      <h2>👥 Пользователи</h2>
      <ul>
        {userStore.users.map((u) => (
          <li key={u.id}>
            {u.name} — <i>{u.email}</i>
          </li>
        ))}
      </ul>
    </div>
  );
});
```

```tsx
// App.tsx
import React from "react";
import { UserList } from "./components/UserList";
import { StoreContext } from "./stores/StoreContext";
import { userStore } from "./stores/UserStore";

export default function App() {
  return (
    <StoreContext.Provider value={{ userStore }}>
      <UserList />
    </StoreContext.Provider>
  );
}
```

---

## ⚙️ Пример 3. Реактивный счётчик с MobX и `autorun`

**Фишки:**

* Использование `autorun` для реакций вне React
* Показ работы `computed`

```tsx
// stores/CounterStore.ts
import { makeAutoObservable, autorun } from "mobx";

export class CounterStore {
  count = 0;

  constructor() {
    makeAutoObservable(this);

    autorun(() => {
      console.log("📈 Значение изменилось:", this.count);
    });
  }

  increment() {
    this.count++;
  }

  get square() {
    return this.count ** 2;
  }
}

export const counterStore = new CounterStore();
```

```tsx
// components/CounterView.tsx
import React from "react";
import { observer } from "mobx-react-lite";
import { counterStore } from "../stores/CounterStore";

export const CounterView = observer(() => {
  return (
    <div style={{ textAlign: "center", padding: 20 }}>
      <h2>🔢 Счётчик: {counterStore.count}</h2>
      <p>Квадрат числа: {counterStore.square}</p>
      <button onClick={() => counterStore.increment()}>+1</button>
    </div>
  );
});
```

```tsx
// App.tsx
import React from "react";
import { CounterView } from "./components/CounterView";

export default function App() {
  return <CounterView />;
}
```


## 📘 Полезные ссылки

* [MobX Docs](https://mobx.js.org/README.html)
* [mobx-react-lite](https://mobx.js.org/react-integration.html)
* [Учебник на TypeScript](https://mobx.js.org/observable-state.html#typescript-support)

