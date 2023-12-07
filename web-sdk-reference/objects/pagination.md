---
description: >-
  Pagination object that provides indication if there is any previous or next
  page and functions to get to the previous or the next page.
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

# Pagination

## Example

```json
{
  "hasNextPage": true,
  "hasPrevPage": true,
  "getNextPage": () => {},
  "getPrevPage": () => {}
}
```

## Type Signature

```typescript
type Pagination = {
  hasNextPage: boolean;
  hasPrevPage: boolean;
  getNextPage: () => Promise<void>;
  getPrevPage: () => Promise<void>;
};
```

## Fields

| Param         | Type                  | Default Value | Description                                |
| ------------- | --------------------- | ------------- | ------------------------------------------ |
| `hasNextPage` | `boolean`             | `false`       | Indicate if there is any next page.        |
| `hasPrevPage` | `boolean`             | `false`       | Indicate if there is any previous page.    |
| `getNextPage` | `() => Promise<void>` | -             | Function to get data in the next page.     |
| `getPrevPage` | `() => Promise<void>` | -             | Function to get data in the previous page. |
