# Chapter 4.2: Declaration Files

Declaration files (`.d.ts`) provide type information for JavaScript code.

---

## What Are Declaration Files?

```typescript
// math.d.ts - Types only, no implementation
declare function add(a: number, b: number): number;
declare const PI: number;

declare class Calculator {
    add(a: number, b: number): number;
    subtract(a: number, b: number): number;
}
```

---

## Declaring Modules

```typescript
// types/lodash.d.ts
declare module "lodash" {
    export function chunk<T>(array: T[], size: number): T[][];
    export function compact<T>(array: T[]): T[];
    export function uniq<T>(array: T[]): T[];
}

// Usage
import { chunk } from "lodash";
chunk([1, 2, 3, 4], 2);  // [[1, 2], [3, 4]]
```

---

## Global Declarations

```typescript
// globals.d.ts
declare global {
    interface Window {
        myApp: {
            version: string;
            init(): void;
        };
    }
    
    const API_URL: string;
}

export {};  // Makes this a module

// Usage
window.myApp.init();
console.log(API_URL);
```

---

## DefinitelyTyped (@types)

```bash
# Install type definitions
npm install --save-dev @types/lodash
npm install --save-dev @types/node
npm install --save-dev @types/express
```

```typescript
// Automatically available after install
import express from "express";
const app = express();
```

---

## Triple-Slash Directives

```typescript
/// <reference path="./custom-types.d.ts" />
/// <reference types="node" />
/// <reference lib="es2020" />
```

---

## Writing .d.ts for Your Own JS

```javascript
// utils.js
export function formatDate(date) {
    return date.toISOString();
}
export const VERSION = "1.0.0";
```

```typescript
// utils.d.ts
export function formatDate(date: Date): string;
export const VERSION: string;
```

---

## Summary

| File Type | Purpose |
|-----------|---------|
| `.d.ts` | Type declarations only |
| `@types/*` | Community type definitions |
| `/// <reference>` | Reference other declarations |
| `declare module` | Declare module types |
| `declare global` | Extend global scope |

---

[← Previous](../4.1-utility-types/README.md) | [Next →](../4.3-decorators/README.md)
