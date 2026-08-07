# PHASE 2.7 — FUNCTION INTERNALS & ADVANCED FUNCTIONS

Now we enter one of the deepest parts of JavaScript.

Most developers use functions daily…

but very few understand:

* what functions actually are internally
* why functions are objects
* higher-order functions
* callbacks
* currying
* recursion internals
* first-class functions
* pure vs impure functions
* memoization
* functional programming concepts

This phase is HUGE for:

* interviews
* React
* backend architecture
* async JS
* performance optimization

---

# FIRST BIG IDEA

In JavaScript:

# Functions are Objects

This is EXTREMELY important.

---

# Example

```js id="x9m2q1"
function test() {}

test.x = 10;

console.log(test.x);
```

Output:

```text id="p7v4m8"
10
```

---

# WAIT…

How can function have properties?

Because functions are objects internally.

---

# FUNCTIONS ARE SPECIAL OBJECTS

Functions can:

* store properties
* be passed around
* returned
* assigned
* stored in arrays
* stored in objects

PLUS:

```text id="m3q7v1"
They are callable
```

Normal objects are not callable.

---

# INTERNAL FUNCTION STRUCTURE

Function internally contains:

```text id="q8m1v5"
1. Code
2. Hidden environment reference
3. Properties
4. Prototype
5. Callable behavior
```

Functions are very powerful objects.

---

# FIRST-CLASS FUNCTIONS

Critical interview term.

---

# DEFINITION

Functions treated like normal values.

Meaning they can be:

* assigned to variables
* passed as arguments
* returned from functions

---

# EXAMPLE 1 — ASSIGNMENT

```js id="v4q9m2"
function greet() {
  console.log("Hello");
}

const x = greet;

x();
```

Functions stored in variables.

---

# EXAMPLE 2 — PASS AS ARGUMENT

```js id="n7m1q8"
function run(fn) {
  fn();
}

run(greet);
```

---

# EXAMPLE 3 — RETURN FUNCTION

```js id="k2v8m5"
function outer() {
  return function inner() {
    console.log("Hello");
  };
}
```

This powers closures.

---

# HIGHER-ORDER FUNCTIONS

Very important concept.

---

# DEFINITION

A higher-order function:

* takes function as argument
  OR
* returns function

---

# EXAMPLE

```js id="x5m9q1"
function greet() {
  console.log("Hello");
}

function execute(fn) {
  fn();
}

execute(greet);
```

`execute` is higher-order function.

---

# WHY IMPORTANT?

Because modern JS heavily depends on them:

* map
* filter
* reduce
* React
* middleware
* event listeners
* promises

Everywhere.

---

# CALLBACK FUNCTIONS

A callback is:

```text id="q7m2v4"
Function passed into another function
to execute later
```

---

# Example

```js id="v1m8q5"
function fetchData(callback) {
  console.log("Fetching...");

  callback();
}

fetchData(() => {
  console.log("Done");
});
```

---

# WHY CALLED CALLBACK?

Because function gets:

```text id="m4q9v2"
"called back later"
```

---

# HUGE ASYNC INSIGHT

JavaScript async model originally built on callbacks.

---

# CALLBACK HELL

Old async JS problem.

---

# Example

```js id="x8m1q7"
login(user, () => {
  getProfile(() => {
    getPosts(() => {
      console.log("done");
    });
  });
});
```

Deep nesting.

Hard to maintain.

Led to:

* Promises
* async/await

---

# PURE FUNCTIONS

Important functional programming concept.

---

# DEFINITION

Pure function:

* same input → same output
* no side effects

---

# PURE

```js id="q2v7m4"
function add(a, b) {
  return a + b;
}
```

Predictable.

---

# IMPURE

```js id="n5m9q1"
let total = 0;

function add(v) {
  total += v;
}
```

Depends on external state.

Side effect exists.

---

# WHY PURE FUNCTIONS IMPORTANT?

Because they are:

* predictable
* testable
* cacheable
* safer

React prefers pure rendering logic.

---

# SIDE EFFECTS

Critical term.

A side effect means:

```text id="v8m2q5"
Function changes something outside itself
```

Examples:

* API calls
* DOM changes
* modifying globals
* database updates

---

# FUNCTION FACTORIES

Functions creating functions.

---

# Example

```js id="x1m7q3"
function multiplier(x) {
  return function(y) {
    return x * y;
  };
}

const double = multiplier(2);

console.log(double(5));
```

Output:

```text id="m9v4q1"
10
```

---

# WHY WORKS?

Closure.

Inner function remembers `x`.

---

# CURRYING

Very famous interview topic.

---

# NORMAL FUNCTION

```js id="q5m1v8"
function add(a, b) {
  return a + b;
}
```

---

# CURRIED VERSION

```js id="n2q7m4"
function add(a) {
  return function(b) {
    return a + b;
  };
}
```

Usage:

```js id="x8m5q1"
add(2)(3)
```

Output:

```text id="v1m9q7"
5
```

---

# WHY CURRYING USEFUL?

Allows:

* partial application
* reusable specialized functions
* functional composition

Huge in advanced JS libraries.

---

# RECURSION

Function calling itself.

---

# Example

```js id="m4q8v2"
function factorial(n) {
  if (n === 1) return 1;

  return n * factorial(n - 1);
}
```

---

# INTERNAL CALL STACK

```text id="q7m5v1"
factorial(3)
 ↓
factorial(2)
 ↓
factorial(1)
```

Then stack unwinds.

---

# WHY RECURSION DANGEROUS?

Too many calls:

```text id="x2m9q4"
Stack Overflow
```

because call stack grows.

---

# BASE CASE

Critical in recursion.

Stops infinite recursion.

---

# MEMOIZATION

Huge performance topic.

---

# IDEA

Cache expensive computation results.

---

# Example

```js id="n8m1q5"
function memoizedAdd() {
  const cache = {};

  return function(n) {
    if (cache[n]) {
      return cache[n];
    }

    console.log("Calculating");

    cache[n] = n + 10;

    return cache[n];
  };
}
```

---

# WHY IMPORTANT?

Avoids repeated expensive work.

Used heavily in:

* React
* algorithms
* backend optimization

---

# FUNCTION DECLARATION VS EXPRESSION

Very important.

---

# DECLARATION

```js id="q3m7v1"
function test() {}
```

Hoisted fully.

---

# EXPRESSION

```js id="v9m2q4"
const test = function() {};
```

Variable hoisted, function not callable before init.

---

# ARROW FUNCTIONS INTERNALS

Different from regular functions.

---

# ARROW FUNCTIONS DO NOT HAVE:

* own `this`
* own `arguments`
* constructor behavior
* prototype

---

# Example

```js id="x6m1q8"
const fn = () => {};
```

---

# IMPORTANT

```js id="m5q9v2"
fn.prototype
```

is:

```text id="n1m7q4"
undefined
```

Because arrows not constructible.

---

# REGULAR FUNCTIONS HAVE PROTOTYPE

```js id="q8m2v5"
function test() {}

console.log(test.prototype);
```

Exists.

---

# WHY?

Because regular functions can be used with:

```js id="x4m9q1"
new
```

Arrow functions cannot.

---

# IIFE (Immediately Invoked Function Expression)

Old important pattern.

---

# Example

```js id="v7m1q5"
(function() {
  console.log("Run immediately");
})();
```

Executes instantly.

Used before modules existed.

---

# FUNCTION BORROWING

Using another object's method.

---

# Example

```js id="m2q8v4"
const user1 = {
  name: "A",

  greet() {
    console.log(this.name);
  }
};

const user2 = {
  name: "B"
};

user1.greet.call(user2);
```

Output:

```text id="q5m1v9"
B
```

Using method with different `this`.

---

# ARGUMENTS OBJECT

Regular functions have special array-like object.

---

# Example

```js id="x9m4q2"
function test() {
  console.log(arguments);
}

test(1,2,3);
```

Arrow functions lack this.

---

# REST PARAMETERS

Modern replacement.

---

# Example

```js id="n6m2q8"
function test(...args) {
  console.log(args);
}
```

True array.

---

# FUNCTION NAME PROPERTY

---

# Example

```js id="q1m8v4"
function hello() {}

console.log(hello.name);
```

Output:

```text id="v4m7q1"
hello
```

Functions store metadata.

---

# LENGTH PROPERTY

Shows expected parameter count.

---

# Example

```js id="x2m5q9"
function add(a,b,c) {}

console.log(add.length);
```

Output:

```text id="n8m1q4"
3
```

---

# HUGE INTERVIEW QUESTION

---

# QUESTION

```js id="q7m2v8"
function test(a, b) {
  arguments[0] = 100;

  console.log(a);
}

test(1,2);
```

In non-strict mode:

```text id="m5q1v7"
100
```

Because arguments linked to parameters historically.

Weird JS behavior.

---

# FUNCTIONAL PROGRAMMING IN JS

Modern JS heavily influenced by FP concepts:

* immutability
* pure functions
* composition
* higher-order functions

React ecosystem uses many FP ideas.

---

# SENIOR-LEVEL UNDERSTANDING

You should now understand:

* functions are special objects
* closures depend on function environments
* callbacks power async behavior
* higher-order functions enable abstraction
* currying uses closures
* recursion uses call stack
* memoization improves performance
* arrow functions differ internally
* functions carry metadata and hidden behavior

---

# MOST IMPORTANT TAKEAWAYS

---

# Functions are first-class citizens

---

# Functions are objects + callable behavior

---

# Closures make advanced patterns possible

---

# Higher-order functions power modern JS

---

# Arrow functions are NOT normal functions internally

---

# Functional programming deeply influences modern JavaScript
