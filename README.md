# My React.js Learning Journey: Day 1

## Execution & Workflow
- **Entry Point:** The browser loads `index.html` first. This file contains a `<div id="root"></div>` which acts as the mount point for the entire application.
- **The Bridge:** `index.html` includes a script tag pointing to `main.jsx`. This file uses `ReactDOM` to inject the `App.jsx` content into the root div.
- **JSX (JavaScript XML):** Components in React return JSX, which looks like HTML but is actually JavaScript. The browser cannot read `.jsx` directly; Vite transforms it into standard JavaScript objects ($React.createElement$) during the build process.
- **The Fragment Rule:** A component function can only return a single parent element. If I don't want to add an unnecessary `<div>` to the DOM, I can use a **Fragment** (`<> ... </>`) to wrap multiple tags.
- **Imports/Exports:** To keep code modular, I create separate `.jsx` files for components (e.g., `Header.jsx`). These must be exported (usually `export default`) and then imported into `App.jsx`.

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
export default App
```

## Project Structure
- **package.json:** This is the manifesto of the project. It tracks dependencies (libraries needed for production), devDependencies (tools for development), and scripts like `dev` or `build`.
- **vite.config.js:** Used to configure the development server, such as changing the PORT or adding plugins.
- **node_modules:** Stores the actual code for all dependencies. This folder is huge and should never be edited or pushed to Git.
- **src vs public:** - **public/**: Files here are served as-is at the root path (e.g., `favicon.ico`).
    - **src/assets/**: Files here are processed by Vite’s build pipeline. I must `import` these files in my JS/JSX to use them so Vite can optimize and hash them for caching.
- **.gitignore:** Prevents heavy or sensitive files (like `node_modules` or `.env`) from being uploaded to GitHub.

## Components
- **Composition:** React UIs are built by nesting components inside each other.
- **Naming Convention:** Component names **must** start with a Capital letter. This is how the transpiler distinguishes between a React component (`<Apple />`) and a standard HTML tag (`<header>`).
- **Nesting Example:**
```javascript
function App() {
    return (
        <div>
            <h1>My Fruit List</h1>
            <Fruit />   
            <Apple />   
        </div>
    )
}

function Fruit() {
    return <h1>This is a generic Fruit component</h1>
}

function Apple() {
    return <h1>This is an Apple component</h1>
}

export default App
```