
---

## 🚀 Урок: Redux Toolkit — современный Redux без боли

### 🎯 Цель

Понять, зачем нужен Redux Toolkit, как им пользоваться, и как сократить код в 3 раза по сравнению с классическим Redux.

---

## 1️⃣ Проблема классического Redux

Классический Redux:

* Много шаблонного кода (actions, types, reducers)
* Трудно поддерживать
* Мутировать состояние нельзя (всё через spread)

RTK решает это с помощью:

* `createSlice` — генерация редьюсера + экшенов
* `configureStore` — простой стор с devTools и middleware
* `createAsyncThunk` — для асинхронных экшенов

---

## 2️⃣ Установка

```bash
npm install @reduxjs/toolkit react-redux
```

---

## 3️⃣ Создание Slice

Обычно слайсы создаются в `src/features/[featureName]/`.

**Пример: counterSlice.js**

```js
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: {
    value: 0,
  },
  reducers: {
    increment: (state) => {
      state.value += 1; // Можно мутировать благодаря Immer
    },
    decrement: (state) => {
      state.value -= 1;
    },
    addByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, addByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

---

## 4️⃣ Настройка Store

**store.js**

```js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./features/counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

**index.js**

```js
import React from "react";
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";
import { store } from "./store";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

---

## 5️⃣ Использование в компонентах

**App.jsx**

```jsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement, addByAmount } from "./features/counterSlice";

export default function App() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div className="p-4 text-center">
      <h1 className="text-2xl font-bold mb-2">Redux Toolkit Counter</h1>
      <p className="text-xl mb-4">{count}</p>
      <div className="flex justify-center gap-2">
        <button
          onClick={() => dispatch(decrement())}
          className="bg-red-500 text-white px-4 py-2 rounded"
        >
          -
        </button>
        <button
          onClick={() => dispatch(increment())}
          className="bg-green-500 text-white px-4 py-2 rounded"
        >
          +
        </button>
        <button
          onClick={() => dispatch(addByAmount(10))}
          className="bg-blue-500 text-white px-4 py-2 rounded"
        >
          +10
        </button>
      </div>
    </div>
  );
}
```

---

## 6️⃣ Асинхронные действия (API-запросы)

Redux Toolkit имеет встроенный хелпер — `createAsyncThunk`.

**Пример: загрузка пользователей**

```js
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";

export const fetchUsers = createAsyncThunk(
  "users/fetchUsers",
  async () => {
    const res = await fetch("https://jsonplaceholder.typicode.com/users");
    return await res.json();
  }
);

const userSlice = createSlice({
  name: "users",
  initialState: {
    data: [],
    status: "idle",
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.status = "loading";
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.status = "succeeded";
        state.data = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});

export default userSlice.reducer;
```

**Использование в компоненте:**

```jsx
import React, { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { fetchUsers } from "./features/userSlice";

export default function Users() {
  const dispatch = useDispatch();
  const { data, status } = useSelector((state) => state.users);

  useEffect(() => {
    if (status === "idle") dispatch(fetchUsers());
  }, [status, dispatch]);

  if (status === "loading") return <p>Loading...</p>;
  if (status === "failed") return <p>Error fetching users</p>;

  return (
    <ul>
      {data.map((user) => (
        <li key={user.id} className="border-b p-2">
          {user.name}
        </li>
      ))}
    </ul>
  );
}
```

---

## 7️⃣ "Крутые" фичи Redux Toolkit 💪

### ✅ `createEntityAdapter`

Позволяет эффективно хранить и обновлять списки (например, посты или пользователи):

```js
import { createSlice, createEntityAdapter } from "@reduxjs/toolkit";

const postsAdapter = createEntityAdapter({
  sortComparer: (a, b) => b.date.localeCompare(a.date),
});

const postsSlice = createSlice({
  name: "posts",
  initialState: postsAdapter.getInitialState(),
  reducers: {
    postAdded: postsAdapter.addOne,
    postUpdated: postsAdapter.updateOne,
    postRemoved: postsAdapter.removeOne,
  },
});

export const { postAdded, postUpdated, postRemoved } = postsSlice.actions;
export default postsSlice.reducer;
```

---

### ✅ RTK Query (API-клиент встроенный в Redux)

Redux Toolkit включает **RTK Query** — мощную систему для запросов к API и кэширования ответов.

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const postsApi = createApi({
  reducerPath: "postsApi",
  baseQuery: fetchBaseQuery({ baseUrl: "https://jsonplaceholder.typicode.com" }),
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => "/posts",
    }),
  }),
});

export const { useGetPostsQuery } = postsApi;
```

**store.js**

```js
import { configureStore } from "@reduxjs/toolkit";
import { postsApi } from "./features/postsApi";

export const store = configureStore({
  reducer: {
    [postsApi.reducerPath]: postsApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(postsApi.middleware),
});
```

**Компонент:**

```jsx
import React from "react";
import { useGetPostsQuery } from "./features/postsApi";

export default function Posts() {
  const { data, isLoading } = useGetPostsQuery();

  if (isLoading) return <p>Loading...</p>;

  return (
    <div>
      {data.slice(0, 5).map((post) => (
        <div key={post.id} className="border p-2 mb-2">
          <h2 className="font-bold">{post.title}</h2>
          <p>{post.body}</p>
        </div>
      ))}
    </div>
  );
}
```

---

## 🧠 Краткое резюме

| Что         | Классический Redux | Redux Toolkit       |
| ----------- | ------------------ | ------------------- |
| Reducers    | вручную            | `createSlice`       |
| Actions     | вручную            | автоматически       |
| Store       | `createStore`      | `configureStore`    |
| Асинхронка  | thunk/saga         | `createAsyncThunk`  |
| API-запросы | вручную            | RTK Query           |
| Код         | громоздкий         | компактный и чистый |

---
