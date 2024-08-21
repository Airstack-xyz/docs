---
description: >-
  Learn how to use the Onchain Data Middleware to inject the onchain data of the
  user interacting with your Frames.js Frames.
---

# ⛓️ Farcaster Data

The Farcaster Data Frames.js middleware enables you to easily enrich your Frames.js Frames with the interactor's onchain data with just a few lines of code. Some data that you can inject into your Frames.js Frames are:

* Farcaster followers/followings
* Farcaster channels participated in
* Farcaster casts

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
    // Fetch the user's participated channels
    // from `ctx.farcasterChannels`
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

## Get Farcaster User's Cast

You can use the [`onchainDataFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#framesjs-middlewares) to fetch all the Farcaster casts casted by a user by adding `FARCASTER_CASTS` to the features' list:

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
    // Fetch the user's Farcaster casts from `ctx.farcasterCasts`
<strong>    console.log(ctx.farcasterCasts);
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
<strong>        features: [Features.FARCASTER_CASTS],
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
    // Fetch the user's Farcaster casts from `ctx.farcasterCasts`
<strong>    console.log(ctx.farcasterCasts);
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
        // Add `FARCASTER_CHANNELS` to the `features` array
<strong>        features: [Features.FARCASTER_CASTS],
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
    "castHash": "0xcee805b0b5a762892512d38d30b72dd692772480",
    "castedAtTimestamp": "2024-04-06T06:24:32Z",
    "castUrl": "https://warpcast.com/betashop.eth/0xcee805b0",
    "embeds": [{ "url": "https://share.airstack.xyz/s/gf" }],
    "text": "hihi follow my trade on @base! cc @betashop.eth @airstack",
    "numberOfRecasts": 17,
    "numberOfLikes": 92,
    "numberOfReplies": 14,
    "channel": "airstaack",
    "mentions": [
      { "fid": "12142", "position": 24 },
      { "fid": "602", "position": 29 },
      { "fid": "20909", "position": 30 }
    ],
    "frame": {
      "frameHash": "0xbbd09a3a2c6b96eff53d9ad622b5637374bd2ec7b9c706fd8c908a6bc1a6bdc0",
      "frameUrl": "https://share.airstack.xyz/s/gf"
    }
  }
]
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding building Farcaster Frames with Airstack Frames.js Middleware, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data Frames.js Middleware Reference](https://www.npmjs.com/package/@airstack/frames#onchain-data-middleware-1)
