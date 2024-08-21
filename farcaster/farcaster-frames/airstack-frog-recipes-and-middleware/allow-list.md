---
description: >-
  Learn how to build an Allow List for your Farcaster Frames based on various
  criterias by using the Airstack Frog Recipes.
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

## createAllowList

You can create an allow list that checks various Farcaster data easily using the [`createAllowList`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#createallowlist) function. Some of the parameters that you can add to the allow list are:

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
} from "@airstack/frog";

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
const { createAllowList, TokenBlockchain } = require("@airstack/frog");

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

## Developer Support

If you have any questions or need help building an allow list for your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Farcaster Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
