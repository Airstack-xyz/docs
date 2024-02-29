---
description: >-
  Learn how to build Allow Lists to allow certain users to interact with your
  Farcaster Frames.
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

# 🎭 Allow Lists for Farcaster Frames

Airstack makes it easy to create dynamic allow lists that check in real-time if a user qualifies to use your Frame based on your preferred criteria. Some of the criteria you can use include:&#x20;

* Has the user Attended IRL POAP events
* Attended Specific POAP event(s)
* Does the user have More than X Farcaster Followers
* Followed By Specific or High Profile Users (e.g. Vitalik, Jesse Pollak, etc.)
* Hold Specific or High Value NFTs (e.g. BAYC)
* Follow The Creator Of The Frame
* Transacted on Blockchain Before Certain Date
* Casted in a Channel

Currently, you can access the most of the criteria can be added by using the newly released [Airstack Frames SDK](https://github.com/Airstack-xyz/airstack-frames-sdk).  While some others will require either the [Node](../../nodejs-sdk-reference/overview.md) or [Python SDK](https://github.com/Airstack-xyz/airstack-python-sdk) to call the GraphQL queries.

The [Airstack Frames SDK](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main) empowers developers to seamlessly integrate onchain data, including token balances, token mints, Farcaster followers and followings, POAPs, and more, into their Frames using just a few lines of code.

Additionally, developers can leverage the SDK to create an allow list feature, enabling checks for token ownership, token mints, following status, and more.

Designed with TypeScript, the SDK offers full type support for those building Frames with TypeScript. For SDK reference, check out the official GitHub repository [here](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main).

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Check If Farcaster User Has Any Non-Virtual POAPs](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-any-non-virtual-poaps)
* [Check If Farcaster User Has Certain POAP(s)](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-certain-poap-s)
* [Check If Farcaster User Has X or More Followers on Farcaster](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-x-or-more-followers-on-farcaster)
* [Check If Farcaster User Is Followed By Certain High Profile Users](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-is-followed-by-certain-high-profile-users)
* [Check If Farcaster User Holds Specific/ or High Value NFTs](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-hold-any-high-value-nfts)
* [Check If Farcaster User Follows The Creator Of The Frames](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-follows-the-creator-of-the-frames)
* [Check If Farcaster User Transacted On Certain Blockchain Before Certain Date](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-transacted-on-certain-blockchain-before-certain-date)
* [Check If Farcaster User Has Casted In A Given Channel](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-casted-in-a-given-channel)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

### Get Started

#### Airstack Frames SDK

To integrate Airstack into your frames, simply install the Airstack Frames SDK:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/frames
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/frames
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @airstack/frames
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code and provide your [Airstack API key](../../get-started/get-api-key.md):

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { init } from "@airstack/frames";

init("YOUR_AIRSTACK_API_KEY");
```
{% endtab %}

{% tab title="JavaScript (Node)" %}
```javascript
import { init } from "@airstack/frames";

init("YOUR_AIRSTACK_API_KEY");
```
{% endtab %}
{% endtabs %}

Once you have the SDK initialized with the `init` function, you can call any available functions within the SDKs. For the list of all available functions, click [here](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#table-of-contents).

#### Others

Otherwise, for GraphQL queries you'll need to either install the Airstack Node or Python SDK:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="Node" %}
```javascript
import { init, fetchQuery } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const { data, error } = await fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """YOUR_QUERY""" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Check If Farcaster User Has Any Non-Virtual POAPs

{% embed url="https://drive.google.com/file/d/1DnNTCN_F6gnR7lCCH8C5eMOv_Vp3zhjt/view?usp=sharing" %}
Video Demo
{% endembed %}

You can check if the Farcaster user has attended any non-virtual or in-real-life (IRL) POAP events by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Poaps`](../../api-references/api-reference/poaps-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/6Dci00KVsa" %}
Show me all POAPs owned by a Farcaster user and see if they are virtual or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwned($farcasterUser: Identity!) {
  Poaps(
    input: {
      filter: { owner: { _eq: $farcasterUser } }
      blockchain: ALL
      limit: 50
    }
  ) {
    Poap {
      mintOrder
      mintHash
      poapEvent {
        isVirtualEvent
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "farcasterUser": "fc_fid:3"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "mintOrder": 740,
          "mintHash": "0xdf9e6af3bab24eaac6e35a0474f97b4373eeff7bfff8fb4c964956860b4975e6",
          "poapEvent": {
<strong>            "isVirtualEvent": false // This event is non-virtual or IRL
</strong>          }
        },
        {
          "mintOrder": 5496,
          "mintHash": "0x8cbd346bbe318483e4c2134de97dc9e93cbc1e8a9774408d8ff20b747d8cd043",
          "poapEvent": {
<strong>            "isVirtualEvent": true // This event is virtual or online
</strong>          }
        },
        // more POAPs held by specified Farcaster user
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Farcaster User Has Certain POAP(s)

You can check if the Farcaster user has attended certain POAP event(s) by using the [`checkPoapAttendedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#searchfarcasterusers) function:

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

{% tab title="Result" %}
```json
[
  { "eventId": 160005, "isAttended": true },
  { "eventId": 159993, "isAttended": true },
  { "eventId": 13242, "isAttended": false }
]
```
{% endtab %}
{% endtabs %}

## Check If Farcaster User Has X or More Followers on Farcaster

{% embed url="https://drive.google.com/file/d/1wOZ7b_XI9nv0OLkFVpI3w2_FAKaNbYLQ/view?usp=sharing" %}
Video Demo
{% endembed %}

You can check if the Farcaster user has X or more Farcaster followers by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/0XTXnQnkK2" %}
Check if Farcaster user has more than or equal to 100 followers on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($farcasterUser: Identity!) {
  Socials(
    input: {
      filter: {
        followerCount: {_gt: 100},
        dappName: {_eq: farcaster},
        identity: {_eq: $farcasterUser}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      followerCount
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "farcasterUser": "fc_fid:3"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
          // Follower number will be shown if above the threshold,
          // in this case above 100. Otherwise, will be shown null.
<strong>          "followerCount": 131001 
</strong>        }
      ]
    }
  }
}
</code></pre>
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
    99,   // jessepollak
    5650  // vitalik.eth
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
const {
  checkIsFollowedByFarcasterUser,
} = require("@airstack/frames");

const input = {
  fid: 602,
  isFollowedBy: [
    99,   // jessepollak
    5650  // vitalik.eth
  ],
};
const { data, error } =
  await checkIsFollowedByFarcasterUser(input);

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

## Check If Farcaster User Hold Any High Value NFTs

You can check if the Farcaster user has any high value NFTs, e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D\&tokenType=\&rawInput=%23%E2%8E%B10xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+ADDRESS+ethereum+null%29\&inputType=ADDRESS\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=farcaster\&activeViewToken=Bored+Ape+Yacht+Club\&activeViewCount=83\&blockchainType=\&sortOrder=\&spamFilter=\&mintFilter=\&resolve6551=\&activeSocialInfo=), by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#checktokenholdbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  checkTokenHoldByFarcasterUser,
  CheckTokenHoldByFarcasterUserInput,
  CheckTokenHoldByFarcasterUserOutput,
  TokenBlockchain,
} from "@airstack/frames";

const input: CheckTokenHoldByFarcasterUserInput = {
  fid: 21018,
  token: [
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
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
const {
  checkTokenHoldByFarcasterUser,
  TokenBlockchain,
} = require("@airstack/frames");

const input = {
  fid: 21018,
  token: [
    {
      chain: TokenBlockchain.Ethereum,
      tokenAddress: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    },
  ],
};
const { data, error } =
  await checkTokenHoldByFarcasterUser(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Result" %}
```json
[
  {
    "chain": "ethereum",
    "tokenAddress": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
    "isHold": true
  }
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
[
  { "fid": 99, "isFollowing": true }
]
```
{% endtab %}
{% endtabs %}

## Check If Farcaster User Transacted On Certain Blockchain Before Certain Date

{% embed url="https://drive.google.com/file/d/1s9uwn6yqLid0-xft00wBux4-H2CAHuJP/view?usp=sharing" %}
Video Demo
{% endembed %}

You can check if the Farcaster user transacted on certain blockchain, e.g. Ethereum, Polygon, Base, Zora, by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable and the before date to check to the `$beforeDate` variable using the [`Wallet`](../../api-references/api-reference/wallet-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5nxW2njR9A" %}
Check If Farcaster user transacted on Ethereum before February 1st, 2024
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery(
  $farcasterUser: Identity!,
  $beforeDate: Time!
) {
  TokenTransfers(
    input: {filter: {from: {_eq: $farcasterUser}, blockTimestamp: {_lt: $beforeDate}}, blockchain: ethereum}
  ) {
    TokenTransfer {
      transactionHash
      blockTimestamp
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "farcasterUser": "fc_fid:3",
  "beforeDate": "2024-02-01T00:00:00Z"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      // If this array is non-empty, then you can allow user to interact w/ Frames
      // Otherwise, do not allow them
<strong>      "TokenTransfer": [
</strong>        {
          "transactionHash": "0xd14592edcacdcf81c06aa9588abcb38c1b553e066c175bbb9aa794a321608ad7",
          "blockTimestamp": "2023-08-08T06:35:35Z"
        },
        {
          "transactionHash": "0x2c40ac77d54a15cb9b766d1bdb036ca2fad11f8bd0f69c9c21d96a2fc4f3116f",
          "blockTimestamp": "2023-08-27T19:39:23Z"
        },
        {
          "transactionHash": "0xb92628bc8769f60b472f4989b81be9a22e4ce606fad5a39292a5c4ced1d8ee6d",
          "blockTimestamp": "2021-04-09T18:01:18Z"
        },
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Farcaster User Has Casted In A Given Channel

You can check if Farcaster user has casted in a given channel by using the [`FarcasterChannelParticipants`](../../api-references/api-reference/farcasterchannelparticipants-api.md) API and providing:

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
        participant: {_eq: "fc_fid:602"},
        channelId: {_eq: "airstack"},
        channelActions: {_eq: cast}
      },
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

If you have any questions or need help regarding building and integrating allow lists to your Farcaster Frame, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Onchain Kit for Farcaster Frames](airstack-onchain-kit-for-farcaster-frames.md)
* [Math Captcha For Farcaster Frames](https://github.com/limone-eth/farcaster-horizon-airstack/blob/main/app/api/captcha/validate/route.ts)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [Poaps API Reference](../../api-references/api-reference/poaps-api.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Airstack Frames SDK Reference](https://github.com/Airstack-xyz/airstack-frames-sdk)
* [Airstack No-Code Frames](../no-code-frames/)
