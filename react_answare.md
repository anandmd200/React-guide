# React Interview Prep — Complete Answer Guide

> **How to use this:** Each answer follows: **Core idea → Why it matters → Simple analogy (where helpful) → Interview-ready explanation**

---

# SECTION 1: Core Concepts & JSX

---

## Q1. What is the Virtual DOM and how does reconciliation work?

### Core Idea
The Virtual DOM is a **lightweight JavaScript copy of the real DOM**. React keeps this copy in memory and uses it to figure out the minimum changes needed to update the real DOM.

### How It Works (Step by Step)
```
1. You call setState() or a state changes
2. React creates a NEW virtual DOM tree
3. React COMPARES it with the PREVIOUS virtual DOM tree (diffing)
4. React calculates the MINIMUM changes needed
5. React applies ONLY those changes to the real DOM (reconciliation)
```

### Why It Matters
Real DOM operations are **expensive**. Reading/writing to the DOM causes browser reflows and repaints. By batching and minimizing DOM operations, React makes UI updates faster.

### Simple Analogy
> Think of it like editing a Word document. Instead of retyping the entire document when you change one word, you just change that one word. React does the same — it figures out exactly what changed and updates only that.

### Interview-Ready Answer
*"The Virtual DOM is a JS object representation of the real DOM. When state changes, React creates a new virtual DOM tree, diffs it against the previous one using its reconciliation algorithm, and then applies the minimum set of DOM mutations needed. This avoids expensive full DOM re-renders."*

---

## Q2. Explain the diffing algorithm — how does React decide what to re-render?

### Core Rules React Uses

**Rule 1: Different element types = destroy and rebuild**
```jsx
// Before
<div><Counter /></div>

// After
<span><Counter /></span>
// React tears down the div tree and builds span tree from scratch
// Counter loses its state!
```

**Rule 2: Same element type = update attributes only**
```jsx
// Before
<div className="old" style={{color: 'red'}} />

// After
<div className="new" style={{color: 'blue'}} />
// React just updates className and color — doesn't recreate the div
```

**Rule 3: Keys help React match list items**
```jsx
// React uses keys to know which item is which in a list
<li key="user-1">Alice</li>
<li key="user-2">Bob</li>
```

### The Big Assumption React Makes
React's diffing is **O(n)** not O(n³) because it assumes:
- Two elements of different types produce different trees
- Keys identify which items are stable across renders

### Interview-Ready Answer
*"React's diffing has two main rules: if the element type changes, React tears down and rebuilds the subtree. If the type stays the same, React just updates the changed attributes. For lists, React uses keys to match items across renders. This makes diffing O(n) rather than the theoretical O(n³)."*

---

## Q3. Why does React need `key` in lists? What happens if you use array index as key?

### Why Keys Exist
Keys tell React **which list item maps to which fiber** across re-renders. Without them, React can't tell if an item moved, was added, or was removed.

### The Index-as-Key Problem

```jsx
// Data: ['Alice', 'Bob', 'Charlie']
// Keys by index: 0, 1, 2

// After removing 'Alice':
// Data: ['Bob', 'Charlie']
// Keys by index: 0, 1  ← React thinks index-0 is still 'Alice's slot!

// React UPDATES 'Alice' → 'Bob' and 'Bob' → 'Charlie'
// Instead of REMOVING 'Alice'

// This causes:
// - Wrong animations
// - Wrong form input values
// - Wrong component state
```

```jsx
// ❌ Bad — reordering will bug out
{users.map((user, index) => (
  <UserCard key={index} user={user} />
))}

// ✅ Good — stable, unique identity
{users.map(user => (
  <UserCard key={user.id} user={user} />
))}
```

### When Index-as-Key is OK (the follow-up trap answer)
✅ Static list that **never reorders, filters, or removes items**
✅ Items have no internal state
✅ No animations tied to list position

### Interview-Ready Answer
*"Keys let React identify which list items changed, moved, or were removed between renders. Using index as key breaks when items are reordered or removed — React maps the key to the wrong item, causing state to end up in the wrong component. Stable unique IDs from your data are always preferred."*

---

## Q4. What is JSX actually compiled into?

### Old Transform (before React 17)
```jsx
// You write:
const element = <h1 className="title">Hello</h1>;

// Babel compiles to:
const element = React.createElement('h1', { className: 'title' }, 'Hello');
// That's why you needed `import React from 'react'` in every file!
```

### New JSX Transform (React 17+)
```jsx
// You write:
const element = <h1 className="title">Hello</h1>;

// Babel compiles to:
import { jsx as _jsx } from 'react/jsx-runtime';
const element = _jsx('h1', { className: 'title', children: 'Hello' });
// No need to import React anymore!
```

### What `React.createElement` Returns
```js
{
  type: 'h1',
  props: {
    className: 'title',
    children: 'Hello'
  },
  key: null,
  ref: null
}
// Just a plain JavaScript object — this IS the virtual DOM node
```

### Interview-Ready Answer
*"JSX is syntactic sugar. With the old transform, it compiled to `React.createElement()` calls, which is why you had to import React in every file. With React 17's new JSX transform, it imports from `react/jsx-runtime` automatically, so you don't need that import. Either way, the result is a plain JS object describing a UI node."*

---

## Q5. Controlled vs Uncontrolled Components — when would you use each?

### Controlled Component
React **owns the state**. The input value always reflects what's in state.

```jsx
function ControlledInput() {
  const [value, setValue] = useState('');

  return (
    <input
      value={value}                          // React controls the value
      onChange={e => setValue(e.target.value)} // React updates on change
    />
  );
}
```

### Uncontrolled Component
The **DOM owns the state**. You read the value only when you need it (via ref).

```jsx
function UncontrolledInput() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    console.log(inputRef.current.value); // Read only when needed
  };

  return <input ref={inputRef} defaultValue="initial" />;
}
```

### Quick Comparison

| | Controlled | Uncontrolled |
|---|---|---|
| State owner | React | DOM |
| Access value | `state` | `ref.current.value` |
| Instant validation | ✅ Easy | ❌ Hard |
| File inputs | ❌ Not possible | ✅ Only way |
| Performance | More re-renders | Fewer re-renders |

### When to Use Which
- **Controlled:** Form validation, conditional disabling, formatting on input, dependent fields
- **Uncontrolled:** File inputs (only option), simple forms where you just need value on submit, third-party DOM integrations

### Interview-Ready Answer
*"Controlled components have React managing the input state — every keystroke triggers a state update. Uncontrolled components let the DOM manage it and you read the value via a ref. I default to controlled because it's easier to validate and react to input changes. But I'd use uncontrolled for file inputs or integrating with non-React libraries."*

---

## Q6. What's the difference between `state` and `props`?

### Simple Distinction
- **Props** = data passed **into** a component (like function arguments) — read-only
- **State** = data managed **inside** a component — can change over time

```jsx
// Props — parent passes data down
function Greeting({ name, age }) {  // name and age are props
  return <p>Hello {name}, you are {age}</p>;
}

// State — component owns and manages this
function Counter() {
  const [count, setCount] = useState(0);  // count is state
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

### Key Rules
| | Props | State |
|---|---|---|
| Who owns it? | Parent component | The component itself |
| Mutable? | ❌ Never mutate props | ✅ Via setState only |
| Triggers re-render? | Yes (when parent re-renders) | Yes (when setState called) |
| Can be defaulted? | Yes (defaultProps) | Yes (initial state) |

### Interview-Ready Answer
*"Props are inputs passed from a parent — a component cannot change its own props. State is internal data a component manages itself, and changing it causes a re-render. A common pattern is 'lifting state up' — moving state to a common parent and passing it down as props to children that need it."*

---

## Q7. Why can't you mutate state directly? What breaks if you do?

### What Happens When You Mutate Directly

```jsx
// ❌ WRONG — direct mutation
const [user, setUser] = useState({ name: 'Alice', age: 25 });

function updateAge() {
  user.age = 26;          // Mutating the object directly!
  setUser(user);          // Passing the SAME object reference
}
// React does a shallow comparison: old reference === new reference → TRUE
// React thinks nothing changed → NO RE-RENDER!
```

```jsx
// ✅ CORRECT — create new object
function updateAge() {
  setUser({ ...user, age: 26 }); // New object → new reference → re-render
}
```

### What Specifically Breaks
1. **No re-render** — React's change detection relies on reference inequality
2. **Unpredictable bugs** — previous state is corrupted (can't time-travel debug)
3. **StrictMode double-invocation** catches this — but only in dev
4. **Redux DevTools / time-travel debugging** breaks — history is mutated

### Why React Uses Reference Equality
Doing a deep comparison of objects on every render would be **O(n)** and kill performance. Reference comparison is **O(1)**.

### Interview-Ready Answer
*"React uses shallow reference comparison to detect state changes. If you mutate state directly, the object reference stays the same, so React thinks nothing changed and skips the re-render. You always need to return a new object or array. This is also why spread operators and array methods like `map`, `filter`, and `slice` are commonly used instead of `push` or direct assignment."*

---

## Q8. Explain synthetic events. Why does React wrap native DOM events?

### What Synthetic Events Are
React wraps native browser events in a **SyntheticEvent** — a cross-browser normalized wrapper that has the same API regardless of which browser you're in.

```jsx
function handleClick(event) {
  // event is a SyntheticEvent, NOT a native MouseEvent
  event.preventDefault();     // Works the same in all browsers
  event.stopPropagation();    // Works the same in all browsers
  console.log(event.target);  // Normalized, consistent
  
  // You can access the native event if needed:
  console.log(event.nativeEvent);
}
```

### Why React Does This
1. **Cross-browser consistency** — IE used `window.event`, others used argument. Normalized API works everywhere.
2. **Event delegation** — React attaches ONE listener at the root (document in React 17, root element in React 18) instead of per-element. Massive performance win.
3. **Control over event system** — React can implement features like batching, concurrent mode priority, etc.

### The Event Pooling Gotcha (pre-React 17)
```jsx
// React 16 and earlier — event pooling recycled events
function handleClick(event) {
  setTimeout(() => {
    console.log(event.type); // ❌ NULL — event was returned to pool!
  }, 100);
  
  // Fix was: const type = event.type; // copy before async
}
// React 17+ removed event pooling — this is no longer an issue
```

### Interview-Ready Answer
*"Synthetic events are React's cross-browser wrapper around native DOM events. They normalize the API so you get the same behavior across all browsers. React also uses event delegation — attaching a single listener at the root rather than per element, which is more memory efficient. In React 17, event pooling was removed, so you can safely access events asynchronously now."*

---

## Q9. `React.Fragment` vs a `div` wrapper — why does it matter for CSS?

### The Problem with Unnecessary Divs

```jsx
// ❌ Extra div that breaks layout
function TableRow() {
  return (
    <div>          {/* Invalid! div inside table breaks HTML */}
      <td>Name</td>
      <td>Age</td>
    </div>
  );
}

// ✅ Fragment — no extra DOM node
function TableRow() {
  return (
    <>
      <td>Name</td>
      <td>Age</td>
    </>
  );
}
```

### Where It Breaks CSS Specifically

```jsx
// Parent is a flex container
<div style={{ display: 'flex' }}>
  <Sidebar />   {/* If Sidebar wraps in a div, flex layout breaks */}
  <Main />
</div>

// Sidebar with extra div:
function Sidebar() {
  return <div><nav>...</nav></div>; // Now flex sees a div, not the nav
}

// Sidebar with Fragment:
function Sidebar() {
  return <><nav>...</nav></>;  // Flex sees the nav directly ✅
}
```

### Fragment with Key (when you need to key fragments in a list)
```jsx
// Shorthand <></> doesn't support keys
// Use full syntax when you need keys:
{items.map(item => (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.description}</dd>
  </React.Fragment>
))}
```

### Interview-Ready Answer
*"Fragments let you return multiple elements without adding an extra DOM node. This matters for CSS flex/grid layouts where an extra wrapper div becomes a flex/grid item and breaks the intended layout. It also matters for valid HTML — you can't put a div inside a table or a select. Fragments are the right tool when the wrapper serves no semantic or styling purpose."*

---

# SECTION 2: Component Lifecycle & Hooks

---

## Q10. Map class lifecycle methods to Hook equivalents

### The Complete Mapping

```jsx
// ============ CLASS COMPONENT ============
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { data: null }; // → useState(null)
  }

  componentDidMount() {
    // Runs once after first render
    fetchData().then(data => this.setState({ data }));
  }

  componentDidUpdate(prevProps, prevState) {
    // Runs after every update
    if (prevProps.id !== this.props.id) {
      fetchData(this.props.id);
    }
  }

  componentWillUnmount() {
    // Cleanup before component is removed
    this.subscription.unsubscribe();
  }

  render() {
    return <div>{this.state.data}</div>;
  }
}
```

```jsx
// ============ HOOKS EQUIVALENT ============
function MyComponent({ id }) {
  const [data, setData] = useState(null);

  // componentDidMount → empty dependency array
  useEffect(() => {
    fetchData().then(data => setData(data));
  }, []); // ← empty array = run once after mount

  // componentDidUpdate → specific dependency
  useEffect(() => {
    fetchData(id);
  }, [id]); // ← runs when `id` changes

  // componentWillUnmount → return cleanup function
  useEffect(() => {
    const subscription = subscribeToSomething();
    return () => {
      subscription.unsubscribe(); // ← cleanup
    };
  }, []);

  return <div>{data}</div>;
}
```

### Quick Reference Table

| Class Lifecycle | Hook Equivalent |
|---|---|
| `constructor` | `useState` initial value |
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [dep])` |
| `componentWillUnmount` | `return () => {}` inside `useEffect` |
| `shouldComponentUpdate` | `React.memo` |
| `getDerivedStateFromProps` | Logic inside render or `useMemo` |
| `getSnapshotBeforeUpdate` | `useLayoutEffect` |

### Interview-Ready Answer
*"`componentDidMount` maps to `useEffect` with an empty dependency array. `componentDidUpdate` maps to `useEffect` with specific dependencies. `componentWillUnmount` maps to the cleanup function returned from `useEffect`. The power of hooks is that you can co-locate related logic — a subscription setup and its cleanup live in the same `useEffect` block instead of being split across three lifecycle methods."*

---

## Q11. `useEffect` vs `useLayoutEffect` — real difference and when layout effect is necessary

### The Timeline Difference

```
RENDER PHASE:
  → React renders (calls your component function)
  → React updates the DOM

THEN:
  → Browser PAINTS the screen to the user   ← visual update visible here
  → useEffect fires                          ← after paint (async)

BUT:
  → useLayoutEffect fires BEFORE the paint  ← blocks paint (sync)
```

### Visual Timeline
```
State change
    ↓
React re-renders
    ↓
React updates DOM
    ↓
useLayoutEffect runs ← synchronous, blocks paint
    ↓
Browser paints (user sees update)
    ↓
useEffect runs ← asynchronous
```

### When to Use `useLayoutEffect`

```jsx
// Use case: measuring DOM element size/position
// If you do this in useEffect, user sees a flicker!
function Tooltip({ children, text }) {
  const ref = useRef(null);
  const [position, setPosition] = useState({ top: 0, left: 0 });

  useLayoutEffect(() => {
    // This runs BEFORE paint, so user never sees the wrong position
    const rect = ref.current.getBoundingClientRect();
    setPosition({
      top: rect.bottom,
      left: rect.left
    });
  }, []); // ← useEffect would cause a flicker here

  return (
    <div>
      <span ref={ref}>{children}</span>
      <div style={position}>{text}</div>
    </div>
  );
}
```
```jsx
import React, { useRef, useLayoutEffect } from "react";

function UseLayoutEffect() {
  const boxRef: any = useRef(null);

  useLayoutEffect(() => {
    boxRef.current.style.width = "300px";
    console.log("useLayoutEffect executed");
  }, []);

  return (
    <div
      ref={boxRef}
      style={{
        width: "100px",
        height: "100px",
        background: "skyblue",
      }}
    >
      Box
    </div>
  );
}

export default UseLayoutEffect;
```


### Rule of Thumb
- **`useEffect` (95% of cases):** API calls, subscriptions, analytics, timers
- **`useLayoutEffect` (5% of cases):** DOM measurements, preventing visual flicker, syncing with third-party DOM libraries

### Interview-Ready Answer
*"`useEffect` runs asynchronously after the browser has painted — so it doesn't block the UI. `useLayoutEffect` runs synchronously after DOM mutations but before the browser paints. You need `useLayoutEffect` when you're reading DOM layout (like element dimensions) and immediately writing back to avoid a visible flicker. The classic case is tooltips that need to position themselves based on their anchor element's position."*

---

## Q12. The dependency array footgun — how stale closures happen

### What a Stale Closure Is

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      // ❌ STALE CLOSURE: count is always 0 here!
      // This function captured count=0 when the effect first ran
      // and never got a new version of count
      console.log('count is:', count); // Always prints 0
      setCount(count + 1);             // Always sets to 1!
    }, 1000);

    return () => clearInterval(timer);
  }, []); // ← empty array = runs once = count is forever 0 inside
```

### Why It Happens
```
Effect runs at mount → captures count=0 in closure
State updates: count becomes 1, 2, 3...
But the interval callback still has count=0 from its closure
It's like a snapshot of count at the time the effect ran
```

### Fix 1: Add to Dependency Array
```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log('count:', count); // Now always fresh
    setCount(count + 1);
  }, 1000);

  return () => clearInterval(timer);
}, [count]); // ✅ Re-runs when count changes, gets fresh count
// Downside: clears and resets interval every second
```

### Fix 2: Functional Updater (Better for timers)
```jsx
useEffect(() => {
  const timer = setInterval(() => {
    setCount(prev => prev + 1); // ✅ Always gets latest state, no stale closure
  }, 1000);

  return () => clearInterval(timer);
}, []); // ✅ Empty array is now safe because we don't read count directly
```

### Fix 3: useRef for latest value
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const countRef = useRef(count);
  countRef.current = count; // Always up to date

  useEffect(() => {
    const timer = setInterval(() => {
      console.log(countRef.current); // ✅ Always fresh — refs don't capture
    }, 1000);
    return () => clearInterval(timer);
  }, []); // ✅ Safe
}
```

### Interview-Ready Answer
*"A stale closure happens when a `useEffect` callback captures a variable from its outer scope, but the dependency array doesn't include that variable — so the effect never re-runs with the updated value. The classic example is a timer that always reads the initial state value. The fix is either adding the variable to the dependency array, using a functional state updater (`prev => prev + 1`), or storing the value in a ref."*

---

## Q13. How to fix "stale state in event listener/timer" bug

```jsx
// ❌ The bug
function SearchComponent() {
  const [query, setQuery] = useState('');

  useEffect(() => {
    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  }, []); // query is stale inside handleKeyDown!

  function handleKeyDown(e) {
    console.log('Current query:', query); // Always empty string!
  }
}
```

### Three Solutions

```jsx
// ✅ Solution 1: Include in deps (re-registers listener on change)
useEffect(() => {
  window.addEventListener('keydown', handleKeyDown);
  return () => window.removeEventListener('keydown', handleKeyDown);
}, [query]); // Re-runs when query changes

// ✅ Solution 2: useRef to hold latest value
const queryRef = useRef(query);
queryRef.current = query; // Update ref on every render

useEffect(() => {
  const handleKeyDown = (e) => {
    console.log('Current query:', queryRef.current); // Always fresh!
  };
  window.addEventListener('keydown', handleKeyDown);
  return () => window.removeEventListener('keydown', handleKeyDown);
}, []); // Safe — uses ref, not closure

// ✅ Solution 3: useCallback with dep (if passing function around)
const handleKeyDown = useCallback((e) => {
  console.log('Current query:', query);
}, [query]);
```

---

## Q14. `useState` vs `useReducer` — when to reach for `useReducer`

### `useState` is Great For
- Simple, independent pieces of state
- Single values (string, number, boolean)
- State changes that don't depend on other state

### `useReducer` is Better When

```jsx
// ❌ useState getting messy with related state
function Form() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [success, setSuccess] = useState(false);

  async function handleSubmit() {
    setLoading(true);
    setError(null);
    setSuccess(false);
    try {
      await submitForm({ name, email });
      setSuccess(true);
    } catch(e) {
      setError(e.message);
    } finally {
      setLoading(false);
    }
  }
}
```

```jsx
// ✅ useReducer — clean, predictable state transitions
const initialState = {
  name: '', email: '', loading: false, error: null, success: false
};

function reducer(state, action) {
  switch (action.type) {
    case 'FIELD_CHANGE':
      return { ...state, [action.field]: action.value };
    case 'SUBMIT_START':
      return { ...state, loading: true, error: null, success: false };
    case 'SUBMIT_SUCCESS':
      return { ...state, loading: false, success: true };
    case 'SUBMIT_ERROR':
      return { ...state, loading: false, error: action.error };
    default:
      return state;
  }
}

function Form() {
  const [state, dispatch] = useReducer(reducer, initialState);

  async function handleSubmit() {
    dispatch({ type: 'SUBMIT_START' });
    try {
      await submitForm(state);
      dispatch({ type: 'SUBMIT_SUCCESS' });
    } catch(e) {
      dispatch({ type: 'SUBMIT_ERROR', error: e.message });
    }
  }
}
```

### Decision Framework
```
Is state a simple scalar value?           → useState
Is state an object with 2-3 properties?   → useState with object
Are there complex state transitions?       → useReducer
Does next state depend on action type?    → useReducer
Multiple setState calls that go together? → useReducer
Need easy testing of state logic alone?   → useReducer
```

### Interview-Ready Answer
*"I use `useState` for simple, independent values. I reach for `useReducer` when state has multiple sub-values that update together, when the next state depends on the current state plus some action type, or when I have complex state transition logic I want to test in isolation. It also makes the 'why' of state changes more explicit since you have named action types."*

---

## Q15. Why can't hooks be called conditionally or inside loops?

### React's Hook System — The Linked List
React stores hook state in a **linked list** on the fiber node, indexed by **call order**.

```
First render:
useState(0)     → Fiber[0] = { value: 0 }
useState('')    → Fiber[1] = { value: '' }
useEffect(fn)   → Fiber[2] = { effect: fn }

Second render (same order assumed):
useState(0)     → Reads Fiber[0] ✅
useState('')    → Reads Fiber[1] ✅
useEffect(fn)   → Reads Fiber[2] ✅
```

### What Happens with Conditional Hooks

```jsx
// ❌ ILLEGAL — conditional hook
function Component({ isLoggedIn }) {
  if (isLoggedIn) {
    const [name, setName] = useState(''); // Hook sometimes called!
  }
  const [count, setCount] = useState(0);

  // First render (isLoggedIn = true):
  // Fiber[0] = useState('') → name
  // Fiber[1] = useState(0)  → count

  // Second render (isLoggedIn = false):
  // Fiber[0] = useState(0)  → WRONG! React gives count the name slot!
  // Fiber[1] = nothing      → count is gone!
}
```

### The Fix
```jsx
// ✅ Hook always called — conditionals go INSIDE
function Component({ isLoggedIn }) {
  const [name, setName] = useState('');  // Always called
  const [count, setCount] = useState(0); // Always called

  useEffect(() => {
    if (isLoggedIn) {        // Condition INSIDE the hook
      fetchUserName().then(setName);
    }
  }, [isLoggedIn]);
}
```

### Interview-Ready Answer
*"React tracks hooks by their call order — it uses a linked list on the fiber where each position maps to a specific hook. If you call hooks conditionally, the order can change between renders, and React reads the wrong state for the wrong hook. That's why the rule is 'call hooks at the top level, always.' The condition should go inside the hook, not around it."*

---

## Q16. What does `useRef` do beyond DOM references?

### The Core Property of `useRef`
`useRef` returns an object `{ current: value }` that:
- **Persists across renders** (same object, not recreated)
- **Does NOT trigger re-renders** when `.current` changes

### Non-DOM Use Cases

```jsx
// Use case 1: Storing previous prop/state value
function Component({ value }) {
  const prevValue = useRef(value);

  useEffect(() => {
    console.log('Changed from', prevValue.current, 'to', value);
    prevValue.current = value; // Update after logging
  }, [value]);
}
```

```jsx
// Use case 2: Tracking if component is mounted (avoid setState on unmounted)
function FetchComponent() {
  const isMounted = useRef(true);
  const [data, setData] = useState(null);

  useEffect(() => {
    return () => { isMounted.current = false; };
  }, []);

  useEffect(() => {
    fetchData().then(result => {
      if (isMounted.current) {  // Don't update if unmounted
        setData(result);
      }
    });
  }, []);
}
```

```jsx
// Use case 3: Storing a timer/interval ID
function Timer() {
  const timerRef = useRef(null);

  function start() {
    timerRef.current = setInterval(() => {
      console.log('tick');
    }, 1000);
  }

  function stop() {
    clearInterval(timerRef.current);
  }
}
```

```jsx
// Use case 4: Storing latest callback (avoiding stale closure)
function useEventListener(event, handler) {
  const handlerRef = useRef(handler);
  handlerRef.current = handler; // Always latest

  useEffect(() => {
    window.addEventListener(event, e => handlerRef.current(e));
    return () => window.removeEventListener(event, ...);
  }, [event]); // Stable — doesn't re-register when handler changes
}
```

### Interview-Ready Answer
*"Beyond DOM references, `useRef` is useful anytime you want a mutable value that persists across renders without triggering a re-render. Common non-DOM uses: storing previous props, holding a timer ID so you can cancel it, tracking component mount status to avoid setting state after unmount, or holding the latest version of a callback to avoid stale closure issues."*

---

## Q17. `useMemo` vs `useCallback` — what problem does each solve?

### `useMemo` — Caches a **computed value**
```jsx
// Without useMemo — expensive calculation runs every render
function ProductList({ products, filterText }) {
  // This runs on EVERY render, even if products and filterText didn't change
  const filtered = products.filter(p =>
    p.name.toLowerCase().includes(filterText.toLowerCase())
  );
  return <List items={filtered} />;
}

// With useMemo — only recalculates when dependencies change
function ProductList({ products, filterText }) {
  const filtered = useMemo(
    () => products.filter(p =>
      p.name.toLowerCase().includes(filterText.toLowerCase())
    ),
    [products, filterText] // Only recompute when these change
  );
  return <List items={filtered} />;
}
```

### `useCallback` — Caches a **function reference**
```jsx
// Without useCallback — new function reference every render
function Parent() {
  const [count, setCount] = useState(0);

  // New function object created every render
  const handleClick = () => console.log('clicked');

  return <MemoizedChild onClick={handleClick} />; // Breaks memo!
}

// With useCallback — same function reference across renders
function Parent() {
  const [count, setCount] = useState(0);

  // Same function reference unless deps change
  const handleClick = useCallback(() => {
    console.log('clicked');
  }, []); // No deps = never recreated

  return <MemoizedChild onClick={handleClick} />; // Memo works! ✅
}
```

### One-Sentence Each (Q26 answer too)
- **`React.memo`** — Prevents a component from re-rendering if its props haven't changed
- **`useMemo`** — Caches the **result** of a calculation between renders
- **`useCallback`** — Caches a **function** between renders (so its reference stays stable)

### The Relationship
```jsx
// useCallback(fn, deps) is essentially:
// useMemo(() => fn, deps)
// Both cache something — one caches functions, one caches values
```

### Interview-Ready Answer
*"`useMemo` memoizes a computed value — useful for expensive calculations you don't want to repeat every render. `useCallback` memoizes a function's reference — useful when passing callbacks to memoized child components, since a new function reference on every render would break `React.memo`. `useCallback(fn, deps)` is actually shorthand for `useMemo(() => fn, deps)`."*

---

## Q18. When does `useMemo`/`useCallback` HURT performance?

### The Overhead They Add
```jsx
// useMemo isn't free — every call:
// 1. Stores the cached value in memory
// 2. Compares dependencies on every render (even if returning cached value)
// 3. Adds complexity to your code

const value = useMemo(() => a + b, [a, b]);
// For something this simple, the memoization cost > computation cost!
```

### When NOT to Use Them

```jsx
// ❌ Over-memoizing simple values
const doubled = useMemo(() => count * 2, [count]); // Unnecessary!
const doubled = count * 2; // ✅ This is fine

// ❌ Memoizing primitive-returning functions passed to non-memoized children
function Parent() {
  const getLabel = useCallback(() => 'hello', []); // Useless!
  return <RegularChild getLabel={getLabel} />;
  // RegularChild isn't memoized, so it re-renders anyway
}

// ❌ Memoizing things that change every render anyway
const value = useMemo(() => expensiveCalc(a, b), [a, b]);
// If a or b change on every render, memoization never helps
```

### The Decision Framework
```
Is the computation actually slow?      (measure first!)
Is the child component actually memo'd?
Do the deps actually stay stable?

If yes to all → useMemo/useCallback helps
If no to any → probably hurting more than helping
```

### Interview-Ready Answer
*"They hurt performance when overused. Every `useMemo`/`useCallback` adds memory overhead and dependency comparison cost on every render. If the computation is cheap, that overhead outweighs the benefit. Also, `useCallback` on a function passed to a non-memoized component is wasted effort since the child re-renders anyway. The rule is: measure first, then optimize — don't premature-memoize."*

---

## Q19. `useImperativeHandle` and `forwardRef`

### The Problem They Solve
Sometimes a parent needs to call a method on a child component (like `.focus()` or `.reset()`). By default, you can't put a ref on a function component.

```jsx
// ❌ This doesn't work — can't ref a function component by default
function Parent() {
  const ref = useRef(null);
  return <FancyInput ref={ref} />; // ref.current will be null
}
```

### `forwardRef` — passes ref through to a DOM element
```jsx
// ✅ forwardRef lets parent access the inner DOM input
const FancyInput = forwardRef(function FancyInput(props, ref) {
  return <input ref={ref} {...props} />;
  // Now parent's ref.current IS the <input> element
});

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

### `useImperativeHandle` — controls WHAT the ref exposes
```jsx
// Instead of exposing the raw DOM node, expose specific methods
const FancyInput = forwardRef(function FancyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    // Parent can ONLY do these things — DOM node is encapsulated
    focus: () => inputRef.current.focus(),
    clear: () => { inputRef.current.value = ''; },
    getValue: () => inputRef.current.value,
    // Parent can't access inputRef.current.style, etc.
  }));

  return <input ref={inputRef} {...props} />;
});

function Parent() {
  const ref = useRef(null);
  // Now ref.current = { focus, clear, getValue } — not the DOM node
  return (
    <>
      <FancyInput ref={ref} />
      <button onClick={() => ref.current.clear()}>Clear</button>
    </>
  );
}
```

### Interview-Ready Answer
*"`forwardRef` lets a function component receive a ref from its parent and forward it to a DOM element inside. `useImperativeHandle` goes a step further — instead of exposing the raw DOM node, you define a custom API object (like `{ focus, reset }`) that the parent can call. This is good for component encapsulation — you control exactly what's accessible via the ref."*

---

## Q20. `useId` — why was it introduced?

### The Problem: SSR Hydration Mismatch

```jsx
// ❌ Before useId — generating IDs manually
let counter = 0;
function Input({ label }) {
  // Using a module-level counter
  const id = `input-${++counter}`; // id="input-1"

  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </>
  );
}

// Problem with SSR:
// Server renders: id="input-1"
// Client hydrates: id="input-2" (counter reset between server & client)
// Hydration MISMATCH → React error, potential UI bugs
```

### The Solution: `useId`
```jsx
// ✅ useId generates consistent IDs between server and client
function Input({ label }) {
  const id = useId(); // e.g., ":r0:" — same on server AND client

  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </>
  );
}

// Multiple IDs in one component
function Form() {
  const nameId = useId();
  const emailId = useId();

  // nameId = ":r0:"
  // emailId = ":r1:"
  // Consistent, unique, no collisions
}
```

### What `useId` Does NOT Replace
```jsx
// ❌ Don't use useId for list keys!
{items.map(item => (
  <li key={useId()}>...</li> // Hooks can't go in loops!
))}
// useId is for accessibility IDs (htmlFor/id pairs), not list keys
```

### Interview-Ready Answer
*"`useId` was introduced to solve SSR hydration mismatches. Before it, developers used counters or Math.random() to generate IDs for `htmlFor`/`id` pairs, but these produced different values on the server vs client, causing React hydration errors. `useId` generates stable, unique IDs that are consistent across server and client renders."*

---

## Q21. How to build a custom hook — `useDebounce` example

```jsx
// useDebounce — delays updating a value until input stops changing
function useDebounce(value, delay = 500) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    // Set a timer to update debouncedValue after `delay` ms
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // CLEANUP: If value changes before delay is up,
    // cancel the previous timer and start fresh
    return () => clearTimeout(timer);
  }, [value, delay]); // Re-run when value or delay changes

  return debouncedValue;
}

// Usage
function SearchInput() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);

  useEffect(() => {
    if (debouncedQuery) {
      fetchSearchResults(debouncedQuery); // Only calls API after 300ms pause
    }
  }, [debouncedQuery]);

  return <input value={query} onChange={e => setQuery(e.target.value)} />;
}
```

### `useFetch` — with cleanup
```jsx
function useFetch(url) {
  const [state, setState] = useState({
    data: null,
    loading: true,
    error: null
  });

  useEffect(() => {
    const controller = new AbortController(); // For cancellation
    setState({ data: null, loading: true, error: null });

    fetch(url, { signal: controller.signal })
      .then(res => {
        if (!res.ok) throw new Error(res.statusText);
        return res.json();
      })
      .then(data => setState({ data, loading: false, error: null }))
      .catch(err => {
        if (err.name !== 'AbortError') { // Ignore intentional cancellation
          setState({ data: null, loading: false, error: err.message });
        }
      });

    // Cleanup: cancel fetch if component unmounts or url changes
    return () => controller.abort();
  }, [url]);

  return state; // { data, loading, error }
}

// Usage
function UserProfile({ userId }) {
  const { data, loading, error } = useFetch(`/api/users/${userId}`);
  if (loading) return <Spinner />;
  if (error) return <Error message={error} />;
  return <Profile user={data} />;
}
```

### Interview-Ready Answer
*"Custom hooks are just functions that start with 'use' and call other hooks. They let you extract and reuse stateful logic. The key in `useDebounce` is the cleanup function — every time the value changes, we cancel the previous timer before starting a new one. Without cleanup, you'd get multiple timers firing and incorrect behavior."*

---

## Q22. What happens if you don't clean up subscriptions/intervals?

### Memory Leak Scenario
```jsx
// ❌ No cleanup
function LiveData() {
  const [data, setData] = useState(null);

  useEffect(() => {
    const interval = setInterval(() => {
      fetch('/api/data').then(r => r.json()).then(setData);
    }, 1000);
    // No return! Interval keeps running after unmount.
  }, []);
}

// What happens:
// 1. Component mounts → interval starts
// 2. Component unmounts (user navigates away)
// 3. Interval KEEPS RUNNING (memory leak!)
// 4. fetch completes → tries to call setData on unmounted component
// 5. React warning: "Can't perform state update on unmounted component"
// 6. Over time: multiple intervals stacking up, performance degrades
```

### With Cleanup
```jsx
// ✅ With cleanup
useEffect(() => {
  const interval = setInterval(() => {
    fetch('/api/data').then(r => r.json()).then(setData);
  }, 1000);

  return () => {
    clearInterval(interval); // Stop when component unmounts
  };
}, []);
```

### Common Things That Need Cleanup
```jsx
// Timers
return () => clearInterval(id);
return () => clearTimeout(id);

// Event listeners
return () => window.removeEventListener('resize', handler);

// Subscriptions
return () => subscription.unsubscribe();

// AbortController for fetch
return () => controller.abort();

// WebSocket
return () => socket.close();
```

### Interview-Ready Answer
*"Without cleanup, you get memory leaks and potential errors. Intervals, event listeners, and subscriptions keep running even after the component unmounts. When they eventually try to update state on the unmounted component, you get a React warning about state updates on unmounted components. The cleanup function in `useEffect` is where you cancel timers, remove listeners, and abort fetches."*

---

# SECTION 3: Rendering Behavior & Performance

---

## Q23. What causes unnecessary re-renders?

### Re-render Trigger Chain
```
setState called
    ↓
That component re-renders
    ↓
ALL children re-render (by default)
    ↓
All THEIR children re-render
    ↓ (unless React.memo stops it)
```

### Common Causes

```jsx
// Cause 1: Parent re-renders = all children re-render
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>+</button>
      <ExpensiveChild />  {/* Re-renders even though count doesn't affect it! */}
    </div>
  );
}

// Cause 2: New object/array/function reference every render
function Parent() {
  // New object every render → child always sees "new" prop
  const style = { color: 'red' }; // ← recreated every render
  return <Child style={style} />;
}

// Cause 3: Context value change triggers all consumers
const ThemeContext = createContext();
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  // This object is new every render!
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
  // Every consumer re-renders on EVERY parent render
}

// Cause 4: State updates in render
function BadComponent() {
  const [x, setX] = useState(0);
  setX(1); // ❌ Infinite loop! Render → setState → render → setState
}
```

### Interview-Ready Answer
*"Unnecessary re-renders mainly come from three sources: parent re-renders propagating down by default, unstable prop references (new object/array/function literals on each render), and Context value changes triggering all consumers. The tools to fix them are `React.memo`, `useMemo`, and `useCallback` — but you should profile first to confirm there's actually a problem."*

---

## Q24. How does `React.memo` work — and why do new function/object props break it?

### How `React.memo` Works
```jsx
// React.memo wraps a component and adds shallow prop comparison
const ExpensiveChild = React.memo(function ExpensiveChild({ name, onClick }) {
  console.log('ExpensiveChild rendered');
  return <button onClick={onClick}>{name}</button>;
});

// Before re-rendering ExpensiveChild, React checks:
// Are all props shallowly equal to previous render?
// If YES → skip re-render (return previous result)
// If NO → re-render
```

### Why New Functions Break It

```jsx
// ❌ Breaks memo — new function reference every render
function Parent() {
  const [count, setCount] = useState(0);

  // New function object created every render
  const handleClick = () => console.log('clicked');

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      {/* handleClick is a new reference → memo comparison fails → re-renders */}
      <ExpensiveChild name="Alice" onClick={handleClick} />
    </>
  );
}

// ✅ Fixed with useCallback
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => console.log('clicked'), []);
  // Same reference every render → memo comparison passes → no re-render

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <ExpensiveChild name="Alice" onClick={handleClick} />
    </>
  );
}
```

### Custom Comparison Function
```jsx
// If you need deep comparison or custom logic:
const ExpensiveChild = React.memo(
  function ExpensiveChild({ data }) { /* ... */ },
  (prevProps, nextProps) => {
    // Return true = skip re-render (props are "equal")
    // Return false = re-render
    return prevProps.data.id === nextProps.data.id;
  }
);
```

### Interview-Ready Answer
*"`React.memo` does a shallow comparison of props before deciding to re-render. Shallow comparison means it checks if `prevProp === nextProp` for each prop. New functions and objects fail this check because a new function literal (`() => {}`) creates a new reference each render, even if it does the same thing. That's why `useCallback` is a companion to `React.memo`."*

---

## Q25. Referential equality issues with objects/arrays/functions

This is directly answered in Q24 above. Key points to add:

```jsx
// Every render creates new references for:
const obj = {};           // New object
const arr = [];           // New array
const fn = () => {};      // New function

// Primitives are stable:
const str = "hello";      // Same reference (interned)
const num = 42;           // Same reference
const bool = true;        // Same reference

// The fix:
const obj = useMemo(() => ({ key: 'value' }), []);
const arr = useMemo(() => [1, 2, 3], []);
const fn = useCallback(() => doSomething(), []);
```

---

## Q27. How to profile a slow React app

### Step 1: React DevTools Profiler
```
1. Install React DevTools browser extension
2. Open DevTools → Profiler tab
3. Click Record → interact with your app → Stop
4. See which components rendered, why, and how long

Look for:
- Components with large render times (wide bars)
- Components rendering too often (many bars)
- "Why did this render?" feature shows which prop/state changed
```

### Step 2: `why-did-you-render` Library
```jsx
// Add to your app entry point (dev only)
import whyDidYouRender from '@welldone-software/why-did-you-render';
import React from 'react';

if (process.env.NODE_ENV === 'development') {
  whyDidYouRender(React, {
    trackAllPureComponents: true,
  });
}

// Mark specific components to watch
ExpensiveComponent.whyDidYouRender = true;
// Will log to console: "ExpensiveComponent re-rendered because prop X changed from A to B"
```

### Step 3: Chrome Performance Tab
```
1. Chrome DevTools → Performance tab
2. Record → interact → Stop
3. Look for:
   - Long tasks (>50ms) in the main thread
   - React's reconciliation time
   - Layout/paint events triggered by React
```

### Interview-Ready Answer
*"I start with the React DevTools Profiler — it shows which components rendered, in what order, and how long each took. The 'why did this render' feature is especially useful. I also use the `why-did-you-render` library in development to catch unnecessary re-renders automatically. For deeper issues, the Chrome Performance tab shows if we're blocking the main thread."*

---

## Q28. Code-splitting with `React.lazy` + `Suspense`

```jsx
// Without code-splitting:
// Everything is bundled into one huge JS file → slow initial load

// With React.lazy:
// Import the component only when it's first needed

// ✅ Lazy load a heavy component
const HeavyDashboard = React.lazy(() => import('./HeavyDashboard'));
// This creates a dynamic import — webpack/vite splits this into a separate chunk

function App() {
  const [showDashboard, setShowDashboard] = useState(false);

  return (
    <div>
      <button onClick={() => setShowDashboard(true)}>Load Dashboard</button>

      {showDashboard && (
        {/* Suspense shows fallback while the chunk downloads */}
        <Suspense fallback={<div>Loading dashboard...</div>}>
          <HeavyDashboard />
        </Suspense>
      )}
    </div>
  );
}

// Common pattern: Route-based code splitting
const Home = React.lazy(() => import('./pages/Home'));
const About = React.lazy(() => import('./pages/About'));
const Admin = React.lazy(() => import('./pages/Admin')); // Huge! Load only when needed

function App() {
  return (
    <Suspense fallback={<PageSpinner />}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/admin" element={<Admin />} />
      </Routes>
    </Suspense>
  );
}
```

### Interview-Ready Answer
*"`React.lazy` takes a function that returns a dynamic import, which causes the bundler to split that module into a separate chunk. `Suspense` wraps the lazy component and shows a fallback while the chunk loads. The most impactful place to apply this is route-level splitting — users only download code for the pages they actually visit."*

---

## Q29. Windowing/Virtualization for long lists

### The Problem
```jsx
// Rendering 10,000 list items:
{users.map(user => <UserCard key={user.id} user={user} />)}
// This creates 10,000 DOM nodes → browser slows to a crawl
// But user can only see ~20 items at a time!
```

### The Solution — Virtualization
```jsx
import { FixedSizeList } from 'react-window';

function VirtualList({ users }) {
  const Row = ({ index, style }) => (
    // style contains position CSS from react-window
    <div style={style}>
      <UserCard user={users[index]} />
    </div>
  );

  return (
    <FixedSizeList
      height={600}        // Container height
      itemCount={users.length}  // Total items
      itemSize={80}       // Height of each row
      width="100%"
    >
      {Row}
    </FixedSizeList>
    // Only renders ~10 rows at a time regardless of list size!
  );
}
```

### How It Works
```
Total: 10,000 items
Visible area: 600px / 80px per item = ~8 visible items

react-window renders only: 8 visible + ~5 buffer = ~13 DOM nodes
As you scroll: it reuses and repositions those ~13 nodes
Result: constant DOM size regardless of list length!
```

### Interview-Ready Answer
*"Virtualization means only rendering the DOM nodes that are actually visible in the viewport, plus a small buffer. `react-window` achieves this by absolutely positioning list items and reusing DOM nodes as you scroll. Instead of 10,000 DOM nodes for a 10,000-item list, you maintain maybe 20. This is crucial for large datasets — the browser can't handle thousands of DOM nodes efficiently."*

---

## Q31. Avoiding prop drilling — Context vs composition vs state managers

### Prop Drilling Problem
```jsx
// ❌ Prop drilling — passing user through many layers unnecessarily
<App>
  <Layout user={user}>
    <Sidebar user={user}>
      <UserMenu user={user}>
        <Avatar user={user} />  {/* Only this actually needs it! */}
      </UserMenu>
    </Sidebar>
  </Layout>
</App>
```

### Solution 1: Component Composition (Underused!)
```jsx
// ✅ Pass components/children instead of data
function App() {
  const user = useCurrentUser();
  const avatar = <Avatar user={user} />; // Compose here, where you have the data

  return (
    <Layout>
      <Sidebar userMenu={<UserMenu avatar={avatar} />}>
        {/* Layout and Sidebar don't need to know about user! */}
      </Sidebar>
    </Layout>
  );
}
```

### Solution 2: Context API
```jsx
// ✅ Good for: auth user, theme, locale — infrequently changing data
const UserContext = createContext(null);

function App() {
  const user = useCurrentUser();
  return (
    <UserContext.Provider value={user}>
      <Layout /> {/* No props needed */}
    </UserContext.Provider>
  );
}

function Avatar() {
  const user = useContext(UserContext); // Reads directly
  return <img src={user.avatar} />;
}
```

### Solution 3: Global State Manager (Zustand, Redux)
```jsx
// ✅ Good for: frequently changing state, complex logic, many consumers
const useStore = create(set => ({
  user: null,
  setUser: (user) => set({ user })
}));

function Avatar() {
  const user = useStore(state => state.user); // Direct access, no providers
}
```

### When to Use What
| Solution | Best For |
|---|---|
| Composition | Avoiding unnecessary prop passing, slots/render props |
| Context | Infrequent updates, global config (theme/locale/auth) |
| State Manager | Frequent updates, complex state logic, many subscribers |

---

## Q32. Why Context causes performance problems and how to fix it

### The Problem
```jsx
// ❌ Every consumer re-renders when ANY part of context changes
const AppContext = createContext();

function AppProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  const [cart, setCart] = useState([]);

  // This object is new on every render
  const value = { user, setUser, theme, setTheme, cart, setCart };

  return (
    <AppContext.Provider value={value}>
      {children}
    </AppContext.Provider>
  );
}

// Changing cart causes ALL context consumers to re-render
// Even components that only use `theme`!
```

### Fix 1: Split Contexts
```jsx
// ✅ Separate contexts = separate subscriptions
const UserContext = createContext();
const ThemeContext = createContext();
const CartContext = createContext();

// Components using ThemeContext don't re-render when cart changes
```

### Fix 2: Memoize the Provider Value
```jsx
// ✅ Stable reference for context value
function AppProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');

  const value = useMemo(
    () => ({ user, setUser, theme, setTheme }),
    [user, theme] // Only creates new object when user or theme changes
  );

  return (
    <AppContext.Provider value={value}>
      {children}
    </AppContext.Provider>
  );
}
```

### Fix 3: Context Selector Pattern (or use Zustand)
```jsx
// If you need fine-grained subscriptions, consider Zustand
// which has built-in selector optimization
const user = useStore(state => state.user); // Only re-renders if user changes
```

---

# SECTION 4: React 18 / Concurrent Features

---

## Q33. What is concurrent rendering?

### Legacy (Synchronous) Mode
```
State update triggered
→ React starts rendering
→ Renders ALL the way through, no matter how long it takes
→ Browser is BLOCKED the entire time
→ If render takes 500ms → UI freezes for 500ms
→ User clicks, types → no response → janky UX
```

### Concurrent Mode
```
State update triggered
→ React starts rendering
→ Rendering is now INTERRUPTIBLE
→ Browser gets control back to handle user input/paint
→ React can PAUSE, RESUME, or ABANDON renders
→ UI stays responsive even during expensive renders
→ No more "frozen" UI
```

### The Key Mental Model
```jsx
// In sync mode: React is a single-lane highway
// One render must complete before anything else happens

// In concurrent mode: React is a multi-lane highway
// Urgent updates (user typing) can jump ahead of
// non-urgent updates (background data refresh)
```

### Interview-Ready Answer
*"Concurrent rendering means React can work on multiple versions of the UI simultaneously and interrupt, pause, or abandon renders. Previously, a render was synchronous and blocked the main thread until completion. Concurrent mode lets the browser stay responsive — React can pause an expensive render, let the browser handle a user click, then resume. This is the foundation for features like `useTransition` and `Suspense`."*

---

## Q34. `useTransition` — what problem does it solve?

### The Problem
```jsx
// Heavy list filtering — typing feels laggy
function SearchPage() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState(allItems);

  function handleChange(e) {
    setQuery(e.target.value);
    // This is expensive — filtering 10,000 items → UI blocks
    setResults(allItems.filter(item =>
      item.name.includes(e.target.value)
    ));
  }

  return (
    <>
      <input value={query} onChange={handleChange} />
      <LargeList items={results} />
    </>
  );
  // Typing feels laggy because list re-render blocks input update
}
```

### The Fix with `useTransition`
```jsx
function SearchPage() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState(allItems);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    const value = e.target.value;

    // URGENT: Update the input immediately (high priority)
    setQuery(value);

    // NON-URGENT: Update results as a transition (can be interrupted)
    startTransition(() => {
      setResults(allItems.filter(item => item.name.includes(value)));
    });
  }

  return (
    <>
      <input value={query} onChange={handleChange} />
      {isPending && <Spinner />}  {/* Show while transition is processing */}
      <LargeList items={results} />
    </>
  );
  // Input updates instantly! Results update when React has time.
}
```

### How It Works Internally
```
User types 'a'
→ setQuery('a') - URGENT, renders immediately
→ startTransition(() => setResults(filtered))
   → React starts this render
   → User types 'ab'
   → React INTERRUPTS the 'a' render
   → Runs urgent setQuery('ab') first
   → Then restarts transition for 'ab'
   → Old 'a' render is thrown away (wasted!)
```

### Interview-Ready Answer
*"`useTransition` lets you mark state updates as non-urgent. React can interrupt, delay, or restart those updates to prioritize urgent ones like user input. The classic use case is search/filter — you mark the list re-render as a transition so typing stays responsive, even if filtering is expensive. The `isPending` flag lets you show a loading indicator during the transition."*

---

## Q35. `useDeferredValue` vs `useTransition`

### Quick Distinction
- **`useTransition`** — you control the state update (you own the setter)
- **`useDeferredValue`** — you only have the value (prop or third-party state)

```jsx
// useTransition: you control the setter
const [isPending, startTransition] = useTransition();
startTransition(() => {
  setResults(filtered); // You control this setState
});

// useDeferredValue: you receive a value from outside
function SearchResults({ query }) { // query comes from parent — can't control when it updates
  const deferredQuery = useDeferredValue(query);
  // deferredQuery lags behind query during transitions
  // React renders with the old deferredQuery first (instant)
  // Then re-renders with the new deferredQuery when idle
}
```

### Side by Side

```jsx
// Same effect, different control point

// useTransition version (you own the state):
function Parent() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    startTransition(() => setQuery(e.target.value));
  };

  return <ExpensiveList query={query} />;
}

// useDeferredValue version (you receive the value):
function ExpensiveList({ query }) {
  const deferredQuery = useDeferredValue(query);
  // Use deferredQuery for expensive rendering
  const results = useMemo(
    () => filterItems(allItems, deferredQuery),
    [deferredQuery]
  );
  return <List items={results} />;
}
```

### Decision
- Own the state? → `useTransition`
- Receive value as prop? → `useDeferredValue`

---

## Q36. Automatic batching in React 18

### React 17 — Batching Only Inside React Event Handlers
```jsx
// React 17 behavior:
function handleClick() {
  setCount(c => c + 1); // Batched
  setName('Alice');      // Batched
  // Only ONE re-render — React batches these
}

// But NOT batched in async contexts:
setTimeout(() => {
  setCount(c => c + 1); // Re-render 1
  setName('Alice');      // Re-render 2 ← TWO re-renders!
}, 0);

fetch('/api').then(() => {
  setCount(c => c + 1); // Re-render 1
  setName('Alice');      // Re-render 2 ← TWO re-renders!
});
```

### React 18 — Automatic Batching Everywhere
```jsx
// React 18 — EVERYTHING is batched automatically
setTimeout(() => {
  setCount(c => c + 1); // Batched!
  setName('Alice');      // Batched!
  // ONE re-render — even in setTimeout
}, 0);

fetch('/api').then(() => {
  setCount(c => c + 1); // Batched!
  setName('Alice');      // Batched!
  // ONE re-render — even in promises
});
```

### Opt Out of Batching (rare)
```jsx
import { flushSync } from 'react-dom';

// Force immediate re-render (opt out of batching)
flushSync(() => {
  setCount(c => c + 1); // Renders immediately
});
// Another render here
flushSync(() => {
  setName('Alice'); // Renders immediately
});
```

### Interview-Ready Answer
*"React 17 only batched state updates inside React event handlers. Updates in `setTimeout`, promises, or native event listeners caused multiple re-renders. React 18 introduced automatic batching — all state updates are batched regardless of where they happen, including timeouts and promises. This reduces unnecessary re-renders and improves performance. You can opt out with `flushSync` if you need immediate DOM updates."*

---

## Q37. Suspense for data fetching vs lazy-loaded components

### Suspense for Lazy Loading (React 16+)
```jsx
// Component-loading Suspense — the chunk isn't downloaded yet
const Dashboard = React.lazy(() => import('./Dashboard'));

<Suspense fallback={<Spinner />}>
  <Dashboard />  {/* Shows spinner while JS chunk downloads */}
</Suspense>
// Suspense activates when: component code isn't loaded yet
```

### Suspense for Data Fetching (React 18, with compatible libraries)
```jsx
// Data-fetching Suspense — the data isn't ready yet
// React Query / SWR / Relay integrate with this

function UserProfile({ userId }) {
  // This "suspends" — throws a Promise that React.Suspense catches
  const user = use(fetchUser(userId)); // React 18's `use` hook

  return <div>{user.name}</div>;
}

<Suspense fallback={<ProfileSkeleton />}>
  <UserProfile userId="123" />  {/* Shows skeleton while data loads */}
</Suspense>
```

### The Mechanism (how data Suspense works)
```
1. Component renders
2. Data isn't ready → library throws a Promise (special signal)
3. React catches the Promise via Suspense boundary
4. Shows fallback
5. Promise resolves (data arrives)
6. React retries rendering the component
7. Data is now available → renders normally
8. Fallback replaced with real content
```

### Key Difference
| | Lazy Suspense | Data Suspense |
|---|---|---|
| What's loading? | JavaScript chunk | Server data |
| Available since? | React 16 | React 18 (proper) |
| How? | `React.lazy` | `use()`, React Query Suspense mode |

---

## Q38. `createRoot` vs `ReactDOM.render`

### Old API (React 17)
```jsx
import ReactDOM from 'react-dom';
import App from './App';

// Old way — synchronous, legacy mode
ReactDOM.render(<App />, document.getElementById('root'));
```

### New API (React 18)
```jsx
import { createRoot } from 'react-dom/client';
import App from './App';

// New way — concurrent mode enabled
const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

### Why the Change?
1. **Opt-in to Concurrent Mode** — `createRoot` enables all React 18 concurrent features (`useTransition`, automatic batching, etc.)
2. **Separation of root creation and rendering** — cleaner API
3. **`root.unmount()`** — explicit cleanup API
4. **Backward compatible** — old `ReactDOM.render` still works but is deprecated

### Interview-Ready Answer
*"`createRoot` is the React 18 way to mount your app — it enables concurrent mode and all React 18 features like automatic batching and `useTransition`. The old `ReactDOM.render` runs in legacy mode without these features. It's a one-line change at your app entry point, but it unlocks everything React 18 has to offer."*

---

## Q39. Server Components (RSC) vs SSR

### Traditional SSR (Server-Side Rendering)
```
1. User requests page
2. Server runs React (renders components to HTML string)
3. Server sends HTML to browser → user sees content fast
4. Browser downloads JavaScript bundle (ALL components)
5. React hydrates (attaches event listeners to HTML)
6. App becomes interactive

Problem: You still send ALL the component JS to the browser
Even components that never need interactivity
```

### React Server Components
```
Server Components run ONLY on the server
They can:
  ✅ Access databases directly
  ✅ Read files
  ✅ Use secrets/API keys (never exposed to client)
  ✅ Render to a lightweight format (not HTML string)

They CANNOT:
  ❌ Use useState
  ❌ Use useEffect
  ❌ Use browser APIs
  ❌ Add event handlers

The JS for Server Components is NEVER sent to the browser
→ Smaller bundles, faster loads
```

### Code Example (Next.js App Router)
```jsx
// app/UserProfile/page.js
// This is a Server Component by default in Next.js App Router
async function UserProfile({ userId }) {
  // Direct DB access — no API needed! Code never runs on client.
  const user = await db.query('SELECT * FROM users WHERE id = ?', [userId]);

  return (
    <div>
      <h1>{user.name}</h1>
      <LikeButton userId={userId} />  {/* Client Component — needs interactivity */}
    </div>
  );
}

// 'use client' — Client Component for interactive parts
'use client';
function LikeButton({ userId }) {
  const [liked, setLiked] = useState(false);
  return <button onClick={() => setLiked(l => !l)}>{liked ? '❤️' : '🤍'}</button>;
}
```

### RSC vs SSR vs CSR Quick Table

| | CSR | SSR | RSC |
|---|---|---|---|
| Renders where? | Browser | Server (then hydrates) | Server (no hydration) |
| Bundle size | Large | Large | Smaller (no server component JS) |
| DB access? | Via API | Via API | Direct! |
| Interactivity? | Full | After hydration | Client components only |
| useState/effects? | Yes | Yes (client) | No |

---

## Q40. Streaming SSR

### Traditional SSR Problem
```
Traditional SSR:
1. Request comes in
2. Server fetches ALL data (slowest query blocks everything)
3. Renders ALL HTML
4. Sends entire HTML at once
5. Browser can only start showing content after ALL data loaded

If one API call takes 3 seconds → user waits 3 seconds for ANYTHING
```

### Streaming SSR (React 18)
```
Streaming SSR:
1. Request comes in
2. Server starts rendering immediately
3. Fast parts stream to browser RIGHT AWAY
4. Browser shows fast content while server works on slow parts
5. Slow parts (wrapped in Suspense) send their HTML when ready
6. No waiting for everything!
```

```jsx
// Next.js with streaming SSR
export default async function Page() {
  return (
    <main>
      <h1>Welcome!</h1>  {/* Streams immediately */}

      <Suspense fallback={<NavSkeleton />}>
        <SlowNavigation />  {/* Streams when its data is ready */}
      </Suspense>

      <Suspense fallback={<FeedSkeleton />}>
        <SlowNewsFeed />    {/* Streams independently — doesn't block nav! */}
      </Suspense>
    </main>
  );
}
```

### Interview-Ready Answer
*"Traditional SSR blocks on the slowest data fetch — nothing is sent until everything is ready. Streaming SSR sends HTML in chunks as it becomes ready. Fast parts of the page appear immediately while slow Suspense boundaries stream in their HTML when their data resolves. Users see content much faster, and the Time to First Byte (TTFB) and Largest Contentful Paint (LCP) metrics improve significantly."*

---

# SECTION 5: State Management

---

## Q41. Context vs Redux vs Zustand/Jotai — trade-offs

### Context API
```
✅ Built-in, no library needed
✅ Great for: theme, auth, locale (slow-changing data)
✅ Simple mental model
❌ No built-in optimization — all consumers re-render on any change
❌ No DevTools, no time-travel debugging
❌ No built-in async handling
❌ Can cause performance issues at scale
```

### Redux (with RTK)
```
✅ Predictable, single source of truth
✅ Excellent DevTools (time-travel, action replay)
✅ Middleware support (async thunks, sagas)
✅ Battle-tested at large scale
❌ Boilerplate (even with RTK)
❌ Overkill for small apps
❌ Learning curve
```

### Zustand
```
✅ Tiny (~1kb)
✅ Simple API — minimal boilerplate
✅ Selector-based subscriptions (only re-render when your slice changes)
✅ No Provider needed
✅ Good DevTools support
❌ Less opinionated (can be a pro or con)
❌ Smaller ecosystem than Redux
```

### Jotai/Recoil (Atomic State)
```
✅ Atom-based — fine-grained subscriptions
✅ Co-locate state with components that need it
✅ Excellent for partially independent state across the tree
❌ Different mental model (atoms, not one store)
❌ Jotai is minimal; Recoil has Facebook backing but slower updates
```

### My Decision Framework
```
Local state only?              → useState/useReducer
Infrequent global state?       → Context
Complex client state + devtools? → Zustand or RTK
Server state (cache, sync)?    → React Query / RTK Query
Fine-grained atom state?       → Jotai
```

---

## Q44. What problem does Redux Toolkit (RTK) solve?

### Vanilla Redux Pain
```js
// ❌ Vanilla Redux — so much boilerplate

// 1. Action types (strings — typo-prone)
const INCREMENT = 'counter/increment';
const DECREMENT = 'counter/decrement';

// 2. Action creators
const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });

// 3. Reducer (must handle immutability manually)
function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case INCREMENT:
      return { ...state, value: state.value + 1 }; // Must spread
    case DECREMENT:
      return { ...state, value: state.value - 1 };
    default:
      return state;
  }
}

// 4. Store setup (with middleware)
import { createStore, applyMiddleware, compose } from 'redux';
import thunk from 'redux-thunk';
const store = createStore(counterReducer, applyMiddleware(thunk));
```

### RTK — Same Thing, Much Cleaner
```js
// ✅ RTK — significantly less boilerplate
import { createSlice, configureStore } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1; }, // Can "mutate"! Immer handles immutability
    decrement: state => { state.value -= 1; },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;

const store = configureStore({
  reducer: { counter: counterSlice.reducer }
  // Redux DevTools and thunk middleware included automatically!
});
```

### RTK's Key Benefits
1. **Immer built-in** — write "mutating" code, get immutable updates
2. **`createSlice`** — generates action creators AND types from one object
3. **`configureStore`** — DevTools and thunk middleware set up automatically
4. **RTK Query** — full data fetching solution (like React Query built into RTK)

---

## Q45. RTK Query / React Query — why use them?

### The Problem They Solve
```jsx
// ❌ Manually managing server state — repetitive and error-prone
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch('/api/users')
      .then(r => r.json())
      .then(data => { setUsers(data); setLoading(false); })
      .catch(err => { setError(err); setLoading(false); });
  }, []);

  // What about:
  // - Caching? (same data fetched on every mount)
  // - Deduplication? (two components fetch same endpoint simultaneously)
  // - Background refetch? (data goes stale)
  // - Optimistic updates?
  // - Refetch on window focus?
  // All manual!
}
```

```jsx
// ✅ React Query — handles all of that
function UserList() {
  const {
    data: users,
    isLoading,
    isError,
    error
  } = useQuery({
    queryKey: ['users'],  // Cache key
    queryFn: () => fetch('/api/users').then(r => r.json()),
    staleTime: 5 * 60 * 1000, // Consider fresh for 5 minutes
  });

  // Automatic:
  // ✅ Caching — same queryKey = same cache
  // ✅ Deduplication — multiple components, one request
  // ✅ Background refetch when window regains focus
  // ✅ Retry on failure
  // ✅ Loading/error states
}
```

### Interview-Ready Answer
*"React Query and RTK Query separate server state from client state. Server state (data from an API) has unique challenges — caching, background updates, deduplication, and stale data management. These libraries handle all of that automatically. You get loading/error/data states for free, automatic refetching, request deduplication, and cache invalidation. This replaces hundreds of lines of manual `useEffect`/loading state code."*

---

## Q47. Optimistic updates and rollback on failure

```jsx
// Optimistic Update: Update UI immediately, then sync with server
// If server fails → roll back to previous state

function TodoList() {
  const queryClient = useQueryClient();

  const toggleMutation = useMutation({
    mutationFn: (todo) => updateTodo(todo), // Slow API call

    // Step 1: Before API call — optimistically update cache
    onMutate: async (newTodo) => {
      // Cancel any outgoing refetches (so they don't overwrite optimistic update)
      await queryClient.cancelQueries({ queryKey: ['todos'] });

      // Snapshot the previous value (for rollback)
      const previousTodos = queryClient.getQueryData(['todos']);

      // Optimistically update cache
      queryClient.setQueryData(['todos'], (old) =>
        old.map(todo =>
          todo.id === newTodo.id ? { ...todo, done: !todo.done } : todo
        )
      );

      return { previousTodos }; // Return context for rollback
    },

    // Step 2: If error → rollback to previous state
    onError: (err, newTodo, context) => {
      queryClient.setQueryData(['todos'], context.previousTodos);
      showErrorToast('Failed to update todo');
    },

    // Step 3: Always refetch after mutation (server is source of truth)
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['todos'] });
    }
  });

  return (
    <ul>
      {todos.map(todo => (
        <li
          key={todo.id}
          onClick={() => toggleMutation.mutate(todo)}
          style={{ opacity: toggleMutation.isPending ? 0.5 : 1 }}
        >
          {todo.done ? '✅' : '⬜'} {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

---

# SECTION 6: Component Design Patterns

---

## Q49. Higher-Order Components (HOC)

### What HOCs Are
```jsx
// An HOC is a function that takes a component and returns a new component
// Pattern: function(Component) → EnhancedComponent

function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { user, isLoading } = useAuth();

    if (isLoading) return <Spinner />;
    if (!user) return <Redirect to="/login" />;

    return <WrappedComponent {...props} user={user} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
const ProtectedProfile = withAuth(Profile);
```

### Downsides of HOCs
```jsx
// 1. Wrapper hell — hard to read component tree
<withAuth(withLogging(withTheme(withData(Dashboard))))>

// 2. Prop collisions — HOC might overwrite component's prop
function withUser(Component) {
  return function(props) {
    return <Component {...props} user={injectedUser} />;
    // If props already has `user` prop → conflict!
  };
}

// 3. Ref forwarding issues — refs don't pass through automatically
// 4. Hard to see what props come from where (prop origin unclear)
// 5. Static composition — can't conditionally apply
```

### Modern Alternative: Custom Hooks
```jsx
// HOC approach:
const ProtectedDashboard = withAuth(Dashboard);

// Hook approach (simpler):
function Dashboard() {
  const { user } = useAuth(); // Same logic, no wrapper
  if (!user) return <Redirect to="/login" />;
  return <div>Dashboard for {user.name}</div>;
}
```

### When HOCs Still Make Sense
- Cross-cutting concerns that need to wrap component structure
- Integrating with class components
- Library patterns (React.memo is technically an HOC!)

---

## Q51. Compound Components Pattern

### The Pattern — Implicit Shared State
```jsx
// External API you're building:
<Select value={selected} onChange={setSelected}>
  <Select.Option value="apple">Apple</Select.Option>
  <Select.Option value="banana">Banana</Select.Option>
</Select>
// Options don't need explicit props — they share parent's state implicitly
```

### Implementation with Context

```jsx
// 1. Create context for internal state sharing
const SelectContext = createContext(null);

// 2. Parent component manages state and provides context
function Select({ value, onChange, children }) {
  return (
    <SelectContext.Provider value={{ value, onChange }}>
      <div className="select-container">
        {children}
      </div>
    </SelectContext.Provider>
  );
}

// 3. Child components consume shared context
function Option({ value, children }) {
  const { value: selectedValue, onChange } = useContext(SelectContext);
  const isSelected = selectedValue === value;

  return (
    <div
      className={`option ${isSelected ? 'selected' : ''}`}
      onClick={() => onChange(value)}
    >
      {isSelected && '✓ '}{children}
    </div>
  );
}

// 4. Attach as static properties for clean API
Select.Option = Option;

// Usage:
function App() {
  const [fruit, setFruit] = useState('apple');

  return (
    <Select value={fruit} onChange={setFruit}>
      <Select.Option value="apple">🍎 Apple</Select.Option>
      <Select.Option value="banana">🍌 Banana</Select.Option>
      <Select.Option value="cherry">🍒 Cherry</Select.Option>
    </Select>
  );
}
```

### Interview-Ready Answer
*"The compound component pattern lets you build components that work together through implicit shared state via Context, rather than explicit prop-passing. The parent manages state and exposes it through a Context provider, child components consume that context. The API becomes clean — `<Select.Option>` doesn't need `onChange` or `selectedValue` as props; it reads them from context automatically."*

---

## Q54. Designing flexible but not over-configurable component APIs

### Too Many Props (Bad)
```jsx
// ❌ Over-configurable — too many concerns in one component
<Button
  size="large"
  variant="primary"
  icon="rocket"
  iconPosition="left"
  loading={true}
  loadingText="Saving..."
  fullWidth={true}
  rounded={true}
  shadow="large"
  hoverEffect="scale"
  onClick={handleClick}
  onHover={handleHover}
  analyticsId="save-btn"
  tooltipText="Save your work"
/>
```

### Better Patterns

```jsx
// ✅ Composition over config
<Button variant="primary" size="large" onClick={handleClick}>
  <RocketIcon />   {/* Icon composition */}
  Save
</Button>

// ✅ Slots pattern
<Button
  leading={<Spinner />}
  trailing={<ChevronRight />}
  onClick={handleClick}
>
  Submit
</Button>

// ✅ Extend with as/className for flexibility
<Button as="a" href="/home">   {/* Render as anchor */}
  Go Home
</Button>

// ✅ Variant maps — limited, named combinations
// Instead of: size="custom-14px-bold"
// Offer: size="sm" | "md" | "lg"
```

### API Design Principles
1. **Start minimal** — add props when users ask for them
2. **Favor composition** — use `children`, slots, render props
3. **Named variants** — `variant="primary"` not `background="blue" fontWeight="bold"`
4. **`className`/`style` escape hatch** — for one-off customizations
5. **`as` prop** — let users change the rendered element

---

# SECTION 7: Forms, Error Handling, Edge Cases

---

## Q55. Form validation — Formik vs React Hook Form performance

### Why React Hook Form is Faster
```jsx
// Formik — controlled components (re-renders on every keystroke)
// Every character typed → state update → entire form re-renders

// React Hook Form — uncontrolled by default
// Uses refs internally → NO re-renders while typing
// Only re-renders on validation trigger or submit
```

```jsx
// React Hook Form example
import { useForm } from 'react-hook-form';

function LoginForm() {
  const {
    register,    // registers an input with validation rules
    handleSubmit, // wraps submit, validates first
    formState: { errors, isSubmitting }
  } = useForm({
    mode: 'onBlur' // Validate on blur, not on every keystroke
  });

  const onSubmit = async (data) => {
    // data is already validated here
    await login(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('email', {
          required: 'Email is required',
          pattern: { value: /^\S+@\S+\.\S+$/, message: 'Invalid email' }
        })}
      />
      {errors.email && <span>{errors.email.message}</span>}

      <input
        type="password"
        {...register('password', {
          required: true,
          minLength: { value: 8, message: 'Min 8 characters' }
        })}
      />
      {errors.password && <span>{errors.password.message}</span>}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

### Interview-Ready Answer
*"React Hook Form is more performant because it uses uncontrolled inputs with refs by default — no re-render per keystroke. Formik uses controlled inputs, so every character typed triggers a state update and re-render. For large forms, this is a significant difference. RHF also has a smaller bundle size and integrates well with Zod/Yup for schema validation."*

---

## Q56. Error Boundaries — what they catch and don't catch

### What Error Boundaries Catch
```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false, error: null };

  // Called when a child throws during rendering
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  // Called for logging — don't use for state updates
  componentDidCatch(error, errorInfo) {
    logToErrorService(error, errorInfo.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <ComponentThatMightCrash />
</ErrorBoundary>
```

### What Error Boundaries DO Catch ✅
- Errors in render methods
- Errors in lifecycle methods
- Errors in constructors of child components

### What Error Boundaries DO NOT Catch ❌
```jsx
// ❌ Event handlers — must use try/catch yourself
function Button() {
  const handleClick = () => {
    try {
      throw new Error('Click error!');
    } catch (e) {
      setError(e.message); // Handle manually
    }
  };
  return <button onClick={handleClick}>Click</button>;
}

// ❌ Async code (setTimeout, promises)
useEffect(() => {
  setTimeout(() => {
    throw new Error('Async!'); // Error Boundary won't catch this
  }, 1000);
}, []);

// ❌ SSR errors

// ❌ Errors in the Error Boundary itself
```

### Modern Hook Alternative (React 18)
```jsx
// useErrorBoundary from react-error-boundary library
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div>
      <p>Something went wrong: {error.message}</p>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

<ErrorBoundary
  FallbackComponent={ErrorFallback}
  onError={(error) => logError(error)}
  onReset={() => { /* reset app state */ }}
>
  <App />
</ErrorBoundary>
```

---

## Q58. Race conditions in data fetching

### The Problem
```jsx
// User types: 'a', then 'ab', then 'abc'
// Three fetches start simultaneously
// Responses come back in any order!
// Last response wins — but it might be 'a' returning after 'abc'!

function Search() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  useEffect(() => {
    fetch(`/api/search?q=${query}`)
      .then(r => r.json())
      .then(data => setResults(data)); // ❌ Race condition!
      // If 'a' returns after 'abc', results show 'a' results for 'abc' query
  }, [query]);
}
```

### Fix 1: Ignore Stale Requests (Flag)
```jsx
useEffect(() => {
  let isStale = false; // Flag for this specific effect run

  fetch(`/api/search?q=${query}`)
    .then(r => r.json())
    .then(data => {
      if (!isStale) { // Only update if this is still the latest request
        setResults(data);
      }
    });

  return () => {
    isStale = true; // Mark as stale when effect cleans up
  };
}, [query]);
```

### Fix 2: AbortController (Preferred)
```jsx
useEffect(() => {
  const controller = new AbortController();

  fetch(`/api/search?q=${query}`, { signal: controller.signal })
    .then(r => r.json())
    .then(data => setResults(data))
    .catch(err => {
      if (err.name !== 'AbortError') setError(err.message);
    });

  return () => controller.abort(); // Cancel previous request on new query
}, [query]);
```

### Fix 3: React Query (Handles Automatically)
```jsx
// React Query handles race conditions for you
const { data } = useQuery({
  queryKey: ['search', query],
  queryFn: ({ signal }) => // signal is provided automatically!
    fetch(`/api/search?q=${query}`, { signal }).then(r => r.json())
});
```

---

# SECTION 8: Testing

---

## Q60. React Testing Library Philosophy

### The Core Principle
> **"Test behavior, not implementation"**
> Write tests the way a user would use the app, not by testing internal component state.

```jsx
// ❌ Implementation-based testing (brittle)
// Tests: does the component have this specific state? Does this method get called?
expect(component.state('isOpen')).toBe(true); // Tests internals
expect(wrapper.find('Button').prop('onClick')).toBeDefined(); // Tests implementation

// ✅ Behavior-based testing (resilient)
// Tests: what does the user see and do?
userEvent.click(screen.getByRole('button', { name: 'Open Menu' }));
expect(screen.getByRole('menu')).toBeVisible(); // Tests what user sees
```

### Queries Priority (from RTL docs)
```
Best → Worst (for semantic testing):

1. getByRole       — 'button', 'textbox', 'heading' — most accessible
2. getByLabelText  — good for form inputs
3. getByPlaceholderText — ok
4. getByText       — for non-interactive elements
5. getByDisplayValue
6. getByAltText    — images
7. getByTitle
8. getByTestId     — last resort! (use data-testid only when necessary)
```

---

## Q61. Mocking API calls — MSW vs jest.mock

### `jest.mock` Approach
```jsx
// Direct mock of module
jest.mock('../api/users', () => ({
  fetchUsers: jest.fn().mockResolvedValue([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
  ])
}));

test('displays users', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});
```

### MSW (Mock Service Worker) Approach — Recommended
```jsx
// Intercepts at network level — much closer to reality
import { setupServer } from 'msw/node';
import { http, HttpResponse } from 'msw';

const server = setupServer(
  http.get('/api/users', () => {
    return HttpResponse.json([
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' }
    ]);
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test('displays users', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});

// Test error state by overriding per-test:
test('shows error on failure', async () => {
  server.use(
    http.get('/api/users', () => new HttpResponse(null, { status: 500 }))
  );
  render(<UserList />);
  expect(await screen.findByText('Error loading users')).toBeInTheDocument();
});
```

### Why MSW is Better
- Tests your actual fetch code, not a mock
- Works the same in tests and in the browser (Storybook, dev)
- More realistic — tests network error handling

---

## Q62. `render`, `screen`, `fireEvent`, `userEvent`

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('form interaction example', async () => {
  const user = userEvent.setup(); // ✅ Always use setup() in RTL v14+

  // render: mounts component into a fake DOM
  render(<LoginForm onSubmit={mockSubmit} />);

  // screen: query the rendered output
  const emailInput = screen.getByRole('textbox', { name: /email/i });
  const passwordInput = screen.getByLabelText(/password/i);
  const submitButton = screen.getByRole('button', { name: /log in/i });

  // userEvent: simulates real user interactions (preferred)
  await user.type(emailInput, 'alice@example.com'); // Types character by character
  await user.type(passwordInput, 'password123');
  await user.click(submitButton);

  // fireEvent: simulates DOM events directly (lower-level, less realistic)
  // Use when userEvent isn't available or for specific events
  fireEvent.change(emailInput, { target: { value: 'bob@example.com' } });

  expect(mockSubmit).toHaveBeenCalledWith({
    email: 'alice@example.com',
    password: 'password123'
  });
});
```

### `fireEvent` vs `userEvent`
| | `fireEvent` | `userEvent` |
|---|---|---|
| What it does | Dispatches a single DOM event | Simulates real user behavior (focus, keydown, keyup, input, etc.) |
| Realistic? | Less realistic | Much more realistic |
| Async? | Synchronous | Async (returns promises) |
| Use case | Simple event dispatch | Most form interactions |

---

## Q65. Snapshot Testing — pros, cons, when to avoid

```jsx
// Snapshot test
test('Button renders correctly', () => {
  const { container } = render(<Button variant="primary">Click me</Button>);
  expect(container).toMatchSnapshot(); // Creates/compares a snapshot file
});
```

### Pros
- Easy to create — almost zero effort
- Catches unexpected output changes
- Good for stable UI components (icon libraries, primitives)

### Cons (why teams avoid over-relying on them)
```
❌ "Snapshot tests fail → just update snapshots" culture
   People hit `--updateSnapshot` without thinking → defeats the purpose

❌ Large snapshots are unreadable
   Nobody can review a 400-line snapshot diff meaningfully

❌ Tests implementation, not behavior
   Renaming a CSS class breaks snapshots — was that a bug? No!

❌ False confidence
   Component works fine but snapshot test fails (or vice versa)

❌ Noisy diffs in PRs
```

### Better Alternative
```jsx
// Instead of snapshot: test specific behavior
test('Button shows loading spinner when loading prop is true', () => {
  render(<Button loading>Submit</Button>);
  expect(screen.getByRole('progressbar')).toBeInTheDocument();
  expect(screen.queryByText('Submit')).not.toBeInTheDocument();
});
```

### Interview-Ready Answer
*"Snapshots are useful for stable, dumb components where you want to catch accidental changes. But they're overused. Teams end up updating snapshots reflexively without thinking, the diffs become unreadable, and they test implementation details rather than behavior. I prefer explicit behavior-based assertions — they're more readable, more meaningful when they fail, and more resilient to refactoring."*

---

# SECTION 9: TypeScript with React

---

## Q66. Typing component props

```tsx
// Basic props
type ButtonProps = {
  label: string;         // Required
  onClick: () => void;   // Required function
  disabled?: boolean;    // Optional
  variant?: 'primary' | 'secondary' | 'danger'; // Union type
};

// With children
type CardProps = {
  title: string;
  children: React.ReactNode;  // Any renderable content
};

// Extending HTML element props (very common pattern)
type InputProps = React.InputHTMLAttributes<HTMLInputElement> & {
  label: string;
  error?: string;
  // All native input props (value, onChange, placeholder, etc.) are included!
};

function Input({ label, error, ...rest }: InputProps) {
  return (
    <div>
      <label>{label}</label>
      <input {...rest} />  {/* All native props forwarded */}
      {error && <span className="error">{error}</span>}
    </div>
  );
}
```

---

## Q67. Typing `useState` with complex types

```tsx
// Simple — TypeScript infers the type
const [count, setCount] = useState(0);     // number
const [name, setName] = useState('');       // string

// Union type — when initial state doesn't reveal full type
const [user, setUser] = useState<User | null>(null);
// Without <User | null>, TypeScript would infer `null` only!

// Complex object
type UserProfile = {
  id: string;
  name: string;
  email: string;
  preferences: {
    theme: 'light' | 'dark';
    notifications: boolean;
  };
};

const [profile, setProfile] = useState<UserProfile | null>(null);

// Array
const [items, setItems] = useState<Product[]>([]);
```

---

## Q68. Typing custom hooks

```tsx
// Return type explicitly typed
function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  const login = async (credentials: { email: string; password: string }) => {
    const user = await loginApi(credentials);
    setUser(user);
  };

  const logout = () => setUser(null);

  // TypeScript infers return type, but explicit is clearer:
  return { user, loading, login, logout } as const;
  // `as const` makes return type readonly tuple-like
}

// Using the hook — types are inferred
function App() {
  const { user, login, logout } = useAuth();
  // user: User | null ✅
  // login: (credentials: {email: string; password: string}) => Promise<void> ✅
}
```

---

## Q69. Generic Components

```tsx
// Without generics — loses type safety
function List({ items }: { items: any[] }) {
  return <ul>{items.map((item, i) => <li key={i}>{item.name}</li>)}</ul>;
}

// With generics — type-safe AND flexible
type ListProps<T> = {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
  keyExtractor: (item: T) => string;
};

function List<T>({ items, renderItem, keyExtractor }: ListProps<T>) {
  return (
    <ul>
      {items.map(item => (
        <li key={keyExtractor(item)}>{renderItem(item)}</li>
      ))}
    </ul>
  );
}

// Usage — TypeScript knows item type in each callback!
<List
  items={users}                              // users: User[]
  keyExtractor={user => user.id}             // user: User ✅
  renderItem={user => <UserCard user={user} />} // user: User ✅
/>

<List
  items={products}                           // products: Product[]
  keyExtractor={p => p.sku}                  // p: Product ✅
  renderItem={p => <ProductCard product={p} />} // p: Product ✅
/>
```

---

## Q70. `interface` vs `type` for props

```tsx
// Both work for props — subtle differences:

// interface — can be extended/merged (declaration merging)
interface ButtonProps {
  label: string;
}
interface ButtonProps { // ✅ Merges! (Declaration merging)
  onClick?: () => void;
}

// type — cannot be merged, but more flexible
type ButtonProps = {
  label: string;
};
// Cannot redeclare type in same scope ❌

// type can do things interface cannot:
type ID = string | number;           // Union type
type Nullable<T> = T | null;        // Generic utility
type EventOrNull = MouseEvent | null; // Union with non-object types
```

### The Real Answer for Interviews
```
In practice: negligible difference for component props.

Team convention matters more than the technical difference.
Many teams use `type` for everything to be consistent.
Some teams use `interface` for public API (components, hooks) and `type` for internal.

React's own types use both.
```

---

## Q71. Typing event handlers

```tsx
// Input change event
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

// Textarea
const handleTextArea = (e: React.ChangeEvent<HTMLTextAreaElement>) => { ... };

// Select
const handleSelect = (e: React.ChangeEvent<HTMLSelectElement>) => {
  setSelected(e.target.value);
};

// Click event
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.preventDefault();
};

// Form submit
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
};

// Keyboard
const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
  if (e.key === 'Enter') { ... }
};

// Drag
const handleDrop = (e: React.DragEvent<HTMLDivElement>) => { ... };

// In JSX — inline type inference
<input onChange={(e) => setValue(e.target.value)} />
// TypeScript infers e: React.ChangeEvent<HTMLInputElement> automatically!
// Only need explicit types when extracting handler to a variable
```

---

# SECTION 10: Architecture / System Design

---

## Q72. Large-scale React app structure

### Feature-Based Structure (Recommended at 4 YOE)
```
src/
├── features/               ← Organized by feature/domain
│   ├── auth/
│   │   ├── components/     AuthForm.tsx, LoginButton.tsx
│   │   ├── hooks/          useAuth.ts, useLogin.ts
│   │   ├── api/            authApi.ts
│   │   ├── store/          authSlice.ts
│   │   ├── types/          auth.types.ts
│   │   └── index.ts        ← Public API of this feature
│   ├── products/
│   │   ├── components/     ProductCard.tsx, ProductList.tsx
│   │   ├── hooks/          useProducts.ts, useProductDetail.ts
│   │   ├── api/            productsApi.ts
│   │   └── index.ts
│   └── cart/
│       ├── components/
│       ├── hooks/
│       └── index.ts
├── shared/                 ← Cross-feature shared code
│   ├── components/         Button.tsx, Modal.tsx, Input.tsx
│   ├── hooks/              useDebounce.ts, useClickOutside.ts
│   ├── utils/              formatDate.ts, validators.ts
│   └── types/              common.types.ts
├── pages/                  ← Route-level components (thin, compose features)
│   ├── HomePage.tsx
│   ├── ProductsPage.tsx
│   └── CartPage.tsx
├── app/                    ← App setup
│   ├── App.tsx
│   ├── router.tsx
│   └── store.ts
└── main.tsx
```

### The `index.ts` Barrel Exports Pattern
```tsx
// features/auth/index.ts
export { LoginForm } from './components/LoginForm';
export { useAuth } from './hooks/useAuth';
export type { User, AuthState } from './types/auth.types';

// Consuming in other features:
import { useAuth } from '@/features/auth'; // Clean import
// Not: import { useAuth } from '@/features/auth/hooks/useAuth'; // Leaks internals
```

---

## Q73. Auth/authorization flows

```tsx
// Protected Route pattern with React Router
function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const { user, isLoading } = useAuth();
  const location = useLocation();

  if (isLoading) return <Spinner />;

  if (!user) {
    // Redirect to login, save intended destination
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  return <>{children}</>;
}

// Router setup
function AppRouter() {
  return (
    <Routes>
      <Route path="/login" element={<LoginPage />} />
      <Route path="/" element={
        <ProtectedRoute>
          <Layout />
        </ProtectedRoute>
      }>
        <Route index element={<Dashboard />} />
        <Route path="profile" element={<Profile />} />
        <Route path="admin" element={
          <RequireRole role="admin">
            <AdminPanel />
          </RequireRole>
        } />
      </Route>
    </Routes>
  );
}

// Token refresh — axios interceptor pattern
axiosInstance.interceptors.response.use(
  response => response,
  async error => {
    if (error.response?.status === 401) {
      try {
        const newToken = await refreshToken();
        error.config.headers.Authorization = `Bearer ${newToken}`;
        return axiosInstance(error.config); // Retry original request
      } catch {
        logout(); // Refresh failed → log out
      }
    }
    return Promise.reject(error);
  }
);
```

---

## Q78. Optimizing initial load time

### Strategy 1: Code Splitting
```jsx
// Route-level splitting — biggest win
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Admin = lazy(() => import('./pages/Admin'));
// Each page is a separate chunk — only downloaded when needed
```

### Strategy 2: Bundle Analysis
```bash
# Identify what's making your bundle large
npx vite build --mode analyze  # Vite
# or
npx webpack-bundle-analyzer    # webpack
```

### Strategy 3: Tree Shaking
```jsx
// ❌ Imports entire lodash (100kb+)
import _ from 'lodash';
const result = _.debounce(fn, 300);

// ✅ Tree-shaking — imports only debounce
import debounce from 'lodash/debounce';
// or use native alternatives where possible
```

### Strategy 4: Lazy Load Images + Heavy Assets
```jsx
// Native lazy loading
<img src="large-image.jpg" loading="lazy" alt="..." />

// Component lazy loading with Intersection Observer
function LazyImage({ src, alt }) {
  const [loaded, setLoaded] = useState(false);
  const imgRef = useRef(null);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        setLoaded(true);
        observer.disconnect();
      }
    });
    if (imgRef.current) observer.observe(imgRef.current);
    return () => observer.disconnect();
  }, []);

  return <div ref={imgRef}>{loaded && <img src={src} alt={alt} />}</div>;
}
```

### Strategy 5: Preloading Critical Resources
```html
<!-- Preload critical fonts/assets -->
<link rel="preload" href="/fonts/main.woff2" as="font" crossorigin />

<!-- Prefetch likely next pages -->
<link rel="prefetch" href="/heavy-dashboard-chunk.js" />
```

---

## Q80. CSR vs SSR vs SSG vs ISR

### Four Rendering Strategies

```
CSR (Client-Side Rendering):
- Server sends: minimal HTML + JS bundle
- Browser downloads JS → runs React → renders page
- Good for: dashboards, SPAs, admin panels (auth-required, not SEO critical)
- Bad for: SEO, initial load performance

SSR (Server-Side Rendering):
- Server runs React on every request → sends full HTML
- Browser displays HTML immediately → React hydrates
- Good for: pages with user-specific data, SEO important, dynamic content
- Bad for: server load (renders every request), slower TTFB on heavy pages

SSG (Static Site Generation):
- React runs at BUILD time → generates static HTML files
- Served from CDN → ultra-fast
- Good for: blogs, marketing pages, docs, anything that doesn't change per-user
- Bad for: frequently changing data, personalized content

ISR (Incremental Static Regeneration) — Next.js only:
- Like SSG but with automatic background regeneration
- Page is static → fast
- Regenerates in background every N seconds when visited
- Good for: e-commerce product pages, news articles — static speed + fresh data
```

### Next.js Code Examples
```jsx
// CSR — in any component
'use client';
function UserDashboard() {
  const { data } = useSWR('/api/user', fetcher); // Client-side fetch
}

// SSR — in Next.js App Router
async function UserPage({ params }) {
  const user = await fetchUser(params.id); // Runs on server, every request
  return <UserProfile user={user} />;
}

// SSG — static, built at build time
async function BlogPost({ params }) {
  const post = await getPost(params.slug);
  return <Article post={post} />;
}
export function generateStaticParams() {
  return posts.map(p => ({ slug: p.slug })); // Pre-generate these paths
}

// ISR — static + periodically updated
export const revalidate = 3600; // Regenerate at most once per hour

async function ProductPage({ params }) {
  const product = await getProduct(params.id);
  return <ProductDetails product={product} />;
}
```

### Decision Framework
```
Is content user-specific?          → SSR or CSR
Is content rarely changing?        → SSG
Changes periodically but not per user? → ISR
SEO required?                      → SSR or SSG
Behind auth (no SEO)?              → CSR
Ultra-fast with fresh-ish data?    → ISR
```

---

# SECTION 11: Practical Coding — Key Patterns

---

## Debounced Search Input
```jsx
function DebouncedSearch() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (!query.trim()) {
      setResults([]);
      return;
    }

    setLoading(true);
    const timer = setTimeout(async () => {
      try {
        const data = await searchApi(query);
        setResults(data);
      } finally {
        setLoading(false);
      }
    }, 300);

    return () => clearTimeout(timer); // Cleanup cancels pending timer
  }, [query]);

  return (
    <div>
      <input
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      {loading && <Spinner />}
      {results.map(r => <ResultItem key={r.id} item={r} />)}
    </div>
  );
}
```

---

## Modal with Portal
```jsx
function Modal({ isOpen, onClose, children }) {
  useEffect(() => {
    if (!isOpen) return;

    const handleEscape = (e) => {
      if (e.key === 'Escape') onClose();
    };

    document.addEventListener('keydown', handleEscape);
    document.body.style.overflow = 'hidden'; // Prevent background scroll

    return () => {
      document.removeEventListener('keydown', handleEscape);
      document.body.style.overflow = ''; // Restore
    };
  }, [isOpen, onClose]);

  if (!isOpen) return null;

  return ReactDOM.createPortal(
    // Portal renders outside normal React tree (to document.body)
    // This ensures modal is above all other content in z-index
    <div className="modal-overlay" onClick={onClose}>
      <div
        className="modal-content"
        onClick={e => e.stopPropagation()} // Don't close when clicking modal
        role="dialog"
        aria-modal="true"
      >
        <button onClick={onClose} aria-label="Close modal">✕</button>
        {children}
      </div>
    </div>,
    document.body // Render into document.body, not parent DOM node
  );
}
```

---

## Mini Redux with Context + useReducer
```jsx
// state.js
const initialState = { count: 0, user: null };

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT': return { ...state, count: state.count + 1 };
    case 'SET_USER':  return { ...state, user: action.payload };
    default:          return state;
  }
}

const StoreContext = createContext(null);

export function StoreProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  // Memoize so context value is stable when state doesn't change
  const value = useMemo(() => ({ state, dispatch }), [state]);

  return (
    <StoreContext.Provider value={value}>
      {children}
    </StoreContext.Provider>
  );
}

export function useStore() {
  const context = useContext(StoreContext);
  if (!context) throw new Error('useStore must be used within StoreProvider');
  return context;
}

// Usage anywhere in tree
function Counter() {
  const { state, dispatch } = useStore();
  return (
    <button onClick={() => dispatch({ type: 'INCREMENT' })}>
      Count: {state.count}
    </button>
  );
}
```

---

# SECTION 12: Rapid-Fire Gotchas

---

## Why calling `setState` in a loop doesn't re-render multiple times?

```jsx
function handleClick() {
  setCount(1);  // Doesn't render yet
  setCount(2);  // Doesn't render yet
  setCount(3);  // Doesn't render yet
  // Event handler finishes
  // React processes all updates TOGETHER → ONE re-render with count=3
}
// React batches all setState calls within the same event handler
// This is automatic batching — in React 18, works everywhere
```

---

## Functional updater vs direct state value — where it bites you

```jsx
// ❌ Problematic — captures count from closure
function handleClick() {
  setCount(count + 1); // count = 0 here
  setCount(count + 1); // count = 0 here (still!)
  setCount(count + 1); // count = 0 here (still!)
  // Result: count = 1 (not 3! All used the same closure value)
}

// ✅ Functional updater — always gets latest state
function handleClick() {
  setCount(prev => prev + 1); // prev = 0, returns 1
  setCount(prev => prev + 1); // prev = 1, returns 2
  setCount(prev => prev + 1); // prev = 2, returns 3
  // Result: count = 3 ✅
}
// React queues these and applies them sequentially
```

---

## Why does `useEffect` fire twice in development (StrictMode React 18)?

```jsx
// StrictMode in React 18 intentionally:
// 1. Mounts component
// 2. Unmounts it (runs cleanup)
// 3. Remounts it (runs effect again)
// → Effect fires TWICE in development

// Purpose: verify your cleanup function properly undoes the effect
// If your effect can't safely run twice → you're missing cleanup!

useEffect(() => {
  const sub = subscribeToData();
  return () => sub.unsubscribe(); // If cleanup works → double-firing is harmless
}, []);

// This ONLY happens in development with StrictMode
// Production: fires once as expected
```

---

## What is Fiber and why did React move away from the stack reconciler?

### Old Stack Reconciler Problems
```
Old Reconciler (React 15 and earlier):
- Used JavaScript call stack
- Recursive tree traversal (like depth-first search)
- SYNCHRONOUS — once started, couldn't be interrupted
- If reconciliation took 200ms → UI frozen for 200ms
- No way to prioritize urgent updates
- Animations, user input suffered during expensive updates
```

### Fiber Architecture
```
Fiber (React 16+):
- Each component = a "fiber node" (a JavaScript object)
- Linked list of fiber nodes instead of call stack
- Work is done in small increments
- Can be PAUSED and RESUMED between frames (16ms chunks)
- Each fiber has a priority level
- Browser can handle input/animation between fiber work units
- Foundation for: concurrent mode, Suspense, useTransition
```

### Simple Analogy
> "Old React was like reading a book cover-to-cover without stopping — you couldn't answer the phone until you finished. Fiber is like using bookmarks — you can stop, handle something urgent, and continue exactly where you left off."

---

## Keys must be stable — what React is protecting internally

```
When a key changes:
1. React assumes it's a DIFFERENT component instance
2. React UNMOUNTS the old fiber (destroys state, refs, effects)
3. React MOUNTS a NEW fiber (initializes fresh state)

With unstable keys (like Math.random()):
- Every render = different key = unmount + remount
- All state is lost
- All effects re-run
- Massive performance hit
- Visible flicker

React is protecting fiber REUSE.
Stable keys = same fiber = React updates props instead of remounting.
```

---

## Why shouldn't you call hooks inside `if` statements? (Final summary)

```jsx
// React's hook state is stored as a linked list:
// Hook 1 → Hook 2 → Hook 3 → null

// If you skip a hook conditionally:
// Render 1 (condition true):  Hook 1 → Hook 2 → Hook 3
// Render 2 (condition false): Hook 1 → Hook 3
// React reads: Hook 1=ok, Hook 3 reads from Hook 2's slot → WRONG STATE

// The rule enforces: hooks always run in the same order
// Same order = same index = correct state mapping

// React even enforces this at runtime:
// "React Hook useState is called conditionally. React Hooks must be called
//  in the exact same order in every component render."
```

---

# Quick Reference Cheat Sheet

```
Virtual DOM → JS copy of DOM, diffed to find minimum changes
Keys → map fiber identity across renders; unstable = remount
JSX → React.createElement / jsx-runtime (React 17+)
Controlled → React owns state; Uncontrolled → DOM owns state

useEffect(() => {}, [])    → componentDidMount
useEffect(() => {}, [dep]) → componentDidUpdate
return () => cleanup       → componentWillUnmount

useLayoutEffect → before paint, DOM measurements
Stale closure → captured var not in deps → use functional updater or ref

React.memo → stops component re-render if props unchanged
useMemo → caches computed value
useCallback → caches function reference

React 18 changes:
  createRoot → concurrent mode on
  Automatic batching → everywhere, not just event handlers
  useTransition → non-urgent state updates
  useDeferredValue → defer a value you don't control
  Streaming SSR → send HTML in chunks as ready

RSC → server-only components, no JS sent to client
SSG → build-time render → CDN → fastest
ISR → SSG + background regeneration every N seconds

Error Boundaries don't catch: event handlers, async, SSR
Race conditions → AbortController or ignore-flag pattern
Keys stable rule → protects fiber reuse
StrictMode double-invoke → verifies cleanup correctness
```
