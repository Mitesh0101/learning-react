# My React.js Learning Journey: Day 1

## 💡 Core Concepts
- **Execution Flow:** The entry point of the application is `index.html`, which contains a single `<div id="root"></div>`. This is the mount point where the entire React application is injected.
- **The Mounting Process:** `index.html` loads `main.jsx` via a script tag. `main.jsx` uses `ReactDOM` to render the `App.jsx` component into the root div.
- **JSX Nature:** I learned that components return JSX (JavaScript XML). While it looks like HTML, it is a syntax extension for JavaScript. During the build process, Vite/Babel transforms JSX into $React.createElement$ calls, which represent the UI as JavaScript objects.
- **The Single Parent Rule:** A component must return a single root element. If I need to return multiple elements without adding extra nodes to the DOM (like an unnecessary `div`), I must wrap them in a **Fragment** (`<> ... </>`).
- **Component Capitalization:** I found that React requires component names to start with a capital letter. This is a strict requirement for the transpiler to distinguish between custom React components and standard HTML tags.

## 🛠️ Implementation
I practiced modularizing my code by creating separate `.jsx` files and importing them into `App.jsx`.

```javascript
import Header from "./Header"

function App() {
    return (
        <>
            <Header />
            <h1>Hello World</h1>
        </>
    )
}

function Header() {
    return <h1>This is the Header</h1>
}

export default App
```

## 📂 Workflow & Tools
- **Project Configuration:** - `package.json` manages dependencies, devDependencies, and scripts (like `npm run dev`).
    - `vite.config.js` is used for server-level configurations like port and hostname.
- **Directory Structure:**
    - `node_modules/`: Contains all external code; must never be edited manually or tracked in Git.
    - `src/`: The primary directory for all source code and logic.
    - `public/` vs `src/assets/`: Files in `public/` are served at the root path. Files in `src/assets/` are part of the build pipeline and should be imported directly into components for optimization and hashing.
- **Git Hygiene:** I use `.gitignore` to ensure heavy folders like `node_modules` and local environment files are not tracked.

## 🔍 Key Corrections
- **JSX vs HTML:** I initially thought of it as returning HTML, but it's actually returning React Elements (JavaScript objects) that describe what the UI should look like.
- **Asset Access:** I learned that `src/assets` is not "private" in the security sense, but rather "internal" to the build system, meaning assets there need to be imported to be included in the final bundle.

# My React.js Learning Journey: Day 2

## 💡 Core Concepts
- **ES6 Modules (Import/Export):** I learned that components are shared between files using two types of exports. 
    - **Default Export:** Used for the main piece of code in a file. I can only have one per file, and I don't use curly braces when importing it.
    - **Named Export:** Used for exporting multiple functions or variables from one file. These must be wrapped in curly braces `{}` during import to specify exactly which members I am accessing.
- **JSX (JavaScript XML):** This is a syntax extension for JavaScript that allows me to write HTML-like structures directly in my code. It is eventually transformed into JavaScript. Unlike standard HTML, all tags in JSX must be explicitly closed (e.g., `<br />` instead of `<br>`).
- **React.createElement:** I found that JSX is actually syntactic sugar for `React.createElement(type, props, ...children)`. While writing raw `createElement` is possible, it leads to messy, nested code that is difficult to debug compared to the clean look of JSX.
- **Event Handling:** In React, I pass function definitions (the reference) to event handlers rather than calling the function immediately. Event attributes use **camelCase** (e.g., `onClick`) instead of the lowercase used in standard HTML.
- **State Management:** I learned that standard variables do not trigger a UI update when they change. I must use the `useState` hook to manage "State." When a state variable is updated via its setter function, React automatically re-renders the component to show the new data on the frontend.
- **Conditional Rendering:** I use ternary operators (`? :`) to dynamically show or hide components and HTML tags based on state. For multiple conditions, I can chain these operators together.
- **Props (Properties):** These act as parameters for components, allowing me to pass data (strings, numbers, arrays, objects) from a parent to a child. Props are read-only and help make components reusable.
- **Component Composition (children):** By using the `children` prop, I can pass JSX or other components inside the opening and closing tags of a custom component. This is essential for creating "Wrapper" components.
- **Controlled Components:** This refers to form elements whose values are tied directly to React state. By linking the `value` attribute to a state variable and updating that state via `onChange`, React becomes the "single source of truth" for the form data.



## 🛠️ Implementation

### Importing and Exporting
I practiced using both default and named exports in a single file to organize my components and constants.

**UserComponents.jsx**
```javascript
function UserComponent() {
    return(
        <h1>This is User Component</h1>
    )
}

export default UserComponent;

export function Profile() {
    return(
        <h1>This is Profile Component</h1>
    )
}

export function Setting() {
    return(
        <h1>This is Setting Component</h1>
    )
}

export const UserKey = "UserKey";
```

**App.jsx**
```javascript
import UserComponent, {Profile, Setting, UserKey} from "./UserComponents";

function App() {
  return(
    <div>
      <h1>Hello World</h1>
      <UserComponent />
      <Profile />
      <Setting />
      <h1>{UserKey}</h1>
    </div>
  )
}

export default App
```

### JSX and Expressions
I used curly braces to embed JavaScript logic, such as variables, function calls, and ternary operators, directly into the JSX.

```javascript
function App() {
  const username = "Agent47";
  let x=20, y=30;
  const imgPath = "https://picsum.photos/500/500";

  function mul(a, b) {
    return a*b;
  }

  return(
    <div>
      <h1>Hello World</h1>
      <h1>Username : {username ? username : "Invalid username"}</h1>
      <h1>{x} * {y} = {mul(x,y)}</h1>
      <button onClick={() => window.alert("Button Clicked")}>Click Me</button>
      <br />
      <img src={imgPath} alt="img" />
    </div>
  )
}
```

### Raw React.createElement (Non-JSX)
I compared the JSX approach to the manual `createElement` method to understand what happens under the hood.

```javascript
import { createElement } from "react";

function App() {
  const username = "Agent47";
  return createElement("div", {id:"rootDiv"}, createElement("h1", {class:"heading"}, username));
}
```

### Event Handling with Parameters
I learned that to pass parameters to an event handler, I must wrap the call in an arrow function to prevent it from executing immediately on render.

```javascript
function App() {
  function hello() {
    alert("Hello World");
  }

  function fruit(name) {
    alert(name);
  }

  return(
    <div>
      <button onClick={hello}>Click Me</button>
      <br />
      <button onClick={()=>{fruit("Apple")}}>Apple</button>
      <br />
      <button onClick={()=>{fruit("Banana")}}>Banana</button>
    </div>
  )
}
```

### State and Counter
I implemented a simple counter to see how `useState` triggers re-renders.

```javascript
import { useState } from "react";

function App() {
  let [count, setCount] = useState(0);
  return(
    <>
      <h1>Counter: {count}</h1>
      <button onClick={()=>setCount(count+1)}>Update Count</button>
    </>
  )
}
```

### Show/Hide and Conditionals
I practiced toggling visibility for both standard tags and custom components using state and ternary logic.

```javascript
import { useState } from "react";
import ToDo from "./ToDo";

function App() {
  let [display, setDisplay] = useState(true);
  let [toDo, setToDo] = useState(true);
  const username = "Larry";

  return (
    <>
      <button onClick={()=>setDisplay(!display)}>Toggle Display</button>
      {display ? <h1>{username}</h1> : null}
      <button onClick={()=>setToDo(!toDo)}>Toggle ToDo</button>
      {toDo ? <ToDo /> : null}
    </>
  )
}
```

### Passing Props (Data and Children)
I practiced passing complex data (objects and arrays) and styling components dynamically via props.

**Student.jsx**
```javascript
function Student(props) {
    let {name="Default Name", age, email, marks, college} = props;
    return(
        <>
            <h1>Student Name : {name}</h1>
            <h1>Student Age : {age}</h1>
            <h1>Student Email : {email}</h1>
            <h1>Student Marks : {marks.toString()}</h1>
            <h1>Student College Name : {college.name}</h1>
            <h1>Student College Location : {college.location}</h1>
        </>
    )
}
```

**Wrapper.jsx**
```javascript
function Wrapper({children, color="red"}) {
    return(
        <div>
            <div style={{color:color, border:"5px solid black", textAlign:"center"}}>
                <h1>Hello World</h1>
            </div>
            {children}
        </div>
    )
}
```

### Controlled Components and Inputs
I built a form where the state is updated on every keystroke, ensuring the UI and the data stay in sync.



```javascript
import { useState } from "react";

function App() {
    let [username, setUsername] = useState("");
    let [password, setPassword] = useState("");

    return(
        <form>
            <input type="text" value={username} onChange={(e) => setUsername(e.target.value)} />
            <input type="password" value={password} onChange={(e) => setPassword(e.target.value)} />
            <h1>Username: {username}</h1>
            <h1>Password: {password}</h1>
        </form>
    )
}
```

## 📂 Workflow & Tools
- **CamelCase naming:** I observed that while HTML uses `onclick`, React requires `onClick`. This applies to almost all event handlers and inline style properties (e.g., `textAlign`).
- **Styling with Objects:** When passing styles through props or directly to the `style` attribute, I must use an object where keys are camelCased strings and values are the CSS values.
- **Input Handling:** I use `event.target.value` inside `onChange` to capture user input.

## 🔍 Key Corrections
- **Function Invocation vs Reference:** I initially thought I could use `onClick={hello()}`, but I learned this calls the function as soon as the component renders. I must use `onClick={hello}` to pass the reference.
- **Reserved Keywords:** I found that I cannot use `class` for CSS classes in JSX; I must use `className` because `class` is a reserved keyword in JavaScript.
- **React State Immutability:** I learned that I should never modify the state variable directly (e.g., `count++`); I must always use the setter function provided by `useState` to ensure React notices the change and re-renders.

# My React.js Learning Journey: Day 3

## 💡 Core Concepts
- **Advanced Form Handling:** - **Checkboxes:** Handling multiple selections requires an array state. I use the spread operator (`...`) to create a new array with the new value when checked, and the `.filter()` method to remove the value when unchecked.
    - **Radio Buttons:** To make radio buttons "controlled," I link the `checked` attribute to a state comparison (e.g., `checked={gender === "Male"}`).
    - **Dropdowns:** I use the `<select>` tag with a `defaultValue` or `value` attribute synced to state to track the user's choice.
- **Lists and the Map Function:** I use the `.map()` method to iterate through arrays and render JSX dynamically. 
    - **Syntax:** I use parentheses `()` for an implicit return of the JSX. 
    - **Keys:** Every element generated by a loop must have a unique `key` prop. This allows React to efficiently track which items change, significantly improving performance.
- **Hooks Overview:** Hooks are built-in functions that allow functional components to "hook" into React features like state and lifecycle methods. They always start with the `use` prefix (e.g., `useState`, `useEffect`).
- **Side Effects and `useEffect`:** Logic that interacts with the "outside world" (API calls, timers, manual DOM changes) is a side effect. 
    - Without `useEffect`, code in the component body runs on every re-render.
    - The **Dependency Array** controls when the effect runs:
        - `[]`: Runs only once (Mounting).
        - `[stateName]`: Runs only when that specific state/prop changes.
        - No array: Runs on every render.
- **The Component Lifecycle:** Every component follows three phases:
    1. **Mounting:** The component is created and inserted into the DOM.
    2. **Updating:** The component re-renders due to changes in props or state.
    3. **Unmounting:** The component is removed from the DOM. I use the **cleanup function** (a function returned inside `useEffect`) to stop timers or subscriptions during this phase.



## 🛠️ Implementation

### Handling Selection Inputs
I practiced managing arrays for checkboxes and simple strings for radio buttons/dropdowns.

```javascript
// Checkbox logic with Spread and Filter
const [skills, setSkills] = useState([]);

const handleSkill = (event) => {
  if (event.target.checked) {
    setSkills([...skills, event.target.value]); // Immutably add
  } else {
    setSkills(skills.filter(skill => skill != event.target.value)); // Immutably remove
  }
}

// Radio and Dropdown implementation
const [gender, setGender] = useState("Male");
const [city, setCity] = useState("Surat");

// JSX for Radio
<input onChange={(e) => setGender(e.target.value)} checked={gender === "Male"} type="radio" name="gender" value="Male" />

// JSX for Dropdown
<select defaultValue={city} onChange={(e) => setCity(e.target.value)}>
  <option value="Ahmedabad">Ahmedabad</option>
  <option value="Surat">Surat</option>
</select>
```

### Rendering Data Lists
I used `.map()` to render a dynamic table and passed data into a child `User` component.



```javascript
// Data Array
const userData = [
  {id:1, name:"Anil", email:"anil@test.com", age:29},
  {id:2, name:"Sam", email:"sam@test.com", age:34}
];

// Rendering Table Rows
{userData.map(user => (
  <tr key={user.id}>
    <td>{user.id}</td>
    <td>{user.name}</td>
    <td>{user.email}</td>
    <td>{user.age}</td>
  </tr>
))}

// Rendering Components in a Loop
{userData.map(user => (
  <div key={user.id}>
    <User data={user} />
  </div>
))}
```

### Lifecycle & useEffect Hook
I implemented a `Counter` to log phase transitions and a clock exercise to test state updates.

```javascript
// Counter.jsx - Tracking Lifecycle
useEffect(() => {
    console.log("Mounting Phase"); // Runs once
    return () => {
        console.log("Unmounting Phase"); // Cleanup: Runs on removal
    }
}, []);

useEffect(() => {
    console.log("Updating Phase"); // Runs when count or data changes
}, [count, data]);

// Clock Exercise Logic
setInterval(() => {
  setTime(new Date().toLocaleTimeString());
}, 1000);
```

### Nested Loops (Logical Concept)
If I have a complex JSON (e.g., a College containing an array of Students), I nest one `.map()` inside another:
```javascript
colleges.map(college => (
  <div key={college.id}>
    <h2>{college.name}</h2>
    {college.students.map(student => (
      <p key={student.id}>{student.name}</p>
    ))}
  </div>
))
```

## 📂 Workflow & Tools
- **Labeling:** I use `htmlFor` instead of the standard HTML `for` attribute to link labels to input IDs.
- **Style Objects:** I define reusable style objects and pass them to the `style` prop using camelCase keys.
```javascript
const divStyle = {
  borderRadius: "2rem",
  textAlign: "center",
  display: "inline-flex"
}
<div style={divStyle}>...</div>
```

## 🔍 Key Corrections
- **Memory Leak Alert (Clock Exercise):** I initially put `setInterval` in the component body. I learned this is dangerous because every re-render creates a *new* interval, leading to thousands of timers running simultaneously. I must wrap it in `useEffect` and use `clearInterval` in the cleanup function.
- **Event Selection:** While I used `onClick` for form elements, I learned that `onChange` is the industry standard for checkboxes, radios, and selects as it more accurately captures the state change.
- **JSX Mapping:** I found that using curly braces `{}` in a map without a `return` statement results in an empty UI. Parentheses `()` are the shorthand for returning JSX directly.
- **Key Location:** The `key` must be placed on the **outermost** element returned within the `.map()` loop (e.g., the `<tr>` or the `<div>` wrapping the component), not inside the child component itself.

# My React.js Learning Journey: Day 4

## 💡 Core Concepts
- **Dynamic Styling with State:** I learned that I can store entire style objects in state. By using the spread operator (`...`), I can update specific properties (like `backgroundColor`) while preserving the rest of the style configuration. This is powerful for theme switching or responding to user interactions.
- **External CSS & Global Scope:** Traditional `.css` files imported into a component are not scoped to that component; they become global once loaded. I found that the best practice is to import general styles in `main.jsx` and component-specific styles within the respective component file, though I must be wary of class name collisions.
- **CSS Modules:** To solve the global collision problem, I use CSS Modules (`filename.module.css`). React treats these as objects where each class name is mapped to a unique, hashed string. This ensures that styles are strictly scoped to the component where they are imported.
- **Styled Components (CSS-in-JS):** This is an external library that allows me to create components that have styles attached to them using tagged template literals. It enables me to keep my styling logic and component logic in the same file without the messiness of standard inline objects.
- **React-Bootstrap:** Instead of just using standard Bootstrap classes, `react-bootstrap` provides pre-built components (like `<Button />`). This feels more "React-like" because I interact with props (e.g., `variant="danger"`) rather than raw string classes.
- **The `useRef` Hook:** This hook allows me to create a "reference" to a DOM element. Unlike state, updating a ref does not trigger a re-render. I use it when I need to perform imperative actions—like focusing an input, manually changing a value, or directly manipulating a tag's style—without going through the standard React declarative flow.



## 🛠️ Implementation

### Dynamic Inline Themes
I practiced updating a theme by passing new colors through a function that updates a state-based style object.

```javascript
const [divstyle, setDivstyle] = useState({
  border: "2px solid black",
  borderRadius: "2rem",
  textAlign: "center"
});

function changeTheme(bgColor, color) {
  setDivstyle({ ...divstyle, backgroundColor: bgColor }); // Preserving other properties
}

// In JSX
<div style={divstyle}>...</div>
```

### CSS Modules vs. Global CSS
I compared the two by observing how global CSS uses strings and Modules use object notation.

```javascript
// Global CSS
import "./style.css";
<div className="container">...</div>

// CSS Modules (Scoped)
import style from "./style.module.css";
<div className={style.container}>...</div>
```

### Styled Components Syntax
I implemented styled components using the tagged template literal syntax, which allows for full CSS power (hover states, media queries) inside JavaScript.

```javascript
import styled from "styled-components";

const Heading = styled.h1`
  color: red;
  background-color: black;
  padding: 2rem;
  text-align: center;
`;

// Usage
<Heading>Hello World</Heading>
```

### DOM Manipulation with `useRef`
I used `useRef` to directly access an input element to trigger focus and toggle visibility.



```javascript
const inputRef = useRef(null);

const refHandler = () => {
  inputRef.current.focus(); // Accessing the DOM node directly
  inputRef.current.style.color = "green";
};

// Assigning the ref
<input type="text" ref={inputRef} />
```

## 📂 Workflow & Tools
- **Dependencies:** I learned to install styling libraries via npm:
  - `npm i styled-components`
  - `npm i react-bootstrap bootstrap`
- **Global Bootstrap Setup:** To use Bootstrap project-wide, I must import the minified CSS in my entry point file (`main.jsx`):
  ```javascript
  import 'bootstrap/dist/css/bootstrap.min.css';
  ```
- **Naming Conventions:** CSS Modules require the `.module.css` suffix to be recognized by the build tool (Vite).

## 🔍 Key Corrections
- **The `className` Requirement:** I have to remember that `class` is a reserved keyword in JavaScript. In JSX, I must always use `className` for CSS classes.
- **`useRef` and `.current`:** I learned that a ref is an object with a single property called `.current`. To access the actual DOM element, I must always use `inputRef.current`, not just `inputRef`.
- **Ref Overuse:** I realized that `useRef` should be used sparingly. Most UI changes should be handled via state; refs are an "escape hatch" for when I need direct access to the DOM node for things like focus or third-party library integration.
- **Object-based Styled Components:** I noted that `styled-components` can also accept an object syntax, but the template literal approach is more common as it supports standard CSS syntax.

# My React.js Learning Journey: Day 5

## 💡 Core Concepts
- **Uncontrolled Components:** I learned that these are form elements where the DOM itself handles the form data, rather than React state. Instead of writing an event handler for every state update, I pull the value from the DOM when I need it (usually on submission). 
- **Imperative DOM Access:** While I can use `querySelector`, I found that `useRef` is the standard "React way" to access DOM nodes in an uncontrolled fashion. It keeps the reference within the React ecosystem without bypassing the virtual DOM entirely.
- **Function Props (Lifting State/Logic):** I practiced passing functions as props to child components. This is essential for centralized logic; if a function is used by multiple children, defining it in the parent ensures consistency and easier maintenance.
- **Modern `ref` Passing (React 19+):** I discovered that starting with React 19, `ref` is treated as a regular prop. I no longer need to wrap components in the `forwardRef` high-order component. I can simply pass `ref` to a child and access it through `props.ref`.
- **Form Actions & Status:** I explored `useFormStatus` from `react-dom`. It allows me to track the status of a form submission (specifically the `pending` state) without manually managing a "loading" state.
- **Transitioning Updates:** I learned about `useTransition`, which helps me handle non-urgent UI updates. It provides a `pending` boolean and a `startTransition` function to wrap long-running tasks, preventing the UI from freezing during heavy processing.
- **The Philosophy of Pure Components:**
    - **Purity Defined:** A function is pure if it returns the same output for the same input and produces no side effects. In React, a Pure Component is one that renders the exact same JSX given the same props.
    - **Side Effects to Avoid:** A component is **impure** if it changes a variable that existed before the render, or if it modifies an object/array passed as a prop.
    - **React.memo:** This is how I can implement pure component optimization. By wrapping a component in `React.memo`, React will skip re-rendering that component if its props haven't changed.
    - **Strict Mode:** I noticed that React's Strict Mode renders components twice in development. This is a deliberate "stress test" to help me catch impurities (like a component changing a global variable during render).



## 🛠️ Implementation

### Uncontrolled Forms (The React Way)
I implemented an uncontrolled form using `useRef` to avoid the overhead of re-rendering on every keystroke. I also used `event.preventDefault()` to stop the browser's default reload behavior.

```javascript
import { useRef } from "react";

function App() {
  const nameRef = useRef(null);
  const passwordRef = useRef(null);

  const handleForm = event => {
    event.preventDefault(); 
    // Accessing values directly from the DOM nodes
    const name = nameRef.current.value;
    const password = passwordRef.current.value;
    console.log(`Submitted: ${name}`);
  }

  return (
    <form onSubmit={handleForm}>
      <input type="text" ref={nameRef} placeholder="Name" />
      <input type="password" ref={passwordRef} placeholder="Password" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Passing Functions to Multiple Children
I centralized an alert function in the parent and passed it down to multiple `User` components.

```javascript
// App.jsx
function App() {
  const displayFunc = (name) => alert(name);
    
  return (
    <>
      <User displayFunc={displayFunc} name="Anil" />
      <User displayFunc={displayFunc} name="Sam" />
    </>
  );
}

// User.jsx
function User({ displayFunc, name }) {
    return <button onClick={() => displayFunc(name)}>Display {name}</button>;
}
```

### Async Form Status (useFormStatus)
I practiced using the `useFormStatus` hook to automatically disable my submit button during an asynchronous action.

```javascript
import { useFormStatus } from "react-dom";

function CustomerForm() {
  const { pending } = useFormStatus(); // Automatically tracks form parent status

  return (
    <button disabled={pending}>
      {pending ? "Submitting..." : "Submit"}
    </button>
  );
}

function App() {
  async function handleSubmit() {
    await new Promise(res => setTimeout(res, 2000));
    console.log("Form Action Complete");
  }

  return (
    <form action={handleSubmit}>
      <input type="text" />
      <CustomerForm />
    </form>
  );
}
```

### Pure Components & Immutability
I learned that to keep a component pure, I must treat props and state as read-only.



```javascript
// PURE: Only uses its own props to calculate output
function Receipt({ items, taxRate }) {
    const total = items.reduce((sum, item) => sum + item.price, 0);
    return <h1>Total: {total * taxRate}</h1>;
}

// IMPURE: Modifies an external variable (AVOID THIS)
let guestCount = 0;
function Cup() {
    guestCount = guestCount + 1; // SIDE EFFECT!
    return <h2>Tea cup for guest #{guestCount}</h2>;
}
```

## 📂 Workflow & Tools
- **Async Simulation:** I use `new Promise(res => setTimeout(res, 2000))` to simulate network latency when testing loading states and transitions.
- **Form Actions:** I started using the `action` attribute on `<form>` instead of `onSubmit` when working with React 19's new form features.
- **React-DOM Hooks:** I noted that `useFormStatus` must be used in a component that is **nested inside** a `<form>`, not the component that contains the `<form>` tag itself.

## 🔍 Key Corrections
- **Manual DOM vs. Refs:** While I practiced `querySelector`, I learned it is technically an "anti-pattern" in React because it searches the whole document. `useRef` is safer because it is scoped to the specific component instance.
- **Transition vs. Form Status:** I realized `useTransition` is for general UI updates (like filtering a long list), while `useFormStatus` is specialized for `react-dom` form actions.
- **React 19 `ref` Prop:** I corrected my understanding of `forwardRef`. In older codebases, I'll see `const MyInput = forwardRef((props, ref) => ...)`, but in my new React 19 projects, I can treat `ref` as just another property in the `props` object.
- **Impurities in Render:** I learned that I should never perform data fetching or timer setup directly in the component's render body; these belong in `useEffect` to maintain component purity during the "Rendering" phase.

# My React.js Learning Journey: Day 6

## 💡 Core Concepts

- **Derived State:** I learned that I don’t need to create a new state for every piece of data. If a value can be calculated from existing props or state, I should calculate it as a normal variable during render. This ensures a **Single Source of Truth (SSOT)** and prevents "syncing" bugs where two states get out of alignment.
- **Lifting State Up:** React follows a unidirectional data flow. If two sibling components need to share the same data, I must move (lift) the state to their closest common parent. The parent then passes the state down via props and provides functions to update that state.
- **Immutability & Reference Integrity:** React determines if it should re-render based on **shallow comparison**. 
    - For **Objects**: If I just change a property (e.g., `data.name = 'New'`), the memory reference remains the same, and React may not re-render. I must use the spread operator (`...`) to create a completely new object reference.
    - **Nested Objects**: I found that spreading only the top level isn't enough for nested data. I must spread every level of the hierarchy that is being updated to ensure a fresh reference.
- **useActionState (React 19):** I explored this new hook designed for Form Actions. It takes an action function and an initial state. It returns the current state of the action, the action function itself to be used in the `<form action={...}>`, and a `pending` boolean to track execution status.
- **The useId Hook:** I found a way to generate unique, stable IDs for accessibility attributes (like `htmlFor` and `id`). This is crucial for linking labels to inputs without manually managing unique strings.



---

## 🛠️ Implementation

### Derived State Calculation
I practiced calculating "Total," "Last User," and "Unique" counts directly from the `users` state array without creating three extra state variables.

```javascript
const [users, setUsers] = useState([]);
const [user, setUser] = useState("");

// Derived States: These recalculate automatically whenever 'users' changes
const totalCount = users.length;
const lastAdded = users[users.length - 1];
const uniqueCount = [...new Set(users)].length;
```

### Immutable Object & Array Updates
I practiced "Deep Spreading" to update a city inside a nested address object and updating specific elements in an array.



```javascript
// Updating a nested object
const handleCity = (event) => {
  setData({
    ...data, 
    address: { 
      ...data.address, 
      city: event.target.value 
    }
  });
};

// Updating an array of objects
const handleAge = (event) => {
  setDataDetails([
    ...dataDetails.slice(0, -1), 
    { ...dataDetails[dataDetails.length - 1], age: event.target.value }
  ]);
};
```

### Form Management with `useActionState`
I implemented a form using the React 19 `useActionState` hook to handle validation and submission status natively.



```javascript
const handleForm = (previousData, formData) => {
  const name = formData.get("name");
  const password = formData.get("password");
  
  if (name && password) {
    return { message: "Success", name, password };
  }
  return { error: "Invalid Data" };
};

const [data, action, pending] = useActionState(handleForm, undefined);

return (
  <form action={action}>
    <input name="name" type="text" />
    <input name="password" type="password" />
    <button disabled={pending}>{pending ? "Submitting..." : "Submit"}</button>
    {data?.error && <span style={{color: "red"}}>{data.error}</span>}
  </form>
);
```

---

## 📂 Workflow & Tools

- **Optional Chaining (`?.`):** I started using `data?.error` to safely access properties. If `data` is `undefined` (which it is at the start), the app won't crash; it just returns `undefined`.
- **Set Object:** I used `new Set(array)` as a quick way to filter for unique values when calculating derived state.
- **useId Formatting:** I learned that I can use a single `useId` and append strings (e.g., `id={id + '-name'}`) to handle multiple related fields in a single component.

---

## 🔍 Key Corrections

- **Derived State vs. Performance:** I initially thought derived state was "faster" because it uses normal variables. I learned the real benefit is **reliability**. It prevents "stale data" bugs where one state update is missed, causing the UI to show conflicting information.
- **The Spread Operator Limitation:** I realized that `...data` only performs a **shallow copy**. If I update `data.address.city` without spreading the `address` object as well, the reference to `address` stays the same, which can lead to React skipping necessary re-renders of components that depend on the address.
- **useId for Keys:** I learned that `useId` should **never** be used to generate keys for lists. Keys should come from the data itself (like a database ID) to help React track items across re-renders. `useId` is strictly for DOM attributes.
- **useActionState Parameter Order:** I noted that the action function for `useActionState` receives `previousData` as the first argument and `formData` as the second, which is a specific requirement of the hook.

# My React.js Learning Journey: Day 7

## 💡 Core Concepts
- **React Fragments:** I learned that Fragments (`<Fragment>` or `<>`) allow me to group multiple sibling elements without adding extra, unnecessary nodes (like wrapper `<div>` tags) to the actual DOM. This keeps the HTML structure clean and prevents CSS layout issues.
- **Custom Hooks:** I can extract component logic into reusable functions called Custom Hooks. Their names must always start with `use` (e.g., `useToggle`). This allows me to share stateful logic—not state itself—across multiple components.
- **Context API:** I discovered that the Context API is the native solution to "prop drilling" (passing props through multiple intermediate components that don't actually need the data). It consists of three main parts:
    - **`createContext`**: Initializes the context object (usually in a separate file to prevent circular dependencies).
    - **`Provider`**: Wraps the parent component and sends the data via the `value` prop.
    - **`useContext`**: A hook used in the deeply nested child component to directly receive the data.
- **React Router:** This library enables client-side routing, turning a React app into a Single Page Application (SPA). 
    - **`BrowserRouter`**: Uses the HTML5 History API to keep the UI in sync with the URL.
    - **`Link`**: Replaces standard `<a>` tags. It allows navigation between pages *without* triggering a full browser reload, making the app much faster since only the necessary components update.
    - **`Routes` & `Route`**: Used to map specific URL paths to their corresponding React components.

## 🛠️ Implementation

### Using Fragments
I practiced replacing unnecessary wrapper `div` elements with Fragments to keep the DOM tree lightweight.

```javascript
import { Fragment } from "react";

function App() {
  return(
    <Fragment>
      <h1>Hello</h1>
    </Fragment>
    
    // Shorthand syntax is also valid:
    // <>
    //   <h1>Hello</h1>
    // </>
  )
}
```

### Building a Custom Hook
I built a `useToggle` hook that manages a boolean state. I implemented logic to either toggle the value automatically or force it to a specific boolean if an argument is passed.

```javascript
// useToggle.jsx
import { useState } from "react";

function useToggle(defaultVal) {
    const [value, setValue] = useState(defaultVal);

    function toggleValue(val) {
        if (typeof val !== 'boolean') {
            setValue(!value); // Standard toggle if triggered by an event object
        } else {
            setValue(val); // Forced state if boolean is passed
        }
    }

    return [value, toggleValue];
}

// App.jsx
import useToggle from "./useToggle";

function App() {
  const [value, toggleValue] = useToggle(true);

  return(
    <>
      <button onClick={toggleValue}>Toggle Display</button>
      <button onClick={() => toggleValue(false)}>Hide Display</button>
      <button onClick={() => toggleValue(true)}>Show Display</button>
      {value ? <h1>Display</h1> : null}
    </>
  )
}
```

### Context API (Avoiding Prop Drilling)
I created a deep component tree (`App` → `College` → `ClassComponent` → `Student` → `Subject`) and used Context to pass a `subject` state directly from `App` to `Subject` without passing it as a prop through the middle components.

```javascript
// contextApp.js
import { createContext } from "react";
export const SubjectContext = createContext();

// App.jsx (The Provider)
import { useState } from "react";
import { SubjectContext } from "./contextApp";
import College from "./College";

function App() {
  const [subject, setSubject] = useState("");

  return(
    <div style={{backgroundColor:"red"}}>
      {/* We can now use Context without writing .Provider in React 19
          <SubjectContext value={subject}> ... </SubjectContext> */}
      <SubjectContext.Provider value={subject}>
        <select value={subject} onChange={e => setSubject(e.target.value)}>
          <option value="">Select Your Subject</option>
          <option value="Maths">Maths</option>
        </select>
        <College /> {/* College -> ClassComponent -> Student -> Subject */}
      </SubjectContext.Provider>
    </div>
  )
}

// Subject.jsx (The Consumer)
import { useContext } from "react";
import { SubjectContext } from "./contextApp";

function Subject() {
    const subject = useContext(SubjectContext); // Accessing data directly

    return(
        <div style={{backgroundColor: "violet"}}>
            <h1>Subject = {subject}</h1>
        </div>
    )
}
```

### React Router Setup
I implemented basic client-side routing, linking different paths to specific components.

```javascript
import { BrowserRouter, Routes, Route, Link } from "react-router";
import User from "./User";

function App() {
  return(
    <div>
      <BrowserRouter>
        {/* Navigation links that do not reload the page */}
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/user">User</Link>

        {/* Route definitions */}
        <Routes>
          <Route path="/" element={<h1>Home Page</h1>} />
          <Route path="/about" element={<h1>About Page</h1>} />
          <Route path="/user" element={<User />} />
        </Routes>
      </BrowserRouter>
    </div>
  )
}
```

## 📂 Workflow & Tools
- **Context Separation:** I learned it is a best practice to create the context (e.g., `export const SubjectContext = createContext();`) in a completely separate file to avoid import/export conflicts between the Provider component and the Consumer components.
- **Router Placement:** While I wrapped `App`'s content in `<BrowserRouter>` for this test, I noted that `BrowserRouter` is usually placed at the very top level of the application, often wrapping the `<App />` component directly inside `main.jsx`.
- **Custom Hook Return Types:** Just like `useState`, my custom `useToggle` hook returns an array `[value, toggleValue]`. This allows the consuming component to destructure and rename the variables easily.

## 🔍 Key Corrections
- **Custom Hook Event Overlap:** In my `useToggle` hook, I initially had to account for how React handles events. If a button simply calls `onClick={toggleValue}`, the `val` parameter becomes the React Synthetic Event object (which is not a boolean). That is why I added the `typeof val !== 'boolean'` check to ensure it falls back to a standard toggle behavior instead of crashing.
- **Link vs Anchor Tags:** I explicitly realized that using standard `<a href="/about">` tags would cause the browser to request a completely new HTML document from the server. Using `<Link to="/about">` intercepts this request and just swaps the React components instantly.

# My React.js Learning Journey: Day 8

## 💡 Core Concepts

- **404 Page Handling:** I learned how to manage invalid URLs using the wildcard path (`path="/*"`). This route acts as a catch-all that renders when no other defined paths match the current URL. 
- **The Navigate Component:** Instead of just showing a "Not Found" message, I can use the `<Navigate />` component to automatically redirect users to a specific page (like the Home page) when they hit an invalid route.
- **Nested Routing & Layouts:** I explored how to nest routes inside one another to create complex UI structures (like a dashboard with a sidebar). 
    - **Relative Paths:** Inside nested routes, child paths should not start with a `/` because they are automatically relative to the parent path.
    - **The Outlet Component:** This is a crucial placeholder. I place `<Outlet />` in the parent component's JSX to tell React Router exactly where to render the child route's content.
    - **Index Routes:** I used the `index` attribute on a child route to specify which component should render by default when the parent's base path is visited.
- **Route Grouping (Prefixes):** I found that I can group related routes under a parent route to give them a shared URL prefix (e.g., grouping `/about` and `/login` under `/user` to create `/user/about` and `/user/login`).
- **Dynamic Routing:** I learned to create "variable" routes using the colon syntax (`:id`). This allows a single component to handle an infinite number of specific pages (like user profiles or product pages) by capturing the variable part of the URL.
- **Optional Segments:** I discovered that I can make parts of a URL optional by adding a question mark (`?`) after the segment name (e.g., `path="/users/list?"`). This allows the route to match both `/users` and `/users/list`.
- **The useParams Hook:** This is the tool I use to retrieve dynamic data from the URL. It returns an object where the keys match the variable names I defined in my route path (e.g., `{ id: '1' }`).



---

## 🛠️ Implementation

### 404 Handling & Redirection
I implemented a catch-all route for error pages and practiced using the `Navigate` component for automatic redirection.

```javascript
import { BrowserRouter, Routes, Route, Navigate } from "react-router";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<h1>Home</h1>} />
        {/* Wildcard catch-all for 404 message */}
        <Route path="/*" element={<h1>404: Page Not Found</h1>} />
        
        {/* Alternative: Redirect to Home for any invalid URL */}
        {/* <Route path="/*" element={<Navigate to="/" />} /> */}
      </Routes>
    </BrowserRouter>
  );
}
```

### Nested Routes with Index & Outlet
I created a `College` parent component that manages its own navigation and uses `<Outlet />` to render its children (`Student`, `Department`, etc.).



**App.jsx (Routing Configuration)**
```javascript
<Route path="/college" element={<College />}>
  {/* Default child rendered via 'index' */}
  <Route index element={<h1>Student Page</h1>} />
  <Route path="department" element={<h1>Department Page</h1>} />
  <Route path="details" element={<h1>College Details Page</h1>} />
</Route>
```

**College.jsx (Parent Layout)**
```javascript
import { Link, Outlet } from "react-router";

function College() {
  return (
    <>
      <nav>
        {/* Empty 'to' points to the index route */}
        <Link to="">Student</Link>
        <Link to="department">Department</Link>
        <Link to="details">College Details</Link>
      </nav>
      {/* Children render here */}
      <Outlet />
    </>
  );
}
```

### Dynamic Parameters and Optional Segments
I implemented a User Detail system where the ID is required, but the name is optional. I used `useParams` to extract this data in the detail view.

```javascript
// Route Definition
<Route path="/users/:id/:name?" element={<UserDetails />} />

// UserDetails.jsx
import { useParams } from "react-router";

export default function UserDetails() {
  const { id, name } = useParams(); // Destructuring dynamic params
  return (
    <>
      <h1>User Detail</h1>
      <p>ID: {id}</p>
      {name && <p>Name: {name}</p>}
    </>
  );
}
```

---

## 📂 Workflow & Tools

- **URL Segments:** I defined segments as the parts of a URL separated by `/`. I learned that React Router treats each segment as a potential logic gate for navigation.
- **Relative Link Syntax:** In nested routes, I noticed that `Link to="department"` (no slash) appends to the current path, while `Link to="/department"` would jump back to the root level.
- **useParams Extraction:** I practiced using `params.id` (or destructuring) to fetch data. This is essential for fetching specific user data from an API based on the URL.

---

## 🔍 Key Corrections

- **The Slash Mistake:** I initially tried to start nested child paths with `/` (e.g., `path="/department"` inside `/college`). I corrected this because leading slashes make paths absolute (root-level), which breaks the nesting logic.
- **Navigate vs Link:** I learned that `<Link>` is for user-initiated clicks, while `<Navigate />` is a component that triggers navigation automatically as soon as it renders.
- **Key Requirement in Maps:** When I generated my user list using `.map()`, I ensured the `key` was placed on the parent `div` wrapping the `<Link>` to maintain React's reconciliation efficiency.
- **Optional Parameter Positioning:** I noted that optional segments (using `?`) are usually placed at the end of the path. If I have `/users/:id?/:name`, it can cause ambiguity in how React Router parses the URL.

# My React.js Learning Journey: Day 9

## 💡 Core Concepts

- **The NavLink Component:** I learned that `NavLink` is a specialized version of the `Link` component designed specifically for navigation bars. Its main advantage is that it automatically knows if it is "active" (meaning its `to` prop matches the current URL). 
    - **Default Behavior:** It adds an `active` class to the element by default.
    - **Custom Styling:** I can pass a function to the `className` prop to apply conditional styles based on the `{ isActive }` boolean.
- **TailwindCSS Integration:** I explored using TailwindCSS, a utility-first CSS framework. Instead of writing separate CSS files, I apply pre-defined utility classes directly to my JSX elements. I found that installing it involves a specific setup with Vite (initializing a config file and adding directives to `index.css`).
- **Data Fetching (The Fetch API):** I practiced retrieving data from external servers. 
    - **Lifecycle Integration:** I use `useEffect` with an empty dependency array `[]` to ensure the API call happens exactly once when the component mounts.
    - **State Management:** I maintain a `loading` state (boolean) to show a "Loading..." message while the asynchronous request is pending, and a `users` state (array) to store the result once it arrives.
- **json-server (Mock APIs):** I found a way to simulate a full REST API without writing backend code. By using `json-server` and a `db.json` file, I can perform GET, POST, PUT, and DELETE operations on my local machine. This is perfect for development when the real backend isn't ready yet.



---

## 🛠️ Implementation

### NavLink and Conditional Classes
I practiced using `NavLink` with both default behavior and a custom function to handle specific active styles.

```javascript
import { BrowserRouter, Routes, Route, NavLink } from "react-router";

function App() {
  return (
    <BrowserRouter>
      <div>
        {/* NavLink automatically adds 'active' class if the URL matches '/' */}
        <NavLink className="link" to="/">Home</NavLink>

        {/* Custom logic: applying 'custom-active' based on isActive state */}
        <NavLink 
          className={({ isActive }) => isActive ? "custom-active link" : "link"} 
          to="/users"
        >
          Users List
        </NavLink>
      </div>

      <Routes>
        <Route path="/" element={<h1>Home Page</h1>} />
        <Route path="/users" element={<h1>Users List</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Fetching API with Loading States
I implemented an asynchronous fetch function inside `useEffect` to populate a table with user data from an external URL.



```javascript
import { useState, useEffect } from "react";

function App() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    setLoading(true);
    fetchUsersData();
  }, []);
  
  async function fetchUsersData() {
    // Standard JS fetch inside an async function
    const response = await fetch("https://dummyjson.com/users");
    const data = await response.json();
    
    setUsers(data.users);
    setLoading(false); // Stop showing the loading message
  }

  return (
    <>
      <h1>Users Data</h1>
      {loading ? (
        <h1>Data Loading...</h1>
      ) : (
        <table border="2" cellPadding="20">
          <thead>
            <tr>
              <th>First Name</th>
              <th>Last Name</th>
              <th>Age</th>
            </tr>
          </thead>
          <tbody>
            {users && users.map(user => (
              <tr key={user.id}>
                <td>{user.firstName}</td>
                <td>{user.lastName}</td>
                <td>{user.age}</td>
              </tr>
            ))}
          </tbody>
        </table>
      )}
    </>
  );
}
```

### Setting up json-server
I practiced creating a local mock database to serve as my backend for testing.

**db.json**
```json
{
    "users": [
        {"name": "Anil", "age": 29, "email": "anil@test.com"},
        {"name": "Sam", "age": 34, "email": "sam@test.com"},
        {"name": "Tony", "age": 31, "email": "tony@test.com"}
    ]
}
```

---

## 📂 Workflow & Tools

- **Tailwind Installation:** I learned to follow the official "Tailwind CSS with Vite" documentation. This requires installing `tailwindcss`, `postcss`, and `autoprefixer`, then generating the `tailwind.config.js` file.
- **json-server Commands:**
    - **Install:** `npm install json-server`
    - **Run:** `npx json-server db.json` (Defaults to `http://localhost:3000`).
- **REST Endpoints:** I noted that `json-server` automatically creates endpoints like `/users` which support all standard HTTP methods (GET, POST, etc.).

---

## 🔍 Key Corrections

- **Fetching Loop Error:** I realized that if I call `fetchUsersData()` directly in the component body instead of inside `useEffect`, it will trigger an infinite loop. This happens because the fetch updates the state, which triggers a re-render, which calls the fetch again.
- **Conditional Rendering with Tables:** I found that checking `!loading` before rendering the table prevents the UI from trying to map over an empty `users` array before the data has arrived.
- **NavLink Function Syntax:** I noted that the `className` prop in `NavLink` expects a function that receives an object. I must destructure it as `({ isActive })` to access the boolean.
- **JSON Format:** In `db.json`, I must use strict JSON syntax (double quotes for keys and strings) or the server will fail to start.

# My React.js Learning Journey: Day 10

## 💡 Core Concepts
- **Programmatic Navigation:** I learned to use the `useNavigate` hook for redirecting users after a logic sequence (like moving back to the list after an "Edit" or "Delete" operation).
- **Full CRUD via Fetch:** I practiced all four HTTP methods with the `fetch` API. 
    - **GET:** The default method for retrieving data.
    - **POST:** Used for adding new data; requires a `method`, `headers`, and a `body` containing `JSON.stringify(data)`.
    - **PUT:** Used for updating existing data; target URLs must include the specific ID.
    - **DELETE:** Used to remove data; requires the specific ID in the URL.
- **Complex State with `useReducer`:** For components with many related state variables (like large forms), I use `useReducer`. It separates state transition logic into a **reducer function** and uses **dispatch** to trigger updates. This is more scalable than managing five or six separate `useState` hooks.
- **Computed Property Names:** In my reducer, I used the ES6 syntax `[action.type]: action.val`. This allows me to use the string stored in `action.type` as a dynamic key for the state object.
- **Performance Optimization (Lazy Loading):** I learned that `lazy` and `Suspense` allow for "code splitting." Instead of the browser downloading the entire application at once, it only loads specific components when they are requested. This significantly improves the initial load speed.
- **The `use` API (React 19):** I discovered the `use` hook, which simplifies promise handling. Unlike `useContext` or `useEffect`, `use` can be called inside conditional statements and loops, providing much more flexibility when consuming context or data resources.



---

## 🛠️ Implementation

### CRUD Operations & Navigation
I implemented a full user management system using `useParams` to fetch IDs for editing and `useNavigate` for post-action redirection.

```javascript
// UserEdit.jsx - Handling PUT Request
const editUser = async () => {
    const response = await fetch(`http://localhost:3000/users/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name, age, email })
    });
    const data = await response.json();
    if (data) {
        alert("User Edited Successfully");
        navigate("/");
    }
}
```

### Form Validation (Manual & useActionState)
I practiced two types of validation: real-time state tracking and the new React 19 `useActionState` for forms.

```javascript
// useActionState Validation Logic
const handleLogin = (prevData, formData) => {
    const username = formData.get("username");
    const password = formData.get("password");

    if (!username || username.length > 5) {
        return { error: "Username must be 1-5 characters", username, password };
    } else if (password.length < 8) {
        return { error: "Password must be at least 8 characters", username, password };
    }
    return { message: "Login Successful", username, password };
}
```

### useReducer for Large Forms
I centralized my state management using a reducer function and the computed property name syntax.

```javascript
const emptyData = {
  name: "",
  password: "",
  email: "",
  city: "",
  address: ""
};

function reducer(data, action) {
  // Using Computed Property Name [action.type] to update dynamic keys
  return { ...data, [action.type]: action.val };
}

function App() {
  const [state, dispatch] = useReducer(reducer, emptyData);

  return (
    <input 
      type="text" 
      value={state.name} 
      onChange={(e) => dispatch({ val: e.target.value, type: "name" })} 
    />
  );
}
```

### Lazy Loading & Suspense
I optimized performance by loading the `User` component only when the user clicks a button.

```javascript
import { lazy, Suspense, useState } from "react";

// Component is only imported when demanded
const User = lazy(() => import("./User"));

function App() {
  const [load, setLoad] = useState(false);

  return (
    <>
      <button onClick={() => setLoad(true)}>Load User</button>
      {load ? (
        <Suspense fallback={<h1>Loading Component...</h1>}>
          <User />
        </Suspense>
      ) : null}
    </>
  );
}
```

### Asynchronous `use` API
I practiced using the `use` API to process promises outside the traditional `useEffect` flow.

```javascript
const fetchData = () => fetch("https://dummyjson.com/users").then(res => res.json());
const userResource = fetchData(); // Call outside to prevent recreation

function Users() {
  const usersData = use(userResource); // Processes the promise directly
  return (
    <>
      {usersData?.users?.map(user => (
        <h1 key={user.id}>{user.firstName} {user.lastName}</h1>
      ))}
    </>
  );
}
```

---

## 📂 Workflow & Tools
- **json-server endpoints:**
    - `GET /users`: Retrieves all records.
    - `POST /users`: Adds a new record.
    - `PUT /users/:id`: Replaces a specific record.
    - `DELETE /users/:id`: Removes a specific record.
- **Form Status:** I use the `pending` boolean from `useActionState` to disable submit buttons while an action is in progress.
- **Context with `use`:** I learned that I can conditionally call `use(ThemeContext)` after an early return, which is impossible with `useContext`.

---

## 🔍 Key Corrections
- **Fetch Headers:** I learned that when using `POST` or `PUT`, I must include `'Content-Type': 'application/json'` in the headers for most servers to correctly parse the `body`.
- **Validation Timing:** In manual state validation, I found it's safer to check the `event.target.value` directly because state updates are asynchronous and the state variable itself might be one step behind during the execution of the change handler.
- **Lazy Load Exports:** I noted that components being lazy-loaded **must** be exported as `export default`.
- **useId Limitation:** I reaffirmed that while `useId` is great for accessibility and form labels, it should never be used as a `key` in a `.map()` loop; I must use data-driven IDs for that purpose.