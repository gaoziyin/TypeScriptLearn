# Chapter 3.1 Exercises

## Exercise 1: Generic Constraints

Create a function that only accepts objects with an `id` property:

<details>
<summary>Solution</summary>

```typescript
function findById<T extends { id: string | number }>(items: T[], id: T["id"]): T | undefined {
    return items.find(item => item.id === id);
}
```
</details>

---

## Exercise 2: Keyof Usage

Create a `pick` function that picks specific keys:

```typescript
pick({ a: 1, b: 2, c: 3 }, ["a", "c"]);  // { a: 1, c: 3 }
```

<details>
<summary>Solution</summary>

```typescript
function pick<T, K extends keyof T>(obj: T, keys: K[]): Pick<T, K> {
    const result = {} as Pick<T, K>;
    keys.forEach(key => { result[key] = obj[key]; });
    return result;
}
```
</details>

---

## Exercise 3: Deep Partial

Implement `DeepPartial<T>`:

<details>
<summary>Solution</summary>

```typescript
type DeepPartial<T> = {
    [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};
```
</details>

---

[← Back](./README.md) | [Next →](../3.2-conditional-types/README.md)
