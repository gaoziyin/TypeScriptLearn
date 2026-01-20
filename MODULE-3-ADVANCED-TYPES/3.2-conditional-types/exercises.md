# Chapter 3.2 Exercises

## Exercise 1: Create Conditional Types

```typescript
type IsArray<T> = ???;  // true if T is array
type GetArrayElement<T> = ???;  // element type of array
type GetFunctionReturn<T> = ???;  // return type of function
```

<details>
<summary>Solution</summary>

```typescript
type IsArray<T> = T extends any[] ? true : false;
type GetArrayElement<T> = T extends (infer E)[] ? E : never;
type GetFunctionReturn<T> = T extends (...args: any[]) => infer R ? R : never;
```
</details>

---

## Exercise 2: UnwrapPromise

Create a type that unwraps nested promises:

```typescript
type UnwrapPromise<T> = ???;
type Test = UnwrapPromise<Promise<Promise<string>>>;  // string
```

<details>
<summary>Solution</summary>

```typescript
type UnwrapPromise<T> = T extends Promise<infer U> ? UnwrapPromise<U> : T;
```
</details>

---

[← Back](./README.md) | [Next →](../3.3-mapped-types/README.md)
