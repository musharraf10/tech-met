# PHASE 3 — REACT INTERNALS (Deep Dive)

Now we move from:

```text id="p1k9xw"
"Using React"
```

to:

```text id="r7m2vq"
Understanding HOW React actually works internally
```

This is where frontend engineering becomes serious.

Most developers know:

* `useState`
* `useEffect`
* props
* components

But few understand:

* reconciliation
* Fiber architecture
* render phases
* batching
* virtual DOM internals
* why re-renders happen
* stale closures
* hook internals
* concurrent rendering

This phase is HUGE for:

* React interviews
* performance optimization
* debugging
* writing scalable apps

---

# REACT IS NOT MAGIC

React is basically:

```text id="n4q8vz"
A UI synchronization engine
```

It tries to keep:

```text id="x2m7qp"
UI synchronized with state
```

That’s React’s core purpose.

---

# FIRST BIG IDEA

React does NOT directly manipulate DOM on every change.

That would be slow.

Instead React uses:

# Virtual DOM

---

# WHAT IS VIRTUAL DOM?

Virtual DOM is:

```text id="v5m1qx"
A lightweight JavaScript representation of real DOM
```

---

# REAL DOM EXAMPLE

Browser DOM:

```html id="s7m9vk"
<div>
  <h1>Hello</h1>
</div>
```

Browser stores huge complex DOM structures internally.

DOM operations expensive.

---

# REACT SOLUTION

React creates lightweight JS objects.

---

# INTERNAL REPRESENTATION

Roughly:

```js id="m3q8vp"
{
  type: "div",
  props: {
    children: [
      {
        type: "h1",
        props: {
          children: "Hello"
        }
      }
    ]
  }
}
```

This is Virtual DOM node.

---

# WHY VIRTUAL DOM?

Because:

```text id="q1m7vx"
JavaScript object operations are cheaper
than direct DOM manipulation
```

---

# HUGE MISCONCEPTION

People say:

```text id="x8m2qv"
"Virtual DOM is faster than real DOM"
```

Not exactly.

Real reason:

```text id="v4m9qp"
React minimizes expensive DOM updates
```

That’s the important part.

---

# REACT RENDERING FLOW

Critical.

---

# STEP 1 — STATE CHANGES

```js id="n6m1qx"
setCount(1);
```

---

# STEP 2 — COMPONENT RE-EXECUTES

Function component runs again.

---

# STEP 3 — NEW VIRTUAL DOM CREATED

React builds fresh virtual tree.

---

# STEP 4 — DIFFING

React compares:

```text id="m9q2vx"
Old Virtual DOM
vs
New Virtual DOM
```

---

# STEP 5 — RECONCILIATION

React determines minimal changes needed.

---

# STEP 6 — REAL DOM UPDATE

Only necessary DOM updates applied.

---

# THIS WHOLE PROCESS IS:

# Reconciliation

Very important interview topic.

---

# HUGE INSIGHT

Function component re-render means:

```text id="q5m8vp"
Entire component function executes again
```

NOT entire DOM recreated.

Critical distinction.

---

# EXAMPLE

```js id="x1m7qv"
function Counter() {
  const [count, setCount] = useState(0);

  console.log("render");

  return <h1>{count}</h1>;
}
```

Every state update:

```text id="v8m2qp"
Counter() runs again
```

Entire function re-executes.

---

# HUGE QUESTION

If function reruns:

```text id="n4m9qx"
Why state doesn't reset?
```

Answer:

# React stores hook state externally

Not inside function variables.

Massive concept.

---

# HOW useState WORKS INTERNALLY

Simplified idea.

---

# REACT STORES

```text id="m2q7vp"
Component state array internally
```

Example conceptual storage:

```js id="q9m1vx"
hooks = [0]
```

---

# FIRST RENDER

```js id="x5m8qp"
const [count] = useState(0);
```

React sees:

```text id="v1m7qx"
hooks[0] = 0
```

---

# NEXT RENDER

Function reruns.

React uses hook order:

```text id="n8m2qv"
"First useState belongs to hooks[0]"
```

Very important.

---

# THIS IS WHY

Hooks must NOT be conditional.

---

# BAD

```js id="m4q9vp"
if (x) {
  useState();
}
```

Breaks hook order mapping.

Huge React rule.

---

# REACT RE-RENDERING

Critical concept.

---

# WHAT CAUSES RE-RENDER?

Usually:

* state changes
* props changes
* parent re-render
* context changes

---

# IMPORTANT INSIGHT

React uses:

```js id="q2m7vx"
Object.is()
```

for state comparison.

Reference identity matters hugely.

---

# EXAMPLE

```js id="x7m1qp"
const [user, setUser] = useState({
  name: "A"
});

user.name = "B";

setUser(user);
```

May not re-render properly.

---

# WHY?

Reference unchanged.

```js id="v3m9qx"
oldUser === newUser
```

TRUE.

React thinks nothing changed.

---

# CORRECT

```js id="n6m2qv"
setUser({
  ...user,
  name: "B"
});
```

New reference created.

---

# THIS IS WHY IMMUTABILITY IMPORTANT

Huge React concept.

---

# RECONCILIATION ALGORITHM

Now deeper.

React compares trees efficiently.

---

# EXAMPLE

OLD:

```html id="m8q1vp"
<ul>
  <li>A</li>
</ul>
```

NEW:

```html id="q4m9vx"
<ul>
  <li>B</li>
</ul>
```

React sees:

```text id="x1m7qp"
Only text changed
```

Updates only text node.

---

# KEYS IN REACT

Very important.

---

# EXAMPLE

```js id="v8m2qx"
items.map(item => (
  <li key={item.id}>{item.name}</li>
))
```

---

# WHY KEYS IMPORTANT?

Help React identify elements between renders.

---

# WITHOUT KEYS

React may incorrectly reuse DOM nodes.

Causing:

* bugs
* incorrect state preservation
* inefficient updates

---

# HUGE INSIGHT

Keys help React answer:

```text id="n5m9qv"
"Which old element matches new element?"
```

---

# FIBER ARCHITECTURE

VERY IMPORTANT ADVANCED TOPIC.

React 16+ introduced Fiber.

---

# BEFORE FIBER

Rendering synchronous.

Large updates could block UI.

---

# FIBER SOLUTION

React broke rendering into:

```text id="q2m8vx"
Small interruptible units
```

---

# HUGE BENEFITS

* smoother UI
* priority scheduling
* concurrent rendering
* pausable work

---

# FIBER NODE

Each component represented internally as Fiber object.

Contains:

* type
* state
* props
* child
* sibling
* parent
* effect info

---

# REACT HAS TWO PHASES

Critical interview concept.

---

# 1. RENDER PHASE

React calculates changes.

Can be paused/interrupted.

NO DOM updates yet.

---

# 2. COMMIT PHASE

Actual DOM updates happen.

This phase synchronous.

---

# IMPORTANT

DOM mutations happen ONLY during commit phase.

---

# useEffect TIMING

Huge concept.

---

# Example

```js id="x7m1qv"
useEffect(() => {
  console.log("effect");
});
```

Runs AFTER paint/commit.

---

# WHY?

Effects should not block rendering.

---

# useLayoutEffect

Runs synchronously before browser paint.

Can block rendering.

Advanced optimization topic.

---

# BATCHING

Very important.

---

# Example

```js id="v3m9qp"
setCount(c => c + 1);
setCount(c => c + 1);
```

React batches updates.

Avoids unnecessary renders.

---

# RESULT

Single render instead of two.

---

# STALE CLOSURES

Massive React interview topic.

---

# Example

```js id="n6m2qx"
useEffect(() => {
  setInterval(() => {
    console.log(count);
  }, 1000);
}, []);
```

Why old count printed?

Because closure captured old render value.

---

# HUGE REACT INSIGHT

Every render creates NEW closures.

Critical concept.

---

# MEMOIZATION IN REACT

---

# React.memo

Prevents unnecessary child renders.

---

# useMemo

Caches expensive calculations.

---

# useCallback

Caches function references.

Important because functions recreated every render.

---

# WHY FUNCTION REFERENCES MATTER

```js id="m8q1qv"
() => {}
```

creates NEW function each render.

Reference changes.

Can trigger child re-renders.

---

# REACT IS DECLARATIVE

Important philosophical concept.

---

# IMPERATIVE

```text id="q4m9qp"
"How to update UI"
```

---

# DECLARATIVE

```text id="x1m7qx"
"UI should look like this state"
```

React figures out updates.

---

# CONCURRENT RENDERING

Advanced modern React.

Allows:

* interruptible rendering
* transitions
* responsiveness

Powered by Fiber scheduler.

---

# STRICT MODE DOUBLE RENDER

React StrictMode intentionally double-invokes some logic in development.

Helps detect side effects.

Huge interview/debugging topic.

---

# SERVER COMPONENTS

Advanced modern React architecture.

Moves rendering work server-side.

Reduces bundle size.

---

# HYDRATION

SSR concept.

React attaches event listeners to server-rendered HTML.

---

# HUGE PERFORMANCE INSIGHT

React optimization mostly about:

* preventing unnecessary renders
* stabilizing references
* reducing expensive calculations
* memoization
* splitting work

---

# SENIOR-LEVEL UNDERSTANDING

You should now understand:

* Virtual DOM is lightweight representation
* reconciliation calculates minimal updates
* rendering ≠ DOM update
* hooks stored externally by React
* hook order matters
* Fiber enables concurrent rendering
* effects run after commit
* closures recreated every render
* immutability drives change detection

---

## MOST IMPORTANT TAKEAWAYS

---

## React is a UI synchronization engine

---

# Components re-execute on render

---

## State lives inside React, not component function

---

## Reconciliation minimizes DOM updates

---

## Fiber enables interruptible rendering

---

## Hook order is critical

---

## Reference identity controls many optimizations

---

# Closures deeply affect React behavior
