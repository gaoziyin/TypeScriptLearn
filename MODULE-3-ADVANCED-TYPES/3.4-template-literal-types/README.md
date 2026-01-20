# Chapter 3.4: Template Literal Types

Create string literal types using template syntax.

---

## Basic Template Literals

```typescript
type Greeting = `Hello, ${string}`;
type ValidGreeting = "Hello, World";  // ✓ assignable to Greeting

// Combine with unions
type Color = "red" | "blue" | "green";
type Size = "sm" | "md" | "lg";

type ColorSize = `${Color}-${Size}`;
// "red-sm" | "red-md" | "red-lg" | "blue-sm" | ...
```

---

## String Manipulation Types

```typescript
type Upper = Uppercase<"hello">;      // "HELLO"
type Lower = Lowercase<"HELLO">;      // "hello"
type Capital = Capitalize<"hello">;   // "Hello"
type Uncap = Uncapitalize<"Hello">;   // "hello"

// Combine with templates
type EventName<T extends string> = `on${Capitalize<T>}`;
type ClickEvent = EventName<"click">;  // "onClick"
```

---

## Practical Patterns

```typescript
// CSS property pattern
type CSSValue = `${number}px` | `${number}em` | `${number}rem`;

// Route parameters
type Route = `/user/${string}/post/${string}`;
const valid: Route = "/user/123/post/456";  // ✓

// Event handler names
type EventHandlers<T extends string> = {
    [K in T as `on${Capitalize<K>}`]: () => void;
};

type Events = EventHandlers<"click" | "focus" | "blur">;
// { onClick: () => void; onFocus: () => void; onBlur: () => void }

// Getter/Setter pattern
type PropAccessors<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
} & {
    [K in keyof T as `set${Capitalize<string & K>}`]: (v: T[K]) => void;
};
```

---

## Pattern Matching with `infer`

```typescript
// Extract parts from string
type ExtractId<T> = T extends `user-${infer Id}` ? Id : never;
type UserId = ExtractId<"user-123">;  // "123"

// Parse route params
type ParseRoute<T> = T extends `/${infer Segment}/${infer Rest}`
    ? { segment: Segment } & ParseRoute<`/${Rest}`>
    : T extends `/${infer Last}`
    ? { last: Last }
    : {};
```

---

## Summary

| Intrinsic Type | Effect |
|----------------|--------|
| `Uppercase<S>` | Convert to uppercase |
| `Lowercase<S>` | Convert to lowercase |
| `Capitalize<S>` | Capitalize first letter |
| `Uncapitalize<S>` | Lowercase first letter |

---

[← Previous](../3.3-mapped-types/README.md) | [Next →](../3.5-type-guards-narrowing/README.md)
