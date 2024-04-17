---
description: >-
  Learn how you can integrate various onchain data into your Frames easily using
  the Airstack Frames SDK.
---

# ⛓️ Onchain Kit

The Airstack Frames SDK allows you to easily fetch any onchain data in your Frames.

Here are some onchain data that you can integrate into your Frames:

- Get Trending Mints For Farcaster Users on Base
- Get All POAPs Attended By Farcaster User
- Get Farcaster User's Token Holdings on Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains)
- Get Farcaster User's Token Mints on Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains)
- Get Farcaster User's Token Transfers Sent on Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains)
- Get Farcaster User's Followers and Followings
- Get Farcaster Channels Participated and Hosted By Farcaster Users

{% embed url="https://drive.google.com/file/d/1Y-jOgOKTP7XYh6bFyzD-evKeoVwsprfh/view?usp=sharing" %}

## Get Started

First, install the Airstack Frog Recipes:

{% tabs %}
{% tab title="npm" %}

```bash
npm install @airstack/frog hono
```

{% endtab %}

{% tab title="yarn" %}

```bash
yarn add @airstack/frog hono
```

{% endtab %}

{% tab title="pnpm" %}

```bash
pnpm install @airstack/frog hono
```

{% endtab %}

{% tab title="bun" %}

```bash
bun install @airstack/frog hono
```

{% endtab %}
{% endtabs %}

## getTrendingMints

You can easily fetch trending mints for Farcaster user on Base by using the [`getTrendingMints`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#gettrendingmints) function, which abstract away all the complexity for you:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getTrendingMints,
  GetTrendingMintsInput,
  GetTrendingMintsOutput,
  Audience,
  Criteria,
  TimeFrame,
} from "@airstack/frames";

const input: GetTrendingMintsInput = {
  audience: Audience.All,
  criteria: Criteria.UniqueWallets,
  timeFrame: TimeFrame.OneDay,
  limit: 100,
};
const { data, error }: GetTrendingMintsOutput = await getTrendingMints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getTrendingMints,
  Audience,
  Criteria,
  TimeFrame,
} = require("@airstack/frames");

const input = {
  audience: Audience.All,
  criteria: Criteria.UniqueWallets,
  timeFrame: TimeFrame.OneDay,
  limit: 100,
};
const { data, error } = await getTrendingMints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Response" %}

```json
[
  {
    "address": "0x9d70ccd59b3124e5227a9148413892f947697afd",
    "erc1155TokenID": "",
    "criteriaCount": 1880,
    "timeFrom": "2024-03-07T15:17:00Z",
    "timeTo": "2024-03-08T14:52:00Z",
    "name": "Base's 2024 Mission, Strategy and Roadmap",
    "symbol": "BASES2024MISSIONSTRATEGYANDROADMAP",
    "type": "ERC721"
  }
]
```

{% endtab %}
{% endtabs %}

## getFarcasterUserPoaps

You can fetch all POAPs owned by a Farcaster user using the [`getFarcasterUserPoaps`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasteruserpoaps) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserPoaps,
  FarcasterUserPoapsInput,
  FarcasterUserPoapsOutput,
} from "@airstack/frames";

const input: FarcasterUserPoapsInput = {
  fid: 602,
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserPoapsOutput = await getFarcasterUserPoaps(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterUserPoaps } = require("@airstack/frames");

const input = {
  fid: 602,
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserPoaps(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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
  // More POAPs
]
```

{% endtab %}
{% endtabs %}

## getFarcasterUserNFTBalances

You can fetch all NFTs owned by a Farcaster user using the [`getFarcasterUserNFTBalances`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusernftbalances) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserNFTBalances,
  FarcasterUserNFTBalancesInput,
  FarcasterUserNFTBalancesOutput,
  TokenBlockchain,
  NFTType,
} from "@airstack/frames";

const variables: FarcasterUserNFTBalancesInput = {
  fid: 602,
  tokenType: [NFTType.ERC721, NFTType.ERC1155],
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserNFTBalancesOutput = await getFarcasterUserNFTBalances(
  variables
);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserNFTBalances,
  TokenBlockchain,
  NFTType,
} = require("@airstack/frames");

const variables = {
  fid: 602,
  tokenType: [NFTType.ERC721, NFTType.ERC1155],
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserNFTBalances(variables);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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
  // Other NFTs
]
```

{% endtab %}
{% endtabs %}

## getFarcasterUserERC20Balances

You can fetch all ERC20 tokens owned by a Farcaster user using the [`getFarcasterUserERC20Balances`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusererc20balances) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserERC20Balances,
  FarcasterUserERC20BalancesInput,
  FarcasterUserERC20BalancesOutput,
  TokenBlockchain,
} from "@airstack/frames";

const input: FarcasterUserERC20BalancesInput = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserERC20BalancesOutput = await getFarcasterUserERC20Balances(
  input
);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserERC20Balances,
  TokenBlockchain,
} = require("@airstack/frames");

const input = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserERC20Balances(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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
  // Other ERC20 tokens
]
```

{% endtab %}
{% endtabs %}

## getFarcasterUserNFTMints

You can fetch all NFTs minted by a Farcaster user across multiple chains, such as Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains), by using the [`getFarcasterUserNFTMints`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusernftmints) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserNFTMints,
  FarcasterUserNFTMintsInput,
  FarcasterUserNFTMintsOutput,
  TokenBlockchain,
  NFTType,
} from "@airstack/frames";

const input: FarcasterUserNFTMintsInput = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [NFTType.ERC721, NFTType.ERC1155],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserNFTMintsOutput = await getFarcasterUserNFTMints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserNFTMints,
  TokenBlockchain,
  NFTType,
} = require("@airstack/frames");

const input = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [NFTType.ERC721, NFTType.ERC1155],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserNFTMints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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

## getFarcasterUserERC20Mints

You can fetch all ERC20 tokens minted by a Farcaster user across multiple chains, such as Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains), by using the [`getFarcasterUserERC20Mints`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusererc20mints) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserERC20Mints,
  FarcasterUserERC20MintsInput,
  FarcasterUserERC20MintsOutput,
  TokenBlockchain,
} from "@airstack/frames";

const input: FarcasterUserERC20MintsInput = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserERC20MintsOutput = await getFarcasterUserERC20Mints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserERC20Mints,
  TokenBlockchain,
} = require("@airstack/frames");

const input = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserERC20Mints(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Response" %}

```json
[
  {
    "blockchain": "Gold",
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

## getFarcasterUserTokenSentFrom

You can fetch all token transfers sent by a given Farcaster user across multiple chains, such as Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains), by using the [`getFarcasterUserTokenSentFrom`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusertokensentfrom) functions:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserTokenSentFrom,
  FarcasterUserTokenSentFromInput,
  FarcasterUserTokenSentFromOutput,
  TokenBlockchain,
  TokenType,
} from "@airstack/frames";

const input: FarcasterUserTokenSentFromInput = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [TokenType.ERC20, TokenType.ERC721, TokenType.ERC1155],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserTokenSentFromOutput = await getFarcasterUserTokenSentFrom(
  input
);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserTokenSentFrom,
  TokenBlockchain,
  TokenType,
} = require("@airstack/frames");

const input = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [TokenType.ERC20, TokenType.ERC721, TokenType.ERC1155],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserTokenSentFrom(input);

if (error) throw new Error(error);

console.log(data);
```

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

## getFarcasterUserTokenReceivedBy

You can fetch all token transfers received by a given Farcaster user across multiple chains, such as Ethereum, Base, Degen Chain, and other [Airstack-supported chains](overview.md#supported-chains) by using the [`getFarcasterUserTokenReceivedBy`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusertokenreceivedby) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserTokenReceivedBy,
  FarcasterUserTokenReceivedByInput,
  FarcasterUserTokenReceivedByOutput,
  TokenBlockchain,
  TokenType,
} from "@airstack/frames";

const input: FarcasterUserTokenReceivedByInput = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [TokenType.ERC20, TokenType.ERC721, TokenType.ERC1155],
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterUserTokenReceivedByOutput = await getFarcasterUserTokenReceivedBy(
  input
);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterUserTokenReceivedBy,
  TokenBlockchain,
  TokenType,
} = require("@airstack/frames");

const input = {
  fid: 602,
  chains: [
    TokenBlockchain.Ethereum,
    TokenBlockchain.Gold,
    TokenBlockchain.Base,
    TokenBlockchain.Zora,
  ],
  tokenType: [TokenType.ERC20, TokenType.ERC721, TokenType.ERC1155],
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterUserTokenReceivedBy(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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

## getFarcasterUserDetails

You can fetch the Farcaster user details, including profile name, fnames, profile image, user connected addresses, follower count, and following count by using the [`getFarcasterUserDetails`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#getfarcasteruserdetails) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterUserDetails,
  FarcasterUserDetailsInput,
  FarcasterUserDetailsOutput,
} from "@airstack/frames";

const input: FarcasterUserDetailsInput = {
  fid: 602,
};
const { data, error }: FarcasterUserDetailsOutput =
  await getFarcasterUserDetails(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterUserDetails } = require("@airstack/frames");

const input = {
  fid: 602,
};
const { data, error } = await getFarcasterUserDetails(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

```json
{
  "profileName": "betashop.eth",
  "fnames": ["betashop", "betashop.eth", "jasongoldberg.eth"],
  "profileImage": {
    "extraSmall": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/extra_small.png",
    "small": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/small.png",
    "medium": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/medium.png",
    "large": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/large.png",
    "original": "https://assets.airstack.xyz/image/social/TQjjhuaajVkwqgzZVvgFQYU1qxNfVHQgSmZjTcXRrzQ=/original_image.png"
  },
  "userAssociatedAddresses": [
    "0x66bd69c7064d35d146ca78e6b186e57679fba249",
    "0xeaf55242a90bb3289db8184772b0b98562053559"
  ],
  "followerCount": 56141,
  "followingCount": 2270
}
```

{% endtab %}
{% endtabs %}

## getFarcasterFollowers

You can fetch all the users following 0x address on Farcaster by using the [`getFarcasterFollowers`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#getfarcasterfollowers) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterFollowers,
  FarcasterFollowersInput,
  FarcasterFollowersOutput,
} from "@airstack/frames";

const input: FarcasterFollowersInput = {
  fid: 602,
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterFollowersOutput = await getFarcasterFollowers(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterFollowers } = require("@airstack/frames");

const input = {
  fid: 602,
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterFollowers(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Result" %}

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
  // More followers
]
```

{% endtab %}
{% endtabs %}

## getFarcasterFollowings

You can fetch all the users being followed by 0x address on Farcaster by using the [`getFarcasterFollowings`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#getfarcasterfollowings) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterFollowings,
  FarcasterFollowingsInput,
  FarcasterFollowingsOutput,
} from "@airstack/frames";

const input: FarcasterFollowingsInput = {
  fid: 602,
  limit: 100,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: FarcasterFollowingsOutput = await getFarcasterFollowings(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterFollowings } = require("@airstack/frames");

const input = {
  fid: 602,
  limit: 100,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await getFarcasterFollowings(input);

if (error) throw new Error(error);

console.log(data);
```

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
  // More followings
]
```

{% endtab %}
{% endtabs %}

## getFarcasterChannelDetails

You can fetch a certain channel details by using the [`getFarcasterChannelDetails`](https://github.com/Airstack-xyz/airstack-frames-sdk#getfarcasterchanneldetails) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterChannelDetails,
  FarcasterChannelDetailsInput,
  FarcasterChannelDetailsOutput,
} from "@airstack/frames";

const input: FarcasterChannelDetailsInput = {
  channel: "farcaster",
};
const { data, error }: FarcasterChannelDetailsOutput =
  await getFarcasterChannelDetails(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterChannelDetails } = require("@airstack/frames");

const input = {
  channel: "farcaster",
};
const { data, error } = await getFarcasterChannelDetails(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "name": "Farcaster",
  "description": "Discussions about Farcaster on Farcaster (meta!)",
  "imageUrl": "https://ipfs.decentralized-content.com/ipfs/bafkreialf5usxssf2eu3e5ct37zzdd553d7lg7oywvdszmrg5p2zpkta7u",
  "createdAtTimestamp": "2023-08-02T22:33:26Z",
  "hosts": [
    {
      "profileName": "v",
      "fnames": ["v", "varunsrin.eth"],
      "fid": "2",
      "profileImage": {
        "extraSmall": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/extra_small.png",
        "small": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/small.png",
        "medium": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/medium.png",
        "large": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/large.png",
        "original": "https://assets.airstack.xyz/image/social/XCPJH5EP49qftYc7+wAFfv5jzo3ddBWc9FMEERWezG8=/original_image.png"
      },
      "userAssociatedAddresses": [
        "0x4114e33eb831858649ea3702e1c9a2db3f626446",
        "0x91031dcfdea024b4d51e775486111d2b2a715871",
        "0x182327170fc284caaa5b1bc3e3878233f529d741",
        "0xf86a7a5b7c703b1fd8d93c500ac4cc75b67477f0"
      ],
      "followerCount": 142424,
      "followingCount": 1127
    }
  ],
  "warpcastUrl": "https://warpcast.com/~/channel/farcaster"
}
```

{% endtab %}
{% endtabs %}

## getFarcasterChannelParticipants

You can fetch all the participants of a channel by using the [`getFarcasterChannelParticipants`](https://github.com/Airstack-xyz/airstack-frames-sdk#getfarcasterchannelparticipants) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterChannelParticipants,
  FarcasterChannelParticipantsInput,
  FarcasterChannelParticipantsOutput,
  FarcasterChannelActionType,
} from "@airstack/frames";

const input: FarcasterChannelParticipantsInput = {
  channel: "farcaster",
  actionType: [
    FarcasterChannelActionType.Cast,
    FarcasterChannelActionType.Reply,
  ],
  lastActionTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 100,
};
const { data, error }: FarcasterChannelParticipantsOutput =
  await getFarcasterChannelParticipants(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterChannelParticipants,
  FarcasterChannelActionType,
} = require("@airstack/frames");

const input = {
  channel: "farcaster",
  actionType: [
    FarcasterChannelActionType.Cast,
    FarcasterChannelActionType.Reply,
  ],
  lastActionTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 100,
};
const { data, error } = await getFarcasterChannelParticipants(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Response" %}

```json
[
  {
    "profileName": "dawufi",
    "fnames": ["dawufi"],
    "fid": "6806",
    "profileImage": {
      "extraSmall": "https://assets.airstack.xyz/image/social/94uonfbLlRHZf6qh2LpPC6Fg4DNg3uCUrXkwlo+jA/I=/extra_small.gif",
      "small": "https://assets.airstack.xyz/image/social/94uonfbLlRHZf6qh2LpPC6Fg4DNg3uCUrXkwlo+jA/I=/small.gif",
      "medium": "https://assets.airstack.xyz/image/social/94uonfbLlRHZf6qh2LpPC6Fg4DNg3uCUrXkwlo+jA/I=/medium.gif",
      "large": "https://assets.airstack.xyz/image/social/94uonfbLlRHZf6qh2LpPC6Fg4DNg3uCUrXkwlo+jA/I=/large.gif",
      "original": "https://assets.airstack.xyz/image/social/94uonfbLlRHZf6qh2LpPC6Fg4DNg3uCUrXkwlo+jA/I=/original_image.gif"
    },
    "userAssociatedAddresses": [
      "0xe1b1e3bbf4f29bd7253d6fc1e2ddc9cacb0a546a",
      "0x0964256674e42d61f0ff84097e28f65311786ccb"
    ],
    "followerCount": 14813,
    "followingCount": 1551
  }
]
```

{% endtab %}
{% endtabs %}

## getFarcasterChannelsByParticipant

You can fetch all the Farcaster channels of a given participants by using the [`getFarcasterChannelsByParticipant`](https://github.com/Airstack-xyz/airstack-frames-sdk#getfarcasterchannelsbyparticipant) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterChannelsByParticipant,
  FarcasterChannelActionType,
  FarcasterChannelsByParticipantInput,
  FarcasterChannelsByParticipantOutput,
} from "@airstack/frames";

const input: FarcasterChannelsByParticipantInput = {
  fid: 602,
  actionType: [
    FarcasterChannelActionType.Cast,
    FarcasterChannelActionType.Reply,
  ],
  lastActionTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 100,
};
const { data, error }: FarcasterChannelsByParticipantOutput =
  await getFarcasterChannelsByParticipant(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  getFarcasterChannelsByParticipant,
  FarcasterChannelActionType,
} = require("@airstack/frames");

const input = {
  fid: 602,
  actionType: [
    FarcasterChannelActionType.Cast,
    FarcasterChannelActionType.Reply,
  ],
  lastActionTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 100,
};
const { data, error } = await getFarcasterChannelsByParticipant(input);

if (error) throw new Error(error);

console.log(data);
```

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

## getFarcasterChannelsByHost

You can fetch all the Farcaster channels of a given hosts by using the [`getFarcasterChannelsByHost`](https://github.com/Airstack-xyz/airstack-frames-sdk#getfarcasterchannelsbyhost) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  getFarcasterChannelsByHost,
  FarcasterChannelsByHostInput,
  FarcasterChannelsByHostOutput,
} from "@airstack/frames";

const input: FarcasterChannelsByHostInput = {
  fid: 602,
  createdAtTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 1,
};
const { data, error }: FarcasterChannelsByHostOutput =
  await getFarcasterChannelsByHost(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const { getFarcasterChannelsByHost } = require("@airstack/frog");

const input = {
  fid: 602,
  createdAtTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 1,
};
const { data, error } = await getFarcasterChannelsByHost(input);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="Response" %}

```json
[
  {
    "name": "airstack",
    "description": "a place for updates about new airstack functionality, user requests, questions, and more!",
    "imageUrl": "https://i.imgur.com/13jY9D4.png",
    "createdAtTimestamp": "2023-12-21T17:30:38Z",
    "warpcastUrl": "https://warpcast.com/~/channel/airstack"
  }
]
```

{% endtab %}
{% endtabs %}

## getFarcasterUserCasts

You can fetch all the Farcaster casts of a Farcaster user by using the [`getFarcasterUserCasts`](https://www.npmjs.com/package/@airstack/frames#getfarcasterusercasts) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  init,
  getFarcasterUserCasts,
  FarcasterUserCastsInput,
  FarcasterUserCastsOutput,
} from "@airstack/frames";

const input: FarcasterUserCastsInput = {
  fid: 602,
  hasEmbeds: true,
  hasFrames: true,
  hasMentions: true,
  limit: 100,
};
const { data, error }: FarcasterUserCastsOutput = await getFarcasterUserCasts(
  input
);

if (error) throw new Error(error);

console.log(data);
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const {
  init,
  getFarcasterUserCasts,
  FarcasterUserCastsInput,
  FarcasterUserCastsOutput,
} = require("@airstack/frames");

const input: FarcasterUserCastsInput = {
  fid: 602,
  hasEmbeds: true,
  hasFrames: true,
  hasMentions: true,
  limit: 100,
};
const { data, error }: FarcasterUserCastsOutput = await getFarcasterUserCasts(
  input
);

if (error) throw new Error(error);

console.log(data);
```

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

If you have any questions or need help regarding integrating onchain data to your Farcaster Frames using the Airstack Frames SDK, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
- [Allow List](../airstack-frog-recipes-and-middleware/allow-list.md)
- [Integrate to Existing Frog Project](../integrations/frog.md)
