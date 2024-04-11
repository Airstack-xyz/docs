---
description: >-
  Fetch data from Airstack by calling queries to the Airstack API. If you need
  any pagination, use fetchQueryWithPagination instead.
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

# fetchQuery

## Pre-requisites

* Calling [`init`](init.md) function

## Example

```javascript
import { fetchQuery } from "@airstack/node";

const { data, error } = await fetchQuery(query, variables);
```

## Function Signature

```typescript
type FetchQuery = {
  data: any;
  error: any;
}

type Variables = Record<string, any>

fetchQuery(
  query: string,
  variables?: Variables | undefined
): Promise<Pick<FetchQuery, "data" | "error">>
```

## Params

| Param       | Type                  | Default Value | Description             |
| ----------- | --------------------- | ------------- | ----------------------- |
| `query`     | `string`              |               | Airstack GraphQL query  |
| `variables` | `Record<string, any>` |               | GraphQL query variables |

## Responses

| Param   | Type   | Default Value | Description                                              |
| ------- | ------ | ------------- | -------------------------------------------------------- |
| `data`  | `any?` |               | Response data from GraphQL `query` if API call succeeds. |
| `error` | `any?` |               | Error logs from GraphQL query if API call failed.        |
