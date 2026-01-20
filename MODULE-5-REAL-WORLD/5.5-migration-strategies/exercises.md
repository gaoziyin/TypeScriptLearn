# Chapter 5.5 Exercises

## Exercise 1: Migration Plan

Create a migration plan for a project with:
- 50 JavaScript files
- jQuery dependencies
- No test coverage

<details>
<summary>Solution</summary>

1. **Week 1**: Setup
   - Install TypeScript with `allowJs: true`
   - Install `@types/jquery`
   
2. **Week 2-4**: Core modules
   - Convert utility files first
   - Add types to shared modules
   
3. **Week 5-6**: Features
   - Convert feature modules
   - Add interfaces for data structures
   
4. **Week 7**: Strict mode
   - Enable strict gradually
   - Fix type errors
   
5. **Week 8**: Cleanup
   - Remove `allowJs`
   - Final review
</details>

---

## Exercise 2: Type Legacy Code

Add types to this JavaScript:

```javascript
function fetchData(url, callback) {
    fetch(url).then(r => r.json()).then(callback);
}
```

<details>
<summary>Solution</summary>

```typescript
interface ApiResponse {
    data: unknown;
}

async function fetchData<T>(
    url: string,
    callback: (data: T) => void
): Promise<void> {
    const response = await fetch(url);
    const data: T = await response.json();
    callback(data);
}
```
</details>

---

## 🎉 Congratulations!

You've completed the TypeScript curriculum. Return to the [main README](../../README.md) for review.
