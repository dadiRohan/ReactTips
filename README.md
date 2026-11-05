# React.js: The Complete Guide, from Basic to Advanced

> A from-scratch, hands-on guide that takes you from "What is React?" to production-grade patterns.
> Examples target **React 19.x** with **Vite**, and every concept comes with runnable code.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Project Setup](#3-project-setup)
4. [Project Structure](#4-project-structure)
5. [JSX](#5-jsx)
6. [Components and Props](#6-components-and-props)
7. [Lists and Conditional Rendering](#7-lists-and-conditional-rendering)
8. [Handling Events](#8-handling-events)
9. [State with useState](#9-state-with-usestate)
10. [Forms](#10-forms)
11. [Styling](#11-styling)
12. [The useEffect Hook](#12-the-useeffect-hook)
13. [useRef, useContext and useReducer](#13-useref-usecontext-and-usereducer)
14. [Memoization Hooks](#14-memoization-hooks)
15. [Other Built-in Hooks](#15-other-built-in-hooks)
16. [Custom Hooks](#16-custom-hooks)
17. [Mini Project: Todo App](#17-mini-project-todo-app)
18. [Routing with React Router](#18-routing-with-react-router)
19. [Data Fetching](#19-data-fetching)
20. [React 19 Features](#20-react-19-features)
21. [State Management](#21-state-management)
22. [Advanced Patterns](#22-advanced-patterns)
23. [Performance Optimization](#23-performance-optimization)
24. [Server-Side Rendering and Server Components](#24-server-side-rendering-and-server-components)
25. [React with TypeScript](#25-react-with-typescript)
26. [Testing](#26-testing)
27. [Accessibility](#27-accessibility)
28. [Security](#28-security)
29. [Build and Deployment](#29-build-and-deployment)
30. [Tooling](#30-tooling)
31. [Best Practices and Common Mistakes](#31-best-practices-and-common-mistakes)
32. [Learning Roadmap](#32-learning-roadmap)
33. [Resources](#33-resources)

---

## 1. Introduction

**React** is an open-source JavaScript library for building user interfaces out of small, reusable pieces called **components**. It was created at Facebook (now Meta) and is maintained by Meta and a large open-source community.

### Why React?

| Idea | What it means |
| --- | --- |
| **Declarative** | You describe *what the UI should look like* for a given state. React figures out how to update the DOM. |
| **Component-based** | UIs are built from isolated, composable, reusable components. |
| **Unidirectional data flow** | Data flows down from parent to child through props, which makes apps predictable. |
| **Learn once, write anywhere** | The same ideas power web (React DOM), mobile (React Native) and more. |
| **Huge ecosystem** | Routing, data fetching, state management, testing and UI kits are all well covered. |

### How React Renders

Every UI update goes through three steps:

```
1. TRIGGER   ->  initial render (createRoot) or a state update (setState)
2. RENDER    ->  React calls your components to work out what should be on screen
3. COMMIT    ->  React applies the minimal set of changes to the real DOM
```

React keeps a lightweight tree of elements (often called the *virtual DOM*), compares the new tree with the previous one (**reconciliation**), and updates only what changed.

### A Component Tree

```
<App>
 ├── <Header />
 ├── <Main>
 │    ├── <ProductList>
 │    │     └── <ProductCard /> x N
 │    └── <Cart />
 └── <Footer />
```

---

## 2. Prerequisites

You will be far more productive if you are comfortable with these JavaScript features:

```js
// Variables and arrow functions
const add = (a, b) => a + b;

// Destructuring
const { name, age = 18 } = user;
const [first, ...rest] = list;

// Spread and rest
const next = { ...user, age: 30 };
const all = [...list, 4];

// Template literals
const msg = `Hello, ${name}`;

// Array methods you will use every day
items.map((x) => x * 2);
items.filter((x) => x > 1);
items.reduce((sum, x) => sum + x, 0);
items.find((x) => x.id === 3);

// Optional chaining and nullish coalescing
const city = user?.address?.city ?? "Unknown";

// Modules
import { something } from "./file.js";
export default function Thing() {}

// Promises and async/await
const res = await fetch("/api/users");
const data = await res.json();
```

You also need:

- **Node.js** (current LTS) and **npm** (or pnpm / yarn)
- Basic HTML and CSS
- A code editor (VS Code recommended)
- Git basics

---

## 3. Project Setup

### Recommended: Vite

> **Note:** Create React App (CRA) has been deprecated. Use **Vite** for single-page apps, or a framework such as **Next.js** or **React Router (framework mode)** when you need server rendering.

```bash
# JavaScript
npm create vite@latest my-react-app -- --template react

# TypeScript
npm create vite@latest my-react-app -- --template react-ts

cd my-react-app
npm install
npm run dev
```

Open the URL printed in the terminal (usually `http://localhost:5173`).

### Scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Starts the dev server with hot module replacement |
| `npm run build` | Creates an optimized production build in `dist/` |
| `npm run preview` | Serves the production build locally |
| `npm run lint` | Runs ESLint |

### Under the Hood: What Does "From Scratch" Really Look Like?

React needs only an HTML page with a root element and one entry file.

**`index.html`**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

**`src/main.jsx`**

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.jsx";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

**`src/App.jsx`**

```jsx
export default function App() {
  return <h1>Hello, React!</h1>;
}
```

`StrictMode` runs extra checks in development (for example, it renders components and runs effects twice to expose bugs). It has no effect in production.

---

## 4. Project Structure

A typical Vite project:

```
my-react-app/
├── public/                # Static files served as-is
├── src/
│   ├── assets/            # Images, fonts
│   ├── components/        # Reusable UI components
│   ├── hooks/             # Custom hooks
│   ├── pages/             # Route-level components
│   ├── context/           # Context providers
│   ├── services/          # API calls
│   ├── utils/             # Helper functions
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── index.html
├── package.json
└── vite.config.js
```

For larger apps, prefer **feature-based** folders so related code lives together:

```
src/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── api.js
│   │   └── index.js
│   └── products/
├── shared/
│   ├── components/
│   └── hooks/
└── app/
```

---

## 5. JSX

**JSX** is a syntax extension that lets you write markup inside JavaScript. It compiles to plain function calls.

```jsx
const element = <h1 className="title">Hello, {name}!</h1>;
```

### The Rules

1. **Return a single root.** Wrap siblings in a parent element or a Fragment (`<>...</>`).
2. **Close every tag.** Use `<img />`, `<input />`, `<br />`.
3. **Use camelCase attributes.** `className`, `htmlFor`, `onClick`, `tabIndex`.
4. **Use `{}` for JavaScript expressions.** Not statements such as `if` or `for`.
5. **Style is an object.** `style={{ color: "red", fontSize: 16 }}`.

```jsx
function Profile() {
  const user = { name: "Asha", avatar: "/asha.png", online: true };

  return (
    <>
      <img src={user.avatar} alt={`${user.name}'s avatar`} className="avatar" />
      <h2 style={{ color: user.online ? "green" : "gray" }}>{user.name}</h2>
      {/* This is a JSX comment */}
    </>
  );
}
```

### Expressions You Can Put in `{}`

```jsx
<p>{2 + 2}</p>
<p>{user.name.toUpperCase()}</p>
<p>{isLoggedIn ? "Welcome back" : "Please sign in"}</p>
<p>{items.length > 0 && `${items.length} items`}</p>
```

> **Gotcha:** `{count && <Badge />}` renders `0` when `count` is `0`. Use `{count > 0 && <Badge />}` instead.

---

## 6. Components and Props

A component is a JavaScript function that returns JSX. Component names **must start with a capital letter**.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default function App() {
  return (
    <>
      <Greeting name="Asha" />
      <Greeting name="Ravi" />
    </>
  );
}
```

### Props

Props are read-only inputs passed from parent to child.

```jsx
function Button({ label, variant = "primary", disabled = false, onClick }) {
  return (
    <button className={`btn btn-${variant}`} disabled={disabled} onClick={onClick}>
      {label}
    </button>
  );
}

<Button label="Save" onClick={() => console.log("saved")} />
<Button label="Delete" variant="danger" />
```

> **Props are immutable.** Never modify them. If a component needs to change something, it should use state, or ask the parent to change it through a callback prop.

> **React 19 note:** `propTypes` and `defaultProps` on function components are no longer supported. Use default parameter values (as above) and TypeScript for type safety.

### The `children` Prop

```jsx
function Card({ title, children }) {
  return (
    <section className="card">
      <h3>{title}</h3>
      <div className="card-body">{children}</div>
    </section>
  );
}

<Card title="Profile">
  <p>Anything can go in here.</p>
  <Button label="Edit" />
</Card>
```

### Spreading Props

```jsx
function Input(props) {
  return <input className="input" {...props} />;
}

<Input type="email" placeholder="you@example.com" required />
```

### Composition over Inheritance

Build complex UIs by combining simple components rather than creating class hierarchies.

```jsx
function Dialog({ title, children, footer }) {
  return (
    <div role="dialog" aria-label={title}>
      <h2>{title}</h2>
      {children}
      <div className="footer">{footer}</div>
    </div>
  );
}

<Dialog title="Confirm" footer={<Button label="OK" />}>
  <p>Are you sure?</p>
</Dialog>
```

### Pure Components

A component should be **pure**: given the same props, state and context, it returns the same JSX, and it does not change anything that existed before it rendered. Put side effects (network calls, subscriptions, DOM changes) in **event handlers** or **effects**, not in the render body.

---

## 7. Lists and Conditional Rendering

### Rendering Lists

```jsx
const products = [
  { id: 1, name: "Keyboard", price: 49 },
  { id: 2, name: "Mouse", price: 25 },
  { id: 3, name: "Monitor", price: 189 },
];

function ProductList() {
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>
          {p.name}: ${p.price}
        </li>
      ))}
    </ul>
  );
}
```

### Keys

- Keys must be **unique among siblings** and **stable** across renders.
- Use a database ID or another stable identifier.
- Avoid array indexes when items can be reordered, inserted or removed. That causes subtle state bugs.
- `key` is not passed to your component as a prop.

### Conditional Rendering

```jsx
function Status({ isLoading, error, data }) {
  // 1. Early return
  if (isLoading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;

  return (
    <div>
      {/* 2. Ternary */}
      {data.length ? <List items={data} /> : <p>No results.</p>}

      {/* 3. Logical AND */}
      {data.length > 10 && <Pagination />}
    </div>
  );
}
```

---

## 8. Handling Events

```jsx
function Demo() {
  function handleClick(event) {
    console.log("Clicked!", event.target);
  }

  function handleSubmit(event) {
    event.preventDefault(); // stop the page from reloading
    console.log("Submitted");
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="button" onClick={handleClick}>Click me</button>
      <button type="button" onClick={() => alert("Inline handler")}>Inline</button>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Common Mistakes

```jsx
// Wrong: calls the function immediately during render
<button onClick={handleClick()}>Click</button>

// Right: pass the function itself
<button onClick={handleClick}>Click</button>

// Right: pass arguments with an arrow function
<button onClick={() => handleDelete(item.id)}>Delete</button>
```

### Common Event Props

`onClick`, `onChange`, `onInput`, `onSubmit`, `onFocus`, `onBlur`, `onKeyDown`, `onKeyUp`, `onMouseEnter`, `onMouseLeave`, `onScroll`, `onDrop`.

### Stopping Propagation

```jsx
<div onClick={() => console.log("parent")}>
  <button onClick={(e) => { e.stopPropagation(); console.log("child"); }}>
    Click
  </button>
</div>
```

---

## 9. State with useState

**State** is data a component remembers between renders. Changing state triggers a re-render.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### State Is a Snapshot

Within a single render, `count` never changes. Calling `setCount` schedules a new render with the new value.

```jsx
function handleClick() {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
  // Result: count increases by 1, not 3 (each call used the same snapshot)
}

// Use the updater function when the next value depends on the previous one
function handleClickCorrect() {
  setCount((c) => c + 1);
  setCount((c) => c + 1);
  setCount((c) => c + 1);
  // Result: count increases by 3
}
```

### Updating Objects and Arrays (Immutably)

Never mutate state directly. Always create a new object or array.

```jsx
const [user, setUser] = useState({ name: "Asha", address: { city: "Mumbai" } });
const [todos, setTodos] = useState([]);

// Object
setUser({ ...user, name: "Ravi" });
setUser({ ...user, address: { ...user.address, city: "Pune" } });

// Array: add, remove, update
setTodos([...todos, newTodo]);
setTodos(todos.filter((t) => t.id !== id));
setTodos(todos.map((t) => (t.id === id ? { ...t, done: !t.done } : t)));
```

### Lazy Initial State

Pass a function when the initial value is expensive to compute. It runs only on the first render.

```jsx
const [data, setData] = useState(() => JSON.parse(localStorage.getItem("data") ?? "[]"));
```

### Choosing and Structuring State

- **Keep state minimal.** If a value can be calculated from other state or props, calculate it during render instead of storing it.
- **Avoid duplicated state** and contradictory flags (for example `isLoading` and `isError` both true).
- **Group related values** into one object or use `useReducer`.
- **Lift state up.** When two components need the same data, move the state to their closest common parent and pass it down.

```jsx
// Derived value: no extra state needed
const [items, setItems] = useState([]);
const total = items.reduce((sum, i) => sum + i.price, 0);
```

### Resetting State with `key`

Changing a component's `key` tells React to throw away its state and start fresh.

```jsx
<Profile key={userId} userId={userId} />
```

---

## 10. Forms

### Controlled Components

React state is the single source of truth for the input's value.

```jsx
import { useState } from "react";

function SignupForm() {
  const [form, setForm] = useState({ name: "", email: "", plan: "free", agree: false });

  function handleChange(e) {
    const { name, value, type, checked } = e.target;
    setForm((prev) => ({ ...prev, [name]: type === "checkbox" ? checked : value }));
  }

  function handleSubmit(e) {
    e.preventDefault();
    console.log(form);
  }

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="name">Name</label>
      <input id="name" name="name" value={form.name} onChange={handleChange} required />

      <label htmlFor="email">Email</label>
      <input id="email" name="email" type="email" value={form.email} onChange={handleChange} required />

      <label htmlFor="plan">Plan</label>
      <select id="plan" name="plan" value={form.plan} onChange={handleChange}>
        <option value="free">Free</option>
        <option value="pro">Pro</option>
      </select>

      <label>
        <input type="checkbox" name="agree" checked={form.agree} onChange={handleChange} />
        I agree to the terms
      </label>

      <button type="submit" disabled={!form.agree}>Sign up</button>
    </form>
  );
}
```

### Uncontrolled Components

Let the DOM hold the value and read it when needed (via `FormData` or a ref).

```jsx
function SearchForm({ onSearch }) {
  function handleSubmit(e) {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    onSearch(data.get("query"));
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="query" defaultValue="" />
      <button>Search</button>
    </form>
  );
}
```

### Validation

Validate on submit (and optionally on blur), keep errors in state, and render them accessibly.

```jsx
const [errors, setErrors] = useState({});

function validate(values) {
  const next = {};
  if (!values.email.includes("@")) next.email = "Enter a valid email";
  if (values.password.length < 8) next.password = "Use at least 8 characters";
  return next;
}

// In the JSX
<input aria-invalid={!!errors.email} aria-describedby="email-error" />
{errors.email && <p id="email-error" role="alert">{errors.email}</p>}
```

For complex forms, use **React Hook Form** with **Zod** (see [Section 20](#20-react-19-features) and [Section 31](#31-best-practices-and-common-mistakes)), or React 19 **Actions**.

---

## 11. Styling

### Plain CSS

```jsx
import "./Button.css";

<button className="btn btn-primary">Save</button>
```

### CSS Modules (scoped by default)

```css
/* Button.module.css */
.button { padding: 8px 16px; border-radius: 6px; }
.primary { background: #4f46e5; color: white; }
```

```jsx
import styles from "./Button.module.css";

<button className={`${styles.button} ${styles.primary}`}>Save</button>
```

### Inline Styles

```jsx
<div style={{ display: "flex", gap: 8, padding: 16 }}>...</div>
```

### Tailwind CSS

```bash
npm install tailwindcss @tailwindcss/vite
```

```jsx
<button className="rounded-lg bg-indigo-600 px-4 py-2 text-white hover:bg-indigo-500">
  Save
</button>
```

Follow the current Tailwind installation guide for your version to register the Vite plugin and import Tailwind in your CSS.

### Conditional Class Names

```jsx
// npm install clsx
import clsx from "clsx";

<button className={clsx("btn", isActive && "btn-active", disabled && "btn-disabled")}>
  Click
</button>
```

### Other Options

- **Component libraries:** MUI, Chakra UI, Mantine, shadcn/ui (built on Radix UI), Ant Design
- **CSS-in-JS:** styled-components, Emotion. Runtime CSS-in-JS can conflict with Server Components, so zero-runtime options (Vanilla Extract, Panda CSS, CSS Modules, Tailwind) are popular for new projects.

---

## 12. The useEffect Hook

`useEffect` lets a component **synchronize with something outside React**: the network, browser APIs, timers, third-party widgets.

```jsx
import { useEffect, useState } from "react";

function Clock() {
  const [time, setTime] = useState(() => new Date());

  useEffect(() => {
    const id = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(id); // cleanup
  }, []); // empty array: run once after mount

  return <p>{time.toLocaleTimeString()}</p>;
}
```

### The Dependency Array

```jsx
useEffect(() => { /* runs after EVERY render */ });
useEffect(() => { /* runs once (mount; cleanup on unmount) */ }, []);
useEffect(() => { /* runs when `a` or `b` change */ }, [a, b]);
```

Every reactive value (props, state, variables derived from them) used inside the effect **must** be listed. The `react-hooks/exhaustive-deps` lint rule enforces this. Do not silence it. Restructure the code instead.

### Fetching Data in an Effect (with Cleanup)

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const controller = new AbortController();
    setLoading(true);
    setError(null);

    fetch(`/api/users/${userId}`, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json();
      })
      .then(setUser)
      .catch((err) => {
        if (err.name !== "AbortError") setError(err);
      })
      .finally(() => setLoading(false));

    return () => controller.abort(); // cancel stale requests
  }, [userId]);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  return <h1>{user.name}</h1>;
}
```

> In real apps, prefer a data-fetching library such as **TanStack Query** (see [Section 19](#19-data-fetching)) rather than writing this by hand.

### You Might Not Need an Effect

Effects are an escape hatch. Avoid them for:

| Situation | Do this instead |
| --- | --- |
| Computing a value from props/state | Calculate it during render (`useMemo` only if truly expensive) |
| Responding to a user action (click, submit) | Put the logic in the **event handler** |
| Resetting state when a prop changes | Use a `key` on the component |
| Notifying a parent of a change | Call the parent's callback in the event handler |

### Effects Run Twice in Development

In `StrictMode`, React mounts, unmounts and re-mounts every component once to verify your cleanup is correct. If your effect breaks when run twice, it is missing a cleanup.

---

## 13. useRef, useContext and useReducer

### useRef

`useRef` holds a value that **persists across renders without causing re-renders**.

```jsx
import { useRef, useEffect } from "react";

// 1. Access a DOM node
function AutoFocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} />;
}

// 2. Store a mutable value (timer IDs, previous values, etc.)
function Stopwatch() {
  const [elapsed, setElapsed] = useState(0);
  const intervalRef = useRef(null);

  const start = () => {
    intervalRef.current = setInterval(() => setElapsed((e) => e + 1), 1000);
  };
  const stop = () => clearInterval(intervalRef.current);

  return (
    <>
      <p>{elapsed}s</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </>
  );
}
```

Do not read or write `ref.current` during rendering (except for initialization). Use it in event handlers and effects.

### useContext

Context passes data through the tree **without prop drilling**.

```jsx
// ThemeContext.jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext(null);

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const toggle = () => setTheme((t) => (t === "light" ? "dark" : "light"));

  // React 19: render the context directly as a provider
  return <ThemeContext value={{ theme, toggle }}>{children}</ThemeContext>;
}

// Custom hook with a helpful error
export function useTheme() {
  const ctx = useContext(ThemeContext);
  if (ctx === null) throw new Error("useTheme must be used inside <ThemeProvider>");
  return ctx;
}
```

```jsx
// main.jsx
<ThemeProvider>
  <App />
</ThemeProvider>

// Any descendant
function ThemeButton() {
  const { theme, toggle } = useTheme();
  return <button onClick={toggle}>Current: {theme}</button>;
}
```

> Before React 19 you wrote `<ThemeContext.Provider value={...}>`. That still works, but `<ThemeContext value={...}>` is the new, shorter form.

**Performance tip:** every consumer re-renders when the context value changes. Split unrelated data into separate contexts, and keep rapidly changing state out of context.

### useReducer

`useReducer` is ideal when state has several sub-values or complex update logic.

```jsx
import { useReducer } from "react";

const initialState = { items: [], filter: "all" };

function reducer(state, action) {
  switch (action.type) {
    case "added":
      return { ...state, items: [...state.items, { id: crypto.randomUUID(), text: action.text, done: false }] };
    case "toggled":
      return {
        ...state,
        items: state.items.map((i) => (i.id === action.id ? { ...i, done: !i.done } : i)),
      };
    case "deleted":
      return { ...state, items: state.items.filter((i) => i.id !== action.id) };
    case "filter_changed":
      return { ...state, filter: action.filter };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function Todos() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <button onClick={() => dispatch({ type: "added", text: "Learn React" })}>Add</button>
      <ul>
        {state.items.map((i) => (
          <li key={i.id} onClick={() => dispatch({ type: "toggled", id: i.id })}>
            {i.done ? "[x]" : "[ ]"} {i.text}
          </li>
        ))}
      </ul>
    </>
  );
}
```

Combine `useReducer` + `useContext` to build a lightweight global store without extra libraries.

---

## 14. Memoization Hooks

React re-renders a component whenever its state changes or its **parent re-renders**. Memoization skips unnecessary work. **Measure first** with the React DevTools Profiler. Do not add memoization by default.

### useMemo: cache a calculated value

```jsx
import { useMemo } from "react";

function ProductTable({ products, query }) {
  const visible = useMemo(
    () => products.filter((p) => p.name.toLowerCase().includes(query.toLowerCase())),
    [products, query]
  );

  return <Table rows={visible} />;
}
```

### useCallback: cache a function

Useful when passing callbacks to memoized children or listing them as effect dependencies.

```jsx
import { memo, useCallback, useState } from "react";

const Row = memo(function Row({ item, onDelete }) {
  console.log("render row", item.id);
  return <li onClick={() => onDelete(item.id)}>{item.name}</li>;
});

function List({ items }) {
  const [list, setList] = useState(items);

  const handleDelete = useCallback((id) => {
    setList((prev) => prev.filter((i) => i.id !== id));
  }, []);

  return <ul>{list.map((i) => <Row key={i.id} item={i} onDelete={handleDelete} />)}</ul>;
}
```

### React.memo: skip re-rendering when props are unchanged

`memo(Component)` re-renders only when a prop changes (shallow comparison). It is only effective if the props you pass are stable (primitives, or memoized objects and functions).

> **React Compiler:** With the [React Compiler](#20-react-19-features) enabled, most manual `useMemo`, `useCallback` and `memo` calls become unnecessary because the compiler memoizes automatically at build time.

---

## 15. Other Built-in Hooks

| Hook | Purpose |
| --- | --- |
| `useLayoutEffect` | Like `useEffect` but runs **before the browser paints**. Use it to measure layout and avoid flicker. |
| `useId` | Generates stable, unique IDs for accessibility attributes (`htmlFor`, `aria-describedby`). |
| `useTransition` | Marks a state update as **non-urgent** so the UI stays responsive. |
| `useDeferredValue` | Defers re-rendering of a slow part of the UI based on a fast-changing value. |
| `useSyncExternalStore` | Subscribes to external stores (used by state libraries). |
| `useImperativeHandle` | Customizes what a parent can access through a ref. |
| `useDebugValue` | Adds a label to custom hooks in DevTools. |
| `useInsertionEffect` | For CSS-in-JS library authors. |
| `useActionState`, `useOptimistic`, `use`, `useFormStatus` | React 19 additions (see [Section 20](#20-react-19-features)). |
| `useEffectEvent` | Extracts non-reactive logic from effects (React 19.2). |

### useId

```jsx
import { useId } from "react";

function Field({ label }) {
  const id = useId();
  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </>
  );
}
```

### useTransition

```jsx
import { useState, useTransition } from "react";

function Search() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    setQuery(e.target.value); // urgent: keep the input responsive
    startTransition(() => {
      setResults(expensiveFilter(e.target.value)); // non-urgent
    });
  }

  return (
    <>
      <input value={query} onChange={handleChange} />
      {isPending && <p>Updating...</p>}
      <ResultList items={results} />
    </>
  );
}
```

### useDeferredValue

```jsx
import { useDeferredValue, useState } from "react";

function SearchPage() {
  const [query, setQuery] = useState("");
  const deferredQuery = useDeferredValue(query);

  return (
    <>
      <input value={query} onChange={(e) => setQuery(e.target.value)} />
      <SlowResults query={deferredQuery} /> {/* wrap in memo for best results */}
    </>
  );
}
```

### useImperativeHandle (with ref as a prop in React 19)

```jsx
import { useImperativeHandle, useRef } from "react";

function FancyInput({ ref }) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => (inputRef.current.value = ""),
  }));

  return <input ref={inputRef} />;
}

function Parent() {
  const ref = useRef(null);
  return (
    <>
      <FancyInput ref={ref} />
      <button onClick={() => ref.current.focus()}>Focus</button>
    </>
  );
}
```

### useSyncExternalStore

```jsx
import { useSyncExternalStore } from "react";

function useOnlineStatus() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener("online", callback);
      window.addEventListener("offline", callback);
      return () => {
        window.removeEventListener("online", callback);
        window.removeEventListener("offline", callback);
      };
    },
    () => navigator.onLine, // client snapshot
    () => true              // server snapshot
  );
}
```

### The Rules of Hooks

1. Call hooks **only at the top level** of a component or custom hook. Never inside loops, conditions or nested functions.
2. Call hooks **only from React function components or custom hooks**.
3. Custom hook names **must start with `use`**.

---

## 16. Custom Hooks

A custom hook is a function whose name starts with `use` and that calls other hooks. It lets you **reuse stateful logic** (not state itself, since each call gets its own state).

### useLocalStorage

```jsx
import { useEffect, useState } from "react";

export function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const stored = localStorage.getItem(key);
      return stored ? JSON.parse(stored) : initialValue;
    } catch {
      return initialValue;
    }
  });

  useEffect(() => {
    try {
      localStorage.setItem(key, JSON.stringify(value));
    } catch {
      /* storage full or unavailable */
    }
  }, [key, value]);

  return [value, setValue];
}

// Usage
const [theme, setTheme] = useLocalStorage("theme", "light");
```

### useDebounce

```jsx
import { useEffect, useState } from "react";

export function useDebounce(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);

  return debounced;
}

// Usage: only search 400ms after the user stops typing
const debouncedQuery = useDebounce(query, 400);
```

### useFetch

```jsx
import { useEffect, useState } from "react";

export function useFetch(url) {
  const [state, setState] = useState({ data: null, error: null, loading: true });

  useEffect(() => {
    const controller = new AbortController();
    setState({ data: null, error: null, loading: true });

    fetch(url, { signal: controller.signal })
      .then((res) => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json();
      })
      .then((data) => setState({ data, error: null, loading: false }))
      .catch((error) => {
        if (error.name !== "AbortError") setState({ data: null, error, loading: false });
      });

    return () => controller.abort();
  }, [url]);

  return state;
}

// Usage
const { data, error, loading } = useFetch("/api/products");
```

### useOnClickOutside

```jsx
import { useEffect } from "react";

export function useOnClickOutside(ref, handler) {
  useEffect(() => {
    const listener = (event) => {
      if (!ref.current || ref.current.contains(event.target)) return;
      handler(event);
    };
    document.addEventListener("pointerdown", listener);
    return () => document.removeEventListener("pointerdown", listener);
  }, [ref, handler]);
}
```

### Other Hooks Worth Writing or Installing

`useToggle`, `useMediaQuery`, `useWindowSize`, `usePrevious`, `useInterval`, `useEventListener`, `useIntersectionObserver`. Libraries such as **usehooks-ts**, **react-use** and **ahooks** provide many of them ready-made.

---

## 17. Mini Project: Todo App

This project combines components, props, `useReducer`, `useContext`, custom hooks, lists, forms and persistence.

```
src/
├── context/TodoContext.jsx
├── components/
│   ├── TodoForm.jsx
│   ├── TodoList.jsx
│   └── TodoItem.jsx
├── hooks/useLocalStorage.js
└── App.jsx
```

**`context/TodoContext.jsx`**

```jsx
import { createContext, useContext, useEffect, useReducer } from "react";

const TodoContext = createContext(null);

function reducer(todos, action) {
  switch (action.type) {
    case "add":
      return [...todos, { id: crypto.randomUUID(), text: action.text, done: false }];
    case "toggle":
      return todos.map((t) => (t.id === action.id ? { ...t, done: !t.done } : t));
    case "delete":
      return todos.filter((t) => t.id !== action.id);
    case "clear_done":
      return todos.filter((t) => !t.done);
    default:
      throw new Error(`Unknown action ${action.type}`);
  }
}

export function TodoProvider({ children }) {
  const [todos, dispatch] = useReducer(reducer, [], () => {
    try {
      return JSON.parse(localStorage.getItem("todos") ?? "[]");
    } catch {
      return [];
    }
  });

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  return <TodoContext value={{ todos, dispatch }}>{children}</TodoContext>;
}

export function useTodos() {
  const ctx = useContext(TodoContext);
  if (!ctx) throw new Error("useTodos must be used within TodoProvider");
  return ctx;
}
```

**`components/TodoForm.jsx`**

```jsx
import { useState } from "react";
import { useTodos } from "../context/TodoContext";

export default function TodoForm() {
  const [text, setText] = useState("");
  const { dispatch } = useTodos();

  function handleSubmit(e) {
    e.preventDefault();
    const trimmed = text.trim();
    if (!trimmed) return;
    dispatch({ type: "add", text: trimmed });
    setText("");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={text} onChange={(e) => setText(e.target.value)} placeholder="What needs doing?" />
      <button type="submit">Add</button>
    </form>
  );
}
```

**`components/TodoList.jsx`**

```jsx
import { useTodos } from "../context/TodoContext";

export default function TodoList() {
  const { todos, dispatch } = useTodos();

  if (todos.length === 0) return <p>Nothing to do. Enjoy your day!</p>;

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          <label>
            <input type="checkbox" checked={todo.done} onChange={() => dispatch({ type: "toggle", id: todo.id })} />
            <span style={{ textDecoration: todo.done ? "line-through" : "none" }}>{todo.text}</span>
          </label>
          <button onClick={() => dispatch({ type: "delete", id: todo.id })} aria-label={`Delete ${todo.text}`}>
            x
          </button>
        </li>
      ))}
    </ul>
  );
}
```

**`App.jsx`**

```jsx
import { TodoProvider } from "./context/TodoContext";
import TodoForm from "./components/TodoForm";
import TodoList from "./components/TodoList";

export default function App() {
  return (
    <TodoProvider>
      <main>
        <h1>Todos</h1>
        <TodoForm />
        <TodoList />
      </main>
    </TodoProvider>
  );
}
```

**Stretch goals:** filters (all / active / done), edit-in-place, drag-and-drop ordering, a backend with TanStack Query, tests with React Testing Library.

---

## 18. Routing with React Router

React itself has no router. **React Router** is the most widely used one. Install it:

```bash
npm install react-router
```

> **Note:** In v7 and later, everything comes from the single **`react-router`** package (DOM-specific entry points live at `react-router/dom`). The old `react-router-dom` package was only a compatibility mirror and has been removed in v8. React Router v8 also requires a recent React 19 release and Vite 7+, so check the current docs when upgrading.

React Router supports three modes: **Declarative** (component-based routes, shown first), **Data** (loaders and actions), and **Framework** (a Vite plugin that adds SSR, code splitting and type-safe routes).

### Declarative Mode

```jsx
// main.jsx
import { BrowserRouter } from "react-router";

createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

```jsx
// App.jsx
import { Routes, Route, Link, NavLink, Outlet, Navigate, useParams, useNavigate, useSearchParams } from "react-router";

function Layout() {
  return (
    <>
      <nav>
        <NavLink to="/" end>Home</NavLink>
        <NavLink to="/products">Products</NavLink>
      </nav>
      <main>
        <Outlet /> {/* child routes render here */}
      </main>
    </>
  );
}

function Product() {
  const { id } = useParams();
  const navigate = useNavigate();
  const [searchParams, setSearchParams] = useSearchParams();
  const tab = searchParams.get("tab") ?? "details";

  return (
    <>
      <h1>Product {id}</h1>
      <p>Tab: {tab}</p>
      <button onClick={() => setSearchParams({ tab: "reviews" })}>Reviews</button>
      <button onClick={() => navigate(-1)}>Back</button>
    </>
  );
}

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="products" element={<ProductList />} />
        <Route path="products/:id" element={<Product />} />
        <Route path="old-path" element={<Navigate to="/products" replace />} />
        <Route path="*" element={<NotFound />} />
      </Route>
    </Routes>
  );
}
```

### Protected Routes

```jsx
function RequireAuth({ children }) {
  const { user } = useAuth();
  if (!user) return <Navigate to="/login" replace />;
  return children;
}

<Route
  path="dashboard"
  element={
    <RequireAuth>
      <Dashboard />
    </RequireAuth>
  }
/>
```

### Data Mode: Loaders and Actions

Fetch data **before** rendering the route, and handle form submissions with actions.

```jsx
import { createBrowserRouter, Form, useLoaderData, redirect } from "react-router";
import { RouterProvider } from "react-router/dom";

const router = createBrowserRouter([
  {
    path: "/",
    Component: Layout,
    children: [
      { index: true, Component: Home },
      {
        path: "products/:id",
        loader: async ({ params }) => {
          const res = await fetch(`/api/products/${params.id}`);
          if (!res.ok) throw new Response("Not found", { status: 404 });
          return res.json();
        },
        Component: ProductPage,
        ErrorBoundary: ProductError,
      },
      {
        path: "products/new",
        action: async ({ request }) => {
          const formData = await request.formData();
          const product = await createProduct(Object.fromEntries(formData));
          return redirect(`/products/${product.id}`);
        },
        Component: NewProduct,
      },
    ],
  },
]);

function ProductPage() {
  const product = useLoaderData();
  return <h1>{product.name}</h1>;
}

function NewProduct() {
  return (
    <Form method="post">
      <input name="name" required />
      <button type="submit">Create</button>
    </Form>
  );
}

createRoot(document.getElementById("root")).render(<RouterProvider router={router} />);
```

### Lazy-Loaded Routes

```jsx
{
  path: "settings",
  lazy: async () => {
    const { default: Settings } = await import("./pages/Settings");
    return { Component: Settings };
  },
}
```

### Other Routers

**TanStack Router** (fully type-safe, built-in search-param handling) is a strong alternative.

---

## 19. Data Fetching

### Levels of Data Fetching

| Approach | When to use |
| --- | --- |
| `fetch` inside `useEffect` | Learning, tiny apps |
| **TanStack Query** (or SWR) | Most client-rendered apps: caching, retries, background refetch, pagination |
| Router loaders | When you want data tied to navigation |
| Server Components / Server Actions | Framework-based apps (Next.js, React Router framework mode) |

### TanStack Query

```bash
npm install @tanstack/react-query
```

```jsx
// main.jsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

createRoot(document.getElementById("root")).render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
);
```

```jsx
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

async function fetchTodos() {
  const res = await fetch("/api/todos");
  if (!res.ok) throw new Error("Failed to load todos");
  return res.json();
}

function Todos() {
  const queryClient = useQueryClient();

  const { data, isPending, isError, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
    staleTime: 60_000, // consider data fresh for 1 minute
  });

  const addTodo = useMutation({
    mutationFn: (text) =>
      fetch("/api/todos", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ text }),
      }).then((r) => r.json()),
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["todos"] }),
  });

  if (isPending) return <p>Loading...</p>;
  if (isError) return <p>Error: {error.message}</p>;

  return (
    <>
      <ul>{data.map((t) => <li key={t.id}>{t.text}</li>)}</ul>
      <button onClick={() => addTodo.mutate("New todo")} disabled={addTodo.isPending}>
        Add
      </button>
    </>
  );
}
```

Key ideas: **query keys** identify cached data; **mutations** change server data and then **invalidate** related queries; stale data is shown instantly while fresh data loads in the background.

### Suspense and `use`

`Suspense` shows a fallback while a child waits. In React 19 the `use` hook reads a promise and suspends until it resolves.

```jsx
import { Suspense, use } from "react";

// The promise must be stable across renders (cached), not created during render
const cache = new Map();
function getUser(id) {
  if (!cache.has(id)) cache.set(id, fetch(`/api/users/${id}`).then((r) => r.json()));
  return cache.get(id);
}

function UserCard({ id }) {
  const user = use(getUser(id)); // suspends until resolved
  return <h2>{user.name}</h2>;
}

function App() {
  return (
    <ErrorBoundary fallback={<p>Failed to load.</p>}>
      <Suspense fallback={<p>Loading user...</p>}>
        <UserCard id={1} />
      </Suspense>
    </ErrorBoundary>
  );
}
```

> In practice, let a framework or a library (TanStack Query, React Router, Next.js) manage promise caching for you.

### Avoiding Request Waterfalls

Do not fetch in a parent, wait for it, then fetch in the child if the requests are independent. Start them together (`Promise.all`, parallel queries, or loaders) and prefetch data on hover or route entry.

---

## 20. React 19 Features

### Actions and `<form action>`

An **Action** is an async function used for a data mutation. React tracks its pending state, errors and optimistic updates for you. Pass a function directly to a form's `action`:

```jsx
function RenameForm({ name, onRename }) {
  async function action(formData) {
    await onRename(formData.get("name"));
  }
  return (
    <form action={action}>
      <input name="name" defaultValue={name} />
      <button>Save</button>
    </form>
  );
}
```

After a successful Action, React automatically resets uncontrolled form fields.

### useActionState

Manages the result and pending state of an Action.

```jsx
import { useActionState } from "react";

async function subscribeAction(previousState, formData) {
  const email = formData.get("email");
  const res = await fetch("/api/subscribe", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email }),
  });
  if (!res.ok) return { ok: false, message: "Something went wrong. Please try again." };
  return { ok: true, message: "Subscribed!" };
}

export default function SubscribeForm() {
  const [state, formAction, isPending] = useActionState(subscribeAction, { ok: null, message: "" });

  return (
    <form action={formAction}>
      <input type="email" name="email" required />
      <button disabled={isPending}>{isPending ? "Subscribing..." : "Subscribe"}</button>
      {state.message && <p role="status">{state.message}</p>}
    </form>
  );
}
```

### useFormStatus

Reads the status of the **parent** `<form>` from any child component (it must be rendered inside the form).

```jsx
import { useFormStatus } from "react-dom";

function SubmitButton({ children }) {
  const { pending } = useFormStatus();
  return <button type="submit" disabled={pending}>{pending ? "Working..." : children}</button>;
}
```

### useOptimistic

Show the expected result immediately while the request is in flight. If it fails, React reverts automatically.

```jsx
import { useOptimistic } from "react";

function Messages({ messages, sendMessage }) {
  const [optimisticMessages, addOptimistic] = useOptimistic(messages, (current, text) => [
    ...current,
    { id: `temp-${crypto.randomUUID()}`, text, sending: true },
  ]);

  async function formAction(formData) {
    const text = formData.get("text");
    addOptimistic(text);
    await sendMessage(text); // parent updates `messages` when done
  }

  return (
    <>
      <ul>
        {optimisticMessages.map((m) => (
          <li key={m.id} style={{ opacity: m.sending ? 0.5 : 1 }}>{m.text}</li>
        ))}
      </ul>
      <form action={formAction}>
        <input name="text" />
        <button>Send</button>
      </form>
    </>
  );
}
```

### The `use` API

```jsx
import { use } from "react";

const data = use(promise);       // read a promise (suspends)
const theme = use(ThemeContext); // read context (can be called conditionally, unlike useContext)
```

### `ref` as a Prop

Function components receive `ref` like any other prop. `forwardRef` is no longer needed in new code.

```jsx
function MyInput({ placeholder, ref }) {
  return <input placeholder={placeholder} ref={ref} />;
}
```

### Ref Cleanup Functions

```jsx
<input
  ref={(node) => {
    // node is attached
    return () => {
      // cleanup when the node is removed
    };
  }}
/>
```

### Document Metadata and Resource Hints

Render `<title>`, `<meta>` and `<link>` anywhere in a component, and React hoists them into `<head>`.

```jsx
function BlogPost({ post }) {
  return (
    <article>
      <title>{post.title}</title>
      <meta name="description" content={post.summary} />
      <link rel="canonical" href={`https://example.com/blog/${post.slug}`} />
      <h1>{post.title}</h1>
    </article>
  );
}
```

`react-dom` also exports `preload`, `preinit`, `prefetchDNS` and `preconnect` for loading resources early.

### React 19.2 Additions

**`useEffectEvent`** extracts non-reactive logic from an effect so it always sees the latest props and state without becoming a dependency.

```jsx
import { useEffect, useEffectEvent } from "react";

function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => showNotification("Connected!", theme));

  useEffect(() => {
    const connection = createConnection(roomId);
    connection.on("connected", onConnected);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // `theme` is intentionally not a dependency
}
```

**`<Activity>`** hides part of the UI while **preserving its state**, and pauses its effects and low-priority updates. It is useful for tabs, wizards and pre-rendering the next screen.

```jsx
import { Activity } from "react";

<Activity mode={tab === "inbox" ? "visible" : "hidden"}>
  <Inbox />
</Activity>
```

### React Compiler

The **React Compiler** is a build-time tool that automatically memoizes components and hooks, so you rarely need to write `useMemo`, `useCallback` or `memo` yourself.

```bash
npm install -D babel-plugin-react-compiler
```

```js
// vite.config.js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react({ babel: { plugins: ["babel-plugin-react-compiler"] } })],
});
```

Newer versions of the Vite React plugin and other toolchains also offer built-in compiler support, so check the React Compiler docs for the setup matching your tools. The compiler assumes your components follow the **Rules of React** (pure rendering, no mutating props or state). `eslint-plugin-react-hooks` includes rules that flag code the compiler cannot optimize.

### Upgrading from React 18

| Change | Action |
| --- | --- |
| `ReactDOM.render` / `hydrate` removed | Use `createRoot` / `hydrateRoot` |
| `propTypes` and `defaultProps` (function components) removed | Use default parameters and TypeScript |
| String refs, legacy Context, `findDOMNode` removed | Use callback/object refs, `createContext`, refs |
| `useRef()` requires an argument (TypeScript types) | `useRef(null)` |
| `forwardRef` | Still works, but pass `ref` as a prop in new code |

Run the official codemods (`npx codemod@latest react/19/migration-recipe`) and read the upgrade guide on react.dev.

---

## 21. State Management

### Choose the Right Tool

| Kind of state | Best tool |
| --- | --- |
| Local UI state (open/closed, input text) | `useState`, `useReducer` |
| Server data (API responses) | **TanStack Query**, SWR, router loaders |
| URL state (filters, pagination, tabs) | Router search params |
| Form state | React 19 Actions, **React Hook Form** |
| Shared, rarely changing (theme, auth user, locale) | **Context** |
| Complex shared client state | **Zustand**, **Redux Toolkit**, Jotai |

Rule of thumb: keep state **as local as possible**, and do not put server data in a global client store.

### Zustand (small, simple, hook-based)

```bash
npm install zustand
```

```jsx
import { create } from "zustand";

export const useCartStore = create((set, get) => ({
  items: [],
  addItem: (item) => set((s) => ({ items: [...s.items, item] })),
  removeItem: (id) => set((s) => ({ items: s.items.filter((i) => i.id !== id) })),
  total: () => get().items.reduce((sum, i) => sum + i.price, 0),
}));

// Select only what you need so the component re-renders only when that slice changes
function CartCount() {
  const count = useCartStore((s) => s.items.length);
  return <span>{count}</span>;
}
```

### Redux Toolkit (structured, great DevTools)

```bash
npm install @reduxjs/toolkit react-redux
```

```jsx
// store.js
import { configureStore, createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },        // Immer lets you "mutate" safely
    incrementBy: (state, action) => { state.value += action.payload; },
  },
});

export const { increment, incrementBy } = counterSlice.actions;
export const store = configureStore({ reducer: { counter: counterSlice.reducer } });
```

```jsx
// main.jsx
import { Provider } from "react-redux";
import { store } from "./store";

<Provider store={store}><App /></Provider>

// Component
import { useDispatch, useSelector } from "react-redux";

function Counter() {
  const value = useSelector((s) => s.counter.value);
  const dispatch = useDispatch();
  return <button onClick={() => dispatch(increment())}>{value}</button>;
}
```

Use **RTK Query** (part of Redux Toolkit) if you want Redux plus a data-fetching layer.

---

## 22. Advanced Patterns

### Compound Components

Components that work together and share implicit state through context.

```jsx
import { createContext, useContext, useState } from "react";

const TabsContext = createContext(null);

function Tabs({ defaultValue, children }) {
  const [value, setValue] = useState(defaultValue);
  return (
    <TabsContext value={{ value, setValue }}>
      <div className="tabs">{children}</div>
    </TabsContext>
  );
}

function TabList({ children }) {
  return <div role="tablist">{children}</div>;
}

function Tab({ value, children }) {
  const ctx = useContext(TabsContext);
  return (
    <button role="tab" aria-selected={ctx.value === value} onClick={() => ctx.setValue(value)}>
      {children}
    </button>
  );
}

function TabPanel({ value, children }) {
  const ctx = useContext(TabsContext);
  return ctx.value === value ? <div role="tabpanel">{children}</div> : null;
}

Tabs.List = TabList;
Tabs.Tab = Tab;
Tabs.Panel = TabPanel;

// Usage
<Tabs defaultValue="a">
  <Tabs.List>
    <Tabs.Tab value="a">Account</Tabs.Tab>
    <Tabs.Tab value="b">Billing</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panel value="a">Account settings</Tabs.Panel>
  <Tabs.Panel value="b">Billing settings</Tabs.Panel>
</Tabs>
```

### Render Props

Share logic by passing a function that returns JSX.

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  return (
    <div onMouseMove={(e) => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  );
}

<MouseTracker render={({ x, y }) => <p>{x}, {y}</p>} />
```

Today, a **custom hook** usually replaces a render prop.

### Higher-Order Components (HOC)

A function that takes a component and returns an enhanced component. Common in older codebases and libraries.

```jsx
function withAuth(Component) {
  return function Protected(props) {
    const { user } = useAuth();
    if (!user) return <Navigate to="/login" replace />;
    return <Component {...props} user={user} />;
  };
}

const ProtectedDashboard = withAuth(Dashboard);
```

### Headless Components and Custom Hooks

Separate **behavior** from **appearance**. Libraries like Radix UI, React Aria, Headless UI and Downshift give you accessible behavior and let you supply the styles.

```jsx
function useDisclosure(initial = false) {
  const [open, setOpen] = useState(initial);
  return {
    open,
    show: () => setOpen(true),
    hide: () => setOpen(false),
    toggle: () => setOpen((o) => !o),
  };
}
```

### Portals

Render children into a different DOM node (modals, tooltips, toasts) while keeping React's event bubbling and context.

```jsx
import { createPortal } from "react-dom";

function Modal({ open, onClose, children }) {
  if (!open) return null;
  return createPortal(
    <div className="backdrop" onClick={onClose}>
      <div className="modal" role="dialog" aria-modal="true" onClick={(e) => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.body
  );
}
```

### Error Boundaries

Catch errors thrown while rendering a subtree and show a fallback UI. Error boundaries must be **class components** (or use the `react-error-boundary` package).

```jsx
import { Component } from "react";

class ErrorBoundary extends Component {
  state = { error: null };

  static getDerivedStateFromError(error) {
    return { error };
  }

  componentDidCatch(error, info) {
    reportToMonitoringService(error, info.componentStack);
  }

  render() {
    if (this.state.error) return this.props.fallback ?? <h2>Something went wrong.</h2>;
    return this.props.children;
  }
}

<ErrorBoundary fallback={<p>Widget failed to load</p>}>
  <Widget />
</ErrorBoundary>
```

Error boundaries do **not** catch errors in event handlers, async code or server-side rendering. Use `try/catch` there.

### Inversion of Control and Slots

Let the parent decide what renders by accepting elements as props.

```jsx
function PageLayout({ header, sidebar, children }) {
  return (
    <div className="layout">
      <header>{header}</header>
      <aside>{sidebar}</aside>
      <main>{children}</main>
    </div>
  );
}
```

### Controlled and Uncontrolled Component APIs

Support both modes in reusable components: if a `value` prop is provided, the parent controls it; otherwise the component manages its own state with `defaultValue`.

```jsx
function Toggle({ checked, defaultChecked = false, onChange }) {
  const [internal, setInternal] = useState(defaultChecked);
  const isControlled = checked !== undefined;
  const value = isControlled ? checked : internal;

  function handleClick() {
    if (!isControlled) setInternal(!value);
    onChange?.(!value);
  }

  return <button role="switch" aria-checked={value} onClick={handleClick}>{value ? "On" : "Off"}</button>;
}
```

---

## 23. Performance Optimization

> **Measure first.** Use the React DevTools **Profiler** and the browser's Performance panel. Most apps do not need heavy optimization.

### 1. Avoid Unnecessary Renders

**Colocate state.** Move state down to the component that uses it, so unrelated siblings do not re-render.

```jsx
// Slow: typing re-renders ExpensiveTree
function Page() {
  const [text, setText] = useState("");
  return (
    <>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <ExpensiveTree />
    </>
  );
}

// Fast: state lives in its own component
function SearchBox() {
  const [text, setText] = useState("");
  return <input value={text} onChange={(e) => setText(e.target.value)} />;
}

function Page() {
  return (
    <>
      <SearchBox />
      <ExpensiveTree />
    </>
  );
}
```

**Pass `children`.** A component that receives elements as `children` does not re-create them when its own state changes.

### 2. Memoization

`memo`, `useMemo`, `useCallback`, or just enable the **React Compiler**.

### 3. Code Splitting and Lazy Loading

```jsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./pages/Dashboard"));
const Settings = lazy(() => import("./pages/Settings"));

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

Split by **route** first, then by heavy components (charts, editors, maps).

### 4. Virtualize Long Lists

Render only the rows visible on screen with **TanStack Virtual**, **react-window** or **react-virtuoso**. Use this for lists with hundreds or thousands of items.

### 5. Keep the UI Responsive

Use `useTransition` and `useDeferredValue` for expensive updates, debounce input, and move heavy computation to a **Web Worker**.

### 6. Reduce Bundle Size

- Analyze your bundle (`rollup-plugin-visualizer`)
- Import only what you use (`import debounce from "lodash/debounce"`, or use `lodash-es`)
- Prefer smaller libraries (for example `date-fns` instead of large date libraries)
- Lazy-load heavy dependencies with dynamic `import()`

### 7. Optimize Assets and Network

- Use modern image formats (WebP/AVIF), correct sizes and `loading="lazy"`
- Cache API data (TanStack Query) and use a CDN
- Prefetch the next route's code and data on hover or idle

### 8. Keys, Context and Store Selectors

- Use stable keys
- Split large contexts, or use a selector-based store (Zustand, Redux)
- Select only the state slice a component needs

### Tools

React DevTools Profiler, Lighthouse, Web Vitals, `why-did-you-render`, bundle visualizers, and React's Performance Tracks in the Chrome DevTools performance panel.

---

## 24. Server-Side Rendering and Server Components

### Rendering Strategies

| Strategy | What happens | Good for |
| --- | --- | --- |
| **CSR** (client-side rendering) | Browser downloads JS, then renders | Dashboards, apps behind login |
| **SSR** (server-side rendering) | Server renders HTML per request, browser **hydrates** it | SEO, fast first paint, dynamic pages |
| **SSG** (static site generation) | HTML generated at build time | Blogs, docs, marketing pages |
| **ISR** (incremental static regeneration) | Static pages refreshed in the background | Large content sites |
| **Streaming SSR** | HTML is sent in chunks as Suspense boundaries resolve | Pages with slow data |
| **RSC** (React Server Components) | Components run only on the server and send results to the client | Less JS shipped, direct data access |

### Hydration

Hydration attaches event handlers to server-rendered HTML.

```jsx
import { hydrateRoot } from "react-dom/client";

hydrateRoot(document.getElementById("root"), <App />);
```

Avoid hydration mismatches: do not render `Date.now()`, `Math.random()`, or `window`-dependent values during the first render. Read them in an effect instead.

### Server Components (RSC)

Server Components run **only on the server**. They can be `async`, read databases and secrets directly, and add **zero JavaScript** to the client bundle.

```jsx
// app/products/page.jsx (a Server Component by default in the Next.js App Router)
import LikeButton from "./LikeButton";

export default async function ProductsPage() {
  const products = await db.product.findMany(); // runs on the server only

  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>
          {p.name} <LikeButton id={p.id} />
        </li>
      ))}
    </ul>
  );
}
```

```jsx
// app/products/LikeButton.jsx: a Client Component
"use client";

import { useState } from "react";

export default function LikeButton({ id }) {
  const [liked, setLiked] = useState(false);
  return <button onClick={() => setLiked(!liked)}>{liked ? "Liked" : "Like"}</button>;
}
```

### Server Actions

Functions marked with `"use server"` run on the server and can be called from forms and event handlers.

```jsx
// actions.js
"use server";

export async function createTodo(formData) {
  const text = formData.get("text");
  await db.todo.create({ data: { text } });
}

// In a component
<form action={createTodo}>
  <input name="text" />
  <button>Add</button>
</form>
```

### Rules of the Server/Client Boundary

- Server Components **cannot** use state, effects, event handlers or browser APIs.
- `"use client"` marks the boundary. That file and everything it imports run on the client.
- Props passed from server to client components must be **serializable** (no functions, except Server Actions).
- Server Actions are **public HTTP endpoints**: always validate input and check authorization inside them.

### Frameworks

| Framework | Notes |
| --- | --- |
| **Next.js** | Most popular full-stack React framework (App Router, RSC, Server Actions) |
| **React Router (framework mode)** | Vite-based, loaders/actions, SSR and static rendering |
| **TanStack Start** | Type-safe full-stack framework built on TanStack Router |
| **Expo** | React Native framework for mobile |

The React team recommends using a framework when you need server rendering, routing and data-loading conventions. For a client-only SPA, Vite is an excellent choice.

---

## 25. React with TypeScript

Create a typed project with `npm create vite@latest my-app -- --template react-ts`. TypeScript catches prop mistakes, typos and missing cases before your code runs.

### Typing Props

```tsx
import type { ReactNode, ComponentProps } from "react";

type ButtonProps = {
  label: string;
  variant?: "primary" | "danger";
  onClick?: () => void;
  children?: ReactNode;
};

function Button({ label, variant = "primary", onClick }: ButtonProps) {
  return <button className={`btn btn-${variant}`} onClick={onClick}>{label}</button>;
}

// Extend native element props
type InputProps = ComponentProps<"input"> & { label: string };

function Field({ label, ...rest }: InputProps) {
  return (
    <label>
      {label}
      <input {...rest} />
    </label>
  );
}
```

### State, Refs and Events

```tsx
import { useRef, useState } from "react";

type User = { id: number; name: string };

function Example() {
  const [user, setUser] = useState<User | null>(null);
  const [items, setItems] = useState<string[]>([]);
  const inputRef = useRef<HTMLInputElement>(null);

  function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
    console.log(e.target.value);
  }

  function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
  }

  function handleClick(e: React.MouseEvent<HTMLButtonElement>) {
    inputRef.current?.focus();
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} onChange={handleChange} />
      <button onClick={handleClick}>Focus</button>
    </form>
  );
}
```

### Typing Reducers (Discriminated Unions)

```tsx
type Todo = { id: string; text: string; done: boolean };

type Action =
  | { type: "add"; text: string }
  | { type: "toggle"; id: string }
  | { type: "delete"; id: string };

function reducer(state: Todo[], action: Action): Todo[] {
  switch (action.type) {
    case "add":
      return [...state, { id: crypto.randomUUID(), text: action.text, done: false }];
    case "toggle":
      return state.map((t) => (t.id === action.id ? { ...t, done: !t.done } : t));
    case "delete":
      return state.filter((t) => t.id !== action.id);
  }
}
```

### Typing Context

```tsx
import { createContext, useContext } from "react";

type AuthContextValue = { user: User | null; login: (u: User) => void; logout: () => void };

const AuthContext = createContext<AuthContextValue | null>(null);

export function useAuth() {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error("useAuth must be used within AuthProvider");
  return ctx;
}
```

### Generic Components

```tsx
type ListProps<T> = {
  items: T[];
  renderItem: (item: T) => ReactNode;
  getKey: (item: T) => string | number;
};

function List<T>({ items, renderItem, getKey }: ListProps<T>) {
  return <ul>{items.map((item) => <li key={getKey(item)}>{renderItem(item)}</li>)}</ul>;
}

<List items={users} getKey={(u) => u.id} renderItem={(u) => u.name} />
```

### Tips

- Prefer `type` aliases or `interface` for props, and avoid `React.FC` (it is optional and adds little).
- Let TypeScript **infer** types where it can (`useState(0)` is a number).
- Derive types from validation schemas (`z.infer<typeof schema>`) so the schema is the single source of truth.
- Enable `"strict": true` in `tsconfig.json`.

---

## 26. Testing

### The Testing Pyramid

| Level | Tools | Purpose |
| --- | --- | --- |
| **Unit** | Vitest (or Jest) | Pure functions, reducers, utilities |
| **Component / integration** | Vitest + React Testing Library | Behavior the user sees |
| **End-to-end** | Playwright or Cypress | Full flows in a real browser |

### Setup (Vitest + React Testing Library)

```bash
npm install -D vitest jsdom @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```js
// vite.config.js
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: "./src/setupTests.js",
    globals: true,
  },
});
```

```js
// src/setupTests.js
import "@testing-library/jest-dom/vitest";
```

Add `"test": "vitest"` to your `package.json` scripts.

### Testing a Component

```jsx
// Counter.test.jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Counter from "./Counter";

test("increments the counter when the button is clicked", async () => {
  const user = userEvent.setup();
  render(<Counter />);

  expect(screen.getByText(/you clicked 0 times/i)).toBeInTheDocument();

  await user.click(screen.getByRole("button", { name: /increment/i }));

  expect(screen.getByText(/you clicked 1 times/i)).toBeInTheDocument();
});
```

### Testing a Form

```jsx
test("submits the form with entered values", async () => {
  const user = userEvent.setup();
  const onSubmit = vi.fn();
  render(<LoginForm onSubmit={onSubmit} />);

  await user.type(screen.getByLabelText(/email/i), "asha@example.com");
  await user.type(screen.getByLabelText(/password/i), "secret123");
  await user.click(screen.getByRole("button", { name: /log in/i }));

  expect(onSubmit).toHaveBeenCalledWith({ email: "asha@example.com", password: "secret123" });
});
```

### Testing a Custom Hook

```jsx
import { renderHook, act } from "@testing-library/react";

test("useDisclosure toggles open state", () => {
  const { result } = renderHook(() => useDisclosure());

  expect(result.current.open).toBe(false);
  act(() => result.current.show());
  expect(result.current.open).toBe(true);
});
```

### Mocking the Network with MSW

```js
import { http, HttpResponse } from "msw";
import { setupServer } from "msw/node";

const server = setupServer(
  http.get("/api/todos", () => HttpResponse.json([{ id: 1, text: "Write tests" }]))
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test("shows todos from the API", async () => {
  render(<TodoList />);
  expect(await screen.findByText("Write tests")).toBeInTheDocument();
});
```

### End-to-End with Playwright

```js
import { test, expect } from "@playwright/test";

test("user can add a todo", async ({ page }) => {
  await page.goto("http://localhost:5173");
  await page.getByPlaceholder("What needs doing?").fill("Learn React");
  await page.getByRole("button", { name: "Add" }).click();
  await expect(page.getByText("Learn React")).toBeVisible();
});
```

### Testing Principles

- Test **behavior, not implementation**. Do not assert on state or internal methods.
- Query like a user: `getByRole`, `getByLabelText`, `getByText`. Use `getByTestId` only as a last resort.
- Use `findBy*` (async) for things that appear later, `queryBy*` to assert absence.
- Wrap providers (router, query client, theme) in a custom `render` helper.
- Keep tests independent, fast and deterministic.

---

## 27. Accessibility

Accessible apps work for everyone, including people using keyboards, screen readers and zoom.

### Checklist

- **Use semantic HTML** (`<button>`, `<nav>`, `<main>`, `<a>`, `<label>`, headings in order) before reaching for ARIA.
- **Label every control** with `<label htmlFor>` or `aria-label`.
- **Support the keyboard.** Everything clickable must be focusable and operable with Enter/Space. Never use a `<div onClick>` as a button.
- **Manage focus.** Move focus into dialogs, return it when they close, and move it to the page heading after route changes.
- **Provide alt text** for meaningful images (`alt=""` for decorative ones).
- **Check color contrast** and never rely on color alone to convey meaning.
- **Announce dynamic changes** with `role="status"` or `aria-live="polite"` and `role="alert"` for errors.
- **Respect user preferences** such as `prefers-reduced-motion`.

```jsx
// Bad
<div onClick={save}>Save</div>

// Good
<button type="button" onClick={save}>Save</button>

// Skip link for keyboard users
<a href="#main" className="skip-link">Skip to content</a>
<main id="main" tabIndex={-1}>...</main>
```

### Tools

- `eslint-plugin-jsx-a11y` for static checks
- axe DevTools and Lighthouse for audits
- `jest-axe` / `vitest-axe` for automated tests
- Manual testing with a keyboard and a screen reader (NVDA, VoiceOver)
- Headless UI libraries (Radix UI, React Aria) for accessible dialogs, menus and comboboxes

---

## 28. Security

React escapes values in JSX by default, which prevents most XSS. Stay safe with these rules:

| Risk | Guidance |
| --- | --- |
| **XSS via `dangerouslySetInnerHTML`** | Avoid it. If you must render HTML, sanitize it with **DOMPurify** first. |
| **Malicious URLs** | Validate user-supplied URLs. Block `javascript:` in `href` and `src`. |
| **Secrets in the bundle** | Anything in client code is public. Variables prefixed with `VITE_` are exposed to the browser. Keep API secrets on the server. |
| **Token storage** | Prefer **HttpOnly, Secure, SameSite cookies** over `localStorage` for session tokens. |
| **CSRF** | Use SameSite cookies and CSRF tokens for cookie-based auth. |
| **Dependencies** | Run `npm audit`, pin versions with a lockfile, and update regularly. |
| **Server Actions / APIs** | Validate and authorize **on the server** every time. Never trust the client. |
| **Content Security Policy** | Add a strict CSP header to limit what scripts can run. |
| **Dynamic component types** | Never render a component chosen directly from user input. |

```jsx
import DOMPurify from "dompurify";

function Article({ html }) {
  return <div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(html) }} />;
}
```

---

## 29. Build and Deployment

### Production Build

```bash
npm run build     # outputs static files to dist/
npm run preview   # test the build locally
```

### Environment Variables (Vite)

```bash
# .env
VITE_API_URL=https://api.example.com
```

```js
const apiUrl = import.meta.env.VITE_API_URL;
```

Only variables starting with `VITE_` are exposed to client code. Do not commit `.env` files containing secrets.

### Where to Deploy

| Platform | Notes |
| --- | --- |
| **Vercel / Netlify / Cloudflare Pages** | Connect your Git repo; build command `npm run build`, output directory `dist` |
| **GitHub Pages** | Set `base: "/your-repo-name/"` in `vite.config.js` |
| **AWS S3 + CloudFront, Firebase Hosting** | Static hosting with a CDN |
| **Docker + Nginx** | Self-hosting |

### Client-Side Routing Needs a Fallback

For a single-page app, configure the host to serve `index.html` for unknown paths so deep links like `/products/42` work.

```nginx
# nginx.conf
server {
  listen 80;
  root /usr/share/nginx/html;
  index index.html;

  location / {
    try_files $uri /index.html;
  }
}
```

### Docker (multi-stage)

```dockerfile
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

### CI Example (GitHub Actions)

```yaml
name: CI
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm test -- --run
      - run: npm run build
```

(Use the current major versions of these actions.)

### Production Checklist

- [ ] Error monitoring (Sentry or similar) and an error boundary at the app root
- [ ] Analytics and Web Vitals tracking
- [ ] Code splitting and compressed assets (gzip/brotli)
- [ ] Caching headers for hashed assets
- [ ] Meta tags, Open Graph tags and a sitemap (use SSR/SSG if SEO matters)
- [ ] Accessibility audit and Lighthouse run
- [ ] Environment variables configured per environment

---

## 30. Tooling

| Tool | Purpose |
| --- | --- |
| **Vite** | Dev server and bundler |
| **ESLint** + `eslint-plugin-react-hooks` | Catch bugs, enforce the Rules of Hooks and Rules of React |
| `eslint-plugin-jsx-a11y` | Accessibility linting |
| **Prettier** (or Biome) | Consistent formatting |
| **TypeScript** | Static types |
| **Vitest** + **React Testing Library** | Tests |
| **Playwright** | End-to-end tests |
| **Storybook** | Develop and document components in isolation |
| **React DevTools** | Inspect components, props, state and profile renders |
| **Husky** + **lint-staged** | Run checks on commit |
| **TanStack Query DevTools / Redux DevTools** | Inspect cached data and store actions |

Install the **React Developer Tools** browser extension. The *Components* tab shows the live tree, and the *Profiler* tab records which components rendered and why.

---

## 31. Best Practices and Common Mistakes

### Best Practices

1. **Keep components small and focused.** One component, one responsibility.
2. **Name things clearly.** Components are `PascalCase`, hooks start with `use`, event handlers are `handleX`, and callback props are `onX`.
3. **Keep state minimal and local.** Derive everything you can.
4. **Never mutate state or props.** Return new objects and arrays.
5. **Lift state up only as far as needed.**
6. **Use stable, unique keys** for lists.
7. **Handle loading, error and empty states** for every async view.
8. **Clean up effects** (timers, listeners, subscriptions, requests).
9. **Prefer events over effects** for responding to user actions.
10. **Colocate** styles, tests and helpers with the component that uses them.
11. **Write accessible markup first.**
12. **Use TypeScript** on anything that will live longer than a prototype.
13. **Measure before you optimize.**
14. **Test behavior** from the user's perspective.
15. **Add an error boundary** around risky or independent parts of the UI.

### Common Mistakes

| Mistake | Fix |
| --- | --- |
| Mutating state (`arr.push(x); setArr(arr)`) | `setArr([...arr, x])` |
| Calling the handler during render (`onClick={fn()}`) | `onClick={fn}` or `onClick={() => fn(arg)}` |
| Missing or wrong effect dependencies | Follow the lint rule and restructure the effect |
| Missing effect cleanup, causing leaks and race conditions | Return a cleanup function, use `AbortController` |
| Storing derived data in state | Compute it during render |
| Using array index as a key in dynamic lists | Use a stable ID |
| Using `useEffect` to sync state with other state | Compute during render or handle it in the event |
| Reading state right after setting it | Use the updater form or compute the next value first |
| Defining components inside other components | Move them to module scope (otherwise they remount every render) |
| Putting everything in one giant context | Split contexts or use a selector-based store |
| `{count && <X />}` rendering `0` | `{count > 0 && <X />}` |
| Fetching the same data in many components | Use a shared cache (TanStack Query) |
| Over-memoizing everything | Profile first, or enable the React Compiler |

### Recommended Ecosystem

| Need | Popular choices |
| --- | --- |
| Routing | React Router, TanStack Router |
| Server state | TanStack Query, SWR, RTK Query |
| Client state | Zustand, Redux Toolkit, Jotai |
| Forms | React Hook Form, TanStack Form, React 19 Actions |
| Validation | Zod, Valibot |
| UI components | shadcn/ui, Radix UI, MUI, Mantine, Chakra UI |
| Animation | Motion (Framer Motion), React Spring, CSS transitions |
| Tables | TanStack Table |
| Charts | Recharts, Visx, Chart.js |
| Dates | date-fns, Day.js |
| Drag and drop | dnd-kit |
| i18n | react-i18next, FormatJS (react-intl) |
| Full-stack frameworks | Next.js, React Router (framework mode), TanStack Start |

### Forms with React Hook Form and Zod

```bash
npm install react-hook-form zod @hookform/resolvers
```

```jsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  email: z.string().email("Enter a valid email"),
  password: z.string().min(8, "Use at least 8 characters"),
});

export default function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm({ resolver: zodResolver(schema) });

  async function onSubmit(values) {
    await login(values);
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      <label htmlFor="email">Email</label>
      <input id="email" type="email" {...register("email")} aria-invalid={!!errors.email} />
      {errors.email && <p role="alert">{errors.email.message}</p>}

      <label htmlFor="password">Password</label>
      <input id="password" type="password" {...register("password")} aria-invalid={!!errors.password} />
      {errors.password && <p role="alert">{errors.password.message}</p>}

      <button disabled={isSubmitting}>{isSubmitting ? "Logging in..." : "Log in"}</button>
    </form>
  );
}
```

---

## 32. Learning Roadmap

### Stage 1: Beginner

- [ ] JavaScript fundamentals (ES6+, array methods, async/await)
- [ ] Vite setup, JSX, components, props, `children`
- [ ] Lists and keys, conditional rendering
- [ ] Events and `useState`
- [ ] Controlled forms
- [ ] `useEffect` basics and cleanup
- [ ] Styling (CSS Modules or Tailwind)

**Projects:** counter, todo list, tip calculator, quiz app, weather app.

### Stage 2: Intermediate

- [ ] `useRef`, `useContext`, `useReducer`
- [ ] Custom hooks
- [ ] React Router (nested routes, params, loaders)
- [ ] Data fetching with TanStack Query
- [ ] Form libraries and schema validation
- [ ] TypeScript with React
- [ ] Testing with Vitest and React Testing Library
- [ ] Authentication flow (protected routes)

**Projects:** movie browser, expense tracker, shopping cart, blog with routing and comments.

### Stage 3: Advanced

- [ ] Rendering performance: Profiler, memoization, React Compiler, virtualization
- [ ] Concurrent features: `useTransition`, `useDeferredValue`, Suspense
- [ ] React 19 Actions, `useActionState`, `useOptimistic`, `use`
- [ ] Design patterns: compound components, headless components, portals, error boundaries
- [ ] SSR, streaming, hydration, Server Components, Server Actions
- [ ] State architecture (server state vs client state vs URL state)
- [ ] Accessibility and internationalization
- [ ] Monorepos, design systems and Storybook
- [ ] End-to-end testing, CI/CD, monitoring

**Projects:** Kanban board with drag and drop and optimistic updates, real-time chat (WebSockets), analytics dashboard with charts and virtualized tables, full-stack app with authentication and a database.

### Beyond Web

- **React Native / Expo** for iOS and Android
- **Electron** or **Tauri** for desktop apps

---

## 33. Resources

**Official**

- [react.dev](https://react.dev): official docs and interactive tutorial
- [React Learn](https://react.dev/learn): the best first stop for fundamentals
- [React API Reference](https://react.dev/reference/react)
- [React Blog](https://react.dev/blog): release notes and upgrade guides

**Ecosystem Docs**

- [Vite](https://vite.dev)
- [React Router](https://reactrouter.com)
- [TanStack (Query, Router, Table, Virtual)](https://tanstack.com)
- [Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app)

**Articles and Patterns**

- [patterns.dev](https://www.patterns.dev)
- [Overreacted](https://overreacted.io) by Dan Abramov
- [Kent C. Dodds' blog](https://kentcdodds.com/blog)
- [web.dev](https://web.dev) for performance and accessibility guidance

---

## Contributing

Found a mistake or want to add a topic? Open an issue or a pull request. Please keep examples small, runnable and aligned with current React best practices.

## License

This guide is released under the [MIT License](https://opensource.org/licenses/MIT).

---

<p align="center">Happy coding, and enjoy building with React!</p>
