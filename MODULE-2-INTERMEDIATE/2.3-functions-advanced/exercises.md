# Chapter 2.3 Exercises

## Exercise 1: Function Overloading

Create overloaded function `stringify`:

```typescript
// Should handle: string (return as-is), number (with decimals), object (JSON)
```

<details>
<summary>Solution</summary>

```typescript
function stringify(value: string): string;
function stringify(value: number): string;
function stringify(value: object): string;
function stringify(value: string | number | object): string {
    if (typeof value === "string") return value;
    if (typeof value === "number") return value.toFixed(2);
    return JSON.stringify(value);
}
```
</details>

---

## Exercise 2: Generic Function

Create a `pluck` function that extracts a property from an array of objects:

```typescript
const users = [{ name: "Alice" }, { name: "Bob" }];
pluck(users, "name"); // ["Alice", "Bob"]
```

<details>
<summary>Solution</summary>

```typescript
function pluck<T, K extends keyof T>(items: T[], key: K): T[K][] {
    return items.map(item => item[key]);
}
```
</details>

---

## Exercise 3: Callback Typing

Type this event emitter:

```typescript
class Emitter {
    on(event: string, callback: ???): void;
    emit(event: string, data: ???): void;
}
```

<details>
<summary>Solution</summary>

```typescript
type Listener<T = unknown> = (data: T) => void;

class Emitter<Events extends Record<string, unknown>> {
    private listeners = new Map<string, Listener[]>();
    
    on<K extends keyof Events>(event: K, callback: Listener<Events[K]>): void {
        const list = this.listeners.get(event as string) || [];
        list.push(callback as Listener);
        this.listeners.set(event as string, list);
    }
    
    emit<K extends keyof Events>(event: K, data: Events[K]): void {
        this.listeners.get(event as string)?.forEach(cb => cb(data));
    }
}
```
</details>

---

## Quiz

1. What is the purpose of function overloading?
   - [x] Multiple signatures for different input types
   - [ ] Running functions in parallel
   - [ ] Making functions faster

2. How do you type rest parameters?
   - [x] `...args: Type[]`
   - [ ] `args...: Type`
   - [ ] `rest args: Type`

---

[← Back](./README.md) | [Next →](../2.4-generics-basics/README.md)
