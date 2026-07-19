# HTML Interview Questions

## 1. What is semantic HTML, and why is it important?

### Answer

Semantic HTML means using HTML elements based on their actual meaning and purpose.

```html
<header></header>
<nav></nav>
<main></main>
<section></section>
<article></article>
<aside></aside>
<footer></footer>
```

Instead of:

```html
<div class="header"></div>
<div class="navigation"></div>
<div class="main"></div>
```

### Explanation

Semantic HTML improves:

* Accessibility
* SEO
* Code readability
* Screen-reader navigation
* Maintainability

For example:

```html
<button onClick={handleSubmit}>Submit</button>
```

is better than:

```html
<div onClick={handleSubmit}>Submit</div>
```

A `button` automatically supports keyboard interaction, focus behavior, and accessibility semantics.

### Interview point

As a React developer, always prefer semantic elements before adding ARIA attributes.

---

# 2. What is the difference between `id` and `class`?

### Answer

`id` should uniquely identify one element.

```html
<div id="header"></div>
```

A `class` can be shared by multiple elements.

```html
<button class="primary-button">Save</button>
<button class="primary-button">Submit</button>
```

### CSS specificity

```css
#header {
  color: red;
}

.header {
  color: blue;
}
```

`id` has higher specificity than `class`.

Approximate specificity priority:

```text
Inline style
    ↓
ID
    ↓
Class / Attribute / Pseudo-class
    ↓
Element / Pseudo-element
```

In React applications, classes are generally preferred for styling because IDs create unnecessarily high CSS specificity.

---

# 3. What is the difference between `<section>`, `<article>`, and `<div>`?

### `<section>`

Represents a logical section of content.

```html
<section>
  <h2>Products</h2>
</section>
```

### `<article>`

Represents independent, reusable content.

```html
<article>
  <h2>React 19 Features</h2>
  <p>Article content...</p>
</article>
```

Examples:

* Blog post
* News article
* Product card
* Forum post

### `<div>`

Has no semantic meaning. It is mainly used for grouping elements for styling or layout.

```html
<div class="card-wrapper"></div>
```

### Interview rule

Use semantic elements when the content has meaning. Use `div` when you only need a generic container.

---

# 4. What is the purpose of the `DOCTYPE` declaration?

```html
<!DOCTYPE html>
```

### Answer

It tells the browser to render the document using modern HTML standards mode.

Without a valid DOCTYPE, browsers may enter **Quirks Mode**, where older browser rendering behaviors are used.

---

# 5. What is the difference between block and inline elements?

### Block

Takes the full available width by default.

Examples:

```html
<div>
<p>
<section>
<h1>
```

### Inline

Only takes the required content width.

Examples:

```html
<span>
<a>
<strong>
```

Example:

```css
span {
  width: 200px;
}
```

The width generally will not work because `span` is inline.

You could use:

```css
span {
  display: inline-block;
  width: 200px;
}
```

---

# 6. What is the difference between `async` and `defer`?

Consider:

```html
<script src="app.js"></script>
```

Normal script:

```text
HTML Parsing
     ↓
Stop parsing
     ↓
Download JS
     ↓
Execute JS
     ↓
Continue HTML
```

## `async`

```html
<script async src="app.js"></script>
```

The script downloads in parallel.

Once downloaded, HTML parsing pauses and the script executes.

Execution order is **not guaranteed**.

Good for:

* Analytics
* Independent scripts

---

## `defer`

```html
<script defer src="app.js"></script>
```

The script downloads in parallel but executes after HTML parsing finishes.

Execution order is preserved.

```text
HTML parsing ──────────────────→ complete
       JS downloading ────────→
                                Execute JS
```

### Interview answer

For application scripts that depend on DOM or other scripts, `defer` is usually safer than `async`.

---

# 7. What is accessibility in HTML?

Accessibility means making web applications usable by people with disabilities, including users who depend on:

* Screen readers
* Keyboard navigation
* Voice navigation
* High-contrast modes

Example:

Bad:

```html
<div onClick={handleSave}>
  Save
</div>
```

Better:

```html
<button onClick={handleSave}>
  Save
</button>
```

For images:

```html
<img
  src="profile.jpg"
  alt="Anand Kumar profile"
/>
```

For decorative images:

```html
<img
  src="divider.svg"
  alt=""
/>
```

---

# 8. What is ARIA?

ARIA means:

```text
Accessible Rich Internet Applications
```

It provides additional accessibility information.

Example:

```html
<button aria-label="Close dialog">
  ×
</button>
```

Another React example:

```jsx
<button
  aria-expanded={isOpen}
  aria-controls="menu"
>
  Menu
</button>
```

### Important interview point

The first rule of ARIA is:

> Use native HTML elements whenever possible.

Instead of:

```html
<div role="button">Save</div>
```

prefer:

```html
<button>Save</button>
```

---

# 9. What is the difference between `localStorage`, `sessionStorage`, and cookies?

| Feature        | localStorage           | sessionStorage     | Cookies                |
| -------------- | ---------------------- | ------------------ | ---------------------- |
| Persistence    | Until manually cleared | Until tab closes   | Configurable           |
| Sent to server | No                     | No                 | Yes                    |
| Storage        | Around 5–10 MB         | Around 5–10 MB     | Small                  |
| Typical use    | Preferences            | Temporary tab data | Authentication/session |

### Security interview question

Should you store authentication tokens in `localStorage`?

You need to explain the trade-off.

`localStorage` is accessible through JavaScript, so a successful XSS attack may read the token.

For highly sensitive session authentication, secure cookies with:

```text
HttpOnly
Secure
SameSite
```

are often preferred because JavaScript cannot access `HttpOnly` cookies.

---

# CSS Interview Questions

# 10. Explain the CSS box model.

Every HTML element can be represented as:

```text
+-----------------------------+
|           Margin            |
|  +-----------------------+  |
|  |        Border         |  |
|  |  +-----------------+  |  |
|  |  |     Padding     |  |  |
|  |  |  +-----------+  |  |  |
|  |  |  | Content   |  |  |  |
|  |  |  +-----------+  |  |  |
|  |  +-----------------+  |  |
|  +-----------------------+  |
+-----------------------------+
```

Default:

```css
box-sizing: content-box;
```

Suppose:

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid;
}
```

Actual width:

```text
200 + 40 + 10 = 250px
```

With:

```css
box-sizing: border-box;
```

the total width remains:

```text
200px
```

Therefore many applications use:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

---

# 11. Explain CSS specificity.

Consider:

```html
<p id="title" class="heading">
  Hello
</p>
```

```css
p {
  color: black;
}

.heading {
  color: blue;
}

#title {
  color: red;
}
```

Final color:

```text
red
```

Specificity can approximately be understood as:

```text
Inline styles     1000
ID                 100
Class               10
Element              1
```

Example:

```css
.card p {
}
```

Specificity:

```text
0 IDs
1 class
1 element
```

Higher specificity wins.

If specificity is equal, the later rule wins.

---

# 12. What is the difference between `display: none` and `visibility: hidden`?

## `display: none`

```css
.element {
  display: none;
}
```

The element is removed from layout.

Other elements take its space.

---

## `visibility: hidden`

```css
.element {
  visibility: hidden;
}
```

The element is invisible but still occupies space.

---

## `opacity: 0`

```css
.element {
  opacity: 0;
}
```

The element is invisible but:

* Still occupies space
* Can still receive pointer events unless disabled

Example:

```css
.element {
  opacity: 0;
  pointer-events: none;
}
```

---

# 13. Explain `position: relative`, `absolute`, `fixed`, and `sticky`.

## Static

Default position.

```css
position: static;
```

---

## Relative

The element stays in normal document flow.

```css
position: relative;
top: 10px;
```

It is also commonly used as the positioning reference for absolute children.

---

## Absolute

```css
.child {
  position: absolute;
}
```

The element is positioned relative to the nearest ancestor that has a non-static position.

Example:

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 0;
  right: 0;
}
```

---

## Fixed

Relative to the viewport.

```css
.header {
  position: fixed;
  top: 0;
}
```

Common uses:

* Floating buttons
* Fixed headers

---

## Sticky

Acts like relative until a scroll threshold is reached.

```css
.header {
  position: sticky;
  top: 0;
}
```

Common uses:

* Sticky table headers
* Sticky navigation

---

# 14. Why doesn't `z-index` sometimes work?

This is a very common 4-year experience interview question.

Example:

```css
.modal {
  z-index: 9999;
}
```

But another element is still above it.

The reason may be a **stacking context**.

A new stacking context can be created by properties such as:

```css
position + z-index
transform
opacity < 1
filter
isolation
```

Example:

```css
.parent {
  transform: translateX(0);
}
```

This can create a new stacking context.

The child's `z-index` is limited by its parent's stacking context.

### Interview answer

`z-index` is not globally compared between every element. Elements are first compared within their own stacking contexts.

---

# 15. Flexbox vs Grid

## Flexbox

Best for one-dimensional layouts.

```text
Row OR Column
```

Example:

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

Common uses:

* Navigation
* Toolbar
* Button groups
* Form rows

---

## CSS Grid

Best for two-dimensional layouts.

```text
Rows AND Columns
```

Example:

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
```

Common uses:

* Dashboard
* Card layouts
* Page layouts

### Interview answer

I use Flexbox when the primary concern is alignment along one axis. I use Grid when rows and columns need to be controlled together.

---

# 16. Explain `justify-content` vs `align-items`.

For:

```css
.container {
  display: flex;
  flex-direction: row;
}
```

Main axis:

```text
Horizontal →
```

Cross axis:

```text
Vertical ↓
```

Therefore:

```css
justify-content
```

controls horizontal alignment.

```css
align-items
```

controls vertical alignment.

But with:

```css
flex-direction: column;
```

the axes switch.

This is the important interview point.

---

# 17. What is the difference between `flex: 1` and `width: 100%`?

```css
.child {
  flex: 1;
}
```

means approximately:

```css
flex-grow: 1;
flex-shrink: 1;
flex-basis: 0;
```

It tells the element to consume available flexible space.

Whereas:

```css
width: 100%;
```

sets the element's width to 100% of its containing block.

Example:

```css
.container {
  display: flex;
}

.left {
  width: 200px;
}

.right {
  flex: 1;
}
```

The right side automatically fills the remaining space.

---

# 18. What is `flex-shrink: 0`?

Consider:

```css
.container {
  display: flex;
}

.sidebar {
  width: 300px;
}
```

Flex items shrink by default.

Therefore the sidebar may become less than `300px`.

To prevent it:

```css
.sidebar {
  width: 300px;
  flex-shrink: 0;
}
```

This is particularly useful for:

* Sidebars
* Icons
* Fixed-width controls

---

# 19. Explain `px`, `%`, `em`, `rem`, `vh`, and `vw`.

## px

Fixed CSS unit.

```css
width: 200px;
```

---

## %

Relative to the parent or containing block.

```css
width: 50%;
```

---

## em

Usually relative to the current element's inherited/computed font size.

```css
.parent {
  font-size: 20px;
}

.child {
  padding: 1em;
}
```

Padding becomes:

```text
20px
```

---

## rem

Relative to the root HTML font size.

```css
html {
  font-size: 16px;
}

.title {
  font-size: 2rem;
}
```

Result:

```text
32px
```

---

## vh

Viewport height.

```css
height: 100vh;
```

---

## vw

Viewport width.

```css
width: 100vw;
```

---

# 20. What is the difference between `min-width`, `max-width`, and `width`?

Example:

```css
.container {
  width: 100%;
  max-width: 1200px;
}
```

The container can shrink below `1200px`, but will never exceed `1200px`.

Common responsive pattern:

```css
.container {
  width: min(100% - 32px, 1200px);
  margin-inline: auto;
}
```

---

# 21. Explain responsive design.

Responsive design allows an application to adapt to different screen sizes.

Example:

```css
.card-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

@media (max-width: 768px) {
  .card-container {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

A more modern approach:

```css
.card-container {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(250px, 1fr));
}
```

This can automatically adjust the number of columns.

---

# 22. What is `minmax()` in CSS Grid?

```css
grid-template-columns:
  repeat(auto-fit, minmax(250px, 1fr));
```

Meaning:

Each column:

```text
Minimum width = 250px
Maximum width = available flexible space
```

The browser automatically decides how many columns can fit.

Very useful for responsive card layouts.

---

# 23. `auto-fit` vs `auto-fill`

Consider:

```css
repeat(auto-fit, minmax(200px, 1fr))
```

## `auto-fill`

Creates as many grid tracks as possible, including empty tracks.

## `auto-fit`

Collapses empty tracks and allows existing items to expand.

In most responsive card layouts, `auto-fit` often produces the expected behavior.

---

# 24. What are pseudo-classes and pseudo-elements?

## Pseudo-class

Represents an element state.

```css
button:hover {
}

input:focus {
}

li:first-child {
}
```

Syntax:

```text
:
```

---

## Pseudo-element

Represents a virtual part of an element.

```css
.title::before {
  content: "";
}

.title::after {
  content: "";
}
```

Syntax:

```text
::
```

Common examples:

```css
::before
::after
::placeholder
::selection
```

---

# 25. What is the difference between `:nth-child()` and `:nth-of-type()`?

HTML:

```html
<div>
  <h2>Title</h2>
  <p>First</p>
  <p>Second</p>
</div>
```

```css
p:nth-child(2)
```

means:

> Select a `p` which is the second child.

Here the first `<p>` matches because it is child number 2.

```css
p:nth-of-type(2)
```

means:

> Select the second `p` element.

Therefore the second paragraph matches.

---

# 26. How do you center an element horizontally and vertically?

Most common answer:

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

Using Grid:

```css
.container {
  display: grid;
  place-items: center;
}
```

Using absolute positioning:

```css
.element {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

For modern applications, Flexbox or Grid is generally cleaner.

---

# 27. Explain CSS inheritance.

Some properties inherit automatically:

```css
color
font-family
font-size
```

Example:

```css
.parent {
  color: red;
}
```

Children normally inherit the color.

But properties such as:

```css
margin
padding
border
width
height
```

are not inherited.

You can explicitly inherit:

```css
button {
  font-family: inherit;
}
```

---

# 28. Why is `!important` considered bad practice?

```css
.button {
  color: red !important;
}
```

It overrides normal specificity rules and makes CSS harder to maintain.

Using many `!important` rules leads to:

```css
color: red !important;
```

then later:

```css
color: blue !important;
```

which creates specificity battles.

Better solutions:

* Improve selector structure
* Reduce unnecessary specificity
* Fix stylesheet order
* Use proper component/theme architecture

---

# 29. What is CSS reflow and repaint?

This is an important performance question.

## Reflow / Layout

The browser recalculates element positions and dimensions.

Examples:

```css
width
height
margin
padding
top
left
```

Changing these may trigger layout calculations.

---

## Repaint

The browser redraws pixels without recalculating layout.

Examples:

```css
color
background
box-shadow
```

---

## Composite

Properties such as:

```css
transform
opacity
```

can often be handled by the compositor without triggering full layout.

Therefore animations commonly prefer:

```css
transform: translateX(100px);
```

instead of repeatedly changing:

```css
left: 100px;
```

---

# 30. Why is `transform` better than `top/left` for animations?

Instead of:

```css
.box {
  left: 100px;
}
```

prefer:

```css
.box {
  transform: translateX(100px);
}
```

`top` and `left` can cause layout recalculation.

`transform` can often be handled in the compositor layer and therefore usually performs better for animation.

---

# 31. How does CSS work in React applications?

You can explain multiple approaches.

### Plain CSS

```jsx
import "./App.css";
```

### CSS Modules

```jsx
import styles from "./Button.module.css";

<button className={styles.primary}>
  Save
</button>
```

### CSS-in-JS

Used by libraries such as MUI.

```jsx
<Box
  sx={{
    display: "flex",
    gap: 2,
  }}
/>
```

### Styled components

```tsx
const Button = styled("button")({
  padding: "8px 16px",
});
```

For enterprise applications, the important point is maintaining:

```text
Design tokens
      ↓
Theme
      ↓
Reusable components
      ↓
Feature components
```

Instead of repeatedly hardcoding:

```css
color: #1976d2;
font-size: 14px;
padding: 8px;
```

---

# 32. What is CSS `overflow`?

```css
overflow: visible;
overflow: hidden;
overflow: auto;
overflow: scroll;
```

For text:

```css
.text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

Produces:

```text
This is a very long text...
```

For multiple lines:

```css
.text {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

---

# 33. Why does `text-overflow: ellipsis` sometimes not work?

This is a common practical interview question.

This alone is insufficient:

```css
text-overflow: ellipsis;
```

Usually you need:

```css
.text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

And the element must have a constrained width.

Inside Flexbox, you may also need:

```css
.flex-child {
  min-width: 0;
}
```

This is a very good advanced answer.

---

# 34. Why is `min-width: 0` important in Flexbox?

Consider:

```css
.container {
  display: flex;
}

.content {
  flex: 1;
}
```

A child containing long text may overflow because flex items default to:

```css
min-width: auto;
```

Adding:

```css
.content {
  min-width: 0;
}
```

allows the flex item to shrink below its content size.

This is often necessary for:

```css
text-overflow: ellipsis;
```

to work inside flex layouts.

---

# 35. Explain BEM CSS methodology.

BEM means:

```text
Block
Element
Modifier
```

Example:

```html
<div class="card">
  <h2 class="card__title">
    Product
  </h2>

  <button class="card__button card__button--disabled">
    Buy
  </button>
</div>
```

Where:

```text
card                     Block
card__title              Element
card__button--disabled   Modifier
```

It helps prevent naming conflicts and improves CSS maintainability.

---

# 36. Practical interview question: Make a responsive layout

### Requirement

Desktop:

```text
Sidebar | Content
```

Mobile:

```text
Content only
```

### Solution

```css
.layout {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 250px;
  flex-shrink: 0;
}

.content {
  flex: 1;
  min-width: 0;
}

@media (max-width: 768px) {
  .sidebar {
    display: none;
  }
}
```

This question tests:

* Flexbox
* Responsive design
* `flex-shrink`
* `min-width`

---

# 37. Practical question: How would you build a sticky DataGrid toolbar?

```css
.toolbar {
  position: sticky;
  top: 0;
  z-index: 10;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  background: white;
}
```

Important considerations:

```text
Parent overflow
Sticky containing block
z-index
Background
Content wrapping
```

`position: sticky` may not behave as expected depending on ancestor overflow configuration.

---

# 38. What happens when the browser renders a webpage?

A strong 4-year experience answer:

```text
HTML
 ↓
DOM

CSS
 ↓
CSSOM

DOM + CSSOM
 ↓
Render Tree
 ↓
Layout
 ↓
Paint
 ↓
Composite
```

JavaScript may modify the DOM or styles, which can cause additional layout or painting work.

This is why frontend performance optimization involves reducing unnecessary:

```text
DOM updates
Layout calculations
Paint operations
```

---

# 39. What is critical CSS?

Critical CSS is the CSS required to render the initially visible portion of the page.

The idea is:

```text
Load essential CSS first
      ↓
Render visible content
      ↓
Load remaining styles
```

It can help improve perceived page-loading performance.

---

# 40. Interview scenario: You have 1,000 elements. Should you add an event listener to each?

Instead of:

```javascript
items.forEach((item) => {
  item.addEventListener("click", handler);
});
```

you can use **event delegation**:

```javascript
container.addEventListener("click", (event) => {
  const target = event.target.closest(".item");

  if (!target) return;

  console.log(target.dataset.id);
});
```

This works because browser events bubble.

In React, the framework's event system handles much of this abstraction, but understanding native event bubbling is still important.

---

# Most Important Topics for a 4-Year React Developer

For interviews, I would prioritize these in this order:

```text
1. CSS Box Model
2. Flexbox
3. CSS Grid
4. CSS Specificity
5. position + z-index
6. Responsive Design
7. Semantic HTML
8. Accessibility
9. Reflow, Repaint and Rendering
10. Flexbox overflow and min-width: 0
11. CSS units
12. Browser rendering process
13. async vs defer
14. Event bubbling
15. CSS architecture / Design systems
```

### A good interview answer pattern

For most HTML/CSS questions, answer using:

```text
Definition
   ↓
How it works
   ↓
Simple example
   ↓
Real project use case
   ↓
Potential issue / edge case
```

For example, instead of saying:

> "`position: sticky` makes an element sticky."

A 4-year developer should say:

> "`position: sticky` behaves like relative positioning until the scroll container reaches the configured threshold such as `top: 0`. It then remains fixed relative to its scrolling container. I commonly use it for table headers and toolbars, but ancestor overflow settings can affect its behavior."

That difference makes your answer sound like an **experienced developer rather than someone who has memorized definitions**.
