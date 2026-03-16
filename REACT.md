# Advanced React Interview Questions — Senior Developer

---

## 1. React Core Concepts & Architecture

- What is React? How does it differ from a framework like Angular?
- What is the Virtual DOM? How does it work?
- What is the difference between the Virtual DOM and the Real DOM?
- What is the Reconciliation algorithm? How does React decide what to update?
- What is the Fiber architecture? How does it differ from the old Stack reconciler?
- What is a React element vs a React component?
- What is JSX? What does it compile to? (`React.createElement` or the new JSX transform)
- What is the new JSX transform (React 17+)? Why was it introduced?
- What is `React.createElement(type, props, ...children)`?
- What is a ReactDOM? What is `ReactDOM.render()` vs `ReactDOM.createRoot()` (React 18)?
- What is `React.StrictMode`? What double-invocations does it perform and why?
- What is Concurrent Mode (React 18)? What problem does it solve?
- What is time-slicing in React?
- What is `startTransition`? What is `useTransition`?
- What is the difference between urgent and non-urgent updates in React 18?
- What is `ReactDOM.hydrateRoot()`?
- What is `React.Fragment`? What is `<>...</>` shorthand?
- What is `React.cloneElement()`?
- What is `React.Children`? What utilities does it provide?
- What is `React.isValidElement()`?

> **Best Practices:**
> - Use `ReactDOM.createRoot()` (React 18) — `ReactDOM.render()` is deprecated.
> - Use `React.StrictMode` in development to surface side-effects and deprecated API usage.
> - Understand Fiber priority levels — UI interactions are urgent; data fetching results are transitions.
> - Use `startTransition` to mark non-urgent state updates (search results, tab switching) to keep UI responsive.
> - Prefer the new JSX transform — no need to import React in every file.

---

## 2. Components — Class vs Functional

- What is a class component? What is the lifecycle of a class component?
- What is a functional component?
- What are the differences between class and functional components in React 18?
- What is `PureComponent`? How does it differ from `Component`?
- What is `shouldComponentUpdate()`? When is it called?
- What are class lifecycle methods: `constructor`, `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`, `getDerivedStateFromProps`, `getSnapshotBeforeUpdate`, `componentDidCatch`?
- What is `getDerivedStateFromProps`? Why was it introduced?
- What is `getSnapshotBeforeUpdate`? Give a use case (scroll position preservation).
- What is `componentDidCatch` vs `getDerivedStateFromError`?
- What is an Error Boundary? Can a functional component be an Error Boundary? (No — only class components)
- What is the equivalent of `componentDidMount` in hooks?
- What is the equivalent of `componentWillUnmount` in hooks?
- What is the equivalent of `componentDidUpdate` in hooks?
- Why were class components problematic? (Confusing `this`, lifecycle logic scattered, hard to share stateful logic)

> **Best Practices:**
> - Write all new components as functional components with hooks.
> - Keep class components only where Error Boundaries are needed (until `use` + error boundaries in future React).
> - Do not use deprecated lifecycle methods (`componentWillMount`, `componentWillReceiveProps`, `componentWillUpdate`).
> - Use `getDerivedStateFromProps` only as a last resort — usually a sign of poor data flow design.

---

## 3. Props & State

- What are props? Are they mutable?
- What is prop drilling? What problems does it cause?
- What is the difference between props and state?
- What is `defaultProps`? What is the modern alternative? (Default parameter values)
- What is `propTypes`? What is the modern alternative? (TypeScript)
- What is state? How is it different from a regular variable?
- What is `useState`? What does it return?
- Is `setState` / `useState` setter synchronous or asynchronous?
- What is state batching? What is automatic batching in React 18?
- What was the batching behavior before React 18? (Only batched inside React event handlers)
- What is the functional form of `setState`? (`setState(prev => prev + 1)`) When is it required?
- What is the difference between `useState` and `useReducer`?
- What is state lifting? When should you lift state?
- What is derived state? Why should you avoid storing it in state?
- What is the stale closure problem with state?
- Can you mutate state directly? What happens if you do? (`Object.assign(state, ...)` — won't trigger re-render)
- What is immutable state updates? Why are they required?
- What is the difference between shallow and deep clone for state updates?
- How do you update nested state immutably?

> **Best Practices:**
> - Never mutate state directly — always return a new reference.
> - Use the functional updater form `setState(prev => ...)` when the new state depends on the previous.
> - Keep state as minimal and flat as possible — derive values rather than duplicating them in state.
> - Lift state to the nearest common ancestor — no higher than necessary.
> - Use `useReducer` when state logic is complex or involves multiple sub-values.

---

## 4. Hooks — Core

### useState
- What is `useState`? What triggers a re-render?
- What is lazy initialization of `useState`? (`useState(() => expensiveComputation())`)
- Why does React bail out of re-render when state is the same object reference? (Object.is comparison)
- What is the `useState` update batching behavior?

### useEffect
- What is `useEffect`? What does it replace?
- What is the dependency array? What happens with `[]` vs no array vs `[dep]`?
- What is the cleanup function in `useEffect`? When is it called?
- What is the difference between `useEffect` and `useLayoutEffect`?
- What is `useLayoutEffect`? When should you use it? (Synchronous DOM mutations, measuring layout)
- What is the React 18 `useEffect` double-invocation in Strict Mode? Why?
- What are common `useEffect` pitfalls: missing dependencies, infinite loops, stale closures?
- What is the exhaustive-deps ESLint rule?
- What is `useEffect` race condition with async functions? How do you fix it? (cleanup cancel flag or AbortController)

### useRef
- What is `useRef`? How does it differ from `useState`?
- Does changing a `ref.current` trigger a re-render? (No)
- What are the two uses of `useRef`? (DOM reference, persisting mutable value across renders)
- What is `forwardRef`? When do you need it?
- What is `useImperativeHandle`? When should you use it?

### useContext
- What is `useContext`? How does it work?
- What is `createContext()`? What is the default value?
- What triggers a re-render when using `useContext`? (Any change to the context value)
- What is the performance issue with context? How do you optimize it?
- What is context value referential stability? Why does `value={{ ... }}` in JSX cause re-renders?

### useReducer
- What is `useReducer(reducer, initialState, init)`?
- When should you prefer `useReducer` over `useState`?
- What is the `dispatch` function? Is it stable across renders? (Yes — stable reference)
- How do you handle async actions with `useReducer`?
- What is the difference between Redux and `useReducer`?

### useMemo & useCallback
- What is `useMemo`? What does it memoize?
- What is `useCallback`? What does it memoize?
- What is the difference between `useMemo` and `useCallback`?
- When should you use `useMemo`? (Expensive computation, referential stability for objects passed to memoized children)
- When should you use `useCallback`? (Stable function reference for memoized children or useEffect deps)
- Is memoization always beneficial? What is the overhead?
- What is the cost of `useMemo` / `useCallback`? (Memory + comparison on every render)

> **Best Practices:**
> - Specify all dependencies in `useEffect` — use ESLint's `exhaustive-deps` rule.
> - Use the `AbortController` pattern to cancel async operations in `useEffect` cleanup.
> - Use `useLayoutEffect` only for DOM measurements — `useEffect` for everything else.
> - Don't overuse `useMemo`/`useCallback` — profile first, memoize second.
> - Pass stable values to context — memoize the context value object with `useMemo`.
> - Use `useRef` for values that don't need to trigger re-renders (timers, previous values, DOM nodes).

---

## 5. Hooks — Advanced & Custom

- What is a custom hook? What naming convention does it follow? (`use` prefix)
- What makes a custom hook different from a utility function?
- Can custom hooks call other hooks? (Yes — they follow the Rules of Hooks)
- What are the Rules of Hooks?
  - Only call hooks at the top level — not inside loops, conditions, or nested functions
  - Only call hooks from React function components or custom hooks
- What is `useId` (React 18)? What problem does it solve? (Stable unique IDs — SSR hydration)
- What is `useDeferredValue` (React 18)? How does it differ from `useTransition`?
- What is `useSyncExternalStore` (React 18)? When is it used? (Subscribing to external stores with tearing prevention)
- What is `useInsertionEffect` (React 18)? When is it used? (CSS-in-JS libraries — fires before DOM mutations)
- What is `use` hook (React 19 / experimental)? (Reads resources — promises, context — inside render)
- What is `useDebugValue`? When should you use it?
- What is `useOptimistic` (React 19)?

> **Best Practices:**
> - Extract reusable stateful logic into custom hooks — keep components thin.
> - Name custom hooks with `use` prefix — enables ESLint rules of hooks linting.
> - Return an array from custom hooks when the caller names the values (like `useState`); return an object for hooks with named values.
> - Use `useSyncExternalStore` for subscribing to any external store — prevents tearing in Concurrent Mode.

---

## 6. React Performance Optimization

- What is `React.memo`? What does it do? How does it compare props? (Shallow equality)
- What is the difference between `React.memo` and `PureComponent`?
- What is prop referential equality? Why does passing inline objects/functions break memoization?
- What is reconciliation cost? How does key help?
- What is the `key` prop? Why should you never use array index as key for dynamic lists?
- What is `key` as a reset mechanism? (Changing key forces full remount)
- What is the React DevTools Profiler? What does it measure?
- What is `why-did-you-render` library?
- What is windowing / virtualization? (`react-window`, `react-virtualized`, `@tanstack/virtual`)
- What is code splitting? What is `React.lazy()` and `Suspense`?
- What is dynamic `import()`? How does webpack handle it?
- What is `React.Suspense`? What is a Suspense boundary?
- What is a loading state with Suspense? What is a fallback?
- What is the render-as-you-fetch pattern?
- What is concurrent rendering benefit for perceived performance?
- What are expensive renders? How do you identify and fix them?
- What is the flamegraph in React DevTools Profiler?
- What is the interaction tracing API?
- What is `scheduler` package in React? What are priority levels?
- What is `IdleCallback` scheduling?
- How does CSS-in-JS affect render performance?

> **Best Practices:**
> - Memoize components with `React.memo` only when profiling confirms unnecessary re-renders.
> - Stabilize function and object props with `useCallback`/`useMemo` before passing to memoized children.
> - Virtualize long lists — rendering 10,000 DOM nodes is always slow.
> - Use `React.lazy` + `Suspense` for route-level code splitting — reduces initial bundle.
> - Never use array index as `key` for lists that can reorder or change — use stable unique IDs.
> - Profile with React DevTools before optimizing — don't guess.

---

## 7. React Patterns

- What is the Container/Presentational (Smart/Dumb) pattern?
- What is the Compound Component pattern? Give an example (`<Select>/<Option>`).
- What is the Render Props pattern? What problems does it solve?
- What is the Higher-Order Component (HOC) pattern? What are its downsides?
- What replaced HOCs and Render Props? (Hooks)
- What is the Provider pattern?
- What is the Custom Hook pattern?
- What is the Controlled vs Uncontrolled component pattern?
- What is the Headless Component pattern? (`react-table`, `downshift`, `@tanstack/react-table`)
- What is the Context + Reducer pattern (Redux-like)?
- What is prop spreading? When is it useful vs harmful?
- What is the children-as-function pattern?
- What is the State Initializer pattern?
- What is the Controlled Props pattern (allow external control of internal state)?
- What is the Slots pattern in React? (Named children)
- What is component composition vs inheritance? Why does React prefer composition?

> **Best Practices:**
> - Prefer hooks over HOCs and Render Props — same power, no wrapper hell.
> - Use Compound Components for closely related UI (Tabs, Accordion, Select) — better API than `isOpen` prop drilling.
> - Keep Presentational components side-effect-free — easier to test and reuse.
> - Use Controlled components for form fields — gives full control of form state.

---

## 8. State Management

- What is local state vs global state?
- When should you use global state?
- What is Context API? When is it sufficient for state management?
- What is the problem with overusing Context? (Performance — all consumers re-render on any change)
- What is Zustand? How does it differ from Redux?
- What is Jotai? What is atomic state?
- What is Recoil? What is an atom and a selector?
- What is Redux? What are actions, reducers, store, dispatch?
- What is Redux Toolkit (RTK)? What does it simplify?
- What is RTK Query? How does it differ from `redux-thunk`?
- What is the Redux data flow (one-way)?
- What is a Redux middleware? How does `redux-thunk` work?
- What is `redux-saga`? How does it differ from `redux-thunk`?
- What is `immer`? How does it enable mutative syntax for immutable updates?
- What is MobX? How does it use observables?
- What is XState? What is a state machine?
- What is `react-query` / TanStack Query? What does it manage?
- What is the difference between server state and client state?
- What is optimistic update? How is it implemented?

> **Best Practices:**
> - Use local state first — don't reach for global state prematurely.
> - Use TanStack Query / SWR for server state — they handle caching, deduplication, and background refresh.
> - Use Zustand or Jotai for lightweight global UI state — simpler than Redux for many apps.
> - Use Redux Toolkit only when you need the Redux ecosystem (DevTools, middleware, serialization).
> - Separate server state (TanStack Query) from client/UI state (Zustand) — they have different lifecycles.

---

## 9. React Router (v6)

- What is React Router? What problem does it solve?
- What is `BrowserRouter` vs `HashRouter` vs `MemoryRouter`?
- What is `Routes` and `Route` in React Router v6?
- What is `<Outlet />`? What is nested routing?
- What is `useNavigate()`? How does it replace `useHistory()`?
- What is `useParams()`?
- What is `useLocation()`? What is `location.state`?
- What is `useSearchParams()`?
- What is `Link` vs `NavLink`?
- What is `Navigate` component?
- What is index route?
- What is route-level code splitting with `React.lazy`?
- What is `loader` and `action` in React Router v6.4+ (Data Router)?
- What is `useLoaderData()`?
- What is `useActionData()`?
- What is `useFetcher()`?
- What is the difference between declarative routing (JSX) and data routing?
- What is `createBrowserRouter()` vs `<BrowserRouter>`?
- How does React Router v6 handle 404 routes? (`path="*"`)
- What is route guards / protected routes? How do you implement them?

> **Best Practices:**
> - Use `<Routes>` not `<Switch>` (React Router v6 — `Switch` removed).
> - Use nested routes with `<Outlet>` for layout components.
> - Use `useNavigate` with `replace: true` after form submissions to prevent back-button resubmission.
> - Implement protected routes with a wrapper component that redirects unauthenticated users.
> - Use Data Router (`createBrowserRouter`) for new apps — enables route-level data loading.

---

## 10. Forms in React

- What is a controlled component for forms?
- What is an uncontrolled component? When would you use one?
- What is `ref` for form input access?
- What is `defaultValue` vs `value`?
- What is form validation in React? How do you implement it?
- What is `React Hook Form`? Why is it performant? (Uncontrolled + schema validation, minimal re-renders)
- What is `Formik`? How does it compare to React Hook Form?
- What is `zod` schema validation? How does it integrate with React Hook Form?
- What is `yup` schema validation?
- What is `register`, `handleSubmit`, `formState` in React Hook Form?
- What is `Controller` in React Hook Form?
- What is field array (`useFieldArray`) in React Hook Form?
- What is form submission handling? How do you prevent default?
- What is optimistic form submission?
- What is the `useFormStatus` hook (React 19)?
- What is the `useFormState` hook (React 19)?
- What is `<form action={serverAction}>` in React 19?

> **Best Practices:**
> - Use React Hook Form for complex forms — it avoids re-rendering the entire form on each keystroke.
> - Validate with `zod` schema — colocates type definition and validation.
> - Use `controlled` components for simple forms and `uncontrolled` for performance-critical ones.
> - Never perform side effects on form change — use `onSubmit` handlers.
> - Show inline validation errors — don't wait for submission to surface them.

---

## 11. React with TypeScript

- How do you type a functional component? (`React.FC<Props>` vs direct typing)
- What is the difference between `React.FC<Props>` and `(props: Props) => JSX.Element`?
- Why do many developers avoid `React.FC`? (implicit `children`, `displayName` issues in older versions)
- How do you type `children`? (`React.ReactNode` vs `React.ReactElement` vs `React.PropsWithChildren`)
- How do you type `useState`? (`useState<Type>(initial)`)
- How do you type `useRef`? (`useRef<HTMLInputElement>(null)`)
- How do you type `useContext`?
- How do you type `useReducer`?
- How do you type event handlers? (`React.ChangeEvent<HTMLInputElement>`, `React.MouseEvent<HTMLButtonElement>`)
- How do you type `forwardRef`? (`React.forwardRef<HTMLInputElement, Props>`)
- How do you type a custom hook?
- What is `React.ComponentProps<typeof Component>`?
- What is `React.ComponentPropsWithoutRef<'button'>`?
- What is `React.HTMLAttributes<HTMLDivElement>`?
- What is discriminated union for component variants?
- What is `as` prop pattern? How do you type a polymorphic component?
- What is `satisfies` operator (TypeScript 4.9+)?
- What is `Awaited<T>` in TypeScript?

> **Best Practices:**
> - Avoid `React.FC` — directly type props parameter and return type for clarity.
> - Use `React.ReactNode` for `children` — it accepts everything renderable.
> - Use `React.ComponentPropsWithoutRef` to extend native element props without inheriting `ref`.
> - Type event handlers explicitly — `React.ChangeEvent<HTMLInputElement>` not `any`.
> - Use discriminated unions for components with mutually exclusive prop sets.

---

## 12. React Testing

- What is `React Testing Library (RTL)`? What philosophy does it follow?
- What is the RTL guiding principle? ("Test the way users interact with your app")
- What is `render()` in RTL? What does it return?
- What are RTL queries: `getBy`, `queryBy`, `findBy`, `getAllBy`, `queryAllBy`, `findAllBy`?
- What is the difference between `getBy`, `queryBy`, and `findBy`?
- What are query priority: `ByRole` > `ByLabelText` > `ByPlaceholderText` > `ByText` > `ByTestId`?
- What is `screen`? Why is it preferred over destructuring `render`?
- What is `userEvent` vs `fireEvent`?
- What is `waitFor()` and `findBy*`?
- What is `act()`? When do you need it explicitly?
- What is `renderHook()` for testing custom hooks?
- What is `msw` (Mock Service Worker)? How does it differ from mocking `fetch`?
- What is snapshot testing? What are its drawbacks?
- What is `jest.mock()`? When should you mock?
- What is `@testing-library/jest-dom`? What custom matchers does it add?
- What is `vitest`? How does it compare to `jest`?
- What is Storybook? How does it complement unit tests?
- What is Playwright or Cypress for E2E tests?
- What is the testing pyramid / testing trophy?

> **Best Practices:**
> - Query by accessible roles (`ByRole`) — ensures your UI is accessible too.
> - Use `userEvent` over `fireEvent` — simulates real browser events (focus, typing, etc.).
> - Use `msw` to mock API calls — avoids brittle `fetch` mocks and tests real request-response cycle.
> - Avoid snapshot tests for logic-heavy components — they become stale and ignored.
> - Test behavior, not implementation — don't assert on internal state or class names.

---

## 13. Server-Side Rendering (SSR) & Static Generation

- What is SSR (Server-Side Rendering)? What problems does it solve?
- What is CSR (Client-Side Rendering)? What are its drawbacks?
- What is SSG (Static Site Generation)?
- What is ISR (Incremental Static Regeneration)? (Next.js feature)
- What is hydration? What is hydration mismatch?
- What is `renderToString()` vs `renderToPipeableStream()` (React 18)?
- What is streaming SSR? What are its benefits?
- What is `Suspense` in SSR streaming? (Stream HTML as each Suspense boundary resolves)
- What is selective hydration (React 18)?
- What is `ReactDOM.hydrateRoot()`?
- What is Next.js? What rendering modes does it support?
- What is the Next.js App Router (Next.js 13+)? How does it differ from Pages Router?
- What are React Server Components (RSC)?
- What is the difference between a Server Component and a Client Component?
- What are the constraints of Server Components? (No hooks, no event handlers, no browser APIs)
- What is the `"use client"` directive?
- What is the `"use server"` directive (Server Actions)?
- What is the component tree serialization in RSC?
- What is the RSC payload?
- What is Next.js `getServerSideProps`, `getStaticProps`, `getStaticPaths`? (Pages Router)
- What is `fetch` caching in Next.js App Router?

> **Best Practices:**
> - Use streaming SSR (`renderToPipeableStream`) — faster TTFB than `renderToString`.
> - Always match server and client render output — hydration mismatches cause subtle bugs.
> - Use React Server Components for data-fetching components — zero client JS bundle cost.
> - Move `"use client"` boundary as far down the tree as possible — maximize Server Components.
> - Avoid putting secrets in Client Components — they are bundled into the browser JS.

---

## 14. React 18 Concurrent Features

- What is Concurrent React? What does it enable?
- What is `startTransition`? What updates should be wrapped in it?
- What is `useTransition`? What does `isPending` indicate?
- What is `useDeferredValue`? How does it differ from `useTransition`?
- When would you use `useDeferredValue` vs `useTransition`?
- What is the Concurrent Mode rendering model? (Render can be interrupted, paused, resumed)
- What is automatic batching in React 18? What did React 17 batch?
- What is `flushSync`? When is it needed?
- What is `startTransition` vs `setTimeout` for deferring work?
- What is `useId`? Why does it exist for SSR?
- What is Suspense in Concurrent Mode vs React 16/17 Suspense?
- What is the `use` hook (React 19)?
- What are React 19 new features: `use`, `useOptimistic`, `useFormStatus`, `useFormState`, Server Actions?

> **Best Practices:**
> - Wrap non-urgent state updates in `startTransition` — keeps interactions responsive.
> - Use `useDeferredValue` when you don't control the state setter (e.g., prop from parent).
> - Do not use `flushSync` routinely — it opts out of concurrent rendering benefits.
> - Do not `use(promise)` without a Suspense boundary above it.

---

## 15. React Context — Advanced

- What is `createContext(defaultValue)` default value? When is it used? (Only when no Provider is above in the tree)
- What is context value referential stability?
- Why does `<Context.Provider value={{ count, setCount }}>` cause all consumers to re-render? (New object on each render)
- How do you split context to avoid unnecessary re-renders?
- What is the context selector pattern?
- Can you have multiple providers of the same context? (Yes — nested providers override outer ones)
- What is the performance difference between Context and Zustand/Jotai?
- What is the pattern of separating state context and dispatch context?
- What is `React.createContext` with a `useContext` wrapper hook pattern?
- What is Context composition for themes, auth, locale?
- Can context replace Redux? When should you use each?

> **Best Practices:**
> - Split context into state + dispatch — dispatch never changes, so only state consumers re-render.
> - Memoize the context value with `useMemo` to prevent referential instability.
> - Use Zustand or Jotai instead of Context for frequently-updated global state.
> - Wrap `useContext` in a custom hook — enforces usage within Provider and provides better error messages.

---

## 16. Refs & the DOM

- What is `useRef`? What are its two purposes?
- What is `createRef()`? How does it differ from `useRef()`?
- What is `ref.current`? When is it populated? (After `componentDidMount` / after first render)
- What is `ref` callback? When is it useful?
- What is `forwardRef`? Why is it needed?
- What is `useImperativeHandle`? What does it expose?
- When should you use `useImperativeHandle`? (Focus management, scroll, animations — last resort)
- What is the difference between DOM refs and component instance refs?
- What is `flushSync` and why might it be needed with refs?
- What is `ref` in React 19? (Can be passed as a prop — `forwardRef` no longer needed)
- What are common use cases for refs? (focus, scroll, media playback, integrating third-party DOM libs)
- Should you read or write DOM via refs during render? (No — side effects only in effects/handlers)

> **Best Practices:**
> - Avoid refs for things that can be done with state — refs escape React's data flow.
> - Use `useImperativeHandle` sparingly — prefer declarative props for component control.
> - Always access `ref.current` inside `useEffect` or event handlers, not during render.

---

## 17. React Suspense & Error Boundaries

- What is Suspense? What does it do when a child suspends?
- What is a Suspense boundary? What is `fallback`?
- What can trigger Suspense? (lazy components, `use(promise)`, data-fetching libraries that integrate with Suspense)
- What is `React.lazy()`? How does it work with dynamic `import()`?
- Can you nest Suspense boundaries? How does React choose which boundary to show?
- What is a Suspense waterfall? How do you avoid it?
- What is an Error Boundary? How do you implement one?
- What is `getDerivedStateFromError`? What is `componentDidCatch`?
- What does `componentDidCatch` receive? (`error`, `info.componentStack`)
- What is `react-error-boundary` library?
- Can you reset an Error Boundary? (`resetKeys`, `onReset` in `react-error-boundary`)
- What is the difference between Suspense fallback and Error Boundary fallback?
- What is Suspense in SSR streaming? (Server streams HTML progressively)
- What is `SuspenseList` (experimental)?

> **Best Practices:**
> - Place Suspense boundaries close to the component that may suspend — granular UX.
> - Place Error Boundaries at route level minimum — prevents full app crash.
> - Use `react-error-boundary` instead of class components for error handling.
> - Use multiple Suspense boundaries to prevent one slow resource from blocking fast ones.

---

## 18. React and Accessibility (a11y)

- What is WCAG? What are the A, AA, AAA levels?
- What is ARIA? What is `role`, `aria-label`, `aria-labelledby`, `aria-describedby`?
- What is `aria-live`? When would you use it in React?
- What is focus management in React? Why is it important for modals and routing?
- What is `useRef` + `.focus()` for focus management?
- What is `tabIndex`? What is `tabIndex={-1}`?
- What is a focus trap? When is it needed? (Modal dialogs)
- What is the `@testing-library/react` role-based querying connection to a11y?
- What is `axe-core`? What is `@axe-core/react`?
- What are semantic HTML elements and why do they improve a11y? (`<button>` vs `<div onClick>`)
- What is keyboard navigation? What must interactive elements support?
- What is `aria-disabled` vs `disabled`? What is the difference?
- What is `aria-expanded`, `aria-selected`, `aria-checked`?
- What is color contrast ratio? Why is it important?

> **Best Practices:**
> - Use semantic HTML elements over `div`s with ARIA — less code, better default behavior.
> - Manage focus on route changes — screen readers need explicit focus management.
> - Use `aria-live` regions for dynamic content updates (notifications, errors).
> - Never remove focus outlines without providing an alternative focus indicator.
> - Use `@axe-core/react` in development to surface a11y violations early.

---

## 19. React Architecture & Design Patterns (Senior)

- What is the Flux architecture?
- What is unidirectional data flow? Why does React enforce it?
- What is the single source of truth principle?
- What is co-location of state? Why is it beneficial?
- What is the principle of "lifting state up"?
- What is a "God component"? How do you avoid it?
- What is the feature-based folder structure vs type-based folder structure?
- What is the Atomic Design methodology? (Atoms, Molecules, Organisms, Templates, Pages)
- What is Micro Frontend architecture? How does React support it?
- What is module federation? (Webpack 5)
- What is a Design System? How is a React component library structured?
- What is `Storybook`? How does it support design systems?
- What is the API boundary pattern? (Keep network calls out of components — use hooks)
- What is the Adapter pattern for third-party library isolation?
- What is a Boundary anti-pattern in React? (Over-engineering context)

> **Best Practices:**
> - Co-locate state with the components that use it — don't hoist everything global.
> - Keep business logic in hooks, data logic in services, presentation in components.
> - Adopt a consistent folder structure early — feature-based scales better than type-based.
> - Isolate third-party libraries behind adapters — switching libraries should not touch components.

---

## 20. React with APIs & Data Fetching

- What is `fetch` API? What is `axios`?
- What are the problems with fetching in `useEffect`? (Race conditions, no caching, no deduplication)
- What is the `useEffect` fetch pattern? What cleanup is needed?
- What is `AbortController`? How do you use it in `useEffect`?
- What is TanStack Query (React Query)?
- What is `useQuery`? What does it return?
- What is `useMutation`?
- What are `isLoading`, `isFetching`, `isError`, `isSuccess` states?
- What is query key? What is query key serialization?
- What is stale time vs cache time in React Query?
- What is background refetching?
- What is `invalidateQueries`?
- What is optimistic update in React Query (`onMutate`, `onError`, `onSettled`)?
- What is SWR (stale-while-revalidate)? How does it compare to React Query?
- What is `suspense: true` in React Query?
- What is GraphQL? What is `Apollo Client`? What is `urql`?
- What is the `useQuery` hook in Apollo Client?
- What is normalized cache in Apollo?
- What is `useSuspenseQuery` in Apollo Client?
- What is `tRPC`? What type safety does it provide?

> **Best Practices:**
> - Use TanStack Query or SWR for server state — never roll your own caching.
> - Always cancel fetch requests in `useEffect` cleanup with `AbortController`.
> - Use `staleTime` to prevent unnecessary background refetches for stable data.
> - Use `invalidateQueries` after mutations — keeps UI in sync without manual refetch.
> - Never store fetched data in Redux — server state and client state have different lifecycles.

---

## 21. React Build Tools & Ecosystem

- What is Webpack? What is a bundle?
- What is Vite? Why is it faster than Webpack in development? (ESBuild + native ES modules)
- What is `esbuild`? What is `Rollup`?
- What is tree-shaking? What enables it? (ES module static analysis)
- What is code splitting? What is dynamic import?
- What is `React.lazy` + `Suspense` for code splitting?
- What is a source map?
- What is HMR (Hot Module Replacement)?
- What is Fast Refresh? How is it different from HMR?
- What is `Create React App (CRA)`? Is it recommended now? (No — use Vite or Next.js)
- What is `Next.js`? `Remix`? `Gatsby`?
- What is the difference between `Next.js` App Router and Remix routing?
- What is `Turbopack`?
- What is `Babel`? What is `SWC`?
- What is `PostCSS`?
- What is `CSS Modules`?
- What is `Tailwind CSS`? How does it integrate with React?
- What is `styled-components`? What is `Emotion`? What is CSS-in-JS overhead?
- What is `ESLint`? What is `Prettier`?
- What is `TypeScript`? How do you set it up with Vite?
- What is `pnpm`? `yarn`? How do they differ from `npm`?

> **Best Practices:**
> - Use Vite for new SPAs — faster dev server, better DX than CRA.
> - Use Next.js for apps that need SSR, SSG, or RSC.
> - Enable tree-shaking — avoid `import * as` and barrel files with large re-exports.
> - Use CSS Modules or Tailwind over CSS-in-JS for better runtime performance.
> - Use TypeScript — catches prop type errors at compile time.

---

## 22. React Security

- What is XSS (Cross-Site Scripting)? How does React prevent it by default? (Escapes JSX output)
- What is `dangerouslySetInnerHTML`? When is it needed and what risks does it carry?
- How do you safely render HTML? (Sanitize with `DOMPurify` before `dangerouslySetInnerHTML`)
- What is CSRF? How does it affect React apps?
- What is a Content Security Policy (CSP)? How does it affect inline scripts?
- What is prototype pollution? Can it affect React apps?
- What is dependency vulnerability scanning? (`npm audit`, `Snyk`)
- What are environment variables in React? What is the `VITE_` / `REACT_APP_` prefix?
- Why should you never put secrets in frontend environment variables? (Bundled into JS — visible to users)
- What is `eval()` risk in React? (Template-based rendering, `new Function`)
- What is open redirect vulnerability in routing?
- What is clickjacking? How does `X-Frame-Options` / `frame-ancestors` CSP help?

> **Best Practices:**
> - Never use `dangerouslySetInnerHTML` with unsanitized user input — always run through `DOMPurify`.
> - Never store secrets (API keys, tokens) in frontend code or environment variables — use a backend proxy.
> - Set a strict CSP header — prevents injection of malicious scripts.
> - Run `npm audit` / `yarn audit` in CI — catch vulnerable dependencies early.
> - Validate and sanitize all URL parameters before using them in navigation or rendering.

---

## 23. React Internals — Deep Dive

- What is a Fiber node? What fields does it have?
- What is the work-in-progress tree vs current tree?
- What is the `alternate` pointer in Fiber?
- What are the two phases of React rendering: render phase and commit phase?
- What happens in the render phase? (Pure — builds Fiber tree, calls render/function, no side effects)
- What happens in the commit phase? (Mutates DOM, runs effects — not interruptible)
- What are the three sub-phases of the commit phase? (`beforeMutation`, `mutation`, `layout`)
- What is a work loop in Fiber? (Processes one unit of work at a time — can yield to browser)
- What is `requestIdleCallback` / `MessageChannel` in React's scheduler?
- What is `useState` stored in? (Fiber node's `memoizedState` — a linked list of hooks)
- What is the hooks linked list? Why can't you call hooks conditionally? (List must stay in same order)
- What is `useEffect` stored in? (Effect list on Fiber)
- What is `commitHookEffectListMount` / `commitHookEffectListUnmount`?
- What is a lane? What are React lanes? (Bit-field priority system — replaces expiration times)
- What is batching in the context of Fiber lanes?
- What is the `ReactDOM.flushSync` relation to lanes?
- What is `Offscreen` component (experimental)? (Keep-alive / pre-render)
- What is `Activity` component (React 19 rename of Offscreen)?

> **Best Practices:**
> - Understand the render vs commit phase distinction — side effects in render phase cause bugs.
> - Don't rely on render order — React may render components multiple times (Strict Mode, Concurrent Mode).
> - Keep render functions pure — same inputs must produce same output, no side effects.

---

## 24. React Portals, Fragments & Special APIs

- What is `ReactDOM.createPortal(children, container)`?
- When would you use a Portal? (Modals, tooltips, dropdowns — escape parent overflow/z-index)
- Does a Portal break the React component tree? (No — events bubble through React tree, not DOM tree)
- What is `React.Fragment`? Why use it?
- What is the difference between `<React.Fragment key={k}>` and `<>`? (Short syntax doesn't support key)
- What is `React.StrictMode`? What does it double-invoke?
- What is `React.Profiler`? How do you use it programmatically?
- What is `React.Children.map` vs `Array.from(children)`?
- What is `React.Children.toArray`? What does it do to keys?
- What is `React.cloneElement`? What is the alternative? (Render props, compound components)
- What is `React.createRef` vs `useRef`?
- What is `flushSync`?
- What is `unstable_batchedUpdates` (pre-React 18)?
- What is `React.cache` (React 19)?

---

## 25. React Native (Conceptual Awareness)

- What is React Native? How does it differ from React DOM?
- What is the Bridge in React Native (old architecture)?
- What is the New Architecture (JSI, Fabric, TurboModules)?
- What is `JSI` (JavaScript Interface)?
- What is `StyleSheet.create()`? Why is it not CSS?
- What is `Flexbox` in React Native? How does it differ from CSS Flexbox?
- What is `FlatList` vs `ScrollView`?
- What is `Expo`?
- What is the difference between `View`, `Text`, `Image`, `TouchableOpacity` and HTML equivalents?
- What is Metro bundler?

---

## 26. Tricky Senior-Level React Questions

- What is the difference between calling `useState` setter with same value vs different value?
- Why does `useEffect` run twice in Strict Mode in React 18? (Simulates mount-unmount-mount to detect non-idempotent effects)
- What is the closure problem in `useEffect`? Give an example.
- What is the stale closure in `setInterval` + `useState`? How do you fix it? (`useRef` for latest value, or `useEffect` with cleanup)
- What happens when you call a state setter during render? (If during current component render: React re-renders immediately — max once. Triggers infinite loop if conditions not met)
- Why does `React.memo` not work when passing an inline function as prop?
- What is the output if you do `setCount(count + 1); setCount(count + 1);` with `count = 0`? (`count = 1` — both use same snapshot; use functional form for `count = 2`)
- Why do two `useEffect` hooks run in order but their cleanups also run in order?
- What is `useRef` used for when you want to read the latest state inside a `setInterval`?
- What is the problem with `useContext` in a deeply nested tree when the value changes frequently?
- What happens to child components when a parent re-renders? (They re-render too, unless memoized)
- Can you update state of an unmounted component? What warning does React give? (Warning removed in React 18 — it was a false positive)
- What is the key prop as a reset mechanism? Give a use case (reset a form).
- What is tearing in Concurrent Mode? What causes it?
- What is `useSyncExternalStore` solving?

---

## 27. "Explain the Output" — React Code Puzzles

```jsx
// Q1 — setState batching
function Counter() {
  const [count, setCount] = useState(0);
  const handleClick = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  };
  return <button onClick={handleClick}>{count}</button>;
}
// Click result: count = 1 (all three use same snapshot count=0)
// Fix: setCount(c => c + 1) three times → count = 3

// Q2 — stale closure in setInterval
function Timer() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // stale closure — count is always 0
    }, 1000);
    return () => clearInterval(id);
  }, []); // missing count dep
  return <div>{count}</div>;
}
// count stays 1 forever — fix: setCount(c => c + 1)

// Q3 — object referential instability breaks React.memo
const Child = React.memo(({ style }) => <div style={style}>Hello</div>);
function Parent() {
  const [x, setX] = useState(0);
  return <Child style={{ color: 'red' }} />; // new object on every render — memo useless
}
// Fix: const style = useMemo(() => ({ color: 'red' }), []);

// Q4 — useEffect cleanup order
useEffect(() => {
  console.log('mount A');
  return () => console.log('unmount A');
}, []);
useEffect(() => {
  console.log('mount B');
  return () => console.log('unmount B');
}, []);
// Mount: "mount A", "mount B"
// Unmount: "unmount A", "unmount B"  ← same order as mount

// Q5 — key as reset
function App() {
  const [id, setId] = useState(1);
  return (
    <>
      <Form key={id} />
      <button onClick={() => setId(id + 1)}>Reset</button>
    </>
  );
}
// Changing key forces Form to fully unmount and remount — resets all internal state

// Q6 — functional update vs snapshot
function Counter() {
  const [count, setCount] = useState(0);
  const handleClick = () => {
    setCount(c => c + 1); // functional form
    setCount(c => c + 1);
    setCount(c => c + 1);
  };
  return <button onClick={handleClick}>{count}</button>;
}
// Click result: count = 3 ← each functional update receives latest queued value

// Q7 — undefined context default value
const ThemeContext = createContext(undefined);
function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
}
// Pattern: throw on missing provider — safer than silent undefined access

// Q8 — child re-render even with React.memo when context changes
const Ctx = createContext(null);
const Child = React.memo(() => {
  const val = useContext(Ctx);
  return <div>{val.count}</div>;
});
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <Ctx.Provider value={{ count }}> {/* new object every render */}
      <Child />
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </Ctx.Provider>
  );
}
// Child re-renders on every click — React.memo is bypassed by context changes
// Fix: memoize context value with useMemo

// Q9 — useEffect runs after paint
function App() {
  const [show, setShow] = useState(false);
  useEffect(() => {
    console.log('effect');
  });
  console.log('render');
  return <button onClick={() => setShow(s => !s)}>Toggle</button>;
}
// On click: "render" → (paint) → "effect"
// useEffect is async (after paint); useLayoutEffect is sync (before paint)

// Q10 — Strict Mode double render
function App() {
  console.log('render');
  const [count] = useState(() => {
    console.log('init');
    return 0;
  });
  return <div>{count}</div>;
}
// In StrictMode (dev): "init" x1, "render" x2 — render is intentionally doubled
// useEffect also fires twice: mount → cleanup → mount
```

> **Key Rules to Memorise:**
> - `setState` with same value bails out — but not immediately if called during render.
> - Multiple `setState` calls in one event handler batch into one re-render (React 18+).
> - Functional updater `setState(prev => ...)` reads from queue, not stale closure.
> - `useEffect` cleanup runs before the next effect fires and before unmount.
> - `React.memo` checks props shallowly — inline objects/functions always create new references.
> - `key` change = full unmount + remount — use it deliberately to reset component state.
> - `useContext` consumers re-render whenever context value reference changes.
> - Strict Mode double-invokes render functions and effects to catch side-effect bugs.

---

## 28. React Version History & Key Milestones

### React 0.x–15 (2013–2016)
- React open-sourced by Facebook (May 2013)
- `React.createClass` (later deprecated in favor of ES6 classes)
- Virtual DOM diffing algorithm
- React 0.14 (2015): Split into `react` and `react-dom` packages
- React 15 (2016): SVG support, `React.PureComponent`

### React 16 (2017) — Fiber Rewrite
- Complete internal rewrite with Fiber architecture
- Error Boundaries (`componentDidCatch`, `getDerivedStateFromError`)
- Portals (`ReactDOM.createPortal`)
- Return arrays and strings from render
- `React.Fragment`
- Streaming server-side rendering (`renderToNodeStream`)
- Reduced file size

### React 16.3 (2018)
- New Context API (`createContext`, `useContext` later)
- `React.createRef()`
- `getDerivedStateFromProps` (replaces `componentWillReceiveProps`)
- `getSnapshotBeforeUpdate`
- Deprecation of legacy lifecycle methods

### React 16.6 (2018)
- `React.memo`
- `React.lazy` + `Suspense`
- `React.contextType`
- `static getDerivedStateFromError`

### React 16.8 (2019) — Hooks
- `useState`, `useEffect`, `useContext`, `useReducer`, `useCallback`, `useMemo`, `useRef`, `useImperativeHandle`, `useLayoutEffect`, `useDebugValue`
- Hooks: write stateful logic without classes

### React 17 (2020)
- No new features — gradual upgrades foundation
- New JSX transform (no need for `import React from 'react'`)
- Event delegation moved from `document` to React root
- `onScroll` event no longer bubbles

### React 18 (2022) — Concurrent
- `createRoot` / `hydrateRoot` (new APIs)
- Automatic batching (all updates batched, not just React event handlers)
- `startTransition` / `useTransition`
- `useDeferredValue`
- `useId`
- `useSyncExternalStore`
- `useInsertionEffect`
- Concurrent rendering (opt-in via `createRoot`)
- Streaming SSR with selective hydration (`renderToPipeableStream`)
- Suspense on the server

### React 19 (2024)
- React Compiler (auto-memoization — no more manual `useMemo`/`useCallback`)
- `use` hook (read promises and context in render)
- Server Actions (`"use server"` directive)
- `useOptimistic`
- `useFormStatus` / `useFormState` (now `useActionState`)
- `ref` as a prop (no more `forwardRef`)
- `React.cache`
- Document metadata support (`<title>`, `<meta>` in components)
- Asset loading APIs (`preload`, `preinit`)
- Better error reporting (hydration diff)

> **Best Practices:**
> - Upgrade to React 18 — concurrent features improve UX significantly.
> - Upgrade to React 19 — React Compiler eliminates most manual memoization.
> - Use `createRoot` in React 18+ — `ReactDOM.render` is removed in React 19.
> - Adopt Server Components (Next.js App Router) to reduce client bundle size.

---

## 29. React Ecosystem Quick Reference

| Tool | Category | When to Use |
|---|---|---|
| Vite | Build tool | New SPAs, fast dev server |
| Next.js | Framework | SSR, SSG, RSC, full-stack |
| Remix | Framework | Data-focused, form-heavy apps |
| TanStack Query | Server state | Data fetching, caching, sync |
| Zustand | Client state | Lightweight global state |
| Jotai | Client state | Atomic state, granular updates |
| Redux Toolkit | Client state | Large apps, serializable state, DevTools |
| React Hook Form | Forms | Performant forms, schema validation |
| Zod | Validation | Schema-first type-safe validation |
| React Router | Routing | SPA routing |
| Radix UI | Accessible primitives | Headless accessible components |
| Tailwind CSS | Styling | Utility-first, design system |
| Storybook | UI dev / docs | Component isolation, design system |
| Playwright / Cypress | E2E testing | Full browser testing |
| React Testing Library | Unit / integration | Component behavior testing |
| MSW | API mocking | Test without real API |
| React DevTools | Debugging | Profiling, component tree inspection |
| TypeScript | Type safety | All production apps |

---

## 30. Senior Interview: Architecture & System Design Questions

- How would you design a large-scale React application folder structure?
- How would you implement authentication in a React SPA? (JWT, cookie-based, OAuth)
- How would you handle authorization (role-based access control) in React?
- How would you implement real-time updates in React? (WebSocket, SSE, polling)
- How would you implement infinite scroll? What are the performance considerations?
- How would you implement a search-as-you-type feature with debouncing?
- How would you share code between multiple React applications?
- How would you implement a multi-step form with validation?
- How would you handle error states and loading states at scale?
- How would you implement i18n (internationalization) in React? (`react-i18next`, `react-intl`)
- How would you structure a design system with React?
- How would you implement feature flags in React?
- How would you implement analytics event tracking without coupling it to components?
- How would you handle WebSocket reconnection logic in React?
- How would you migrate a large class-component app to hooks?
- How would you approach micro-frontend architecture with React?
- How would you implement a drag-and-drop interface?
- How would you prevent memory leaks in a React SPA?
- How would you implement a virtualized table with 100,000 rows?
- How would you implement undo/redo functionality?

> **Best Practices:**
> - Separate concerns: UI components know nothing about API URLs or business logic.
> - Use a service layer for API calls — components call hooks, hooks call services.
> - Implement a global error boundary at the app root + feature-level boundaries.
> - Use feature flags stored server-side — keep them out of the bundle.
> - Profile before optimizing — React DevTools Profiler shows exactly where time is spent.


