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
import { fetchQueryWithPagination } from "@airstack/node";

const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await fetchQueryWithPagination(query, variables);
```

## Function Signature

```typescript
type FetchQuery = {
  data: any;
  error: any;
  hasNextPage: boolean;
  hasPrevPage: boolean;
  getNextPage: () => Promise<FetchQuery | null>;
  getPrevPage: () => Promise<FetchQuery | null>;
}

type Variables = Record<string, any>

function fetchQueryWithPagination(
  query: string,
  variables?: Variables
): Promise<FetchQuery>
```

## Params

| Param       | Type                  | Default Value | Description             |
| ----------- | --------------------- | ------------- | ----------------------- |
| `query`     | `string`              |               | Airstack GraphQL query  |
| `variables` | `Record<string, any>` |               | GraphQL query variables |

## Responses

| Param         | Type                                | Default Value | Description                                              |
| ------------- | ----------------------------------- | ------------- | -------------------------------------------------------- |
| `data`        | `any?`                              |               | Response data from GraphQL `query` if API call succeeds. |
| `error`       | `any?`                              |               | Error logs from GraphQL query if API call failed.        |
| `hasNextPage` | `boolean`                           | false         | Indicate if there is any following page.                 |
| `hasPrevPage` | `boolean`                           | false         |                                                          |
| `getNextPage` | `() => Promise<FetchQuery \| null>` |               | Function to get data in the next page.                   |
| `getPrevPage` | `() => Promise<FetchQuery \| null>` |               | Function to get data in the previous page.               |
