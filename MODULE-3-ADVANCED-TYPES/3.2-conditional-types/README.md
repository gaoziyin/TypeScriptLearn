# Chapter 3.2: Conditional Types

Conditional types enable type-level logic, similar to ternary expressions.

---

## Basic Syntax

```typescript
// Syntax: T extends U ? X : Y
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;   // true
type B = IsString<number>;   // false
type C = IsString<"hello">;  // true
```

---

## The `infer` Keyword

Extract types from other types:

```typescript
// Extract return type
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R : never;

type FnReturn = ReturnTypeOf<() => string>;  // string

// Extract array element type
type ElementOf<T> = T extends (infer E)[] ? E : never;

type Elem = ElementOf<string[]>;  // string

// Extract Promise value
type Awaited<T> = T extends Promise<infer U> ? U : T;

type Value = Awaited<Promise<number>>;  // number
```

---

## Distributive Conditional Types

Conditional types distribute over unions:

```typescript
type ToArray<T> = T extends any ? T[] : never;

// Distributes over union
type Result = ToArray<string | number>;
// = ToArray<string> | ToArray<number>
// = string[] | number[]

// Prevent distribution with [T]
type ToArrayNonDist<T> = [T] extends [any] ? T[] : never;
type Result2 = ToArrayNonDist<string | number>;
// = (string | number)[]
```

---

## Practical Examples

```typescript
// Exclude null/undefined
type NonNullable<T> = T extends null | undefined ? never : T;

// Extract function parameters
type Parameters<T> = T extends (...args: infer P) => any ? P : never;

// Flatten type
type Flatten<T> = T extends (infer U)[] ? U : T;

// Extract object value types
type ValueOf<T> = T[keyof T];
```

---

## Summary

| Pattern | Purpose |
|---------|---------|
| `T extends U ? X : Y` | Conditional check |
| `infer` | Extract/capture type |
| Distribution | Applies to each union member |
| `[T] extends [U]` | Prevent distribution |

---

[← Previous](../3.1-advanced-generics/README.md) | [Next →](../3.3-mapped-types/README.md)
