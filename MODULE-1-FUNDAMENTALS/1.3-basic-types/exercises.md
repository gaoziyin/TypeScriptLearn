# Chapter 1.3 Exercises

## Exercise 1: Type Declaration Practice

Declare variables with the correct types for each value:

```typescript
// Declare the correct types
let username = "john_doe";
let itemCount = 42;
let isLoggedIn = false;
let tags = ["typescript", "javascript", "coding"];
let userProfile = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```

<details>
<summary>Solution</summary>

```typescript
let username: string = "john_doe";
let itemCount: number = 42;
let isLoggedIn: boolean = false;
let tags: string[] = ["typescript", "javascript", "coding"];
let userProfile: {
    id: number;
    name: string;
    email: string;
} = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```
</details>

---

## Exercise 2: Tuple Challenge

Create a tuple type for a database row that contains:
- ID (number)
- Name (string)
- Active status (boolean)
- Tags (array of strings)

```typescript
type DatabaseRow = // Your type here

const row: DatabaseRow = // Your value here
```

<details>
<summary>Solution</summary>

```typescript
type DatabaseRow = [number, string, boolean, string[]];

const row: DatabaseRow = [1, "Product A", true, ["featured", "new"]];

// With named tuples (more readable)
type NamedRow = [id: number, name: string, active: boolean, tags: string[]];
const namedRow: NamedRow = [2, "Product B", false, ["sale"]];
```
</details>

---

## Exercise 3: Enum Creation

Create an enum for HTTP methods (GET, POST, PUT, DELETE, PATCH) and use it in a function:

```typescript
// Create the enum

// Use it in this function
function makeRequest(method: /* Your enum */, url: string): void {
    console.log(`Making ${method} request to ${url}`);
}
```

<details>
<summary>Solution</summary>

```typescript
enum HttpMethod {
    GET = "GET",
    POST = "POST",
    PUT = "PUT",
    DELETE = "DELETE",
    PATCH = "PATCH"
}

function makeRequest(method: HttpMethod, url: string): void {
    console.log(`Making ${method} request to ${url}`);
}

// Usage
makeRequest(HttpMethod.GET, "/api/users");
makeRequest(HttpMethod.POST, "/api/users");
```
</details>

---

## Exercise 4: Unknown vs Any

Convert this unsafe `any` code to use `unknown` with proper type guards:

```typescript
function processData(data: any) {
    console.log(data.toUpperCase());  // Unsafe!
    console.log(data.length);          // Unsafe!
}
```

<details>
<summary>Solution</summary>

```typescript
function processData(data: unknown): void {
    // Type guard for string
    if (typeof data === "string") {
        console.log(data.toUpperCase());  // Safe!
        console.log(data.length);          // Safe!
    } else if (Array.isArray(data)) {
        console.log(data.length);          // Safe for arrays
    } else {
        console.log("Unsupported data type");
    }
}

// Test
processData("hello");      // HELLO, 5
processData([1, 2, 3]);    // 3
processData(42);           // Unsupported data type
```
</details>

---

## Exercise 5: Never Type

Complete the exhaustive switch statement using the `never` type:

```typescript
type Status = "pending" | "approved" | "rejected" | "cancelled";

function getStatusMessage(status: Status): string {
    switch (status) {
        case "pending":
            return "Your request is being processed";
        case "approved":
            return "Your request has been approved";
        // Add the remaining cases with exhaustive check
    }
}
```

<details>
<summary>Solution</summary>

```typescript
type Status = "pending" | "approved" | "rejected" | "cancelled";

function getStatusMessage(status: Status): string {
    switch (status) {
        case "pending":
            return "Your request is being processed";
        case "approved":
            return "Your request has been approved";
        case "rejected":
            return "Your request has been rejected";
        case "cancelled":
            return "Your request was cancelled";
        default:
            // Exhaustive check - if we add a new status but forget to handle it,
            // TypeScript will error
            const _exhaustiveCheck: never = status;
            return _exhaustiveCheck;
    }
}
```
</details>

---

## Exercise 6: Array and Object Types

Fix the type errors in this code:

```typescript
// Fix these declarations
const numbers = [1, 2, "3", 4];  // Should only be numbers
const user = {
    name: "Alice",
    age: 30
};
user.email = "alice@example.com";  // Error!
user.age = "thirty";               // Error!
```

<details>
<summary>Solution</summary>

```typescript
// Fixed declarations
const numbers: number[] = [1, 2, 3, 4];

interface User {
    name: string;
    age: number;
    email?: string;  // Optional property
}

const user: User = {
    name: "Alice",
    age: 30
};

user.email = "alice@example.com";  // Now works!
// user.age = "thirty";            // Still error - correct!
user.age = 31;                     // This is the correct way
```
</details>

---

## Quiz

1. What is the difference between `number[]` and `Array<number>`?
   - [x] They are equivalent
   - [ ] `number[]` is for primitives only
   - [ ] `Array<number>` is more performant
   - [ ] `number[]` doesn't support generics

2. Which type should you use instead of `any` for unknown data?
   - [ ] `void`
   - [ ] `never`
   - [x] `unknown`
   - [ ] `undefined`

3. What does a function with return type `never` do?
   - [ ] Returns `undefined`
   - [ ] Returns `null`
   - [x] Never returns (throws or infinite loop)
   - [ ] Returns an empty value

4. Which is correct for a tuple with name and age?
   - [ ] `(string, number)`
   - [x] `[string, number]`
   - [ ] `<string, number>`
   - [ ] `{string, number}`

5. What is the purpose of `const enum`?
   - [ ] Makes values constant
   - [x] Inlines enum values at compile time
   - [ ] Creates immutable objects
   - [ ] Disables reverse mapping

---

[← Back to Chapter](./README.md) | [Next Chapter →](../1.4-type-inference/README.md)
