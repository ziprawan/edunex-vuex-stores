# Todo Store Definitions

## namespaced

true

## state

```typescript
interface ITodoState {
  list: Record<string, any>;
}

let state: ITodoState = { list: {} };
```
