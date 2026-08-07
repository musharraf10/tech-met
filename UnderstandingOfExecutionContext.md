# PHASE 2.1 — EXECUTION CONTEXT (Deep Dive)

This is one of the MOST IMPORTANT concepts in JavaScript.

If you deeply understand execution context:

* hoisting becomes easy
* closures become easy
* scope becomes easy
* `this` becomes easier
* async behavior becomes clearer

Because almost everything in JS depends on:

```text id="tfj0ci"
How execution contexts are created and managed
```

---

# FIRST UNDERSTAND THIS

JavaScript does NOT directly execute code line by line.

Before execution, engine prepares an environment.

That environment is called:

# EXECUTION CONTEXT

---

# SIMPLE DEFINITION

Execution Context =

```text id="a8p4pk"
Environment where JavaScript code executes
```

It contains everything needed to run code:

* variables
* functions
* scope info
* this binding
* memory references

---

# REAL LIFE ANALOGY

Imagine a chef cooking.

Before cooking starts:

* ingredients arranged
* utensils prepared
* stove setup
* recipe checked

Only then cooking begins.

Execution context is this preparation environment.

---

# TYPES OF EXECUTION CONTEXT

There are mainly 3.

---

# 1. GLOBAL EXECUTION CONTEXT (GEC)

Created when JS program starts.

Only ONE global context exists.

---

# 2. FUNCTION EXECUTION CONTEXT (FEC)

Created every time function called.

Each function call gets NEW context.

---

# 3. EVAL CONTEXT

Rarely used.

Ignore for now.

---

# MOST IMPORTANT THING

Every execution context has TWO PHASES.

---

# PHASE 1 — MEMORY CREATION PHASE

Also called:

```text id="qug2qm"
Creation Phase
```

Engine scans code BEFORE execution.

Allocates memory for:

* variables
* functions

---

# PHASE 2 — EXECUTION PHASE

Now code runs line by line.

Assignments happen.

Functions execute.

Calculations occur.

---

# LET'S GO VERY DEEP

---

# EXAMPLE 1

```js id="g8mjlwm"
console.log(a);

var a = 10;
```

Output:

```text id="8qctut"
undefined
```

Most beginners confused.

Let’s see internally.

---

# STEP 1 — GLOBAL EXECUTION CONTEXT CREATED

Engine creates GEC.

---

# MEMORY CREATION PHASE STARTS

Engine scans code.

Finds:

```js id="0lvj9u"
var a
```

Memory allocated.

---

# INTERNAL MEMORY STATE

```text id="msdrs8"
Global Memory
--------------
a → undefined
```

IMPORTANT:

`a` already exists BEFORE execution.

This is hoisting.

---

# NOW EXECUTION PHASE STARTS

Code runs line by line.

---

# LINE 1

```js id="3d2t76"
console.log(a);
```

JS checks memory.

Finds:

```text id="rllzmu"
a → undefined
```

So prints:

```text id="4k6o7t"
undefined
```

---

# LINE 2

```js id="8gkqu8"
a = 10;
```

Memory updates:

```text id="e3m7n5"
a → 10
```

---

# FINAL MEMORY

```text id="s17cbw"
Global Memory
--------------
a → 10
```

---

# IMPORTANT INSIGHT

Hoisting does NOT move code physically.

This is WRONG understanding:

```text id="hfgf8f"
"JS moves declarations upward"
```

NO.

Actually:

```text id="vxz6ij"
Memory allocation happens before execution
```

Very important distinction.

---

# FUNCTION HOISTING

Now deeper.

---

# EXAMPLE

```js id="ggn7s6"
sayHello();

function sayHello() {
  console.log("Hello");
}
```

Output:

```text id="hll4sm"
Hello
```

WHY?

---

# MEMORY PHASE

Function declarations fully stored in memory.

---

# MEMORY STATE

```text id="1mcrk3"
sayHello → entire function
```

Unlike variables:

```text id="5cg65k"
var → undefined
```

Functions store COMPLETE function body.

---

# EXECUTION PHASE

When:

```js id="0eyvwx"
sayHello();
```

runs, function already exists.

---

# VISUAL

```text id="k2t3zd"
MEMORY PHASE
----------------
a → undefined
sayHello → function definition
```

---

# FUNCTION EXECUTION CONTEXT

Now VERY IMPORTANT.

---

# EXAMPLE

```js id="l8x3v9"
function one() {
  var a = 10;

  console.log(a);
}

one();
```

---

# STEP 1

Global execution context created.

Memory phase:

```text id="pibaxw"
one → function
```

---

# STEP 2

Execution phase reaches:

```js id="n6r18u"
one();
```

Now NEW execution context created for function.

---

# CALL STACK

```text id="8g5vxe"
one()
Global()
```

Function context placed on top.

---

# FUNCTION MEMORY PHASE

Inside function:

```js id="4m9nmf"
var a
```

allocated:

```text id="ygt1dw"
a → undefined
```

---

# FUNCTION EXECUTION PHASE

Runs:

```js id="83odzh"
a = 10
```

Then:

```js id="t7j1ri"
console.log(a)
```

prints:

```text id="vqg7hh"
10
```

---

# FUNCTION FINISHES

Execution context removed from stack.

Stack:

```text id="6a57ca"
Global()
```

---

# HUGE INSIGHT

Every function call creates:

* new memory space
* new variables
* new execution context

This is why functions are isolated.

---

# EXAMPLE

```js id="h2mmyf"
function test() {
  var x = 1;
}

test();
test();
```

Each call gets separate `x`.

Different memory allocations.

---

# CALL STACK DEEP DIVE

Critical topic.

---

# Example

```js id="c2q1h0"
function one() {
  two();
}

function two() {
  three();
}

function three() {
  console.log("Hello");
}

one();
```

---

# STACK FLOW

---

# INITIAL

```text id="a0jj23"
Global()
```

---

# one() called

```text id="4y1q2l"
one()
Global()
```

---

# two() called

```text id="j9dujlwm"
two()
one()
Global()
```

---

# three() called

```text id="cltxy8"
three()
two()
one()
Global()
```

---

# three finishes

```text id="mz0gwy"
two()
one()
Global()
```

---

# two finishes

```text id="k9m9vh"
one()
Global()
```

---

# one finishes

```text id="8z1oz8"
Global()
```

---

# THIS IS LIFO

```text id="l4o4qj"
Last In First Out
```

Last pushed executes first.

---

# WHY JS IS SINGLE-THREADED

JavaScript has:

```text id="rq0v1w"
ONE call stack
```

One thing executes at a time.

Critical concept.

---

# NOW LET’S TALK ABOUT let AND const

This is where things become advanced.

---

# EXAMPLE

```js id="1teq3y"
console.log(a);

let a = 10;
```

Output:

```text id="jpmwb0"
ReferenceError
```

BUT WAIT…

Didn’t we say variables hoisted?

YES.

So why error?

---

# TEMPORAL DEAD ZONE (TDZ)

Most misunderstood concept.

---

# IMPORTANT

`let` and `const` ARE hoisted.

But differently.

---

# MEMORY PHASE

```text id="ef81vb"
a → uninitialized
```

NOT undefined.

---

# BEFORE INITIALIZATION

Variable exists internally.

BUT inaccessible.

This zone:

```text id="n8t0jo"
From start of scope
to variable initialization
```

is called:

# TEMPORAL DEAD ZONE

---

# VISUAL

```js id="7w75b6"
console.log(a);

let a = 10;
```

---

# MEMORY

```text id="blzlh4"
a → <uninitialized>
```

---

# EXECUTION LINE 1

Access attempt:

```js id="9t2rf7"
console.log(a)
```

Engine says:

```text id="a6fxy8"
"Variable exists but not initialized"
```

Throws:

```text id="q1tjsd"
ReferenceError
```

---

# ONLY AFTER THIS LINE

```js id="zwd7vv"
let a = 10;
```

does variable become usable.

---

# WHY TDZ EXISTS

To prevent accidental usage before initialization.

Makes code safer.

---

# DIFFERENCE BETWEEN var let const

| Feature                         | var       | let         | const       |
| ------------------------------- | --------- | ----------- | ----------- |
| Hoisted                         | Yes       | Yes         | Yes         |
| Initialized during memory phase | undefined | No          | No          |
| TDZ                             | No        | Yes         | Yes         |
| Re-declaration                  | Allowed   | Not allowed | Not allowed |
| Scope                           | Function  | Block       | Block       |

---

# BLOCK SCOPE

---

# Example

```js id="5n9ezz"
{
  let x = 10;
}

console.log(x);
```

Error.

Because:

```text id="lajdmy"
let is block scoped
```

Exists only inside `{}`.

---

# BUT var

```js id="fpj6qf"
{
  var x = 10;
}

console.log(x);
```

Works.

Because var ignores block scope.

Function scoped only.

---

# INTERVIEW QUESTION

---

# Q1

```js id="n4f9qb"
console.log(a);

var a = 10;
```

Why undefined not ReferenceError?

---

# Q2

```js id="jviy2n"
console.log(a);

let a = 10;
```

Why ReferenceError?

---

# Q3

```js id="i38ltx"
function test() {
  console.log(a);

  var a = 5;
}
```

Output?

```text id="jcc8w4"
undefined
```

Because local `a` hoisted inside function context.

---

# Q4

```js id="bktdn0"
var a = 1;

function x() {
  console.log(a);

  var a = 2;
}

x();
```

Output?

```text id="w4ik6w"
undefined
```

WHY?

Function’s local `a` shadows global `a`.

Local variable hoisted first.

---

# CRITICAL INSIGHT

Execution context is basically:

```text id="px9uj7"
A memory environment + execution environment
```

Every function creates:

* its own variables
* its own scope
* its own memory
* its own execution flow

This is the base for:

* closures
* scope chain
* async behavior
* this keyword
* prototype lookup

Everything ahead depends on this.
