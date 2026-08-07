# PHASE 2.10 — ADVANCED INTERVIEW CONCEPTS & REAL-WORLD PATTERNS

Now we enter the phase where interviewers usually differentiate:

```text id="x2m8q4"
"Knows JavaScript"
vs
"Thinks like an engineer"
```

These concepts combine everything you learned earlier:

* closures
* event loop
* prototypes
* memory
* functions
* async behavior

This phase is highly practical and heavily asked in interviews.

---

# TOPICS IN THIS PHASE

We’ll cover:

1. Debounce
2. Throttle
3. Deep Clone
4. Event Delegation
5. Polyfills
6. Custom bind/call/apply
7. Promise Internals
8. Memoization
9. Currying Advanced
10. Composition
11. Immutability Patterns
12. Memory Leak Patterns
13. Performance Optimization
14. React Rendering Insights

This phase connects theory to real engineering.

---

# 1. DEBOUNCE

Very important frontend/backend optimization concept.

---

# PROBLEM

Imagine search input:

```js id="q7m1v9"
input.addEventListener("input", searchAPI);
```

If user types:

```text id="v3m8q1"
h
he
hel
hell
hello
```

API called 5 times.

Wasteful.

---

# DEBOUNCE IDEA

Wait until user stops typing.

Then execute ONCE.

---

# REAL-WORLD USES

* search bars
* resize handlers
* auto-save
* validation
* expensive API calls

---

# IMPLEMENTATION

```js id="x5m2q8"
function debounce(fn, delay) {
  let timer;

  return function(...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

---

# HUGE INSIGHT

Debounce works because of:

# Closures

`timer` survives between calls.

---

# FLOW

Each new call:

```text id="n1m7q4"
clear old timer
↓
start fresh timer
```

Only last execution survives.

---

# THROTTLE

Similar but different.

---

# THROTTLE IDEA

Execute at fixed intervals.

---

# USE CASES

* scroll events
* mouse movement
* game controls
* resize performance

---

# EXAMPLE

Allow execution only once every second.

---

# IMPLEMENTATION

```js id="v8m4q2"
function throttle(fn, delay) {
  let waiting = false;

  return function(...args) {
    if (waiting) return;

    fn.apply(this, args);

    waiting = true;

    setTimeout(() => {
      waiting = false;
    }, delay);
  };
}
```

---

# DIFFERENCE

---

# Debounce

```text id="m3q9v1"
Wait until activity stops
```

---

# Throttle

```text id="x7m1q5"
Limit execution frequency
```

---

# HUGE INTERVIEW QUESTION

When to use debounce vs throttle?

Critical frontend performance question.

---

# 2. DEEP CLONE

Very important.

---

# PROBLEM

Spread operator:

```js id="n6m2q8"
const copy = { ...obj };
```

creates:

# Shallow copy

Nested references shared.

---

# EXAMPLE

```js id="q2m8v7"
const a = {
  user: {
    name: "A"
  }
};

const b = { ...a };

b.user.name = "B";

console.log(a.user.name);
```

Output:

```text id="x4m9q1"
B
```

---

# WHY?

Nested object reference shared.

---

# TRUE DEEP CLONE

---

# MODERN WAY

```js id="v1m7q4"
structuredClone(obj);
```

---

# OLDER WAY

```js id="n8m2q5"
JSON.parse(JSON.stringify(obj))
```

BUT dangerous.

---

# PROBLEMS

Loses:

* functions
* undefined
* Date
* Map
* Set
* Symbols

Important interview insight.

---

# 3. EVENT DELEGATION

EXTREMELY IMPORTANT.

---

# PROBLEM

Imagine 1000 buttons.

---

# BAD

```js id="q5m1v9"
buttons.forEach(btn => {
  btn.addEventListener("click", handler);
});
```

1000 listeners.

More memory.

---

# BETTER

Attach ONE listener to parent.

---

# Example

```js id="x8m4q1"
parent.addEventListener("click", (e) => {
  if (e.target.matches("button")) {
    console.log("Button clicked");
  }
});
```

---

# WHY WORKS?

Because of:

# Event Bubbling

---

# EVENT FLOW

```text id="v2m8q4"
button
 ↓
parent
 ↓
body
 ↓
document
```

Event bubbles upward.

---

# HUGE BENEFITS

* less memory
* dynamic elements supported
* better performance

Very common interview topic.

---

# 4. POLYFILLS

Very important.

---

# WHAT IS POLYFILL?

Custom implementation of modern feature.

Used when browser lacks support.

---

# Example — Custom map()

```js id="m7q1v9"
Array.prototype.myMap = function(cb) {
  const result = [];

  for (let i = 0; i < this.length; i++) {
    result.push(cb(this[i], i, this));
  }

  return result;
};
```

---

# HUGE INTERVIEW INSIGHT

Polyfills test:

* prototypes
* this
* arrays
* iteration
* callbacks

Everything together.

---

# 5. CUSTOM BIND IMPLEMENTATION

Very common interview question.

---

# INTERNAL IDEA

bind creates new function with fixed `this`.

---

# SIMPLE IMPLEMENTATION

```js id="q4m9v2"
Function.prototype.myBind = function(context) {
  const fn = this;

  return function(...args) {
    return fn.apply(context, args);
  };
};
```

---

# HUGE INSIGHT

Uses:

* closures
* apply
* function references

Combined concepts.

---

# 6. PROMISE INTERNALS

Critical advanced topic.

---

# PROMISE STATES

```text id="x1m7q5"
pending
fulfilled
rejected
```

Only one final state possible.

---

# IMPORTANT

Promise callbacks:

```js id="v8m2q1"
.then()
.catch()
```

go to:

# Microtask Queue

---

# WHY PROMISES BETTER THAN CALLBACKS?

* chaining
* error handling
* readability
* inversion of control improvement

---

# Promise.all()

---

# Example

```js id="n5m9q4"
Promise.all([p1, p2, p3]);
```

Runs concurrently.

Fails immediately if any reject.

---

# Promise.allSettled()

Waits for ALL results.

---

# Promise.race()

First settled promise wins.

---

# 7. MEMOIZATION

Now deeper.

---

# IDEA

Cache expensive function results.

---

# Example

```js id="q2m8v7"
function memo(fn) {
  const cache = {};

  return function(n) {
    if (cache[n]) {
      return cache[n];
    }

    const result = fn(n);

    cache[n] = result;

    return result;
  };
}
```

---

# HUGE INSIGHT

Again powered by:

# Closures

Cache survives across calls.

---

# 8. FUNCTION COMPOSITION

Functional programming concept.

---

# IDEA

Combine small functions into pipelines.

---

# Example

```js id="x7m1q4"
const add = x => x + 1;
const double = x => x * 2;

const result = double(add(5));
```

---

# WHY IMPORTANT?

React and FP-heavy libraries use this heavily.

---

# 9. IMMUTABILITY PATTERNS

Important in React/Redux.

---

# BAD

```js id="v3m9q1"
state.user.name = "A";
```

Mutation.

---

# GOOD

```js id="n6m2q8"
{
  ...state,
  user: {
    ...state.user,
    name: "A"
  }
}
```

New references created.

---

# WHY IMPORTANT?

React detects changes using reference identity.

---

# 10. MEMORY LEAK PATTERNS

Critical engineering topic.

---

# COMMON LEAKS

---

# Event listeners not removed

```js id="q5m1v9"
button.addEventListener(...)
```

---

# Timers not cleared

```js id="x8m4q1"
setInterval(...)
```

---

# Large closures retained

---

# Detached DOM references

Huge frontend issue.

---

# 11. REACT RENDERING INSIGHTS

VERY IMPORTANT.

---

# WHY COMPONENT RE-RENDERS

Usually because:

```text id="v2m8q4"
State reference changed
```

NOT deep equality.

---

# WHY IMMUTABILITY IMPORTANT

React uses cheap comparisons:

```js id="m7q1v9"
oldState === newState
```

---

# useCallback/useMemo

Optimization hooks.

Prevent unnecessary:

* function recreation
* recalculations

---

# STALE CLOSURES

Massive React interview topic.

Old render values trapped inside closures.

---

# 12. BIG-O & PERFORMANCE

Senior interviews often ask:

* time complexity
* space complexity
* optimization tradeoffs

---

# Example

```js id="q4m9v2"
arr.includes(x)
```

O(n)

---

# Better

```js id="x1m7q5"
set.has(x)
```

O(1) average.

---

# 13. CUSTOM EVENT LOOP QUESTIONS

Very common.

---

# QUESTION

```js id="v8m2q1"
console.log(1);

setTimeout(() => console.log(2));

Promise.resolve().then(() => console.log(3));

console.log(4);
```

Answer:

```text id="n5m9q4"
1
4
3
2
```

Must deeply understand queues.

---

# 14. ADVANCED EQUALITY

---

# QUESTION

```js id="q2m8v7"
NaN === NaN
```

FALSE.

Weird JS behavior.

---

# WHY?

IEEE floating-point rules.

Use:

```js id="x7m1q4"
Object.is(NaN, NaN)
```

---

# ANOTHER TRICK

```js id="v3m9q1"
0 === -0
```

TRUE.

BUT:

```js id="n6m2q8"
Object.is(0, -0)
```

FALSE.

---

# SENIOR-LEVEL UNDERSTANDING

At this point you should now connect:

* closures
* event loop
* prototypes
* async behavior
* memory
* immutability
* rendering
* optimization

into one unified mental model.

That’s real JavaScript mastery.

---

# YOU HAVE NOW COMPLETED

# CORE ADVANCED JAVASCRIPT FOUNDATIONS

You now deeply understand:

* memory model
* execution contexts
* closures
* this
* prototypes
* classes
* async/event loop
* engine optimization
* advanced patterns

This is already beyond most mid-level developers.

---

# NEXT MAJOR LEVELS AVAILABLE

Now you can deeply learn:

---

# REACT INTERNALS

* Fiber
* reconciliation
* hooks internals
* batching
* rendering lifecycle

---

# NODE.JS INTERNALS

* libuv
* streams
* worker threads
* clustering

---

# BROWSER INTERNALS

* rendering pipeline
* repaint/reflow
* DOM internals

---

# SYSTEM DESIGN

* scaling
* caching
* databases
* queues
* architecture

These are the next major engineering levels.
