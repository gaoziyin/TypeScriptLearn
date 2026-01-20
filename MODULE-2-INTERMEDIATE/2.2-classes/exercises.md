# Chapter 2.2 Exercises

## Exercise 1: Create a Class

Create a `Product` class with the following requirements:
- Properties: id (readonly), name, price, quantity
- Constructor to initialize all properties
- Method `getTotal()` returns price * quantity
- Method `isInStock()` returns boolean

```typescript
// Your implementation here
```

<details>
<summary>Solution</summary>

```typescript
class Product {
    constructor(
        public readonly id: string,
        public name: string,
        public price: number,
        public quantity: number
    ) {}
    
    getTotal(): number {
        return this.price * this.quantity;
    }
    
    isInStock(): boolean {
        return this.quantity > 0;
    }
}

const product = new Product("P001", "Widget", 29.99, 10);
console.log(product.getTotal());   // 299.9
console.log(product.isInStock());  // true
```
</details>

---

## Exercise 2: Access Modifiers

Fix this class to properly encapsulate the password:

```typescript
class User {
    username: string;
    password: string;  // Should be private!
    
    constructor(username: string, password: string) {
        this.username = username;
        this.password = password;
    }
    
    // Add methods to:
    // - Check if password matches (checkPassword)
    // - Change password (only if old password is correct)
}
```

<details>
<summary>Solution</summary>

```typescript
class User {
    public readonly username: string;
    private password: string;
    
    constructor(username: string, password: string) {
        this.username = username;
        this.password = password;
    }
    
    checkPassword(attempt: string): boolean {
        return this.password === attempt;
    }
    
    changePassword(oldPassword: string, newPassword: string): boolean {
        if (this.checkPassword(oldPassword)) {
            this.password = newPassword;
            return true;
        }
        return false;
    }
}

const user = new User("alice", "secret123");
console.log(user.checkPassword("wrong"));     // false
console.log(user.checkPassword("secret123")); // true
console.log(user.changePassword("secret123", "newSecret")); // true
// console.log(user.password);  // ❌ Error: 'password' is private
```
</details>

---

## Exercise 3: Inheritance

Create a class hierarchy for a game:

```typescript
// Create:
// 1. Character (base): name, health, attack()
// 2. Warrior (extends Character): armor, defend()
// 3. Mage (extends Character): mana, castSpell()
```

<details>
<summary>Solution</summary>

```typescript
class Character {
    constructor(
        public name: string,
        protected health: number = 100
    ) {}
    
    attack(target: Character): void {
        console.log(`${this.name} attacks ${target.name}!`);
        target.takeDamage(10);
    }
    
    takeDamage(amount: number): void {
        this.health -= amount;
        console.log(`${this.name} takes ${amount} damage. Health: ${this.health}`);
    }
    
    isAlive(): boolean {
        return this.health > 0;
    }
}

class Warrior extends Character {
    constructor(
        name: string,
        public armor: number = 20
    ) {
        super(name, 150);  // Warriors have more health
    }
    
    // Override to reduce damage
    takeDamage(amount: number): void {
        const reducedDamage = Math.max(0, amount - this.armor / 4);
        super.takeDamage(reducedDamage);
    }
    
    defend(): void {
        this.armor += 5;
        console.log(`${this.name} raises shield! Armor: ${this.armor}`);
    }
}

class Mage extends Character {
    constructor(
        name: string,
        public mana: number = 100
    ) {
        super(name, 80);  // Mages have less health
    }
    
    castSpell(target: Character): void {
        if (this.mana >= 20) {
            this.mana -= 20;
            console.log(`${this.name} casts fireball at ${target.name}!`);
            target.takeDamage(25);
        } else {
            console.log(`${this.name} is out of mana!`);
        }
    }
}

// Usage
const warrior = new Warrior("Thorin");
const mage = new Mage("Gandalf");

mage.castSpell(warrior);  // High damage
warrior.defend();
warrior.attack(mage);
```
</details>

---

## Exercise 4: Abstract Class

Create an abstract `PaymentProcessor` class:

```typescript
// Abstract class with:
// - Abstract method: processPayment(amount: number): boolean
// - Abstract method: refund(transactionId: string): boolean
// - Concrete method: validateAmount(amount: number): boolean

// Implement two concrete classes:
// - CreditCardProcessor
// - PayPalProcessor
```

<details>
<summary>Solution</summary>

```typescript
abstract class PaymentProcessor {
    constructor(protected merchantId: string) {}
    
    abstract processPayment(amount: number): Promise<string>;
    abstract refund(transactionId: string): Promise<boolean>;
    
    validateAmount(amount: number): boolean {
        return amount > 0 && amount < 10000;
    }
    
    protected generateTransactionId(): string {
        return `TX-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`;
    }
}

class CreditCardProcessor extends PaymentProcessor {
    constructor(
        merchantId: string,
        private apiKey: string
    ) {
        super(merchantId);
    }
    
    async processPayment(amount: number): Promise<string> {
        if (!this.validateAmount(amount)) {
            throw new Error("Invalid amount");
        }
        const txId = this.generateTransactionId();
        console.log(`Credit card: Processing $${amount} via ${this.merchantId}`);
        return txId;
    }
    
    async refund(transactionId: string): Promise<boolean> {
        console.log(`Credit card: Refunding ${transactionId}`);
        return true;
    }
}

class PayPalProcessor extends PaymentProcessor {
    constructor(
        merchantId: string,
        private clientId: string,
        private clientSecret: string
    ) {
        super(merchantId);
    }
    
    async processPayment(amount: number): Promise<string> {
        if (!this.validateAmount(amount)) {
            throw new Error("Invalid amount");
        }
        const txId = this.generateTransactionId();
        console.log(`PayPal: Processing $${amount} via ${this.merchantId}`);
        return txId;
    }
    
    async refund(transactionId: string): Promise<boolean> {
        console.log(`PayPal: Refunding ${transactionId}`);
        return true;
    }
}
```
</details>

---

## Exercise 5: Getters and Setters

Create a `Rectangle` class with validation:

```typescript
// Requirements:
// - width and height must be positive
// - Getter for area (calculated)
// - Getter for perimeter (calculated)
// - Setter for dimensions that validates
```

<details>
<summary>Solution</summary>

```typescript
class Rectangle {
    private _width: number = 1;
    private _height: number = 1;
    
    constructor(width: number, height: number) {
        this.width = width;   // Use setters for validation
        this.height = height;
    }
    
    get width(): number {
        return this._width;
    }
    
    set width(value: number) {
        if (value <= 0) {
            throw new Error("Width must be positive");
        }
        this._width = value;
    }
    
    get height(): number {
        return this._height;
    }
    
    set height(value: number) {
        if (value <= 0) {
            throw new Error("Height must be positive");
        }
        this._height = value;
    }
    
    get area(): number {
        return this._width * this._height;
    }
    
    get perimeter(): number {
        return 2 * (this._width + this._height);
    }
    
    // Method to set both dimensions
    setDimensions(width: number, height: number): void {
        this.width = width;
        this.height = height;
    }
}

const rect = new Rectangle(10, 5);
console.log(rect.area);       // 50
console.log(rect.perimeter);  // 30

rect.width = 20;
console.log(rect.area);       // 100

// rect.width = -5;  // ❌ Error: Width must be positive
```
</details>

---

## Exercise 6: Implement Interfaces

Create a class that implements multiple interfaces:

```typescript
interface Identifiable {
    id: string;
}

interface Timestamped {
    createdAt: Date;
    updatedAt: Date;
}

interface Serializable {
    toJSON(): string;
    fromJSON(json: string): void;
}

// Create a Document class that implements all three
```

<details>
<summary>Solution</summary>

```typescript
interface Identifiable {
    id: string;
}

interface Timestamped {
    createdAt: Date;
    updatedAt: Date;
}

interface Serializable {
    toJSON(): string;
}

class Document implements Identifiable, Timestamped, Serializable {
    public id: string;
    public createdAt: Date;
    public updatedAt: Date;
    
    constructor(
        public title: string,
        public content: string
    ) {
        this.id = `DOC-${Date.now()}`;
        this.createdAt = new Date();
        this.updatedAt = new Date();
    }
    
    update(content: string): void {
        this.content = content;
        this.updatedAt = new Date();
    }
    
    toJSON(): string {
        return JSON.stringify({
            id: this.id,
            title: this.title,
            content: this.content,
            createdAt: this.createdAt.toISOString(),
            updatedAt: this.updatedAt.toISOString()
        });
    }
    
    static fromJSON(json: string): Document {
        const data = JSON.parse(json);
        const doc = new Document(data.title, data.content);
        doc.id = data.id;
        doc.createdAt = new Date(data.createdAt);
        doc.updatedAt = new Date(data.updatedAt);
        return doc;
    }
}
```
</details>

---

## Quiz

1. What is the default access modifier in TypeScript?
   - [x] `public`
   - [ ] `private`
   - [ ] `protected`
   - [ ] None (must be specified)

2. Can you instantiate an abstract class?
   - [ ] Yes
   - [x] No
   - [ ] Only with `new` keyword
   - [ ] Only in subclasses

3. What does `readonly` prevent?
   - [ ] Reading the property
   - [x] Modifying after initialization
   - [ ] Inheritance
   - [ ] Accessing from outside

4. What's the difference between `private` and `protected`?
   - [ ] No difference
   - [ ] `protected` is stricter
   - [x] `protected` allows subclass access
   - [ ] `private` allows subclass access

---

[← Back to Chapter](./README.md) | [Next Chapter →](../2.3-functions-advanced/README.md)
