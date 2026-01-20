# Chapter 4.3: Decorators

Decorators are special functions that modify classes, methods, and properties.

> **Note**: Enable `experimentalDecorators` in tsconfig.json

---

## Class Decorators

```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
}
```

---

## Decorator Factories

```typescript
function log(prefix: string) {
    return function(target: any, key: string, descriptor: PropertyDescriptor) {
        const original = descriptor.value;
        descriptor.value = function(...args: any[]) {
            console.log(`${prefix}: Calling ${key}`);
            return original.apply(this, args);
        };
        return descriptor;
    };
}

class Calculator {
    @log("DEBUG")
    add(a: number, b: number): number {
        return a + b;
    }
}
```

---

## Method Decorators

```typescript
function validateArgs(target: any, key: string, descriptor: PropertyDescriptor) {
    const original = descriptor.value;
    descriptor.value = function(...args: any[]) {
        if (args.some(arg => arg === undefined || arg === null)) {
            throw new Error("Invalid arguments");
        }
        return original.apply(this, args);
    };
    return descriptor;
}

class UserService {
    @validateArgs
    createUser(name: string, email: string) {
        return { name, email };
    }
}
```

---

## Property Decorators

```typescript
function readonly(target: any, key: string) {
    Object.defineProperty(target, key, {
        writable: false
    });
}

class Config {
    @readonly
    apiUrl: string = "https://api.example.com";
}
```

---

## Parameter Decorators

```typescript
function required(target: any, key: string, index: number) {
    // Mark parameter as required
    const requiredParams = Reflect.getMetadata("required", target, key) || [];
    requiredParams.push(index);
    Reflect.defineMetadata("required", requiredParams, target, key);
}

class Service {
    greet(@required name: string) {
        return `Hello, ${name}`;
    }
}
```

---

## Common Use Cases

```typescript
// Logging, validation, caching, authorization
@Controller("/users")
class UserController {
    @Get("/:id")
    @Authenticated
    @Cache(60)
    getUser(@Param("id") id: string) { }
}
```

---

## Summary

| Decorator Type | Applies To |
|----------------|------------|
| Class | Class constructor |
| Method | Class methods |
| Property | Class properties |
| Parameter | Method parameters |
| Accessor | Getters/setters |

---

[← Previous](../4.2-declaration-files/README.md) | [Next →](../4.4-modules-namespaces/README.md)
