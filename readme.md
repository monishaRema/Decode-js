# 🧩 Decode-JS

**Decoding JavaScript — one concept, one day, one mental model at a time.**

---

## About this project

**Decode-JS** is a personal 60-day journey into the internals of JavaScript.  
Each day explores one concept — explained in plain English, visualized with analogies, and proven through real code.  
The goal isn’t memorization; it’s to understand how the JavaScript engine actually *thinks* — from memory to closures, from async flow to performance.

---

## Table of Contents

<table>
  <thead>
    <tr>
      <th>Part</th>
      <th>Theme</th>
      <th>Days</th>
      <th>Focus</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="#part-1">Part 1</a></td>
      <td><a href="#part-1">Engine & Core Fundamentals</a></td>
      <td>1–15</td>
      <td><a href="#part-1">Execution, Scope, Closures, Memory</a></td>
    </tr>
    <tr>
      <td><a href="#part-2">Part 2</a></td>
      <td>Async & Concurrency</td>
      <td>16–30</td>
      <td>Event Loop, Promises, Async/Await</td>
    </tr>
    <tr>
      <td><a href="#part-3">Part 3</a></td>
      <td>DOM & Performance</td>
      <td>31–45</td>
      <td>Events, Observers, Rendering</td>
    </tr>
    <tr>
      <td><a href="#part-4">Part 4</a></td>
      <td>Advanced Mechanics</td>
      <td>46–60</td>
      <td>Prototypes, Classes, Generators, Proxy, Security</td>
    </tr>
  </tbody>
</table>

---

<h2 id="part-1">⚙️ Part 1 — Engine & Core Fundamentals</h2>

### Table Of Content

<a href="#day-1">Day 1 – Execution Context</a>

<a href="#day-2">Day 2 – Variable Environment & Lexical Scope</a>

<a href="#Day-3">Day 3 – Call Stack</a>

---

<h3 id="day-1">Day 1 – Execution Context</h3>

**Definition**  
An execution context is the environment where JavaScript code is evaluated and executed.

**Explanation:**  
Every time JavaScript runs code, it builds a workspace that holds variables, functions, and the `this` reference.  
The global context is created once when your script starts. Each function call builds a new context, pushed onto the **call stack**, and destroyed when finished.

**Analogy:**  
Imagine each function call as opening a new “office.” Inside it are your own desks (variables) and whiteboards (functions). When you leave, the office is cleaned — unless you leave something pinned to the hallway (global scope).

**Example**
```js
let name = "Monisha";
function greet() {
  let msg = "Hello " + name;
  console.log(msg);
}
greet(); // Hello Monisha

```

**Use Case:**

Understanding execution contexts helps you reason about variable lifetime, memory cleanup, and stack traces.

#### Interview Questions

1. What are the two main types of execution contexts?

2. What happens during the creation and execution phases?

3. How does the call stack interact with execution contexts?

4. How is this determined within an execution context?

5. What variables exist in the global execution context?



**Key Takeaway:**

Every function call gets its own isolated workspace — once it finishes, that space is destroyed.

---

<h3 id="day-2">🧩 Day 2 – Variable Environment & Lexical Scope</h3>

**Definition**

A variable environment stores identifiers and their values inside each execution context.

**Explanation**
When a function runs, JavaScript builds a list of variables it knows about (from declarations).
Each function also remembers where it was defined — its lexical scope — so it can access outer variables even after the outer function finishes.

Analogy
Think of nested rooms. You can see what’s in your own room and any room outside, but not inside your child’s room.

Example

```
let city = "Doha";
function showCity() {
  console.log(city); // "Doha"
}
showCity();
```

**Use Case**

Scope chains determine which variable you access when multiple exist with the same name.

**Interview Questions**

1. What is a lexical environment?

2. How does lexical scoping differ from dynamic scoping?

3. How does JavaScript resolve variable references through the scope chain?

4. What is the global environment record?

5. Can inner scopes access outer-scope variables after execution?

**Key Takeaway**

Lexical scope is fixed when you write the function, not when you call it.

<h3 id="Day-3">🧱 Day 3 – Call Stack</h3>

**Definition**

The call stack keeps track of which function is currently running and where to return after it finishes.

**Explanation**

Each time a function is invoked, a new execution context is pushed onto the stack.
When the function ends, its context is popped off. If too many stack frames build up (like unending recursion), you get a stack overflow.

**Analogy**

Imagine a pile of dinner plates — you can only remove the top one (LIFO: last in, first out).

Example

```
function a(){ b(); }
function b(){ c(); }
function c(){ console.log("End"); }
a(); // a → b → c → pop pop pop

```
**Use Case**

Understanding the call stack helps debug recursive functions and async timing.

Interview Questions

How does the call stack work in JavaScript?

What happens during a stack overflow?

Why is JS single-threaded but non-blocking?

How are async functions handled differently from the stack?

How does the call stack relate to the event loop?

Key Takeaway
The call stack executes synchronously: one function at a time, top to bottom.

🪄 Day 4-5 – Hoisting
Definition
Hoisting is JavaScript’s behavior of moving declarations (not initializations) to the top of their scope during compilation.

Explanation
Variables declared with var are hoisted and initialized as undefined.
let and const are hoisted too but placed in the temporal dead zone until their line executes.
Function declarations are fully hoisted and callable before definition.

Analogy
Think of JS reserving a seat for each variable before the show starts — but for let/const, the seat is roped off until the curtain rises.

Example

js
Copy code
console.log(a); // undefined
var a = 5;

sayHi(); // works
function sayHi(){ console.log("Hello"); }

console.log(b); // ReferenceError
let b = 10;
Use Case
Explains why some variables are “undefined” before declaration and why TDZ errors occur.

Interview Questions

What does “hoisting” mean in JS?

Are let and const variables hoisted?

Why does accessing a let variable before declaration throw a ReferenceError?

How are function expressions hoisted compared to declarations?

What is the Temporal Dead Zone (TDZ)?

Key Takeaway
Declarations are hoisted; initializations are not.

🧩 Day 6 – Scope Chain & Lexical Environment
Definition
The scope chain is the hierarchy that determines how JS looks up variables.

Explanation
When you reference a variable, JS first checks the current scope; if not found, it climbs the outer scopes until the global scope.
If it isn’t found anywhere, you get a ReferenceError.

Analogy
You ask your team for a pen; if no one has one, you walk up the management chain until the CEO (global scope).

Example

js
Copy code
let x = 10;
function outer(){
  let y = 20;
  function inner(){
    let z = 30;
    console.log(x + y + z);
  }
  inner();
}
outer(); // 60
Use Case
Essential for understanding closures and function nesting.

Interview Questions

What is the scope chain?

How does JS resolve variables across nested scopes?

What happens if a variable isn’t found in any scope?

How do closures rely on lexical environments?

Can global variables shadow local ones?

Key Takeaway
JS resolves variables by searching outward through the scope chain.

🌀 Day 7-8 – Closures
Definition
A closure is when a function remembers its lexical scope even after the outer function has finished.

Explanation
Returning a function from another function allows it to retain access to outer variables.
This enables private state, encapsulation, and powerful function patterns.

Analogy
A child keeps a copy of their parent’s house key even after moving out — they can still enter that house.

Example

js
Copy code
function counter(){
  let count = 0;
  return function(){
    count++;
    console.log(count);
  };
}
const add = counter();
add(); // 1
add(); // 2
Use Case
Used in React hooks, module patterns, and data privacy.

Interview Questions

What is a closure in JS?

How does a closure maintain access to outer variables?

Give a practical use case for closures.

How can closures cause memory leaks?

How do closures differ from simple scope references?

Key Takeaway
Closures let functions carry memory from their creation environment.

🧭 Day 9 – this Keyword
Definition
this is a special reference that depends on how a function is called, not where it’s written.

Explanation
In the global scope (window or global), this refers to that object.
Inside an object method, this refers to that object.
In arrow functions, this is lexically bound — it inherits from the outer scope.

Analogy
Think of this as a “name tag.” It changes depending on which room you’re in.

Example

js
Copy code
const user = {
  name: "Monisha",
  greet() {
    console.log("Hi " + this.name);
  }
};
user.greet(); // Hi Monisha
Use Case
Critical for object methods, event handlers, and class constructors.

Interview Questions

How is this determined in different call contexts?

How does this behave in arrow functions?

How can you manually set this?

What is the value of this in strict mode?

What common bugs happen when this is misused?

Key Takeaway
this is not lexically scoped; it’s determined by how the function is invoked.

🧩 Day 10 – call / apply / bind
Definition
These methods manually control what this refers to inside a function.

Explanation
call and apply execute the function immediately with a chosen this.
bind returns a new function permanently bound to that context.

Analogy
You borrow someone else’s phone (call/apply) for a single call, or you get your own SIM card (bind).

Example

js
Copy code
function intro(job){
  console.log(`I'm ${this.name}, a ${job}`);
}
const user = { name: "Monisha" };
intro.call(user, "Developer");
intro.apply(user, ["Engineer"]);
const bound = intro.bind(user, "Architect");
bound();
Use Case
Used in event handlers, class methods, and function currying.

Interview Questions

What’s the difference between call, apply, and bind?

How can bind help in event listeners?

What happens if you call a bound function again with call?

Why does apply take an array?

When would you prefer bind over call?

Key Takeaway
Use bind to lock context, call/apply to borrow temporarily.

💧 Day 11 – Pure vs Impure Functions
Definition
A pure function always returns the same output for the same input and has no side effects.

Explanation
Impure functions change external state or depend on it. Purity makes code predictable and testable.

Analogy
A pure function is like a vending machine — put in a coin, get the same snack every time.
An impure function is like a human barista — might forget sugar tomorrow.

Example

js
Copy code
// Pure
function add(a,b){ return a+b; }

// Impure
let total=0;
function addToTotal(a){ total+=a; }
Use Case
Core of functional programming and React render logic.

Interview Questions

What defines a pure function?

Why are pure functions easier to test?

Give an example of a side effect.

How do impure functions affect React rendering?

What’s referential transparency?

Key Takeaway
Pure functions are deterministic and safe for reuse.

🔗 Day 12 – Copy by Value vs Reference
Definition
Primitives (string, number, boolean, etc.) are copied by value; objects and arrays by reference.

Explanation
When you assign a primitive, you get a new copy.
When you assign an object, you get a pointer to the same memory.

Analogy
Copying by value = photocopy a document.
Copying by reference = hand the same folder to someone else.

Example

js
Copy code
let x = 10;
let y = x;
y++;
console.log(x); //10

let obj1 = {name:"A"};
let obj2 = obj1;
obj2.name = "B";
console.log(obj1.name); //B
Use Case
Understanding reference behavior prevents state mutation bugs in React and Redux.

Interview Questions

Which data types are copied by value vs reference?

How can mutating a shared object cause bugs?

How to clone an object safely?

Why does spreading an array create a shallow copy?

What’s the difference between reference equality and value equality?

Key Takeaway
Objects share memory references unless explicitly cloned.

🧩 Day 13 – Deep vs Shallow Copy
Definition
A shallow copy duplicates only top-level properties; a deep copy duplicates nested objects too.

Explanation
Object.assign() and spread operators make shallow copies.
Deep copies can be done with structuredClone(), _.cloneDeep(), or manual recursion.

Analogy
Shallow copy = copy the outer box but not the contents.
Deep copy = copy the entire box and everything inside.

Example

js
Copy code
const original = { info: { name: "M" } };
const shallow = { ...original };
shallow.info.name = "X";
console.log(original.info.name); // X

const deep = structuredClone(original);
deep.info.name = "Y";
console.log(original.info.name); // X
Use Case
Needed when managing immutable state or duplicating complex data safely.

Interview Questions

What’s the difference between shallow and deep copy?

Why does spread create only a shallow copy?

How can structuredClone simplify deep copying?

When is deep copy required in state management?

Why is JSON.parse(JSON.stringify()) not always safe?

Key Takeaway
Shallow copy shares nested references; deep copy breaks them.

🪶 Day 14 – Immutability
Definition
Immutability means data cannot be changed once created — you create new copies instead of mutating existing ones.

Explanation
Immutable patterns make apps predictable and debuggable.
You can achieve it by copying objects before changing them.

Analogy
Like bank records — you never edit past transactions; you add a new entry.

Example

js
Copy code
const user = {name:"M"};
const updated = {...user, name:"N"};
console.log(user.name); //M
Use Case
Essential in React, Redux, and functional design.

Interview Questions

Why is immutability important in functional programming?

How does immutability prevent unexpected side effects?

What tools or methods ensure immutability in JS?

How does immutability improve React performance?

What’s the cost of copying large immutable structures?

Key Takeaway
Never mutate; create new versions to keep history clean.

🧹 Day 15 – Garbage Collection & Memory Leaks
Definition
Garbage Collection (GC) automatically frees memory from objects no longer reachable.
A memory leak happens when unused data remains referenced.

Explanation
JS uses reachability: if no variable points to an object, it’s eligible for collection.
Leaks often come from event listeners, intervals, or global references.

Analogy
Think of a waiter clearing plates only when the table is empty.
If you keep holding a fork (reference), the plate stays forever.

Example

js
Copy code
let data = { bigArray: new Array(100000).fill(1) };
data = null; // now free for GC

// Memory leak
let arr = [];
setInterval(()=> arr.push(1), 1000); // never cleared
Use Case
Prevent performance degradation in long-running apps by clearing timers and listeners.

Interview Questions

What algorithm does JS GC commonly use (mark-and-sweep)?

How do closures cause memory