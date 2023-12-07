---
description: >-
  Manually trigger data fetching from Airstack by calling queries to the
  Airstack API. If you need any pagination, use useLazyQueryWithPagination
  instead.
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

# useLazyQuery

## Pre-requisites

* Calling [`init`](../../nodejs-sdk-reference/init.md) function

## Example

```javascript
import { useLazyQuery } from "@airstack/airstack-react";

const [fetch, { data, error, loading }] =
  useLazyQuery(query, variables, configAndCallbacks);
```

## Function Signature

```typescript
function useLazyQuery(
  query: string,
  variables?: Variables,
  configAndCallbacks?: ConfigAndCallbacks
): useLazyQueryReturnType
```

## Params

| Param                | Type                                                      | Default Value | Description                                          |
| -------------------- | --------------------------------------------------------- | ------------- | ---------------------------------------------------- |
| `query`              | `string`                                                  | -             | [Airstack API key](../../get-started/get-api-key.md) |
| `variables`          | [`Variables?`](../objects/variables.md)                   | `null`        | GraphQL query variables                              |
| `configAndCallbacks` | [`ConfigAndCallbacks?`](../objects/configandcallbacks.md) | `null`        | Additional configurations and callbacks.             |

## Responses

| Param      | Type                                                             | Default Value | Description                                                                                       |
| ---------- | ---------------------------------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------- |
| `response` | [`UseLazyQueryReturnType`](../objects/uselazyqueryreturntype.md) | -             | Response from the hook that will return an execute function, data, error logs, and loading state. |

