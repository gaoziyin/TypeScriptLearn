# Chapter 5.4: Performance and Best Practices

Optimize TypeScript compilation and write maintainable code.

---

## Compiler Performance

```json
// tsconfig.json
{
    "compilerOptions": {
        "incremental": true,           // Cache compilations
        "tsBuildInfoFile": ".tsbuildinfo",
        "skipLibCheck": true,          // Skip .d.ts checking
        "isolatedModules": true        // Faster bundler mode
    }
}
```

---

## Project References

For large monorepos:

```json
// tsconfig.json
{
    "references": [
        { "path": "./packages/core" },
        { "path": "./packages/app" }
    ]
}
```

```bash
tsc --build  # Incremental project build
```

---

## Type-Level Performance

```typescript
// ✗ Slow: Deep recursion
type DeepPartial<T> = T extends object
    ? { [P in keyof T]?: DeepPartial<T[P]> }
    : T;

// ✓ Faster: Limit depth
type DeepPartialSafe<T, Depth extends number = 5> = Depth extends 0
    ? T
    : T extends object
    ? { [P in keyof T]?: DeepPartialSafe<T[P], Prev[Depth]> }
    : T;
```

---

## Code Best Practices

```typescript
// ✓ Use strict mode
{ "strict": true }

// ✓ Prefer interfaces for objects
interface User { name: string }

// ✓ Use const assertions
const config = { api: "/v1" } as const;

// ✓ Avoid any, use unknown
function parse(input: unknown) { /* validate */ }

// ✓ Use discriminated unions
type Result<T> = { ok: true; value: T } | { ok: false; error: Error };
```

---

## File Organization

```
src/
├── types/           # Shared types
├── utils/           # Utility functions
├── components/      # React components
├── services/        # Business logic
├── hooks/           # Custom hooks
└── index.ts         # Entry point
```

---

## Summary

| Area | Tip |
|------|-----|
| Compilation | Use `incremental`, `skipLibCheck` |
| Monorepo | Use project references |
| Types | Avoid deep recursion |
| Code | Use strict mode, avoid any |

---

[← Previous](../5.3-testing-typescript/README.md) | [Next →](../5.5-migration-strategies/README.md)
