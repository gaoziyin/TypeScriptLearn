# Chapter 4.4: Modules and Namespaces

Organize code with ES Modules and TypeScript's module system.

---

## ES Modules

```typescript
// math.ts - Export
export function add(a: number, b: number): number {
    return a + b;
}

export const PI = 3.14159;

export default class Calculator {
    add(a: number, b: number) { return a + b; }
}

// main.ts - Import
import Calculator, { add, PI } from "./math";
import * as math from "./math";
```

---

## Module Resolution

```json
// tsconfig.json
{
    "compilerOptions": {
        "moduleResolution": "NodeNext",  // or "bundler", "node"
        "module": "NodeNext"
    }
}
```

```typescript
// Different import styles
import fs from "fs";                    // Node module
import { Component } from "react";      // Package
import { helper } from "./utils";       // Relative
import config from "./config.json";     // JSON
```

---

## Re-exporting

```typescript
// index.ts - Barrel file
export { User } from "./user";
export { Post } from "./post";
export * from "./utils";
export * as helpers from "./helpers";
export { default as API } from "./api";

// Usage
import { User, Post, helpers } from "./models";
```

---

## Dynamic Imports

```typescript
async function loadModule() {
    const { default: Chart } = await import("chart.js");
    return new Chart();
}

// Type-only imports
import type { User } from "./types";
import { type Post, createPost } from "./posts";
```

---

## Namespaces (Legacy)

```typescript
// Use modules instead where possible
namespace Validation {
    export interface Validator {
        validate(s: string): boolean;
    }
    
    export class EmailValidator implements Validator {
        validate(s: string) { return s.includes("@"); }
    }
}

const validator = new Validation.EmailValidator();
```

---

## Module vs Namespace

| Feature | Modules | Namespaces |
|---------|---------|------------|
| Modern approach | ✓ Yes | ✗ Legacy |
| Tree-shaking | ✓ Yes | ✗ No |
| File-based | ✓ Yes | ✗ No |
| Preferred for | New code | Global scripts |

---

## Summary

- Use ES Modules for new projects
- Use barrel files (`index.ts`) for clean exports
- Avoid namespaces in modern code
- Use `import type` for type-only imports

---

[← Previous](../4.3-decorators/README.md) | [Next →](../4.5-typescript-5.7-features/README.md)
