



# 10 Integrated Coding Questions (All 5 Classes)


---

## Question 1: Stateful Counter with Private State

**Problem Statement:** Implement `createCounter(initial, step)` that returns an object with `increment()`, `decrement()`, `getCount()`, and `reset()`. Each counter must maintain its own independent state. Use closures for private state and ensure `this` works correctly when methods are passed as callbacks.

**Topics:** Closures (Class 4), `this` (Class 1)

**Expected Result:**
```javascript
const c1 = createCounter(0, 2);
c1.increment(); c1.increment();  // getCount() → 4
c1.reset();                       // getCount() → 0
const c2 = createCounter(10, 1);
c2.decrement();                   // getCount() → 9
```

**Code Stub:**
```javascript
function createCounter(initial, step) {
  // TODO: Use closure to keep count private
  let count = initial;

  return {
    increment: function () {
      // TODO: Add step to count and return new count
    },
    decrement: function () {
      // TODO: Subtract step from count and return new count
    },
    getCount: function () {
      // TODO: Return current count
    },
    reset: function () {
      // TODO: Reset count to initial and return
    }
  };
}

// Test
const c1 = createCounter(0, 2);
console.log(c1.increment(), c1.increment(), c1.getCount()); // 2, 4, 4
console.log(c1.reset(), c1.getCount()); // 0, 0
```

---

## Question 2: Product Constructor with Prototype Methods

**Problem Statement:** Implement a `Product` constructor that takes `name` and `price`. Add `getPrice()` and `applyDiscount(percent)` on the prototype. Methods should modify and return the discounted price.

**Topics:** Constructor functions (Class 2), Prototype (Class 2)

**Expected Result:**
```javascript
const p = new Product("Laptop", 1000);
p.applyDiscount(10);  // returns 900
p.getPrice();         // 900
```

**Code Stub:**
```javascript
function Product(name, price) {
  // TODO: Assign name and price to the new object using this
}

// TODO: Add getPrice method on Product.prototype

// TODO: Add applyDiscount(percent) on Product.prototype
// applyDiscount should: this.price = this.price * (1 - percent/100), return this.price

// Test
const p = new Product("Laptop", 1000);
console.log(p.applyDiscount(10)); // 900
console.log(p.getPrice());        // 900
```

---

## Question 3: Temperature Class with Private Field and Inheritance

**Problem Statement:** Create a `Temperature` class with private `#celsius` and methods `getCelsius()`, `setCelsius(val)`, and `getFahrenheit()`. Create `TemperatureDisplay` extending `Temperature` with `display()` that prints both units.

**Topics:** ES6 Classes (Class 3), Private fields (Class 3), Inheritance (Class 3), `super` (Class 3)

**Expected Result:**
```javascript
const t = new TemperatureDisplay(25);
t.display();  // "25°C / 77°F"
t.setCelsius(0);
t.display();  // "0°C / 32°F"
```

**Code Stub:**
```javascript
class Temperature {
  #celsius;

  constructor(celsius) {
    // TODO: Initialize #celsius
  }

  getCelsius() {
    // TODO: Return #celsius
  }

  setCelsius(val) {
    // TODO: Set #celsius = val
  }

  getFahrenheit() {
    // TODO: Return (this.#celsius * 9/5) + 32
  }
}

class TemperatureDisplay extends Temperature {
  constructor(celsius) {
    // TODO: Call super with celsius
  }

  display() {
    // TODO: Return string like "25°C / 77°F" using getCelsius() and getFahrenheit()
  }
}

// Test
const t = new TemperatureDisplay(25);
console.log(t.display()); // "25°C / 77°F"
```

---

## Question 4: Memoized Calculator Using Closures

**Problem Statement:** Implement `createCalculator()` that returns `add(a, b)` and `multiply(a, b)`. Cache results so repeated calls with the same arguments return cached values. Cache must be private.

**Topics:** Closures (Class 4), Memoization (Class 4)

**Expected Result:**
```javascript
const calc = createCalculator();
calc.add(2, 3);      // 5 (computed)
calc.add(2, 3);      // 5 (from cache)
calc.multiply(4, 5); // 20 (computed)
```

**Code Stub:**
```javascript
function createCalculator() {
  // TODO: Create private cache object (closure)

  return {
    add: function (a, b) {
      // TODO: Create cache key, check cache, compute if missing, store and return
    },
    multiply: function (a, b) {
      // TODO: Same memoization logic for a * b
    }
  };
}

// Test
const calc = createCalculator();
console.log(calc.add(2, 3), calc.add(2, 3));     // 5, 5
console.log(calc.multiply(4, 5), calc.multiply(4, 5)); // 20, 20
```

---

## Question 5: myReduce Polyfill

**Problem Statement:** Implement `Array.prototype.myReduce` that behaves like native `reduce`. Callback receives `(acc, curr, index, array)`. Handle empty array and missing `initialValue`.

**Topics:** Polyfills (Class 5), Higher-order functions (Class 5)

**Expected Result:**
```javascript
[1, 2, 3].myReduce((a, c) => a + c, 0);  // 6
[1, 2, 3].myReduce((a, c) => a + c);    // 6
[].myReduce((a, c) => a + c, 10);        // 10
```

**Code Stub:**
```javascript
Array.prototype.myReduce = function (callback, initialValue) {
  // TODO: Handle empty array + no initialValue -> throw TypeError
  if (this.length === 0 && initialValue === undefined) {
    throw new TypeError("Reduce of empty array with no initial value");
  }

  let acc;
  let startIndex;

  if (initialValue !== undefined) {
    acc = initialValue;
    startIndex = 0;
  } else {
    // TODO: No initialValue - use first element, start from index 1
  }

  for (let i = startIndex; i < this.length; i++) {
    // TODO: acc = callback(acc, this[i], i, this)
  }

  return acc;
};

// Test
console.log([1, 2, 3].myReduce((a, c) => a + c, 0)); // 6
console.log([1, 2, 3].myReduce((a, c) => a + c));   // 6
```

---

## Question 6: Bank Account – Closure vs Class

**Problem Statement:** Implement `createBankAccount(initialBalance)` returning `{ deposit(amt), withdraw(amt), getBalance() }` with private balance. Also implement `BankAccount` class with private `#balance` and the same API.

**Topics:** Closures (Class 4), Private variables (Class 4), ES6 Classes (Class 3), Private fields (Class 3)

**Expected Result:**
```javascript
const acc1 = createBankAccount(1000);
acc1.deposit(500);   // 1500
acc1.withdraw(200);  // 1300
acc1.getBalance();   // 1300
```

**Code Stub:**
```javascript
// Closure version
function createBankAccount(initialBalance) {
  // TODO: Private balance variable (closure)
  let balance = initialBalance;

  return {
    deposit: function (amt) {
      // TODO: Add amt to balance, return new balance
    },
    withdraw: function (amt) {
      // TODO: Subtract amt from balance (check sufficient funds), return new balance
    },
    getBalance: function () {
      // TODO: Return balance
    }
  };
}

// Class version
class BankAccount {
  #balance;

  constructor(initialBalance) {
    // TODO: Initialize #balance
  }

  deposit(amt) {
    // TODO: Add amt to #balance, return new balance
  }

  withdraw(amt) {
    // TODO: Subtract amt from #balance, return new balance
  }

  getBalance() {
    // TODO: Return #balance
  }
}

// Test
const acc = createBankAccount(1000);
console.log(acc.deposit(500), acc.withdraw(200), acc.getBalance()); // 1500, 1300, 1300
```

---

## Question 7: Shape Hierarchy with Static Method

**Problem Statement:** Create `Shape` with `getArea()` returning 0. Create `Rectangle` and `Circle` extending `Shape` with correct `getArea()`. Add static `Shape.getTotalArea(shapes)` that sums areas of all shapes.

**Topics:** ES6 Classes (Class 3), Inheritance (Class 3), Static methods (Class 3), Polymorphism (Class 3)

**Expected Result:**
```javascript
const rect = new Rectangle(4, 5);
const circle = new Circle(3);
Shape.getTotalArea([rect, circle]); // ~48.27
```

**Code Stub:**
```javascript
class Shape {
  constructor(color = "white") {
    this.color = color;
  }

  getArea() {
    return 0;
  }

  static getTotalArea(shapes) {
    // TODO: Use reduce to sum shapes.map(s => s.getArea())
    return shapes.reduce((sum, s) => sum + s.getArea(), 0);
  }
}

class Rectangle extends Shape {
  constructor(color, width, height) {
    // TODO: Call super(color), assign width and height
  }

  getArea() {
    // TODO: Return width * height
  }
}

class Circle extends Shape {
  constructor(color, radius) {
    // TODO: Call super(color), assign radius
  }

  getArea() {
    // TODO: Return Math.PI * radius * radius
  }
}

// Test
const rect = new Rectangle("red", 4, 5);
const circle = new Circle("blue", 3);
console.log(rect.getArea());                    // 20
console.log(Shape.getTotalArea([rect, circle])); // ~48.27
```

---

## Question 8: Middleware Runner Using Closures

**Problem Statement:** Implement `createMiddlewareRunner(middlewares)` that returns `run(req, res)`. Each middleware receives `(req, res, next)`. `next` must be a closure that invokes the next middleware in the chain.

**Topics:** Closures (Class 4), Higher-order functions (Class 5)

**Expected Result:**
```javascript
const runner = createMiddlewareRunner([
  (req, res, next) => { req.log = [1]; next(); },
  (req, res, next) => { req.log.push(2); next(); },
  (req, res, next) => { req.log.push(3); next(); }
]);
const req = {}; const res = {};
runner(req, res);
// req.log === [1, 2, 3]
```

**Code Stub:**
```javascript
function createMiddlewareRunner(middlewares) {
  return function run(req, res) {
    let index = 0;

    function next() {
      // TODO: Get current middleware, increment index
      // TODO: If middleware exists, call it with (req, res, next)
      const middleware = middlewares[index++];
      if (middleware) {
        middleware(req, res, next);
      }
    }

    next();
  };
}

// Test
const runner = createMiddlewareRunner([
  (req, res, next) => { req.log = [1]; next(); },
  (req, res, next) => { req.log.push(2); next(); },
  (req, res, next) => { req.log.push(3); next(); }
]);
const req = {}; const res = {};
runner(req, res);
console.log(req.log); // [1, 2, 3]
```

---

## Question 9: myMap Polyfill with Prototype

**Problem Statement:** Implement `Array.prototype.myMap` with callback `(element, index, array)`. Handle empty array and preserve behavior of native `map`.

**Topics:** Polyfills (Class 5), Prototype (Class 2), Higher-order functions (Class 5)

**Expected Result:**
```javascript
[1, 2, 3].myMap(x => x * 2);           // [2, 4, 6]
[1, 2, 3].myMap((x, i) => x + i);      // [1, 3, 5]
```

**Code Stub:**
```javascript
Array.prototype.myMap = function (callback) {
  if (this == null || typeof callback !== "function") {
    throw new TypeError("Invalid arguments");
  }

  const result = [];

  for (let i = 0; i < this.length; i++) {
    // TODO: Push callback(element, index, array) to result
    result.push(callback(this[i], i, this));
  }

  return result;
};

// Test
console.log([1, 2, 3].myMap(x => x * 2));       // [2, 4, 6]
console.log([1, 2, 3].myMap((x, i) => x + i)); // [1, 3, 5]
```

---

## Question 10: Shopping Cart with Class, Static, and HOF

**Problem Statement:** Create a `ShoppingCart` class with private `#items` array and methods `addItem(name, price)`, `removeItem(name)`, `getTotal()`, and `getTotalWithDiscount(discountFn)` where `discountFn(price)` returns the discounted price. Add static `ShoppingCart.createFromArray(items)`.

**Topics:** ES6 Classes (Class 3), Private fields (Class 3), Static methods (Class 3), Higher-order functions (Class 5)

**Expected Result:**
```javascript
const cart = ShoppingCart.createFromArray([{ name: "A", price: 100 }, { name: "B", price: 200 }]);
cart.getTotal();                            // 300
cart.getTotalWithDiscount(p => p * 0.9);   // 270
cart.addItem("C", 50);
cart.getTotal();                            // 350
```

**Code Stub:**
```javascript
class ShoppingCart {
  #items = [];

  constructor() {}

  addItem(name, price) {
    // TODO: Push { name, price } to #items
  }

  removeItem(name) {
    // TODO: Remove item where item.name === name from #items
  }

  getTotal() {
    // TODO: Return sum of all item prices using reduce
    return this.#items.reduce((sum, item) => sum + item.price, 0);
  }

  getTotalWithDiscount(discountFn) {
    // TODO: Apply discountFn to each price, sum and return
    return this.#items.reduce((sum, item) => sum + discountFn(item.price), 0);
  }

  static createFromArray(items) {
    // TODO: Create new ShoppingCart, add each item, return cart
    const cart = new ShoppingCart();
    items.forEach(item => cart.addItem(item.name, item.price));
    return cart;
  }
}

// Test
const cart = ShoppingCart.createFromArray([
  { name: "A", price: 100 },
  { name: "B", price: 200 }
]);
console.log(cart.getTotal());                       // 300
console.log(cart.getTotalWithDiscount(p => p * 0.9)); // 270
cart.addItem("C", 50);
console.log(cart.getTotal());                       // 350
```

---

## Summary: Topic Coverage

| Q# | Class 1 (this) | Class 2 (Constructor/Prototype) | Class 3 (Classes) | Class 4 (Closures) | Class 5 (HOF/Polyfills) |
|----|----------------|----------------------------------|-------------------|---------------------|-------------------------|
| 1  | ✓              |                                  |                   | ✓✓                  |                         |
| 2  |                | ✓✓                               |                   |                     |                         |
| 3  |                |                                  | ✓✓✓✓              |                     |                         |
| 4  |                |                                  |                   | ✓✓                  |                         |
| 5  |                |                                  |                   |                     | ✓✓                      |
| 6  |                |                                  | ✓                 | ✓✓                  |                         |
| 7  |                |                                  | ✓✓✓               |                     |                         |
| 8  |                |                                  |                   | ✓✓                  | ✓                       |
| 9  |                | ✓                                |                   |                     | ✓✓                      |
| 10 |                |                                  | ✓✓✓               |                     | ✓                       |
```

---

