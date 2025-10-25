
---

# ⚛️ Урок: React + Zustand

---

## 🚀 1. Установка

```bash
npm install zustand
```

---

## 🧠 2. Что такое Zustand

Zustand — это **лёгкий и быстрый стейт-менеджер** для React.
Он проще Redux, не требует контекстов и не засоряет код бойлерплейтом.

---

## 📦 3. Простейший пример (Counter)

**store/counterStore.js**

```js
import { create } from 'zustand'

export const useCounterStore = create((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}))
```

**App.jsx**

```jsx
import React from 'react'
import { useCounterStore } from './store/counterStore'

export default function App() {
  const { count, increase, decrease, reset } = useCounterStore()

  return (
    <div style={{ textAlign: 'center', marginTop: 50 }}>
      <h1>🔥 Zustand Counter</h1>
      <h2>{count}</h2>
      <div style={{ display: 'flex', gap: 10, justifyContent: 'center' }}>
        <button onClick={decrease}>-</button>
        <button onClick={reset}>Reset</button>
        <button onClick={increase}>+</button>
      </div>
    </div>
  )
}
```

✅ Глобальное состояние работает без контекстов и пропсов!

---

## 💾 4. Хранение данных в localStorage (`persist`)

Иногда нужно сохранять данные (например, тему, корзину и т.д.)

**store/themeStore.js**

```js
import { create } from 'zustand'
import { persist } from 'zustand/middleware'

export const useThemeStore = create(
  persist(
    (set) => ({
      theme: 'light',
      toggleTheme: () =>
        set((state) => ({
          theme: state.theme === 'light' ? 'dark' : 'light',
        })),
    }),
    {
      name: 'theme-storage', // ключ в localStorage
    }
  )
)
```

**ThemeButton.jsx**

```jsx
import React from 'react'
import { useThemeStore } from './store/themeStore'

export default function ThemeButton() {
  const { theme, toggleTheme } = useThemeStore()

  return (
    <button
      onClick={toggleTheme}
      style={{
        padding: '10px 20px',
        backgroundColor: theme === 'dark' ? '#333' : '#ddd',
        color: theme === 'dark' ? '#fff' : '#000',
        border: 'none',
        borderRadius: 6,
      }}
    >
      Current theme: {theme}
    </button>
  )
}
```

💡 После перезагрузки страницы тема сохраняется, потому что Zustand сам работает с `localStorage`.

---

## 🌐 5. Пример с запросом к API

Создадим стор, который:

* хранит пользователей;
* делает запрос к API (`https://jsonplaceholder.typicode.com/users`);
* сохраняет состояние загрузки и ошибки.

---

**store/userStore.js**

```js
import { create } from 'zustand'

export const useUserStore = create((set) => ({
  users: [],
  loading: false,
  error: null,

  fetchUsers: async () => {
    set({ loading: true, error: null })
    try {
      const res = await fetch('https://jsonplaceholder.typicode.com/users')
      if (!res.ok) throw new Error('Failed to fetch users')
      const data = await res.json()
      set({ users: data, loading: false })
    } catch (err) {
      set({ error: err.message, loading: false })
    }
  },
}))
```

---

**Users.jsx**

```jsx
import React, { useEffect } from 'react'
import { useUserStore } from './store/userStore'

export default function Users() {
  const { users, loading, error, fetchUsers } = useUserStore()

  useEffect(() => {
    fetchUsers()
  }, [fetchUsers])

  if (loading) return <p>⏳ Loading users...</p>
  if (error) return <p>❌ Error: {error}</p>

  return (
    <div style={{ textAlign: 'center', marginTop: 20 }}>
      <h2>👥 Users List</h2>
      <ul style={{ listStyle: 'none', padding: 0 }}>
        {users.map((user) => (
          <li key={user.id} style={{ margin: '6px 0' }}>
            <strong>{user.name}</strong> — {user.email}
          </li>
        ))}
      </ul>
    </div>
  )
}
```

---

**App.jsx**

```jsx
import React from 'react'
import { useCounterStore } from './store/counterStore'
import ThemeButton from './ThemeButton'
import Users from './Users'

export default function App() {
  const { count, increase, decrease, reset } = useCounterStore()

  return (
    <div style={{ textAlign: 'center', marginTop: 40 }}>
      <h1>🔥 Zustand + React Example</h1>

      <section style={{ marginTop: 30 }}>
        <h2>Counter</h2>
        <h3>{count}</h3>
        <div style={{ display: 'flex', gap: 10, justifyContent: 'center' }}>
          <button onClick={decrease}>-</button>
          <button onClick={reset}>Reset</button>
          <button onClick={increase}>+</button>
        </div>
      </section>

      <section style={{ marginTop: 30 }}>
        <h2>Theme Switcher</h2>
        <ThemeButton />
      </section>

      <section style={{ marginTop: 30 }}>
        <h2>Users (API Example)</h2>
        <Users />
      </section>
    </div>
  )
}
```

---

## ⚡ 6. Что мы узнали

✅ Zustand позволяет:

* хранить **глобальное состояние** без Redux/Context API;
* **сохранять данные** через `persist`;
* **делать API-запросы** прямо в store;
* использовать **асинхронные функции** внутри стора;
* удобно **шарить стейт между компонентами**.

---
