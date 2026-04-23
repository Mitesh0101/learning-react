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