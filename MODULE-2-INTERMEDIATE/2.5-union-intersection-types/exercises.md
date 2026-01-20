# Chapter 2.5 Exercises

## Exercise 1: Discriminated Unions

Create a discriminated union for payment methods:

```typescript
// CreditCard: type, cardNumber, cvv
// PayPal: type, email
// BankTransfer: type, accountNumber, routingNumber
```

<details>
<summary>Solution</summary>

```typescript
type CreditCard = { type: "credit"; cardNumber: string; cvv: string };
type PayPal = { type: "paypal"; email: string };
type BankTransfer = { type: "bank"; accountNumber: string; routingNumber: string };
type PaymentMethod = CreditCard | PayPal | BankTransfer;

function processPayment(method: PaymentMethod) {
    switch (method.type) {
        case "credit": return `Card: ${method.cardNumber}`;
        case "paypal": return `PayPal: ${method.email}`;
        case "bank": return `Bank: ${method.accountNumber}`;
    }
}
```
</details>

---

## Exercise 2: Intersection Types

Create a type for an entity with audit fields:

```typescript
// Combine: Identifiable, Timestamped, and specific entity fields
```

<details>
<summary>Solution</summary>

```typescript
type Identifiable = { id: string };
type Timestamped = { createdAt: Date; updatedAt: Date };
type UserFields = { name: string; email: string };

type User = Identifiable & Timestamped & UserFields;

const user: User = {
    id: "1",
    name: "Alice",
    email: "alice@example.com",
    createdAt: new Date(),
    updatedAt: new Date()
};
```
</details>

---

## Exercise 3: Type Guards

Write a type guard function:

```typescript
function isString(value: unknown): value is string;
```

<details>
<summary>Solution</summary>

```typescript
function isString(value: unknown): value is string {
    return typeof value === "string";
}

function process(value: unknown) {
    if (isString(value)) {
        console.log(value.toUpperCase());  // value is string
    }
}
```
</details>

---

## Quiz

1. What does `string | number` mean?
   - [x] Value can be string or number
   - [ ] Value is both string and number
   
2. What does `A & B` create?
   - [ ] Either A or B
   - [x] Type with all properties from both

---

[← Back](./README.md) | [Next Module →](../../MODULE-3-ADVANCED-TYPES/3.1-advanced-generics/README.md)
