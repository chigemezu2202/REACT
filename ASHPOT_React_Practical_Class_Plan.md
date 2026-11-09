# ASHPOT Microsystems
# Practical React Course: JavaScript Modules to Advanced React

**Instructor:** Chigemezu Ezimoha  
**Audience:** Students who have completed the vanilla JavaScript Student Course Management Dashboard.  
**Format:** Practical modules with brief explanations, working features, debugging exercises and demonstrations of understanding.  
**Pacing:** Determined by the instructor. Module numbers indicate learning order, not dates or lesson duration.  
**Document purpose:** Instructor class plan. Student walkthroughs, starter files, visual references and video production follow as a separate preparation stage.

## 1. Teaching direction

Begin with JavaScript modules and file paths. Then teach React through function components, props, state and Hooks. Introduce JavaScript classes and React lifecycle methods after students can already build and explain a functioning React application.

React's documentation recommends function components for new code; class components remain supported. Therefore, `componentDidMount` belongs in a practical comparison and migration lesson, not as a prerequisite for a beginner's first component. See [React Component reference](https://react.dev/reference/react/Component).

Use JavaScript and Vite for the main course so students can concentrate on React. Keep ordinary CSS initially; introduce CSS Modules when style collisions become a concrete problem. TypeScript and framework features belong in the extension track. React documents a build-tool approach as suitable for learning React fundamentals: [Build a React app from scratch](https://react.dev/learn/build-a-react-app-from-scratch).

### The repeated classroom pattern

1. **Show the target behaviour.** Demonstrate one feature students are about to build.
2. **Explain the immediate reason.** For example: “Both the form and the table need these records, so their parent will own the student state.”
3. **Build a small part together.** Stop at visible, working increments.
4. **Require an independent variation.** Change a field, add a feature, or repeat the pattern in another component.
5. **Introduce a deliberate fault.** Let students read the error and repair it.
6. **Run the checkpoint.** Advance when each student can demonstrate, modify and briefly explain the feature.

The explanation supports the practical activity; it does not become a separate lecture. A copied result alone is insufficient evidence of understanding. On shared computers, rotate the keyboard operator and require an individual modification from each learner.

## 2. Use the completed project as the bridge

The existing capstone already includes registration, editing, deletion, search, course filtering, grading, statistics, API users and local persistence. Rebuild these behaviours in React before expanding the application.

| Existing vanilla JavaScript work | React learning connection |
|---|---|
| Separate helper files | ES module imports and exports |
| `createElement()` and row rendering | JSX and components |
| `addEventListener()` | React event props such as `onClick` and `onSubmit` |
| An array of student records | State owned by an appropriate component |
| Calling a dashboard redraw function | State updates that cause React to render |
| `map()`, `filter()`, `find()`, `reduce()` | List rendering, immutable updates and calculated summaries |
| `FormData` and validation | Controlled inputs first; alternative form patterns later |
| Fetching external users | Loading, success, empty, error and retry UI |
| Local storage helpers | Persistence logic wrapped in a custom Hook |

Keep a working copy of the vanilla application for comparison. Reuse pure grading and collection helpers where suitable. React will own the rendered interface; the old DOM-manipulating `ui.js` should be replaced by components.

### Keep the same domain rules

- Record fields: `id`, `name`, `email`, `course`, `score`.
- Generate a stable ID when adding a record; preserve it when editing.
- Keep the existing course options unless the class deliberately changes them.
- Grades: A = 70–100; B = 60–69; C = 50–59; D = 45–49; E = 40–44; F = 0–39.
- Passing score: 50 or higher.
- Dashboard summary cards describe the complete student list, even while the table is filtered.
- External API users remain separate from locally registered students.
- Course enrolment counts are derived from the student records; include courses with zero students.

### Final project pages

| Page | Required behaviour |
|---|---|
| Overview | Total, passed, failed and average score; course summaries; external users section |
| Students | Register, list, search, filter, edit, cancel edit and delete; clear validation and empty states |
| Courses | Display available courses and enrolment counts; open a course's filtered student view |
| Student details | Open a student by ID in the URL; handle an unknown or deleted ID |
| Supporting interface | Shared navigation, responsive layout, meaningful loading/error feedback and keyboard access |

The first three pages preserve the current project direction. Student details is a teaching extension for route parameters.

## 3. Module map

| Module | Focus | Practical deliverable |
|---|---|---|
| 01 | JavaScript modules: named and default exports | Modular grade-report feature |
| 02 | File paths and other import forms | Reorganised files with correctly repaired imports |
| 03 | React purpose, Vite and application entry | Running React dashboard shell |
| 04 | JSX, components, styling and assets | Reusable static dashboard layout |
| 05 | Props, children and composition | Configurable summary and student cards |
| 06 | State, events and ordinary Hook rules | Interactive counter, toggle and course selector |
| 07 | Lists, conditions and keys | Student table with meaningful empty states |
| 08 | Forms and validation | Working student registration form |
| 09 | Shared state and immutable CRUD | Add, edit, cancel and delete across components |
| 10 | Derived data and state design | Search, filters, statistics and course counts |
| 11 | Rendering, Effects and cleanup | Mount/unmount timer or subscription lab |
| 12 | API reading and asynchronous UI | External users panel with retry and stale-request protection |
| 13 | Persistence and custom Hooks | Refresh-safe student records and reusable logic |
| 14 | Routing | Linked Overview, Students, Courses and Details pages |
| 15 | Reducers and context | Shared application state with explicit actions |
| 16 | Refs and reusable accessible UI | Focusable form and reusable confirmation dialog |
| 17 | Classes and lifecycle migration | Class timer converted into a function component |
| 18 | Error boundaries and recovery | Failed widget isolated behind a recovery interface |
| 19 | Dynamic imports and Suspense | Reports screen loaded on demand |
| 20 | Performance and responsive updates | Measured improvement to a large student list |
| 21 | API mutations, server state and session UI | Persistent backend CRUD and a session-aware interface |
| 22 | Behaviour testing and accessibility | Meaningful automated tests and keyboard validation |
| 23 | Structure, build and release | Reproducible production build and release checklist |
| 24 | Independent capstone validation | Completed React dashboard with an independent extension |

## 4. Detailed opening class: JavaScript modules

### Module 01 — Export and import the correct thing

**Starting point:** A working grading function in the existing application.

**Brief explanation:** A module is a file treated as a module by its environment. Its top-level declarations have their own scope. `export` makes selected values available to other modules; `import` creates access to those exports. Importing a function does not call that function. Imports are also not a mechanism for updating React state.

**Build:** Separate grading logic and report formatting, then use both from an entry file.

#### A. Named exports

```js
// src/modules/grades.js
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

```js
// src/main.js
import { PASS_MARK, getGrade, isPassing } from './modules/grades.js';

console.log(PASS_MARK);       // 50
console.log(getGrade(78));    // A
console.log(isPassing(49));   // false
```

Explain the braces and exact exported names. A module may have several named exports, or just one. The grading helper assumes a validated score; checking empty, non-numeric and out-of-range input remains the form's responsibility.

An alternative is to declare values first and export them together:

```js
// Alternative style; use this instead of exporting the same names twice.
const PASS_MARK = 50;
const isPassing = (score) => score >= PASS_MARK;

export { PASS_MARK, isPassing };
```

#### B. Default export

```js
// src/modules/report.js
import { getGrade } from './grades.js';

export default function formatReport(student) {
  return `${student.name} scored ${student.score} and earned ${getGrade(student.score)}.`;
}
```

```js
// src/main.js
import formatReport from './modules/report.js';

console.log(formatReport({ name: 'Ada', score: 78 }));
// Ada scored 78 and earned A.
```

A module can have at most one default export. The ordinary default-import form has no braces. The importing file chooses its local name:

```js
// Another valid local name for the same default export:
import buildReport from './modules/report.js';
```

Use consistent names in class projects even though renaming a default import is allowed. Default does not mean “runs automatically”, “required in every file” or “a React-only feature”.

#### C. Named import aliases

```js
import { getGrade as calculateGrade } from './modules/grades.js';

console.log(calculateGrade(50)); // C
```

Here `getGrade` is the exported name and `calculateGrade` is the local name. An alias is useful when two imported modules export the same name.

The braces in an import declaration are module syntax. They are not object destructuring, even though the appearance is similar.

#### D. A module with both export styles

```js
// src/modules/course.js
export const COURSE_NAME = 'React';

export default function getCourseLabel() {
  return `Course: ${COURSE_NAME}`;
}
```

```js
import getCourseLabel, { COURSE_NAME } from './modules/course.js';
```

This is legal. For the classroom, start with named exports for related helper functions and a default export for a file centred on one main component. This is a teaching convention, not a React requirement. Components can use named exports too.

#### E. Do not confuse an exported object with named exports

```js
// This exports ONE default object.
const settings = { passMark: 50, school: 'ASHPOT' };
export default settings;
```

The consumer uses a default import and then reads a property, for example `settings.passMark`. Writing `import { passMark }` would require a separately declared named export called `passMark`.

#### F. Read the entire import statement

In `import { getGrade } from './modules/grades.js'`:

| Part | Meaning |
|---|---|
| `import` | Declare access to another module's exports |
| `{ getGrade }` | Select its named export `getGrade` |
| `from` | Introduce the module specifier |
| `'./modules/grades.js'` | Locate the module relative to the importing file |
| `getGrade(78)` later in the code | Actually call the imported function |

Static imports belong at module top level; do not put them inside an `if`, loop or event handler. Use dynamic `import()` for on-demand loading later. Imported bindings cannot be reassigned by the consumer; that does not automatically freeze an imported object. Treat shared imported configuration and sample data as read-only in class exercises.

**Independent task:** Add a named `getResultLabel(score)` export returning `Pass` or `Fail`. Use it in the report without duplicating the passing rule. Then import it with a local alias in another file.

**Deliberate faults:** Wrong braces; misspelled exported name; two default exports; a default-exported object mistaken for named exports.

**Checkpoint:** Students write a named export/import pair, a default pair and an alias without copying. They explain why two consumers can give a default import different local names.

**References:** [MDN export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export), [MDN import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [React imports and exports](https://react.dev/learn/importing-and-exporting-components).

### Module 02 — Find the file before choosing the import syntax

**Build:** Move a component into a deeper folder and repair every affected import.

For the initial executable drill, use plain `.js` helper files in the existing Vite project. The `.jsx` folder map below previews the React structure; repeat it with actual components after Modules 03–04. Package, asset and CSS Module examples are introduced as their dependencies become available.

Explain the two independent questions in an import: **Which file is being loaded? Which export is being selected?** Correct braces cannot repair an incorrect path, and a correct path cannot create a missing export.

#### A. The symbols

| Syntax | Meaning in the examples |
|---|---|
| `.` | Current location; `./` starts from the importing module's folder |
| `..` | Parent location; `../` goes up one folder |
| `../../` | Go up two folders |
| `/` between names | Separate path segments, for example `components/students` |
| `/` at the start | In browser URLs, start at the origin root; Vite has root-based development resolution too. It is not a portable alias for every project's `src` folder. |
| `react` | A bare module specifier; in this Vite project, resolved through the installed package |
| `@/components/...` | An alias only if the project has configured it; `@` is not built-in directory navigation |
| `.js` or `.jsx` at the end | Part of the filename extension; this dot is not a parent-directory instruction |

Use forward slashes in imports, including on Windows. Do not paste `C:\Users\...` filesystem paths into source imports.

The path starts from the file containing the import, not from `index.html`, the terminal's current folder or the file currently selected in VS Code. Browser image URLs have their own resolution rules; they should not be confused with JavaScript import paths.

#### B. A concrete folder exercise

These paths are relative to the project root:

| File | Role |
|---|---|
| `src/App.jsx` | Main application component |
| `src/main.jsx` | React entry point |
| `src/components/students/StudentCard.jsx` | One student card |
| `src/components/students/StudentBadge.jsx` | Badge beside the card |
| `src/components/shared/Button.jsx` | Shared button |
| `src/modules/grades.js` | Grade helpers |
| `src/data/students.js` | Sample student records |
| `src/assets/ashpot-logo.png` | Image included through the build system |
| `src/styles/global.css` | Shared stylesheet |
| `public/ashpot-logo.png` | Alternative public-asset location for a separate demonstration |

From `src/components/students/StudentCard.jsx`:

| Target | Correct relative specifier |
|---|---|
| Badge in the same folder | `./StudentBadge.jsx` |
| Shared button in a sibling folder | `../shared/Button.jsx` |
| Grading helper | `../../modules/grades.js` |
| Sample data | `../../data/students.js` |
| Main application component | `../../App.jsx` — path exercise only; a child should not need to import its parent |

Walk through `../../modules/grades.js` aloud: start in `students`; first `../` reaches `components`; second reaches `src`; then enter `modules`; finally open `grades.js`.

From `src/App.jsx`, the card path is `./components/students/StudentCard.jsx`. The target is unchanged; the starting point changed.

#### C. Other import patterns students will meet

These are independent examples, not one block to paste into the same file:

| Pattern | Example | Practical meaning |
|---|---|---|
| Namespace import | `import * as grades from './modules/grades.js';` | Access named exports through `grades.getGrade(78)` |
| Side-effect import | `import './styles/global.css';` | In Vite, include the stylesheet without importing a local variable |
| Asset import | `import logoUrl from './assets/ashpot-logo.png';` | Vite supplies a usable asset URL; later use it as an image `src` |
| CSS Module import | `import styles from './StudentCard.module.css';` | Vite supplies class-name mappings; introduced when teaching component styling |
| Package import | `import { useState } from 'react';` | Load an export from an installed dependency |
| Explicit folder entry | `import { getGrade } from './modules/index.js';` | Load a deliberately created re-export entry file |
| Dynamic import | `const grades = await import('./modules/grades.js');` | Inside an async function, load a module on demand and receive its namespace |

CSS and image imports here are build-tool features. Do not present them as universally valid native-browser JavaScript behaviour. A bare browser module also needs an import map or another resolver to understand bare names such as `react`.

For a public asset, show `<img src="/ashpot-logo.png" alt="ASHPOT" />` when the application is served from the domain root. The URL does not include `public`. A subdirectory deployment needs the correct configured base path; revisit that during release preparation.

#### D. Re-exporting through an index file

```js
// src/modules/index.js
export { getGrade, isPassing } from './grades.js';
export { default as formatReport } from './report.js';
```

```js
// src/main.js
import { getGrade, formatReport } from './modules/index.js';
```

`formatReport` is now a named export of `index.js`, even though its original file uses a default export. `export * from './report.js'` would not automatically forward the default export. Introduce explicit re-exports before broad barrel files, and discuss circular dependencies only when students can trace the dependency chain.

#### E. Environment differences and debugging

- Keep explicit `.js` and `.jsx` extensions during these exercises. Build tools may resolve extensionless paths; native browser modules do not automatically search for `.js` or `/index.js` files.
- Use `.jsx` for files containing JSX in this course; use `.js` for plain JavaScript helpers.
- Match filename case exactly. An import tolerated by a case-insensitive local filesystem can fail elsewhere.
- In plain HTML, a module entry uses `<script type="module" src="./src/main.js"></script>`. Serve the exercise through the existing Vite development server.
- Briefly identify CommonJS as another module system using `require()` and `module.exports`. Do not mix it into these frontend ES-module exercises.

| Failure | First check |
|---|---|
| Cannot resolve or find a module | Start folder, `../` count, filename, extension and case |
| Module does not provide an export | Named/default agreement and exact exported name |
| Cannot use import outside a module | Entry configuration and execution environment |
| Package cannot be resolved | Whether the dependency is installed in this project |
| Imported value exists but the page does not change | Whether React state is being updated; importing is not rendering |

**Independent task:** Move `StudentBadge.jsx` into a new `badges` folder beneath `students`. Repair its consumer's path. Then move the grading helper and repair its consumers. Explain each path before using autocomplete.

**Checkpoint:** Correctly solve same-folder, child-folder, parent-folder, sibling-folder and two-parent-folder imports; distinguish a package import from a relative import; fix both a path fault and an export fault.

**References:** [MDN modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), [MDN dynamic import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import), [Vite assets](https://vite.dev/guide/assets).

## 5. React fundamentals: build the interface and make it interactive

### Module 03 — React, Vite and the entry point

**Build:** Create `ashpot-react-dashboard`, run it and display the dashboard name.

**Explain while building:** React is a library for composing interfaces from components. Vite provides the development and build environment. React DOM connects the component tree to the browser. JSX is transformed by the toolchain; the browser does not directly execute JSX source.

```bash
npm create vite@latest ashpot-react-dashboard -- --template react
cd ashpot-react-dashboard
npm install
npm run dev
```

Use a Node version supported by the installed Vite release. Inspect the generated scripts and retain the lockfile. During resource preparation, record the exact tested dependency versions for the whole class.

**Practical sequence:** Locate `index.html`, the root element, `src/main.jsx`, `createRoot`, `App.jsx`, CSS files and `package.json`. Trace how `App` reaches the browser. Remove the sample interface incrementally. Leave development checks enabled.

**Independent task:** Add a welcome message containing the student's name.

**Debug:** Break the `App` import, read the error, then repair it.

**Checkpoint:** Start the application from the correct folder and explain the entry flow in plain language. See [Vite setup](https://vite.dev/guide/).

### Module 04 — JSX, function components, styling and assets

**Build:** `Sidebar`, `Header`, `DashboardLayout` and a static summary section.

**Teach through edits:** Capitalised component names; returning JSX; one enclosing element or Fragment; closing tags; `className`; `htmlFor`; JavaScript expressions inside braces; ordinary stylesheet and asset imports. Explain the difference between the JavaScript `class` keyword and JSX `className`.

**Practical sequence:** Convert one section of existing HTML into JSX. Extract it into its own component. Export it and import it into `App`. Repeat with the sidebar. Introduce CSS Modules only after showing a real style-name collision.

**Independent task:** Add a footer component without duplicating its markup in each view.

**Debug:** Lowercase a component name or leave a tag unclosed; locate the actual cause of the error.

**Checkpoint:** Build a component from a blank file, import it correctly and style it without changing unrelated elements.

### Module 05 — Props, children and composition

**Build:** A `StatCard` reused for Total Students, Passed Students, Failed Students and Average Score; a reusable `Panel` accepting `children`.

**Teach through edits:** Props pass information from parent to child. Destructure parameters, pass strings/numbers/functions appropriately and use parameter defaults where helpful. Treat props as read-only.

**Practical sequence:** Replace four duplicated summary blocks with four uses of one component. Change labels and values using props. Wrap different content in `Panel` using `children`.

**Independent task:** Create a `StudentCard` accepting a student object and showing a grade badge.

**Debug:** Pass a number as an unsuitable string and inspect the resulting calculation or formatting.

**Checkpoint:** Reuse the same component for different records without editing the component for each record.

### Module 06 — State, events and the ordinary Hook rules

**Build:** A practice counter, a show/hide registration panel and a selected-course control.

**Teach through edits:** `useState`; array destructuring; state setters; state as a render snapshot; passing event handlers; functional state updates when the next value depends on the previous value. Ordinary Hooks such as `useState` and `useEffect` belong at the top level of function components or custom Hooks.

**Practical sequence:** First change an ordinary local variable and observe why it does not update the interface. Replace it with state. Compare `onClick={handleClick}` with accidentally calling the function during rendering. Demonstrate `setCount(previous => previous + 1)`.

**Independent task:** Add a reset button and a second independent counter.

**Debug:** Explain why logging a state variable immediately after its setter still sees the current render's value.

**Checkpoint:** Update the interface through state without manually changing DOM text. General reference: [React Quick Start](https://react.dev/learn).

### Module 07 — Lists, conditional rendering and stable keys

**Build:** Render sample students as rows, then display pass/fail badges and an empty-table message.

**Teach through edits:** `map()` for JSX; ternary expressions; conditional sections; stable record IDs as keys. A key helps React identify a list item; it is not automatically passed to the component as an ordinary prop.

**Practical sequence:** Start with two records. Replace repeated JSX with a mapped list. Add status badges. Remove all records and display a useful empty state. Reorder the list and inspect row identity.

**Independent task:** Add a separate message for “no students registered” and “no search matches”.

**Debug:** Remove a key and inspect the warning; then demonstrate why a fresh random key on every render is unsuitable.

**Checkpoint:** Render, reorder and remove rows using IDs created when records are added.

### Module 08 — Forms and validation

**Build:** Register a student using name, email, course and score.

**Teach through edits:** Controlled inputs; `value` and `onChange`; submit handling; `preventDefault`; form-object updates; browser validation plus explicit application rules. Input values arrive as strings; validate blank values before numeric conversion.

**Practical sequence:** Control one field first, then extend to the full form. Use visible labels. Trim text, check required values and validate an integer score from 0 to 100. Preserve a useful error message and clear the form only after a successful registration.

**Independent task:** Add an optional field with a clear validation rule and display it in the interface.

**Debug:** Ensure `0` is accepted while an empty score is rejected; do not use a truthiness check that treats zero as missing.

**Checkpoint:** Valid registration works by mouse or keyboard; invalid submission does not add a record.

### Module 09 — Shared state and immutable CRUD

**Build:** Add, edit, cancel and delete students through separate form and table components.

**Teach through edits:** Lift shared state to the nearest appropriate parent. Pass values down and callbacks to request changes. Use spread, `map()` and `filter()` to produce new arrays and changed records. Keep IDs stable.

**Practical sequence:** Place `students` in the common parent. Pass `onAdd`, `onEdit` and `onDelete` callbacks. Store `editingId`; find the selected record. Reuse the form for add/edit, and clear edit mode on cancel or when the selected record is deleted.

**Independent task:** Add a duplicate-record action that generates a new ID and leaves the original unchanged.

**Debug:** Demonstrate a mutation bug and repair it with an immutable update.

**Checkpoint:** Form, table and summary receive the same accepted updates; cancelled edits preserve the record. Reference: [Sharing state](https://react.dev/learn/sharing-state-between-components).

### Module 10 — Derived values and state design

**Build:** Combined name/email search, course filtering, the four summary cards and enrolment counts.

**Teach through edits:** Keep source records and user choices in state. Calculate filtered rows, grade labels and totals from them. Avoid separate state for values that can be calculated reliably during rendering.

**Practical sequence:** Combine search and course conditions. Calculate overall totals from all records and visible rows from the filtered records. Derive course counts from the full list and the configured course options. Sort a copied array instead of mutating state.

**Independent task:** Add a pass/fail filter that works alongside text and course filters.

**Debug:** Intentionally calculate summary cards from the filtered table, then restore the agreed full-list behaviour.

**Checkpoint:** Ada at 78 and Chidi at 49 produce 2 students, 1 passed, 1 failed and average 63.5. Searching for Ada changes the table but not these overall totals.

## 6. Synchronisation, navigation and reusable state

### Module 11 — Rendering, Effects and cleanup

**Build:** A timer panel that can be shown, hidden and configured.

**Teach through observation:** Rendering calculates UI; committing applies the result. An Effect synchronises with an external system. Use a timer or browser event subscription so the need for cleanup is visible.

**Practical sequence:** Create a timer Effect and return a cleanup that clears it. Hide and show the panel. Change a timer setting included in the dependency list. Observe cleanup before resynchronisation and when the panel unmounts.

| Effect form | Teaching point |
|---|---|
| No dependency argument | Runs after each committed render; rarely the intended subscription pattern |
| `[]` | No reactive dependencies; setup on mount, cleanup on unmount, plus applicable development checks |
| `[courseId]` or another dependency list | Resynchronise when a listed dependency changes; include every reactive value read by the Effect |

Do not describe `[]` as an absolute promise to run only once. Strict Mode can perform an extra development setup/cleanup cycle. Repair cleanup instead of disabling the check. Use an event handler for a submit action and calculate filtered data during rendering; neither needs an Effect merely because values changed.

**Independent task:** Add an online/offline indicator with a cleaned-up browser subscription.

**Checkpoint:** Repeatedly opening the panel does not accumulate timers or listeners. References: [useEffect](https://react.dev/reference/react/useEffect), [When Effects are unnecessary](https://react.dev/learn/you-might-not-need-an-effect).

### Module 12 — Fetching data and handling asynchronous states

**Build:** The separate External API Users panel with Load and Retry controls.

**Teach through failures:** `fetch`, `async/await`, HTTP status checks, response validation and explicit loading/success/empty/error states. Clicking Load belongs in an event handler. A request that must track a changing selected ID can be synchronised through an Effect.

**Practical sequence:** Use a service module for request logic. Display a loading message, disable duplicate submissions, validate the response, show results and recover from errors. Add a selection-driven request and use abort/ignore logic so an old response cannot replace the latest result. Treat an intentional abort separately from a user-facing failure.

**Independent task:** Add a user-details panel opened by selecting a returned user.

**Debug:** Switch the network offline, retry, and change selection rapidly under a slow connection.

**Checkpoint:** Success, an empty response, failure, retry and stale-response handling all have predictable UI.

### Module 13 — Persistence and custom Hooks

**Build:** Refresh-safe student data, then extract `useStudents` or a similarly focused custom Hook.

**Teach through refactoring:** A custom Hook packages reusable React logic. Separate calls reuse logic but do not automatically share the same state. Initialise deliberately, validate saved data and handle storage errors.

**Practical sequence:** Adapt the existing storage helpers. Load before enabling persistent writes; avoid overwriting stored records with an initial empty array. Preserve the earlier policy: a failed load shows a warning and disables persistent writes, while a failed save leaves the previously accepted list unchanged. A guarded update function can save the proposed array before accepting it into state. Keep side effects out of state updater callbacks.

**Independent task:** Extract the online/offline subscription into its own Hook and use it in two components.

**Debug:** Supply malformed saved JSON and simulate a failed write.

**Checkpoint:** Add/edit/delete survive refresh; corrupt storage is not silently overwritten; Hook consumers expose clear status. Reference: [Custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks).

### Module 14 — Routing and URL state

**Build:** Overview, Students, Courses, Student Details and a not-found page.

**Teach through navigation:** Routing is an ecosystem capability supplied here by React Router. Use links, shared layouts, route parameters and search parameters. Choose the documented declarative setup for the installed version.

**Practical sequence:** Create the three main routes. Add a nested/shared layout. Open `/students/:id` and look up a student by ID. Link course cards to a course-filtered student URL. Treat the URL as the source of truth for any filter that should survive sharing or browser Back.

**Independent task:** Preserve the text search in URL search parameters.

**Debug:** Open an unknown student ID; navigate Back and Forward; reload a nested URL.

**Checkpoint:** Direct links, browser navigation, active menu styles and missing-record states work. Production deep-link fallback is checked during release. Reference: [React Router installation](https://reactrouter.com/start/declarative/installation).

### Module 15 — Reducers and context

**Build:** Shared student state available to all routed pages.

**Teach through refactoring:** `useReducer` expresses state changes as actions; context makes a value available through a subtree. Neither is needed for every small component. Keep temporary form text local unless another feature needs it.

**Practical sequence:** Identify where callback and prop passing has become difficult. Refactor accepted record changes into pure reducer actions such as add, update and remove. Perform storage/network work outside the reducer. Expose state and actions through a provider and a consumer Hook.

**Independent task:** Add a reset-filter or bulk-removal action with predictable behaviour.

**Debug:** Render a consumer outside its provider and provide a useful development error.

**Checkpoint:** Students, Overview and Courses agree on the same data after every change, without storing duplicate copies of derived totals.

### Module 16 — Refs and reusable accessible interface elements

**Build:** A form that focuses the name field when appropriate, a reusable labelled field and a delete-confirmation dialog.

**Teach through interaction:** `useRef` retains a reference without causing a render. Use state for visible values. Use `useId` for unique label/control relationships when needed. Introduce portals when a dialog needs to render outside a clipping container.

**Practical sequence:** Focus a real DOM input through a ref. Build a reusable field with label and error association. Implement dialog semantics, Escape handling, initial focus, focus containment and return focus; a native dialog or an accessible library can support these behaviours. A portal alone does not provide accessibility.

**Independent task:** Reuse the dialog for a different confirmation action.

**Debug:** Create two form instances and fix duplicate IDs.

**Checkpoint:** The complete interaction works with keyboard only. Introduce `useLayoutEffect` only for a demonstrated before-paint measurement need, such as tooltip positioning.

## 7. Advanced React and application reliability

### Module 17 — JavaScript classes, lifecycle methods and migration

**Build:** A class-based timer, then convert it into a function component while keeping its behaviour.

**Class prerequisites taught inside this module:** `class`, `extends`, `this`, a constructor when needed, `super(props)`, instance methods and React's `Component` base class. Keep the exercise small.

| Class API | Practical meaning | Function-component comparison |
|---|---|---|
| `render()` | Describe the UI from props and state | Component function returns JSX |
| `this.props` | Input from a parent | Function parameter / destructured props |
| `this.state` and `this.setState()` | Store and update class state | `useState` or `useReducer`; their update behaviour is not identical |
| `componentDidMount()` | Run mount work after the initial DOM commit | Effect setup for the external resource |
| `componentDidUpdate()` | React to a committed update, often with previous-value guards | Effect dependencies describe when resynchronisation is required |
| `componentWillUnmount()` | Release subscriptions, timers and other resources | Effect cleanup; it also runs before a changed Effect's next setup |

**Practical sequence:** Start a timer on mount, replace it when its interval prop changes, and clear it on unmount. Migrate the complete resource lifecycle into an Effect with correct dependencies. Compare class `setState`'s shallow merge with a Hook setter replacing its state value.

This is a behaviour comparison, not a rule that one lifecycle method always equals one Effect. Function components do not contain a `componentDidMount` method.

**Independent task:** Migrate a second class example with a browser subscription.

**Checkpoint:** Both implementations clean up correctly; students explain when mount, update and unmount occur. Reference: [React class components and migration](https://react.dev/reference/react/Component).

### Module 18 — Error boundaries and recovery

**Build:** Isolate a deliberately failing dashboard widget behind a helpful fallback.

**Teach through failure:** An error boundary handles supported rendering/lifecycle errors in its descendant tree. It is different from an API request's `try/catch`. Show a small class-based boundary with `getDerivedStateFromError` and `componentDidCatch`, building directly on Module 17.

**Practical sequence:** Trigger a render error. Keep navigation and unaffected panels working. Add a reset/retry path and diagnostic logging suitable for development.

**Independent task:** Place boundaries at useful feature boundaries and explain the choice.

**Debug:** Trigger an error in an event handler or asynchronous request and handle it at its actual source; the boundary does not automatically catch everything.

**Checkpoint:** A broken widget does not erase the entire usable application, and request failures still show their own meaningful error states.

### Module 19 — Dynamic imports, lazy loading and Suspense

**Build:** A Reports screen whose component code loads when it is needed.

**Teach through the network panel:** Dynamic `import()` returns a Promise for a module namespace. `React.lazy` consumes a loader whose resolved module provides a default component; `Suspense` displays the loading fallback.

**Practical sequence:** Split the reports component, lazy-load it, wrap the relevant region in Suspense and observe the separate request. Use an error boundary for a failed component load. Revisit the difference between default and named exports in this concrete case.

**Independent task:** Add another infrequently opened panel using the same pattern.

**Debug:** Point the lazy loader at a named-only module and repair the export or adapt the resolved value.

**Checkpoint:** Initial navigation works before reports load; the fallback and failure state are usable. An arbitrary `fetch` inside `useEffect` is not automatically managed by Suspense. Reference: [Suspense](https://react.dev/reference/react/Suspense).

### Module 20 — Performance and responsive updates

**Build:** Search and sort a deliberately larger sample dataset.

**Teach through measurement:** Use the React profiler and browser performance tools to find a problem before changing code. Introduce `useMemo`, `useCallback` and `memo` when their particular benefit is visible. They are optimisations, not correctness mechanisms.

**Practical sequence:** Measure the baseline. Fix unnecessary duplicated work and poor state placement. Evaluate memoisation. Demonstrate `useDeferredValue` or a transition for an expensive non-urgent view update while keeping typing responsive. A deferred render is not network-request debouncing.

**Independent task:** Record before/after behaviour and explain which change made a measurable difference.

**Debug:** Remove a necessary dependency from a memoised calculation, observe stale output, then repair it.

**Checkpoint:** Correct results and responsive input remain intact. If React Compiler is configured, explain how it changes the need for manual memoisation; do not assume it is active in every project. Reference: [useMemo](https://react.dev/reference/react/useMemo).

### Module 21 — API mutations, server state and session interfaces

**Build:** Connect student CRUD to an instructor-provided persistent API, then add a session-aware interface.

**Teach through requests:** Distinguish local UI state from records owned by a backend. The server validates changes and authorises access. A hidden button or frontend route guard only changes the interface; it does not protect backend data.

**Practical sequence:** Replace the local-only data source through a service layer. Implement create/update/delete, pending states, server validation messages, retry and duplicate-submit prevention. Reconcile successful responses with the screen. Then consume provided sign-in, current-session and sign-out endpoints.

**Independent task:** Handle session expiry during a save without falsely reporting success or discarding the user's draft.

**Debug:** Use a failed write, a validation response, an unauthorised response and a refresh after a successful write.

**Checkpoint:** Refresh retrieves durable changes, and failures preserve honest UI state. Backend implementation is supplied for this React exercise. A fake write API must be labelled as a simulation; it is not evidence of persistence. Keep secrets out of frontend source and exposed Vite variables.

### Module 22 — Behaviour tests and accessibility

**Build:** A small test suite covering the important student workflows.

**Teach through verification:** Use Vitest with React Testing Library, or an equivalent tested setup, to check behaviour a user can observe. Query by role and accessible name. Mock network responses deliberately, including failure cases.

**Practical sequence:** Test registration, invalid score rejection, editing, deletion, combined filtering and API retry. Test a storage failure or missing student route where it protects a meaningful requirement. Run a keyboard check for the complete add/edit/delete flow.

**Independent task:** Write a regression test for a bug encountered earlier, then repair the bug.

**Debug:** Remove an accessible label and notice the effect on both keyboard/screen-reader usability and the test query.

**Checkpoint:** Tests fail for an intentional behaviour regression and pass after the repair; avoid tests that only confirm internal variable names. Reference: [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/).

### Module 23 — Structure, production build and release

**Build:** A reviewable repository with a reproducible build and a documented release process.

**Teach through preparation:** Separate page composition, feature components, Hooks, pure helpers and service code once the application is large enough to benefit. Revisit imports after moves. Avoid creating folders with no current purpose.

**Practical sequence:** Remove unused files; run lint, relevant tests and the production build; inspect the built app locally. Check environment configuration, asset paths, responsive layout and direct route reloads. Add installation, run, build and feature instructions to the README. Verify from a fresh dependency installation using the lockfile.

**Independent task:** Have another student run the project from its README and report missing steps.

**Debug:** Test a production deep link and an asset path that differs from development.

**Checkpoint:** The production build runs with no unresolved import errors, console errors or exposed secrets. Explain that `VITE_` variables are visible to the client, and localStorage persists only in that browser/origin. Reference: [Vite environment variables](https://vite.dev/guide/env-and-mode).

### Module 24 — Independent capstone demonstration

**Build:** Complete the React Student Course Management Dashboard and add one independently implemented extension.

**Required demonstration:** Register; reject invalid input; edit and cancel; delete; combine filters; display correct grades and summary cards; navigate the three main pages; open a student by URL; handle an unknown ID; recover from API failure; refresh saved changes; complete key actions with the keyboard.

**Independent extension choices:** Attendance tracking, course capacity with a clear limit, a printable student report, or pagination that preserves the current filters. Choose one and state its acceptance criteria before implementation.

**Explain while demonstrating:** One default/named import pair, one non-trivial relative path, the owner of student state, an immutable update, a calculated value, an Effect and its cleanup, a custom Hook, and a handled failure.

**Checkpoint:** Students can make a small unseen requirement change and repair one injected bug without a complete solution. An additional small expense tracker or inventory app can assess transfer to a different domain if the instructor wants a stronger independence check.

## 8. Practical readiness gates

| Gate | Required evidence |
|---|---|
| Before React setup | Correct imports/exports and path repairs; reusable functions and immutable collection updates |
| Before Effects and API work | Components, props, state, forms and local CRUD work without manual DOM rendering |
| Before shared state architecture | Students can identify the owner of each value and distinguish stored from derived data |
| Before advanced work | Cleanup, loading/error handling, refresh persistence and routing behave correctly |
| Before course completion | The capstone, failure cases, behaviour tests, independent extension and production build pass |

Use **Ready / Needs support** with a short evidence note rather than inventing a grading scale. A student needing support repeats a smaller exercise targeting the missed skill; the instructor controls pacing.

### Reference acceptance data

| Action | Expected outcome |
|---|---|
| Add Ada: 78; Chidi: 49 | A/Pass; D/Fail; total 2; passed 1; failed 1; average 63.5 |
| Search for Ada | One visible row; overall totals unchanged |
| Edit Chidi to 50 | C/Pass; passed 2; failed 0; average 64 |
| Cancel a pending edit | Stored and displayed record unchanged |
| Delete Ada | Chidi remains at 50; average 50 |
| Add score 0 | Accepted as F/Fail |
| Submit an empty score or 101 | Rejected without adding a record |
| Remove every record | Empty-state guidance; no divide-by-zero output; average shown as an em dash |
| Refresh after accepted changes | Records reload from the chosen persistent source |
| Load API users offline, then retry online | Clear error followed by recovery; registered students remain intact |

## 9. Advanced extensions after the main sequence

These are additional practical modules, not prerequisites for learning components or Hooks.

| Extension | Build | Learning outcome |
|---|---|---|
| TypeScript with React | Convert the Student model, component props, events and API results | Catch invalid data contracts and distinguish optional values |
| React Actions and optimistic UI | Refactor one asynchronous form with the appropriate Action pattern, `useActionState`, `useFormStatus` and `useOptimistic` | Pending feedback, result state and recovery after optimistic failure |
| Server-state tooling | Replace repeated request/cache logic with one chosen data-fetching approach | Query identity, caching, invalidation and refetching without confusing server data with form state |
| External state library | Solve a demonstrated shared-state requirement using one selected library | Compare its value against the reducer/context solution already understood |
| Framework and server rendering | Move a small proven React feature into a chosen framework | Understand routing conventions, client/server boundaries and rendering strategies |

For Actions, verify APIs against the version installed for the resource pack. Place `useFormStatus` in the appropriate descendant of the form it observes. Browser Action functions do not require pretending that server execution exists; server functions require a supporting framework/backend arrangement. References: [useActionState](https://react.dev/reference/react/useActionState), [useFormStatus](https://react.dev/reference/react-dom/hooks/useFormStatus), [useOptimistic](https://react.dev/reference/react/useOptimistic).

## 10. Preparation brief for the next stage

Prepare the teaching resources against this module order. For each module, the later resource pack should contain:

1. A one-page instructor run sheet: target feature, starting state, short explanation and checkpoint.
2. A student walkthrough with explicit file locations and small, numbered edits.
3. Tested starter and completed project checkpoints, with the same dependency lockfile.
4. An independent task and a deliberate-bug exercise with instructor answers.
5. A visual reference showing the intended interface and relevant file relationships.
6. A video storyboard and narration only after the code and checkpoints have been verified.

Prepare Modules 01 and 02 first because misunderstandings about imports and paths will otherwise recur throughout React. Then prepare the React foundation modules in order. Use fictional student records throughout examples and demonstrations.

**Scope of this document:** The course structure, instructional approach, opening explanations, projects and completion criteria are prepared. Starter repositories, student solution guides, slides, voiceovers and videos are subsequent deliverables.
