---
description: >-
  The return type of the useLazyQueryWithPagination hook comprises of the manual
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

# UseLazyQueryWithPaginationReturnType

## Example

```json
[
  (variables) => {
    "data": {},
    "error": {},
    "pagination": {
      "getNextPage": () => {},
      "getPrevPage": () => {}
    }
  },
  {
    "data": {},
    "error": {},
    "loading": false,
    "pagination": {
      "hasNextPage": true,
      "hasPrevPage": true,
      "getNextPage": () => {},
      "getPrevPage": () => {}
    }
  }
]
```

## Type Signature

```typescript
type LazyHookReturnType = [
  (variables?: Variables) => Promise<
    {
      data: any;
      error: any;
      pagination: Omit<Pagination, "getNextPage" | "getPrevPage">;
    }
  >,
  {
    data: any;
    error: any;
    loading: boolean;
    pagination: Pagination;
  }
];
```

## Items

| Param   | Type                                                                                                   | Default Value | Description                                                                         |
| ------- | ------------------------------------------------------------------------------------------------------ | ------------- | ----------------------------------------------------------------------------------- |
| `fetch` | `(variables?: Variables) => Promise<Omit<`[`UseQueryReturnType`](usequeryreturntype.md)`, "loading">>` | -             | The function that can be triggered manually to call the GraphQL query.              |
| `state` | [`UseQueryWithPaginationReturnType`](../hooks/usequerywithpagination.md)                               | -             | The current data, error logs, and loading state from the latest GraphQL query call. |
