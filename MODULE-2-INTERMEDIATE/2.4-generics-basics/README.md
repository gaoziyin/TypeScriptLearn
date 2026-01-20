# Chapter 2.4: Generics Basics

Generics enable writing reusable code that works with any type while maintaining type safety.

---

## Why Generics?

```typescript
// Without generics - loses type information
function identity(arg: any): any {
    return arg;
}

// With generics - preserves type
function identity<T>(arg: T): T {
    return arg;
}

identity<string>("hello");  // returns string
identity(42);               // inferred as number
```

---

## Generic Functions

```typescript
// Single type parameter
function first<T>(arr: T[]): T | undefined {
    return arr[0];
}

first([1, 2, 3]);        // number | undefined
first(["a", "b"]);       // string | undefined

// Multiple type parameters
function pair<T, U>(a: T, b: U): [T, U] {
    return [a, b];
}

pair("hello", 42);       // [string, number]
```

---

## Generic Interfaces

```typescript
interface Container<T> {
    value: T;
    getValue(): T;
}

interface Response<T> {
    data: T;
    status: number;
    message: string;
}

const userResponse: Response<{ name: string }> = {
    data: { name: "Alice" },
    status: 200,
    message: "OK"
};
```

---

## Generic Classes

```typescript
class Stack<T> {
    private items: T[] = [];
    
    push(item: T): void {
        this.items.push(item);
    }
    
    pop(): T | undefined {
        return this.items.pop();
    }
    
    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }
}

const numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
numberStack.pop();  // 2
```

---

## Generic Constraints

```typescript
// Constraint with extends
interface Lengthable {
    length: number;
}

function getLength<T extends Lengthable>(item: T): number {
    return item.length;
}

getLength("hello");     // 5
getLength([1, 2, 3]);   // 3
// getLength(123);      // ❌ Error

// Constraint with keyof
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
```

---

## Default Type Parameters

```typescript
interface ApiResponse<T = unknown> {
    data: T;
    error?: string;
}

const res1: ApiResponse<string> = { data: "hello" };
const res2: ApiResponse = { data: { anything: true } };
```

---

## Summary

| Concept | Syntax | Example |
|---------|--------|---------|
| Type Parameter | `<T>` | `function fn<T>()` |
| Multiple Params | `<T, U>` | `function fn<T, U>()` |
| Constraint | `<T extends X>` | `<T extends {length: number}>` |
| Default | `<T = X>` | `<T = string>` |
| keyof Constraint | `<K extends keyof T>` | Access object keys |

---

[← Previous](../2.3-functions-advanced/README.md) | [Next →](../2.5-union-intersection-types/README.md)
