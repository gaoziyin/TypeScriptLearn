# Chapter 1.1 Exercises

## Exercise 1: JavaScript Pain Points

Identify the potential bugs in this JavaScript code:

```javascript
function processOrder(order) {
    const total = order.price * order.quantity;
    const discount = order.discountCode ? 0.1 : 0;
    return total - (total * discount);
}

processOrder({ 
    price: "29.99", 
    quantity: 2 
});
```

<details>
<summary>Solution</summary>

**Issues:**
1. `price` is a string, causing `"29.99" * 2 = 59.98` (works due to coercion but unreliable)
2. `discountCode` is undefined, so `discount = 0` (works but unclear intent)
3. No validation if `order` is null/undefined
4. Return type is not guaranteed to be a number

**TypeScript solution:**

```typescript
interface Order {
    price: number;
    quantity: number;
    discountCode?: string;
}

function processOrder(order: Order): number {
    const total = order.price * order.quantity;
    const discount = order.discountCode ? 0.1 : 0;
    return total - (total * discount);
}
```
</details>

---

## Exercise 2: Type the Function

Convert this JavaScript function to TypeScript with proper types:

```javascript
function createUser(name, email, isAdmin) {
    return {
        id: Math.random().toString(36),
        name: name,
        email: email,
        isAdmin: isAdmin || false,
        createdAt: new Date()
    };
}
```

<details>
<summary>Solution</summary>

```typescript
interface User {
    id: string;
    name: string;
    email: string;
    isAdmin: boolean;
    createdAt: Date;
}

function createUser(
    name: string, 
    email: string, 
    isAdmin: boolean = false
): User {
    return {
        id: Math.random().toString(36),
        name,
        email,
        isAdmin,
        createdAt: new Date()
    };
}
```
</details>

---

## Exercise 3: Find the Bug

What's wrong with this code? How would TypeScript help?

```javascript
const config = {
    apiUrl: "https://api.example.com",
    timeout: 5000,
    retries: 3
};

function makeRequest(url, options) {
    console.log(`Fetching ${url} with timeout ${options.timeOut}`);
}

makeRequest(config.apiUrl, config);
```

<details>
<summary>Solution</summary>

**Bug:** Property name mismatch! `config.timeout` vs `options.timeOut` (capital O)

TypeScript catches this:

```typescript
interface Config {
    apiUrl: string;
    timeout: number;
    retries: number;
}

function makeRequest(url: string, options: Config): void {
    // ❌ Error: Property 'timeOut' does not exist on type 'Config'. 
    // Did you mean 'timeout'?
    console.log(`Fetching ${url} with timeout ${options.timeOut}`);
}
```
</details>

---

## Exercise 4: Benefits Analysis

List 3 scenarios in your own experience where TypeScript would have prevented a bug.

<details>
<summary>Example Scenarios</summary>

1. **Typo in property access** - Accessing `user.emial` instead of `user.email`
2. **Wrong argument order** - Calling `createRect(height, width)` instead of `createRect(width, height)`
3. **Null reference** - Accessing `data.results.map()` when API returned no results
</details>

---

## Quiz

1. What does TypeScript compile to?
   - [ ] Machine code
   - [x] JavaScript
   - [ ] WebAssembly
   - [ ] Bytecode

2. When are type errors detected in TypeScript?
   - [x] At compile time
   - [ ] At runtime
   - [ ] Never
   - [ ] Only in production

3. Is TypeScript a superset of JavaScript?
   - [x] Yes - all valid JS is valid TS
   - [ ] No - they are completely different languages

---

[← Back to Chapter](./README.md) | [Next Chapter →](../1.2-setup-configuration/README.md)
