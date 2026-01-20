# Chapter 5.3 Exercises

## Exercise 1: Write Tests

Write tests for this function:

```typescript
function validateEmail(email: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}
```

<details>
<summary>Solution</summary>

```typescript
describe("validateEmail", () => {
    it("returns true for valid emails", () => {
        expect(validateEmail("test@example.com")).toBe(true);
    });
    
    it("returns false for invalid emails", () => {
        expect(validateEmail("invalid")).toBe(false);
        expect(validateEmail("no@domain")).toBe(false);
    });
});
```
</details>

---

## Exercise 2: Mock a Service

Create a typed mock for an API service:

<details>
<summary>Solution</summary>

```typescript
interface ApiService {
    fetchUser(id: number): Promise<User>;
}

const mockApi: jest.Mocked<ApiService> = {
    fetchUser: jest.fn().mockResolvedValue({ id: 1, name: "Test" })
};
```
</details>

---

[← Back](./README.md) | [Next →](../5.4-performance-best-practices/README.md)
