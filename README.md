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