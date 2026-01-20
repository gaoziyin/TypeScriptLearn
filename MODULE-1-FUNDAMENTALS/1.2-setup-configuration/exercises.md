# Chapter 1.2 Exercises

## Exercise 1: Create a Project

Set up a new TypeScript project from scratch:

1. Create a new directory called `ts-practice`
2. Initialize npm
3. Install TypeScript as a dev dependency
4. Generate `tsconfig.json`
5. Create a `src/index.ts` file that exports a greeting function
6. Compile and run

<details>
<summary>Solution</summary>

```bash
mkdir ts-practice
cd ts-practice
npm init -y
npm install --save-dev typescript
npx tsc --init
mkdir src
```

`src/index.ts`:
```typescript
export function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet("TypeScript"));
```

```bash
npx tsc
node dist/index.js
```
</details>

---

## Exercise 2: Configure tsconfig.json

Fix this `tsconfig.json` to:
- Output to a `build` folder instead of root
- Enable strict mode
- Target ES2022
- Use ESM modules

```json
{
    "compilerOptions": {
        "target": "ES5"
    }
}
```

<details>
<summary>Solution</summary>

```json
{
    "compilerOptions": {
        "target": "ES2022",
        "module": "NodeNext",
        "moduleResolution": "NodeNext",
        "strict": true,
        "outDir": "./build",
        "rootDir": "./src",
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "build"]
}
```
</details>

---

## Exercise 3: Add npm Scripts

Add these scripts to `package.json`:
- `build`: Compile TypeScript
- `start`: Run compiled code
- `dev`: Run with watch mode using tsx
- `check`: Type-check without emitting

<details>
<summary>Solution</summary>

```json
{
    "scripts": {
        "build": "tsc",
        "start": "node build/index.js",
        "dev": "tsx watch src/index.ts",
        "check": "tsc --noEmit"
    }
}
```
</details>

---

## Exercise 4: Strict Mode Investigation

With `strict: true`, predict which of these will cause errors:

```typescript
// A
let name;
console.log(name.toUpperCase());

// B
function greet(name) {
    return `Hello, ${name}`;
}

// C
class User {
    name: string;
}

// D
const config = null;
console.log(config.timeout);
```

<details>
<summary>Solution</summary>

**ALL of them cause errors with strict mode!**

- **A**: `'name' implicitly has type 'any'` and `Object is possibly 'undefined'`
- **B**: `Parameter 'name' implicitly has an 'any' type`
- **C**: `Property 'name' has no initializer and is not definitely assigned`
- **D**: `Object is possibly 'null'`

Fixes:
```typescript
// A
let name: string = "Alice";
console.log(name.toUpperCase());

// B
function greet(name: string): string {
    return `Hello, ${name}`;
}

// C
class User {
    name: string = "";
    // or
    constructor(name: string) {
        this.name = name;
    }
}

// D
const config: { timeout: number } | null = null;
console.log(config?.timeout);  // Optional chaining
```
</details>

---

## Quiz

1. Which file configures the TypeScript compiler?
   - [ ] `typescript.json`
   - [x] `tsconfig.json`
   - [ ] `config.ts`
   - [ ] `package.json`

2. What command generates a `tsconfig.json` file?
   - [ ] `tsc create`
   - [x] `tsc --init`
   - [ ] `npm init ts`
   - [ ] `ts init`

3. Which option compiles TypeScript without generating JavaScript?
   - [ ] `tsc --dry`
   - [x] `tsc --noEmit`
   - [ ] `tsc --check`
   - [ ] `tsc --validate`

4. What does `"strict": true` enable?
   - [ ] Only `strictNullChecks`
   - [ ] Only `noImplicitAny`
   - [x] All strict mode family options
   - [ ] Console warnings

---

[← Back to Chapter](./README.md) | [Next Chapter →](../1.3-basic-types/README.md)
