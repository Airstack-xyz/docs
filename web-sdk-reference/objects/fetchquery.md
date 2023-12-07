---
description: >-
  The return type of the fetchQuery function comprises of the data, error logs,
  loading indicator, and pagination object.
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

# FetchQuery

## Example

```typescript
{
  "data": {},
  "error": {},
  "hasNextPage": true,
  "hasPrevPage": true,
  "getNextPage": () => {},
  "getPrevPage": () => {}
}
```

## Type Signature

```typescript
type FetchQuery = {
  data: any;
  error: any;
  hasNextPage: boolean;
  hasPrevPage: boolean;
  getNextPage: () => Promise<FetchQuery | null>;
  getPrevPage: () => Promise<FetchQuery | null>;
};
```

## Fields

| Param         | Type                                                     | Default Value | Description                                              |
| ------------- | -------------------------------------------------------- | ------------- | -------------------------------------------------------- |
| `data`        | `any?`                                                   | `null`        | Response data from GraphQL `query` if API call succeeds. |
| `error`       | `any?`                                                   | `null`        | Error logs from GraphQL query if API call failed.        |
| `hasNextPage` | `boolean`                                                | `false`       | Indicate if there is any next page.                      |
| `hasPrevPage` | `boolean`                                                | `false`       | Indicate if there is any previous page.                  |
| `getNextPage` | `() => Promise<`[`FetchQuery`](fetchquery.md) `\| null>` | -             | Function to get data in the next page.                   |
| `getPrevPage` | `() => Promise<`[`FetchQuery`](fetchquery.md) `\| null>` | -             | Function to get data in the previous page.               |
