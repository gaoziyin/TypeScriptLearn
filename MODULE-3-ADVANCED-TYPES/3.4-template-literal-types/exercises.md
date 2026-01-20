# Chapter 3.4 Exercises

## Exercise 1: Create CSS Types

```typescript
type CSSLength = ???;  // "10px", "2em", "1.5rem"
type CSSColor = ???;   // "#fff", "rgb(0,0,0)"
```

<details>
<summary>Solution</summary>

```typescript
type CSSUnit = "px" | "em" | "rem" | "%";
type CSSLength = `${number}${CSSUnit}`;

type HexDigit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | "a" | "b" | "c" | "d" | "e" | "f";
type CSSColor = `#${string}` | `rgb(${number},${number},${number})`;
```
</details>

---

## Exercise 2: Event Types

Create typed event handler mapping:

```typescript
type DOMEvents = "click" | "focus" | "blur" | "submit";
// Create: { onClick, onFocus, onBlur, onSubmit }
```

<details>
<summary>Solution</summary>

```typescript
type EventHandlers<T extends string> = {
    [K in T as `on${Capitalize<K>}`]: (event: Event) => void;
};
type Handlers = EventHandlers<DOMEvents>;
```
</details>

---

[← Back](./README.md) | [Next →](../3.5-type-guards-narrowing/README.md)
