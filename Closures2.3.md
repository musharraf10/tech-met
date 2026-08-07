# PHASE 2.3 — CLOSURES (One of the Most Important Concepts in JavaScript)

Closures are where many developers start feeling:

```text id="b8qyy6"
"JavaScript is weird"
```

But once deeply understood, closures explain:

* React hooks
* event listeners
* callbacks
* async behavior
* timers
* module patterns
* private variables
* memoization
* currying
* debounce/throttle
* state persistence

Closures are EVERYWHERE in real-world JS.

---

# FIRST UNDERSTAND THIS

Functions in JavaScript are NOT just code.

They are:

```text id="s83m3x"
Code + hidden reference to lexical environment
```

That hidden environment memory is closure.

---

# SIMPLE DEFINITION

A closure is:

> A function bundled together with its surrounding lexical environment.

Simple words:

```text id="x4pl3d"
Function remembers variables from parent scope
even after parent function finished execution
```

THIS is the magic part.

---

# MOST IMPORTANT EXAMPLE

```js id="c54s1w"
function outer() {
  let count = 0;

  function inner() {
    count++;

    console.log(count);
  }

  return inner;
}

const fn = outer();

fn();
fn();
fn();
```

Output:

```text id="jlwmn7"
1
2
3
```

---

# HUGE QUESTION

`outer()` already finished.

Normally local variables should disappear.

Then HOW does `count` still exist?

This is closure.

---

# LET'S GO STEP BY STEP INTERNALLY

---

# STEP 1 — GLOBAL EXECUTION CONTEXT

Memory:

```text id="yjlwmr"
outer → function
fn → undefined
```

---

# STEP 2 — outer() called

New execution context created.

Memory inside outer():

```text id="2f03pb"
count → 0
inner → function
```

---

# IMPORTANT PART

When `inner` function created:

JS secretly attaches:

```text id="j5z54u"
[[Environment]]
```

reference.

Pointing to outer’s lexical environment.

---

# VISUAL

```text id="f39bgk"
inner function
   ↓ hidden reference
outer lexical environment
   ↓
count → 0
```

---

# STEP 3 — return inner

`outer()` finishes.

Normally its memory should be destroyed.

BUT…

`inner` still references outer environment.

So GC cannot remove it.

---

# CRITICAL INSIGHT

As long as something references lexical environment:

```text id="jlwmt5"
Memory survives
```

---

# STEP 4 — fn()

Actually executes `inner()`.

Inside inner:

```js id="ztb41r"
count++;
```

JS searches variable.

---

# SEARCH

Local memory?

No `count`.

---

# Follow closure reference

Found in outer lexical environment:

```text id="w5g2pd"
count → 0
```

Increment:

```text id="i6v6m5"
count → 1
```

Prints:

```text id="nh70pb"
1
```

---

# SECOND CALL

Same lexical environment reused.

Now:

```text id="jlwmv9"
count → 1
```

Increment:

```text id="jlwmw7"
count → 2
```

Prints:

```text id="jlwmx8"
2
```

---

# HUGE UNDERSTANDING

Closure does NOT copy variables.

It keeps LIVE reference to environment.

Very important.

---

# VISUAL MEMORY MODEL

---

# STACK

```text id="jlwmz0"
fn → inner function
```

---

# HEAP

```text id="jlwmzw"
inner function
   ↓
[[Environment]]
   ↓
outer lexical environment
   ↓
count → 3
```

---

# THIS IS WHY CLOSURES ARE POWERFUL

Functions carry memory with them.

---

# REAL-LIFE ANALOGY

Imagine:

A person leaves office.

But carries office access card.

Even after leaving building:

* still can access files
* still remembers office location

Closure is similar.

Function leaves parent execution.

But still carries environment access.

---

# PRIVATE VARIABLES USING CLOSURES

Before private class fields existed, closures created privacy.

---

# Example

```js id="jlwmzz"
function createBankAccount() {
  let balance = 1000;

  return {
    deposit(amount) {
      balance += amount;
    },

    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount();

account.deposit(500);

console.log(account.getBalance());
```

Output:

```text id="jlwn12"
1500
```

---

# IMPORTANT

Can we directly access:

```js id="s8v7s9"
account.balance
```

NO.

Undefined.

---

# WHY?

Because `balance` lives only inside closure environment.

Not exposed publicly.

---

# THIS IS DATA ENCAPSULATION

Using closures.

Very important historically.

---

# CLOSURE IN EVENT LISTENERS

Huge real-world use.

---

# Example

```js id="0yxcr4"
function setup() {
  let count = 0;

  button.addEventListener("click", () => {
    count++;

    console.log(count);
  });
}
```

Even after `setup()` finishes:

Event listener still remembers `count`.

Closure.

---

# CLOSURE IN setTimeout

---

# Example

```js id="vvjlwm"
function test() {
  let msg = "Hello";

  setTimeout(() => {
    console.log(msg);
  }, 1000);
}

test();
```

After 1 second:

```text id="6fy5lk"
Hello
```

---

# WHY?

`test()` already finished.

But callback closure retained `msg`.

---

# HUGE ASYNC INSIGHT

Almost all async JS depends on closures.

Callbacks remember surrounding variables.

---

# COMMON INTERVIEW QUESTION

---

# QUESTION

```js id="jlwm5x"
for (var i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output?

```text id="jlwm68"
4
4
4
```

NOT:

```text id="jlwm7d"
1 2 3
```

---

# WHY?

Critical closure understanding.

---

# STEP-BY-STEP

Using:

```js id="jlwm8n"
var i
```

Only ONE shared variable exists.

Loop updates SAME variable:

```text id="jlwm9q"
i → 1
i → 2
i → 3
i → 4
```

---

# After loop finishes:

```text id="jlwmar"
i = 4
```

Now callbacks execute.

Each closure references SAME `i`.

Thus:

```text id="jlwmc1"
4
4
4
```

---

# FIX USING let

```js id="jlwmda"
for (let i = 1; i <= 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```

Output:

```text id="jlwmel"
1
2
3
```

---

# WHY?

`let` creates NEW binding each iteration.

Each closure gets separate variable.

---

# VISUAL

```text id="jlwmfo"
Iteration 1 → i=1
Iteration 2 → i=2
Iteration 3 → i=3
```

Separate lexical environments.

---

# THIS IS EXTREMELY IMPORTANT

Many interviewers ask this.

---

# CLOSURES AND MEMORY LEAKS

Closures can accidentally keep huge memory alive.

---

# Example

```js id="jlwmgv"
function outer() {
  const bigData = new Array(1000000);

  return function inner() {
    console.log("hello");
  };
}
```

Even though inner doesn’t use `bigData`,
older engines may retain whole environment.

Can increase memory usage.

---

# MODERN JS ENGINES

Modern engines optimize unused variables better.

But closures still can retain memory unexpectedly.

---

# REACT HEAVILY USES CLOSURES

Hooks depend deeply on closures.

---

# Example

```js id="jlwmhx"
function Counter() {
  const [count, setCount] = useState(0);

  function increment() {
    setCount(count + 1);
  }
}
```

`increment()` closes over `count`.

Every render creates new closure.

Huge React concept.

---

# STALE CLOSURE PROBLEM

Advanced React issue.

---

# Example

```js id="jlwmj5"
useEffect(() => {
  setInterval(() => {
    console.log(count);
  }, 1000);
}, []);
```

Why old count printed?

Because closure captured OLD render value.

Very advanced insight.

---

# CLOSURE VS SCOPE

Different concepts.

---

# Scope

Defines:

```text id="jlwmk6"
Where variable accessible
```

---

# Closure

Defines:

```text id="jlwml9"
Function retaining access to scope
after parent finished
```

---

# IMPORTANT INTERVIEW QUESTION

---

# QUESTION

```js id="jlwmmh"
function x() {
  var a = 10;

  function y() {
    console.log(a);
  }

  return y;
}

var z = x();

z();
```

WHY output 10 even after x() finished?

---

# PERFECT ANSWER

Because function `y` forms closure with lexical environment of `x`, retaining access to variable `a` through hidden environment reference.

That’s senior-level explanation.

---

# HUGE FINAL INSIGHT

Closures happen because:

```text id="jlwn0i"
Functions are first-class objects
+
Lexical scoping exists
+
Functions carry environment references
```

Without lexical scoping, closures impossible.

---

# MOST IMPORTANT THINGS TO REMEMBER

Closures are NOT special syntax.

They naturally happen whenever:

```text id="jlwn1q"
Function accesses parent variables
and survives beyond parent execution
```

This is one of the deepest and most powerful parts of JavaScript.
