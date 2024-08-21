---
description: >-
  Learn how you can integrate various Farcaster data into your Frames easily
  using the Airstack Frames SDK.
---

# ⛓️ Farcaster Data

The Airstack Frames SDK allows you to easily fetch any Farcaster data in your Frames.

Here are some Farcaster data data that you can integrate into your Frames:

* Get Farcaster User's Followers and Followings
* Get Farcaster Channels Participated and Hosted By Farcaster Users

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

If you have any questions or need help regarding integrating Farcaster data to your Farcaster Frames using the Airstack Frames SDK, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Allow List](../airstack-frog-recipes-and-middleware/allow-list.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
