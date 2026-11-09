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
| --- | --- |
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
