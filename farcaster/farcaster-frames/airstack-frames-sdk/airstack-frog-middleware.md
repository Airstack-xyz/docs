---
description: >-
  Build your Farcaster Frames with Airstack frog middleware. Seamlessly
  integrate profiles, follows, trending mints, tokens, and everything onchain.
---

# 🥪 Airstack Frog Middleware

{% embed url="https://drive.google.com/file/d/1zlOMMufSKMLwvmMBKRuKx0KShcLdT9Cb/view?usp=sharing" %}
Airstack Frog Middleware Demo
{% endembed %}

The Airstack Frog Middleware is available as a community-built middleware and can be easily incorporated into any Frog app by installing the [Airstack Frames SDK](https://www.npmjs.com/package/@airstack/frames). (The Airstack Frog Middleware can also be accessed via the [Airstack Frog Recipes](https://www.npmjs.com/package/@airstack/frog))

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Airstack Frog Middleware Code Snippets</p></figcaption></figure>

## Onchain Data Middleware

You can use the [`onchainData`](https://www.npmjs.com/package/@airstack/frames#frog-middlewares) middleware to seamlessly incorporate the interactor's onchain data directly into your Farcaster Frames. Simply specify the onchain data to retrieve in the `features` field and integrate the middleware into the desired route for data access:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { onchainData } from "@airstack/frames";

const onchainDataMiddleware = onchainData({
  features: {
    userDetails: {},
  },
  env: "dev",
});

app.frame(
  "/",
  // Add Onchain Data Middleware to the routes that need to access
  // User's onchain data
  onchainDataMiddleware,
  async function (c) {
    const { status } = c;
    if (status === "response") console.log(c.var);
    c.res({});
  }
);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { onchainData } = require("@airstack/frames");

const onchainDataMiddleware = onchainData({
  apiKey: process.env.AIRSTACK_API_KEY,
  features: {
    userDetails: {},
  },
  env: "dev",
});

app.frame("/", onchainDataMiddleware, async function (c) {
  const { status } = c;
  if (status === "response") console.log(c.var);
  c.res({});
});
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "userDetails": {
    "profileName": "betashop.eth",
    "fnames": ["betashop", "betashop.eth", "jasongoldberg.eth"],
    "profileImage": {
      "extraSmall": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/extra_small.jpg",
      "small": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/small.jpg",
      "medium": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/medium.jpg",
      "large": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/large.jpg",
      "original": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/original_image.jpg"
    },
    "userAssociatedAddresses": [
      "0x66bd69c7064d35d146ca78e6b186e57679fba249",
      "0xeaf55242a90bb3289db8184772b0b98562053559"
    ],
    "followerCount": 65820,
    "followingCount": 2303
  }
}
```
{% endtab %}
{% endtabs %}

## Allow List Middleware

Enhance your Farcaster Frames with user's onchain data using the [`allowList`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware) middleware from Airstack Frog Recipes. Effortlessly set up by specifying your criteria in the `allowListCriteria` field and incorporating the middleware into the route where you wish to access the data:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { allowList } from "@airstack/frames";

const allowListMiddleware = allowList({
  allowListCriteria: {
    eventIds: [166577],
    tokens: [
      {
        tokenAddress: "0xe03ef4b9db1a47464de84fb476f9baf493b3e886",
        chain: TokenBlockchain.Zora,
      },
    ],
  },
});

app.frame("/", allowListMiddleware, async function (c) {
  const { status } = c;
  if (status === "response") console.log(c.var);
  c.res({});
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { allowList } = require("@airstack/frames");

const allowListMiddleware = allowList({
  allowListCriteria: {
    eventIds: [166577],
    tokens: [
      {
        tokenAddress: "0xe03ef4b9db1a47464de84fb476f9baf493b3e886",
        chain: TokenBlockchain.Zora,
      },
    ],
  },
});

app.frame("/", allowListMiddleware, async function (c) {
  const { status } = c;
  if (status === "response") console.log(c.var);
  c.res({});
});
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "isAllowed": true
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding building Farcaster Frames with Airstack Frog Middleware, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data Middleware Reference](https://www.npmjs.com/package/@airstack/frames#frog-middlewares)
* [Allow List Middleware Reference](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware)
