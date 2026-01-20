# Chapter 4.1: Utility Types

TypeScript provides built-in utility types for common type transformations.

---

## Property Modifiers

```typescript
// Make all properties optional
type Partial<T> = { [P in keyof T]?: T[P] };

// Make all properties required
type Required<T> = { [P in keyof T]-?: T[P] };

// Make all properties readonly
type Readonly<T> = { readonly [P in keyof T]: T[P] };

// Usage
interface User { name: string; age: number; email?: string }
type OptionalUser = Partial<User>;     // all optional
type StrictUser = Required<User>;       // all required
type FrozenUser = Readonly<User>;       // all readonly
```

---

## Property Selection

```typescript
// Pick specific properties
type Pick<T, K extends keyof T> = { [P in K]: T[P] };

// Omit specific properties
type Omit<T, K> = Pick<T, Exclude<keyof T, K>>;

// Usage
type UserName = Pick<User, "name">;           // { name }
type UserWithoutEmail = Omit<User, "email">;  // { name, age }
```

---

## Object Creation

```typescript
// Create object type from key union and value type
type Record<K extends keyof any, T> = { [P in K]: T };

// Usage
type StringMap = Record<string, string>;
type StatusCodes = Record<"ok" | "error", number>;
```

---

## Union Manipulation

```typescript
// Remove types from union
type Exclude<T, U> = T extends U ? never : T;

// Extract types from union
type Extract<T, U> = T extends U ? T : never;

// Remove null and undefined
type NonNullable<T> = T extends null | undefined ? never : T;

// Usage
type Numbers = Exclude<string | number, string>;  // number
type Strings = Extract<string | number, string>;  // string
type Defined = NonNullable<string | null>;         // string
```

---

## Function Utilities

```typescript
// Get parameter types
type Parameters<T> = T extends (...args: infer P) => any ? P : never;

// Get return type
type ReturnType<T> = T extends (...args: any) => infer R ? R : any;

// Get constructor parameters
type ConstructorParameters<T> = T extends new (...args: infer P) => any ? P : never;

// Get instance type
type InstanceType<T> = T extends new (...args: any) => infer R ? R : any;

// Usage
function greet(name: string, age: number): string { return ""; }
type Params = Parameters<typeof greet>;      // [string, number]
type Return = ReturnType<typeof greet>;      // string
```

---

## Summary

| Utility | Purpose |
|---------|---------|
| `Partial<T>` | All props optional |
| `Required<T>` | All props required |
| `Readonly<T>` | All props readonly |
| `Pick<T, K>` | Select specific props |
| `Omit<T, K>` | Remove specific props |
| `Record<K, V>` | Create object type |
| `Exclude<T, U>` | Remove from union |
| `Extract<T, U>` | Keep from union |
| `NonNullable<T>` | Remove null/undefined |
| `Parameters<F>` | Function param types |
| `ReturnType<F>` | Function return type |

---

[← Previous Module](../../MODULE-3-ADVANCED-TYPES/3.5-type-guards-narrowing/README.md) | [Next →](../4.2-declaration-files/README.md)
