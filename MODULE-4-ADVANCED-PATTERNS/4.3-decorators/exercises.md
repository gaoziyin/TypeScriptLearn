# Chapter 4.3 Exercises

## Exercise 1: Timing Decorator

Create a decorator that logs method execution time:

<details>
<summary>Solution</summary>

```typescript
function timing(target: any, key: string, descriptor: PropertyDescriptor) {
    const original = descriptor.value;
    descriptor.value = function(...args: any[]) {
        const start = performance.now();
        const result = original.apply(this, args);
        const end = performance.now();
        console.log(`${key} took ${end - start}ms`);
        return result;
    };
    return descriptor;
}
```
</details>

---

## Exercise 2: Memoization Decorator

Create a decorator that caches function results:

<details>
<summary>Solution</summary>

```typescript
function memoize(target: any, key: string, descriptor: PropertyDescriptor) {
    const original = descriptor.value;
    const cache = new Map();
    
    descriptor.value = function(...args: any[]) {
        const cacheKey = JSON.stringify(args);
        if (cache.has(cacheKey)) return cache.get(cacheKey);
        const result = original.apply(this, args);
        cache.set(cacheKey, result);
        return result;
    };
    return descriptor;
}
```
</details>

---

[← Back](./README.md) | [Next →](../4.4-modules-namespaces/README.md)
