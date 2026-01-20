# Chapter 5.2 Exercises

## Exercise 1: Typed API Route

Create a typed route for updating a user:

<details>
<summary>Solution</summary>

```typescript
interface UpdateUserParams { id: string }
interface UpdateUserBody { name?: string; email?: string }
interface UpdateUserResponse { success: boolean; user: User }

app.put(
    "/users/:id",
    async (req: Request<UpdateUserParams, UpdateUserResponse, UpdateUserBody>, res) => {
        const { id } = req.params;
        const { name, email } = req.body;
        // Update logic
        res.json({ success: true, user: { id: Number(id), name: name!, email: email! } });
    }
);
```
</details>

---

## Exercise 2: Middleware Types

Create typed authentication middleware:

<details>
<summary>Solution</summary>

```typescript
interface AuthRequest extends Request {
    user?: { id: number; role: string };
}

function authMiddleware(req: AuthRequest, res: Response, next: NextFunction) {
    const token = req.headers.authorization;
    if (!token) return res.status(401).json({ error: "Unauthorized" });
    req.user = { id: 1, role: "admin" };
    next();
}
```
</details>

---

[← Back](./README.md) | [Next →](../5.3-testing-typescript/README.md)
