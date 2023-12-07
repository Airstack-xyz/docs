---
description: >-
  SDK initialization configuration object. Set environment type and caching
  options with Config.
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

# Config

## Example

```json
{
  "env": "dev",
  "cache": true
}
```

## Type Signature

```typescript
type Config = {
  env?: Env;
  cache?: boolean;
};
```

## Fields

| Param   | Type                     | Default Value | Description                                                                                       |
| ------- | ------------------------ | ------------- | ------------------------------------------------------------------------------------------------- |
| `env`   | [`Env`](../enums/env.md) | `"dev"`       | `dev` provides verbose logging. `prod` provides minimal logging, best for production environment. |
| `cache` | `boolean`                | `true`        | Caching configuration. `true` to cache results, otherwise set to `false`.                         |
