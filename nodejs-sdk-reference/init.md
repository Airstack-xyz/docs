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

* [Airstack API key](../get-api-key.md)

## Example

```javascript
import { init } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY", "dev");
```

## Function Signature

```typescript
function init(
  key: string,
  env?: "dev" | "prod"
): void
```

## Params

| Param | Type          | Default Value | Description                                                                                       |
| ----- | ------------- | ------------- | ------------------------------------------------------------------------------------------------- |
| `key` | `string`      |               | [Airstack API key](../get-api-key.md)                                                             |
| `env` | `dev \| prod` | `dev`         | `dev` provides verbose logging. `prod` provides minimal logging, best for production environment. |
