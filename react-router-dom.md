

### Установка

```bash
npm install react-router-dom
```

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

## 📄 Пример 1. Простой многостраничник

### 📁 Структура проекта

```
src/
 ├─ pages/
 │   ├─ Home.tsx
 │   ├─ About.tsx
 │   └─ Contact.tsx
 ├─ App.tsx
 └─ main.tsx
```

### 🧱 `main.tsx`

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

### 🧭 `App.tsx`

```tsx
import { Routes, Route, Link } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";
import Contact from "./pages/Contact";

const App = () => {
  return (
    <div>
      <nav style={{ display: "flex", gap: "1rem" }}>
        <Link to="/">🏠 Home</Link>
        <Link to="/about">ℹ️ About</Link>
        <Link to="/contact">📞 Contact</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </div>
  );
};

export default App;
```

### 📜 Примеры страниц

```tsx
// src/pages/Home.tsx
export default function Home() {
  return <h1>Welcome to the Home page</h1>;
}

// src/pages/About.tsx
export default function About() {
  return <h1>About Us</h1>;
}

// src/pages/Contact.tsx
export default function Contact() {
  return <h1>Contact Page</h1>;
}
```

---

### 📦 `package.json` для многостраничника

```json
{
  "name": "react-router-simple-ts",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.26.1"
  },
  "devDependencies": {
    "@types/react": "^18.3.3",
    "@types/react-dom": "^18.3.3",
    "typescript": "^5.5.4",
    "vite": "^5.4.3"
  }
}
```

---

## 🪟 Пример 2. Модальное окно через маршрутизацию

**Идея:**
Открываем модалку при переходе на `/photo/:id`, но сохраняем фон (предыдущий экран).

### 📁 Структура

```
src/
 ├─ pages/
 │   ├─ Gallery.tsx
 │   └─ PhotoModal.tsx
 ├─ App.tsx
 └─ main.tsx
```

---

### 🧱 `main.tsx`

(аналогично предыдущему примеру)

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

---

### 🧭 `App.tsx`

```tsx
import { Routes, Route, useLocation } from "react-router-dom";
import Gallery from "./pages/Gallery";
import PhotoModal from "./pages/PhotoModal";

export default function App() {
  const location = useLocation();
  const state = location.state as { backgroundLocation?: Location };

  return (
    <>
      {/* Основной слой */}
      <Routes location={state?.backgroundLocation || location}>
        <Route path="/" element={<Gallery />} />
      </Routes>

      {/* Модальное окно */}
      {state?.backgroundLocation && (
        <Routes>
          <Route path="/photo/:id" element={<PhotoModal />} />
        </Routes>
      )}
    </>
  );
}
```

---

### 🖼️ `Gallery.tsx`

```tsx
import { Link, useLocation } from "react-router-dom";

const photos = [
  { id: 1, title: "Mountains" },
  { id: 2, title: "Ocean" },
  { id: 3, title: "Forest" },
];

export default function Gallery() {
  const location = useLocation();

  return (
    <div style={{ padding: "1rem" }}>
      <h1>Gallery</h1>
      <div style={{ display: "flex", gap: "1rem" }}>
        {photos.map((photo) => (
          <Link
            key={photo.id}
            to={`/photo/${photo.id}`}
            state={{ backgroundLocation: location }}
          >
            <div
              style={{
                width: 120,
                height: 120,
                background: "#ccc",
                display: "flex",
                alignItems: "center",
                justifyContent: "center",
                borderRadius: 8,
              }}
            >
              {photo.title}
            </div>
          </Link>
        ))}
      </div>
    </div>
  );
}
```

---

### 🪟 `PhotoModal.tsx`

```tsx
import { useNavigate, useParams } from "react-router-dom";

export default function PhotoModal() {
  const navigate = useNavigate();
  const { id } = useParams();

  return (
    <div
      onClick={() => navigate(-1)}
      style={{
        position: "fixed",
        inset: 0,
        backgroundColor: "rgba(0,0,0,0.5)",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
      }}
    >
      <div
        onClick={(e) => e.stopPropagation()}
        style={{
          background: "white",
          padding: "2rem",
          borderRadius: 12,
          textAlign: "center",
        }}
      >
        <h2>Photo #{id}</h2>
        <p>Some photo details...</p>
        <button onClick={() => navigate(-1)}>Close</button>
      </div>
    </div>
  );
}
```

---

### 📦 `package.json` для примера с модалкой

```json
{
  "name": "react-router-modal-ts",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.26.1"
  },
  "devDependencies": {
    "@types/react": "^18.3.3",
    "@types/react-dom": "^18.3.3",
    "typescript": "^5.5.4",
    "vite": "^5.4.3"
  }
}
```

---

## ✅ Итого

| Пример          | Цель                      | Ключевая особенность                |
| --------------- | ------------------------- | ----------------------------------- |
| Многостраничник | Базовая навигация         | `<Routes>` + `<Link>`               |
| Модальное окно  | Навигация без потери фона | `location.state.backgroundLocation` |

---

