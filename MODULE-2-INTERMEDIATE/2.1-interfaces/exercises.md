# Chapter 2.1 Exercises

## Exercise 1: Define an Interface

Create interfaces for a blog post system:

```typescript
// Define interfaces for:
// 1. Author (id, name, email, bio?)
// 2. Post (id, title, content, author, tags, publishedAt?, updatedAt)
// 3. Comment (id, author, content, createdAt)
```

<details>
<summary>Solution</summary>

```typescript
interface Author {
    id: number;
    name: string;
    email: string;
    bio?: string;
}

interface Post {
    id: number;
    title: string;
    content: string;
    author: Author;
    tags: string[];
    publishedAt?: Date;
    updatedAt: Date;
}

interface Comment {
    id: number;
    author: Author;
    content: string;
    createdAt: Date;
}

// Extended version with comments
interface PostWithComments extends Post {
    comments: Comment[];
}
```
</details>

---

## Exercise 2: Readonly and Optional

Fix this interface to prevent mutation and make appropriate fields optional:

```typescript
interface UserProfile {
    id: number;           // Should not be changeable
    username: string;     // Should not be changeable
    displayName: string;  // Can be changed
    avatar: string;       // Optional, can be changed
    createdAt: Date;      // Should not be changeable
}

// After creation, only displayName and avatar should be changeable
```

<details>
<summary>Solution</summary>

```typescript
interface UserProfile {
    readonly id: number;
    readonly username: string;
    displayName: string;
    avatar?: string;
    readonly createdAt: Date;
}

// Usage
const profile: UserProfile = {
    id: 1,
    username: "alice",
    displayName: "Alice",
    createdAt: new Date()
};

profile.displayName = "Alice Smith";  // ✓ OK
profile.avatar = "avatar.png";        // ✓ OK
// profile.id = 2;                    // ❌ Error
// profile.username = "bob";          // ❌ Error
```
</details>

---

## Exercise 3: Interface Extension

Create a hierarchy of shapes using interface extension:

```typescript
// Create interfaces for:
// - Shape (base: has color)
// - Circle (extends Shape: has radius)
// - Rectangle (extends Shape: has width, height)
// - Square (extends ???: has side)
```

<details>
<summary>Solution</summary>

```typescript
interface Shape {
    color: string;
    getArea(): number;
}

interface Circle extends Shape {
    radius: number;
}

interface Rectangle extends Shape {
    width: number;
    height: number;
}

// Square extends Rectangle since it's a special case
interface Square extends Shape {
    side: number;
}

// Implementation
const circle: Circle = {
    color: "red",
    radius: 5,
    getArea() {
        return Math.PI * this.radius ** 2;
    }
};

const rectangle: Rectangle = {
    color: "blue",
    width: 10,
    height: 5,
    getArea() {
        return this.width * this.height;
    }
};

const square: Square = {
    color: "green",
    side: 4,
    getArea() {
        return this.side ** 2;
    }
};
```
</details>

---

## Exercise 4: Index Signatures

Create an interface for a theme configuration that allows dynamic CSS properties:

```typescript
// Should allow:
// {
//     primary: "#3178c6",
//     secondary: "#f7df1e",
//     fontSize: "16px",
//     fontFamily: "Arial",
//     customProperty123: "value"
// }
```

<details>
<summary>Solution</summary>

```typescript
interface Theme {
    primary: string;
    secondary: string;
    [cssProperty: string]: string;  // Allow any additional string properties
}

const theme: Theme = {
    primary: "#3178c6",
    secondary: "#f7df1e",
    fontSize: "16px",
    fontFamily: "Arial",
    customProperty123: "value"
};

// More specific version
interface SpecificTheme {
    colors: {
        primary: string;
        secondary: string;
        [colorName: string]: string;
    };
    fonts: {
        heading: string;
        body: string;
    };
    [key: string]: unknown;
}
```
</details>

---

## Exercise 5: Interface vs Type

Convert between interface and type, noting what's possible:

```typescript
// Convert this interface to a type alias
interface User {
    name: string;
    age: number;
}

interface Admin extends User {
    permissions: string[];
}

// Convert this type to an interface
type Status = "active" | "inactive" | "pending";
type ID = string | number;
type Pair<T> = [T, T];
```

<details>
<summary>Solution</summary>

```typescript
// Interface to Type
type UserType = {
    name: string;
    age: number;
};

type AdminType = UserType & {
    permissions: string[];
};

// Type to Interface - Status, ID, Pair CANNOT be interfaces!
// Unions and tuples can only be type aliases

// What you CAN do:
interface UserWithStatus {
    name: string;
    status: "active" | "inactive" | "pending";  // Inline union in interface
}

// But you can't define the union itself as an interface
// type Status = "active" | "inactive" | "pending";  // Must stay as type

// Summary:
// ✓ Object shapes: Can be either interface or type
// ✗ Unions, tuples, primitives: Must be type
```
</details>

---

## Exercise 6: Callable Interfaces

Create an interface for a validation function:

```typescript
// The validator should:
// - Be callable: validator(value) returns boolean
// - Have a 'message' property for error messages
// - Have an 'isRequired' property (boolean)
// - Have a 'setMessage' method
```

<details>
<summary>Solution</summary>

```typescript
interface Validator<T> {
    // Callable signature
    (value: T): boolean;
    
    // Properties
    message: string;
    isRequired: boolean;
    
    // Method
    setMessage(msg: string): void;
}

// Factory function to create validators
function createValidator<T>(
    predicate: (value: T) => boolean,
    message: string,
    isRequired: boolean = false
): Validator<T> {
    const validator = predicate as Validator<T>;
    validator.message = message;
    validator.isRequired = isRequired;
    validator.setMessage = (msg: string) => {
        validator.message = msg;
    };
    return validator;
}

// Usage
const isEmail: Validator<string> = createValidator(
    (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
    "Invalid email format",
    true
);

console.log(isEmail("test@example.com"));  // true
console.log(isEmail.message);               // "Invalid email format"
console.log(isEmail.isRequired);            // true
```
</details>

---

## Quiz

1. What keyword is used to inherit from an interface?
   - [ ] `implements`
   - [x] `extends`
   - [ ] `inherits`
   - [ ] `derives`

2. Can interfaces be merged in TypeScript?
   - [x] Yes, interfaces with the same name are automatically merged
   - [ ] No, duplicate interfaces cause errors
   - [ ] Only in modules
   - [ ] Only with a special keyword

3. How do you make an interface property immutable?
   - [ ] `const property: type`
   - [ ] `immutable property: type`
   - [x] `readonly property: type`
   - [ ] `final property: type`

4. Which can define a union type?
   - [ ] Only interface
   - [x] Only type alias
   - [ ] Both interface and type
   - [ ] Neither

---

[← Back to Chapter](./README.md) | [Next Chapter →](../2.2-classes/README.md)
