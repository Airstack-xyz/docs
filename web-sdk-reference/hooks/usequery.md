---
description: >-
  Fetch data from Airstack by calling queries to the Airstack API. If you need
  any pagination, use useQueryWithPagination instead.
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

# useQuery

## Pre-requisites

* Calling [`init`](../../nodejs-sdk-reference/init.md) function

## Example

```javascript
import { useQuery } from "@airstack/airstack-react";

const { data, error, loading } = useQuery(query, variables);
```

## Function Signature

```typescript
function useQuery(
  query: string,
  variables?: Variables,
  configAndCallbacks?: ConfigAndCallabacks
): UseQueryReturnType
```

## Params

| Param                | Type                                                      | Default Value | Description                                          |
| -------------------- | --------------------------------------------------------- | ------------- | ---------------------------------------------------- |
| `query`              | `string`                                                  | -             | [Airstack API key](../../get-started/get-api-key.md) |
| `variables`          | [`Variables?`](../objects/variables.md)                   | `null`        | GraphQL query variables                              |
| `configAndCallbacks` | [`ConfigAndCallbacks?`](../objects/configandcallbacks.md) | `null`        | Additional configurations and callbacks.             |

## Responses

| Param      | Type                                                     | Default Value | Description                                                                  |
| ---------- | -------------------------------------------------------- | ------------- | ---------------------------------------------------------------------------- |
| `response` | [`UseQueryReturnType`](../objects/usequeryreturntype.md) | -             | Response from the hook that will return data, error logs, and loading state. |
