
### Базовые понятия

| Компонент              | Назначение                                                     |
| ---------------------- | -------------------------------------------------------------- |
| `<BrowserRouter>`      | Контейнер, обеспечивающий навигацию с помощью истории браузера |
| `<Routes>`             | Обёртка для всех маршрутов                                     |
| `<Route>`              | Описание конкретного пути и соответствующего компонента        |
| `<Link>` / `<NavLink>` | Ссылки для навигации без перезагрузки страницы                 |
| `useNavigate()`        | Хук для программной навигации                                  |
| `useParams()`          | Хук для получения параметров URL                               |
| `useLocation()`        | Хук для получения текущего пути и состояния                    |
| `useSearchParams()`    | Работа с query-параметрами                                     |

---



1. 🧩 Простой многостраничник
2. 🪟 Пример модалки через роут

---

## 🧭 1. Установка

```bash
npm install react-router-dom
```

(если ещё нет типов)

```bash
npm install -D @types/react-router-dom
```

---

## 📘 2. Основы

Импортируем:

```tsx
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  useParams,
  useNavigate,
  Outlet,
  useLocation,
} from "react-router-dom";
```

---

## 🧩 3. Пример простого многостраничного приложения (TypeScript)

**Структура:**

```
src/
 ├─ pages/
 │   ├─ Home.tsx
 │   ├─ About.tsx
 │   └─ User.tsx
 ├─ App.tsx
 └─ main.tsx
```

### `pages/Home.tsx`

```tsx
import { Link } from "react-router-dom";

export default function Home() {
  return (
    <div>
      <h1>Главная</h1>
      <p>Добро пожаловать!</p>
      <nav>
        <Link to="/about">О нас</Link> | <Link to="/user/42">Профиль</Link>
      </nav>
    </div>
  );
}
```

### `pages/About.tsx`

```tsx
export default function About() {
  return (
    <div>
      <h1>О нас</h1>
      <p>Это демо-приложение на React Router 6 + TypeScript.</p>
    </div>
  );
}
```

### `pages/User.tsx`

```tsx
import { useParams } from "react-router-dom";

type UserParams = {
  id: string;
};

export default function User() {
  const { id } = useParams<UserParams>();
  return (
    <div>
      <h1>Профиль пользователя</h1>
      <p>ID: {id}</p>
    </div>
  );
}
```

### `App.tsx`

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";
import User from "./pages/User";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/user/:id" element={<User />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## 🪟 4. Пример модального окна по роуту (TypeScript)

### Файловая структура

```
src/
 ├─ pages/
 │   ├─ Gallery.tsx
 │   └─ PhotoModal.tsx
 └─ App.tsx
```

### `pages/Gallery.tsx`

```tsx
import { Link, Outlet, useLocation } from "react-router-dom";

type Photo = {
  id: number;
  url: string;
};

const photos: Photo[] = [
  { id: 1, url: "https://picsum.photos/id/101/300/200" },
  { id: 2, url: "https://picsum.photos/id/102/300/200" },
  { id: 3, url: "https://picsum.photos/id/103/300/200" },
];

export default function Gallery() {
  const location = useLocation();

  return (
    <div>
      <h1>Галерея</h1>
      <div className="grid grid-cols-3 gap-4">
        {photos.map((p) => (
          <Link
            key={p.id}
            to={`/photo/${p.id}`}
            state={{ background: location }}
          >
            <img
              src={p.url}
              alt={`Фото ${p.id}`}
              className="rounded-xl shadow-lg"
            />
          </Link>
        ))}
      </div>

      {/* Outlet нужен для модального окна */}
      <Outlet />
    </div>
  );
}
```

---

### `pages/PhotoModal.tsx`

```tsx
import { useNavigate, useParams } from "react-router-dom";

type PhotoParams = {
  id: string;
};

export default function PhotoModal() {
  const { id } = useParams<PhotoParams>();
  const navigate = useNavigate();

  return (
    <div className="fixed inset-0 bg-black/50 flex justify-center items-center z-50">
      <div className="bg-white rounded-2xl p-6 shadow-lg">
        <img
          src={`https://picsum.photos/id/10${id}/600/400`}
          alt={`Фото ${id}`}
          className="rounded-xl mb-4"
        />
        <button
          onClick={() => navigate(-1)}
          className="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700"
        >
          Закрыть
        </button>
      </div>
    </div>
  );
}
```

---

### `App.tsx`

```tsx
import {
  BrowserRouter,
  Routes,
  Route,
  useLocation,
  Location,
} from "react-router-dom";
import Gallery from "./pages/Gallery";
import PhotoModal from "./pages/PhotoModal";

function AppRoutes() {
  const location = useLocation();
  const state = location.state as { background?: Location };

  return (
    <>
      {/* Основные маршруты */}
      <Routes location={state?.background || location}>
        <Route path="/" element={<Gallery />} />
      </Routes>

      {/* Модальное окно поверх */}
      {state?.background && (
        <Routes>
          <Route path="/photo/:id" element={<PhotoModal />} />
        </Routes>
      )}
    </>
  );
}

export default function App() {
  return (
    <BrowserRouter>
      <AppRoutes />
    </BrowserRouter>
  );
}
```

---

## ⚡️ Важные моменты

* **Типизация параметров:**
  `useParams<{ id: string }>()`
* **Типизация состояния маршрута:**
  `const state = location.state as { background?: Location };`
* **Закрытие модалки:**
  `navigate(-1)` возвращает на предыдущую страницу.

---

