# Chapter 5.2: TypeScript with Node.js

Backend development with type safety.

---

## Setup

```bash
npm init -y
npm install --save-dev typescript @types/node tsx
npx tsc --init
```

```json
// tsconfig.json
{
    "compilerOptions": {
        "target": "ES2022",
        "module": "NodeNext",
        "moduleResolution": "NodeNext",
        "strict": true,
        "outDir": "./dist",
        "rootDir": "./src"
    }
}
```

---

## Express with TypeScript

```typescript
import express, { Request, Response, NextFunction } from "express";

const app = express();

interface User {
    id: number;
    name: string;
}

// Typed route handler
app.get("/users/:id", (req: Request<{ id: string }>, res: Response<User>) => {
    const user = { id: parseInt(req.params.id), name: "Alice" };
    res.json(user);
});

// Error handler
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
    res.status(500).json({ error: err.message });
});
```

---

## Request/Response Types

```typescript
// Custom request with body
interface CreateUserBody {
    name: string;
    email: string;
}

interface CreateUserResponse {
    id: number;
    name: string;
    email: string;
}

app.post(
    "/users",
    (req: Request<{}, CreateUserResponse, CreateUserBody>, res) => {
        const { name, email } = req.body;
        res.json({ id: 1, name, email });
    }
);
```

---

## Environment Variables

```typescript
// env.ts
import { z } from "zod";

const envSchema = z.object({
    PORT: z.string().default("3000"),
    DATABASE_URL: z.string(),
    NODE_ENV: z.enum(["development", "production"]).default("development")
});

export const env = envSchema.parse(process.env);
```

---

## Database Types

```typescript
// With Prisma
import { PrismaClient, User } from "@prisma/client";

const prisma = new PrismaClient();

async function getUser(id: number): Promise<User | null> {
    return prisma.user.findUnique({ where: { id } });
}
```

---

## Summary

| Area | Approach |
|------|----------|
| Express | `@types/express` |
| Env vars | Zod validation |
| Database | Prisma/TypeORM |
| API types | Request/Response generics |

---

[← Previous](../5.1-typescript-react/README.md) | [Next →](../5.3-testing-typescript/README.md)
