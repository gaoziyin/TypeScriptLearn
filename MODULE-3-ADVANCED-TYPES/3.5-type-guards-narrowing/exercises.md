# Chapter 3.5 Exercises

## Exercise 1: Type Guard Functions

Create type guards for these types:

```typescript
type Admin = { role: "admin"; permissions: string[] };
type User = { role: "user"; name: string };

function isAdmin(person: Admin | User): person is Admin { ??? }
```

<details>
<summary>Solution</summary>

```typescript
function isAdmin(person: Admin | User): person is Admin {
    return person.role === "admin";
}
```
</details>

---

## Exercise 2: Assertion Function

Create an assertion that validates a configuration object:

```typescript
interface Config { host: string; port: number; }
function assertValidConfig(obj: unknown): asserts obj is Config { ??? }
```

<details>
<summary>Solution</summary>

```typescript
function assertValidConfig(obj: unknown): asserts obj is Config {
    if (typeof obj !== "object" || obj === null) {
        throw new Error("Config must be an object");
    }
    if (!("host" in obj) || typeof (obj as any).host !== "string") {
        throw new Error("Config must have string 'host'");
    }
    if (!("port" in obj) || typeof (obj as any).port !== "number") {
        throw new Error("Config must have number 'port'");
    }
}
```
</details>

---

[← Back](./README.md) | [Next Module →](../../MODULE-4-ADVANCED-PATTERNS/4.1-utility-types/README.md)
