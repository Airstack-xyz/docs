---
description: >-
  Learn how to build an Allow List for your Farcaster Frames based on various
  criterias by using the Airstack Frames SDK.
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

## Create A Custom Allow List

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
} from "@airstack/frames";

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
const { createAllowList, TokenBlockchain } = require("@airstack/frames");

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

## Check If POAP(s) Is Attended By A Farcaster User

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
const { data, error } = await checkPoapAttendedByFarcasterUser(input);

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

## Check If Token(s) Hold By A Farcaster User

You can check if a Farcaster user is holding a given list of tokens across Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains) by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checktokenholdbyfarcasteruser) function:

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
} from "@airstack/frames";

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
const { data, error } = await checkTokenHoldByFarcasterUser(input);

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

## Check If Token(s) Minted By A Farcaster User

You can check if a Farcaster user has minted a given list of tokens across Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains) by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#checktokenmintedbyfarcasteruser) function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
import {
  checkTokenMintedByFarcasterUser,
  CheckTokenMintedByFarcasterUserInput,
  CheckTokenMintedByFarcasterUserOutput,
  TokenBlockchain,
} from "@airstack/frames";

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
} = require("@airstack/frames");

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
const { data, error } = await checkTokenMintedByFarcasterUser(input);

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

## Check If Farcaster User Has Any Non-Virtual POAPs

{% embed url="https://drive.google.com/file/d/1DnNTCN_F6gnR7lCCH8C5eMOv_Vp3zhjt/view?usp=sharing" %}
Video Demo
{% endembed %}

You can check if the Farcaster user has attended any non-virtual or in-real-life (IRL) POAP events by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Poaps`](../../../api-references/api-reference/poaps-api.md) API:

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
const { data, error } = await checkPoapAttendedByFarcasterUser(input);

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

You can check if the Farcaster user has X or more Farcaster followers by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Socials`](../../../api-references/api-reference/socials-api.md) API:

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
        followerCount: { _gt: 100 }
        dappName: { _eq: farcaster }
        identity: { _eq: $farcasterUser }
      }
      blockchain: ethereum
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

You can check if the Farcaster user is followed by certain high profile users, e.g. [vitalik.eth](https://explorer.airstack.xyz/token-balances?address=fc_fname%3Avitalik.eth&rawInput=%23%E2%8E%B1fc_fname%3Avitalik.eth%E2%8E%B1%28fc_fname%3Avitalik.eth++ethereum+null%29&inputType=ADDRESS), [jessepollak](https://explorer.airstack.xyz/token-balances?address=fc_fname%3Ajessepollak&rawInput=%23%E2%8E%B1fc_fname%3Ajessepollak%E2%8E%B1%28fc_fname%3Ajessepollak++ethereum+null%29&inputType=ADDRESS), etc., by using the [`checkIsFollowedByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#searchfarcasterusers) function:

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

## Check If Farcaster User Hold Any High Value NFTs

You can check if the Farcaster user has any high value NFTs, e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&tokenType=&rawInput=%23%E2%8E%B10xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+ADDRESS+ethereum+null%29&inputType=ADDRESS&activeTokenInfo=&activeSnapshotInfo=&tokenFilters=farcaster&activeViewToken=Bored+Ape+Yacht+Club&activeViewCount=83&blockchainType=&sortOrder=&spamFilter=&mintFilter=&resolve6551=&activeSocialInfo=), by using the [`checkTokenHoldByFarcasterUser`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#checktokenholdbyfarcasteruser) function:

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
const { data, error } = await checkTokenHoldByFarcasterUser(input);

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
[{ "fid": 99, "isFollowing": true }]
```

{% endtab %}
{% endtabs %}

## Check If Farcaster User Transacted On Certain Blockchain Before Certain Date

{% embed url="https://drive.google.com/file/d/1s9uwn6yqLid0-xft00wBux4-H2CAHuJP/view?usp=sharing" %}
Video Demo
{% endembed %}

You can check if the Farcaster user transacted on certain blockchain, e.g. Ethereum, Base, Zora, and other [Airstack-supported chains](overview.md#supported-chains) by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable and the before date to check to the `$beforeDate` variable using the [`Wallet`](../../../api-references/api-reference/wallet-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5nxW2njR9A" %}
Check If Farcaster user transacted on Ethereum before February 1st, 2024
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery($farcasterUser: Identity!, $beforeDate: Time!) {
  TokenTransfers(
    input: {
      filter: {
        from: { _eq: $farcasterUser }
        blockTimestamp: { _lt: $beforeDate }
      }
      blockchain: ethereum
    }
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

You can check if Farcaster user has casted in a given channel by using the [`FarcasterChannelParticipants`](../../../api-references/api-reference/farcasterchannelparticipants-api.md) API and providing:

- the "cast" value to the `$channelActions` variable,
- the channel ID (e.g. /farcaster channel ID is "farcaster") to `$channelId` variable, and
- the FID to the `$participant` variable

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

## Check If Farcaster User Has A Power Badge

You can check if Farcaster user has a power badge by using the [`Socials`](../../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/VyYgdkCrPi" %}
Check if FID 602 is a power user or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { identity: { _eq: "fc_fid:602" }, dappName: { _eq: farcaster } }
      blockchain: ethereum
    }
  ) {
    Social {
      isFarcasterPowerUser
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "isFarcasterPowerUser": true
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

- [Onchain Data](../airstack-frog-recipes-and-middleware/onchain-data.md)
- [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
- [Integrate to Existing Frog Project](../integrations/frog.md)
