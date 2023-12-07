---
description: >-
  The return type of the useQueryWithPagination hook comprises of the manual
  fetching function, data, error logs, loading indicator, and pagination object.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# UseLazyQueryReturnType

## Example

```json
[
  "fetch": (variables) => {
    "data": {},
    "error": {}
  },
  "state": {
    "data": {},
    "error": {},
    "loading": false
  }
]
```

## TypeSignature

```typescript
type UseLazyQueryReturnType = [
  (variables?: Variables) => Promise<Omit<UseQueryReturnType, "loading">>,
  UseQueryReturnType
];
```

## Items

| Param   | Type                                                                                                   | Default Value | Description                                                                         |
| ------- | ------------------------------------------------------------------------------------------------------ | ------------- | ----------------------------------------------------------------------------------- |
| `fetch` | `(variables?: Variables) => Promise<Omit<`[`UseQueryReturnType`](usequeryreturntype.md)`, "loading">>` | -             | The function that can be triggered manually to call the GraphQL query.              |
| `state` | [`UseQueryReturnType`](usequeryreturntype.md)                                                          | -             | The current data, error logs, and loading state from the latest GraphQL query call. |

