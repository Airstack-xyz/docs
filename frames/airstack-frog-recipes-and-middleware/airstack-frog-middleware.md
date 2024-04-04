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

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Airstack Frog Middleware Code Snippets</p></figcaption></figure>

## Onchain Data Middleware

You can use the [`onchainData`](https://www.npmjs.com/package/@airstack/frames#frog-middlewares) middleware to seamlessly incorporate the interactor's onchain data directly into your Farcaster Frames. Simply specify the onchain data to retrieve in the `features` field and integrate the middleware into the desired route for data access:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import { onchainData } from "@airstack/frog";

const onchainDataMiddleware = onchainData({
  features: {
    userDetails: {},
    erc20Mints: {
      chains: [TokenBlockchain.Base],
      limit: 1,
    },
    nftMints: {
      limit: 1,
      chains: [TokenBlockchain.Base],
    },
    erc20Balances: {
      chains: [TokenBlockchain.Base],
      limit: 1,
    },
    nftBalances: {
      limit: 1,
      chains: [TokenBlockchain.Base],
    },
    poaps: {
      limit: 1,
    },
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
const { onchainData } = require("@airstack/frog");

const onchainDataMiddleware = onchainData({
  apiKey: process.env.AIRSTACK_API_KEY,
  features: {
    userDetails: {},
    erc20Mints: {
      chains: [TokenBlockchain.Base],
      limit: 1,
    },
    nftMints: {
      limit: 1,
      chains: [TokenBlockchain.Base],
    },
    erc20Balances: {
      chains: [TokenBlockchain.Base],
      limit: 1,
    },
    nftBalances: {
      limit: 1,
      chains: [TokenBlockchain.Base],
    },
    poaps: {
      limit: 1,
    },
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
  },
  "erc20Balances": [
    {
      "blockchain": "base",
      "tokenAddress": "0x10503dbed34e291655100a3c204528425abe3235",
      "amount": 740,
      "amountInWei": "740000000000000000000",
      "name": "am00r",
      "symbol": "AM00R"
    }
  ],
  "nftBalances": [
    {
      "blockchain": "base",
      "tokenAddress": "0xbd2019982628d26786d75e4477daf15489260d66",
      "tokenId": "1",
      "amount": 1,
      "amountInWei": "1",
      "name": "Doom scrolling",
      "symbol": "",
      "image": {},
      "metaData": {},
      "tokenType": "ERC1155"
    }
  ],
  "erc20Mints": [
    {
      "blockchain": "base",
      "tokenAddress": "0x058d96baa6f9d16853970b333ed993acc0c35add",
      "amount": 50,
      "amountInWei": "50000000000000000000",
      "name": "Staked SPORK",
      "symbol": "sSPORK",
      "blockTimestamp": "2024-01-03T18:43:02Z",
      "blockNumber": 51901326
    }
  ],
  "nftMints": [
    {
      "blockchain": "base",
      "tokenAddress": "0xb3da098a7251a647892203e0c256b4398d131a54",
      "tokenId": "4442",
      "tokenType": "ERC721",
      "amount": 1,
      "amountInWei": "1",
      "name": "Mint A Penny",
      "symbol": "PENNY",
      "blockTimestamp": "2024-02-12T22:43:35Z",
      "blockNumber": 10494234,
      "image": {},
      "metaData": {}
    }
  ],
  "poaps": [
    {
      "eventName": "You have met Patricio in February of 2024 (IRL)",
      "eventId": "167539",
      "eventURL": "https://POAP.xyz",
      "isVirtualEvent": false,
      "startDate": "2024-02-01T00:00:00Z",
      "endDate": "2024-03-01T00:00:00Z",
      "city": ""
    }
  ]
}
```

{% endtab %}
{% endtabs %}

## Allow List Middleware

Enhance your Farcaster Frames with user's onchain data using the [`allowList`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware) middleware from Airstack Frog Recipes. Effortlessly set up by specifying your criteria in the `allowListCriteria` field and incorporating the middleware into the route where you wish to access the data:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import { allowList } from "@airstack/frog";

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
const { allowList } = require("@airstack/frog");

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

If you have any questions or need help regarding building Farcaster Frames with Airstack Frog Middleware & Recipes, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Onchain Data Middleware Reference](https://www.npmjs.com/package/@airstack/frames#frog-middlewares)
- [Allow List Middleware Reference](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware)
