---
description: >-
  The return type of the useQuery hook comprises the data, error logs, and
  loading indicators.
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

# UseQueryReturnType

## Example

```json
{
  "data": {},
  "error": {},
  "loading": false
}
```

## Type Signature

```typescript
type UseQueryReturnType = {
  data: any;
  error: any;
  loading: boolean;
};
```

## Fields

| Param     | Type      | Default Value | Description                                                                          |
| --------- | --------- | ------------- | ------------------------------------------------------------------------------------ |
| `data`    | `any`     | `null`        | Response data from GraphQL `query` if API call succeeds.                             |
| `error`   | `any`     | `null`        | Error logs from GraphQL query if API call failed.                                    |
| `loading` | `boolean` | `false`       | Returns `true` to indicate if the API call is in a loading state, otherwise `false`. |
