---
description: >-
  Set caching option, callbacks, and data formatting function when querying data
  from Airstack API.
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

# ConfigAndCallbacks

## Example

```json
{
  "cache": true,
  "onCompleted": (data) => {},
  "onError": (error) => {},
  "dataFormatter": (data) => {}
}
```

## Type Signature

```typescript
type ConfigAndCallbacks = {
  cache?: boolean;
  onCompleted?: (data: any) => void;
  onError?: (error: any) => void;
  dataFormatter?: any;
}
```

## Fields

| Param           | Type                   | Default Value | Description                                                                           |
| --------------- | ---------------------- | ------------- | ------------------------------------------------------------------------------------- |
| `cache`         | `boolean`              | `true`        | Caching configuration. `true` to cache results, otherwise set to `false`.             |
| `onCompleted`   | `(data: any) => void`  | -             | A callback function that will be called when the query is successfully completed.     |
| `onError`       | `(error: any) => void` | -             | A callback function that will be called when an error occurs during the query.        |
| `dataFormatter` | `(data: any) => any`   | -             | A function that allows custom formatting of the data before returning it to the user. |
