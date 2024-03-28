---
description: >-
  Learn how Airstack automatically configures hubs for you and how you can add
  custom hubs configuration, as needed.
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

# 🛜 Farcaster Hubs

In Airstack Frog Recipes, Airstack Hubs is the default option available if you don't set anything to the `hub` field. All you need to provide in your new Frog instance will be the [Airstack API key](../../get-started/get-api-key.md):&#x20;

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Frog } from "@airstack/frog";

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { Frog } = require("@airstack/frog");

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY,
});
```
{% endtab %}
{% endtabs %}

You can also add custom Hub URL and additional headers for API keys:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Frog } from "@airstack/frog";
 
const app = new Frog({
  hub: {
    apiUrl: "CUSTOM_HUBS_URL",
    fetchOptions: {
      headers: {
        // API keys
      }
    }
  }
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { Frog } = require("@airstack/frog");
 
const app = new Frog({
  hub: {
    apiUrl: "CUSTOM_HUBS_URL",
    fetchOptions: {
      headers: {
        // API keys
      }
    }
  }
});
```
{% endtab %}
{% endtabs %}

Otherwise, Frog also has some pre-configured hubs that you can choose for your `hub` configuration:

{% embed url="https://frog.fm/concepts/securing-frames#supplying-hub-configuration" %}
Add Hubs Configuration to Frog
{% endembed %}

## Developer Support

If you have any questions or need help regarding Farcaster Hubs configuration for your Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](onchain-data.md)
* [Allow List](allow-list.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
