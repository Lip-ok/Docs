
React + TanStack + MobX
---

## 🧩 Часть 1. React — фундамент

React отвечает за **UI и логику отображения**.
Его главная идея — **компонентный подход**: каждая часть интерфейса — отдельный компонент.

---

### ⚛️ Пример: список задач

```jsx
import { useState } from "react";

function TodoApp() {
  const [todos, setTodos] = useState(["Выучить React", "Выучить TanStack Query"]);
  const [text, setText] = useState("");

  const addTodo = () => {
    if (!text.trim()) return;
    setTodos([...todos, text]);
    setText("");
  };

  return (
    <div className="p-4">
      <h1>Мои задачи</h1>
      <ul>
        {todos.map((todo, i) => (
          <li key={i}>{todo}</li>
        ))}
      </ul>
      <input value={text} onChange={e => setText(e.target.value)} placeholder="Новая задача" />
      <button onClick={addTodo}>Добавить</button>
    </div>
  );
}
```

👉 **Что здесь важно:**

* `useState` хранит состояние (локальное, внутри компонента)
* Компонент **перерисовывается** при изменении состояния
* Всё работает **только в пределах UI**, без внешних данных

---

## ⚙️ Часть 2. TanStack Query — работа с сервером

TanStack Query решает **главную проблему фронтенда** — синхронизацию UI с сервером:

* Запрашивает данные (`useQuery`)
* Отправляет изменения (`useMutation`)
* Управляет кэшированием и актуальностью данных

---

### 💡 Пример: получение списка пользователей

```jsx
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

function UsersList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const res = await axios.get("https://jsonplaceholder.typicode.com/users");
      return res.data;
    },
  });

  if (isLoading) return <p>Загрузка...</p>;
  if (error) return <p>Ошибка: {error.message}</p>;

  return (
    <div>
      <h2>Пользователи:</h2>
      <ul>
        {data.map(user => (
          <li key={user.id}>
            {user.name} — {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

👉 **Что важно:**

* `queryKey` = уникальный ключ для кэша
* `queryFn` = функция получения данных
* React Query автоматически:

  * Кэширует результат
  * Делает refetch при фокусе окна
  * Позволяет обновлять данные без ручных useEffect

---

### 🔄 Пример: добавление пользователя через `useMutation`

```jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";
import axios from "axios";
import { useState } from "react";

function AddUser() {
  const [name, setName] = useState("");
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: async (newUser) => {
      return axios.post("https://jsonplaceholder.typicode.com/users", newUser);
    },
    onSuccess: () => {
      queryClient.invalidateQueries(["users"]); // обновляем список
    },
  });

  const handleAdd = () => {
    mutation.mutate({ name });
    setName("");
  };

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} placeholder="Имя" />
      <button onClick={handleAdd}>Добавить</button>
    </div>
  );
}
```

👉 **Фишки:**

* `useMutation` используется для действий (POST/PUT/DELETE)
* После успеха можно обновить (`invalidateQueries`) или напрямую обновить кэш
* Можно показывать загрузку и ошибки из `mutation.isLoading` и `mutation.error`

---

## 🧠 Часть 3. MobX — управление состоянием приложения

React Query отвечает за **серверное состояние**,
MobX — за **клиентское (UI)**.

Используем MobX для управления:

* фильтрами, модальными окнами, текущим пользователем, авторизацией, настройками и т.п.

---

### 🏗 Пример: MobX store для фильтра и темной темы

```jsx
import { makeAutoObservable } from "mobx";

class UIStore {
  theme = "light";
  filter = "";

  constructor() {
    makeAutoObservable(this);
  }

  setTheme(theme) {
    this.theme = theme;
  }

  setFilter(value) {
    this.filter = value;
  }
}

export const uiStore = new UIStore();
```

👉 Теперь можно использовать `observer` из `mobx-react-lite` для связывания с React:

```jsx
import { observer } from "mobx-react-lite";
import { uiStore } from "./uiStore";

const Header = observer(() => {
  return (
    <header className="flex justify-between p-4 bg-gray-100">
      <input
        type="text"
        value={uiStore.filter}
        onChange={(e) => uiStore.setFilter(e.target.value)}
        placeholder="Поиск..."
      />
      <button onClick={() => uiStore.setTheme(uiStore.theme === "light" ? "dark" : "light")}>
        {uiStore.theme === "light" ? "🌞" : "🌚"}
      </button>
    </header>
  );
});
```

👉 **MobX автоматически следит за состоянием** — никаких useState/useEffect не нужно.

---

## ⚡ Пример объединения: React + TanStack Query + MobX

Создадим **приложение пользователей**, где:

* MobX хранит фильтр поиска
* TanStack Query загружает пользователей
* React отображает фильтрованный список

---

```jsx
import { makeAutoObservable } from "mobx";
import { observer } from "mobx-react-lite";
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

// === MobX Store ===
class UsersStore {
  filter = "";
  constructor() {
    makeAutoObservable(this);
  }
  setFilter(value) {
    this.filter = value.toLowerCase();
  }
}
const usersStore = new UsersStore();

// === Компонент ===
const UsersApp = observer(() => {
  const { data, isLoading } = useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const res = await axios.get("https://jsonplaceholder.typicode.com/users");
      return res.data;
    },
  });

  if (isLoading) return <p>Загрузка...</p>;

  const filtered = data.filter((u) =>
    u.name.toLowerCase().includes(usersStore.filter)
  );

  return (
    <div className="p-4">
      <input
        placeholder="Поиск по имени..."
        onChange={(e) => usersStore.setFilter(e.target.value)}
        className="border p-2"
      />
      <ul>
        {filtered.map((u) => (
          <li key={u.id}>
            {u.name} — {u.email}
          </li>
        ))}
      </ul>
    </div>
  );
});

export default UsersApp;
```

---

## 🧭 Как выстраивать архитектуру

| Уровень      | Технология     | Что делает                                             |
| ------------ | -------------- | ------------------------------------------------------ |
| UI           | React          | Отображение компонентов                                |
| Client state | MobX           | Управление локальным состоянием (UI, фильтры, модалки) |
| Server state | TanStack Query | Получение, кэш и мутации серверных данных              |


---

## 📚 План изучения по шагам

| Этап | Тема        | Что изучить                              | Пример              |
| ---- | ----------- | ---------------------------------------- | ------------------- |
| 1    | React Core  | useState, useEffect, props               | TodoApp             |
| 2    | React Query | useQuery, useMutation, invalidateQueries | UsersList + AddUser |
| 3    | MobX        | makeAutoObservable, observer, computed   | UIStore             |
| 4    | Интеграция  | React + Query + MobX                     | UsersApp            |
| 5    | Архитектура | Файловая структура, слои состояния       | booking app         |

---

## 🎯 Идеи для практики

1. **Список задач с API** — загрузка/добавление задач через Query + фильтр через MobX
2. **Каталог товаров** — серверный список, MobX хранит сортировку и фильтры
3. **Панель администратора** — React Query получает данные, MobX управляет вкладками и UI

---

