# Chapter 4.2 Exercises

## Exercise 1: Module Declaration

Write a declaration for a fictional `string-utils` module:

```typescript
// Functions: capitalize, reverse, truncate
declare module "string-utils" { ??? }
```

<details>
<summary>Solution</summary>

```typescript
declare module "string-utils" {
    export function capitalize(str: string): string;
    export function reverse(str: string): string;
    export function truncate(str: string, maxLength: number): string;
}
```
</details>

---

## Exercise 2: Global Declaration

Extend Window with a config object:

<details>
<summary>Solution</summary>

```typescript
// globals.d.ts
declare global {
    interface Window {
        config: {
            apiUrl: string;
            debug: boolean;
        };
    }
}
export {};
```
</details>

---

[← Back](./README.md) | [Next →](../4.3-decorators/README.md)
