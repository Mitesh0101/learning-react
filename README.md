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