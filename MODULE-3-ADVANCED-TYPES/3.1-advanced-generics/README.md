# Chapter 3.1: Advanced Generics

Deep dive into powerful generic patterns and constraints.

---

## Generic Constraints with `extends`

```typescript
// Basic constraint
function getLength<T extends { length: number }>(item: T): number {
    return item.length;
}

// Multiple constraints (intersection)
function merge<T extends object, U extends object>(a: T, b: U): T & U {
    return { ...a, ...b };
}
```

---

## `keyof` and Indexed Access

```typescript
type User = { name: string; age: number; email: string };

// keyof creates union of keys
type UserKeys = keyof User;  // "name" | "age" | "email"

// Indexed access type
type NameType = User["name"];       // string
type AgeType = User["age"];         // number
type AllTypes = User[keyof User];   // string | number

// In functions
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const user = { name: "Alice", age: 30 };
getProperty(user, "name");  // string
getProperty(user, "age");   // number
```

---

## Generic Utility Patterns

```typescript
// Make all properties optional
type DeepPartial<T> = {
    [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Make specific properties required
type RequireKeys<T, K extends keyof T> = T & Required<Pick<T, K>>;

// Make specific properties optional
type OptionalKeys<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>;

// Readonly deep
type DeepReadonly<T> = {
    readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};
```

---

## Variance in Generics

```typescript
// Covariance (output position) - Array<Dog> assignable to Array<Animal>
// Contravariance (input position) - (animal: Animal) => void assignable to (dog: Dog) => void

interface Animal { name: string }
interface Dog extends Animal { breed: string }

// Covariant
const dogs: Dog[] = [{ name: "Buddy", breed: "Lab" }];
const animals: Animal[] = dogs;  // ✓ OK

// Contravariant (function parameters)
type AnimalHandler = (animal: Animal) => void;
type DogHandler = (dog: Dog) => void;

const handleAnimal: AnimalHandler = (a) => console.log(a.name);
// const handleDog: DogHandler = handleAnimal;  // ✓ Works
```

---

## Mapped Type Generic Patterns

```typescript
// Convert all properties to getters
type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type User = { name: string; age: number };
type UserGetters = Getters<User>;
// { getName: () => string; getAge: () => number }
```

---

## Summary

| Pattern | Description |
|---------|-------------|
| `T extends X` | Constrain T to type X |
| `keyof T` | Union of T's keys |
| `T[K]` | Type of property K in T |
| `in` | Iterate over keys |
| `as` | Remap keys |

---

[← Previous Module](../../MODULE-2-INTERMEDIATE/2.5-union-intersection-types/README.md) | [Next →](../3.2-conditional-types/README.md)
