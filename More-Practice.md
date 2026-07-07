Below is a **React interview coding practice sheet for 4 years experience**. Practice these one by one and explain the **problem, approach, code, and follow-up improvements**.

---

# 1. Counter with Hooks

**Question:** Build a counter with increment, decrement, and reset.

```tsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState<number>(0);

  return (
    <div>
      <h2>Count: {count}</h2>

      <button onClick={() => setCount(prev => prev + 1)}>Increment</button>
      <button onClick={() => setCount(prev => prev - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**Explain:**

> I used `useState` to manage local component state. I used functional updates because the next value depends on the previous state.

---

# 2. Todo App with `useReducer`

**Question:** Add, delete, and complete todos.

```tsx
import { useReducer, useState } from "react";

type Todo = {
  id: number;
  title: string;
  completed: boolean;
};

type Action =
  | { type: "ADD"; payload: string }
  | { type: "DELETE"; payload: number }
  | { type: "TOGGLE"; payload: number };

function todoReducer(state: Todo[], action: Action): Todo[] {
  switch (action.type) {
    case "ADD":
      return [
        ...state,
        {
          id: Date.now(),
          title: action.payload,
          completed: false,
        },
      ];

    case "DELETE":
      return state.filter(todo => todo.id !== action.payload);

    case "TOGGLE":
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );

    default:
      return state;
  }
}

export default function TodoApp() {
  const [todos, dispatch] = useReducer(todoReducer, []);
  const [title, setTitle] = useState("");

  const handleAdd = () => {
    if (!title.trim()) return;
    dispatch({ type: "ADD", payload: title });
    setTitle("");
  };

  return (
    <div>
      <input
        value={title}
        onChange={e => setTitle(e.target.value)}
        placeholder="Enter todo"
      />

      <button onClick={handleAdd}>Add</button>

      {todos.map(todo => (
        <div key={todo.id}>
          <span
            style={{
              textDecoration: todo.completed ? "line-through" : "none",
            }}
          >
            {todo.title}
          </span>

          <button onClick={() => dispatch({ type: "TOGGLE", payload: todo.id })}>
            Complete
          </button>

          <button onClick={() => dispatch({ type: "DELETE", payload: todo.id })}>
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}
```

**Explain:**

> I used `useReducer` because todo logic has multiple actions. It keeps state update logic separate and predictable.

---

# 3. Search Filter with `useMemo`

**Question:** Filter users by search text.

```tsx
import { useMemo, useState } from "react";

type User = {
  id: number;
  name: string;
};

const users: User[] = [
  { id: 1, name: "Anand" },
  { id: 2, name: "Aman" },
  { id: 3, name: "Rahul" },
];

export default function SearchUsers() {
  const [search, setSearch] = useState("");

  const filteredUsers = useMemo(() => {
    return users.filter(user =>
      user.name.toLowerCase().includes(search.toLowerCase())
    );
  }, [search]);

  return (
    <div>
      <input
        value={search}
        onChange={e => setSearch(e.target.value)}
        placeholder="Search user"
      />

      {filteredUsers.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

**Explain:**

> I used `useMemo` to avoid recalculating filtered users on every render unless search changes.

---

# 4. Debounced Search

**Question:** Call API only after user stops typing.

```tsx
import { useEffect, useState } from "react";

export default function DebouncedSearch() {
  const [search, setSearch] = useState("");

  useEffect(() => {
    if (!search) return;

    const timer = setTimeout(() => {
      console.log("API call with:", search);
    }, 500);

    return () => clearTimeout(timer);
  }, [search]);

  return (
    <input
      value={search}
      onChange={e => setSearch(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

**Explain:**

> Debouncing prevents unnecessary API calls on every keystroke. Cleanup clears the previous timer before setting a new one.

---

# 5. Custom `useDebounce` Hook

```tsx
import { useEffect, useState } from "react";

function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

export default function SearchExample() {
  const [search, setSearch] = useState("");
  const debouncedSearch = useDebounce(search, 500);

  useEffect(() => {
    if (debouncedSearch) {
      console.log("API call:", debouncedSearch);
    }
  }, [debouncedSearch]);

  return (
    <input
      value={search}
      onChange={e => setSearch(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

**Explain:**

> I created a reusable hook so the debounce logic can be shared across multiple components.

---

# 6. API Fetch with Loading, Error, and Cleanup

```tsx
import { useEffect, useState } from "react";

type User = {
  id: number;
  name: string;
};

export default function UsersList() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  useEffect(() => {
    const controller = new AbortController();

    async function fetchUsers() {
      try {
        setLoading(true);
        setError("");

        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
          { signal: controller.signal }
        );

        if (!response.ok) {
          throw new Error("Failed to fetch users");
        }

        const data: User[] = await response.json();
        setUsers(data);
      } catch (err) {
        if (err instanceof Error && err.name !== "AbortError") {
          setError(err.message);
        }
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();

    return () => controller.abort();
  }, []);

  if (loading) return <h3>Loading...</h3>;
  if (error) return <h3>{error}</h3>;

  return (
    <div>
      {users.map(user => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}
```

**Explain:**

> I used `AbortController` to cancel the API request if the component unmounts.

---

# 7. Pagination

```tsx
import { useState } from "react";

const items = Array.from({ length: 50 }, (_, index) => `Item ${index + 1}`);

export default function PaginationExample() {
  const [page, setPage] = useState(1);
  const pageSize = 10;

  const totalPages = Math.ceil(items.length / pageSize);

  const currentItems = items.slice((page - 1) * pageSize, page * pageSize);

  return (
    <div>
      {currentItems.map(item => (
        <p key={item}>{item}</p>
      ))}

      <button disabled={page === 1} onClick={() => setPage(prev => prev - 1)}>
        Previous
      </button>

      <span> Page {page} of {totalPages} </span>

      <button
        disabled={page === totalPages}
        onClick={() => setPage(prev => prev + 1)}
      >
        Next
      </button>
    </div>
  );
}
```

**Explain:**

> Pagination improves performance and user experience by showing limited records per page.

---

# 8. Sorting Table

```tsx
import { useState } from "react";

type User = {
  id: number;
  name: string;
  age: number;
};

const initialUsers: User[] = [
  { id: 1, name: "Anand", age: 28 },
  { id: 2, name: "Aman", age: 24 },
  { id: 3, name: "Rahul", age: 30 },
];

export default function SortTable() {
  const [users, setUsers] = useState(initialUsers);
  const [sortOrder, setSortOrder] = useState<"asc" | "desc">("asc");

  const handleSort = () => {
    const sorted = [...users].sort((a, b) =>
      sortOrder === "asc" ? a.age - b.age : b.age - a.age
    );

    setUsers(sorted);
    setSortOrder(sortOrder === "asc" ? "desc" : "asc");
  };

  return (
    <table border={1}>
      <thead>
        <tr>
          <th>Name</th>
          <th onClick={handleSort}>Age</th>
        </tr>
      </thead>

      <tbody>
        {users.map(user => (
          <tr key={user.id}>
            <td>{user.name}</td>
            <td>{user.age}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

**Explain:**

> I copied the array before sorting because `sort()` mutates the original array.

---

# 9. Infinite Scroll with Intersection Observer

```tsx
import { useEffect, useRef, useState } from "react";

export default function InfiniteScroll() {
  const [items, setItems] = useState<number[]>(Array.from({ length: 20 }, (_, i) => i + 1));
  const loaderRef = useRef<HTMLDivElement | null>(null);

  const loadMore = () => {
    setItems(prev => [
      ...prev,
      ...Array.from({ length: 10 }, (_, i) => prev.length + i + 1),
    ]);
  };

  useEffect(() => {
    const observer = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting) {
        loadMore();
      }
    });

    if (loaderRef.current) {
      observer.observe(loaderRef.current);
    }

    return () => observer.disconnect();
  }, []);

  return (
    <div>
      {items.map(item => (
        <p key={item}>Item {item}</p>
      ))}

      <div ref={loaderRef}>Loading more...</div>
    </div>
  );
}
```

**Explain:**

> Intersection Observer detects when the user reaches the bottom and loads more records.

---

# 10. Parent to Child and Child to Parent

```tsx
type ChildProps = {
  title: string;
  onSend: (message: string) => void;
};

function Child({ title, onSend }: ChildProps) {
  return (
    <div>
      <h3>{title}</h3>
      <button onClick={() => onSend("Message from child")}>Send to Parent</button>
    </div>
  );
}

export default function Parent() {
  const handleChildMessage = (message: string) => {
    console.log(message);
  };

  return <Child title="Hello Child" onSend={handleChildMessage} />;
}
```

**Explain:**

> Parent passes data using props. Child communicates back using callback functions.

---

# 11. `React.memo` and `useCallback`

```tsx
import React, { useCallback, useState } from "react";

type ChildProps = {
  onClick: () => void;
};

const Child = React.memo(({ onClick }: ChildProps) => {
  console.log("Child rendered");

  return <button onClick={onClick}>Child Button</button>;
});

export default function MemoExample() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return (
    <div>
      <h2>{count}</h2>

      <button onClick={() => setCount(prev => prev + 1)}>Increment</button>

      <Child onClick={handleClick} />
    </div>
  );
}
```

**Explain:**

> `React.memo` prevents unnecessary child re-rendering. `useCallback` keeps the function reference stable.

---

# 12. `usePrevious` Custom Hook

```tsx
import { useEffect, useRef, useState } from "react";

function usePrevious<T>(value: T): T | undefined {
  const ref = useRef<T>();

  useEffect(() => {
    ref.current = value;
  }, [value]);

  return ref.current;
}

export default function PreviousExample() {
  const [count, setCount] = useState(0);
  const previousCount = usePrevious(count);

  return (
    <div>
      <h3>Current: {count}</h3>
      <h3>Previous: {previousCount}</h3>

      <button onClick={() => setCount(prev => prev + 1)}>Increment</button>
    </div>
  );
}
```

**Explain:**

> `useRef` stores values without causing re-render. That is why it is useful for previous value tracking.

---

# 13. `forwardRef`

```tsx
import { forwardRef, useRef } from "react";

type InputProps = {
  placeholder?: string;
};

const CustomInput = forwardRef<HTMLInputElement, InputProps>(
  ({ placeholder }, ref) => {
    return <input ref={ref} placeholder={placeholder} />;
  }
);

export default function ForwardRefExample() {
  const inputRef = useRef<HTMLInputElement | null>(null);

  return (
    <div>
      <CustomInput ref={inputRef} placeholder="Enter name" />

      <button onClick={() => inputRef.current?.focus()}>
        Focus Input
      </button>
    </div>
  );
}
```

**Explain:**

> `forwardRef` allows the parent component to access a child DOM element.

---

# 14. Modal Component

```tsx
import { useEffect } from "react";

type ModalProps = {
  open: boolean;
  onClose: () => void;
};

export default function Modal({ open, onClose }: ModalProps) {
  useEffect(() => {
    const handleEsc = (event: KeyboardEvent) => {
      if (event.key === "Escape") {
        onClose();
      }
    };

    document.addEventListener("keydown", handleEsc);

    return () => document.removeEventListener("keydown", handleEsc);
  }, [onClose]);

  if (!open) return null;

  return (
    <div
      onClick={onClose}
      style={{
        position: "fixed",
        inset: 0,
        background: "rgba(0,0,0,0.5)",
      }}
    >
      <div
        onClick={e => e.stopPropagation()}
        style={{
          background: "white",
          padding: 20,
          margin: "100px auto",
          width: 300,
        }}
      >
        <h2>Modal</h2>
        <button onClick={onClose}>Close</button>
      </div>
    </div>
  );
}
```

**Explain:**

> Modal closes on ESC and overlay click. `stopPropagation` prevents closing when clicking inside modal content.

---

# 15. Tabs Component

```tsx
import { useState } from "react";

const tabs = ["Profile", "Settings", "Security"];

export default function Tabs() {
  const [activeTab, setActiveTab] = useState("Profile");

  return (
    <div>
      {tabs.map(tab => (
        <button key={tab} onClick={() => setActiveTab(tab)}>
          {tab}
        </button>
      ))}

      <div>
        {activeTab === "Profile" && <h3>Profile Content</h3>}
        {activeTab === "Settings" && <h3>Settings Content</h3>}
        {activeTab === "Security" && <h3>Security Content</h3>}
      </div>
    </div>
  );
}
```

**Explain:**

> Tabs are controlled by active state. Based on selected tab, we conditionally render content.

---

# 16. Accordion

```tsx
import { useState } from "react";

const faqs = [
  { id: 1, question: "What is React?", answer: "React is a UI library." },
  { id: 2, question: "What is JSX?", answer: "JSX is syntax extension for JS." },
];

export default function Accordion() {
  const [openId, setOpenId] = useState<number | null>(null);

  return (
    <div>
      {faqs.map(item => (
        <div key={item.id}>
          <h3 onClick={() => setOpenId(openId === item.id ? null : item.id)}>
            {item.question}
          </h3>

          {openId === item.id && <p>{item.answer}</p>}
        </div>
      ))}
    </div>
  );
}
```

**Explain:**

> I store only one active accordion ID, so only one item opens at a time.

---

# 17. File Upload with Preview

```tsx
import { useState } from "react";

export default function FileUploadPreview() {
  const [preview, setPreview] = useState<string>("");

  const handleFileChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const file = event.target.files?.[0];

    if (!file) return;

    if (!file.type.startsWith("image/")) {
      alert("Only images allowed");
      return;
    }

    setPreview(URL.createObjectURL(file));
  };

  return (
    <div>
      <input type="file" onChange={handleFileChange} />

      {preview && <img src={preview} alt="Preview" width={200} />}
    </div>
  );
}
```

**Explain:**

> I validate file type and use `URL.createObjectURL` to show preview.

---

# 18. Dynamic Form Fields

```tsx
import { useState } from "react";

export default function DynamicForm() {
  const [phones, setPhones] = useState<string[]>([""]);

  const handleChange = (index: number, value: string) => {
    const updatedPhones = [...phones];
    updatedPhones[index] = value;
    setPhones(updatedPhones);
  };

  const addPhone = () => {
    setPhones(prev => [...prev, ""]);
  };

  const removePhone = (index: number) => {
    setPhones(prev => prev.filter((_, i) => i !== index));
  };

  return (
    <div>
      {phones.map((phone, index) => (
        <div key={index}>
          <input
            value={phone}
            onChange={e => handleChange(index, e.target.value)}
            placeholder="Phone number"
          />

          <button onClick={() => removePhone(index)}>Remove</button>
        </div>
      ))}

      <button onClick={addPhone}>Add Phone</button>
    </div>
  );
}
```

**Explain:**

> Dynamic forms are managed using array state. I avoid direct mutation by copying the array first.

---

# 19. Theme Toggle with Context

```tsx
import { createContext, useContext, useState } from "react";

type ThemeContextType = {
  theme: "light" | "dark";
  toggleTheme: () => void;
};

const ThemeContext = createContext<ThemeContextType | null>(null);

function useThemeContext() {
  const context = useContext(ThemeContext);

  if (!context) {
    throw new Error("useThemeContext must be used inside ThemeProvider");
  }

  return context;
}

function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<"light" | "dark">("light");

  const toggleTheme = () => {
    setTheme(prev => (prev === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemeButton() {
  const { theme, toggleTheme } = useThemeContext();

  return (
    <button onClick={toggleTheme}>
      Current Theme: {theme}
    </button>
  );
}

export default function App() {
  return (
    <ThemeProvider>
      <ThemeButton />
    </ThemeProvider>
  );
}
```

**Explain:**

> Context is useful when data is needed by many components without prop drilling.

---

# 20. Authentication with Protected Route

```tsx
import { Navigate } from "react-router-dom";

type ProtectedRouteProps = {
  children: React.ReactNode;
};

export default function ProtectedRoute({ children }: ProtectedRouteProps) {
  const token = localStorage.getItem("token");

  if (!token) {
    return <Navigate to="/login" replace />;
  }

  return <>{children}</>;
}
```

Usage:

```tsx
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

**Explain:**

> Protected route checks authentication before allowing access to private pages.

---

# 21. RTK Query CRUD Example

```tsx
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

type User = {
  id: number;
  name: string;
};

export const userApi = createApi({
  reducerPath: "userApi",

  baseQuery: fetchBaseQuery({
    baseUrl: "https://jsonplaceholder.typicode.com",
  }),

  tagTypes: ["Users"],

  endpoints: builder => ({
    getUsers: builder.query<User[], void>({
      query: () => "/users",
      providesTags: ["Users"],
    }),

    addUser: builder.mutation<User, Partial<User>>({
      query: body => ({
        url: "/users",
        method: "POST",
        body,
      }),
      invalidatesTags: ["Users"],
    }),

    deleteUser: builder.mutation<void, number>({
      query: id => ({
        url: `/users/${id}`,
        method: "DELETE",
      }),
      invalidatesTags: ["Users"],
    }),
  }),
});

export const {
  useGetUsersQuery,
  useAddUserMutation,
  useDeleteUserMutation,
} = userApi;
```

Component:

```tsx
export default function Users() {
  const { data, isLoading, error } = useGetUsersQuery();
  const [addUser] = useAddUserMutation();
  const [deleteUser] = useDeleteUserMutation();

  if (isLoading) return <h3>Loading...</h3>;
  if (error) return <h3>Something went wrong</h3>;

  return (
    <div>
      <button onClick={() => addUser({ name: "Anand" })}>
        Add User
      </button>

      {data?.map(user => (
        <div key={user.id}>
          {user.name}

          <button onClick={() => deleteUser(user.id)}>
            Delete
          </button>
        </div>
      ))}
    </div>
  );
}
```

**Explain:**

> RTK Query handles API calls, caching, loading state, error state, and automatic refetching using tags.

---

# 22. Virtual List Logic

```tsx
import { useState } from "react";

const ITEM_HEIGHT = 40;
const CONTAINER_HEIGHT = 400;

const items = Array.from({ length: 10000 }, (_, index) => `Item ${index + 1}`);

export default function VirtualList() {
  const [scrollTop, setScrollTop] = useState(0);

  const startIndex = Math.floor(scrollTop / ITEM_HEIGHT);
  const visibleCount = Math.ceil(CONTAINER_HEIGHT / ITEM_HEIGHT);
  const endIndex = startIndex + visibleCount;

  const visibleItems = items.slice(startIndex, endIndex);

  return (
    <div
      style={{
        height: CONTAINER_HEIGHT,
        overflow: "auto",
        position: "relative",
        border: "1px solid #ccc",
      }}
      onScroll={e => setScrollTop(e.currentTarget.scrollTop)}
    >
      <div style={{ height: items.length * ITEM_HEIGHT, position: "relative" }}>
        {visibleItems.map((item, index) => {
          const actualIndex = startIndex + index;

          return (
            <div
              key={item}
              style={{
                position: "absolute",
                top: actualIndex * ITEM_HEIGHT,
                height: ITEM_HEIGHT,
              }}
            >
              {item}
            </div>
          );
        })}
      </div>
    </div>
  );
}
```

**Explain:**

> Virtualization renders only visible items instead of all 10,000 rows. This improves performance.

---

# 23. OTP Input

```tsx
import { useRef, useState } from "react";

export default function OTPInput() {
  const [otp, setOtp] = useState<string[]>(Array(6).fill(""));
  const inputRefs = useRef<Array<HTMLInputElement | null>>([]);

  const handleChange = (index: number, value: string) => {
    if (!/^\d?$/.test(value)) return;

    const updatedOtp = [...otp];
    updatedOtp[index] = value;
    setOtp(updatedOtp);

    if (value && index < 5) {
      inputRefs.current[index + 1]?.focus();
    }
  };

  const handleKeyDown = (
    index: number,
    event: React.KeyboardEvent<HTMLInputElement>
  ) => {
    if (event.key === "Backspace" && !otp[index] && index > 0) {
      inputRefs.current[index - 1]?.focus();
    }
  };

  return (
    <div>
      {otp.map((value, index) => (
        <input
          key={index}
          ref={el => {
            inputRefs.current[index] = el;
          }}
          value={value}
          maxLength={1}
          onChange={e => handleChange(index, e.target.value)}
          onKeyDown={e => handleKeyDown(index, e)}
          style={{ width: 40, height: 40, marginRight: 8, textAlign: "center" }}
        />
      ))}

      <p>OTP: {otp.join("")}</p>
    </div>
  );
}
```

**Explain:**

> I used array state for OTP values and refs to move focus automatically.

---

# 24. Recursive Tree View

```tsx
type TreeNode = {
  id: number;
  name: string;
  children?: TreeNode[];
};

const data: TreeNode[] = [
  {
    id: 1,
    name: "src",
    children: [
      { id: 2, name: "components" },
      {
        id: 3,
        name: "pages",
        children: [{ id: 4, name: "Home.tsx" }],
      },
    ],
  },
];

function TreeItem({ node }: { node: TreeNode }) {
  return (
    <div style={{ marginLeft: 20 }}>
      <p>{node.name}</p>

      {node.children?.map(child => (
        <TreeItem key={child.id} node={child} />
      ))}
    </div>
  );
}

export default function TreeView() {
  return (
    <div>
      {data.map(node => (
        <TreeItem key={node.id} node={node} />
      ))}
    </div>
  );
}
```

**Explain:**

> Tree structures are best handled using recursion because each node can have children of the same shape.

---

# 25. Reusable Table Component

```tsx
type Column<T> = {
  key: keyof T;
  label: string;
};

type TableProps<T> = {
  data: T[];
  columns: Column<T>[];
};

function ReusableTable<T extends { id: number }>({
  data,
  columns,
}: TableProps<T>) {
  return (
    <table border={1}>
      <thead>
        <tr>
          {columns.map(column => (
            <th key={String(column.key)}>{column.label}</th>
          ))}
        </tr>
      </thead>

      <tbody>
        {data.map(row => (
          <tr key={row.id}>
            {columns.map(column => (
              <td key={String(column.key)}>
                {String(row[column.key])}
              </td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
  );
}

type User = {
  id: number;
  name: string;
  email: string;
};

export default function TableExample() {
  const users: User[] = [
    { id: 1, name: "Anand", email: "anand@test.com" },
  ];

  const columns: Column<User>[] = [
    { key: "name", label: "Name" },
    { key: "email", label: "Email" },
  ];

  return <ReusableTable data={users} columns={columns} />;
}
```

**Explain:**

> I used generics to make the table reusable for different data types.

---

# 26. Error Boundary

```tsx
import React from "react";

type State = {
  hasError: boolean;
};

export default class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  State
> {
  state: State = {
    hasError: false,
  };

  static getDerivedStateFromError(): State {
    return { hasError: true };
  }

  componentDidCatch(error: Error) {
    console.log("Error caught:", error);
  }

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    return this.props.children;
  }
}
```

**Explain:**

> Error boundaries catch rendering errors in child components and prevent the whole app from crashing.

---

# 27. Stopwatch

```tsx
import { useEffect, useState } from "react";

export default function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const [running, setRunning] = useState(false);

  useEffect(() => {
    if (!running) return;

    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, [running]);

  return (
    <div>
      <h2>{seconds}s</h2>

      <button onClick={() => setRunning(true)}>Start</button>
      <button onClick={() => setRunning(false)}>Pause</button>
      <button
        onClick={() => {
          setRunning(false);
          setSeconds(0);
        }}
      >
        Reset
      </button>
    </div>
  );
}
```

**Explain:**

> I clear interval in cleanup to avoid memory leaks.

---

# 28. Lazy Loading

```tsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./Dashboard"));

export default function App() {
  return (
    <Suspense fallback={<h3>Loading page...</h3>}>
      <Dashboard />
    </Suspense>
  );
}
```

**Explain:**

> Lazy loading reduces initial bundle size by loading components only when needed.

---

# 29. Multi-step Form

```tsx
import { useState } from "react";

export default function MultiStepForm() {
  const [step, setStep] = useState(1);

  return (
    <div>
      {step === 1 && <input placeholder="Name" />}
      {step === 2 && <input placeholder="Email" />}
      {step === 3 && <input placeholder="Password" />}

      <button disabled={step === 1} onClick={() => setStep(prev => prev - 1)}>
        Back
      </button>

      <button disabled={step === 3} onClick={() => setStep(prev => prev + 1)}>
        Next
      </button>
    </div>
  );
}
```

**Explain:**

> Multi-step forms divide large forms into smaller steps and manage active step using state.

---

# 30. Drag and Drop Reorder Logic

Even if you use `@hello-pangea/dnd`, interviewer may ask the reorder logic.

```tsx
type Item = {
  id: number;
  name: string;
};

function reorder(list: Item[], startIndex: number, endIndex: number): Item[] {
  const result = [...list];
  const [removed] = result.splice(startIndex, 1);
  result.splice(endIndex, 0, removed);

  return result;
}
```

**Explain:**

> I remove the dragged item from the old index and insert it at the new index.

---

# Most Important Practice Order

Prepare in this order:

1. Todo with `useReducer`
2. Debounced search
3. API fetch with loading/error
4. Pagination + sorting table
5. RTK Query CRUD
6. Custom hooks
7. React.memo + useCallback
8. Infinite scroll
9. Virtual list
10. OTP input
11. Dynamic form
12. Protected route
13. Modal
14. Tree view recursion
15. Reusable generic table

For your experience level, interviewers will focus more on **clean code, TypeScript, state management, performance, API handling, and reusable components** than only basic UI tasks.
