---
description: >-
  Learn how to build an Allow List for your Farcaster Frames based on various
  criterias by using the Airstack Frog Recipes.
---

# 🎭 Allow List

You can limit access to your Farcaster Frame to only certain users that fulfill a requirement by creating an allow list based on certain criteria.

{% embed url="https://drive.google.com/file/d/1ow_6I03g5Mh65DVJIBsrN_-pyNjKwGy9/view?usp=sharing" %}
Allow List Demo
{% endembed %}

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

## createAllowList

You can create an allow list that checks various onchain data easily using the [`createAllowList`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#createallowlist) function. Some of the parameters that you can add to the allow list are:

| Name                           | Type                                                | Description                                                                                 |
| ------------------------------ | --------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `eventIds`                     | `number[]`                                          | **POAP Gating** – Check If POAPs with the given event IDs are attended by a Farcaster user. |
| `numberOfFollowersOnFarcaster` | number                                              | Check If the number of Farcaster followers greater than or equal to the given number.       |
| `isFollowingOnFarcaster`       | `number[]`                                          | Check if the given FIDs are being followed by a Farcaster user.                             |
| `tokens`                       | `{tokenAddress: string; chain: TokenBlockchain;}[]` | **Token Gating** – Check If tokens are currently held by a Farcaster user.                  |

Once you have the criteria set, the function will help you check whether all the criterias are fulfilled.

By default, it will only return `true` for Farcaster users that satisfy **ALL** the given requirements. However, if you would like to check your user with a different logic, you can provide an optional custom `isAllowedFunction`:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  createAllowList,
  CreateAllowListInput,
  CreateAllowListOutput,
  TokenBlockchain,
} from "@airstack/frog";

const allowListCriteria = {
  eventIds: [166577],
  numberOfFollowersOnFarcaster: 100,
  isFollowingOnFarcaster: [2602],
  tokens: [
    {
      tokenAddress: "0x95cb845b525f3a2126546e39d84169f1eca8c77f",
      chain: TokenBlockchain.Ethereum,
    },
    {
      tokenAddress: "0xd57867f2fdb89eadc8e859a89e3d5039c913d1d9",
      chain: TokenBlockchain.Polygon,
    },
    {
      tokenAddress: "0x2d45c399d7ca25341992038f12610c41a00a66ed",
      chain: TokenBlockchain.Base,
    },
    {
      tokenAddress: "0x743658ace931ea241dd0cb4ed38ec72cc8162ce1",
      chain: TokenBlockchain.Zora,
    },
  ],
};
const input: CreateAllowListInput = {
  fid: 602,
  allowListCriteria,
  isAllowedFunction: function (data) {
    console.log(data);
    return true;
  },
};
const { isAllowed, error }: CreateAllowListOutput = await createAllowList(
  input
);

if (error) throw new Error(error);

console.log(isAllowed);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const {
  createAllowList,
  TokenBlockchain,
} = require("@airstack/frog");

const allowListCriteria = {
  eventIds: [166577],
  numberOfFollowersOnFarcaster: 100,
  isFollowingOnFarcaster: [2602],
  tokens: [
    {
      tokenAddress: "0x95cb845b525f3a2126546e39d84169f1eca8c77f",
      chain: TokenBlockchain.Ethereum,
    },
    {
      tokenAddress: "0xd57867f2fdb89eadc8e859a89e3d5039c913d1d9",
      chain: TokenBlockchain.Polygon,
    },
    {
      tokenAddress: "0x2d45c399d7ca25341992038f12610c41a00a66ed",
      chain: TokenBlockchain.Base,
    },
    {
      tokenAddress: "0x743658ace931ea241dd0cb4ed38ec72cc8162ce1",
      chain: TokenBlockchain.Zora,
    },
  ],
};
const input = {
  fid: 602,
  allowListCriteria,
  isAllowedFunction: function (data) {
    console.log(data);
    return true;
  },
};
const { isAllowed, error } = await createAllowList(
  input
);

if (error) throw new Error(error);

console.log(isAllowed);
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "isAllowed": true,
  "error": null
}
```
{% endtab %}
{% endtabs %}

## checkPoapAttendedByFarcasterUser

You can check if a Farcaster user has attended a given list of POAP event IDs by using the [`checkPoapAttendedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checkpoapattendedbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkPoapAttendedByFarcasterUser,
  CheckPoapAttendedByFarcasterUserInput,
  CheckPoapAttendedByFarcasterUserOutput,
} from "@airstack/frames";

const input: CheckPoapAttendedByFarcasterUserInput = {
  fid: 15971,
  eventId: [160005, 159993, 13242],
};
const { data, error }: CheckPoapAttendedByFarcasterUserOutput =
  await checkPoapAttendedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { checkPoapAttendedByFarcasterUser } = require("@airstack/frames");

const input = {
  fid: 15971,
  eventId: [160005, 159993, 13242],
};
const { data, error } =
  await checkPoapAttendedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Response" %}
```json
[
  { "eventId": 160005, "isAttended": true },
  { "eventId": 159993, "isAttended": true },
  { "eventId": 13242, "isAttended": false }
]
```
{% endtab %}
{% endtabs %}

## checkTokenHoldByFarcasterUser

You can check if a Farcaster user is holding a given list of tokens across Ethereum, Polygon, Base, and Zora by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checktokenholdbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkTokenHoldByFarcasterUser,
  CheckTokenHoldByFarcasterUserInput,
  CheckTokenHoldByFarcasterUserOutput,
  TokenBlockchain,
} from "@airstack/frog";

const input: CheckTokenHoldByFarcasterUserInput = {
  fid: 15971,
  token: [
    {
      chain: TokenBlockchain.Base,
      tokenAddress: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
    },
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    },
    {
      chain: TokenBlockchain.Zora,
      tokenAddress: "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    },
  ],
};
const { data, error }: CheckTokenHoldByFarcasterUserOutput =
  await checkTokenHoldByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import {
  checkTokenHoldByFarcasterUser,
  TokenBlockchain,
} from "@airstack/frog";

const input = {
  fid: 15971,
  token: [
    {
      chain: TokenBlockchain.Base,
      tokenAddress: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
    },
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    },
    {
      chain: TokenBlockchain.Zora,
      tokenAddress: "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    },
  ],
};
const { data, error } =
  await checkTokenHoldByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "chain": "base",
    "tokenAddress": "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
    "isHold": false
  },
  {
    "chain": "ethereum",
    "tokenAddress": "0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85",
    "isHold": true
  },
  {
    "chain": "zora",
    "tokenAddress": "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    "isHold": true
  }
]
```
{% endtab %}
{% endtabs %}

## checkTokenMintedByFarcasterUser

You can check if a Farcaster user has minted a given list of tokens across Ethereum, Polygon, Base, and Zora by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checktokenmintedbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkTokenMintedByFarcasterUser,
  CheckTokenMintedByFarcasterUserInput,
  CheckTokenMintedByFarcasterUserOutput,
  TokenBlockchain,
} from "@airstack/frog";

const input: CheckTokenMintedByFarcasterUserInput = {
  fid: 15971,
  token: [
    {
      chain: TokenBlockchain.Base,
      tokenAddress: "0x57965af45c3b33571aa5419cc5e9012d8dcab181",
    },
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    },
    {
      chain: TokenBlockchain.Zora,
      tokenAddress: "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    },
  ],
};
const { data, error }: CheckTokenMintedByFarcasterUserOutput =
  await checkTokenMintedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const {
  checkTokenMintedByFarcasterUser,
  TokenBlockchain,
} = require("@airstack/frog");

const input = {
  fid: 15971,
  token: [
    {
      chain: TokenBlockchain.Base,
      tokenAddress: "0x57965af45c3b33571aa5419cc5e9012d8dcab181",
    },
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    },
    {
      chain: TokenBlockchain.Zora,
      tokenAddress: "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    },
  ],
};
const { data, error } =
  await checkTokenMintedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Response" %}
```json
[
  {
    "chain": "base",
    "tokenAddress": "0x57965af45c3b33571aa5419cc5e9012d8dcab181",
    "isMinted": true
  },
  {
    "chain": "ethereum",
    "tokenAddress": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    "isMinted": true
  },
  {
    "chain": "zora",
    "tokenAddress": "0xa15bb830acd9ab46164e6840e3ef2dbbf9c5e2b3",
    "isMinted": false
  }
]
```
{% endtab %}
{% endtabs %}

## checkIsFollowingFarcasterUser

You can check if a Farcaster user is following a list of FIDs by using the [`checkIsFollowingFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checkisfollowingfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowingFarcasterUser,
  CheckIsFollowingFarcasterUserInput,
  CheckIsFollowingFarcasterUserOutput,
} from "@airstack/frog";

const input: CheckIsFollowingFarcasterUserInput = {
  fid: 602,
  isFollowing: [2602, 15971, 13242],
};
const { data, error }: CheckIsFollowingFarcasterUserOutput =
  await checkIsFollowingFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { checkIsFollowingFarcasterUser } = require("@airstack/frog");

const input = {
  fid: 602,
  isFollowing: [2602, 15971, 13242],
};
const { data, error } =
  await checkIsFollowingFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Response" %}
```json
[
  { "fid": 2602, "isFollowing": true },
  { "fid": 15971, "isFollowing": true },
  { "fid": 13242, "isFollowing": false }
]
```
{% endtab %}
{% endtabs %}

## checkIsFollowedByFarcasterUser

You can check if a Farcaster user is being followed by a list of FIDs by using the [`checkIsFollowedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checkisfollowedbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowedByFarcasterUser,
  CheckIsFollowedByFarcasterUserInput,
  CheckIsFollowedByFarcasterUserOutput,
} from "@airstack/frog";

const input: CheckIsFollowedByFarcasterUserInput = {
  fid: 602,
  isFollowedBy: [2602, 15971, 13242],
};
const { data, error }: CheckIsFollowedByFarcasterUserOutput =
  await checkIsFollowedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const {
  checkIsFollowedByFarcasterUser,
  CheckIsFollowedByFarcasterUserInput,
  CheckIsFollowedByFarcasterUserOutput,
} = require("@airstack/frog");

const input = {
  fid: 602,
  isFollowedBy: [2602, 15971, 13242],
};
const { data, error } =
  await checkIsFollowedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Response" %}
```json
[
  { "fid": 2602, "isFollowedBy": true },
  { "fid": 15971, "isFollowedBy": true },
  { "fid": 13242, "isFollowedBy": false }
]
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help building an allow list for your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Integrate to Existing Frog Project](integrate-to-existing-frog-project.md)
