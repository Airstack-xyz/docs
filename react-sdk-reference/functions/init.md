---
description: >-
  Initialize the Airstack NodeJS SDK with Airstack API key. This is required to
  call the Airstack API.
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

# init

## Pre-requisites

* [Airstack API key](../../get-started/get-api-key.md)

## Example

```javascript
import { init } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY", { env: "dev", cache: true });
```

## Function Signature

```typescript
function init(
  key: string,
  _config?: Config
): void
```

## Params

| Param     | Type                              | Default Value                 | Description                                          |
| --------- | --------------------------------- | ----------------------------- | ---------------------------------------------------- |
| `key`     | `string`                          | `null`                        | [Airstack API key](../../get-started/get-api-key.md) |
| `_config` | [`Config?`](../objects/config.md) | `{ env: "dev", cache: true }` | SDK Configuration, e.g. logging, caching, etc.       |

