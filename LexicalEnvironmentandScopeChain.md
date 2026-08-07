# PHASE 2.2 — LEXICAL ENVIRONMENT & SCOPE CHAIN

Now we enter the MOST IMPORTANT foundation behind:

* closures
* variable access
* nested functions
* callback behavior
* React hooks
* async functions
* module patterns

This phase explains:

> HOW JavaScript knows where variables exist.

---

# FIRST BIG IDEA

JavaScript is:

# LEXICALLY SCOPED

This single sentence is extremely important.

---

# What Does Lexical Mean?

Lexical means:

```text id="2yyhyq"
Based on physical code structure
```

OR

```text id="xkqqku"
Where function is WRITTEN
```

NOT where function is called.

This difference is huge.

---

# SIMPLE EXAMPLE

```js id="skw2oz"
const a = 10;

function test() {
  console.log(a);
}

test();
```

Output:

```text id="qqrbr4"
10
```

Question:

> How did function access `a`?

`a` is outside function.

Answer:

Because of:

# Lexical Environment + Scope Chain

---

# WHAT IS A LEXICAL ENVIRONMENT?

A lexical environment is basically:

```text id="0d7c9x"
Local memory + reference to parent environment
```

Every execution context gets one.

---

# THINK OF IT LIKE THIS

Each function has:

```text id="fh5r7n"
1. Its own variables
2. Knowledge of parent scope
```

This parent link is EVERYTHING.

---

# INTERNAL STRUCTURE

Each lexical environment contains:

---

# 1. Environment Record

Stores local variables/functions.

Example:

```js id="vfyl8h"
let x = 10;
```

Stored here.

---

# 2. Reference to Outer Environment

Link to parent scope.

This creates:

# Scope Chain

---

# LET’S GO STEP BY STEP

---

# EXAMPLE 1

```js id="q8mkkz"
const a = 10;

function outer() {
  console.log(a);
}

outer();
```

---

# STEP 1 — GLOBAL CONTEXT

Global lexical environment created.

Memory:

```text id="1vl0cd"
a → 10
outer → function
```

---

# STEP 2 — outer() called

New function execution context created.

Its lexical environment:

```text id="4g0g6m"
Local Memory:
(empty)

Outer Reference:
Global Environment
```

IMPORTANT:

Function remembers parent environment.

---

# STEP 3 — console.log(a)

JS searches variable.

---

# SEARCH PROCESS

First local memory:

```text id="uofnfe"
Does outer() contain a?
```

NO.

---

# THEN

JS follows outer reference:

```text id="1t30ig"
Go to parent environment
```

Global scope contains:

```text id="wzhygi"
a → 10
```

Found.

Prints:

```text id="iicgcb"
10
```

---

# THIS SEARCHING MECHANISM IS:

# Scope Chain

---

# SCOPE CHAIN

Definition:

```text id="m3hkeb"
JavaScript searches variables upward through parent lexical environments
```

until variable found.

---

# IMPORTANT RULE

Search direction:

```text id="l4y9sh"
Inside → Outside
```

NEVER:

```text id="ywzd0o"
Outside → Inside
```

Parent cannot access child variables.

---

# EXAMPLE

```js id="l9r5es"
function outer() {
  const x = 10;

  function inner() {
    console.log(x);
  }

  inner();
}

outer();
```

Output:

```text id="6rjlwm"
10
```

---

# WHY?

Because inner function’s outer reference points to outer function environment.

---

# VISUAL

```text id="1byrhm"
inner()
   ↓ outer reference
outer()
   ↓ outer reference
Global()
```

This linked chain:

```text id="k2aq2z"
Scope Chain
```

---

# VARIABLE LOOKUP PROCESS

When:

```js id="rqfwzr"
console.log(x);
```

runs inside inner():

JS searches:

---

# STEP 1

Inside inner local memory.

Found?

NO.

---

# STEP 2

Move to outer() environment.

Found?

YES.

Stop searching.

---

# HUGE INSIGHT

Functions carry hidden reference to parent scope.

This is the foundation of closures.

---

# NESTED SCOPES

---

# Example

```js id="ukes1g"
const a = 1;

function one() {
  const b = 2;

  function two() {
    const c = 3;

    console.log(a, b, c);
  }

  two();
}

one();
```

Output:

```text id="svu5xw"
1 2 3
```

---

# SEARCH FLOW

Inside `two()`:

---

# c

Found locally.

---

# b

Not local.

Search parent (`one`).

Found.

---

# a

Not in `two`
Not in `one`

Go global.

Found.

---

# THIS IS SCOPE CHAIN TRAVERSAL

---

# IMPORTANT INTERVIEW QUESTION

---

# QUESTION

```js id="t8ltvp"
function outer() {
  let x = 10;

  function inner() {
    console.log(x);
  }

  return inner;
}

const fn = outer();

fn();
```

Output?

```text id="0e9jj0"
10
```

Many beginners expect error.

But function remembers lexical environment.

This becomes:

# Closure

(next phase deeply).

---

# SHADOWING

Important concept.

---

# Example

```js id="97pcrq"
const x = 1;

function test() {
  const x = 2;

  console.log(x);
}

test();
```

Output:

```text id="r2o7nv"
2
```

---

# WHY?

Local variable shadows parent variable.

Search stops immediately once variable found.

---

# VISUAL

```text id="8g8vzt"
Global:
x → 1

test():
x → 2
```

Inside test():

JS finds local `x` first.

Never checks parent.

---

# ILLEGAL SHADOWING

Advanced concept.

---

# Example

```js id="7hmowq"
let x = 10;

{
  var x = 20;
}
```

Error.

Because:

```text id="wf1wba"
var crosses block boundary
```

Would conflict with existing lexical binding.

---

# GLOBAL SCOPE

Variables outside functions belong to global lexical environment.

Browser:

```text id="mbh53l"
window object
```

Node.js:

```text id="0q2nro"
global object
```

---

# IMPORTANT DIFFERENCE

---

# var

Creates property on global object.

```js id="r8xqgb"
var a = 10;

console.log(window.a);
```

Works in browser.

---

# let / const

Do NOT attach to global object.

---

# WHY LEXICAL SCOPE MATTERS

Because JS determines scope:

```text id="a87pq4"
WHEN FUNCTION IS CREATED
```

NOT when executed.

Critical concept.

---

# EXAMPLE

```js id="1j39w6"
const x = 1;

function outer() {
  const x = 2;

  function inner() {
    console.log(x);
  }

  return inner;
}

const fn = outer();

fn();
```

Output:

```text id="jlwm9j"
2
```

WHY?

Because inner function lexically belongs to outer() scope.

Even after outer finished.

---

# THIS LEADS DIRECTLY TO CLOSURES

Functions remember surrounding environment.

One of most powerful JS features.

---

# DYNAMIC SCOPE VS LEXICAL SCOPE

Very advanced interview topic.

JavaScript uses:

```text id="84dqgq"
Lexical Scope
```

NOT dynamic scope.

---

# WHAT WOULD DYNAMIC SCOPE MEAN?

Variable lookup based on:

```text id="xf1a5q"
Who called function
```

JS does NOT work this way.

It uses:

```text id="06wq9u"
Where function was defined
```

---

# IMPORTANT INTERVIEW QUESTION

---

# QUESTION

```js id="3rvgfg"
let x = 10;

function a() {
  console.log(x);
}

function b() {
  let x = 20;

  a();
}

b();
```

Output?

```text id="rg2e7y"
10
```

NOT 20.

---

# WHY?

Because `a()` was lexically defined in global scope.

Its parent environment is global.

NOT `b()`.

---

# THIS IS MASSIVE CONCEPTUALLY

Function scope depends on:

```text id="abxv0j"
definition location
```

NOT caller location.

---

# MEMORY VIEW

---

# GLOBAL ENVIRONMENT

```text id="11gkvr"
x → 10
a → function
b → function
```

---

# b() ENVIRONMENT

```text id="qewl1q"
x → 20
```

---

# a() ENVIRONMENT

Outer reference:

```text id="r72ylq"
Global Environment
```

NOT b().

Thus finds:

```text id="5ll3i9"
x → 10
```

---

# SCOPE TYPES

---

# 1. Global Scope

Accessible everywhere.

---

# 2. Function Scope

Accessible only inside function.

---

# 3. Block Scope

Inside `{}` for let/const.

---

# EXAMPLE

```js id="8dvcv5"
{
  let a = 10;
}

console.log(a);
```

Error.

Because block-scoped.

---

# BUT

```js id="s1wq8z"
{
  var a = 10;
}

console.log(a);
```

Works.

var ignores block scope.

---

# HUGE INSIGHT

Every function carries hidden metadata:

```text id="j2y9jg"
[[Environment]]
```

pointing to lexical parent.

This hidden link powers:

* closures
* scope chain
* callbacks
* async memory retention

Everything.

---

# MOST IMPORTANT TAKEAWAY

When JS searches variable:

It does NOT search randomly.

It follows:

```text id="89j9ta"
Current Scope
   ↓
Parent Scope
   ↓
Parent's Parent
   ↓
Global Scope
   ↓
null
```

This chain is the backbone of JavaScript execution.
