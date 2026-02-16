````markdown
TanStack Ecosystem — подробный гайд с базовыми примерами

📌 Что такое TanStack?

[TanStack](https://tanstack.com) — это набор мощных **headless-библиотек** для фронтенда (React, Vue, Solid, Svelte и др.).

Главная идея:  
> Логика без UI.

Вы получаете готовую бизнес-логику (кеширование, таблицы, роутинг и т.д.), но полностью контролируете разметку и стили.


 📦 Основные библиотеки TanStack


1️⃣ TanStack Query (React Query)

Библиотека для управления **server state**.

 Решает проблемы:
- Кеширование
- Refetch
- Background updates
- Retry
- Pagination
- Infinite scroll
- Optimistic updates


📦 Установка


npm install @tanstack/react-query
````


### ⚙️ Базовая настройка

```tsx
// main.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

root.render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
)
```

---

### 🔍 Пример useQuery

```tsx
import { useQuery } from '@tanstack/react-query'

function Users() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetch('/api/users').then(res => res.json())
  })

  if (isLoading) return <p>Loading...</p>
  if (error) return <p>Error!</p>

  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  )
}
```

---

### ✍️ Пример useMutation

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

function AddUser() {
  const queryClient = useQueryClient()

  const mutation = useMutation({
    mutationFn: (newUser) =>
      fetch('/api/users', {
        method: 'POST',
        body: JSON.stringify(newUser),
      }),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] })
    }
  })

  return (
    <button
      onClick={() =>
        mutation.mutate({ name: 'John' })
      }
    >
      Add user
    </button>
  )
}
```

---

## 2️⃣ TanStack Table

Headless-таблица для сложных интерфейсов.

### Возможности:

* Сортировка
* Фильтрация
* Пагинация
* Grouping
* Row selection
* Виртуализация

---

### 📦 Установка

```bash
npm install @tanstack/react-table
```

---

### 📊 Пример базовой таблицы

```tsx
import {
  createColumnHelper,
  useReactTable,
  getCoreRowModel
} from '@tanstack/react-table'

const columnHelper = createColumnHelper()

const columns = [
  columnHelper.accessor('firstName', {
    header: 'First Name',
  }),
  columnHelper.accessor('age', {
    header: 'Age',
  }),
]

function Table({ data }) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
  })

  return (
    <table>
      <thead>
        {table.getHeaderGroups().map(headerGroup => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map(header => (
              <th key={header.id}>
                {header.column.columnDef.header}
              </th>
            ))}
          </tr>
        ))}
      </thead>

      <tbody>
        {table.getRowModel().rows.map(row => (
          <tr key={row.id}>
            {row.getVisibleCells().map(cell => (
              <td key={cell.id}>
                {cell.getValue()}
              </td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
  )
}
```

---

## 3️⃣ TanStack Router

Типобезопасный роутер.

### Плюсы:

* Type-safe routes
* Data loaders
* Suspense-ready
* Возможность file-based routing

---

### 📦 Установка

```bash
npm install @tanstack/react-router
```

---

### 🛣 Минимальный пример

```tsx
import {
  createRouter,
  RouterProvider,
  Route,
  RootRoute,
  Outlet
} from '@tanstack/react-router'

const rootRoute = new RootRoute({
  component: () => (
    <div>
      <Outlet />
    </div>
  )
})

const indexRoute = new Route({
  getParentRoute: () => rootRoute,
  path: '/',
  component: () => <h1>Home</h1>
})

const routeTree = rootRoute.addChildren([indexRoute])

const router = createRouter({ routeTree })

function App() {
  return <RouterProvider router={router} />
}
```

---

## 4️⃣ TanStack Virtual

Библиотека для виртуализации больших списков.

### Используется для:

* Больших таблиц
* Infinite scroll
* Чатов
* Списков с тысячами элементов

---

### 📦 Установка

```bash
npm install @tanstack/react-virtual
```

---

### ⚡ Пример

```tsx
import { useVirtualizer } from '@tanstack/react-virtual'
import React from 'react'

function List({ items }) {
  const parentRef = React.useRef(null)

  const rowVirtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 35,
  })

  return (
    <div ref={parentRef} style={{ height: 400, overflow: 'auto' }}>
      <div
        style={{
          height: `${rowVirtualizer.getTotalSize()}px`,
          position: 'relative',
        }}
      >
        {rowVirtualizer.getVirtualItems().map(virtualRow => (
          <div
            key={virtualRow.key}
            style={{
              position: 'absolute',
              top: 0,
              transform: `translateY(${virtualRow.start}px)`
            }}
          >
            {items[virtualRow.index]}
          </div>
        ))}
      </div>
    </div>
  )
}
```

---

# 🧠 Как библиотеки работают вместе

Типичная архитектура:

* **TanStack Router** → маршрутизация
* **TanStack Query** → загрузка и кеширование данных
* **TanStack Table** → отображение таблиц
* **TanStack Virtual** → оптимизация производительности

---

# 🚀 Почему TanStack — это уровень senior-разработки?

* Чёткое разделение server state и client state
* Отличная типизация
* Минимум boilerplate
* Высокая производительность
* Гибкость и контроль

---

# 📚 Полезные ссылки

* [https://tanstack.com/query](https://tanstack.com/query)
* [https://tanstack.com/table](https://tanstack.com/table)
* [https://tanstack.com/router](https://tanstack.com/router)
* [https://tanstack.com/virtual](https://tanstack.com/virtual)

---

# 🏁 Итог

TanStack — это экосистема инструментов для построения масштабируемых фронтенд-приложений.

Если ты хочешь:

* писать enterprise-код
* проходить senior-собеседования
* строить архитектурно чистые приложения

TanStack должен быть в твоём арсенале.

```
```
