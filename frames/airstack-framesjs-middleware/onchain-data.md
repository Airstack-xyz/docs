---
description: >-
  Learn how to use the Onchain Data Middleware to inject the user's onchain data
  into your Frames.js Frames.
---

# ⛓️ Onchain Data

## Get Started

First, install the Airstack Frames SDK:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/frames
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/frames
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @airstack/frames
```
{% endtab %}

{% tab title="bun" %}
```bash
bun install @airstack/frames
```
{% endtab %}
{% endtabs %}

## Get User's Farcaster Profile Details

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch Farcaster profile details by adding `USER_DETAILS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the Farcaster profile details data from `ctx.userDetails`
<strong>    console.log(ctx.userDetails);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `USER_DETAILS` to the `features` array
<strong>        features: [Features.USER_DETAILS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the Farcaster profile details data from `ctx.userDetails`
<strong>    console.log(ctx.userDetails);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `USER_DETAILS` to the `features` array
<strong>        features: [Features.USER_DETAILS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
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
```
{% endtab %}
{% endtabs %}

## Get User's ERC20 Balances

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch user's ERC20 balances by adding `ERC20_BALANCES` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's ERC20 balances from `ctx.erc20Balances`
<strong>    console.log(ctx.erc20Balances);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `ERC20_BALANCES` to the `features` array
<strong>        features: [Features.ERC20_BALANCES],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's ERC20 balances from `ctx.erc20Balances`
<strong>    console.log(ctx.erc20Balances);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `ERC20_BALANCES` to the `features` array
<strong>        features: [Features.ERC20_BALANCES],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "blockchain": "ethereum",
    "tokenAddress": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    "amount": 125,
    "amountInWei": "125000000",
    "name": "USD Coin",
    "symbol": "USDC"
  }
]
```
{% endtab %}
{% endtabs %}

## Get User's NFT Balances

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch user's NFT balances by adding `NFT_BALANCES` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT balances from `ctx.nftBalances`
<strong>    console.log(ctx.nftBalances);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `NFT_BALANCES` to the `features` array
<strong>        features: [Features.NFT_BALANCES],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT balances from `ctx.nftBalances`
<strong>    console.log(ctx.nftBalances);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `NFT_BALANCES` to the `features` array
<strong>        features: [Features.NFT_BALANCES],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "blockchain": "zora",
    "tokenAddress": "0xe03ef4b9db1a47464de84fb476f9baf493b3e886",
    "tokenId": "110",
    "amount": 1,
    "amountInWei": "1",
    "name": "Farcaster OG",
    "symbol": "$FCOG",
    "image": {
      "extraSmall": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/extra_small.png",
      "small": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/small.png",
      "medium": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/medium.png",
      "large": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/large.png",
      "original": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/original_image.png"
    },
    "metaData": {
      "name": "Farcaster OG 43",
      "description": "Celebrating Farcaster at permissionless.",
      "image": "ipfs://bafybeihbx6nx4h2wblf6nlsy6nkotzqynzsrgimgqzwqgw6gf7d27ewfqu",
      "imageData": "",
      "externalUrl": "",
      "animationUrl": "",
      "youtubeUrl": "",
      "backgroundColor": "",
      "attributes": null
    },
    "tokenType": "ERC721"
  }
]
```
{% endtab %}
{% endtabs %}

## Get User's ERC20 Mints

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch user's ERC20 mints by adding `ERC20_MINTS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's ERC20 mints from `ctx.erc20Mints`
<strong>    console.log(ctx.erc20Mints);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `ERC20_MINTS` to the `features` array
<strong>        features: [Features.ERC20_MINTS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's ERC20 mints from `ctx.erc20Mints`
<strong>    console.log(ctx.erc20Mints);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `ERC20_MINTS` to the `features` array
<strong>        features: [Features.ERC20_MINTS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
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
]
```
{% endtab %}
{% endtabs %}

## Get User's NFT Mints

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch user's NFT mints by adding `NFT_MINTS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT mints from `ctx.nftMints`
<strong>    console.log(ctx.nftMints);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `NFT_MINTS` to the `features` array
<strong>        features: [Features.NFT_MINTS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT mints from `ctx.nftMints`
<strong>    console.log(ctx.nftMints);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `NFT_MINTS` to the `features` array
<strong>        features: [Features.NFT_MINTS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "blockchain": "base",
    "tokenAddress": "0x7d5861cfe1c74aaa0999b7e2651bf2ebd2a62d89",
    "tokenId": "94613",
    "tokenType": "ERC721",
    "amount": 1,
    "amountInWei": "1",
    "name": "Base Day One",
    "symbol": "$BASEDAYONE",
    "blockTimestamp": "2023-08-11T08:23:43Z",
    "blockNumber": 2476438,
    "image": {
      "extraSmall": "https://assets.airstack.xyz/image/nft/8453/VsImj/jHMngFqJSF7KWaDce4NeMMkjjE6vKrM78PzD+4gKMh74NjmrMUXl9+slIronvYbTNTE8aDkB1TuYwHPA==/extra_small.gif",
      "small": "https://assets.airstack.xyz/image/nft/8453/VsImj/jHMngFqJSF7KWaDce4NeMMkjjE6vKrM78PzD+4gKMh74NjmrMUXl9+slIronvYbTNTE8aDkB1TuYwHPA==/small.gif",
      "medium": "https://assets.airstack.xyz/image/nft/8453/VsImj/jHMngFqJSF7KWaDce4NeMMkjjE6vKrM78PzD+4gKMh74NjmrMUXl9+slIronvYbTNTE8aDkB1TuYwHPA==/medium.gif",
      "large": "https://assets.airstack.xyz/image/nft/8453/VsImj/jHMngFqJSF7KWaDce4NeMMkjjE6vKrM78PzD+4gKMh74NjmrMUXl9+slIronvYbTNTE8aDkB1TuYwHPA==/large.gif",
      "original": "https://assets.airstack.xyz/image/nft/8453/VsImj/jHMngFqJSF7KWaDce4NeMMkjjE6vKrM78PzD+4gKMh74NjmrMUXl9+slIronvYbTNTE8aDkB1TuYwHPA==/original_image.gif"
    },
    "metaData": {
      "name": "Base Day One 94613",
      "description": "Base Day One commemorates the first day of Base. Watch it evolve as more people come onchain and collectively create our story. All proceeds will support the next generation of builders on Base; this does not confer any other rights. GET ONCHAIN at onchainsummer.xyz and mint to join us.",
      "image": "ipfs://bafybeidkxtd2qck3omiccqhi2iebklr5yfsm33vivmgyfarlh62l462zka",
      "imageData": "",
      "externalUrl": "",
      "animationUrl": "",
      "youtubeUrl": "",
      "backgroundColor": "",
      "attributes": null
    }
  }
]
```
{% endtab %}
{% endtabs %}

## Get User's POAPs

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch user's POAPs by adding `POAPS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's POAPs from `ctx.poaps`
<strong>    console.log(ctx.poaps);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `POAPS` to the `features` array
<strong>        features: [Features.POAPS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's POAPs from `ctx.poaps`
<strong>    console.log(ctx.poaps);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `POAPS` to the `features` array
<strong>        features: [Features.POAPS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "eventName": "ETHGlobal New York 2023 Speaker",
    "eventId": "151055",
    "eventURL": "https://ethglobal.com/events/newyork2023",
    "isVirtualEvent": false,
    "startDate": "2023-09-22T00:00:00Z",
    "endDate": "2023-09-25T00:00:00Z",
    "city": "New York City"
  }
]
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Sent From The User

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all token transfers sent from the user by adding `TOKEN_TRANSFERS_SENT` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's sent token transfers
    // from `ctx.tokenTransfersSent`
<strong>    console.log(ctx.tokenTransfersSent);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `TOKEN_TRANSFERS_SENT` to the `features` array
<strong>        features: [Features.TOKEN_TRANSFERS_SENT],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's sent token transfers
    // from `ctx.tokenTransfersSent`
<strong>    console.log(ctx.tokenTransfersSent);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `TOKEN_TRANSFERS_SENT` to the `features` array
<strong>        features: [Features.TOKEN_TRANSFERS_SENT],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "blockchain": "base",
    "tokenAddress": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
    "amount": 100,
    "amountInWei": "100000000",
    "name": "USD Coin",
    "symbol": "USDC",
    "blockTimestamp": "2023-12-18T15:15:35Z",
    "blockNumber": 8061594,
    "tokenType": "ERC20",
    "txHash": "0xf30a550eece968e1abdcae4de3bdb5f7b84f3d0b2335150149a7398b351567f5",
    "receiver": {
      "addresses": ["0x3a23f943181408eac424116af7b7790c94cb97a5"],
      "socials": null
    }
  }
]
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Received By The User

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all token transfers received by the user by adding `TOKEN_TRANSFERS_RECEIVED` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's token transfers received
    // from `ctx.tokenTransfersReceived`
<strong>    console.log(ctx.tokenTransfersReceived);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `TOKEN_TRANSFERS_RECEIVED` to the `features` array
<strong>        features: [Features.TOKEN_TRANSFERS_RECEIVED],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's token transfers received
    // from `ctx.tokenTransfersReceived`
<strong>    console.log(ctx.tokenTransfersReceived);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `TOKEN_TRANSFERS_RECEIVED` to the `features` array
<strong>        features: [Features.TOKEN_TRANSFERS_RECEIVED],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "blockchain": "zora",
    "tokenAddress": "0xe03ef4b9db1a47464de84fb476f9baf493b3e886",
    "amount": 1,
    "amountInWei": "1",
    "name": "Farcaster OG",
    "symbol": "$FCOG",
    "blockTimestamp": "2023-10-11T21:02:39Z",
    "blockNumber": 5182160,
    "tokenType": "ERC721",
    "txHash": "0x116d7d7d2f6e8adb7b6991348ff1869742dae538f0b68f36624ed2496bc2091e",
    "sender": {
      "addresses": ["0x3a23f943181408eac424116af7b7790c94cb97a5"],
      "socials": null
    },
    "metaData": {
      "name": "Farcaster OG 43",
      "description": "Celebrating Farcaster at permissionless.",
      "image": "ipfs://bafybeihbx6nx4h2wblf6nlsy6nkotzqynzsrgimgqzwqgw6gf7d27ewfqu",
      "imageData": "",
      "externalUrl": "",
      "animationUrl": "",
      "youtubeUrl": "",
      "backgroundColor": "",
      "attributes": null
    },
    "image": {
      "extraSmall": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/extra_small.png",
      "small": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/small.png",
      "medium": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/medium.png",
      "large": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/large.png",
      "original": "https://assets.airstack.xyz/image/nft/7777777/PtYr9f5cHxXadiklS+Xzp805o/lFKCmd1jvpLmU58tO5UgOEdm56cjqIt1Gf/UK052NE4yYf2xmpwwrzjcDl+w==/original_image.png"
    },
    "tokenId": "110"
  }
]
```
{% endtab %}
{% endtabs %}

## Get User's Farcaster Followers

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all user's Farcaster followers by adding `FARCASTER_FOLLOWERS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's Farcaster followers
    // from `ctx.farcasterFollowers`
<strong>    console.log(ctx.farcasterFollowers);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `FARCASTER_FOLLOWERS` to the `features` array
<strong>        features: [Features.FARCASTER_FOLLOWERS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's Farcaster followers
    // from `ctx.farcasterFollowers`
<strong>    console.log(ctx.farcasterFollowers);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `FARCASTER_FOLLOWERS` to the `features` array
<strong>        features: [Features.FARCASTER_FOLLOWERS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "profileName": "allison985",
    "fnames": ["allison985"],
    "fid": "220757",
    "profileImage": {
      "extraSmall": "https://assets.airstack.xyz/image/social/RS9r7sdCb5orXeB0+tLLRPxtnJo80la3zRRVAYc9gPR+ne8TitCLgEJ41Gp1LV6g/extra_small.jpg",
      "small": "https://assets.airstack.xyz/image/social/RS9r7sdCb5orXeB0+tLLRPxtnJo80la3zRRVAYc9gPR+ne8TitCLgEJ41Gp1LV6g/small.jpg",
      "medium": "https://assets.airstack.xyz/image/social/RS9r7sdCb5orXeB0+tLLRPxtnJo80la3zRRVAYc9gPR+ne8TitCLgEJ41Gp1LV6g/medium.jpg",
      "large": "https://assets.airstack.xyz/image/social/RS9r7sdCb5orXeB0+tLLRPxtnJo80la3zRRVAYc9gPR+ne8TitCLgEJ41Gp1LV6g/large.jpg",
      "original": "https://assets.airstack.xyz/image/social/RS9r7sdCb5orXeB0+tLLRPxtnJo80la3zRRVAYc9gPR+ne8TitCLgEJ41Gp1LV6g/original_image.jpg"
    },
    "userAssociatedAddresses": ["0x42fae5a53f0194f6f9587926e206a852c5c726bf"],
    "followerCount": 1,
    "followingCount": 74
  }
]
```
{% endtab %}
{% endtabs %}

## Get User's Farcaster Followings

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all user's Farcaster followings by adding `FARCASTER_FOLLOWINGS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's Farcaster Followings from `ctx.farcasterFollowings`
<strong>    console.log(ctx.farcasterFollowings);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `FARCASTER_FOLLOWINGS` to the `features` array
<strong>        features: [Features.FARCASTER_FOLLOWINGS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's Farcaster Followings from `ctx.farcasterFollowings`
<strong>    console.log(ctx.farcasterFollowings);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
        // Add `FARCASTER_FOLLOWINGS` to the `features` array
<strong>        features: [Features.FARCASTER_FOLLOWINGS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "profileName": "jayhuq",
    "fnames": ["jayhuq"],
    "fid": "1775",
    "profileImage": {
      "extraSmall": "https://assets.airstack.xyz/image/social/HmDDiN8HQWR/6f6nrPI8+P6fwctlKaEu/dM8+QnZz/Y=/extra_small.png",
      "small": "https://assets.airstack.xyz/image/social/HmDDiN8HQWR/6f6nrPI8+P6fwctlKaEu/dM8+QnZz/Y=/small.png",
      "medium": "https://assets.airstack.xyz/image/social/HmDDiN8HQWR/6f6nrPI8+P6fwctlKaEu/dM8+QnZz/Y=/medium.png",
      "large": "https://assets.airstack.xyz/image/social/HmDDiN8HQWR/6f6nrPI8+P6fwctlKaEu/dM8+QnZz/Y=/large.png",
      "original": "https://assets.airstack.xyz/image/social/HmDDiN8HQWR/6f6nrPI8+P6fwctlKaEu/dM8+QnZz/Y=/original_image.png"
    },
    "userAssociatedAddresses": ["0xda52abca28fadeab9771ba45a2ff346c4db97d7f"],
    "followerCount": 58,
    "followingCount": 0
  }
]
```
{% endtab %}
{% endtabs %}

## Get Farcaster Channels Participated By User

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all the Farcaster channels the user participated in by adding `FARCASTER_CHANNELS` to the features' list:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT balances from `ctx.farcasterChannels`
<strong>    console.log(ctx.farcasterChannels);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
<strong>      onchainData({
</strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY as string,
        // Add `FARCASTER_CHANNELS` to the `features` array
<strong>        features: [Features.FARCASTER_CHANNELS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  onchainDataFramesjsMiddleware as onchainData,
  Features,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Fetch the user's NFT balances from `ctx.farcasterChannels`
<strong>    console.log(ctx.farcasterChannels);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Add Onchain Data Middleware
      onchainData({
<strong>        apiKey: process.env.NEXT_PUBLIC_AIRSTACK_API_KEY,
</strong>        // Add `FARCASTER_CHANNELS` to the `features` array
<strong>        features: [Features.FARCASTER_CHANNELS],
</strong>      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "name": "Based Management",
    "description": "Things worth doing onchain.",
    "imageUrl": "https://i.imgur.com/f0BFBfH.png",
    "createdAtTimestamp": "2023-11-06T19:23:10Z",
    "hosts": [
      {
        "profileName": "lght.eth",
        "fnames": ["0xlght", "lght.eth"],
        "fid": "13121",
        "profileImage": {
          "extraSmall": "https://assets.airstack.xyz/image/social/sxSmw/OjqyuT+uMDpHiSTmqOH5F76hwnx6Q35elGlUkt5nWRe8xrgnJemShOmjeN/extra_small.jpg",
          "small": "https://assets.airstack.xyz/image/social/sxSmw/OjqyuT+uMDpHiSTmqOH5F76hwnx6Q35elGlUkt5nWRe8xrgnJemShOmjeN/small.jpg",
          "medium": "https://assets.airstack.xyz/image/social/sxSmw/OjqyuT+uMDpHiSTmqOH5F76hwnx6Q35elGlUkt5nWRe8xrgnJemShOmjeN/medium.jpg",
          "large": "https://assets.airstack.xyz/image/social/sxSmw/OjqyuT+uMDpHiSTmqOH5F76hwnx6Q35elGlUkt5nWRe8xrgnJemShOmjeN/large.jpg",
          "original": "https://assets.airstack.xyz/image/social/sxSmw/OjqyuT+uMDpHiSTmqOH5F76hwnx6Q35elGlUkt5nWRe8xrgnJemShOmjeN/original_image.jpg"
        },
        "userAssociatedAddresses": [
          "0x53667ed77b56d5a94d6df994ab4fd142b7585e68",
          "0x547a2e8d97dc99be21e509fa93c4fa5dd76b8ed0"
        ],
        "followerCount": 16127,
        "followingCount": 345
      }
    ],
    "warpcastUrl": "https://warpcast.com/~/channel/based-management"
  }
]
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding building Farcaster Frames with Airstack Frames.js Middleware, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data Frames.js Middleware Reference](https://www.npmjs.com/package/@airstack/frames#onchain-data-middleware-1)
