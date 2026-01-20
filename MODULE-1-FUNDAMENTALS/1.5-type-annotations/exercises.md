# Chapter 1.5 Exercises

## Exercise 1: Add Missing Annotations

Add type annotations to this code to make it type-safe:

```typescript
function createProduct(name, price, inStock) {
    return {
        id: Math.random().toString(36).substr(2, 9),
        name,
        price,
        inStock,
        createdAt: new Date()
    };
}

const product = createProduct("Widget", 29.99, true);
```

<details>
<summary>Solution</summary>

```typescript
interface Product {
    id: string;
    name: string;
    price: number;
    inStock: boolean;
    createdAt: Date;
}

function createProduct(
    name: string, 
    price: number, 
    inStock: boolean
): Product {
    return {
        id: Math.random().toString(36).substr(2, 9),
        name,
        price,
        inStock,
        createdAt: new Date()
    };
}

const product: Product = createProduct("Widget", 29.99, true);
```
</details>

---

## Exercise 2: Function Types

Create proper type annotations for these callback patterns:

```typescript
// 1. A simple callback
function fetchData(callback) {
    setTimeout(() => callback({ data: [1, 2, 3] }), 1000);
}

// 2. An error-first callback
function readFile(path, callback) {
    // callback(error, data)
}

// 3. A reducer function
function reduce(arr, reducer, initial) {
    let result = initial;
    for (const item of arr) {
        result = reducer(result, item);
    }
    return result;
}
```

<details>
<summary>Solution</summary>

```typescript
// 1. Simple callback
interface FetchResult {
    data: number[];
}

function fetchData(callback: (result: FetchResult) => void): void {
    setTimeout(() => callback({ data: [1, 2, 3] }), 1000);
}

// 2. Error-first callback
function readFile(
    path: string, 
    callback: (error: Error | null, data: string | null) => void
): void {
    try {
        const data = "file contents";
        callback(null, data);
    } catch (error) {
        callback(error as Error, null);
    }
}

// 3. Generic reducer
function reduce<T, R>(
    arr: T[], 
    reducer: (accumulator: R, current: T) => R, 
    initial: R
): R {
    let result = initial;
    for (const item of arr) {
        result = reducer(result, item);
    }
    return result;
}

// Usage
const sum = reduce([1, 2, 3], (acc, n) => acc + n, 0);
```
</details>

---

## Exercise 3: Object Type Annotations

Create type annotations for this API response handler:

```typescript
const response = {
    status: 200,
    data: {
        users: [
            { id: 1, name: "Alice", email: "alice@example.com" },
            { id: 2, name: "Bob", email: "bob@example.com" }
        ],
        pagination: {
            page: 1,
            perPage: 10,
            total: 2
        }
    },
    headers: {
        "content-type": "application/json",
        "x-request-id": "abc123"
    }
};
```

<details>
<summary>Solution</summary>

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

interface Pagination {
    page: number;
    perPage: number;
    total: number;
}

interface ApiResponse {
    status: number;
    data: {
        users: User[];
        pagination: Pagination;
    };
    headers: {
        [key: string]: string;
    };
}

const response: ApiResponse = {
    status: 200,
    data: {
        users: [
            { id: 1, name: "Alice", email: "alice@example.com" },
            { id: 2, name: "Bob", email: "bob@example.com" }
        ],
        pagination: {
            page: 1,
            perPage: 10,
            total: 2
        }
    },
    headers: {
        "content-type": "application/json",
        "x-request-id": "abc123"
    }
};
```
</details>

---

## Exercise 4: Optional and Default Parameters

Refactor this function to use optional and default parameters properly:

```typescript
function createButton(text, onClick, disabled, className, style) {
    return {
        text: text || "Click me",
        onClick: onClick || (() => {}),
        disabled: disabled || false,
        className: className || "",
        style: style || {}
    };
}
```

<details>
<summary>Solution</summary>

```typescript
interface ButtonStyle {
    color?: string;
    backgroundColor?: string;
    fontSize?: string;
}

interface Button {
    text: string;
    onClick: () => void;
    disabled: boolean;
    className: string;
    style: ButtonStyle;
}

function createButton(
    text: string = "Click me",
    onClick: () => void = () => {},
    disabled: boolean = false,
    className: string = "",
    style: ButtonStyle = {}
): Button {
    return { text, onClick, disabled, className, style };
}

// Or with options object pattern (better for many optional params)
interface ButtonOptions {
    text?: string;
    onClick?: () => void;
    disabled?: boolean;
    className?: string;
    style?: ButtonStyle;
}

function createButtonV2(options: ButtonOptions = {}): Button {
    return {
        text: options.text ?? "Click me",
        onClick: options.onClick ?? (() => {}),
        disabled: options.disabled ?? false,
        className: options.className ?? "",
        style: options.style ?? {}
    };
}
```
</details>

---

## Exercise 5: Rest Parameters

Create a properly typed logging function:

```typescript
// Should support:
// log("info", "User logged in", "userId", 123)
// log("error", "Failed to fetch", "endpoint", "/api/users", "status", 404)
function log(level, message, ...context) {
    console.log(`[${level}] ${message}`, context);
}
```

<details>
<summary>Solution</summary>

```typescript
type LogLevel = "info" | "warn" | "error" | "debug";

function log(
    level: LogLevel, 
    message: string, 
    ...context: (string | number | boolean)[]
): void {
    const timestamp = new Date().toISOString();
    
    // Format context as key-value pairs
    const contextStr = [];
    for (let i = 0; i < context.length; i += 2) {
        if (context[i + 1] !== undefined) {
            contextStr.push(`${context[i]}=${context[i + 1]}`);
        }
    }
    
    console.log(`[${timestamp}] [${level.toUpperCase()}] ${message}`, 
                contextStr.length > 0 ? `{ ${contextStr.join(", ")} }` : "");
}

// Usage
log("info", "User logged in", "userId", 123);
log("error", "Failed to fetch", "endpoint", "/api/users", "status", 404);

// Even better: typed context object
interface LogContext {
    [key: string]: string | number | boolean | undefined;
}

function logV2(
    level: LogLevel, 
    message: string, 
    context?: LogContext
): void {
    console.log(`[${level}] ${message}`, context || "");
}

logV2("info", "User logged in", { userId: 123 });
```
</details>

---

## Quiz

1. Which syntax is correct for function parameter types?
   - [ ] `function add(a, b: number number)`
   - [x] `function add(a: number, b: number)`
   - [ ] `function add(number a, number b)`
   - [ ] `function add(a number, b number)`

2. How do you make a parameter optional?
   - [ ] `param = undefined`
   - [x] `param?: Type`
   - [ ] `optional param: Type`
   - [ ] `param: Type | undefined`

3. What's the difference between `?:` and `= value`?
   - [x] `?:` is optional (can be undefined), `= value` provides a default
   - [ ] They are the same
   - [ ] `?:` is for null, `= value` is for undefined
   - [ ] `= value` is optional, `?:` is required

4. When should you use explicit return type annotations?
   - [ ] Always
   - [ ] Never
   - [x] For public APIs and complex return types
   - [ ] Only for void functions

---

[← Back to Chapter](./README.md) | [Next Module →](../../MODULE-2-INTERMEDIATE/2.1-interfaces/README.md)
