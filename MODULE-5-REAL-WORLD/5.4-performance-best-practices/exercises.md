# Chapter 5.4 Exercises

## Exercise 1: Optimize tsconfig

Identify improvements for this config:

```json
{
    "compilerOptions": {
        "target": "ES5",
        "strict": false
    }
}
```

<details>
<summary>Solution</summary>

```json
{
    "compilerOptions": {
        "target": "ES2022",
        "strict": true,
        "incremental": true,
        "skipLibCheck": true,
        "moduleResolution": "NodeNext",
        "outDir": "./dist"
    }
}
```
</details>

---

## Exercise 2: Refactor Code

Improve this code:

```typescript
function process(data: any): any {
    return data.value.toUpperCase();
}
```

<details>
<summary>Solution</summary>

```typescript
interface Data {
    value: string;
}

function process(data: Data): string {
    return data.value.toUpperCase();
}
```
</details>

---

[← Back](./README.md) | [Next →](../5.5-migration-strategies/README.md)
