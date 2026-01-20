# Chapter 2.5: Union and Intersection Types

Combine types to create more flexible and precise type definitions.

---

## Union Types (`|`)

A value can be one of several types:

```typescript
// Basic union
let id: string | number;
id = "abc";  // ✓
id = 123;    // ✓
// id = true; // ❌

// In functions
function format(input: string | number): string {
    if (typeof input === "string") {
        return input.toUpperCase();
    }
    return input.toFixed(2);
}

// Union of literals
type Status = "pending" | "approved" | "rejected";
type HttpCode = 200 | 400 | 404 | 500;
```

---

## Type Narrowing

TypeScript narrows types based on checks:

```typescript
function process(value: string | number | null) {
    // Type guard with typeof
    if (typeof value === "string") {
        console.log(value.length);  // value is string
    } else if (typeof value === "number") {
        console.log(value.toFixed(2));  // value is number
    } else {
        console.log("null");  // value is null
    }
}

// Truthiness check
function greet(name: string | undefined) {
    if (name) {
        console.log(`Hello, ${name}`);  // name is string
    }
}
```

---

## Discriminated Unions

Use a common property to discriminate between types:

```typescript
type Circle = { kind: "circle"; radius: number };
type Rectangle = { kind: "rectangle"; width: number; height: number };
type Shape = Circle | Rectangle;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "rectangle":
            return shape.width * shape.height;
    }
}
```

---

## Intersection Types (`&`)

Combine multiple types into one:

```typescript
type Person = { name: string; age: number };
type Employee = { employeeId: number; department: string };

type Staff = Person & Employee;

const staff: Staff = {
    name: "Alice",
    age: 30,
    employeeId: 12345,
    department: "Engineering"
};
```

---

## Union vs Intersection

```typescript
// Union: A OR B
type StringOrNumber = string | number;

// Intersection: A AND B
type Named = { name: string };
type Aged = { age: number };
type Person = Named & Aged;  // Must have both
```

---

## Practical Patterns

```typescript
// API Response
type Success<T> = { success: true; data: T };
type Failure = { success: false; error: string };
type Result<T> = Success<T> | Failure;

function handleResult<T>(result: Result<T>) {
    if (result.success) {
        console.log(result.data);  // TypeScript knows data exists
    } else {
        console.log(result.error);
    }
}

// Mixins
type Timestamped = { createdAt: Date; updatedAt: Date };
type Identifiable = { id: string };

type Entity = Timestamped & Identifiable;
```

---

## Summary

| Type | Syntax | Meaning |
|------|--------|---------|
| Union | `A \| B` | Either A or B |
| Intersection | `A & B` | Both A and B |
| Literal Union | `"a" \| "b"` | Specific values |
| Discriminated | `{kind: "x"}` | Tagged union |

---

[← Previous](../2.4-generics-basics/README.md) | [Next Module →](../../MODULE-3-ADVANCED-TYPES/3.1-advanced-generics/README.md)
