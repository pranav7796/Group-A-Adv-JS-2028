# Advanced JS Exam Revision Notes (3-Hour Sprint)

Use this as a last-minute high-yield sheet for all 5 classes and all 20 questions.

## 3-Hour Strategy

- 0:00-0:35: Class 1 + Class 2 quick revision and 4 key code templates.
- 0:35-1:10: Class 3 + Class 4 key patterns and common pitfalls.
- 1:10-1:45: Class 5 polyfills and HOF chaining.
- 1:45-2:30: Re-solve weak questions only (target 8 to 10).
- 2:30-3:00: Output formatting and syntax checks.

## Class 1: `this` Keyword (Q1-Q4)

### Core Rules

- `this` depends on how function is called, not where defined.
- `obj.method()` -> `this === obj`.
- Plain function call in strict mode -> `this === undefined`.
- Arrow functions do not have their own `this`; they capture lexical `this`.

### Exam Pitfalls

- Nested regular function inside object method loses outer `this`.
- `setTimeout(function(){})` loses object `this`.
- In strict mode examples, expected output can differ by environment for top-level `this`.

### Fast Fix Patterns

```js
// Pattern A: nested function this fix
const self = this;
function inner() { console.log(self.name); }

// Pattern B: arrow callback in method
setTimeout(() => console.log(this.name), 100);

// Pattern C: bind
setTimeout(function(){ console.log(this.name); }.bind(this), 100);
```

## Class 2: Constructor Functions + Prototype (Q5-Q8)

### Core Rules

- Constructor function is called with `new`.
- Instance properties in constructor (`this.x = x`).
- Shared methods on prototype (`Fn.prototype.method = ...`) for memory efficiency.
- `new` internally does:
  1. Create object
  2. Link prototype
  3. Call constructor with `this`
  4. Return object (or returned object if constructor returns one)

### Exam Pitfalls

- Forgetting `new`.
- Putting methods inside constructor (memory waste).
- Modifying built-in prototypes where not required (Q8 warns against that).

### Fast Templates

```js
function Book(title, author) {
  this.title = title;
  this.author = author;
}
Book.prototype.getInfo = function () {
  return `${this.title} by ${this.author}`;
};
```

```js
function myNew(Ctor, ...args) {
  const obj = Object.create(Ctor.prototype);
  const out = Ctor.apply(obj, args);
  return out !== null && (typeof out === "object" || typeof out === "function") ? out : obj;
}
```

```js
function Car(make, model) { this.make = make; this.model = model; }
Car.prototype.describe = function () { return `${this.make} - ${this.model}`; };
function createCar(make, model) {
  return { make, model, describe() { return `${make} - ${model}`; } };
}
```

## Class 3: ES6 Classes (Q9-Q12)

### Core Rules

- Class methods are on prototype by default.
- `static` belongs to class, not instance.
- Private members use `#name`.
- Child class constructor must call `super(...)` before using `this`.

### Exam Pitfalls

- Accessing private field without `#`.
- Calling static method on instance.
- Using `this` before `super()` in child class.

### Fast Templates

```js
class Order {
  static count = 0;
  #id;
  constructor() { this.#id = Order.count++; }
  getId() { return this.#id; }
  static getCount() { return Order.count; }
}
```

```js
class Shape {
  constructor(name) { this.name = name; }
  describe() { return `Shape: ${this.name}`; }
}
class Circle extends Shape {
  constructor(name, radius) { super(name); this.radius = radius; }
  describe() { return `Circle: ${this.name}, radius ${this.radius}`; }
}
```

## Class 4: Closures (Q13-Q16)

### Core Rules

- Closure = function + remembered lexical variables.
- Outer variable remains alive if inner function references it.
- Great for private state and memoization.

### Exam Pitfalls

- Returning value but not mutating private variable.
- Sharing state accidentally between instances (should be separate per function call).
- Memo cache check using truthy test only (`if (cache[n])`) fails for value `0`; use `n in cache`.

### Fast Templates

```js
function createCounter(initial, step) {
  let count = initial;
  return {
    increment() { count += step; return count; },
    decrement() { count -= step; return count; },
    getCount() { return count; }
  };
}
```

```js
function createMemoizedAdd() {
  const cache = {};
  return function (n) {
    if (n in cache) return cache[n];
    cache[n] = n + n;
    return cache[n];
  };
}
```

```js
function createBankAccount(initialBalance) {
  let balance = initialBalance;
  return {
    deposit(amount) { balance += amount; return balance; },
    withdraw(amount) { balance -= amount; return balance; },
    getBalance() { return balance; }
  };
}
```

```js
function createMultiplier(factor) {
  return function (n) { return n * factor; };
}
```

## Class 5: HOF + Polyfills (Q17-Q20)

### Core Rules

- HOF means function takes function/returns function.
- `map`: transform each item.
- `filter`: keep matching items.
- `reduce`: accumulate to one value.
- Polyfill should not use native same method internally (for exam intent).

### Exam Pitfalls

- `myReduce` empty array without initial value must throw.
- Forgetting callback params `(value, index, array)`.
- Missing `thisArg` support in map/filter.
- Output format mismatch in Q20 (`"; "` not just `";"`).

### Fast Polyfill Templates

```js
Array.prototype.myMap = function (callback, thisArg) {
  if (this == null) throw new TypeError("Array is null/undefined");
  if (typeof callback !== "function") throw new TypeError("callback is not a function");
  const arr = Object(this);
  const out = [];
  for (let i = 0; i < arr.length; i++) {
    if (i in arr) out.push(callback.call(thisArg, arr[i], i, arr));
  }
  return out;
};
```

```js
Array.prototype.myFilter = function (callback, thisArg) {
  if (this == null) throw new TypeError("Array is null/undefined");
  if (typeof callback !== "function") throw new TypeError("callback is not a function");
  const arr = Object(this);
  const out = [];
  for (let i = 0; i < arr.length; i++) {
    if (i in arr && callback.call(thisArg, arr[i], i, arr)) out.push(arr[i]);
  }
  return out;
};
```

```js
Array.prototype.myReduce = function (callback, initialValue) {
  if (this == null) throw new TypeError("Array is null/undefined");
  if (typeof callback !== "function") throw new TypeError("callback is not a function");
  const arr = Object(this);
  if (arr.length === 0 && arguments.length < 2) {
    throw new TypeError("Reduce of empty array with no initial value");
  }
  let i = 0;
  let acc = initialValue;
  if (arguments.length < 2) {
    acc = arr[0];
    i = 1;
  }
  for (; i < arr.length; i++) {
    if (i in arr) acc = callback(acc, arr[i], i, arr);
  }
  return acc;
};
```

```js
const people = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 17 },
  { name: "Charlie", age: 30 }
];

const result = people
  .filter(p => p.age >= 18)
  .map(p => `${p.name} is adult`)
  .reduce((acc, curr) => acc + (acc ? "; " : "") + curr, "");

console.log(result); // Alice is adult; Charlie is adult
```

## All 20 Questions: One-Line Recall

- Q1: Nested function loses `this`; use arrow/self/bind.
- Q2: setTimeout callback loses `this`; use arrow/bind.
- Q3: Lost method `this`; solve with closure over base.
- Q4: Strict mode plain function `this` is undefined.
- Q5: Constructor + prototype method.
- Q6: Implement `new` internals.
- Q7: Constructor vs factory + `instanceof` difference.
- Q8: Add method on custom prototype only; avoid pollution.
- Q9: Private field + static counter.
- Q10: Inheritance with `extends/super` and override.
- Q11: Private helper method and public API.
- Q12: Getter with private field and formula conversion.
- Q13: Counter closure with independent state.
- Q14: Memoization closure with private cache.
- Q15: Private balance with closure methods.
- Q16: Function factory closure.
- Q17: `myMap` polyfill.
- Q18: `myFilter` polyfill.
- Q19: `myReduce` polyfill edge cases.
- Q20: Native `filter -> map -> reduce` chain with exact output format.

## 10-Minute Final Checklist Before Exam

- Confirm semicolons/spaces in expected output strings.
- Confirm method names exactly match question (`getInfo`, `getId`, etc.).
- Confirm `super(...)` is first in child constructor.
- Confirm private fields use `#` everywhere.
- Confirm reduce edge case throw for empty array no initial value.
- Confirm you are not redefining built-ins when question forbids it.
- Run each snippet once mentally with sample input.

## Bonus: Module Wrapper (if asked in viva/interview)

- Node wraps each CommonJS file in:
  `(function (exports, require, module, __filename, __dirname) { ... })`
- Why: module scope, no global pollution, injects CommonJS helpers.
- `exports` is reference to `module.exports` (reassigning breaks link).
- `require` loads, executes once, caches, returns `module.exports`.

All the best. Stay calm, write clean, and optimize for correctness over cleverness.
