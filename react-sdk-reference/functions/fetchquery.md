---
description: >-
  Fetch data from Airstack by calling queries to the Airstack API. If you need
  any pagination, use fetchQueryWithPagination instead.
---

# fetchQuery

## Pre-requisites

* Calling [`init`](init.md) function

## Example

```javascript
import { fetchQuery } from "@airstack/airstack-react";

const { data, error } = await fetchQuery(query, variables, config);
```

## Function Signature

```typescript
fetchQuery(
  query: string,
  variables?: Variables,
  config: Config
): Promise<Pick<FetchQuery, "data" | "error">>
```

## Params

| Param       | Type                                   | Default Value | Description                                          |
| ----------- | -------------------------------------- | ------------- | ---------------------------------------------------- |
| `query`     | `string`                               | -             | [Airstack API key](../../get-started/get-api-key.md) |
| `variables` | [`Variables`](../objects/variables.md) | `null`        | GraphQL query variables                              |
| `config`    | `{ cache?: boolean }`                  | `null`        | Caching configuration                                |

## Responses

| Param      | Type                                                                           | Default Value | Description                           |
| ---------- | ------------------------------------------------------------------------------ | ------------- | ------------------------------------- |
| `response` | `Promise<Pick<`[`FetchQuery`](../objects/fetchquery.md)`, "data" \| "error">>` | `null`        | Response object from GraphQL `query`. |

