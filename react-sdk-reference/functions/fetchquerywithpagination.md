---
description: >-
  Fetch data from Airstack by calling queries to the Airstack API with
  pagination included. If you don't need pagination, use fetchQuery instead.
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

# fetchQueryWithPagination

## Pre-requisites

* Calling [`init`](init.md) function

## Example

```javascript
import { fetchQueryWithPagination } from "@airstack/airstack-react";

const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await fetchQueryWithPagination(query, variables, config);
```

## Function Signature

```typescript
function fetchQueryWithPagination(
  query: string,
  variables?: Variables,
  config?: { cache?: boolean }
): Promise<FetchQuery>
```

## Params

| Param       | Type                                   | Default Value | Description                                          |
| ----------- | -------------------------------------- | ------------- | ---------------------------------------------------- |
| `query`     | `string`                               | -             | [Airstack API key](../../get-started/get-api-key.md) |
| `variables` | [`Variables`](../objects/variables.md) | `null`        | GraphQL query variables                              |
| `config`    | `{ cache?: boolean }`                  | `null`        | Caching configuration                                |

## Responses

| Param      | Type                                                  | Default Value | Description                           |
| ---------- | ----------------------------------------------------- | ------------- | ------------------------------------- |
| `response` | `Promise<`[`FetchQuery`](../objects/fetchquery.md)`>` | `null`        | Response object from GraphQL `query`. |

