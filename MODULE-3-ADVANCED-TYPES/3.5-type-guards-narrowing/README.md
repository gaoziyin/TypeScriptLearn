# Chapter 3.5: Type Guards and Narrowing

Type guards help TypeScript narrow types based on runtime checks.

---

## typeof Guards

```typescript
function process(value: string | number) {
    if (typeof value === "string") {
        console.log(value.toUpperCase());  // value is string
    } else {
        console.log(value.toFixed(2));     // value is number
    }
}
```

---

## instanceof Guards

```typescript
class Dog { bark() {} }
class Cat { meow() {} }

function speak(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        animal.bark();  // animal is Dog
    } else {
        animal.meow();  // animal is Cat
    }
}
```

---

## `in` Operator

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
    if ("swim" in animal) {
        animal.swim();  // animal is Fish
    } else {
        animal.fly();   // animal is Bird
    }
}
```

---

## Custom Type Guards

```typescript
// Type predicate: parameterName is Type
function isString(value: unknown): value is string {
    return typeof value === "string";
}

function isUser(obj: unknown): obj is { name: string; age: number } {
    return typeof obj === "object" && obj !== null 
        && "name" in obj && "age" in obj;
}

function process(value: unknown) {
    if (isString(value)) {
        console.log(value.toUpperCase());  // value is string
    }
    if (isUser(value)) {
        console.log(value.name);  // value is User
    }
}
```

---

## Discriminated Unions

```typescript
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; side: number };
type Shape = Circle | Square;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.side ** 2;
    }
}
```

---

## Assertion Functions

```typescript
function assertIsString(value: unknown): asserts value is string {
    if (typeof value !== "string") {
        throw new Error("Not a string!");
    }
}

function process(value: unknown) {
    assertIsString(value);
    // After assertion, value is string
    console.log(value.toUpperCase());
}
```

---

## Control Flow Analysis

```typescript
function example(x: string | number | null) {
    if (x === null) {
        return;  // x is null, early return
    }
    // x is string | number
    
    if (typeof x === "string") {
        x;  // x is string
    } else {
        x;  // x is number
    }
}
```

---

## Summary

| Guard Type | Use Case |
|------------|----------|
| `typeof` | Primitives |
| `instanceof` | Class instances |
| `in` | Property existence |
| Type predicate | Custom logic |
| `asserts` | Throw on invalid |

---

[← Previous](../3.4-template-literal-types/README.md) | [Next Module →](../../MODULE-4-ADVANCED-PATTERNS/4.1-utility-types/README.md)
