---
description: >-
  Learn how to build an Allow List for your Farcaster Frames based on various
  criterias by using the Airstack Frames SDK.
---

# 🎭 Allow List

You can limit access to your Farcaster Frame to only certain users that fulfill a requirement by creating an allow list based on certain criteria.

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

## Create A Custom Allow List

You can create an allow list that checks various onchain data easily using the [`createAllowList`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#createallowlist) function. Some of the parameters that you can add to the allow list are:

| Name                           | Type       | Description                                                                           |
| ------------------------------ | ---------- | ------------------------------------------------------------------------------------- |
| `numberOfFollowersOnFarcaster` | number     | Check If the number of Farcaster followers greater than or equal to the given number. |
| `isFollowingOnFarcaster`       | `number[]` | Check if the given FIDs are being followed by a Farcaster user.                       |

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
} from "@airstack/frames";

const allowListCriteria = {
  numberOfFollowersOnFarcaster: 100,
  isFollowingOnFarcaster: [2602],
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
const { createAllowList, TokenBlockchain } = require("@airstack/frames");

const allowListCriteria = {
  numberOfFollowersOnFarcaster: 100,
  isFollowingOnFarcaster: [2602],
};
const input = {
  fid: 602,
  allowListCriteria,
  isAllowedFunction: function (data) {
    console.log(data);
    return true;
  },
};
const { isAllowed, error } = await createAllowList(input);

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

## Check If User Is Following Farcaster User(s)

You can check if a Farcaster user is following a list of FIDs by using the [`checkIsFollowingFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checkisfollowingfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowingFarcasterUser,
  CheckIsFollowingFarcasterUserInput,
  CheckIsFollowingFarcasterUserOutput,
} from "@airstack/frames";

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
const { checkIsFollowingFarcasterUser } = require("@airstack/frames");

const input = {
  fid: 602,
  isFollowing: [2602, 15971, 13242],
};
const { data, error } = await checkIsFollowingFarcasterUser(input);

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

## Check If User Is Followed By Farcaster User(s)

You can check if a Farcaster user is being followed by a list of FIDs by using the [`checkIsFollowedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checkisfollowedbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowedByFarcasterUser,
  CheckIsFollowedByFarcasterUserInput,
  CheckIsFollowedByFarcasterUserOutput,
} from "@airstack/frames";

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
} = require("@airstack/frames");

const input = {
  fid: 602,
  isFollowedBy: [2602, 15971, 13242],
};
const { data, error } = await checkIsFollowedByFarcasterUser(input);

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

## Check If Farcaster User Is Followed By Certain High Profile Users

You can check if the Farcaster user is followed by certain high profile users, e.g. [vitalik.eth](https://explorer.airstack.xyz/token-balances?address=fc\_fname%3Avitalik.eth\&rawInput=%23%E2%8E%B1fc\_fname%3Avitalik.eth%E2%8E%B1%28fc\_fname%3Avitalik.eth++ethereum+null%29\&inputType=ADDRESS), [jessepollak](https://explorer.airstack.xyz/token-balances?address=fc\_fname%3Ajessepollak\&rawInput=%23%E2%8E%B1fc\_fname%3Ajessepollak%E2%8E%B1%28fc\_fname%3Ajessepollak++ethereum+null%29\&inputType=ADDRESS), etc., by using the [`checkIsFollowedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#searchfarcasterusers) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowedByFarcasterUser,
  CheckIsFollowedByFarcasterUserInput,
  CheckIsFollowedByFarcasterUserOutput,
} from "@airstack/frames";

const input: CheckIsFollowedByFarcasterUserInput = {
  fid: 602,
  isFollowedBy: [
    99, // jessepollak
    5650, // vitalik.eth
  ],
};
const { data, error }: CheckIsFollowedByFarcasterUserOutput =
  await checkIsFollowedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { checkIsFollowedByFarcasterUser } = require("@airstack/frames");

const input = {
  fid: 602,
  isFollowedBy: [
    99, // jessepollak
    5650, // vitalik.eth
  ],
};
const { data, error } = await checkIsFollowedByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Result" %}
```json
[
  { "fid": 99, "isFollowedBy": true },
  { "fid": 5650, "isFollowedBy": false }
]
```
{% endtab %}
{% endtabs %}

## Check If Farcaster User Follows The Creator Of The Frames

You can check if the Farcaster user follows the Frames' creator by using the [`checkIsFollowingFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#checkisfollowingfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkIsFollowingFarcasterUser,
  CheckIsFollowingFarcasterUserInput,
  CheckIsFollowingFarcasterUserOutput,
} from "@airstack/frames";

const input: CheckIsFollowingFarcasterUserInput = {
  fid: 3,
  isFollowing: [99],
};
const { data, error }: CheckIsFollowingFarcasterUserOutput =
  await checkIsFollowingFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
import {
  checkIsFollowingFarcasterUser,
  CheckIsFollowingFarcasterUserInput,
  CheckIsFollowingFarcasterUserOutput,
} from "@airstack/frames";

const input: CheckIsFollowingFarcasterUserInput = {
  fid: 3, // The creator of frame
  isFollowing: [99], // The fid of user interacting with your frames
};
const { data, error }: CheckIsFollowingFarcasterUserOutput =
  await checkIsFollowingFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Result" %}
```json
[{ "fid": 99, "isFollowing": true }]
```
{% endtab %}
{% endtabs %}

## Check If Farcaster User Has Casted In A Given Channel

You can check if Farcaster user has casted in a given channel by using the [`FarcasterChannelParticipants`](../../../api-references/api-reference/farcasterchannelparticipants-api.md) API and providing:

* the "cast" value to the `$channelActions` variable,
* the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable, and
* the FID to the `$participant` variable

### Try Demo

{% embed url="https://app.airstack.xyz/query/PkFu8vdw9o" %}
Check if Farcaster user FID 602 is participating in airstack channel
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannelParticipants(
    input: {
      filter: {
        participant: { _eq: "fc_fid:602" }
        channelId: { _eq: "airstack" }
        channelActions: { _eq: cast }
      }
      blockchain: ALL
    }
  ) {
    FarcasterChannelParticipant {
      lastActionTimestamp
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannelParticipants": {
      "FarcasterChannelParticipant": [
        {
          "lastActionTimestamp": "2024-02-23T17:12:13Z"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help building an allow list for your Farcaster Frames using the Airstack Frames SDK, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Farcaster Data](onchain-kit.md)
* [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
