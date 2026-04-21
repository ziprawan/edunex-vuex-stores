# 2b03 - isEmpty

```typescript
function isEmpty(obj: any): boolean {
  if (obj === undefined || obj === null) return true;
  if (Array.isArray(obj) || typeof obj === "string") return obj.length === 0;
  if (Object.keys(obj).length === 0) return true;

  return JSON.stringify(obj) === JSON.stringify({});
}
```
