---
description: >-
  Manually trigger data fetching from Airstack by calling queries to the
  Airstack API with pagination included. If you don't need pagination, use
  useLazyQuery instead.
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

# useLazyQueryWithPagination

## Pre-requisites

* Calling [`init`](../../nodejs-sdk-reference/init.md) function

## Example

```javascript
import { useLazyQueryWithPagination } from "@airstack/airstack-react";

const { data, error, loading pagination } =
  useLazyQueryWithPagination(query, variables, configAndCallbacks);
```

## Function Signature

```typescript
function useLazyQueryWithPagination(
  query: string,
  variables?: Variables,
  configAndCallbacks: ConfigAndCallbacks
): useLazyQueryWithPaginationReturnType
```

## Params

| Param                | Type                                                      | Default Value | Description                              |
| -------------------- | --------------------------------------------------------- | ------------- | ---------------------------------------- |
| `query`              | `string`                                                  | -             | [Airstack API key](../../get-api-key.md) |
| `variables`          | [`Variables?`](../objects/variables.md)                   | `null`        | GraphQL query variables                  |
| `configAndCallbacks` | [`ConfigAndCallbacks?`](../objects/configandcallbacks.md) | `null`        | Additional configurations and callbacks. |

## Responses

| Param      | Type                                                                    | Default Value | Description                                                                                                   |
| ---------- | ----------------------------------------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------- |
| `response` | [`UseLazyQueryWithPaginationReturnType`](uselazyquerywithpagination.md) | -             | Response from the hook that will return an execute function, data, error logs, loading state, and pagination. |
