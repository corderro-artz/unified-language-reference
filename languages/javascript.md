# JavaScript

`multi-paradigm` · `dynamic, weak, duck-typed` · `interpreted / JIT-compiled` · `1995`

## Contents

- **Identity**
  - [Overview](#overview)
  - [Language Type](#language-type)
  - [Paradigms](#paradigms)
  - [Mental Model](#mental-model)
- **Foundations**
  - [Lexical & Syntax](#lexical--syntax)
  - [Variables & Bindings](#variables--bindings)
  - [Type System](#type-system)
  - [Data Structures](#data-structures)
  - [Operators & Expressions](#operators--expressions)
- **Logic**
  - [Control Flow](#control-flow)
  - [Functions](#functions)
  - [Error Handling](#error-handling)
- **Abstraction**
  - [Object Model](#object-model)
  - [Functional Constructs](#functional-constructs)
  - [Modules & Namespaces](#modules--namespaces)
  - [Metaprogramming & Reflection](#metaprogramming--reflection)
- **Runtime**
  - [Memory Management](#memory-management)
  - [Concurrency & Parallelism](#concurrency--parallelism)
- **Practice**
  - [Standard Library — Start Here](#standard-library--start-here)
  - [Tooling & Ecosystem](#tooling--ecosystem)
  - [Conventions & Style](#conventions--style)
  - [Idioms & Gotchas](#idioms--gotchas)
- **Reference**
  - [Versioning & Editions](#versioning--editions)
  - [Resources](#resources)

---

## Identity

### Overview

JavaScript is a general-purpose, dynamically typed language and the
language of the web, standardized as ECMAScript (ECMA-262). It runs
in browsers and on server-side and edge runtimes — Node.js, Deno,
and Bun — each pairing the language with its own host APIs. It is
known for first-class functions, prototype-based objects, a
single-threaded non-blocking event loop, and ubiquity across the
entire stack. I/O, timers, `console`, the DOM, and the filesystem
are host APIs, not part of the language itself.

### Language Type

| Axis      | This language                                              |
|-----------|------------------------------------------------------------|
| Execution | interpreted / JIT-compiled by host engines (V8, SpiderMonkey, JavaScriptCore) |
| Domain    | general-purpose                                            |
| Typing    | dynamic, weak, duck-typed; structural                     |
| Memory    | managed; tracing garbage collector                        |

### Paradigms

- **Imperative** — a program is a sequence of statements that change state. Here: the default statement-by-statement flow.
- **Object-Oriented** — bundles state and behavior into types; objects interact through methods. Here: prototype-based objects; `class` is sugar over prototypes.
- **Functional** — computation as the evaluation of functions; values over mutation, functions as first-class data. Here: closures, arrow functions, `map`/`filter`/`reduce`.
- **Event-driven** — control flow directed by events and their handlers. Here: callbacks, Promises, and event listeners on the event loop.
- **Concurrent** — work structured as independently progressing tasks. Here: `async`/`await` over Promises, plus Web Workers.
- **Metaprogramming** — programs that read, generate, or transform code. Here: `Proxy`, `Reflect`, symbols, and property descriptors.

### Mental Model

Everything is either a primitive or an object; objects link to other
objects through prototypes, and `class` is syntactic sugar over that
prototypal chain. Execution is a single-threaded, non-blocking event
loop, so asynchronous code is pervasive and you never block the
thread. Values coerce freely across types, so prefer `===` over `==`
to avoid surprises. `this` is bound by the call site, not where a
function is defined — except arrow functions, which capture `this`
lexically.

## Foundations

### Lexical & Syntax

- **Comments** — `//` to end of line, `/* ... */` block; `/** ... */`
  is the JSDoc documentation form.
- **Statements** — terminated by `;`, which is optional via automatic
  semicolon insertion (ASI); `{ }` delimits blocks.
- **Keywords** — lowercase (`function`, `const`, `class`, `await`).
- **Entry point** — top-level module or script code runs directly;
  there is no `main` function.
- **File structure** — `.js`, `.mjs` (ESM), or `.cjs` (CommonJS);
  ESM `import` statements sit at the top of the file.
- **Strict mode** — `"use strict"` opts a script into stricter
  semantics; modules are always strict.

```js
console.log("Hello, World!");
```

<details>
<summary>Examples — one per item</summary>

```js
let a = 1;   // Comments — line
/* block */  // and /* ... */
/** JSDoc */

let b = 2;   // Statements — ; optional (ASI)
{ const c = 3; }  // { } blocks

const f = () => {};  // Keywords — lowercase

console.log("hi");   // Entry point — top-level

// File structure — .mjs is ESM
import { x } from "./m.js";

"use strict";  // Strict mode
```

</details>

### Variables & Bindings

- **`const`** — block-scoped binding that cannot be reassigned (the
  value may still be mutated).
- **`let`** — block-scoped binding that can be reassigned.
- **`var`** — function-scoped, hoisted binding; legacy, avoid.
- **Hoisting & TDZ** — `let`/`const` hoist but stay in a temporal
  dead zone until declared; `var` initializes to `undefined`.
- **Destructuring** — unpack from objects and arrays:
  `const { a } = o`, `const [x] = arr`.
- **Globals** — an unqualified assignment creates an implicit global
  in sloppy mode; in strict mode it throws `ReferenceError`.

```js
const PI = 3.14;
let count = 0;
const { a } = obj;
```

<details>
<summary>Examples — one per item</summary>

```js
const PI = 3.14;     // const — no reassign

let count = 0;       // let — reassignable
count += 1;

var legacy = 1;      // var — function-scoped

// Hoisting & TDZ
// console.log(t);   // ReferenceError
let t = 1;

const { a } = obj;   // Destructuring
const [x] = arr;

// Globals — strict mode throws
g = 1;               // ReferenceError in strict
```

</details>

### Type System

| Syntax      | Type             | Size      | Range                | Default     | Literal      |
|-------------|------------------|-----------|----------------------|-------------|--------------|
| `number`    | number           | 64-bit    | IEEE-754 double      | `0`         | `42`         |
| `string`    | string           | n/a       | UTF-16 code units    | `''`        | `"hi"`       |
| `boolean`   | boolean          | n/a       | `true` / `false`     | `false`     | `true`       |
| `bigint`    | big integer      | arbitrary | unbounded integer    | `0n`        | `10n`        |
| `symbol`    | unique symbol    | n/a       | unique tokens        | `—`         | `Symbol()`   |
| `undefined` | unset / absent   | n/a       | the value `undefined`| `undefined` | `undefined`  |
| `null`      | intentional empty| n/a       | the value `null`     | `—`         | `null`       |

> [!NOTE]
> JavaScript has no zero-initialization — an unset binding is
> `undefined`. The Default column is each type's zero-arg
> constructor/coercion result (`Number()`→`0`, `String()`→`''`,
> `Boolean()`→`false`, `BigInt(0)`→`0n`). Everything non-primitive
> is an `object`, including functions and arrays. `typeof null ===
> "object"` is a historical bug. `typeof` returns the 7 primitive
> tags plus `"object"` and `"function"`. The `object` type lives in
> [Data Structures](#data-structures).

<details>
<summary>typeof tags</summary>

| Value          | `typeof` result |
|----------------|-----------------|
| `42`           | `"number"`      |
| `"hi"`         | `"string"`      |
| `true`         | `"boolean"`     |
| `10n`          | `"bigint"`      |
| `Symbol()`     | `"symbol"`      |
| `undefined`    | `"undefined"`   |
| `null`         | `"object"`      |
| `{}` / `[]`    | `"object"`      |
| `() => {}`     | `"function"`    |
</details>

- **Inference** — there is no static inference; `typeof` reads the
  value's runtime tag.
- **Conversion** — implicit coercion happens in `==`, `+`, and
  template literals; convert explicitly with `Number()`, `String()`,
  `Boolean()`.
- **Nullability** — `undefined` means unset, `null` means
  intentionally empty; reach for `??` and `?.` to handle absence.
- **Truthiness** — the falsy set is `false`, `0`, `-0`, `0n`, `''`,
  `null`, `undefined`, and `NaN`; everything else is truthy.

```js
let x = 42;          // runtime tag "number"
let n = Number("3"); // explicit conversion
let v = a ?? b;      // null/undefined fallback
```

<details>
<summary>Examples — one per item</summary>

```js
let x = 42;          // Inference — runtime tag

let n = Number("3"); // Conversion — explicit
let s = String(42);

let v = a ?? "def";  // Nullability — ?? / ?.
let w = obj?.prop;

// Truthiness — falsy: 0 '' null
if (value) {
  use(value);
}
```

</details>

### Data Structures

| Structure    | Syntax              | Ordered | Mutable | Use                   |
|--------------|---------------------|---------|---------|-----------------------|
| `Array`      | `[1, 2, 3]`         | yes     | yes     | ordered sequence      |
| `Object`     | `{ a: 1 }`          | yes     | yes     | keyed record          |
| `Map`        | `new Map()`         | yes     | yes     | any-key lookup        |
| `Set`        | `new Set()`         | yes     | yes     | unique membership     |
| `WeakMap`    | `new WeakMap()`     | no      | yes     | keyed by object       |
| `WeakSet`    | `new WeakSet()`     | no      | yes     | object membership     |
| `TypedArray` | `new Int32Array(n)` | yes     | yes     | binary numeric buffer |

> [!NOTE]
> `Object` key order is specified: integer-like keys come first in
> ascending order, then string keys in insertion order, then
> symbols. Use a `Map` when keys are non-strings or insertion order
> must hold for every key.

### Operators & Expressions

| Category   | Operator | Name              | Example       | Note                   |
|------------|----------|-------------------|---------------|------------------------|
| Arithmetic | `+`      | add               | `a + b`       | also concatenates      |
| Arithmetic | `-`      | subtract          | `a - b`       |                        |
| Arithmetic | `*`      | multiply          | `a * b`       |                        |
| Arithmetic | `/`      | divide            | `a / b`       |                        |
| Arithmetic | `%`      | remainder         | `a % b`       |                        |
| Arithmetic | `**`     | exponent          | `a ** b`      | right-associative      |
| Comparison | `===`    | strict equal      | `a === b`     | no coercion            |
| Comparison | `!==`    | strict not-equal  | `a !== b`     | no coercion            |
| Comparison | `==`     | loose equal       | `a == b`      | coerces operands       |
| Comparison | `!=`     | loose not-equal   | `a != b`      | coerces operands       |
| Comparison | `<`      | less-than         | `a < b`       |                        |
| Comparison | `>`      | greater-than      | `a > b`       |                        |
| Logical    | `&&`     | and               | `a && b`      | short-circuits         |
| Logical    | `\|\|`   | or                | `a \|\| b`    | short-circuits         |
| Logical    | `!`      | not               | `!a`          |                        |
| Bitwise    | `&`      | and               | `a & b`       |                        |
| Bitwise    | `\|`     | or                | `a \| b`      |                        |
| Bitwise    | `^`      | xor               | `a ^ b`       |                        |
| Bitwise    | `~`      | not               | `~a`          |                        |
| Bitwise    | `<<`     | left shift        | `a << 2`      |                        |
| Bitwise    | `>>`     | signed shift      | `a >> 2`      |                        |
| Bitwise    | `>>>`    | unsigned shift    | `a >>> 2`     |                        |
| Assignment | `=`      | assign            | `a = b`       |                        |
| Assignment | `+=`     | add-assign        | `a += b`      |                        |
| Assignment | `??=`    | nullish-assign    | `a ??= b`     | ES2021                 |
| Assignment | `\|\|=`  | or-assign         | `a \|\|= b`   | ES2021                 |
| Assignment | `&&=`    | and-assign        | `a &&= b`     | ES2021                 |
| Special    | `?.`     | optional chain    | `a?.b`        | ES2020                 |
| Special    | `??`     | nullish coalesce  | `a ?? b`      | ES2020                 |
| Special    | `typeof` | type tag          | `typeof a`    | returns a string       |
| Special    | `instanceof` | prototype test | `a instanceof C` | walks the chain     |
| Special    | `in`     | property test     | `"k" in o`    |                        |
| Special    | `...`    | spread / rest     | `[...a]`      |                        |
| Special    | `=>`     | arrow function    | `x => x + 1`  | lexical `this`         |

> [!NOTE]
> `==` coerces its operands (`0 == ""` is `true`, `null ==
> undefined` is `true`) — prefer `===`, which compares without
> coercion. `??` only falls back on `null`/`undefined`, unlike
> `\|\|`, which falls back on any falsy value. `**` is
> right-associative (`2 ** 3 ** 2 === 512`).

## Logic

### Control Flow

- **Conditionals** — `if` / `else if` / `else`, plus the ternary
  `cond ? a : b`.
- **`switch`** — matches by `===`; cases fall through unless you
  `break`.
- **Loops** — `for`, `while`, and `do…while`.
- **`for…of`** — iterates values of any iterable (arrays, strings,
  `Map`, `Set`).
- **`for…in`** — iterates enumerable keys, including inherited ones.
- **Jumps** — `break`, `continue`, labels, and `return`.

```js
for (const x of [1, 2, 3]) {
  console.log(x);
}
```

<details>
<summary>Examples — one per item</summary>

```js
if (n > 0) {         // Conditionals
  pos();
} else {
  neg();
}
let s = n > 0 ? "+" : "-";

switch (cmd) {       // switch — fall-through
  case "go":
    run();
    break;
  default:
    stop();
}

while (n > 0) {      // Loops
  n -= 1;
}

for (const x of xs) {  // for…of — values
  use(x);
}

for (const k in obj) { // for…in — keys
  use(k);
}

// Jumps — break / continue / return
outer: for (;;) {
  break outer;
}
```

</details>

### Functions

- **Declarations** — `function f() {}` is hoisted and named.
- **Expressions / arrow** — `const f = () => {}`; arrows capture
  `this` lexically and have no own `arguments`.
- **Parameters** — defaults (`a = 1`), rest (`...args`), and
  destructured parameters.
- **First-class & closures** — functions are values; a nested
  function closes over its enclosing scope.
- **Higher-order** — pass and return functions: `map`, `filter`,
  `reduce`.
- **Generators** — `function*` with `yield` produces lazy iterators.
- **`async` functions** — return Promises; see
  [Concurrency](#concurrency--parallelism).
- **`this` binding** — set by the call site; `bind`, `call`, and
  `apply` control it explicitly.

```js
const add = (a, b = 0) => a + b;
function* seq() { yield 1; }
```

<details>
<summary>Examples — one per item</summary>

```js
function sq(x) {     // Declarations — hoisted
  return x * x;
}

const f = () => 1;   // Expressions / arrow

function p(a, b = 0, ...xs) {  // Parameters
  return a + b;
}

function outer() {   // First-class & closures
  let n = 1;
  return () => n;
}

const ev = xs.filter(  // Higher-order
  x => x % 2 === 0
);

function* gen() {    // Generators
  yield 1;
  yield 2;
}

async function load() {  // async functions
  return await fetchIt();
}

// this binding — bind / call / apply
const g = fn.bind(obj);
```

</details>

### Error Handling

- **`try` / `catch` / `finally`** — `catch` may omit its binding
  (`catch {}`); `finally` always runs.
- **`throw`** — throws any value, idiomatically an `Error`.
- **Error types** — `TypeError`, `RangeError`, `SyntaxError`, and
  custom subclasses; pass `{ cause }` to chain (ES2022).
- **Async errors** — a rejected Promise surfaces in `try`/`catch`
  around `await`, or via `.catch()`.
- **Propagation** — an uncaught throw unwinds the call stack to the
  nearest `catch`.
- **No checked exceptions** — there are no result types and no
  declared throws.

```js
try {
  risky();
} catch (e) {
  log(e);
} finally {
  cleanup();
}
```

<details>
<summary>Examples — one per item</summary>

```js
try {                // try / catch / finally
  risky();
} catch {            // binding optional
  recover();
} finally {
  cleanup();
}

throw new Error("bad");  // throw

// Error types — cause (ES2022)
throw new TypeError("x", {
  cause: err,
});

async function a() { // Async errors
  try {
    await work();
  } catch (e) {
    log(e);
  }
}

// Propagation — unwinds the stack
function p() {
  throw new Error("up");
}

// No checked exceptions — none declared
```

</details>

## Abstraction

### Object Model

- **Object literals** — `{ key: value }` creates an object directly.
- **Classes** — `class C { constructor() {} }`; sugar over
  prototypes.
- **Prototypes** — every object has a `[[Prototype]]`;
  `Object.create` and the prototype chain drive lookup.
- **Fields & methods** — instance and `static` members; private
  fields use `#field` (ES2022).
- **Encapsulation** — `#private` fields and closures; there are no
  `public`/`protected` keywords.
- **Inheritance** — `extends` a base class and call `super(...)`.
- **Polymorphism** — method calls dispatch dynamically through the
  prototype chain.
- **Getters / setters** — `get` and `set` define computed accessors.

```js
class Point {
  #id = 0;
  constructor(x) { this.x = x; }
}
```

<details>
<summary>Examples — one per item</summary>

```js
const o = { a: 1 };  // Object literals

class Animal {       // Classes
  constructor(n) { this.n = n; }
}

const p = Object.create(proto);  // Prototypes

class Box {          // Fields & methods
  static kind = "box";
  #count = 0;        // private (ES2022)
}

// Encapsulation — #private only
class Safe { #secret = 1; }

class Dog extends Animal {  // Inheritance
  constructor(n) { super(n); }
}

// Polymorphism — dynamic dispatch
animals.forEach(a => a.speak());

class Temp {         // Getters / setters
  get c() { return this._c; }
  set c(v) { this._c = v; }
}
```

</details>

> [!NOTE]
> Classes are syntactic sugar over prototypal inheritance. `class`,
> `extends`, and `super` compile down to prototype links — the
> prototype chain is the real model.

### Functional Constructs

- **First-class functions & closures** — functions are values that
  capture their enclosing scope.
- **Immutability** — `const` binds but does not deep-freeze;
  `Object.freeze` is shallow; spread copies one level.
- **Array combinators** — `map`, `filter`, `reduce`, and `flatMap`
  transform sequences declaratively.
- **Currying / partial** — build with closures or `Function.bind`.
- **Pure functions** — a convention, not enforced by the language.
- **No native ADTs** — model sum types with objects plus a
  discriminant field; there is no pattern matching.

```js
const total = xs.reduce((a, b) => a + b, 0);
```

<details>
<summary>Examples — one per item</summary>

```js
// First-class functions & closures
const adder = n => x => x + n;

const frozen = Object.freeze({  // Immutability
  a: 1,
});

const out = xs           // Array combinators
  .filter(x => x > 0)
  .map(x => x * 2);

const add = (a, b) => a + b;  // Currying / partial
const inc = add.bind(null, 1);

function pure(x) {       // Pure functions
  return x + 1;
}

// No native ADTs — tag with a field
const shape = { kind: "circle", r: 2 };
```

</details>

### Modules & Namespaces

- **ES Modules** — static `import` / `export`; the language
  standard, used in `.mjs` or with `"type": "module"`.
- **Named vs default** — `export { x }` and `export default y`;
  import each by name or as the default.
- **Dynamic import** — `import(specifier)` returns a Promise,
  loading a module on demand.
- **CommonJS** — Node's legacy system: `require` and
  `module.exports`.
- **Top-level `await`** — allowed at module scope (ES2022).
- **Packaging** — npm with `package.json`; registries include npm
  and jsr.

```js
import { sum } from "./math.js";
export default function main() {}
```

<details>
<summary>Examples — one per item</summary>

```js
import { sum } from "./m.js";  // ES Modules
export const tau = 6.28;

export default fn;       // Named vs default
import fn from "./fn.js";

const mod = await import(  // Dynamic import
  "./lazy.js"
);

const fs = require("fs");  // CommonJS (Node)
module.exports = api;

// Top-level await (ES2022, modules)
const data = await load();

// Packaging — npm + package.json
import dayjs from "dayjs";
```

</details>

> [!NOTE]
> ES Modules are the language standard; CommonJS (`require` /
> `module.exports`) is a Node.js runtime system, not part of
> ECMAScript.

### Metaprogramming & Reflection

- **`typeof` / `instanceof` / `in`** — runtime type and property
  introspection.
- **`Reflect`** — a functional API mirroring object internal
  operations (`Reflect.get`, `Reflect.has`).
- **`Proxy`** — wraps an object to intercept operations through
  trap handlers.
- **Symbols** — unique keys; well-known symbols like
  `Symbol.iterator` and `Symbol.asyncIterator` hook into protocols.
- **Property descriptors** — `Object.defineProperty` configures
  enumerability, writability, getters, and setters.
- **Decorators** — Stage 3, not yet standardized; flag before use.
- **`eval` / `Function`** — execute code built from strings at
  runtime.

```js
const p = new Proxy(target, {
  get: (o, k) => o[k],
});
```

<details>
<summary>Examples — one per item</summary>

```js
// typeof / instanceof / in
if (typeof x === "number") {}

Reflect.has(obj, "key");  // Reflect

const p = new Proxy(t, {  // Proxy
  get: (o, k) => o[k],
});

// Symbols & well-known symbols
const it = obj[Symbol.iterator]();

Object.defineProperty(    // Property descriptors
  o, "x", { value: 1 }
);

// Decorators — Stage 3, not standard
// @logged class C {}

eval("1 + 1");           // eval / Function
```

</details>

> [!WARNING]
> `eval` and `new Function` execute arbitrary code — never pass them
> untrusted input. They also defeat optimization; avoid them outside
> tooling.

## Runtime

### Memory Management

- **Tracing GC** — a mark-and-sweep collector reclaims unreachable
  objects; collection is engine-specific and non-deterministic.
- **References, not pointers** — variables hold references; there is
  no pointer arithmetic.
- **Closures retain** — variables captured by a closure stay alive
  as long as the closure does.
- **No manual free** — you cannot deallocate; drop references and
  let the collector reclaim.
- **`WeakMap` / `WeakSet`** — hold keys weakly so they do not
  prevent collection.
- **`WeakRef` / `FinalizationRegistry`** — weak references and
  finalizers (ES2021); advanced and discouraged.

<details>
<summary>Examples — one per item</summary>

```js
// Tracing GC — unreachable is reclaimed
let o = { big: data };
o = null;            // now collectable

let a = [1];         // References, not pointers
let b = a;           // both name one array

function counter() { // Closures retain
  let n = 0;
  return () => ++n;
}

// No manual free — drop the reference
let ref = make();
ref = null;

const wm = new WeakMap();  // WeakMap / WeakSet
wm.set(key, value);

// WeakRef / FinalizationRegistry (ES2021)
const wr = new WeakRef(obj);
```

</details>

> [!NOTE]
> Garbage collection is engine-specific (V8 uses a generational
> collector) and is not specified by the language. Timing of
> collection and finalizer callbacks is never guaranteed.

### Concurrency & Parallelism

- **Single-threaded event loop** — code runs to completion on one
  thread; the loop is a host concept, not part of the language.
- **Promises** — represent eventual values; chain with `then`,
  `catch`, and `finally`.
- **`async` / `await`** — write Promise-based code in a sequential
  style.
- **Microtasks vs macrotasks** — Promise jobs (microtasks) run
  before timer callbacks (macrotasks).
- **Web Workers / `worker_threads`** — true parallelism via
  isolated threads that communicate by message passing.
- **`SharedArrayBuffer` & `Atomics`** — shared memory with atomic
  operations across workers.
- **No shared mutable state** — the main thread shares nothing
  directly; communicate by messages.

```js
async function main() {
  const data = await fetchIt();
}
```

<details>
<summary>Examples — one per item</summary>

```js
// Single-threaded event loop
queueMicrotask(() => work());

const p = Promise.resolve(1)  // Promises
  .then(v => v + 1);

async function load() {  // async / await
  return await fetchIt();
}

// Microtasks vs macrotasks
Promise.resolve().then(a);  // before
setTimeout(b, 0);           // after

// Web Workers / worker_threads
const w = new Worker("./w.js");
w.postMessage(task);

// SharedArrayBuffer & Atomics
const sab = new SharedArrayBuffer(8);
Atomics.add(new Int32Array(sab), 0, 1);

// No shared mutable state — message pass
w.onmessage = e => use(e.data);
```

</details>

> [!WARNING]
> Blocking the event loop — a long synchronous loop or a sync call —
> freezes everything, including rendering and other tasks. Keep work
> async or offload it to a worker. Timers like `setTimeout` are host
> APIs, not part of the language.

## Practice

### Standard Library — Start Here

The language ships a set of built-in global objects available with
no import (`Object`, `Array`, `JSON`, `Math`, …). I/O, the DOM, the
filesystem, and `console` are host APIs; ESM `import` is for host
and npm modules, not these globals.

| Module    | Purpose                          | Import     |
|-----------|----------------------------------|------------|
| `Object`  | object utilities and reflection  | `(global)` |
| `Array`   | ordered sequences and combinators| `(global)` |
| `String`  | text values and methods          | `(global)` |
| `Number`  | numeric parsing and checks       | `(global)` |
| `Math`    | mathematical functions           | `(global)` |
| `JSON`    | JSON parse and stringify         | `(global)` |
| `Map`     | any-key keyed collection         | `(global)` |
| `Set`     | unique-value collection          | `(global)` |
| `Promise` | asynchronous result values       | `(global)` |
| `Date`    | dates and times                  | `(global)` |
| `RegExp`  | regular expressions              | `(global)` |
| `Symbol`  | unique property keys             | `(global)` |
| `Intl`    | internationalization formatting  | `(global)` |
| `Reflect` | functional object operations     | `(global)` |
| `Proxy`   | intercept object operations      | `(global)` |

### Tooling & Ecosystem

| Tool       | Role                       | Invoke           |
|------------|----------------------------|------------------|
| Node.js    | server-side runtime        | `node`           |
| npm        | package manager            | `npm install`    |
| Deno       | secure runtime             | `deno run`       |
| Bun        | fast runtime + toolkit     | `bun run`        |
| ESLint     | linter                     | `eslint`         |
| Prettier   | code formatter             | `prettier`       |
| Vitest     | test runner                | `vitest`         |
| TypeScript | typed superset compiler    | `tsc`            |
| Vite       | dev server + bundler       | `vite`           |
| REPL       | interactive shell          | `node`           |
| debugger   | inspector protocol         | `node --inspect` |

### Conventions & Style

| Identifier            | Convention      | Example           |
|-----------------------|-----------------|-------------------|
| variables / functions | camelCase       | `userId`          |
| classes               | PascalCase      | `HttpClient`      |
| constants             | UPPER_SNAKE     | `MAX_LEN`         |
| files                 | kebab-case.js   | `user-service.js` |
| private fields        | `#field`        | `#count`          |

- **Formatting** — two-space indentation; semicolon use is
  style-dependent and enforced by Prettier.
- **Project layout** — application code under `src/`, tests under
  `tests/`, with `package.json` at the root.
- **Doc comments** — JSDoc `/** ... */` documents functions and
  generates API docs.

Typical project layout — source under `src/`, tests in a parallel
`tests/`:

```text
myproject/
├─ src/                 application code
│  ├─ index.js          entry module
│  └─ user-service.js
├─ tests/               test suite
│  └─ user-service.test.js
├─ package.json         manifest + scripts
├─ package-lock.json
└─ .gitignore
```

Popular style guide:
<https://github.com/airbnb/javascript>

### Idioms & Gotchas

- **Prefer `===`** — strict equality avoids surprising coercion.
- **Array combinators** — favor `map`/`filter`/`reduce` over manual
  index loops.
- **Template literals** — interpolate with `` `${name}=${value}` ``.
- **Optional chaining** — guard deep access with `a?.b?.c`.

> [!WARNING]
> `==` coerces operands before comparing — `0 == ""` and `null ==
> undefined` are both `true`. Use `===`.

> [!WARNING]
> `this` is bound by the call site: a method pulled off its object
> loses `this`. Use an arrow function or `bind`.

> [!WARNING]
> `var` is function-scoped and hoisted; `let`/`const` sit in a
> temporal dead zone until declared. Prefer `const` and `let`.

> [!WARNING]
> `NaN !== NaN` — test with `Number.isNaN`. Floats are inexact:
> `0.1 + 0.2 !== 0.3`.

> [!WARNING]
> A long synchronous task blocks the single-threaded event loop and
> freezes the whole program — keep work async or offload to a worker.

## Reference

### Versioning & Editions

ECMAScript is published annually by Ecma International, with a new
edition each June named by its year. **ES2025 (ECMA-262, 16th
edition) is the current published standard**, ratified in June 2025.
ES2026 (the 17th edition) is finished — its proposals have reached
Stage 4 — but is pending Ecma's formal ratification as of June 2026.
Engines ship Stage 4 features continuously, often before the yearly
snapshot is published, so feature support tracks the engine more than
the edition; version tags still matter when targeting older runtimes.

> [!NOTE]
> Recent features by edition: optional chaining `?.` and nullish
> coalescing `??` (ES2020); logical assignment `??=`/`\|\|=`/`&&=`
> (ES2021); top-level `await`, private `#` fields, error `cause`,
> and `.at()` (ES2022); `findLast` and `toSorted` (ES2023);
> `Object.groupBy` (ES2024); `Promise.try`, new `Set` methods,
> `RegExp.escape`, iterator helpers, and `Float16Array` (ES2025).
> Decorators, pattern matching, and records & tuples are still
> proposals (not yet shipped).

### Resources

- ECMAScript specification (ECMA-262): <https://tc39.es/ecma262/>
- MDN JavaScript: <https://developer.mozilla.org/en-US/docs/Web/JavaScript>
- Node.js docs: <https://nodejs.org/en/docs>
- TC39 proposals: <https://github.com/tc39/proposals>
- Package registry (npm): <https://www.npmjs.com/>
- Eloquent JavaScript: <https://eloquentjavascript.net/>
