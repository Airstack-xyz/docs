---
description: >-
  The return type of the useQueryWithPagination hook comprises of the data,
  error logs, loading indicator, and pagination object.
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

# UseQueryWithPaginationReturnType

## Example

```json
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
```

## Type Signature

```typescript
type UseQueryWithPaginationReturnType = {
  data: any;
  error: any;
  loading: boolean;
  pagination: Pagination;
};
```

## Fields

| Param        | Type                          | Default Value | Description                                                                          |
| ------------ | ----------------------------- | ------------- | ------------------------------------------------------------------------------------ |
| `data`       | `any`                         | `null`        | Response data from GraphQL `query` if API call succeeds.                             |
| `error`      | `any`                         | `null`        | Error logs from GraphQL query if API call failed.                                    |
| `loading`    | `boolean`                     | `false`       | Returns `true` to indicate if the API call is in a loading state, otherwise `false`. |
| `pagination` | [`Pagination`](pagination.md) | -             | A pagination object that returns the pagination state and function to change pages   |
