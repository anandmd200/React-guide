# 🚀 Complete Frontend Development Mastery: A Step-by-Step Tutorial

## Roadmap Overview

```
Phase 1: Foundations (Weeks 1-4)
  └── HTML, CSS, JavaScript Basics

Phase 2: JavaScript Deep Dive (Weeks 5-8)
  └── Advanced JS, ES6+, Async Programming

Phase 3: TypeScript (Weeks 9-11)
  └── Types, Interfaces, Generics

Phase 4: React Fundamentals (Weeks 12-16)
  └── Components, State, Props, Hooks

Phase 5: React Advanced (Weeks 17-20)
  └── Context, Reducers, Performance

Phase 6: Ecosystem & Tools (Weeks 21-24)
  └── Routing, State Management, API Integration

Phase 7: Testing & Deployment (Weeks 25-27)
  └── Unit Tests, E2E, CI/CD

Phase 8: Real-World Projects (Weeks 28-32)
  └── Portfolio-worthy applications
```

---

---

# 📘 PHASE 1: FOUNDATIONS (Weeks 1–4)

---

## Week 1: HTML Essentials

### 1.1 — Basic HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is my first webpage.</p>
</body>
</html>
```

### 1.2 — Common HTML Elements

```html
<!-- Headings -->
<h1>Main Heading</h1>
<h2>Sub Heading</h2>
<h3>Sub-sub Heading</h3>

<!-- Paragraphs and Text -->
<p>This is a paragraph with <strong>bold</strong> and <em>italic</em> text.</p>

<!-- Links -->
<a href="https://google.com" target="_blank">Visit Google</a>

<!-- Images -->
<img src="photo.jpg" alt="Description of photo" width="300">

<!-- Lists -->
<ul>
    <li>Unordered Item 1</li>
    <li>Unordered Item 2</li>
</ul>

<ol>
    <li>Ordered Item 1</li>
    <li>Ordered Item 2</li>
</ol>
```

### 1.3 — Forms

```html
<form action="/submit" method="POST">
    <!-- Text Input -->
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" placeholder="Enter username" required>

    <!-- Email Input -->
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <!-- Password -->
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" minlength="8" required>

    <!-- Select Dropdown -->
    <label for="role">Role:</label>
    <select id="role" name="role">
        <option value="">Select a role</option>
        <option value="developer">Developer</option>
        <option value="designer">Designer</option>
        <option value="manager">Manager</option>
    </select>

    <!-- Radio Buttons -->
    <fieldset>
        <legend>Gender:</legend>
        <input type="radio" id="male" name="gender" value="male">
        <label for="male">Male</label>
        <input type="radio" id="female" name="gender" value="female">
        <label for="female">Female</label>
    </fieldset>

    <!-- Checkbox -->
    <input type="checkbox" id="terms" name="terms" required>
    <label for="terms">I agree to terms</label>

    <!-- Textarea -->
    <label for="bio">Bio:</label>
    <textarea id="bio" name="bio" rows="4" cols="50"></textarea>

    <!-- Submit Button -->
    <button type="submit">Register</button>
</form>
```

### 1.4 — Semantic HTML

```html
<body>
    <header>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="home">
            <h1>Welcome to My Site</h1>
            <article>
                <h2>Blog Post Title</h2>
                <time datetime="2024-01-15">January 15, 2024</time>
                <p>Blog content goes here...</p>
                <figure>
                    <img src="chart.png" alt="Sales chart">
                    <figcaption>Q4 Sales Data</figcaption>
                </figure>
            </article>
        </section>

        <aside>
            <h3>Related Links</h3>
            <ul>
                <li><a href="#">Link 1</a></li>
            </ul>
        </aside>
    </main>

    <footer>
        <p>&copy; 2024 My Website. All rights reserved.</p>
    </footer>
</body>
```

### 📝 Practice Exercise: Build a personal profile page with:
- A header with navigation
- A section with your photo, name, and bio
- A section listing your skills (use lists)
- A contact form
- A footer

---

## Week 2: CSS Essentials

### 2.1 — CSS Selectors & Basic Styling

```css
/* ========== SELECTORS ========== */

/* Element selector */
h1 {
    color: #333;
    font-size: 2.5rem;
}

/* Class selector */
.highlight {
    background-color: yellow;
    padding: 4px 8px;
}

/* ID selector */
#main-title {
    text-align: center;
}

/* Descendant selector */
nav ul li a {
    text-decoration: none;
    color: #007bff;
}

/* Pseudo-classes */
a:hover {
    color: #0056b3;
    text-decoration: underline;
}

input:focus {
    outline: 2px solid #007bff;
    border-color: #007bff;
}

li:first-child {
    font-weight: bold;
}

li:nth-child(even) {
    background-color: #f8f9fa;
}

/* Pseudo-elements */
p::first-line {
    font-weight: bold;
}

h2::before {
    content: "📌 ";
}
```

### 2.2 — Box Model

```css
/* Understanding the Box Model */
.box {
    /* Content dimensions */
    width: 300px;
    height: 200px;

    /* Padding - space INSIDE the border */
    padding: 20px;               /* all sides */
    padding: 10px 20px;          /* vertical | horizontal */
    padding: 10px 20px 15px 25px; /* top | right | bottom | left */

    /* Border */
    border: 2px solid #333;
    border-radius: 8px;          /* rounded corners */

    /* Margin - space OUTSIDE the border */
    margin: 20px auto;           /* vertical | horizontal (auto centers) */

    /* Box-sizing: include padding & border in width/height */
    box-sizing: border-box;      /* ALWAYS USE THIS */
}

/* Apply box-sizing globally (best practice) */
*, *::before, *::after {
    box-sizing: border-box;
}
```

### 2.3 — Flexbox (Most Important Layout Tool)

```css
/* ========== FLEXBOX CONTAINER ========== */
.flex-container {
    display: flex;

    /* Direction */
    flex-direction: row;          /* default: left to right */
    flex-direction: column;       /* top to bottom */

    /* Main axis alignment */
    justify-content: flex-start;  /* default */
    justify-content: center;      /* center items */
    justify-content: space-between; /* equal space between */
    justify-content: space-around;  /* equal space around */
    justify-content: space-evenly;  /* truly even spacing */

    /* Cross axis alignment */
    align-items: stretch;         /* default: fill height */
    align-items: center;          /* center vertically */
    align-items: flex-start;      /* align to top */
    align-items: flex-end;        /* align to bottom */

    /* Wrapping */
    flex-wrap: wrap;              /* allow items to wrap */

    /* Gap between items */
    gap: 16px;                    /* modern way to add spacing */
}

/* ========== FLEXBOX CHILDREN ========== */
.flex-item {
    flex-grow: 1;    /* how much item should grow */
    flex-shrink: 0;  /* how much item should shrink */
    flex-basis: 200px; /* initial size before grow/shrink */

    /* Shorthand */
    flex: 1;              /* grow:1, shrink:1, basis:0 */
    flex: 0 0 200px;      /* fixed 200px, no grow/shrink */

    /* Individual alignment */
    align-self: center;
}

/* ===== PRACTICAL EXAMPLE: Navbar ===== */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
    background-color: #333;
    color: white;
}

.navbar .logo {
    font-size: 1.5rem;
    font-weight: bold;
}

.navbar .nav-links {
    display: flex;
    gap: 1.5rem;
    list-style: none;
}

/* ===== PRACTICAL EXAMPLE: Card Grid ===== */
.card-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 24px;
    padding: 24px;
}

.card {
    flex: 1 1 300px;  /* grow, shrink, minimum 300px */
    max-width: 400px;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 16px;
}

/* ===== PRACTICAL EXAMPLE: Center anything ===== */
.center-everything {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}
```

### 2.4 — CSS Grid

```css
/* ========== GRID CONTAINER ========== */
.grid-container {
    display: grid;

    /* Define columns */
    grid-template-columns: 200px 1fr 200px;     /* fixed-fluid-fixed */
    grid-template-columns: repeat(3, 1fr);       /* 3 equal columns */
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* responsive! */

    /* Define rows */
    grid-template-rows: auto 1fr auto;

    /* Gaps */
    gap: 20px;
    column-gap: 20px;
    row-gap: 10px;
}

/* ========== GRID CHILDREN ========== */
.grid-item {
    grid-column: 1 / 3;     /* span from column 1 to 3 */
    grid-column: span 2;     /* span 2 columns */
    grid-row: 1 / 3;         /* span from row 1 to 3 */
}

/* ===== PRACTICAL EXAMPLE: Page Layout ===== */
.page-layout {
    display: grid;
    grid-template-areas:
        "header header header"
        "sidebar main aside"
        "footer footer footer";
    grid-template-columns: 200px 1fr 200px;
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
    gap: 16px;
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.aside   { grid-area: aside; }
.footer  { grid-area: footer; }
```

### 2.5 — Responsive Design

```css
/* Mobile-First Approach (recommended) */

/* Base styles (mobile) */
.container {
    width: 100%;
    padding: 0 16px;
}

.grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 16px;
}

/* Tablet (768px and up) */
@media (min-width: 768px) {
    .container {
        max-width: 720px;
        margin: 0 auto;
    }

    .grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* Desktop (1024px and up) */
@media (min-width: 1024px) {
    .container {
        max-width: 960px;
    }

    .grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

/* Large Desktop (1200px and up) */
@media (min-width: 1200px) {
    .container {
        max-width: 1140px;
    }

    .grid {
        grid-template-columns: repeat(4, 1fr);
    }
}

/* Responsive Typography */
html {
    font-size: 16px;
}

h1 { font-size: 1.75rem; }

@media (min-width: 768px) {
    h1 { font-size: 2.25rem; }
}

@media (min-width: 1024px) {
    h1 { font-size: 3rem; }
}
```

### 📝 Practice Exercise:
Build a responsive portfolio page with:
- A sticky navigation bar (flexbox)
- A hero section (centered content)
- A project grid (CSS Grid, responsive)
- A contact section
- Make it look great on mobile, tablet, and desktop

---

## Week 3–4: JavaScript Fundamentals

### 3.1 — Variables & Data Types

```javascript
// ========== VARIABLES ==========
// Use 'const' by default, 'let' when you need to reassign
const PI = 3.14159;            // cannot be reassigned
let count = 0;                  // can be reassigned
// var oldWay = "avoid this";   // DON'T use var (scoping issues)

count = 10;  // ✅ OK
// PI = 3;   // ❌ Error: Assignment to constant

// ========== DATA TYPES ==========
// Primitives
const name = "Alice";           // string
const age = 25;                 // number
const price = 9.99;             // number (no separate float type)
const isActive = true;          // boolean
const nothing = null;           // null (intentional absence)
let notDefined;                 // undefined (not yet assigned)
const id = Symbol("id");        // symbol (unique identifier)
const bigNum = 9007199254740991n; // bigint

// Type checking
console.log(typeof name);       // "string"
console.log(typeof age);        // "number"
console.log(typeof isActive);   // "boolean"
console.log(typeof nothing);    // "object" (known JS quirk!)
console.log(typeof notDefined); // "undefined"

// ========== STRINGS ==========
const firstName = "John";
const lastName = 'Doe';

// Template literals (backticks) - USE THESE
const greeting = `Hello, ${firstName} ${lastName}!`;
const multiLine = `
    This is a
    multi-line string.
    2 + 2 = ${2 + 2}
`;

// String methods
const str = "Hello, World!";
console.log(str.length);            // 13
console.log(str.toUpperCase());     // "HELLO, WORLD!"
console.log(str.toLowerCase());     // "hello, world!"
console.log(str.includes("World")); // true
console.log(str.startsWith("Hello")); // true
console.log(str.slice(0, 5));       // "Hello"
console.log(str.split(", "));       // ["Hello", "World!"]
console.log(str.trim());            // removes whitespace from ends
console.log(str.replace("World", "JS")); // "Hello, JS!"
console.log(str.indexOf("World"));  // 7
```

### 3.2 — Operators & Control Flow

```javascript
// ========== COMPARISON OPERATORS ==========
console.log(5 === 5);       // true  (strict equality - USE THIS)
console.log(5 === "5");     // false (different types)
console.log(5 == "5");      // true  (loose equality - AVOID)
console.log(5 !== "5");     // true  (strict inequality - USE THIS)
console.log(5 > 3);         // true
console.log(5 >= 5);        // true

// ========== LOGICAL OPERATORS ==========
const a = true, b = false;
console.log(a && b);        // false (AND)
console.log(a || b);        // true  (OR)
console.log(!a);             // false (NOT)

// ========== CONDITIONALS ==========
const score = 85;

if (score >= 90) {
    console.log("A");
} else if (score >= 80) {
    console.log("B");        // This runs
} else if (score >= 70) {
    console.log("C");
} else {
    console.log("F");
}

// Ternary operator (for simple conditions)
const grade = score >= 90 ? "A" : score >= 80 ? "B" : "C";
const message = isActive ? "User is active" : "User is inactive";

// Nullish coalescing (??)
const username = null;
const displayName = username ?? "Anonymous";  // "Anonymous"
// Only uses fallback for null/undefined (not 0, "", false)

// Optional chaining (?.)
const user = { profile: { name: "Alice" } };
console.log(user?.profile?.name);     // "Alice"
console.log(user?.address?.street);   // undefined (no error!)

// Switch
const day = "Monday";
switch (day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        console.log("Weekday");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend");
        break;
    default:
        console.log("Invalid day");
}

// ========== LOOPS ==========
// For loop
for (let i = 0; i < 5; i++) {
    console.log(i);  // 0, 1, 2, 3, 4
}

// While loop
let count2 = 0;
while (count2 < 5) {
    console.log(count2);
    count2++;
}

// For...of (iterate over values - arrays, strings)
const colors = ["red", "green", "blue"];
for (const color of colors) {
    console.log(color);
}

// For...in (iterate over keys - objects)
const person = { name: "Alice", age: 25 };
for (const key in person) {
    console.log(`${key}: ${person[key]}`);
}
```

### 3.3 — Functions

```javascript
// ========== FUNCTION DECLARATION ==========
function add(a, b) {
    return a + b;
}

// ========== FUNCTION EXPRESSION ==========
const subtract = function(a, b) {
    return a - b;
};

// ========== ARROW FUNCTIONS (preferred in React) ==========
const multiply = (a, b) => {
    return a * b;
};

// Arrow function - concise body (implicit return)
const divide = (a, b) => a / b;

// Single parameter - no parentheses needed
const double = n => n * 2;

// No parameters
const greet = () => "Hello!";

// ========== DEFAULT PARAMETERS ==========
const createUser = (name, role = "user", active = true) => {
    return { name, role, active };
};

createUser("Alice");              // { name: "Alice", role: "user", active: true }
createUser("Bob", "admin");       // { name: "Bob", role: "admin", active: true }

// ========== REST PARAMETERS ==========
const sum = (...numbers) => {
    return numbers.reduce((total, num) => total + num, 0);
};

sum(1, 2, 3, 4, 5);  // 15

// ========== HIGHER-ORDER FUNCTIONS ==========
// A function that takes or returns another function
const createMultiplier = (multiplier) => {
    return (number) => number * multiplier;
};

const triple = createMultiplier(3);
console.log(triple(5));  // 15

// ========== CLOSURES ==========
const createCounter = () => {
    let count = 0;  // This variable is "closed over"
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count,
    };
};

const counter = createCounter();
counter.increment();  // 1
counter.increment();  // 2
counter.decrement();  // 1
counter.getCount();   // 1
```

### 3.4 — Arrays (You'll use these CONSTANTLY in React)

```javascript
// ========== CREATING ARRAYS ==========
const fruits = ["apple", "banana", "cherry"];
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "hello", true, null, { name: "Alice" }];

// ========== ACCESSING ELEMENTS ==========
console.log(fruits[0]);          // "apple"
console.log(fruits[fruits.length - 1]);  // "cherry" (last element)
console.log(fruits.at(-1));      // "cherry" (modern way)

// ========== IMPORTANT ARRAY METHODS (Immutable - return new arrays) ==========

// .map() - Transform each element (MOST USED IN REACT)
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10]

const userNames = [
    { first: "Alice", last: "Smith" },
    { first: "Bob", last: "Jones" },
];
const fullNames = userNames.map(user => `${user.first} ${user.last}`);
// ["Alice Smith", "Bob Jones"]

// .filter() - Keep elements that pass a test
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4]

const adults = users.filter(user => user.age >= 18);

// .find() - Get the FIRST element that matches
const found = numbers.find(n => n > 3);
// 4

// .findIndex() - Get the INDEX of first match
const index = numbers.findIndex(n => n > 3);
// 3

// .reduce() - Accumulate values into one result
const total = numbers.reduce((accumulator, current) => {
    return accumulator + current;
}, 0);  // 15

// .some() - Does ANY element match?
const hasNegative = numbers.some(n => n < 0);  // false

// .every() - Do ALL elements match?
const allPositive = numbers.every(n => n > 0);  // true

// .includes() - Does array contain this value?
const hasBanana = fruits.includes("banana");  // true

// .sort() - Sort elements (MUTATES original! Make a copy)
const sorted = [...numbers].sort((a, b) => a - b);  // ascending
const descending = [...numbers].sort((a, b) => b - a);

// .flat() - Flatten nested arrays
const nested = [1, [2, 3], [4, [5]]];
nested.flat();     // [1, 2, 3, 4, [5]]
nested.flat(2);    // [1, 2, 3, 4, 5]

// ========== CHAINING METHODS ==========
const result = numbers
    .filter(n => n > 2)       // [3, 4, 5]
    .map(n => n * 10)         // [30, 40, 50]
    .reduce((sum, n) => sum + n, 0);  // 120

// ========== SPREAD OPERATOR WITH ARRAYS ==========
const moreFruits = [...fruits, "date", "elderberry"];
// ["apple", "banana", "cherry", "date", "elderberry"]

const combined = [...fruits, ...numbers];

// ========== DESTRUCTURING ARRAYS ==========
const [first, second, ...rest] = fruits;
// first = "apple", second = "banana", rest = ["cherry"]

// Swapping variables
let x = 1, y = 2;
[x, y] = [y, x];  // x = 2, y = 1
```

### 3.5 — Objects

```javascript
// ========== CREATING OBJECTS ==========
const person = {
    firstName: "Alice",
    lastName: "Smith",
    age: 30,
    hobbies: ["reading", "coding"],
    address: {
        street: "123 Main St",
        city: "Springfield",
        state: "IL",
    },
    // Method
    getFullName() {
        return `${this.firstName} ${this.lastName}`;
    },
};

// ========== ACCESSING PROPERTIES ==========
console.log(person.firstName);           // "Alice" (dot notation)
console.log(person["lastName"]);         // "Smith" (bracket notation)
console.log(person.address.city);        // "Springfield"
console.log(person?.address?.zipCode);   // undefined (safe access)

// Dynamic key access
const key = "age";
console.log(person[key]);               // 30

// ========== DESTRUCTURING OBJECTS (VERY important for React) ==========
const { firstName, lastName, age } = person;
console.log(firstName);  // "Alice"

// Rename during destructuring
const { firstName: fName, lastName: lName } = person;
console.log(fName);  // "Alice"

// Default values
const { middleName = "N/A" } = person;
console.log(middleName);  // "N/A"

// Nested destructuring
const { address: { city, state } } = person;
console.log(city);  // "Springfield"

// Rest in destructuring
const { firstName: fn, ...restOfPerson } = person;
// restOfPerson = { lastName, age, hobbies, address, getFullName }

// ========== SPREAD OPERATOR WITH OBJECTS ==========
const updatedPerson = {
    ...person,
    age: 31,                    // override age
    email: "alice@example.com", // add new property
};

// ========== OBJECT METHODS ==========
const keys = Object.keys(person);        // ["firstName", "lastName", "age", ...]
const values = Object.values(person);    // ["Alice", "Smith", 30, ...]
const entries = Object.entries(person);  // [["firstName", "Alice"], ...]

// Iterate over object
Object.entries(person).forEach(([key, value]) => {
    console.log(`${key}: ${value}`);
});

// Check if property exists
console.log("age" in person);                  // true
console.log(person.hasOwnProperty("age"));     // true

// ========== COMPUTED PROPERTY NAMES ==========
const field = "email";
const formData = {
    [field]: "alice@example.com",  // email: "alice@example.com"
    [`${field}Verified`]: true,    // emailVerified: true
};

// ========== SHORTHAND PROPERTIES ==========
const name2 = "Bob";
const age2 = 25;
const user = { name2, age2 };  // same as { name2: name2, age2: age2 }
```

### 📝 Practice Exercises for Phase 1:
1. **Todo List (HTML/CSS/JS)**: Create a basic todo list with add, delete, and mark complete
2. **Calculator**: Build a working calculator
3. **Quiz App**: Multiple choice quiz with score tracking

---

---

# 📗 PHASE 2: JavaScript Deep Dive (Weeks 5–8)

---

## Week 5: Advanced Functions & Scope

### 5.1 — Scope & Hoisting

```javascript
// ========== SCOPE ==========
// Global scope
const globalVar = "I'm global";

function outerFunction() {
    // Function scope
    const outerVar = "I'm in outer";

    if (true) {
        // Block scope
        const blockVar = "I'm in block";
        let blockLet = "Also in block";
        // var would leak out! That's why we avoid it
        console.log(globalVar);  // ✅ accessible
        console.log(outerVar);   // ✅ accessible
        console.log(blockVar);   // ✅ accessible
    }

    console.log(globalVar);  // ✅ accessible
    console.log(outerVar);   // ✅ accessible
    // console.log(blockVar); // ❌ ReferenceError
}

// ========== CLOSURES IN PRACTICE ==========
// Closures "remember" their outer scope

// Example 1: Data privacy
const createBankAccount = (initialBalance) => {
    let balance = initialBalance;  // private variable

    return {
        deposit(amount) {
            balance += amount;
            return balance;
        },
        withdraw(amount) {
            if (amount > balance) throw new Error("Insufficient funds");
            balance -= amount;
            return balance;
        },
        getBalance() {
            return balance;
        },
    };
};

const account = createBankAccount(100);
account.deposit(50);    // 150
account.withdraw(30);   // 120
account.getBalance();   // 120
// account.balance;     // undefined (private!)

// Example 2: Function factories
const createGreeter = (greeting) => {
    return (name) => `${greeting}, ${name}!`;
};

const sayHello = createGreeter("Hello");
const sayHola = createGreeter("Hola");
sayHello("Alice");  // "Hello, Alice!"
sayHola("Bob");     // "Hola, Bob!"
```

### 5.2 — Callbacks, Promises, and Async/Await

```javascript
// ========== CALLBACKS (old way) ==========
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: "Alice" };
        callback(data);
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});

// Callback hell (why we moved to Promises)
// getData((a) => {
//     getMoreData(a, (b) => {
//         getEvenMoreData(b, (c) => {
//             // deeply nested... hard to read
//         });
//     });
// });

// ========== PROMISES ==========
const fetchUser = (id) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id > 0) {
                resolve({ id, name: "Alice" });
            } else {
                reject(new Error("Invalid ID"));
            }
        }, 1000);
    });
};

// Using .then()/.catch()
fetchUser(1)
    .then(user => {
        console.log(user);
        return fetchUser(2);  // chain another promise
    })
    .then(user2 => {
        console.log(user2);
    })
    .catch(error => {
        console.error("Error:", error.message);
    })
    .finally(() => {
        console.log("Done!");  // always runs
    });

// ========== ASYNC/AWAIT (modern way - USE THIS) ==========
const loadUserData = async () => {
    try {
        const user1 = await fetchUser(1);
        console.log("User 1:", user1);

        const user2 = await fetchUser(2);
        console.log("User 2:", user2);

        return [user1, user2];
    } catch (error) {
        console.error("Failed to load:", error.message);
        throw error;  // re-throw if needed
    } finally {
        console.log("Loading complete");
    }
};

// ========== PARALLEL ASYNC OPERATIONS ==========
const loadAllUsers = async () => {
    try {
        // Run in parallel (much faster!)
        const [user1, user2, user3] = await Promise.all([
            fetchUser(1),
            fetchUser(2),
            fetchUser(3),
        ]);

        console.log(user1, user2, user3);
    } catch (error) {
        // If ANY promise rejects, catch runs
        console.error(error);
    }
};

// Promise.allSettled - get results even if some fail
const results = await Promise.allSettled([
    fetchUser(1),
    fetchUser(-1),  // this will reject
    fetchUser(3),
]);
// results = [
//   { status: "fulfilled", value: { id: 1, name: "Alice" } },
//   { status: "rejected", reason: Error("Invalid ID") },
//   { status: "fulfilled", value: { id: 3, name: "Alice" } },
// ]

// ========== FETCH API (real HTTP requests) ==========
const fetchPosts = async () => {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts");

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        const posts = await response.json();
        console.log(posts);
        return posts;
    } catch (error) {
        console.error("Failed to fetch posts:", error);
    }
};

// POST request
const createPost = async (postData) => {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(postData),
        });

        const newPost = await response.json();
        return newPost;
    } catch (error) {
        console.error("Failed to create post:", error);
    }
};

await createPost({
    title: "My Post",
    body: "Post content here",
    userId: 1,
});
```

### 5.3 — Destructuring & Spread (Deep Dive)

```javascript
// ========== ADVANCED DESTRUCTURING ==========

// Function parameter destructuring (VERY common in React)
const UserCard = ({ name, age, email, role = "user" }) => {
    return `${name} (${age}) - ${email} [${role}]`;
};

UserCard({ name: "Alice", age: 30, email: "alice@test.com" });

// Nested destructuring
const company = {
    name: "TechCorp",
    ceo: {
        name: "Bob",
        contact: {
            email: "bob@techcorp.com",
            phone: "555-0100",
        },
    },
    employees: [
        { name: "Alice", dept: "Engineering" },
        { name: "Charlie", dept: "Design" },
    ],
};

const {
    name: companyName,
    ceo: {
        name: ceoName,
        contact: { email: ceoEmail },
    },
    employees: [firstEmployee, ...otherEmployees],
} = company;

// ========== SPREAD FOR IMMUTABLE UPDATES (crucial for React state) ==========

// Updating nested objects immutably
const state = {
    user: {
        name: "Alice",
        preferences: {
            theme: "dark",
            language: "en",
        },
    },
    posts: [
        { id: 1, title: "Post 1" },
        { id: 2, title: "Post 2" },
    ],
};

// Update nested property
const newState = {
    ...state,
    user: {
        ...state.user,
        preferences: {
            ...state.user.preferences,
            theme: "light",  // only change theme
        },
    },
};

// Add item to array
const withNewPost = {
    ...state,
    posts: [...state.posts, { id: 3, title: "Post 3" }],
};

// Remove item from array
const withoutPost = {
    ...state,
    posts: state.posts.filter(post => post.id !== 2),
};

// Update item in array
const withUpdatedPost = {
    ...state,
    posts: state.posts.map(post =>
        post.id === 1 ? { ...post, title: "Updated Post 1" } : post
    ),
};
```

### 5.4 — ES6+ Modules

```javascript
// ========== math.js ==========
// Named exports
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const PI = 3.14159;

// Default export (one per file)
const Calculator = {
    add,
    subtract,
    multiply: (a, b) => a * b,
    divide: (a, b) => a / b,
};
export default Calculator;

// ========== utils.js ==========
export const formatDate = (date) => {
    return new Intl.DateTimeFormat("en-US").format(date);
};

export const capitalize = (str) => {
    return str.charAt(0).toUpperCase() + str.slice(1);
};

// ========== app.js ==========
// Import named exports
import { add, subtract, PI } from "./math.js";

// Import default export
import Calculator from "./math.js";

// Import all named exports as namespace
import * as MathUtils from "./math.js";
MathUtils.add(2, 3);

// Import with rename
import { formatDate as fmtDate, capitalize } from "./utils.js";

// Mixed imports
import Calculator, { add, PI } from "./math.js";
```

### 5.5 — Error Handling

```javascript
// ========== TRY/CATCH/FINALLY ==========
const parseJSON = (jsonString) => {
    try {
        const data = JSON.parse(jsonString);
        return { success: true, data };
    } catch (error) {
        console.error("Parse error:", error.message);
        return { success: false, error: error.message };
    } finally {
        console.log("Parse attempt complete");
    }
};

parseJSON('{"name": "Alice"}');     // { success: true, data: { name: "Alice" } }
parseJSON('invalid json');           // { success: false, error: "..." }

// ========== CUSTOM ERRORS ==========
class ValidationError extends Error {
    constructor(field, message) {
        super(message);
        this.name = "ValidationError";
        this.field = field;
    }
}

class NotFoundError extends Error {
    constructor(resource, id) {
        super(`${resource} with id ${id} not found`);
        this.name = "NotFoundError";
        this.resource = resource;
        this.id = id;
    }
}

const validateUser = (user) => {
    if (!user.email) {
        throw new ValidationError("email", "Email is required");
    }
    if (!user.email.includes("@")) {
        throw new ValidationError("email", "Invalid email format");
    }
    if (!user.password || user.password.length < 8) {
        throw new ValidationError("password", "Password must be at least 8 characters");
    }
    return true;
};

try {
    validateUser({ email: "invalid", password: "123" });
} catch (error) {
    if (error instanceof ValidationError) {
        console.log(`Validation failed on ${error.field}: ${error.message}`);
    } else {
        throw error;  // re-throw unexpected errors
    }
}
```

## Week 6: DOM Manipulation (Pre-React Understanding)

### 6.1 — DOM Basics

```javascript
// ========== SELECTING ELEMENTS ==========
const title = document.getElementById("title");
const buttons = document.querySelectorAll(".btn");
const firstButton = document.querySelector(".btn");
const nav = document.querySelector("nav");

// ========== MODIFYING ELEMENTS ==========
title.textContent = "New Title";      // text only
title.innerHTML = "<em>New</em> Title"; // includes HTML (be careful - XSS risk)

// Styles
title.style.color = "blue";
title.style.fontSize = "2rem";

// Classes (preferred over inline styles)
title.classList.add("active");
title.classList.remove("hidden");
title.classList.toggle("highlight");
title.classList.contains("active");  // true

// Attributes
const link = document.querySelector("a");
link.setAttribute("href", "https://google.com");
link.getAttribute("href");
link.removeAttribute("target");

// ========== CREATING ELEMENTS ==========
const newDiv = document.createElement("div");
newDiv.textContent = "I'm new!";
newDiv.classList.add("card");
document.body.appendChild(newDiv);

// ========== EVENT HANDLING ==========
const button = document.querySelector("#myButton");

button.addEventListener("click", (event) => {
    console.log("Button clicked!");
    console.log("Event target:", event.target);
    console.log("Event type:", event.type);
});

// Common events: click, submit, input, change, keydown, keyup,
//                mouseover, mouseout, focus, blur, scroll, load

// Form handling
const form = document.querySelector("form");
form.addEventListener("submit", (event) => {
    event.preventDefault();  // prevent page reload

    const formData = new FormData(form);
    const data = Object.fromEntries(formData);
    console.log(data);  // { username: "...", email: "...", ... }
});

// Input handling
const input = document.querySelector("#search");
input.addEventListener("input", (event) => {
    console.log("Current value:", event.target.value);
});
```

> **Why learn DOM manipulation?** React does this for you, but understanding the DOM helps you debug issues and understand what React is doing under the hood.

---

## Week 7: Advanced JavaScript Patterns

### 7.1 — Array & Object Patterns Used in React

```javascript
// ========== PATTERN: Immutable State Updates ==========
// React requires immutable updates. Practice these patterns!

const todos = [
    { id: 1, text: "Learn JS", completed: false },
    { id: 2, text: "Learn React", completed: false },
    { id: 3, text: "Build project", completed: false },
];

// ADD an item
const addTodo = (todos, newTodo) => [...todos, newTodo];

const updated1 = addTodo(todos, {
    id: 4,
    text: "Deploy app",
    completed: false,
});

// REMOVE an item
const removeTodo = (todos, idToRemove) =>
    todos.filter(todo => todo.id !== idToRemove);

const updated2 = removeTodo(todos, 2);

// UPDATE an item
const toggleTodo = (todos, idToToggle) =>
    todos.map(todo =>
        todo.id === idToToggle
            ? { ...todo, completed: !todo.completed }
            : todo
    );

const updated3 = toggleTodo(todos, 1);

// UPDATE specific field
const updateTodoText = (todos, id, newText) =>
    todos.map(todo =>
        todo.id === id
            ? { ...todo, text: newText }
            : todo
    );

// ========== PATTERN: Group/Categorize data ==========
const users = [
    { name: "Alice", dept: "Engineering" },
    { name: "Bob", dept: "Design" },
    { name: "Charlie", dept: "Engineering" },
    { name: "Diana", dept: "Design" },
    { name: "Eve", dept: "Marketing" },
];

const groupByDept = users.reduce((groups, user) => {
    const dept = user.dept;
    return {
        ...groups,
        [dept]: [...(groups[dept] || []), user],
    };
}, {});

// Result:
// {
//   Engineering: [{ name: "Alice", ... }, { name: "Charlie", ... }],
//   Design: [{ name: "Bob", ... }, { name: "Diana", ... }],
//   Marketing: [{ name: "Eve", ... }],
// }

// ========== PATTERN: Transform API data ==========
const apiResponse = {
    data: {
        users: [
            { user_id: 1, first_name: "Alice", last_name: "Smith", is_active: true },
            { user_id: 2, first_name: "Bob", last_name: "Jones", is_active: false },
        ],
    },
};

// Transform to frontend-friendly format
const transformUsers = (apiData) =>
    apiData.data.users.map(user => ({
        id: user.user_id,
        name: `${user.first_name} ${user.last_name}`,
        isActive: user.is_active,
    }));

// [{ id: 1, name: "Alice Smith", isActive: true }, ...]

// ========== PATTERN: Lookup maps for O(1) access ==========
const usersArray = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" },
];

const usersById = usersArray.reduce((map, user) => {
    map[user.id] = user;
    return map;
}, {});

// Fast lookup: usersById[2] → { id: 2, name: "Bob" }
// Instead of: usersArray.find(u => u.id === 2)
```

### 7.2 — Classes (Used by some libraries)

```javascript
class Animal {
    // Constructor
    constructor(name, sound) {
        this.name = name;
        this.sound = sound;
    }

    // Method
    speak() {
        return `${this.name} says ${this.sound}!`;
    }

    // Getter
    get info() {
        return `${this.name} (${this.constructor.name})`;
    }

    // Static method (called on class, not instance)
    static create(name, sound) {
        return new Animal(name, sound);
    }
}

// Inheritance
class Dog extends Animal {
    constructor(name, breed) {
        super(name, "Woof");  // call parent constructor
        this.breed = breed;
    }

    fetch(item) {
        return `${this.name} fetches the ${item}!`;
    }
}

const dog = new Dog("Rex", "Golden Retriever");
dog.speak();  // "Rex says Woof!"
dog.fetch("ball");  // "Rex fetches the ball!"
```

### 📝 Practice Exercises for Phase 2:
1. **Weather App**: Fetch weather data from an API and display it
2. **GitHub Profile Finder**: Fetch and display GitHub user profiles
3. **Shopping Cart**: Implement add/remove/update with immutable patterns

---

---

# 📕 PHASE 3: TypeScript (Weeks 9–11)

---

## Week 9: TypeScript Basics

### 9.1 — Setup & Basic Types

```bash
# Install TypeScript
npm install -g typescript

# Initialize a TS project
mkdir my-ts-project && cd my-ts-project
npm init -y
tsc --init

# Compile TypeScript
tsc filename.ts

# Run with ts-node (for practice)
npm install -g ts-node
ts-node filename.ts
```

```typescript
// ========== BASIC TYPES ==========
// Type annotations come after the variable name

let username: string = "Alice";
let age: number = 25;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;

// TypeScript infers types (you don't always need annotations)
let inferredString = "Hello";  // TypeScript knows this is a string
// inferredString = 42;  // ❌ Error: Type 'number' is not assignable to type 'string'

// ========== ARRAYS ==========
let numbers: number[] = [1, 2, 3, 4, 5];
let names: string[] = ["Alice", "Bob"];
let mixed: (string | number)[] = [1, "hello", 2];

// Alternative syntax
let scores: Array<number> = [95, 87, 92];

// ========== TUPLE (fixed-length array with specific types) ==========
let coordinate: [number, number] = [10, 20];
let userEntry: [string, number, boolean] = ["Alice", 25, true];

// ========== ENUM ==========
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}

let move: Direction = Direction.Up;

enum Status {
    Pending,    // 0
    Active,     // 1
    Inactive,   // 2
}

// Prefer const enum or union types in modern TS:
type DirectionType = "UP" | "DOWN" | "LEFT" | "RIGHT";

// ========== ANY, UNKNOWN, NEVER ==========
// any - opt out of type checking (AVOID when possible)
let anything: any = 42;
anything = "string";  // no error

// unknown - safer version of any (must narrow type before use)
let mystery: unknown = 42;
// mystery.toFixed(2);  // ❌ Error
if (typeof mystery === "number") {
    mystery.toFixed(2);  // ✅ OK after type check
}

// never - function never returns (throws or infinite loop)
function throwError(message: string): never {
    throw new Error(message);
}

// void - function doesn't return a value
function logMessage(message: string): void {
    console.log(message);
}
```

### 9.2 — Functions in TypeScript

```typescript
// ========== FUNCTION TYPE ANNOTATIONS ==========

// Parameters and return type
function add(a: number, b: number): number {
    return a + b;
}

// Arrow function
const multiply = (a: number, b: number): number => a * b;

// Optional parameters
function greet(name: string, greeting?: string): string {
    return `${greeting || "Hello"}, ${name}!`;
}

greet("Alice");           // "Hello, Alice!"
greet("Alice", "Hola");  // "Hola, Alice!"

// Default parameters
function createUser(
    name: string,
    role: string = "user",
    active: boolean = true
): { name: string; role: string; active: boolean } {
    return { name, role, active };
}

// Rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((total, n) => total + n, 0);
}

// ========== FUNCTION TYPES ==========
// Define the shape of a function
type MathOperation = (a: number, b: number) => number;

const subtract: MathOperation = (a, b) => a - b;
const divide: MathOperation = (a, b) => a / b;

// Function that takes a callback
function processNumbers(
    numbers: number[],
    operation: (n: number) => number
): number[] {
    return numbers.map(operation);
}

processNumbers([1, 2, 3], (n) => n * 2);  // [2, 4, 6]
```

### 9.3 — Interfaces & Type Aliases

```typescript
// ========== INTERFACE ==========
// Define the shape of an object
interface User {
    id: number;
    name: string;
    email: string;
    age?: number;              // optional property
    readonly createdAt: Date;  // can't be modified after creation
}

const alice: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
    createdAt: new Date(),
};

// alice.createdAt = new Date();  // ❌ Error: readonly

// ========== TYPE ALIAS ==========
type Product = {
    id: number;
    name: string;
    price: number;
    category: string;
};

// ========== INTERFACE vs TYPE: When to use which? ==========
// Interface: for object shapes, can be extended/merged
// Type: for unions, intersections, mapped types, primitives

// ========== EXTENDING INTERFACES ==========
interface Person {
    name: string;
    age: number;
}

interface Employee extends Person {
    employeeId: string;
    department: string;
    salary: number;
}

const emp: Employee = {
    name: "Bob",
    age: 30,
    employeeId: "E001",
    department: "Engineering",
    salary: 80000,
};

// ========== INTERSECTION TYPES (Type version of extends) ==========
type PersonType = {
    name: string;
    age: number;
};

type EmployeeType = PersonType & {
    employeeId: string;
    department: string;
};

// ========== UNION TYPES ==========
type Status = "pending" | "active" | "inactive" | "banned";
type ID = string | number;

let userId: ID = 123;
userId = "abc-123";  // also valid

function processStatus(status: Status) {
    switch (status) {
        case "pending":
            return "Awaiting approval";
        case "active":
            return "Account active";
        case "inactive":
            return "Account deactivated";
        case "banned":
            return "Account banned";
    }
}

// ========== DISCRIMINATED UNIONS (very powerful pattern) ==========
type Shape =
    | { kind: "circle"; radius: number }
    | { kind: "rectangle"; width: number; height: number }
    | { kind: "triangle"; base: number; height: number };

function calculateArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "rectangle":
            return shape.width * shape.height;
        case "triangle":
            return (shape.base * shape.height) / 2;
    }
}

calculateArea({ kind: "circle", radius: 5 });           // 78.54
calculateArea({ kind: "rectangle", width: 10, height: 5 }); // 50

// ========== INTERFACE FOR REACT COMPONENT PROPS ==========
interface ButtonProps {
    label: string;
    onClick: () => void;
    variant?: "primary" | "secondary" | "danger";
    disabled?: boolean;
    icon?: React.ReactNode;
    size?: "sm" | "md" | "lg";
}

interface CardProps {
    title: string;
    description: string;
    imageUrl?: string;
    tags: string[];
    onSelect: (id: string) => void;
}
```

## Week 10: Intermediate TypeScript

### 10.1 — Generics

```typescript
// ========== GENERICS - Make reusable, type-safe functions ==========

// Without generics - not reusable
function getFirstNumber(arr: number[]): number | undefined {
    return arr[0];
}

function getFirstString(arr: string[]): string | undefined {
    return arr[0];
}

// With generics - ONE function for all types!
function getFirst<T>(arr: T[]): T | undefined {
    return arr[0];
}

getFirst<number>([1, 2, 3]);       // number | undefined
getFirst<string>(["a", "b"]);      // string | undefined
getFirst([true, false]);            // TypeScript infers boolean

// ========== GENERIC FUNCTIONS ==========
function wrapInArray<T>(value: T): T[] {
    return [value];
}

function merge<T, U>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}

const merged = merge(
    { name: "Alice" },
    { age: 25 }
);
// merged: { name: string } & { age: number }

// ========== GENERIC CONSTRAINTS ==========
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(item: T): void {
    console.log(`Length: ${item.length}`);
}

logLength("hello");    // ✅ string has length
logLength([1, 2, 3]);  // ✅ array has length
// logLength(123);     // ❌ number doesn't have length

// Constraint with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person = { name: "Alice", age: 25, email: "alice@test.com" };
getProperty(person, "name");   // string
getProperty(person, "age");    // number
// getProperty(person, "foo"); // ❌ Error: "foo" is not a key of person

// ========== GENERIC INTERFACES ==========
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
    timestamp: Date;
}

interface PaginatedResponse<T> {
    items: T[];
    total: number;
    page: number;
    pageSize: number;
    totalPages: number;
}

// Usage
type UserResponse = ApiResponse<User>;
type UserListResponse = PaginatedResponse<User>;

const response: ApiResponse<User[]> = {
    data: [{ id: 1, name: "Alice", email: "alice@test.com", createdAt: new Date() }],
    status: 200,
    message: "Success",
    timestamp: new Date(),
};

// ========== GENERIC REACT COMPONENT PATTERN ==========
interface ListProps<T> {
    items: T[];
    renderItem: (item: T) => React.ReactNode;
    keyExtractor: (item: T) => string;
    emptyMessage?: string;
}

// This component can render a list of ANY type
// function List<T>({ items, renderItem, keyExtractor }: ListProps<T>) { ... }
```

### 10.2 — Utility Types (Built-in TypeScript helpers)

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
    role: "admin" | "user";
    createdAt: Date;
    updatedAt: Date;
}

// ========== Partial<T> - Make all properties optional ==========
type UpdateUserPayload = Partial<User>;
// { id?: number; name?: string; email?: string; ... }

function updateUser(id: number, updates: Partial<User>): User {
    // Can pass any subset of User properties
    return { ...existingUser, ...updates };
}

updateUser(1, { name: "New Name" });  // only update name
updateUser(1, { email: "new@email.com", role: "admin" });

// ========== Required<T> - Make all properties required ==========
type RequiredUser = Required<User>;

// ========== Pick<T, Keys> - Select specific properties ==========
type UserPreview = Pick<User, "id" | "name" | "email">;
// { id: number; name: string; email: string }

type LoginCredentials = Pick<User, "email" | "password">;
// { email: string; password: string }

// ========== Omit<T, Keys> - Exclude specific properties ==========
type PublicUser = Omit<User, "password">;
// User without password field

type CreateUserPayload = Omit<User, "id" | "createdAt" | "updatedAt">;
// { name: string; email: string; password: string; role: ... }

// ========== Record<Keys, Type> - Create object type with specific keys ==========
type Role = "admin" | "user" | "moderator";

type RolePermissions = Record<Role, string[]>;

const permissions: RolePermissions = {
    admin: ["read", "write", "delete", "manage_users"],
    user: ["read", "write"],
    moderator: ["read", "write", "delete"],
};

// Dynamic object maps
type UserMap = Record<string, User>;
type CountMap = Record<string, number>;

// ========== Readonly<T> - Make all properties readonly ==========
type ImmutableUser = Readonly<User>;
// All properties become readonly

// ========== ReturnType<T> - Get return type of a function ==========
function fetchUsers() {
    return [
        { id: 1, name: "Alice" },
        { id: 2, name: "Bob" },
    ];
}

type FetchUsersReturn = ReturnType<typeof fetchUsers>;
// { id: number; name: string }[]

// ========== COMBINING UTILITY TYPES ==========
// Create user DTO (no auto-generated fields)
type CreateUserDTO = Omit<User, "id" | "createdAt" | "updatedAt">;

// Update user DTO (all fields optional, no id)
type UpdateUserDTO = Partial<Omit<User, "id">>;

// Public user response (no sensitive fields)
type PublicUserResponse = Pick<User, "id" | "name" | "email" | "role">;

// Form state (all optional while filling)
type UserFormState = Partial<Pick<User, "name" | "email" | "password" | "role">>;
```

### 10.3 — Type Guards & Narrowing

```typescript
// ========== typeof guard ==========
function processValue(value: string | number) {
    if (typeof value === "string") {
        // TypeScript knows value is string here
        return value.toUpperCase();
    }
    // TypeScript knows value is number here
    return value.toFixed(2);
}

// ========== instanceof guard ==========
class Dog {
    bark() { return "Woof!"; }
}

class Cat {
    meow() { return "Meow!"; }
}

function makeSound(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        return animal.bark();
    }
    return animal.meow();
}

// ========== "in" operator guard ==========
interface Car {
    drive(): void;
    honk(): void;
}

interface Bicycle {
    pedal(): void;
    ring(): void;
}

function useVehicle(vehicle: Car | Bicycle) {
    if ("drive" in vehicle) {
        vehicle.drive();
        vehicle.honk();
    } else {
        vehicle.pedal();
        vehicle.ring();
    }
}

// ========== Custom type guard ==========
interface SuccessResponse {
    status: "success";
    data: any;
}

interface ErrorResponse {
    status: "error";
    error: string;
}

type ApiResult = SuccessResponse | ErrorResponse;

// Type predicate: "response is SuccessResponse"
function isSuccess(response: ApiResult): response is SuccessResponse {
    return response.status === "success";
}

function handleResponse(response: ApiResult) {
    if (isSuccess(response)) {
        console.log(response.data);   // TS knows it's SuccessResponse
    } else {
        console.log(response.error);  // TS knows it's ErrorResponse
    }
}
```

### 📝 Practice: Convert your JavaScript projects to TypeScript
- Add types to all functions
- Create interfaces for all data structures
- Use generics where appropriate

---

---

# 📙 PHASE 4: React Fundamentals (Weeks 12–16)

---

## Week 12: React Setup & JSX

### 12.1 — Project Setup

```bash
# Create a React + TypeScript project with Vite (recommended)
npm create vite@latest my-react-app -- --template react-ts
cd my-react-app
npm install
npm run dev

# Project structure:
# my-react-app/
# ├── public/
# ├── src/
# │   ├── assets/
# │   ├── App.tsx          ← Main component
# │   ├── App.css
# │   ├── main.tsx         ← Entry point
# │   └── vite-env.d.ts
# ├── index.html
# ├── package.json
# ├── tsconfig.json
# └── vite.config.ts
```

### 12.2 — JSX Fundamentals

```tsx
// ========== JSX is NOT HTML - it's JavaScript ==========

// JSX expressions must return ONE root element
function App() {
    // Variables you can use in JSX
    const name = "Alice";
    const age = 25;
    const isLoggedIn = true;
    const hobbies = ["Reading", "Coding", "Gaming"];

    return (
        // ONE root element (or use Fragment: <> </>)
        <div className="app">
            {/* Use className, not class */}

            {/* Embedding JavaScript expressions with {} */}
            <h1>Hello, {name}!</h1>
            <p>Age: {age}</p>
            <p>Next year: {age + 1}</p>
            <p>Name uppercase: {name.toUpperCase()}</p>

            {/* Conditional rendering */}
            {isLoggedIn && <p>Welcome back!</p>}

            {isLoggedIn ? (
                <button>Logout</button>
            ) : (
                <button>Login</button>
            )}

            {/* Rendering lists */}
            <ul>
                {hobbies.map((hobby, index) => (
                    <li key={index}>{hobby}</li>
                ))}
            </ul>

            {/* Inline styles use objects with camelCase */}
            <p style={{ color: "blue", fontSize: "1.2rem", fontWeight: "bold" }}>
                Styled text
            </p>

            {/* Self-closing tags */}
            <img src="photo.jpg" alt="Photo" />
            <input type="text" />
            <br />
        </div>
    );
}

// ========== FRAGMENTS ==========
// When you don't want an extra DOM element:
function UserInfo() {
    return (
        <>
            <h2>Alice</h2>
            <p>Age: 25</p>
            <p>Email: alice@example.com</p>
        </>
    );
}
```

### 12.3 — Your First Components

```tsx
// ========== FUNCTIONAL COMPONENTS ==========
// Components are just functions that return JSX

// Simple component (no props)
function Header() {
    return (
        <header style={{ background: "#333", color: "white", padding: "1rem" }}>
            <h1>My App</h1>
        </header>
    );
}

// Component with props
interface GreetingProps {
    name: string;
    age?: number;  // optional
}

function Greeting({ name, age }: GreetingProps) {
    return (
        <div>
            <h2>Hello, {name}!</h2>
            {age && <p>You are {age} years old.</p>}
        </div>
    );
}

// ========== COMPONENT COMPOSITION ==========
function App() {
    return (
        <div>
            <Header />
            <main>
                <Greeting name="Alice" age={25} />
                <Greeting name="Bob" />
                <Greeting name="Charlie" age={35} />
            </main>
        </div>
    );
}

export default App;
```

## Week 13: Props & Component Patterns

### 13.1 — Props Deep Dive

```tsx
// ========== DEFINING PROP TYPES ==========

interface UserCardProps {
    // Required props
    name: string;
    email: string;

    // Optional props
    avatar?: string;
    bio?: string;
    role?: "admin" | "user" | "moderator";

    // Function props (event handlers)
    onEdit: (userId: string) => void;
    onDelete?: (userId: string) => void;

    // Children
    children?: React.ReactNode;
}

function UserCard({
    name,
    email,
    avatar,
    bio = "No bio provided",    // default value
    role = "user",
    onEdit,
    onDelete,
    children,
}: UserCardProps) {
    return (
        <div className="user-card">
            {avatar && <img src={avatar} alt={name} />}
            <h3>{name}</h3>
            <p>{email}</p>
            <p>{bio}</p>
            <span className={`badge badge-${role}`}>{role}</span>

            <div className="actions">
                <button onClick={() => onEdit(email)}>Edit</button>
                {onDelete && (
                    <button onClick={() => onDelete(email)}>Delete</button>
                )}
            </div>

            {/* Render children if provided */}
            {children && <div className="extra">{children}</div>}
        </div>
    );
}

// ========== USING THE COMPONENT ==========
function UserList() {
    const handleEdit = (userId: string) => {
        console.log(`Editing user: ${userId}`);
    };

    const handleDelete = (userId: string) => {
        console.log(`Deleting user: ${userId}`);
    };

    return (
        <div>
            <UserCard
                name="Alice"
                email="alice@example.com"
                avatar="/avatars/alice.jpg"
                role="admin"
                onEdit={handleEdit}
                onDelete={handleDelete}
            >
                <p>Additional content goes here</p>
            </UserCard>

            <UserCard
                name="Bob"
                email="bob@example.com"
                onEdit={handleEdit}
            />
        </div>
    );
}
```

### 13.2 — Rendering Lists

```tsx
interface Todo {
    id: number;
    text: string;
    completed: boolean;
    priority: "low" | "medium" | "high";
}

// Individual todo item component
interface TodoItemProps {
    todo: Todo;
    onToggle: (id: number) => void;
    onDelete: (id: number) => void;
}

function TodoItem({ todo, onToggle, onDelete }: TodoItemProps) {
    return (
        <li
            style={{
                textDecoration: todo.completed ? "line-through" : "none",
                opacity: todo.completed ? 0.6 : 1,
            }}
        >
            <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => onToggle(todo.id)}
            />
            <span>{todo.text}</span>
            <span className={`priority-${todo.priority}`}>
                [{todo.priority}]
            </span>
            <button onClick={() => onDelete(todo.id)}>🗑️</button>
        </li>
    );
}

// Parent component rendering the list
function TodoList() {
    const todos: Todo[] = [
        { id: 1, text: "Learn TypeScript", completed: true, priority: "high" },
        { id: 2, text: "Learn React", completed: false, priority: "high" },
        { id: 3, text: "Build a project", completed: false, priority: "medium" },
    ];

    const handleToggle = (id: number) => {
        console.log(`Toggle todo ${id}`);
    };

    const handleDelete = (id: number) => {
        console.log(`Delete todo ${id}`);
    };

    return (
        <div>
            <h2>My Todos ({todos.filter(t => !t.completed).length} remaining)</h2>

            {todos.length === 0 ? (
                <p>No todos yet! Add one above.</p>
            ) : (
                <ul>
                    {todos.map(todo => (
                        <TodoItem
                            key={todo.id}  // ALWAYS provide unique key!
                            todo={todo}
                            onToggle={handleToggle}
                            onDelete={handleDelete}
                        />
                    ))}
                </ul>
            )}
        </div>
    );
}
```

### 13.3 — Conditional Rendering Patterns

```tsx
interface DashboardProps {
    isLoading: boolean;
    error: string | null;
    user: { name: string; role: string } | null;
    notifications: string[];
}

function Dashboard({ isLoading, error, user, notifications }: DashboardProps) {
    // Pattern 1: Early return
    if (isLoading) {
        return <div className="spinner">Loading...</div>;
    }

    if (error) {
        return <div className="error">Error: {error}</div>;
    }

    if (!user) {
        return <div>Please log in</div>;
    }

    return (
        <div>
            <h1>Welcome, {user.name}!</h1>

            {/* Pattern 2: && operator (show if true) */}
            {user.role === "admin" && (
                <div className="admin-panel">
                    <h2>Admin Panel</h2>
                </div>
            )}

            {/* Pattern 3: Ternary (if-else) */}
            {notifications.length > 0 ? (
                <div className="notifications">
                    <h3>Notifications ({notifications.length})</h3>
                    <ul>
                        {notifications.map((note, i) => (
                            <li key={i}>{note}</li>
                        ))}
                    </ul>
                </div>
            ) : (
                <p>No new notifications</p>
            )}

            {/* Pattern 4: Extracted variable */}
            {(() => {
                switch (user.role) {
                    case "admin":
                        return <AdminView />;
                    case "moderator":
                        return <ModeratorView />;
                    default:
                        return <UserView />;
                }
            })()}
        </div>
    );
}

// Better pattern for switch - helper component or object map
const roleComponents: Record<string, React.FC> = {
    admin: AdminView,
    moderator: ModeratorView,
    user: UserView,
};

function RoleBasedContent({ role }: { role: string }) {
    const Component = roleComponents[role] || UserView;
    return <Component />;
}
```

## Week 14–15: State & Hooks

### 14.1 — useState

```tsx
import { useState } from "react";

// ========== BASIC STATE ==========
function Counter() {
    // useState returns [currentValue, setterFunction]
    const [count, setCount] = useState<number>(0);

    return (
        <div>
            <h2>Count: {count}</h2>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(count - 1)}>Decrement</button>
            <button onClick={() => setCount(0)}>Reset</button>
        </div>
    );
}

// ========== STRING STATE ==========
function SearchBar() {
    const [query, setQuery] = useState("");

    return (
        <div>
            <input
                type="text"
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Search..."
            />
            <p>Searching for: {query}</p>
            {query.length > 0 && (
                <button onClick={() => setQuery("")}>Clear</button>
            )}
        </div>
    );
}

// ========== BOOLEAN STATE ==========
function TogglePanel() {
    const [isOpen, setIsOpen] = useState(false);

    return (
        <div>
            <button onClick={() => setIsOpen(!isOpen)}>
                {isOpen ? "Close" : "Open"} Panel
            </button>

            {isOpen && (
                <div className="panel">
                    <h3>Panel Content</h3>
                    <p>This panel is toggleable!</p>
                </div>
            )}
        </div>
    );
}

// ========== OBJECT STATE ==========
interface UserProfile {
    name: string;
    email: string;
    bio: string;
}

function ProfileEditor() {
    const [profile, setProfile] = useState<UserProfile>({
        name: "",
        email: "",
        bio: "",
    });

    // Update a specific field (IMMUTABLE update!)
    const handleChange = (field: keyof UserProfile, value: string) => {
        setProfile(prev => ({
            ...prev,       // spread existing properties
            [field]: value, // override the one that changed
        }));
    };

    return (
        <form>
            <input
                value={profile.name}
                onChange={(e) => handleChange("name", e.target.value)}
                placeholder="Name"
            />
            <input
                value={profile.email}
                onChange={(e) => handleChange("email", e.target.value)}
                placeholder="Email"
            />
            <textarea
                value={profile.bio}
                onChange={(e) => handleChange("bio", e.target.value)}
                placeholder="Bio"
            />

            <pre>{JSON.stringify(profile, null, 2)}</pre>
        </form>
    );
}

// ========== ARRAY STATE (CRUD Operations) ==========
interface Todo {
    id: number;
    text: string;
    completed: boolean;
}

function TodoApp() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [inputValue, setInputValue] = useState("");
    const [nextId, setNextId] = useState(1);

    // CREATE - Add a todo
    const addTodo = () => {
        if (!inputValue.trim()) return;

        const newTodo: Todo = {
            id: nextId,
            text: inputValue.trim(),
            completed: false,
        };

        setTodos(prev => [...prev, newTodo]);  // append to array
        setInputValue("");                       // clear input
        setNextId(prev => prev + 1);
    };

    // UPDATE - Toggle completion
    const toggleTodo = (id: number) => {
        setTodos(prev =>
            prev.map(todo =>
                todo.id === id
                    ? { ...todo, completed: !todo.completed }
                    : todo
            )
        );
    };

    // DELETE - Remove a todo
    const deleteTodo = (id: number) => {
        setTodos(prev => prev.filter(todo => todo.id !== id));
    };

    // DERIVED STATE (computed from existing state)
    const completedCount = todos.filter(t => t.completed).length;
    const remainingCount = todos.length - completedCount;

    return (
        <div>
            <h1>Todo App</h1>

            {/* Add form */}
            <div>
                <input
                    value={inputValue}
                    onChange={(e) => setInputValue(e.target.value)}
                    onKeyDown={(e) => e.key === "Enter" && addTodo()}
                    placeholder="Add a todo..."
                />
                <button onClick={addTodo}>Add</button>
            </div>

            {/* Stats */}
            <p>{remainingCount} remaining, {completedCount} completed</p>

            {/* Todo list */}
            <ul>
                {todos.map(todo => (
                    <li key={todo.id}>
                        <input
                            type="checkbox"
                            checked={todo.completed}
                            onChange={() => toggleTodo(todo.id)}
                        />
                        <span
                            style={{
                                textDecoration: todo.completed ? "line-through" : "none",
                            }}
                        >
                            {todo.text}
                        </span>
                        <button onClick={() => deleteTodo(todo.id)}>Delete</button>
                    </li>
                ))}
            </ul>

            {todos.length === 0 && <p>No todos yet!</p>}
        </div>
    );
}
```

### 14.2 — useEffect

```tsx
import { useState, useEffect } from "react";

// ========== BASIC EFFECTS ==========
function Timer() {
    const [seconds, setSeconds] = useState(0);

    useEffect(() => {
        // This runs after every render
        document.title = `Timer: ${seconds}s`;
    });

    useEffect(() => {
        // This runs ONCE on mount (empty dependency array)
        console.log("Component mounted!");

        // Cleanup function runs on unmount
        return () => {
            console.log("Component unmounting!");
        };
    }, []);

    useEffect(() => {
        // This runs when `seconds` changes
        if (seconds === 60) {
            alert("One minute has passed!");
        }
    }, [seconds]);

    // Interval with cleanup
    useEffect(() => {
        const interval = setInterval(() => {
            setSeconds(prev => prev + 1);
        }, 1000);

        // CLEANUP: clear interval when component unmounts
        return () => clearInterval(interval);
    }, []);

    return <h1>Timer: {seconds}s</h1>;
}

// ========== DATA FETCHING ==========
interface Post {
    id: number;
    title: string;
    body: string;
    userId: number;
}

function PostList() {
    const [posts, setPosts] = useState<Post[]>([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        const fetchPosts = async () => {
            try {
                setLoading(true);
                setError(null);

                const response = await fetch(
                    "https://jsonplaceholder.typicode.com/posts?_limit=10"
                );

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const data: Post[] = await response.json();
                setPosts(data);
            } catch (err) {
                setError(err instanceof Error ? err.message : "An error occurred");
            } finally {
                setLoading(false);
            }
        };

        fetchPosts();
    }, []); // Empty array = run once on mount

    if (loading) return <div>Loading posts...</div>;
    if (error) return <div>Error: {error}</div>;

    return (
        <div>
            <h1>Posts</h1>
            {posts.map(post => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                    <p>{post.body}</p>
                </article>
            ))}
        </div>
    );
}

// ========== FETCHING WITH PARAMETERS ==========
function UserProfile({ userId }: { userId: number }) {
    const [user, setUser] = useState<any>(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        let isCancelled = false; // prevent state update after unmount

        const fetchUser = async () => {
            setLoading(true);
            try {
                const response = await fetch(
                    `https://jsonplaceholder.typicode.com/users/${userId}`
                );
                const data = await response.json();

                if (!isCancelled) {
                    setUser(data);
                }
            } catch (error) {
                if (!isCancelled) {
                    console.error("Failed to fetch user:", error);
                }
            } finally {
                if (!isCancelled) {
                    setLoading(false);
                }
            }
        };

        fetchUser();

        // Cleanup: cancel if userId changes or component unmounts
        return () => {
            isCancelled = true;
        };
    }, [userId]); // Re-fetch when userId changes!

    if (loading) return <p>Loading user...</p>;
    if (!user) return <p>User not found</p>;

    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    );
}

// ========== DEBOUNCED SEARCH ==========
function SearchWithDebounce() {
    const [query, setQuery] = useState("");
    const [results, setResults] = useState<any[]>([]);
    const [loading, setLoading] = useState(false);

    useEffect(() => {
        if (!query.trim()) {
            setResults([]);
            return;
        }

        // Debounce: wait 500ms after user stops typing
        const timeoutId = setTimeout(async () => {
            setLoading(true);
            try {
                const response = await fetch(
                    `https://jsonplaceholder.typicode.com/posts?q=${query}`
                );
                const data = await response.json();
                setResults(data);
            } finally {
                setLoading(false);
            }
        }, 500);

        // Cleanup: cancel timeout if query changes
        return () => clearTimeout(timeoutId);
    }, [query]);

    return (
        <div>
            <input
                value={query}
                onChange={e => setQuery(e.target.value)}
                placeholder="Search posts..."
            />
            {loading && <p>Searching...</p>}
            {results.map(post => (
                <div key={post.id}>{post.title}</div>
            ))}
        </div>
    );
}
```

### 14.3 — useRef

```tsx
import { useRef, useState, useEffect } from "react";

// ========== DOM REFERENCES ==========
function FocusInput() {
    const inputRef = useRef<HTMLInputElement>(null);

    const focusInput = () => {
        inputRef.current?.focus();
    };

    // Auto-focus on mount
    useEffect(() => {
        inputRef.current?.focus();
    }, []);

    return (
        <div>
            <input ref={inputRef} placeholder="I'll be auto-focused" />
            <button onClick={focusInput}>Focus Input</button>
        </div>
    );
}

// ========== PERSISTING VALUES (without re-renders) ==========
function StopWatch() {
    const [time, setTime] = useState(0);
    const [isRunning, setIsRunning] = useState(false);
    const intervalRef = useRef<number | null>(null);

    const start = () => {
        if (isRunning) return;
        setIsRunning(true);
        intervalRef.current = window.setInterval(() => {
            setTime(prev => prev + 1);
        }, 1000);
    };

    const stop = () => {
        if (intervalRef.current) {
            clearInterval(intervalRef.current);
        }
        setIsRunning(false);
    };

    const reset = () => {
        stop();
        setTime(0);
    };

    // Cleanup on unmount
    useEffect(() => {
        return () => {
            if (intervalRef.current) {
                clearInterval(intervalRef.current);
            }
        };
    }, []);

    return (
        <div>
            <h1>{time}s</h1>
            <button onClick={start} disabled={isRunning}>Start</button>
            <button onClick={stop} disabled={!isRunning}>Stop</button>
            <button onClick={reset}>Reset</button>
        </div>
    );
}

// ========== TRACKING PREVIOUS VALUE ==========
function usePrevious<T>(value: T): T | undefined {
    const ref = useRef<T>();

    useEffect(() => {
        ref.current = value;
    }, [value]);

    return ref.current;
}

function PriceTracker() {
    const [price, setPrice] = useState(100);
    const previousPrice = usePrevious(price);

    const priceChange = previousPrice !== undefined ? price - previousPrice : 0;

    return (
        <div>
            <h2>Price: ${price}</h2>
            {previousPrice !== undefined && (
                <p style={{ color: priceChange >= 0 ? "green" : "red" }}>
                    Change: {priceChange >= 0 ? "+" : ""}{priceChange}
                </p>
            )}
            <button onClick={() => setPrice(prev => prev + Math.round(Math.random() * 20 - 10))}>
                Update Price
            </button>
        </div>
    );
}
```

## Week 16: Forms & Events

### 16.1 — Controlled Forms

```tsx
import { useState, FormEvent, ChangeEvent } from "react";

// ========== COMPREHENSIVE FORM EXAMPLE ==========
interface FormData {
    firstName: string;
    lastName: string;
    email: string;
    password: string;
    age: number | "";
    role: string;
    agreeToTerms: boolean;
    bio: string;
    skills: string[];
}

interface FormErrors {
    [key: string]: string;
}

function RegistrationForm() {
    const [formData, setFormData] = useState<FormData>({
        firstName: "",
        lastName: "",
        email: "",
        password: "",
        age: "",
        role: "",
        agreeToTerms: false,
        bio: "",
        skills: [],
    });

    const [errors, setErrors] = useState<FormErrors>({});
    const [isSubmitting, setIsSubmitting] = useState(false);
    const [isSubmitted, setIsSubmitted] = useState(false);

    // Generic change handler for text inputs
    const handleChange = (
        e: ChangeEvent<HTMLInputElement | HTMLTextAreaElement | HTMLSelectElement>
    ) => {
        const { name, value, type } = e.target;

        setFormData(prev => ({
            ...prev,
            [name]: type === "checkbox"
                ? (e.target as HTMLInputElement).checked
                : type === "number"
                    ? value === "" ? "" : Number(value)
                    : value,
        }));

        // Clear error when user starts typing
        if (errors[name]) {
            setErrors(prev => {
                const next = { ...prev };
                delete next[name];
                return next;
            });
        }
    };

    // Checkbox group handler
    const handleSkillChange = (skill: string) => {
        setFormData(prev => ({
            ...prev,
            skills: prev.skills.includes(skill)
                ? prev.skills.filter(s => s !== skill)
                : [...prev.skills, skill],
        }));
    };

    // Validation
    const validate = (): FormErrors => {
        const newErrors: FormErrors = {};

        if (!formData.firstName.trim()) {
            newErrors.firstName = "First name is required";
        }

        if (!formData.lastName.trim()) {
            newErrors.lastName = "Last name is required";
        }

        if (!formData.email.trim()) {
            newErrors.email = "Email is required";
        } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
            newErrors.email = "Invalid email format";
        }

        if (!formData.password) {
            newErrors.password = "Password is required";
        } else if (formData.password.length < 8) {
            newErrors.password = "Password must be at least 8 characters";
        }

        if (!formData.role) {
            newErrors.role = "Please select a role";
        }

        if (!formData.agreeToTerms) {
            newErrors.agreeToTerms = "You must agree to the terms";
        }

        return newErrors;
    };

    // Submit handler
    const handleSubmit = async (e: FormEvent<HTMLFormElement>) => {
        e.preventDefault();

        const validationErrors = validate();
        if (Object.keys(validationErrors).length > 0) {
            setErrors(validationErrors);
            return;
        }

        setIsSubmitting(true);
        try {
            // Simulate API call
            await new Promise(resolve => setTimeout(resolve, 1500));
            console.log("Form submitted:", formData);
            setIsSubmitted(true);
        } catch (error) {
            console.error("Submission failed:", error);
        } finally {
            setIsSubmitting(false);
        }
    };

    if (isSubmitted) {
        return (
            <div className="success">
                <h2>Registration Successful! ✅</h2>
                <p>Welcome, {formData.firstName}!</p>
            </div>
        );
    }

    const availableSkills = ["JavaScript", "TypeScript", "React", "Node.js", "Python"];

    return (
        <form onSubmit={handleSubmit} noValidate>
            <h2>Registration Form</h2>

            {/* Text inputs */}
            <div className="field">
                <label htmlFor="firstName">First Name *</label>
                <input
                    id="firstName"
                    name="firstName"
                    value={formData.firstName}
                    onChange={handleChange}
                    className={errors.firstName ? "error" : ""}
                />
                {errors.firstName && <span className="error-msg">{errors.firstName}</span>}
            </div>

            <div className="field">
                <label htmlFor="lastName">Last Name *</label>
                <input
                    id="lastName"
                    name="lastName"
                    value={formData.lastName}
                    onChange={handleChange}
                />
                {errors.lastName && <span className="error-msg">{errors.lastName}</span>}
            </div>

            {/* Email */}
            <div className="field">
                <label htmlFor="email">Email *</label>
                <input
                    id="email"
                    name="email"
                    type="email"
                    value={formData.email}
                    onChange={handleChange}
                />
                {errors.email && <span className="error-msg">{errors.email}</span>}
            </div>

            {/* Password */}
            <div className="field">
                <label htmlFor="password">Password *</label>
                <input
                    id="password"
                    name="password"
                    type="password"
                    value={formData.password}
                    onChange={handleChange}
                />
                {errors.password && <span className="error-msg">{errors.password}</span>}
            </div>

            {/* Number */}
            <div className="field">
                <label htmlFor="age">Age</label>
                <input
                    id="age"
                    name="age"
                    type="number"
                    value={formData.age}
                    onChange={handleChange}
                    min={13}
                    max={120}
                />
            </div>

            {/* Select */}
            <div className="field">
                <label htmlFor="role">Role *</label>
                <select
                    id="role"
                    name="role"
                    value={formData.role}
                    onChange={handleChange}
                >
                    <option value="">Select a role</option>
                    <option value="developer">Developer</option>
                    <option value="designer">Designer</option>
                    <option value="manager">Manager</option>
                </select>
                {errors.role && <span className="error-msg">{errors.role}</span>}
            </div>

            {/* Checkbox group */}
            <div className="field">
                <label>Skills</label>
                {availableSkills.map(skill => (
                    <label key={skill} style={{ display: "block" }}>
                        <input
                            type="checkbox"
                            checked={formData.skills.includes(skill)}
                            onChange={() => handleSkillChange(skill)}
                        />
                        {skill}
                    </label>
                ))}
            </div>

            {/* Textarea */}
            <div className="field">
                <label htmlFor="bio">Bio</label>
                <textarea
                    id="bio"
                    name="bio"
                    value={formData.bio}
                    onChange={handleChange}
                    rows={4}
                />
            </div>

            {/* Single checkbox */}
            <div className="field">
                <label>
                    <input
                        type="checkbox"
                        name="agreeToTerms"
                        checked={formData.agreeToTerms}
                        onChange={handleChange}
                    />
                    I agree to the terms and conditions *
                </label>
                {errors.agreeToTerms && (
                    <span className="error-msg">{errors.agreeToTerms}</span>
                )}
            </div>

            <button type="submit" disabled={isSubmitting}>
                {isSubmitting ? "Submitting..." : "Register"}
            </button>
        </form>
    );
}
```

### 📝 Phase 4 Practice Projects:
1. **Todo App** (complete CRUD with filters)
2. **Weather Dashboard** (fetch API data, display with loading/error states)
3. **Multi-step Form** (wizard pattern with validation)

---

---

# 📒 PHASE 5: Advanced React (Weeks 17–20)

---

## Week 17: Custom Hooks

### 17.1 — Creating Reusable Hooks

```tsx
import { useState, useEffect, useCallback } from "react";

// ========== useFetch - Reusable data fetching ==========
interface UseFetchResult<T> {
    data: T | null;
    loading: boolean;
    error: string | null;
    refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {
    const [data, setData] = useState<T | null>(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    const fetchData = useCallback(async () => {
        try {
            setLoading(true);
            setError(null);
            const response = await fetch(url);
            if (!response.ok) throw new Error(`HTTP ${response.status}`);
            const json = await response.json();
            setData(json);
        } catch (err) {
            setError(err instanceof Error ? err.message : "Unknown error");
        } finally {
            setLoading(false);
        }
    }, [url]);

    useEffect(() => {
        fetchData();
    }, [fetchData]);

    return { data, loading, error, refetch: fetchData };
}

// Usage:
function PostsPage() {
    const { data: posts, loading, error, refetch } = useFetch<Post[]>(
        "https://jsonplaceholder.typicode.com/posts"
    );

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error} <button onClick={refetch}>Retry</button></p>;

    return (
        <div>
            {posts?.map(post => <div key={post.id}>{post.title}</div>)}
        </div>
    );
}

// ========== useLocalStorage ==========
function useLocalStorage<T>(key: string, initialValue: T) {
    const [storedValue, setStoredValue] = useState<T>(() => {
        try {
            const item = localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch {
            return initialValue;
        }
    });

    const setValue = (value: T | ((prev: T) => T)) => {
        const valueToStore = value instanceof Function ? value(storedValue) : value;
        setStoredValue(valueToStore);
        localStorage.setItem(key, JSON.stringify(valueToStore));
    };

    return [storedValue, setValue] as const;
}

// Usage:
function Settings() {
    const [theme, setTheme] = useLocalStorage("theme", "light");
    const [fontSize, setFontSize] = useLocalStorage("fontSize", 16);

    return (
        <div>
            <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
                Theme: {theme}
            </button>
            <button onClick={() => setFontSize(prev => prev + 1)}>
                Font Size: {fontSize}
            </button>
        </div>
    );
}

// ========== useDebounce ==========
function useDebounce<T>(value: T, delay: number): T {
    const [debouncedValue, setDebouncedValue] = useState(value);

    useEffect(() => {
        const timer = setTimeout(() => setDebouncedValue(value), delay);
        return () => clearTimeout(timer);
    }, [value, delay]);

    return debouncedValue;
}

// Usage:
function Search() {
    const [query, setQuery] = useState("");
    const debouncedQuery = useDebounce(query, 500);

    const { data: results } = useFetch<any[]>(
        debouncedQuery
            ? `https://api.example.com/search?q=${debouncedQuery}`
            : ""
    );

    return (
        <div>
            <input
                value={query}
                onChange={e => setQuery(e.target.value)}
                placeholder="Search..."
            />
            {results?.map(item => <div key={item.id}>{item.name}</div>)}
        </div>
    );
}

// ========== useToggle ==========
function useToggle(initialValue = false) {
    const [value, setValue] = useState(initialValue);

    const toggle = useCallback(() => setValue(prev => !prev), []);
    const setTrue = useCallback(() => setValue(true), []);
    const setFalse = useCallback(() => setValue(false), []);

    return { value, toggle, setTrue, setFalse };
}

// Usage:
function Modal() {
    const { value: isOpen, toggle, setFalse: close } = useToggle();

    return (
        <div>
            <button onClick={toggle}>Toggle Modal</button>
            {isOpen && (
                <div className="modal-overlay" onClick={close}>
                    <div className="modal" onClick={e => e.stopPropagation()}>
                        <h2>Modal Title</h2>
                        <p>Modal content here</p>
                        <button onClick={close}>Close</button>
                    </div>
                </div>
            )}
        </div>
    );
}
```

## Week 18: Context API & useReducer

### 18.1 — Context API (Global State)

```tsx
import { createContext, useContext, useState, ReactNode } from "react";

// ========== STEP 1: Define Types ==========
interface User {
    id: string;
    name: string;
    email: string;
    role: "admin" | "user";
}

interface AuthContextType {
    user: User | null;
    isAuthenticated: boolean;
    login: (email: string, password: string) => Promise<void>;
    logout: () => void;
    loading: boolean;
}

// ========== STEP 2: Create Context ==========
const AuthContext = createContext<AuthContextType | undefined>(undefined);

// ========== STEP 3: Create Provider ==========
function AuthProvider({ children }: { children: ReactNode }) {
    const [user, setUser] = useState<User | null>(null);
    const [loading, setLoading] = useState(false);

    const login = async (email: string, password: string) => {
        setLoading(true);
        try {
            // Simulate API call
            await new Promise(resolve => setTimeout(resolve, 1000));

            const loggedInUser: User = {
                id: "1",
                name: "Alice",
                email,
                role: "admin",
            };

            setUser(loggedInUser);
        } catch (error) {
            throw new Error("Login failed");
        } finally {
            setLoading(false);
        }
    };

    const logout = () => {
        setUser(null);
    };

    const value: AuthContextType = {
        user,
        isAuthenticated: !!user,
        login,
        logout,
        loading,
    };

    return (
        <AuthContext.Provider value={value}>
            {children}
        </AuthContext.Provider>
    );
}

// ========== STEP 4: Create Custom Hook ==========
function useAuth(): AuthContextType {
    const context = useContext(AuthContext);
    if (context === undefined) {
        throw new Error("useAuth must be used within an AuthProvider");
    }
    return context;
}

// ========== STEP 5: Wrap App with Provider ==========
function App() {
    return (
        <AuthProvider>
            <Header />
            <MainContent />
        </AuthProvider>
    );
}

// ========== STEP 6: Use in Components ==========
function Header() {
    const { user, isAuthenticated, logout } = useAuth();

    return (
        <header>
            {isAuthenticated ? (
                <div>
                    <span>Welcome, {user?.name}!</span>
                    <button onClick={logout}>Logout</button>
                </div>
            ) : (
                <span>Please log in</span>
            )}
        </header>
    );
}

function LoginForm() {
    const { login, loading } = useAuth();
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    const handleSubmit = async (e: FormEvent) => {
        e.preventDefault();
        await login(email, password);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input value={email} onChange={e => setEmail(e.target.value)} placeholder="Email" />
            <input value={password} onChange={e => setPassword(e.target.value)} type="password" />
            <button disabled={loading}>{loading ? "Logging in..." : "Login"}</button>
        </form>
    );
}
```

### 18.2 — useReducer (Complex State Logic)

```tsx
import { useReducer, createContext, useContext, ReactNode } from "react";

// ========== TYPES ==========
interface CartItem {
    id: string;
    name: string;
    price: number;
    quantity: number;
    image: string;
}

interface CartState {
    items: CartItem[];
    isOpen: boolean;
}

// ========== ACTIONS (Discriminated Union) ==========
type CartAction =
    | { type: "ADD_ITEM"; payload: Omit<CartItem, "quantity"> }
    | { type: "REMOVE_ITEM"; payload: { id: string } }
    | { type: "UPDATE_QUANTITY"; payload: { id: string; quantity: number } }
    | { type: "CLEAR_CART" }
    | { type: "TOGGLE_CART" };

// ========== REDUCER ==========
function cartReducer(state: CartState, action: CartAction): CartState {
    switch (action.type) {
        case "ADD_ITEM": {
            const existingItem = state.items.find(item => item.id === action.payload.id);

            if (existingItem) {
                return {
                    ...state,
                    items: state.items.map(item =>
                        item.id === action.payload.id
                            ? { ...item, quantity: item.quantity + 1 }
                            : item
                    ),
                };
            }

            return {
                ...state,
                items: [...state.items, { ...action.payload, quantity: 1 }],
            };
        }

        case "REMOVE_ITEM":
            return {
                ...state,
                items: state.items.filter(item => item.id !== action.payload.id),
            };

        case "UPDATE_QUANTITY":
            if (action.payload.quantity <= 0) {
                return {
                    ...state,
                    items: state.items.filter(item => item.id !== action.payload.id),
                };
            }
            return {
                ...state,
                items: state.items.map(item =>
                    item.id === action.payload.id
                        ? { ...item, quantity: action.payload.quantity }
                        : item
                ),
            };

        case "CLEAR_CART":
            return { ...state, items: [] };

        case "TOGGLE_CART":
            return { ...state, isOpen: !state.isOpen };

        default:
            return state;
    }
}

// ========== CONTEXT + PROVIDER ==========
interface CartContextType {
    state: CartState;
    addItem: (item: Omit<CartItem, "quantity">) => void;
    removeItem: (id: string) => void;
    updateQuantity: (id: string, quantity: number) => void;
    clearCart: () => void;
    toggleCart: () => void;
    totalItems: number;
    totalPrice: number;
}

const CartContext = createContext<CartContextType | undefined>(undefined);

function CartProvider({ children }: { children: ReactNode }) {
    const [state, dispatch] = useReducer(cartReducer, {
        items: [],
        isOpen: false,
    });

    const addItem = (item: Omit<CartItem, "quantity">) => {
        dispatch({ type: "ADD_ITEM", payload: item });
    };

    const removeItem = (id: string) => {
        dispatch({ type: "REMOVE_ITEM", payload: { id } });
    };

    const updateQuantity = (id: string, quantity: number) => {
        dispatch({ type: "UPDATE_QUANTITY", payload: { id, quantity } });
    };

    const clearCart = () => dispatch({ type: "CLEAR_CART" });
    const toggleCart = () => dispatch({ type: "TOGGLE_CART" });

    // Derived values
    const totalItems = state.items.reduce((sum, item) => sum + item.quantity, 0);
    const totalPrice = state.items.reduce(
        (sum, item) => sum + item.price * item.quantity, 0
    );

    return (
        <CartContext.Provider value={{
            state, addItem, removeItem, updateQuantity,
            clearCart, toggleCart, totalItems, totalPrice,
        }}>
            {children}
        </CartContext.Provider>
    );
}

function useCart() {
    const context = useContext(CartContext);
    if (!context) throw new Error("useCart must be used within CartProvider");
    return context;
}

// ========== COMPONENTS ==========
function CartIcon() {
    const { totalItems, toggleCart } = useCart();

    return (
        <button onClick={toggleCart} className="cart-icon">
            🛒 {totalItems > 0 && <span className="badge">{totalItems}</span>}
        </button>
    );
}

function ProductCard({ product }: { product: Omit<CartItem, "quantity"> }) {
    const { addItem } = useCart();

    return (
        <div className="product-card">
            <img src={product.image} alt={product.name} />
            <h3>{product.name}</h3>
            <p>${product.price.toFixed(2)}</p>
            <button onClick={() => addItem(product)}>Add to Cart</button>
        </div>
    );
}

function CartSidebar() {
    const { state, removeItem, updateQuantity, clearCart, totalPrice } = useCart();

    if (!state.isOpen) return null;

    return (
        <div className="cart-sidebar">
            <h2>Shopping Cart</h2>

            {state.items.length === 0 ? (
                <p>Your cart is empty</p>
            ) : (
                <>
                    {state.items.map(item => (
                        <div key={item.id} className="cart-item">
                            <span>{item.name}</span>
                            <div>
                                <button onClick={() => updateQuantity(item.id, item.quantity - 1)}>
                                    -
                                </button>
                                <span>{item.quantity}</span>
                                <button onClick={() => updateQuantity(item.id, item.quantity + 1)}>
                                    +
                                </button>
                            </div>
                            <span>${(item.price * item.quantity).toFixed(2)}</span>
                            <button onClick={() => removeItem(item.id)}>🗑️</button>
                        </div>
                    ))}

                    <div className="cart-total">
                        <strong>Total: ${totalPrice.toFixed(2)}</strong>
                    </div>

                    <button onClick={clearCart}>Clear Cart</button>
                    <button className="checkout-btn">Checkout</button>
                </>
            )}
        </div>
    );
}
```

## Week 19–20: Performance & Advanced Patterns

### 19.1 — React.memo, useMemo, useCallback

```tsx
import { useState, useMemo, useCallback, memo } from "react";

// ========== React.memo - Prevent re-renders of child components ==========
interface ExpensiveListItemProps {
    item: { id: number; name: string };
    onSelect: (id: number) => void;
}

// Without memo: re-renders every time parent renders
// With memo: only re-renders when its props change
const ExpensiveListItem = memo(function ExpensiveListItem({
    item,
    onSelect,
}: ExpensiveListItemProps) {
    console.log(`Rendering item: ${item.name}`);

    return (
        <li onClick={() => onSelect(item.id)}>
            {item.name}
        </li>
    );
});

// ========== useMemo - Memoize expensive calculations ==========
// ========== useCallback - Memoize functions ==========

function ProductList() {
    const [products] = useState([
        { id: 1, name: "Laptop", price: 999, category: "Electronics" },
        { id: 2, name: "Shirt", price: 29, category: "Clothing" },
        { id: 3, name: "Book", price: 15, category: "Books" },
        // ... imagine hundreds of products
    ]);

    const [searchQuery, setSearchQuery] = useState("");
    const [sortBy, setSortBy] = useState<"name" | "price">("name");
    const [selectedId, setSelectedId] = useState<number | null>(null);

    // useMemo: only recalculate when dependencies change
    const filteredAndSortedProducts = useMemo(() => {
        console.log("Recalculating products...");  // only runs when needed

        return products
            .filter(p =>
                p.name.toLowerCase().includes(searchQuery.toLowerCase())
            )
            .sort((a, b) =>
                sortBy === "name"
                    ? a.name.localeCompare(b.name)
                    : a.price - b.price
            );
    }, [products, searchQuery, sortBy]); // only recalculate when these change

    const totalPrice = useMemo(
        () => filteredAndSortedProducts.reduce((sum, p) => sum + p.price, 0),
        [filteredAndSortedProducts]
    );

    // useCallback: memoize function so child components don't re-render
    const handleSelect = useCallback((id: number) => {
        setSelectedId(id);
    }, []); // stable reference

    return (
        <div>
            <input
                value={searchQuery}
                onChange={e => setSearchQuery(e.target.value)}
                placeholder="Search products..."
            />

            <select value={sortBy} onChange={e => setSortBy(e.target.value as any)}>
                <option value="name">Sort by Name</option>
                <option value="price">Sort by Price</option>
            </select>

            <p>Total: ${totalPrice} ({filteredAndSortedProducts.length} products)</p>

            <ul>
                {filteredAndSortedProducts.map(product => (
                    <ExpensiveListItem
                        key={product.id}
                        item={product}
                        onSelect={handleSelect}  // stable reference thanks to useCallback
                    />
                ))}
            </ul>
        </div>
    );
}
```

---

---

# 📓 PHASE 6: Ecosystem & Tools (Weeks 21–24)

---

## Week 21: React Router

```bash
npm install react-router-dom
```

```tsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link,
    NavLink,
    useNavigate,
    useParams,
    useSearchParams,
    Outlet,
    Navigate,
} from "react-router-dom";

// ========== APP WITH ROUTING ==========
function App() {
    return (
        <BrowserRouter>
            <Routes>
                {/* Layout route */}
                <Route path="/" element={<Layout />}>
                    {/* Index route (default child) */}
                    <Route index element={<HomePage />} />

                    {/* Static routes */}
                    <Route path="about" element={<AboutPage />} />
                    <Route path="contact" element={<ContactPage />} />

                    {/* Dynamic route */}
                    <Route path="users/:userId" element={<UserProfile />} />

                    {/* Nested routes */}
                    <Route path="dashboard" element={<ProtectedRoute />}>
                        <Route index element={<DashboardHome />} />
                        <Route path="settings" element={<Settings />} />
                        <Route path="analytics" element={<Analytics />} />
                    </Route>

                    {/* 404 catch-all */}
                    <Route path="*" element={<NotFound />} />
                </Route>
            </Routes>
        </BrowserRouter>
    );
}

// ========== LAYOUT COMPONENT ==========
function Layout() {
    return (
        <div>
            <nav>
                {/* NavLink adds "active" class automatically */}
                <NavLink to="/" end className={({ isActive }) =>
                    isActive ? "nav-link active" : "nav-link"
                }>
                    Home
                </NavLink>
                <NavLink to="/about" className={({ isActive }) =>
                    isActive ? "nav-link active" : "nav-link"
                }>
                    About
                </NavLink>
                <NavLink to="/dashboard">Dashboard</NavLink>
            </nav>

            {/* Child routes render here */}
            <main>
                <Outlet />
            </main>

            <footer>© 2024 My App</footer>
        </div>
    );
}

// ========== DYNAMIC ROUTE ==========
function UserProfile() {
    const { userId } = useParams<{ userId: string }>();
    const navigate = useNavigate();

    return (
        <div>
            <h1>User Profile: {userId}</h1>
            <button onClick={() => navigate("/")}>Go Home</button>
            <button onClick={() => navigate(-1)}>Go Back</button>
        </div>
    );
}

// ========== PROTECTED ROUTE ==========
function ProtectedRoute() {
    const { isAuthenticated } = useAuth();

    if (!isAuthenticated) {
        return <Navigate to="/login" replace />;
    }

    return <Outlet />;
}

// ========== SEARCH PARAMS ==========
function ProductsPage() {
    const [searchParams, setSearchParams] = useSearchParams();

    const category = searchParams.get("category") || "all";
    const page = Number(searchParams.get("page")) || 1;

    const handleCategoryChange = (cat: string) => {
        setSearchParams({ category: cat, page: "1" });
    };

    return (
        <div>
            <h1>Products - {category} (Page {page})</h1>
            <button onClick={() => handleCategoryChange("electronics")}>Electronics</button>
            <button onClick={() => handleCategoryChange("clothing")}>Clothing</button>
        </div>
    );
}
```

## Week 22: State Management (Zustand - Recommended for beginners)

```bash
npm install zustand
```

```tsx
import { create } from "zustand";
import { devtools, persist } from "zustand/middleware";

// ========== SIMPLE STORE ==========
interface TodoState {
    todos: Todo[];
    filter: "all" | "active" | "completed";

    // Actions
    addTodo: (text: string) => void;
    toggleTodo: (id: number) => void;
    deleteTodo: (id: number) => void;
    setFilter: (filter: "all" | "active" | "completed") => void;
    clearCompleted: () => void;
}

let nextId = 1;

const useTodoStore = create<TodoState>()(
    devtools(
        persist(
            (set, get) => ({
                todos: [],
                filter: "all",

                addTodo: (text) =>
                    set(
                        (state) => ({
                            todos: [
                                ...state.todos,
                                { id: nextId++, text, completed: false },
                            ],
                        }),
                        false,
                        "addTodo"
                    ),

                toggleTodo: (id) =>
                    set(
                        (state) => ({
                            todos: state.todos.map((todo) =>
                                todo.id === id
                                    ? { ...todo, completed: !todo.completed }
                                    : todo
                            ),
                        }),
                        false,
                        "toggleTodo"
                    ),

                deleteTodo: (id) =>
                    set(
                        (state) => ({
                            todos: state.todos.filter((todo) => todo.id !== id),
                        }),
                        false,
                        "deleteTodo"
                    ),

                setFilter: (filter) => set({ filter }),

                clearCompleted: () =>
                    set((state) => ({
                        todos: state.todos.filter((t) => !t.completed),
                    })),
            }),
            { name: "todo-storage" }  // localStorage key
        )
    )
);

// ========== USING THE STORE IN COMPONENTS ==========
// No Provider needed! Just use the hook directly.

function TodoApp() {
    const todos = useTodoStore((state) => state.todos);
    const filter = useTodoStore((state) => state.filter);
    const addTodo = useTodoStore((state) => state.addTodo);
    const setFilter = useTodoStore((state) => state.setFilter);

    const [input, setInput] = useState("");

    const filteredTodos = todos.filter((todo) => {
        if (filter === "active") return !todo.completed;
        if (filter === "completed") return todo.completed;
        return true;
    });

    return (
        <div>
            <input
                value={input}
                onChange={(e) => setInput(e.target.value)}
                onKeyDown={(e) => {
                    if (e.key === "Enter" && input.trim()) {
                        addTodo(input.trim());
                        setInput("");
                    }
                }}
            />

            <div>
                <button onClick={() => setFilter("all")}>All</button>
                <button onClick={() => setFilter("active")}>Active</button>
                <button onClick={() => setFilter("completed")}>Completed</button>
            </div>

            <ul>
                {filteredTodos.map((todo) => (
                    <TodoItemComponent key={todo.id} todo={todo} />
                ))}
            </ul>
        </div>
    );
}

function TodoItemComponent({ todo }: { todo: Todo }) {
    const toggleTodo = useTodoStore((state) => state.toggleTodo);
    const deleteTodo = useTodoStore((state) => state.deleteTodo);

    return (
        <li>
            <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => toggleTodo(todo.id)}
            />
            <span>{todo.text}</span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
        </li>
    );
}
```

## Week 23: API Integration with React Query (TanStack Query)

```bash
npm install @tanstack/react-query
```

```tsx
import {
    QueryClient,
    QueryClientProvider,
    useQuery,
    useMutation,
    useQueryClient,
} from "@tanstack/react-query";

// ========== SETUP ==========
const queryClient = new QueryClient({
    defaultOptions: {
        queries: {
            staleTime: 5 * 60 * 1000,  // 5 minutes
            retry: 3,
        },
    },
});

function App() {
    return (
        <QueryClientProvider client={queryClient}>
            <PostsApp />
        </QueryClientProvider>
    );
}

// ========== API FUNCTIONS ==========
const API_URL = "https://jsonplaceholder.typicode.com";

interface Post {
    id: number;
    title: string;
    body: string;
    userId: number;
}

async function fetchPosts(): Promise<Post[]> {
    const response = await fetch(`${API_URL}/posts?_limit=20`);
    if (!response.ok) throw new Error("Failed to fetch posts");
    return response.json();
}

async function fetchPost(id: number): Promise<Post> {
    const response = await fetch(`${API_URL}/posts/${id}`);
    if (!response.ok) throw new Error("Failed to fetch post");
    return response.json();
}

async function createPost(data: Omit<Post, "id">): Promise<Post> {
    const response = await fetch(`${API_URL}/posts`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
    });
    if (!response.ok) throw new Error("Failed to create post");
    return response.json();
}

// ========== READING DATA ==========
function PostsList() {
    const {
        data: posts,
        isLoading,
        isError,
        error,
        refetch,
    } = useQuery({
        queryKey: ["posts"],          // cache key
        queryFn: fetchPosts,          // fetch function
    });

    if (isLoading) return <div>Loading posts...</div>;
    if (isError) return <div>Error: {error.message} <button onClick={() => refetch()}>Retry</button></div>;

    return (
        <div>
            <h1>Posts</h1>
            {posts?.map(post => (
                <PostCard key={post.id} post={post} />
            ))}
        </div>
    );
}

// ========== DETAIL VIEW WITH CACHE ==========
function PostDetail({ postId }: { postId: number }) {
    const { data: post, isLoading } = useQuery({
        queryKey: ["posts", postId],    // unique cache key per post
        queryFn: () => fetchPost(postId),
        enabled: !!postId,               // don't fetch if no postId
    });

    if (isLoading) return <p>Loading...</p>;
    if (!post) return <p>Post not found</p>;

    return (
        <article>
            <h2>{post.title}</h2>
            <p>{post.body}</p>
        </article>
    );
}

// ========== MUTATIONS (Create, Update, Delete) ==========
function CreatePostForm() {
    const queryClient = useQueryClient();
    const [title, setTitle] = useState("");
    const [body, setBody] = useState("");

    const mutation = useMutation({
        mutationFn: createPost,
        onSuccess: (newPost) => {
            // Invalidate and refetch the posts list
            queryClient.invalidateQueries({ queryKey: ["posts"] });

            // Or optimistically add to cache
            // queryClient.setQueryData(["posts"], (old: Post[]) => [...old, newPost]);

            setTitle("");
            setBody("");
        },
        onError: (error) => {
            alert(`Error: ${error.message}`);
        },
    });

    const handleSubmit = (e: FormEvent) => {
        e.preventDefault();
        mutation.mutate({ title, body, userId: 1 });
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                value={title}
                onChange={e => setTitle(e.target.value)}
                placeholder="Title"
                required
            />
            <textarea
                value={body}
                onChange={e => setBody(e.target.value)}
                placeholder="Body"
                required
            />
            <button type="submit" disabled={mutation.isPending}>
                {mutation.isPending ? "Creating..." : "Create Post"}
            </button>
            {mutation.isError && <p className="error">{mutation.error.message}</p>}
        </form>
    );
}
```

## Week 24: Styling Solutions

### CSS Modules

```tsx
// Button.module.css
// .button { padding: 8px 16px; border-radius: 4px; }
// .primary { background: blue; color: white; }
// .secondary { background: gray; color: white; }

import styles from "./Button.module.css";

function Button({ variant = "primary", children }: ButtonProps) {
    return (
        <button className={`${styles.button} ${styles[variant]}`}>
            {children}
        </button>
    );
}
```

### Tailwind CSS (Most Popular)

```bash
npm install -D tailwindcss @tailwindcss/vite
```

```tsx
// With Tailwind CSS
function Card({ title, description, imageUrl }: CardProps) {
    return (
        <div className="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow">
            {imageUrl && (
                <img
                    src={imageUrl}
                    alt={title}
                    className="w-full h-48 object-cover"
                />
            )}
            <div className="p-6">
                <h3 className="text-xl font-semibold text-gray-800 mb-2">
                    {title}
                </h3>
                <p className="text-gray-600 leading-relaxed">
                    {description}
                </p>
                <button className="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors">
                    Learn More
                </button>
            </div>
        </div>
    );
}

// Responsive with Tailwind
function ResponsiveGrid({ children }: { children: React.ReactNode }) {
    return (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 p-6">
            {children}
        </div>
    );
}
```

---

---

# 📔 PHASE 7: Testing & Deployment (Weeks 25–27)

---

## Week 25: Testing

```bash
# Vitest (recommended with Vite projects)
npm install -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

```tsx
// ========== UNIT TEST: Utility functions ==========
// utils.test.ts
import { describe, it, expect } from "vitest";
import { formatPrice, capitalize, isValidEmail } from "./utils";

describe("formatPrice", () => {
    it("formats whole numbers", () => {
        expect(formatPrice(10)).toBe("$10.00");
    });

    it("formats decimals", () => {
        expect(formatPrice(9.99)).toBe("$9.99");
    });

    it("handles zero", () => {
        expect(formatPrice(0)).toBe("$0.00");
    });
});

describe("isValidEmail", () => {
    it("accepts valid emails", () => {
        expect(isValidEmail("test@example.com")).toBe(true);
        expect(isValidEmail("user.name@domain.org")).toBe(true);
    });

    it("rejects invalid emails", () => {
        expect(isValidEmail("not-an-email")).toBe(false);
        expect(isValidEmail("@domain.com")).toBe(false);
        expect(isValidEmail("")).toBe(false);
    });
});

// ========== COMPONENT TEST ==========
// Counter.test.tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, it, expect } from "vitest";
import Counter from "./Counter";

describe("Counter", () => {
    it("renders initial count of 0", () => {
        render(<Counter />);
        expect(screen.getByText("Count: 0")).toBeInTheDocument();
    });

    it("increments when + button is clicked", async () => {
        const user = userEvent.setup();
        render(<Counter />);

        await user.click(screen.getByRole("button", { name: /increment/i }));

        expect(screen.getByText("Count: 1")).toBeInTheDocument();
    });

    it("decrements when - button is clicked", async () => {
        const user = userEvent.setup();
        render(<Counter />);

        await user.click(screen.getByRole("button", { name: /decrement/i }));

        expect(screen.getByText("Count: -1")).toBeInTheDocument();
    });
});

// ========== FORM TEST ==========
// LoginForm.test.tsx
import { render, screen, waitFor } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import LoginForm from "./LoginForm";

describe("LoginForm", () => {
    it("shows validation errors for empty fields", async () => {
        const user = userEvent.setup();
        render(<LoginForm onSubmit={vi.fn()} />);

        await user.click(screen.getByRole("button", { name: /login/i }));

        expect(screen.getByText(/email is required/i)).toBeInTheDocument();
        expect(screen.getByText(/password is required/i)).toBeInTheDocument();
    });

    it("calls onSubmit with form data", async () => {
        const handleSubmit = vi.fn();
        const user = userEvent.setup();
        render(<LoginForm onSubmit={handleSubmit} />);

        await user.type(screen.getByLabelText(/email/i), "test@example.com");
        await user.type(screen.getByLabelText(/password/i), "password123");
        await user.click(screen.getByRole("button", { name: /login/i }));

        await waitFor(() => {
            expect(handleSubmit).toHaveBeenCalledWith({
                email: "test@example.com",
                password: "password123",
            });
        });
    });
});
```

## Week 26–27: Build & Deploy

```bash
# Build for production
npm run build

# Preview production build locally
npm run preview
```

### Deploy to Vercel (Easiest)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Or connect GitHub repo at vercel.com for auto-deployments
```

### Deploy to Netlify

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=dist
```

---

---

# 🏆 PHASE 8: Real-World Projects (Weeks 28–32)

Build these projects to solidify your skills and create a portfolio:

---

## Project 1: Task Management App (Trello Clone)

```
Features:
✅ User authentication (login/register)
✅ Create/edit/delete boards
✅ Create/edit/delete lists within boards
✅ Create/edit/delete cards within lists
✅ Drag and drop cards between lists
✅ Labels, due dates, descriptions on cards
✅ Search and filter

Tech Stack:
- React + TypeScript
- React Router (navigation)
- Zustand (state management)
- React Query (API calls)
- Tailwind CSS (styling)
- dnd-kit (drag and drop)
```

## Project 2: E-Commerce Store

```
Features:
✅ Product listing with categories
✅ Product detail pages
✅ Shopping cart with quantity management
✅ Search and filter products
✅ Responsive design
✅ Checkout form with validation

Tech Stack:
- React + TypeScript
- React Router
- Zustand (cart state)
- React Query (product data)
- Tailwind CSS
```

## Project 3: Real-Time Chat Application

```
Features:
✅ User registration and login
✅ Create and join chat rooms
✅ Real-time messaging
✅ Online/offline status
✅ Message history
✅ Emoji support

Tech Stack:
- React + TypeScript
- Socket.io (real-time)
- Zustand (state)
- React Router
```

## Project 4: Dashboard with Data Visualization

```
Features:
✅ Interactive charts and graphs
✅ Data tables with sorting/filtering/pagination
✅ KPI cards
✅ Date range filters
✅ Export to CSV
✅ Responsive layout

Tech Stack:
- React + TypeScript
- Recharts or Chart.js
- TanStack Table
- React Query
- Tailwind CSS
```

---

---

# 📋 Quick Reference: Recommended Tools & Libraries

```
Category            | Recommended
--------------------|---------------------------
Build Tool          | Vite
Language            | TypeScript
Routing             | React Router
State Management    | Zustand (simple) / Redux Toolkit (complex)
Server State        | TanStack Query (React Query)
Forms               | React Hook Form + Zod
Styling             | Tailwind CSS
UI Components       | shadcn/ui, Radix UI
Testing             | Vitest + Testing Library
Linting             | ESLint + Prettier
Animation           | Framer Motion
Drag & Drop         | dnd-kit
Date Handling       | date-fns or dayjs
HTTP Client         | fetch (built-in) or Axios
Icons               | Lucide React
Deployment          | Vercel or Netlify
```

---

# 📚 Learning Resources

```
Documentation:
- MDN Web Docs (HTML/CSS/JS)     → developer.mozilla.org
- TypeScript Handbook             → typescriptlang.org/docs
- React Docs                      → react.dev
- Vite Docs                       → vitejs.dev

Practice:
- Frontend Mentor                 → frontendmentor.io (UI challenges)
- LeetCode (JS section)           → leetcode.com
- TypeScript Exercises            → typescript-exercises.github.io
- React Challenges                → reacterry.com

YouTube Channels:
- Fireship (quick concepts)
- Web Dev Simplified
- Traversy Media
- Jack Herrington (advanced React)
- Matt Pocock (TypeScript)

Courses:
- freeCodeCamp.org (free)
- The Odin Project (free)
- Frontend Masters (paid, excellent)
- Josh Comeau's courses (paid, excellent)
```

---

# 🎯 Final Tips for Mastery

```
1. CODE EVERY DAY - Even 30 minutes counts
2. BUILD PROJECTS - Reading tutorials ≠ learning
3. READ ERROR MESSAGES - They tell you exactly what's wrong
4. USE TYPESCRIPT - The initial friction pays off 10x
5. LEARN TO DEBUG - Chrome DevTools + React DevTools
6. READ OTHER PEOPLE'S CODE - GitHub open source projects
7. DON'T MEMORIZE - Understand concepts, reference documentation
8. START SIMPLE - Add complexity gradually
9. WRITE TESTS - Even basic ones save hours of debugging
10. STAY CURRENT - Follow React blog, Twitter/X tech community
```

**You've got this! 🚀 Start with Phase 1 today, build something small every day, and you'll be a confident frontend developer in 6-8 months.**
