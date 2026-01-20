# Chapter 3.3 Exercises

## Exercise 1: Create Mapped Types

```typescript
type Nullable<T> = ???;  // All props can be null
type Required<T> = ???;  // All props required
type StringKeys<T> = ???; // Only string-valued props
```

<details>
<summary>Solution</summary>

```typescript
type Nullable<T> = { [P in keyof T]: T[P] | null };
type Required<T> = { [P in keyof T]-?: T[P] };
type StringKeys<T> = { [K in keyof T as T[K] extends string ? K : never]: T[K] };
```
</details>

---

## Exercise 2: Prefix Keys

Add "on" prefix to event handlers:

```typescript
type Events = { click: () => void; change: (e: Event) => void };
// Result: { onClick: () => void; onChange: (e: Event) => void }
```

<details>
<summary>Solution</summary>

```typescript
type Handlers<T> = {
    [K in keyof T as `on${Capitalize<string & K>}`]: T[K];
};
```
</details>

---

[← Back](./README.md) | [Next →](../3.4-template-literal-types/README.md)
