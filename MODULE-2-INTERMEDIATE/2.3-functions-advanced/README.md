# Chapter 2.3: Advanced Functions

TypeScript provides powerful features for typing functions, including overloading, generics, and advanced parameter handling.

---

## Function Overloading

```typescript
// Overload signatures
function format(value: string): string;
function format(value: number): string;
function format(value: Date): string;

// Implementation
function format(value: string | number | Date): string {
    if (typeof value === "string") return value.trim();
    if (typeof value === "number") return value.toFixed(2);
    return value.toISOString();
}

format("hello");       // string
format(42);            // string
format(new Date());    // string
```

---

## Rest Parameters

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((acc, n) => acc + n, 0);
}

sum(1, 2, 3);        // 6
sum(10, 20, 30, 40); // 100

// With other parameters
function log(level: string, ...messages: unknown[]): void {
    console.log(`[${level}]`, ...messages);
}
```

---

## Typing `this`

```typescript
interface Deck {
    cards: string[];
    shuffle(this: Deck): void;
}

const deck: Deck = {
    cards: ["A", "K", "Q"],
    shuffle(this: Deck) {
        this.cards.sort(() => Math.random() - 0.5);
    }
};
```

---

## Callback Types

```typescript
type Callback<T> = (error: Error | null, result: T | null) => void;

function fetchData(url: string, callback: Callback<string>): void {
    fetch(url)
        .then(res => res.text())
        .then(data => callback(null, data))
        .catch(err => callback(err, null));
}
```

---

## Generic Functions

```typescript
function identity<T>(arg: T): T {
    return arg;
}

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const user = { name: "Alice", age: 30 };
getProperty(user, "name");  // string
```

---

## Summary

| Feature | Description |
|---------|-------------|
| Overloading | Multiple signatures |
| Rest Parameters | Variable args with `...` |
| `this` typing | Explicit `this` type |
| Generics | Type parameters |
| `Parameters<F>` | Extract param types |
| `ReturnType<F>` | Extract return type |

---

[← Previous](../2.2-classes/README.md) | [Next →](../2.4-generics-basics/README.md)
