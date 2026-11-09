# ASHPOT React Beginner Guide: Setup and Module 01

**Instructor:** Chigemezu Ezimoha
**Goal:** Install the tools, create your first React project, and complete Module 01 (JavaScript modules: named and default exports).

---

## Part 1: Set Up Your Computer

### Step 1. Install Node.js

Node.js lets you run JavaScript tools (including Vite) on your computer.

1. Go to [nodejs.org](https://nodejs.org) and download the **LTS** version.
2. Install it with the default options.
3. Open a terminal (Command Prompt, PowerShell, or the VS Code terminal) and check:

```bash
node -v
npm -v
```

Each command should print a version number. If you see "command not found", close the terminal, reopen it, and try again. If it still fails, reinstall Node.js.

> Vite needs a reasonably recent Node version. If the setup below complains about your Node version, install the latest LTS.

### Step 2. Install VS Code

Download it from [code.visualstudio.com](https://code.visualstudio.com). Open it once the install finishes.

---

## Part 2: Create Your React Project

React needs a "build tool" to run. We use **Vite** because it is fast and simple.

### Step 3. Create the project

In your terminal, go to the folder where you keep your projects (for example `Documents`), then run:

```bash
npm create vite@latest ashpot-react-dashboard -- --template react
```

If it asks questions, choose:

- **Framework:** React
- **Variant:** JavaScript

### Step 4. Install dependencies and start the app

```bash
cd ashpot-react-dashboard
npm install
npm run dev
```

The terminal prints a local address, usually `http://localhost:5173`. Open it in your browser. You should see the Vite + React starter page.

To stop the server, press `Ctrl + C` in the terminal.

### Step 5. Open the project in VS Code

```bash
code .
```

If `code .` does not work, open VS Code and choose **File > Open Folder**, then select `ashpot-react-dashboard`.

### Step 6. Know what you are looking at

| File or folder | What it does |
|---|---|
| `index.html` | The single HTML page. Contains `<div id="root">` |
| `src/main.jsx` | Entry point. Connects React to the `root` element |
| `src/App.jsx` | Your main component |
| `package.json` | Lists your project's scripts and dependencies |
| `node_modules/` | Installed packages. Never edit or share this folder |

---

## Part 3: Prepare the Project for Module 01

Module 01 is about **JavaScript modules**, not React yet. We will practise `export` and `import` using plain `.js` files and see the results in the browser console.

### Step 7. Create the module folder

Inside `src`, create a new folder called `modules`.

Your structure should now look like this:

```text
ashpot-react-dashboard/
├── index.html
├── package.json
└── src/
    ├── App.jsx
    ├── main.jsx
    └── modules/        <-- new folder
```

### Step 8. Create a practice entry file

Inside `src`, create a file named `practice.js`. This is where we will test our modules.

Open `src/main.jsx` and add this line **at the very top**, above the other imports:

```js
import './practice.js';
```

> This is a **side-effect import**. It runs the file without importing any value from it. We use it so our `console.log` results appear in the browser.

Now open your browser, press `F12`, and click the **Console** tab. This is where our output will appear.

---

## Part 4: Module 01 Practice

### Key idea

- A **module** is a file with its own scope.
- `export` makes something available to other files.
- `import` brings it into the file that needs it.
- Importing a function does **not** run it. You must call it.

---

### Exercise A: Named exports

**1.** Create `src/modules/grades.js` and type this:

```js
export const PASS_MARK = 50;

export function getGrade(score) {
  if (score >= 70) return 'A';
  if (score >= 60) return 'B';
  if (score >= 50) return 'C';
  if (score >= 45) return 'D';
  if (score >= 40) return 'E';
  return 'F';
}

export const isPassing = (score) => score >= PASS_MARK;
```

**2.** Replace the contents of `src/practice.js` with:

```js
import { PASS_MARK, getGrade, isPassing } from './modules/grades.js';

console.log(PASS_MARK);      // 50
console.log(getGrade(78));   // A
console.log(isPassing(49));  // false
```

**3.** Save and check the browser console. You should see `50`, `A`, and `false`.

**Remember:**

- Named imports use **curly braces**.
- The names inside the braces must **exactly match** the exported names.
- Spelling and capital letters matter.

---

### Exercise B: Default export

**1.** Create `src/modules/report.js`:

```js
import { getGrade } from './grades.js';

export default function formatReport(student) {
  return `${student.name} scored ${student.score} and earned ${getGrade(student.score)}.`;
}
```

**2.** Add this to `src/practice.js` (below what you already have):

```js
import formatReport from './modules/report.js';

console.log(formatReport({ name: 'Ada', score: 78 }));
// Ada scored 78 and earned A.
```

> Imports are usually written together at the top of a file. If you prefer, move this new `import` line to the top, beside the first one.

**Remember:**

- A file can have **only one** default export.
- A default import has **no curly braces**.
- You choose the local name. `import buildReport from './modules/report.js'` also works, but in this class we keep the same name to avoid confusion.
- "Default" does not mean "runs automatically".

---

### Exercise C: Import aliases

An alias gives an import a different local name. This helps when two modules export the same name.

```js
import { getGrade as calculateGrade } from './modules/grades.js';

console.log(calculateGrade(50)); // C
```

- `getGrade` is the **exported** name.
- `calculateGrade` is the **local** name used in this file.

> The braces in an import are **module syntax**. They are not object destructuring, even though they look similar.

---

### Exercise D: One file with both export styles

Create `src/modules/course.js`:

```js
export const COURSE_NAME = 'React';

export default function getCourseLabel() {
  return `Course: ${COURSE_NAME}`;
}
```

Import both in `src/practice.js`:

```js
import getCourseLabel, { COURSE_NAME } from './modules/course.js';

console.log(COURSE_NAME);        // React
console.log(getCourseLabel());   // Course: React
```

The default import comes first (no braces), then the named import (with braces).

---

### Exercise E: Exporting an object is not the same as named exports

```js
// src/modules/settings.js
const settings = { passMark: 50, school: 'ASHPOT' };
export default settings;
```

```js
// src/practice.js
import settings from './modules/settings.js';

console.log(settings.passMark); // 50
```

This will **not** work:

```js
import { passMark } from './modules/settings.js'; // Wrong
```

`passMark` is a property inside the object. It is not a named export.

---

### How to read an import line

```js
import { getGrade } from './modules/grades.js';
```

| Part | Meaning |
|---|---|
| `import` | I want something from another module |
| `{ getGrade }` | Take the named export called `getGrade` |
| `from` | Here is where to find it |
| `'./modules/grades.js'` | The file path, starting from the current file's folder |

**Rules to remember:**

- Put `import` statements at the **top** of the file, not inside `if`, loops, or functions.
- You cannot reassign an imported value. Treat imported data as read-only.
- Always write the file extension (`.js`) in this course.

---

## Part 5: Your Independent Task

Do this **without copying** from the examples.

1. In `grades.js`, add a named export `getResultLabel(score)` that returns `'Pass'` or `'Fail'`.
2. Reuse `isPassing` inside it. Do not write the number `50` again.
3. Use it inside `report.js` so the report also says Pass or Fail.
4. In another file, import it using an **alias**, and log the result.

Expected output example:

```text
Ada scored 78 and earned A. Result: Pass
```

---

## Part 6: Break It on Purpose

Real developers learn by reading errors. Try each fault below one at a time, read the error in the browser or terminal, then fix it.

| Fault | What to try | What you should learn |
|---|---|---|
| Wrong braces | `import { formatReport } from './modules/report.js'` | A default export needs no braces |
| Misspelled name | `import { getgrade } from './modules/grades.js'` | Export names are case-sensitive |
| Two default exports | Add a second `export default` in one file | Only one default per file |
| Object mistaken for named exports | `import { passMark } from './modules/settings.js'` | Properties are not named exports |
| Wrong path | `import { getGrade } from './grades.js'` inside `practice.js` | The path starts from the importing file |

---

## Part 7: Checkpoint

You are ready to move on when you can do all of these **without copying**:

- [ ] Write a named export and import it.
- [ ] Write a default export and import it.
- [ ] Use an alias on a named import.
- [ ] Explain why two files can use different local names for the same default import.
- [ ] Fix at least three of the faults in Part 6.

Show your instructor your working console output and explain one exercise in your own words.

---

## Troubleshooting

| Problem | Likely fix |
|---|---|
| `npm` or `node` not recognised | Reinstall Node.js, then reopen the terminal |
| `npm run dev` fails | Make sure you ran `cd ashpot-react-dashboard` and `npm install` first |
| Nothing appears in the console | Check that `main.jsx` has `import './practice.js';` and that you saved all files |
| "Failed to resolve import" | Check the file path, folder names, spelling and `.js` extension |
| "does not provide an export named..." | Check that the name matches exactly, and whether it is a default or named export |
| Page is blank | Read the red error in the terminal or browser console |

---

## Quick Reference

```js
// Named export / import
export const A = 1;
import { A } from './file.js';

// Default export / import
export default function thing() {}
import thing from './file.js';

// Alias
import { A as myA } from './file.js';

// Both together
import thing, { A } from './file.js';
```

**Next:** Module 02 teaches file paths (`./`, `../`, `../../`) and other import forms.
