# React Interview Prep — 4 Years Experience

A structured question bank covering fundamentals → advanced → system design.
At 4 YOE, interviewers expect you to know **why**, not just **what** — so each section includes the trap/follow-up questions too.

---

## 1. Core Concepts & JSX

1. What is the Virtual DOM and how does reconciliation work?
2. Explain the diffing algorithm — how does React decide what to re-render?
3. Why does React need `key` in lists? What happens if you use array index as key?
4. What is JSX actually compiled into? (`React.createElement` vs new JSX transform)
5. Controlled vs uncontrolled components — when would you use each?
6. What's the difference between `state` and `props`?
7. Why can't you mutate state directly? What breaks if you do?
8. Explain synthetic events. Why does React wrap native DOM events?
9. What is the difference between `React.Fragment` and a `div` wrapper — why does it matter for CSS (flex/grid)?

**Follow-up trap:** "If keys are unique and stable, is index-as-key ever fine?" (Yes — static, non-reorderable lists.)

---

## 2. Component Lifecycle & Hooks

10. Map class lifecycle methods to their Hook equivalents (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`).
11. `useEffect` vs `useLayoutEffect` — real difference, and when layout effect is necessary.
12. What's the dependency array footgun? How do stale closures happen inside `useEffect`?
13. How do you fix the "stale state in event listener/timer" bug?
14. `useState` vs `useReducer` — when do you reach for `useReducer`?
15. Why can't hooks be called conditionally or inside loops? (Explain the fiber's linked-list hook order.)
16. What does `useRef` do beyond DOM references? Give a non-DOM use case.
17. Explain `useMemo` vs `useCallback` — what problem does each actually solve?
18. When does `useMemo`/`useCallback` **hurt** performance instead of helping?
19. What is `useImperativeHandle` and when would you use it with `forwardRef`?
20. Explain `useId` — why was it introduced (SSR hydration mismatch problem)?
21. How do you build a custom hook? Show a `useDebounce` or `useFetch` example and explain the cleanup logic.
22. What happens if you don't clean up a subscription/interval in `useEffect`?

**Follow-up trap:** "Does `useEffect` run before or after paint? Does `useLayoutEffect`?"

---

## 3. Rendering Behavior & Performance

23. What causes unnecessary re-renders? Walk through parent-child re-render propagation.
24. How does `React.memo` work — and why does passing a new function/object prop break it?
25. Explain referential equality issues with objects/arrays/functions as props.
26. What's the difference between `React.memo`, `useMemo`, and `useCallback` — in one sentence each.
27. How would you profile a slow React app? (React DevTools Profiler, why-did-you-render, Chrome Performance tab)
28. What is code-splitting? How do `React.lazy` + `Suspense` work together?
29. Explain windowing/virtualization (e.g., `react-window`) — why is it needed for long lists?
30. What is the cost of inline function/object definitions in JSX during render?
31. How do you avoid prop drilling — Context vs component composition vs state managers?
32. Why does overusing Context cause performance problems, and how do you mitigate it (splitting contexts, memoizing provider value)?

---

## 4. React 18 / Concurrent Features (important at 4 YOE — commonly probed)

33. What is concurrent rendering and how is it different from the legacy synchronous mode?
34. Explain `useTransition` — what problem does it solve? Give a real example (e.g., search filter typing lag).
35. `useDeferredValue` vs `useTransition` — when do you use which?
36. What is automatic batching in React 18, and how is it different from React 17 (e.g., inside promises/timeouts)?
37. Explain Suspense for data fetching — how is it different from Suspense for lazy-loaded components?
38. What is `createRoot` vs the old `ReactDOM.render`, and why did the API change?
39. What are Server Components (RSC)? How are they different from SSR?
40. Streaming SSR — what problem does it solve over traditional SSR?

---

## 5. State Management

41. Context API vs Redux vs Zustand/Jotai/Recoil — trade-offs of each.
42. When is Context the wrong tool (frequent updates, large trees)?
43. Explain Redux data flow: action → reducer → store → subscriber.
44. What problem does Redux Toolkit (RTK) solve over vanilla Redux?
45. What is RTK Query / React Query, and why use them over manually managing loading/error/cache state?
46. How do you handle server state vs client (UI) state differently?
47. Explain optimistic updates and how you'd roll back on failure.
48. How would you structure global state in a mid-size app (feature-based slices vs single store)?

---

## 6. Component Design Patterns

49. Higher-Order Components (HOC) — what problem do they solve, and what are the downsides (wrapper hell, prop collisions)?
50. Render props pattern — example and comparison with hooks.
51. Compound components pattern (e.g., how `<Select><Option/></Select>` shares implicit state) — how would you implement one using Context?
52. Controlled component pattern for building reusable form inputs.
53. What is the "container/presentational component" split, and is it still relevant with hooks?
54. How do you design a component API that's flexible but not over-configurable (too many props)?

---

## 7. Forms, Error Handling, and Edge Cases

55. How do you handle form validation — manual vs libraries (Formik, React Hook Form)? Why is React Hook Form generally more performant?
56. What is an Error Boundary? What errors does it NOT catch (event handlers, async code, SSR)?
57. How do you implement a global error boundary + fallback UI strategy in a large app?
58. How do you handle race conditions in data fetching (e.g., fast typing triggers multiple API calls)?
59. How do you cancel an in-flight fetch when a component unmounts (AbortController)?

---

## 8. Testing

60. Unit testing with React Testing Library — philosophy: "test behavior, not implementation."
61. How do you mock API calls in tests (MSW vs jest.mock)?
62. Difference between `render`, `screen`, `fireEvent`, and `userEvent`.
63. How do you test custom hooks (`renderHook`)?
64. Snapshot testing — pros, cons, and why teams often avoid over-relying on it.
65. How do you approach testing components that use Context or Redux?

---

## 9. TypeScript with React (very commonly asked at 4 YOE)

66. How do you type component props, including children and optional props?
67. Typing `useState` with a union type or complex object.
68. Typing custom hooks' return values.
69. Generic components — how and when would you write one?
70. Difference between `interface` and `type` for props — is there a real difference here?
71. How do you type event handlers (`React.ChangeEvent<HTMLInputElement>`, etc.)?

---

## 10. Architecture / System Design (senior-leaning, but expected at 4 YOE)

72. How would you structure a large-scale React application (folder structure, feature-based vs type-based)?
73. How do you handle authentication/authorization flows (protected routes, token refresh)?
74. How would you design a design system / shared component library used across multiple teams?
75. How do you handle environment-specific config and feature flags?
76. How would you approach migrating a legacy class-component app to hooks incrementally?
77. How do you manage API layer/data fetching architecture (service layer, React Query, caching strategy)?
78. How would you optimize the initial load time of a large React app (bundle size, code splitting, tree-shaking, lazy loading routes)?
79. How do you handle accessibility (a11y) in your components — what do you check for by default?
80. CSR vs SSR vs SSG vs ISR (Next.js) — trade-offs and when to use each.

---

## 11. Common Practical/Coding Exercises to Practice

- Build a debounced search input with `useEffect` + cleanup.
- Build an infinite scroll / paginated list.
- Implement a custom `useFetch` hook with loading/error/data states + cancellation.
- Build a controlled multi-step form wizard.
- Implement a modal using a portal (`ReactDOM.createPortal`).
- Build a compound component (e.g., Tabs or Accordion) using Context.
- Fix a "why does this re-render infinitely" bug (usually a `useEffect` dependency issue).
- Implement `React.memo` + `useCallback` correctly to stop a child from re-rendering unnecessarily.
- Build a simple global state manager using Context + `useReducer` (mini-Redux).

---

## 12. Rapid-Fire / Conceptual Gotchas Interviewers Love

- Why does calling `setState` inside a loop not re-render multiple times?
- Why is state update "asynchronous" (batched) inside event handlers but was synchronous in older non-batched contexts?
- What happens if two `setState` calls in the same handler use the previous state directly vs a functional updater — where does this bite you?
- Why shouldn't you call hooks inside `if` statements?
- What is the "keys must be stable" rule really protecting against internally (fiber reuse)?
- Why does `useEffect` fire twice in development with `StrictMode` in React 18?
- What is fiber architecture and why did React move away from the old stack reconciler?

---

## How to Use This List

1. **Don't just memorize answers** — for every question, be ready to draw a quick example (whiteboard or verbal) since interviewers will follow up with "show me."
2. Prioritize **Sections 2, 3, 4, 6, 9, 10** — these are where 4 YOE candidates most often lose points (hooks internals, performance, React 18, patterns, TS, architecture).
3. Practice explaining trade-offs out loud, not just definitions — interviewers weigh reasoning over recall at this level.
4. Have 1–2 real stories ready per topic ("a time I fixed a performance issue," "a time I chose Context over Redux and why") — behavioral-technical hybrid questions are common at 4 YOE.

Good luck — you've got this. 🚀
