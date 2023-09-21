---
description: >-
  Fetch data from Airstack by calling queries to the Airstack API with
  pagination included. If you don't need pagination, use useQuery instead.
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

# useQueryWithPagination

## Pre-requisites

* Calling [`init`](../../nodejs-sdk-reference/init.md) function

## Example

```javascript
import { useQueryWithPagination } from "@airstack/airstack-react";

const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  useQueryWithPagination(query, variables);
```

## Function Signature

```typescript
function useQueryWithPagination(
  query: string,
  variables?: Variables
): useQueryWithPaginationReturnType
```

## Params

| Param                | Type                                                      | Default Value | Description                                          |
| -------------------- | --------------------------------------------------------- | ------------- | ---------------------------------------------------- |
| `query`              | `string`                                                  | -             | [Airstack API key](../../get-started/get-api-key.md) |
| `variables`          | [`Variables?`](../objects/variables.md)                   | `null`        | GraphQL query variables                              |
| `configAndCallbacks` | [`ConfigAndCallbacks?`](../objects/configandcallbacks.md) | `null`        | Additional configurations and callbacks.             |

## Responses

| Param      | Type                                                                                 | Default Value | Description                                                                              |
| ---------- | ------------------------------------------------------------------------------------ | ------------- | ---------------------------------------------------------------------------------------- |
| `response` | [`UseQueryWithPaginationReturnType`](../objects/usequerywithpaginationreturntype.md) | -             | Response from the hook that will return data, error logs, loading state, and pagination. |

