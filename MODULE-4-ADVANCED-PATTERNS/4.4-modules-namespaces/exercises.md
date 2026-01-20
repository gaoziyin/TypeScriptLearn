# Chapter 4.4 Exercises

## Exercise 1: Create Barrel File

Create an index.ts that re-exports from multiple files:

```
/models
  /user.ts
  /post.ts
  /comment.ts
  /index.ts  <- Create this
```

<details>
<summary>Solution</summary>

```typescript
// models/index.ts
export { User, UserRole } from "./user";
export { Post, PostStatus } from "./post";
export { Comment } from "./comment";
export type { UserDTO, PostDTO } from "./types";
```
</details>

---

## Exercise 2: Dynamic Import

Load a heavy module only when needed:

<details>
<summary>Solution</summary>

```typescript
async function showChart(data: number[]) {
    const { Chart } = await import("chart.js");
    const chart = new Chart(document.getElementById("canvas"), {
        type: "line",
        data: { datasets: [{ data }] }
    });
    return chart;
}
```
</details>

---

[← Back](./README.md) | [Next →](../4.5-typescript-5.7-features/README.md)
