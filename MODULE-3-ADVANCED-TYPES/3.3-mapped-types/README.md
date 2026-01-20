# Chapter 3.3: Mapped Types

Mapped types transform existing types by iterating over their properties.

---

## Basic Mapped Types

```typescript
// Make all properties optional
type Partial<T> = {
    [P in keyof T]?: T[P];
};

// Make all properties required
type Required<T> = {
    [P in keyof T]-?: T[P];
};

// Make all properties readonly
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

// Remove readonly
type Mutable<T> = {
    -readonly [P in keyof T]: T[P];
};
```

---

## Key Remapping with `as`

```typescript
// Rename keys
type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type User = { name: string; age: number };
type UserGetters = Getters<User>;
// { getName: () => string; getAge: () => number }

// Filter keys
type OnlyStrings<T> = {
    [K in keyof T as T[K] extends string ? K : never]: T[K];
};
```

---

## Built-in Mapped Types

```typescript
type User = { id: number; name: string; age: number };

// Pick specific properties
type UserName = Pick<User, "name">;  // { name: string }

// Omit specific properties
type UserWithoutId = Omit<User, "id">;  // { name: string; age: number }

// Create object type with specific keys
type Config = Record<"host" | "port", string>;
// { host: string; port: string }
```

---

## Custom Mapped Types

```typescript
// Nullable version
type Nullable<T> = {
    [P in keyof T]: T[P] | null;
};

// Deep readonly
type DeepReadonly<T> = {
    readonly [P in keyof T]: T[P] extends object
        ? DeepReadonly<T[P]>
        : T[P];
};

// Property to getter/setter
type Accessors<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
} & {
    [K in keyof T as `set${Capitalize<string & K>}`]: (value: T[K]) => void;
};
```

---

## Summary

| Modifier | Effect |
|----------|--------|
| `?` | Make optional |
| `-?` | Make required |
| `readonly` | Make readonly |
| `-readonly` | Remove readonly |
| `as` | Remap key |
| `never` | Filter out |

---

[← Previous](../3.2-conditional-types/README.md) | [Next →](../3.4-template-literal-types/README.md)
