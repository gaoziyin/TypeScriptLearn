# Chapter 4.1 Exercises

## Exercise 1: Combine Utilities

Create a type that makes only specific properties optional:

```typescript
type PartialBy<T, K extends keyof T> = ???;
// PartialBy<{ a: 1; b: 2; c: 3 }, "a" | "b"> = { a?: 1; b?: 2; c: 3 }
```

<details>
<summary>Solution</summary>

```typescript
type PartialBy<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;
```
</details>

---

## Exercise 2: Deep Readonly

Create `DeepReadonly<T>`:

<details>
<summary>Solution</summary>

```typescript
type DeepReadonly<T> = {
    readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};
```
</details>

---

[← Back](./README.md) | [Next →](../4.2-declaration-files/README.md)
