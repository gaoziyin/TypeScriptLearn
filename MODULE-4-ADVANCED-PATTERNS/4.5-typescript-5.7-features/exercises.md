# Chapter 4.5 Exercises

## Exercise 1: ES2024 Features

Use `Object.groupBy` to group users by role:

```typescript
const users = [
    { name: "Alice", role: "admin" },
    { name: "Bob", role: "user" },
    { name: "Charlie", role: "admin" }
];
```

<details>
<summary>Solution</summary>

```typescript
const grouped = Object.groupBy(users, user => user.role);
// { admin: [{name: "Alice"}, {name: "Charlie"}], user: [{name: "Bob"}] }
```
</details>

---

## Exercise 2: Promise.withResolvers

Create a deferred promise pattern:

<details>
<summary>Solution</summary>

```typescript
function createDeferred<T>() {
    return Promise.withResolvers<T>();
}

const { promise, resolve, reject } = createDeferred<string>();

// Can resolve/reject from anywhere
setTimeout(() => resolve("Success!"), 1000);

promise.then(console.log);
```
</details>

---

[← Back](./README.md) | [Next Module →](../../MODULE-5-REAL-WORLD/5.1-typescript-react/README.md)
