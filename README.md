# ⚛️ React Frontend Architecture — Scalable Conventions Guide

This document defines the **folder structure**, **naming conventions**, **routing strategy**, and **state management practices** for a large-scale, maintainable, and performant React application built with:

- ⚛️ React + React Router
- 🧰 Redux Toolkit
- 🎨 Tailwind CSS
- 📂 Modular, page-based architecture
- 🔄 Deeply nested and lazy-loaded routes

---

## 📁 Recommended Folder Structure

```
src/
├── assets/
├── components/
│   └── common/
├── constants/
├── hooks/
├── layouts/
├── pages/
│   └── settings/
│       ├── index.jsx
│       ├── components/
│       └── account/
│           └── preferences/
│               └── password/
│                   ├── index.jsx
│                   ├── components/
│                   └── history/
│                       ├── index.jsx
│                       └── components/
├── routes/
├── services/
├── store/
│   ├── store.js
│   └── features/
│       ├── auth/
│       │   ├── authSlice.js
│       │   └── authApi.js
│       └── settings/
│           ├── settingsSlice.js
│           └── settingsApi.js
├── styles/
├── utils/
├── App.jsx
└── main.jsx
```

---

## 🧱 Component & File Naming Conventions

| Type           | Convention           | Example                  |
| -------------- | -------------------- | ------------------------ |
| Component Name | PascalCase           | UserProfile              |
| Folder Name    | camelCase            | userSettings/            |
| File Name      | PascalCase.jsx       | UserCard.jsx             |
| Page File      | index.jsx            | pages/settings/index.jsx |
| CSS Module     | Component.module.css | UserCard.module.css      |

---

## 🌐 Routing Best Practices (React Router v6+)

### Nested Route Structure Example

```js
// routes/routes.tsx
import AppLayout from "@/layouts/AppLayout";
import Dashboard from "@/pages/dashboard/Dashboard";
import Invoices from "@/pages/invoices/Invoices";
import NotFound from "@/pages/notFound/NotFound";
import type { ReactElement } from "react";
import { Navigate } from "react-router";

export interface IRouteConfig {
  path?: string;
  element: ReactElement;
  index?: boolean;
  children?: IRouteConfig[];
}

export const routes: IRouteConfig[] = [
  {
    path: "/",
    element: <AppLayout />,
    children: [
      {
        index: true,
        element: <Navigate to="dashboard" replace />,
      },
      {
        path: "dashboard",
        element: <Dashboard />,
      },
      {
        path: "invoices",
        element: <Invoices />,
      },
    ],
  },
  {
    path: "*",
    element: <NotFound />,
  },
];

```

### Recursive Route Renderer

```js
// routes/AppRouter.tsx
import { Routes, Route } from "react-router";
import { routes, type IRouteConfig } from "./routes";

const renderRoutes = (routes: IRouteConfig[]): React.ReactNode =>
  routes.map(({ path, element, index, children }, key) => {
    if (index) {
      return <Route key={`index-${key}`} index element={element} />;
    }
    return (
      <Route key={`path-${key}`} path={path} element={element}>
        {children && renderRoutes(children)}
      </Route>
    );
  });

const AppRouter = () => {
  return <Routes>{renderRoutes(routes)}</Routes>;
};

export default AppRouter;
```

---

## 🧰 Redux Toolkit Setup

### Folder: `src/store/`

```
store/
├── store.js
└── features/
    ├── auth/
    │   ├── authSlice.js
    │   └── authApi.js
    └── settings/
        ├── settingsSlice.js
        ├── settingsApi.js
        └── account/
            ├── accountSlice.js
            ├── accountApi.js
            └── preferences/
                ├── preferencesSlice.js
                ├── preferencesApi.js
                └── password/
                    ├── passwordSlice.js
                    ├── passwordApi.js
                    └── history/
                        ├── historySlice.js
                        └── historyApi.js
```

### Store Example

```js
// store/store.js
import { configureStore } from "@reduxjs/toolkit";
import authReducer from "./features/auth/authSlice";
import settingsReducer from "./features/settings/settingsSlice";
import historyReducer from "./features/settings/account/preferences/password/history/historySlice";

export const store = configureStore({
  reducer: {
    auth: authReducer,
    settings: settingsReducer,
    history: historyReducer,
  },
});
```

---

## 🎨 Tailwind CSS Best Practices

- Use Tailwind via `globals.css`
- Extend `tailwind.config.js` for themes
- Use utility classes instead of inline styles
- Combine Tailwind with `*.module.css` if needed for scoped styles

---

## 🔁 Route Naming Conventions

| Route Path              | File Location                                                 |
| ----------------------- | ------------------------------------------------------------- |
| `/settings`             | pages/settings/index.jsx                                      |
| `/settings/account`     | pages/settings/account/index.jsx                              |
| `/settings/.../history` | pages/settings/account/preferences/password/history/index.jsx |

✅ Folder path = Route path  
✅ Use `index.jsx` for default pages  
✅ Local components go inside `components/` folders near usage

---

## ✅ Summary

| Topic                | Recommendation                      |
| -------------------- | ----------------------------------- |
| Routing              | Recursive nested + route configs    |
| Component Naming     | PascalCase                          |
| Folder Naming        | camelCase                           |
| Redux Setup          | Central `store/` with domain slices |
| Tailwind Integration | Utility-first + custom config       |
| Component Location   | Local to scope (`components/`)      |
| Page Files           | Use `index.jsx` per route segment   |

---

> 🧠 Follow these to build a solid foundation for scalable, maintainable React apps!
