---
description: >-
  Use Authentication to verify payload request coming to your endpoint is coming
  from Airstack.
---

# Authentication

## Example

```json
{
  "header_name": "x-airstack-webhook",
  "header_value": "0L9wprY8VIlR7jZT"
}
```

## Fields

| Name           | Value    | Description                                                                     |
| -------------- | -------- | ------------------------------------------------------------------------------- |
| `header_name`  | `String` | `x-airstack-webhook`                                                            |
| `header_value` | `String` | Authentication header value for verifying payload request coming from Airstack. |
