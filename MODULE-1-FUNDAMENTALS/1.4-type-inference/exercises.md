# Chapter 1.4 Exercises

## Exercise 1: Inference Detection

For each variable, predict what type TypeScript will infer:

```typescript
const a = 100;
let b = 100;
const c = "hello" + "world";
let d = [1, 2, 3];
const e = { x: 10, y: 20 };
let f = null;
const g = [1, "two", true];
```

<details>
<summary>Solution</summary>

```typescript
const a = 100;                  // type: 100 (literal)
let b = 100;                    // type: number
const c = "hello" + "world";    // type: string
let d = [1, 2, 3];              // type: number[]
const e = { x: 10, y: 20 };     // type: { x: number; y: number }
let f = null;                   // type: any (bad practice!)
const g = [1, "two", true];     // type: (string | number | boolean)[]
```
</details>

---

## Exercise 2: Fix Inference Issues

Fix the type inference problems in this code without adding explicit types where not needed:

```typescript
// Problem 1: Empty array
const users = [];
users.push({ name: "Alice", age: 30 });

// Problem 2: Null initial value
let currentUser = null;
currentUser = { name: "Bob" };

// Problem 3: Function parameter
const double = (x) => x * 2;
```

<details>
<summary>Solution</summary>

```typescript
// Solution 1: Type the empty array
interface User {
    name: string;
    age: number;
}
const users: User[] = [];
users.push({ name: "Alice", age: 30 });

// Solution 2: Type the null variable
let currentUser: { name: string } | null = null;
currentUser = { name: "Bob" };

// Solution 3: Type the parameter
const double = (x: number): number => x * 2;
```
</details>

---

## Exercise 3: as const Practice

Use `as const` to create strict literal types:

```typescript
// Convert this to have literal types
const CONFIG = {
    API_URL: "https://api.example.com",
    TIMEOUT: 5000,
    METHODS: ["GET", "POST", "PUT"]
};

// Goal: CONFIG.API_URL should be type "https://api.example.com"
// Goal: METHODS should be readonly ["GET", "POST", "PUT"]
```

<details>
<summary>Solution</summary>

```typescript
const CONFIG = {
    API_URL: "https://api.example.com",
    TIMEOUT: 5000,
    METHODS: ["GET", "POST", "PUT"]
} as const;

// Now:
// CONFIG.API_URL is type: "https://api.example.com"
// CONFIG.TIMEOUT is type: 5000
// CONFIG.METHODS is type: readonly ["GET", "POST", "PUT"]

// Bonus: Create a type from METHODS
type HttpMethod = typeof CONFIG.METHODS[number];
// Type is: "GET" | "POST" | "PUT"
```
</details>

---

## Exercise 4: Contextual Typing

Complete the callback functions - TypeScript should infer parameter types:

```typescript
const numbers = [1, 2, 3, 4, 5];

// Complete these without adding type annotations
const squared = numbers.map(/* your code */);       // [1, 4, 9, 16, 25]
const evens = numbers.filter(/* your code */);      // [2, 4]
const sum = numbers.reduce(/* your code */);        // 15

const buttons = document.querySelectorAll("button");
buttons.forEach(/* your code with click handler */);
```

<details>
<summary>Solution</summary>

```typescript
const numbers = [1, 2, 3, 4, 5];

// TypeScript infers 'n' as number from array type
const squared = numbers.map(n => n * n);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

const buttons = document.querySelectorAll("button");
// TypeScript infers 'button' as Element, 'event' as Event
buttons.forEach(button => {
    button.addEventListener("click", event => {
        console.log("Clicked!", event.target);
    });
});
```
</details>

---

## Exercise 5: Best Practices

Identify which explicit types are redundant and which are necessary:

```typescript
// Which explicit types are unnecessary?
const name: string = "Alice";
const age: number = 30;
const getData: () => Promise<void> = async () => { };
const items: string[] = ["a", "b", "c"];

function processUser(user: User): void {
    console.log(user.name);
}

const result: number = calculateTotal(items);
```

<details>
<summary>Solution</summary>

```typescript
// Redundant - TypeScript can infer these
const name = "Alice";           // Remove : string
const age = 30;                 // Remove : number
const items = ["a", "b", "c"];  // Remove : string[]

// Necessary or recommended
const getData: () => Promise<void> = async () => { };
// ^-- Could be inferred, but explicit is clearer for complex types

function processUser(user: User): void {
    // ^-- user: User is REQUIRED (parameters need types)
    // ^-- : void is optional but good for documentation
    console.log(user.name);
}

// Optional - return type is inferred but explicit can help
const result: number = calculateTotal(items);
// If calculateTotal has proper types, : number is redundant
```
</details>

---

## Exercise 6: Destructuring Inference

Add types only where necessary:

```typescript
// API response
const response = await fetch("/api/user").then(r => r.json());
const { id, name, email } = response;

// Config with defaults
const config = { timeout: 5000 };
const { timeout, retries = 3 } = config;

// Complex destructuring
function processOptions({ onSuccess, onError, maxRetries = 3 }) {
    // ...
}
```

<details>
<summary>Solution</summary>

```typescript
// API response - needs explicit type (json() returns 'any')
interface UserResponse {
    id: number;
    name: string;
    email: string;
}
const response: UserResponse = await fetch("/api/user").then(r => r.json());
const { id, name, email } = response;
// id: number, name: string, email: string

// Config with defaults - types mostly inferred
const config = { timeout: 5000 };
const { timeout, retries = 3 } = config;
// timeout: number, retries: number

// Complex destructuring - parameter types required
interface Options {
    onSuccess: () => void;
    onError: (error: Error) => void;
    maxRetries?: number;
}

function processOptions({ onSuccess, onError, maxRetries = 3 }: Options) {
    // Now properly typed
}
```
</details>

---

## Quiz

1. What type does `const x = 10` have?
   - [ ] number
   - [x] 10 (literal type)
   - [ ] const number
   - [ ] integer

2. What does `as const` do?
   - [ ] Makes variable constant
   - [x] Prevents type widening
   - [ ] Adds const keyword
   - [ ] Creates immutable copy

3. When should you use explicit types?
   - [ ] Always
   - [ ] Never
   - [x] For complex types, empty collections, function parameters
   - [ ] Only for primitives

4. What is contextual typing?
   - [ ] Typing based on variable context
   - [x] TypeScript inferring types from usage context
   - [ ] Manual type annotations
   - [ ] IDE auto-complete

---

[← Back to Chapter](./README.md) | [Next Chapter →](../1.5-type-annotations/README.md)
