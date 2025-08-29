## JavaScript Core Concepts – Interview Question Bank

This is a **rock-solid question bank** covering fundamental and advanced JS topics with definitions, examples, pitfalls, tricky interview questions, and real-world scenarios.

---

### 1. Execution Context

**Definition:**
The environment in which JavaScript code is executed. It consists of:

* **Global Execution Context (GEC)**
* **Function Execution Context (FEC)**
* **Eval Execution Context** (rare)

**Phases:**

1. Creation phase (memory allocation, hoisting)
2. Execution phase (code runs)

**Example:**

```js
var a = 10;
function test() {
  var b = 20;
  console.log(a, b);
}
test();
```

**Pitfalls:**

* Variables declared with `var` are hoisted but initialized as `undefined`.
* Functions are hoisted completely.

**Tricky Question:**

```js
console.log(a);
var a = 5;
```

Output → `undefined` (due to hoisting).

---

### 2. Hoisting

**Definition:**
JavaScript moves variable/function declarations to the top of their scope.

**Example:**

```js
foo();
function foo() {
  console.log("Hoisted!");
}
```

✅ Works due to hoisting.

**Pitfall Example:**

```js
console.log(x);
let x = 5; // ReferenceError (Temporal Dead Zone)
```

**Tricky Question:**

```js
var a = 1;
(function() {
  console.log(a);
  var a = 2;
})();
```

Output → `undefined` (inner `a` shadows outer `a`).

---

### 3. Closures

**Definition:**
A closure is a function that "remembers" variables from its outer scope even after that scope is closed.

**Example:**

```js
function outer() {
  let counter = 0;
  return function inner() {
    counter++;
    return counter;
  };
}
const inc = outer();
console.log(inc()); // 1
console.log(inc()); // 2
```

**Pitfall:**

* Closures can cause **memory leaks** if not handled properly.

**Tricky Question:**

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
```

Output → `3, 3, 3` (due to `var`). Fix with `let`.

---

### 4. Scope & Lexical Environment

**Definition:**
Scope defines accessibility of variables. Lexical environment is created during execution and contains variable bindings.

**Types:**

* Global Scope
* Function Scope
* Block Scope (`let`, `const`)

**Example:**

```js
let x = 10;
function foo() {
  let y = 20;
  console.log(x + y);
}
foo(); // 30
```

**Pitfall:**

* Using `var` → no block scope.

**Tricky Question:**

```js
if (true) {
  var a = 10;
}
console.log(a); // 10 (not block-scoped)
```

---

### 5. `this` Keyword

**Definition:**
`this` refers to the execution context.

**Examples:**

```js
console.log(this); // Window (browser)

const obj = {
  val: 42,
  fn: function() {
    console.log(this.val);
  }
};
obj.fn(); // 42
```

**Arrow Function Example:**

```js
const obj = {
  val: 100,
  fn: () => console.log(this.val)
};
obj.fn(); // undefined (arrow functions do not bind `this`)
```

**Pitfall:**

* Losing context when passing methods as callbacks.

**Tricky Question:**

```js
const obj = {
  val: 10,
  fn() {
    return this.val;
  }
};
const fnRef = obj.fn;
console.log(fnRef()); // undefined (not bound to obj)
```

---

### 6. Prototypal Inheritance

**Definition:**
Objects inherit from other objects via prototype chain.

**Example:**

```js
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  console.log("Hello, I am " + this.name);
};

const p = new Person("Amin");
p.greet(); // Hello, I am Amin
```

**Pitfall:**

* Overriding prototype methods incorrectly can break inheritance.

**Tricky Question:**

```js
const arr = [];
console.log(arr.__proto__ === Array.prototype); // true
console.log(Array.prototype.__proto__ === Object.prototype); // true
```

---

### 7. Event Loop & Concurrency Model

**Definition:**
JavaScript uses an event loop with a call stack, microtask queue (Promises, MutationObserver), and task queue (setTimeout, setInterval).

**Example:**

```js
console.log("start");
setTimeout(() => console.log("timeout"), 0);
Promise.resolve().then(() => console.log("promise"));
console.log("end");
```

Output → `start → end → promise → timeout`

**Pitfall:**

* Misunderstanding order of async execution.

**Tricky Question:**

```js
setTimeout(() => console.log(1));
Promise.resolve().then(() => console.log(2));
console.log(3);
```

Output → `3, 2, 1`

---

### 8. Promises & Async/Await

**Definition:**

* Promise represents a future value.
* Async/Await makes async code look synchronous.

**Example:**

```js
const fetchData = () => new Promise(res => setTimeout(() => res("done"), 1000));

async function run() {
  const result = await fetchData();
  console.log(result);
}
run();
```

**Pitfall:**

* Forgetting `try...catch` around `await`.

**Tricky Question:**

```js
async function test() {
  return 1;
}
test().then(console.log); // 1 (wrapped in Promise)
```

---

### 9. Debouncing & Throttling

**Definition:**

* **Debounce:** Executes after delay when user stops triggering.
* **Throttle:** Executes at fixed intervals.

**Debounce Example:**

```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

**Throttle Example:**

```js
function throttle(fn, limit) {
  let lastCall = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      fn(...args);
    }
  };
}
```

**Pitfall:**

* Overusing debounce on input fields can delay responsiveness.

---

### 10. ES6+ Features (Must-Knows)

* `let` & `const`
* Template literals
* Destructuring
* Default parameters
* Spread & Rest operators
* Modules (import/export)
* Async/Await
* Optional chaining (`?.`)

**Example:**

```js
const user = { name: "Amin", age: 25 };
const { name } = user;
console.log(`Hello ${name}`);
```

---

### 11. Pitfalls & Traps

**Type Coercion Example:**

```js
console.log([] + []); // ""
console.log([] + {}); // "[object Object]"
console.log({} + []); // 0 (weird!)
```

**== vs === Example:**

```js
console.log(0 == false); // true
console.log(0 === false); // false
```

**Strict vs Non-Strict Mode:**

```js
(function(){
  'use strict';
  x = 10; // ReferenceError (not declared)
})();
```

---

✅ This document is part of our **Question Bank** for JS interviews.
Next step → We can expand each topic with **real-world system design integrations (React, Node.js, etc.)**.
