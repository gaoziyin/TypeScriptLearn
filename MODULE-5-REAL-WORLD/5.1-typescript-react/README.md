# Chapter 5.1: TypeScript with React

Type-safe React development patterns.

---

## Setup

```bash
# Create new project
npx create-react-app my-app --template typescript
# or
npm create vite@latest my-app -- --template react-ts
```

---

## Component Types

```typescript
// Function Component
interface Props {
    name: string;
    age?: number;
}

const Greeting: React.FC<Props> = ({ name, age }) => {
    return <div>Hello, {name}{age && ` (${age})`}</div>;
};

// Or without FC (preferred)
function Greeting({ name, age }: Props) {
    return <div>Hello, {name}{age && ` (${age})`}</div>;
}

// Props with children
interface ContainerProps {
    title: string;
    children: React.ReactNode;
}

function Container({ title, children }: ContainerProps) {
    return <div><h1>{title}</h1>{children}</div>;
}
```

---

## Hooks with Types

```typescript
// useState
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);

// useRef
const inputRef = useRef<HTMLInputElement>(null);
const timerRef = useRef<number | undefined>();

// useReducer
type Action = { type: "increment" } | { type: "set"; value: number };

function reducer(state: number, action: Action): number {
    switch (action.type) {
        case "increment": return state + 1;
        case "set": return action.value;
    }
}

const [count, dispatch] = useReducer(reducer, 0);
```

---

## Event Handlers

```typescript
// Click events
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    console.log(e.currentTarget);
};

// Change events
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value);
};

// Form events
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
};

// Keyboard events
const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === "Enter") submit();
};
```

---

## Context with Types

```typescript
interface ThemeContext {
    theme: "light" | "dark";
    toggle: () => void;
}

const ThemeContext = React.createContext<ThemeContext | null>(null);

function useTheme() {
    const context = useContext(ThemeContext);
    if (!context) throw new Error("Must use within ThemeProvider");
    return context;
}
```

---

## Summary

| Pattern | Type |
|---------|------|
| Props | `interface Props` |
| Children | `React.ReactNode` |
| Events | `React.MouseEvent<HTMLElement>` |
| Refs | `useRef<HTMLElement>(null)` |
| Context | `createContext<T \| null>(null)` |

---

[← Previous Module](../../MODULE-4-ADVANCED-PATTERNS/4.5-typescript-5.7-features/README.md) | [Next →](../5.2-typescript-nodejs/README.md)
