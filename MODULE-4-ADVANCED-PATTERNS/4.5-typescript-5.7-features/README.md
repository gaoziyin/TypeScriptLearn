# Chapter 4.5: TypeScript 5.7 Features

Latest features from TypeScript 5.7 (November 2024).

---

## Enhanced Variable Initialization Checks

```typescript
// TS 5.7 catches uninitialized variables in nested functions
let value: string;

function init() {
    value = "initialized";
}

// ❌ Error in 5.7: 'value' used before being assigned
console.log(value.toUpperCase());

init();
console.log(value.toUpperCase());  // ✓ OK after initialization
```

---

## `--rewriteRelativeImportExtensions`

```typescript
// tsconfig.json
{
    "compilerOptions": {
        "rewriteRelativeImportExtensions": true
    }
}

// Code
import { helper } from "./utils.ts";  // Write .ts
// Compiled output uses .js automatically
```

Useful for Deno, ts-node, and direct TypeScript execution.

---

## ES2024 Support

```typescript
// tsconfig.json
{
    "compilerOptions": {
        "target": "ES2024",
        "lib": ["ES2024"]
    }
}

// Object.groupBy
const items = [
    { type: "fruit", name: "apple" },
    { type: "vegetable", name: "carrot" }
];
const grouped = Object.groupBy(items, item => item.type);
// { fruit: [...], vegetable: [...] }

// Map.groupBy
const map = Map.groupBy(items, item => item.type);

// Promise.withResolvers
const { promise, resolve, reject } = Promise.withResolvers<string>();
setTimeout(() => resolve("done"), 1000);
```

---

## TypedArrays Generic over ArrayBufferLike

```typescript
// TypedArrays now preserve buffer type
function process<T extends ArrayBufferLike>(
    buffer: T
): Uint8Array<T> {
    return new Uint8Array(buffer);
}

const shared = new SharedArrayBuffer(16);
const arr = process(shared);  // Uint8Array<SharedArrayBuffer>
```

---

## V8 Compile Caching

Automatic performance improvement when running TypeScript in Node.js with ts-node or similar tools.

---

## Summary

| Feature | Description |
|---------|-------------|
| Init checks | Better uninitialized variable detection |
| `rewriteRelativeImportExtensions` | Rewrite .ts to .js imports |
| ES2024 | `Object.groupBy`, `Promise.withResolvers` |
| TypedArrays | Generic buffer type preservation |
| V8 caching | Faster execution in Node.js |

---

[← Previous](../4.4-modules-namespaces/README.md) | [Next Module →](../../MODULE-5-REAL-WORLD/5.1-typescript-react/README.md)
