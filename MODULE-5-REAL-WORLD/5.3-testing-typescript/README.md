# Chapter 5.3: Testing TypeScript

Type-safe testing strategies.

---

## Jest Setup

```bash
npm install --save-dev jest @types/jest ts-jest
npx ts-jest config:init
```

```json
// jest.config.js
module.exports = {
    preset: "ts-jest",
    testEnvironment: "node"
};
```

---

## Basic Tests

```typescript
// math.ts
export function add(a: number, b: number): number {
    return a + b;
}

// math.test.ts
import { add } from "./math";

describe("add", () => {
    it("adds two numbers", () => {
        expect(add(1, 2)).toBe(3);
    });
    
    it("handles negative numbers", () => {
        expect(add(-1, 1)).toBe(0);
    });
});
```

---

## Mocking

```typescript
// UserService.ts
export class UserService {
    async getUser(id: number): Promise<User> { /* ... */ }
}

// test.ts
jest.mock("./UserService");

const mockGetUser = jest.fn<Promise<User>, [number]>();
mockGetUser.mockResolvedValue({ id: 1, name: "Alice" });
```

---

## Type Testing with tsd

```bash
npm install --save-dev tsd
```

```typescript
// test-types.ts
import { expectType, expectError } from "tsd";
import { add } from "./math";

expectType<number>(add(1, 2));
expectError(add("1", 2));  // Should error
```

---

## Testing React Components

```typescript
import { render, screen, fireEvent } from "@testing-library/react";
import { Button } from "./Button";

test("calls onClick when clicked", () => {
    const handleClick = jest.fn();
    render(<Button label="Click" onClick={handleClick} />);
    
    fireEvent.click(screen.getByText("Click"));
    expect(handleClick).toHaveBeenCalled();
});
```

---

## Summary

| Tool | Purpose |
|------|---------|
| Jest + ts-jest | Test runner |
| tsd | Type testing |
| @testing-library | React testing |
| jest.fn() | Typed mocks |

---

[← Previous](../5.2-typescript-nodejs/README.md) | [Next →](../5.4-performance-best-practices/README.md)
