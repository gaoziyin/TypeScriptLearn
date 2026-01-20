# Chapter 5.1 Exercises

## Exercise 1: Typed Component

Create a Button component with proper types:

```typescript
// Props: label, onClick, variant ("primary" | "secondary"), disabled
```

<details>
<summary>Solution</summary>

```typescript
interface ButtonProps {
    label: string;
    onClick: () => void;
    variant?: "primary" | "secondary";
    disabled?: boolean;
}

function Button({ label, onClick, variant = "primary", disabled }: ButtonProps) {
    return (
        <button className={variant} onClick={onClick} disabled={disabled}>
            {label}
        </button>
    );
}
```
</details>

---

## Exercise 2: Custom Hook

Create a typed useLocalStorage hook:

<details>
<summary>Solution</summary>

```typescript
function useLocalStorage<T>(key: string, initial: T): [T, (v: T) => void] {
    const [value, setValue] = useState<T>(() => {
        const stored = localStorage.getItem(key);
        return stored ? JSON.parse(stored) : initial;
    });
    
    const setStored = (newValue: T) => {
        setValue(newValue);
        localStorage.setItem(key, JSON.stringify(newValue));
    };
    
    return [value, setStored];
}
```
</details>

---

[← Back](./README.md) | [Next →](../5.2-typescript-nodejs/README.md)
