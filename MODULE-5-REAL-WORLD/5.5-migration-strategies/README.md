# Chapter 5.5: Migration Strategies

Migrate JavaScript projects to TypeScript incrementally.

---

## Migration Approaches

1. **Gradual** - Add TypeScript alongside JavaScript
2. **Module-by-module** - Convert one file at a time
3. **Big bang** - Convert everything at once (risky)

---

## Step 1: Setup

```bash
npm install --save-dev typescript
npx tsc --init
```

```json
// tsconfig.json
{
    "compilerOptions": {
        "allowJs": true,          // Allow .js files
        "checkJs": false,         // Don't check JS (yet)
        "strict": false,          // Relax initially
        "outDir": "./dist"
    },
    "include": ["src/**/*"]
}
```

---

## Step 2: Rename Files

```bash
# Rename .js to .ts (or .jsx to .tsx)
mv src/utils.js src/utils.ts
```

---

## Step 3: Add Types Gradually

```typescript
// Before
function greet(name) {
    return "Hello, " + name;
}

// After
function greet(name: string): string {
    return "Hello, " + name;
}
```

---

## Step 4: Enable Strict Mode

```json
{
    "compilerOptions": {
        "strict": true,
        "noImplicitAny": true,
        "strictNullChecks": true
    }
}
```

---

## Common Patterns

```typescript
// Temporarily use 'any' for complex migrations
const legacy: any = oldCode.process();

// Use 'unknown' and validate
function handleLegacy(data: unknown) {
    if (typeof data === "object" && data && "value" in data) {
        return (data as { value: string }).value;
    }
}

// Declare types for third-party JS
declare module "legacy-lib" {
    export function doSomething(): void;
}
```

---

## Migration Checklist

- [ ] Install TypeScript
- [ ] Create tsconfig.json with allowJs
- [ ] Convert files one by one (.js → .ts)
- [ ] Add type annotations
- [ ] Enable strict mode
- [ ] Add @types packages
- [ ] Remove allowJs
- [ ] Fix all errors

---

## Summary

| Phase | Action |
|-------|--------|
| Setup | allowJs + loose settings |
| Convert | Rename files, add types |
| Strict | Enable strict mode |
| Clean | Remove any, fix errors |

---

[← Previous](../5.4-performance-best-practices/README.md) | [Course Complete! 🎉](../../README.md)
