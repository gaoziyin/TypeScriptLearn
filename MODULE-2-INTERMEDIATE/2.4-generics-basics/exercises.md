# Chapter 2.4 Exercises

## Exercise 1: Generic Array Utilities

Create generic functions:

```typescript
function last<T>(arr: T[]): T | undefined;
function unique<T>(arr: T[]): T[];
function flatten<T>(arr: T[][]): T[];
```

<details>
<summary>Solution</summary>

```typescript
function last<T>(arr: T[]): T | undefined {
    return arr[arr.length - 1];
}

function unique<T>(arr: T[]): T[] {
    return [...new Set(arr)];
}

function flatten<T>(arr: T[][]): T[] {
    return arr.flat();
}
```
</details>

---

## Exercise 2: Generic Class

Create a `Queue<T>` class:

```typescript
// enqueue(item): void
// dequeue(): T | undefined
// isEmpty(): boolean
```

<details>
<summary>Solution</summary>

```typescript
class Queue<T> {
    private items: T[] = [];
    
    enqueue(item: T): void {
        this.items.push(item);
    }
    
    dequeue(): T | undefined {
        return this.items.shift();
    }
    
    isEmpty(): boolean {
        return this.items.length === 0;
    }
}
```
</details>

---

## Exercise 3: Constrained Generic

Create `merge` that only works with objects:

```typescript
merge({ a: 1 }, { b: 2 }); // { a: 1, b: 2 }
```

<details>
<summary>Solution</summary>

```typescript
function merge<T extends object, U extends object>(a: T, b: U): T & U {
    return { ...a, ...b };
}
```
</details>

---

## Quiz

1. What does `<T extends { length: number }>` mean?
   - [x] T must have a length property
   - [ ] T must be a number
   - [ ] T extends another class

2. Can you have default type parameters?
   - [x] Yes, with `<T = DefaultType>`
   - [ ] No

---

[← Back](./README.md) | [Next →](../2.5-union-intersection-types/README.md)
