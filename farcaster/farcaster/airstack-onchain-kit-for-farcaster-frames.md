---
description: Airstack Onchain Kit enables you to build Farcaster Frames with onchain data.
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

# 🖼️ Airstack Onchain Kit for Farcaster Frames

[Farcaster Frames](https://warpcast.notion.site/Farcaster-Frames-4bd47fe97dc74a42a48d3a234636d8c5) enables any cast on Farcaster to be an interactive app. Airstack Onchain Kit helps you enrich your Frame with onchain data.

You can access the majority of use cases in Airstack Onchain Kit through the newly released [**Airstack Frames SDK**](https://github.com/Airstack-xyz/airstack-frames-sdk)**.** Some other use cases will require either the [Node](../../nodejs-sdk-reference/overview.md) or [Python SDK](https://github.com/Airstack-xyz/airstack-python-sdk) to call the GraphQL queries.

The [**Airstack Frames SDK**](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main) empowers developers to seamlessly integrate onchain data, including channels, token balances, token mints, Farcaster followers and followings, POAPs, and more, into their Frames using just a few lines of code.

Additionally, developers can leverage the SDK to validate [Frames Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) and create an **allow list**, enabling checks for token ownership, token mints, following status, and more.

Designed with TypeScript, the SDK offers full type support for those building Frames with TypeScript. For SDK reference, check out the official GitHub repository [here](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main). In the guides below the Frames SDK is also highlighted throughout.

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Validate Frames](../farcaster-frames/frames-validator.md)
* [Get Farcaster User Details](airstack-onchain-kit-for-farcaster-frames.md#get-started)
* [Get all of the Farcaster User's Followers](airstack-onchain-kit-for-farcaster-frames.md#get-farcaster-users-followers)
* [Get all of the Farcaster User's Followings](airstack-onchain-kit-for-farcaster-frames.md#get-farcaster-users-followings)
* [Get All Users Followed by Those Followed by A Certain User](airstack-onchain-kit-for-farcaster-frames.md#get-all-users-followed-by-those-followed-by-a-certain-user) (2nd-degree contact)
* [Get All Users Following the Followers of a Specific User](airstack-onchain-kit-for-farcaster-frames.md#get-all-users-following-the-followers-of-a-specific-user)
* [Get All Users Commonly Followed By Two Users](airstack-onchain-kit-for-farcaster-frames.md#get-all-users-commonly-followed-by-two-users)
* [Resolve Solana Address](airstack-onchain-kit-for-farcaster-frames.md#resolve-solana-address)
* [Get Channel Details](airstack-onchain-kit-for-farcaster-frames.md#get-channel-details)
* [Get Participants Of A Channel](airstack-onchain-kit-for-farcaster-frames.md#get-participants-of-a-channel)
* [Get Farcaster Channels By Participant](airstack-onchain-kit-for-farcaster-frames.md#get-farcaster-channels-by-participant)
* [Get Farcaster Channels By Host](airstack-onchain-kit-for-farcaster-frames.md#get-farcaster-channels-by-host)
* [Get All POAPs Attended By Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-all-poaps-attended-by-farcaster-user)
* [Get All NFTs Held By Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-all-nfts-hold-by-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get All ERC20 Tokens Hold By Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-all-erc20-tokens-hold-by-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get Historical NFT Balance of Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-historical-nft-balance-of-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get Historical ERC20 Token Balance of Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-historical-erc20-token-balance-of-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get NFT Mints By A Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-nft-mints-by-a-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get Trending Mints for Farcaster Users on Base](airstack-onchain-kit-for-farcaster-frames.md#get-trending-mints-for-farcaster-user-on-base)
* [Get ERC20 Token Mints By A Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-erc20-token-mints-by-a-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get Token Transfers Sent From A Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-token-transfers-sent-from-a-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get Token Transfers Received From A Farcaster User](airstack-onchain-kit-for-farcaster-frames.md#get-token-transfers-received-by-a-farcaster-user) on Ethereum, Base, Zora, and Gold
* [Get All Farcaster Users Whose Names Start With Certain Terms (auto-complete regex in API)](airstack-onchain-kit-for-farcaster-frames.md#get-all-farcaster-users-whose-names-start-with-certain-terms-auto-complete)
* [Get All Farcaster Users Whose Names Contain Certain Terms (auto-complete regex in API)](airstack-onchain-kit-for-farcaster-frames.md#get-all-farcaster-users-whose-names-contain-certain-terms-auto-complete)
* [Search All Farcaster Channels Whose Names Start With Certain Terms (auto-complete)](airstack-onchain-kit-for-farcaster-frames.md#search-all-farcaster-channels-whose-names-start-with-certain-terms-auto-complete)
* [Search All Farcaster Channels Whose Names Contain Certain Terms (auto-complete)](airstack-onchain-kit-for-farcaster-frames.md#search-all-farcaster-channels-whose-names-contain-certain-terms-auto-complete)
* [Get Onchain Graph of Farcaster user](../../guides/onchain-graph.md) (all users on/off farcaster who have the most onchain overlaps with the user)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

## Get Farcaster User Details

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

## Get Farcaster User's Followers

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

## Get Farcaster User's Followings

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

## Get All Users Followed by Those Followed by A Certain User

First, fetch all the Farcaster users followed by a given user by providing the user's Farcaster user's 0x address to the `identity` variable using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Rp4a6SResj" %}
Get all Farcaster followings of 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2
{% endembed %}

### Code

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  # Get all the user's followings
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_eq: "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2"},
        # Only get followings on Farcaster
<strong>        dappName: {_eq: farcaster}
</strong>      },
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        addresses
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x3ff3b2c9d7e0fa1b71ce0587d5bb5497270ca044",
              "0x294af33e5590725f684de80d88e72009b1ab6f76"
            ]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0x94cc672b9ec5f0c2c7a6284d68c16f405ebae357"]
          }
        },
        {
          "followingAddress": {
            "addresses": ["0x66926469a4b05a0c45f09a264b50cd531b4bb861"]
          }
        }
        // Other addresses that is following the user
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, compile a list of addresses following the user on Farcaster and provide it as a variable input to `$userFollowings` by using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API to fetch all the followings of the those users following the given user:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Nbbaomy5X7" %}
Show me Farcaster users that is being followed by the 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2's followings on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery($userFollowings: [Identity!]) {
  # Get all the Farcaster followings of the user's followings
<strong>  SocialFollowings(
</strong>    input: {
      filter: {
        identity: {_in: $userFollowings},
        # Only check on Farcaster
        dappName: {_eq: farcaster}
      },
      blockchain: ALL
    }
  ) {
    Following {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variables" %}
```json
{
  "userFollowings": [
    "0x3ff3b2c9d7e0fa1b71ce0587d5bb5497270ca044",
    "0x294af33e5590725f684de80d88e72009b1ab6f76",
    "0x94cc672b9ec5f0c2c7a6284d68c16f405ebae357",
    "0x66926469a4b05a0c45f09a264b50cd531b4bb861"
    // Other Farcaster user that is following the given user
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followerAddress": {
            "addresses": ["0x66926469a4b05a0c45f09a264b50cd531b4bb861"],
            "socials": [
              {
                "profileName": "bigcymbal",
                "userId": "2229"
              }
            ]
          }
        }
        // Other Farcaster users that is being followed by
        // the list of Farcaster users followed by the user
        // (2nd degree contact)
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users Following the Followers of a Specific User

First, fetch all the Farcaster followers of a user by providing the user's Farcaster user's 0x address to the `identity` variable using the [`SocialFollowers`](../../api-references/api-reference/socialfollowers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Rp4a6SResj" %}
Get all Farcaster followers of 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  # Get all the user's followers
<strong>  SocialFollowers(
</strong>    input: {
      filter: {
        identity: {_eq: "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2"},
        # Only get followers on Farcaster
<strong>        dappName: {_eq: farcaster}
</strong>      },
      blockchain: ALL
    }
  ) {
    Follower {
      followerAddress {
        addresses
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xbfcf8facf17e1ae112bff95fea8405fb69563501",
              "0x3d5ed9d644d30776b55645812a31d8b9950d923f"
            ]
          }
        },
        {
          "followerAddress": {
            "addresses": [
              "0xac973be076a9d48b74109641e104b45bce009b66",
              "0x82a72e22cc27a34427fbf949866d5b64857974e6"
            ]
          }
        },
        {
          "followerAddress": {
            "addresses": ["0xe48ae12adf04877d374e992f8f57f643be2f62a3"]
          }
        }
        // Other user's Farcaster followers
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

Then, compile the list of users followers' addresses and provide it as a variable input to `$userFollowers` by using the [`SocialFollowers`](../../api-references/api-reference/socialfollowers-api.md) API to fetch all the users following the user's followers:

### Try Demo

{% embed url="https://app.airstack.xyz/query/A1qyXfHYPu" %}
Show me Farcaster users that is following the 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2's followers on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery($userFollowers: [Identity!]) {
  # Get all the Farcaster followings of the user's followers
<strong>  SocialFollowers(
</strong>    input: {
      filter: {
        identity: {_in: $userFollowers},
        # Only check on Farcaster
        dappName: {_eq: farcaster}
      },
      blockchain: ALL
    }
  ) {
    Follower {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variables" %}
```json
{
  "userFollowers": [
    "0xbfcf8facf17e1ae112bff95fea8405fb69563501",
    "0x3d5ed9d644d30776b55645812a31d8b9950d923f",
    "0xac973be076a9d48b74109641e104b45bce009b66",
    "0x82a72e22cc27a34427fbf949866d5b64857974e6",
    "0xe48ae12adf04877d374e992f8f57f643be2f62a3"
    // Other user's followers
  ]
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followingAddress": {
            "addresses": [
              "0xac973be076a9d48b74109641e104b45bce009b66",
              "0x82a72e22cc27a34427fbf949866d5b64857974e6"
            ],
            "socials": [
              {
                "profileName": "binancedaily",
                "userId": "218522"
              }
            ]
          }
        }
        // Followers of user's followers on Farcaster
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Users Commonly Followed By Two Users

{% embed url="https://drive.google.com/file/d/1kGDixfBEVahLV45UexygV4S30_qJwyq0/view?usp=sharing" %}
Video Demo
{% endembed %}

You can fetch all the users commonly followed by two given Farcaster users using the [`SocialFollowings`](../../api-references/api-reference/socialfollowings-api.md) API and providing the two users into `$userA` and `$userB` variables:

### Try Demo

{% embed url="https://app.airstack.xyz/query/COCYRqc6vd" %}
show me all Farcaster users that both user A and B follow
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($userA: Identity!, $userB: Identity!) {
  SocialFollowings(
    input: {
      filter: { dappName: { _eq: farcaster }, identity: { _eq: $userA } }
      blockchain: ALL
    }
  ) {
    Following {
      followingAddress {
        socialFollowings(
          input: {
            filter: { dappName: { _eq: farcaster }, identity: { _eq: $userB } }
          }
        ) {
          Following {
            followingAddress {
              addresses
              socials(input: { filter: { dappName: { _eq: farcaster } } }) {
                profileName
              }
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "userA": "fc_fname:vitalik.eth",
  "userB": "fc_fname:jessepollak"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "socialFollowings": {
              "Following": [
                {
                  "followingAddress": {
                    "addresses": [
                      "0x9b170c5e56422a49ba38034de04d6b9c9151a801",
                      "0x01b7baa7baa864fef3cd1c7bc118cc97cedcb33f"
                    ],
                    "socials": [
                      {
<strong>                        "profileName": "gaby" // This user is followed by both vitalik and jesse
</strong>                      }
                    ]
                  }
                }
              ]
            }
          }
        },
        {
          "followingAddress": {
            "socialFollowings": {
<strong>              "Following": null // This user is followed by vitalik, but not followed by jesse
</strong>            }
          }
        },
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Resolve Solana Address

You can resolve any solana address to their 0x address, Farcaster, and ENS domains by using the [`Wallet`](../../api-references/api-reference/wallet-api.md) API and providing the solana address to the `identity` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NLsyR0fYZL" %}
Resolve Solana address GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV to 0x address, Farcaster, and ENS
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV"
      blockchain: ethereum
    }
  ) {
    addresses
    farcsater: socials(input: { filter: { dappName: { _eq: farcaster } } }) {
      profileName
      userId
    }
    domains {
      name
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Wallet": {
      "addresses": ["0xe0235804378c31948e81441f656d826ee5998bc6"],
      "farcsater": [
        {
          "profileName": "alexcomeau",
          "userId": "11161"
        }
      ],
      "domains": [
        {
          "name": "alexjcomeau.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Channel Details

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

## Get Participants Of A Channel

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

## Get Farcaster Channels By Participant

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

## Get Farcaster Channels By Host

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
const { getFarcasterChannelsByHost } = require("@airstack/frames");

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

## Get All POAPs Attended By Farcaster User

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

## Get All NFTs Hold By Farcaster User

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

## Get All ERC20 Tokens Hold By Farcaster User

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

## Get Historical NFT Balance of Farcaster User

You can fetch Farcaster user's historical NFT balances and their time series data at a specified time by using [`Snapshots`](../../api-references/api-reference/snapshots-api.md) API and providing the given Farcaster user's 0x address to the `owner` input and specifying `tokenType` with the token type `ERC721` and `ERC1155`:

{% hint style="info" %}
For fetching historical NFT balances data from multiple chains, check out [Cross-Chain Queries](../../guides/basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/v8ZMGGNOrl" %}
Show me Farcaster user with address 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2 historical ERC20 balance on Aug 18, 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _eq: "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2"
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>        tokenType: {_in: [ERC721, ERC1155]}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Snapshot {
      tokenAddress
      tokenId
      tokenType
      token {
        name
        symbol
        isSpam
      }
      tokenNft {
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
          }
        }
      }
      startBlockNumber
      startBlockTimestamp
      endBlockNumber
      endBlockTimestamp
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "tokenAddress": "0x2dec96736e7d24e382e25d386457f490ae64889e",
          "tokenId": "1570",
          "tokenType": "ERC721",
          "token": {
            "name": "Peacefall",
            "symbol": "PF",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/pL4M7vP0WKi6EzxauXahZ3bnHOWqaDGo1gQv53MH7J/WW6v19vhKaQ9tTSsZS9nzP9ipEmB0+c+aeAlAf9RGMA==/extra_small.gif",
                "large": "https://assets.airstack.xyz/image/nft/pL4M7vP0WKi6EzxauXahZ3bnHOWqaDGo1gQv53MH7J/WW6v19vhKaQ9tTSsZS9nzP9ipEmB0+c+aeAlAf9RGMA==/large.gif",
                "medium": "https://assets.airstack.xyz/image/nft/pL4M7vP0WKi6EzxauXahZ3bnHOWqaDGo1gQv53MH7J/WW6v19vhKaQ9tTSsZS9nzP9ipEmB0+c+aeAlAf9RGMA==/medium.gif",
                "original": "https://assets.airstack.xyz/image/nft/pL4M7vP0WKi6EzxauXahZ3bnHOWqaDGo1gQv53MH7J/WW6v19vhKaQ9tTSsZS9nzP9ipEmB0+c+aeAlAf9RGMA==/original_image.gif",
                "small": "https://assets.airstack.xyz/image/nft/pL4M7vP0WKi6EzxauXahZ3bnHOWqaDGo1gQv53MH7J/WW6v19vhKaQ9tTSsZS9nzP9ipEmB0+c+aeAlAf9RGMA==/small.gif"
              }
            }
          },
          "startBlockNumber": 14450519,
          "startBlockTimestamp": "2022-03-24T18:12:38Z",
<strong>          "endBlockNumber": -1, // -1 implies that the token is no longer held
</strong>          "endBlockTimestamp": "2024-01-15T15:42:14Z"
        },
        // Other historical Ethereum NFT balances ever held
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Historical ERC20 Token Balance of Farcaster User

You can fetch Farcaster user's historical ERC20 token balances and their time series data at a specified time by using [`Snapshots`](../../api-references/api-reference/snapshots-api.md) API and providing the given Farcaster user's 0x address to the `owner` input and specifying `tokenType` with the token type `ERC20`:

{% hint style="info" %}
For fetching historical ERC20 token balances data from multiple chains, check out [Cross-Chain Queries](../../guides/basics/cross-chain-queries.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/v8ZMGGNOrl" %}
Show me Farcaster user with address 0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2 historical ERC20 balance on Aug 18, 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _eq: "0xD7029BDEa1c17493893AAfE29AAD69EF892B8ff2"
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>        tokenType: {_in: [ERC20]}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Snapshot {
      tokenAddress
      token {
        name
        symbol
        isSpam
      }
      startBlockNumber
      startBlockTimestamp
      endBlockNumber
      endBlockTimestamp
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "tokenAddress": "0x3ebd76b25a453252ca6b61334ed6f156bf5dd928",
          "token": {
            "name": "pudgy.looksrare.team",
            "symbol": "pudgy.looksrare.team",
            "isSpam": false
          },
          "startBlockNumber": 15566321,
          "startBlockTimestamp": "2022-09-19T08:17:35Z",
<strong>          "endBlockNumber": -1, // -1 implies that the token is no longer held
</strong>          "endBlockTimestamp": "2024-01-15T15:37:28Z"
        },
        {
          "tokenAddress": "0xd13017c013ae2eb708c4fcdb70b20d16ba3b64a9",
          "token": {
            "name": "os20.io",
            "symbol": "Opensea",
            "isSpam": false
          },
          "startBlockNumber": 15675664,
          "startBlockTimestamp": "2022-10-04T15:28:47Z",
<strong>          "endBlockNumber": -1, // -1 implies that the token is no longer held
</strong>          "endBlockTimestamp": "2024-01-15T15:37:28Z"
        },
        // Other historical Ethereum ERC20 token balances ever held
      ],
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get NFT Mints By A Farcaster User

You can fetch all NFTs minted by a Farcaster user across multiple chains, such as Ethereum, Gold, Base, and Zora, by using the [`getFarcasterUserNFTMints`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusernftmints) function:

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

## Get Trending Mints For Farcaster User On Base

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
const { data, error }: GetTrendingMintsOutput =
  await getTrendingMints(input);

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

## Get ERC20 Token Mints By A Farcaster User

You can fetch all ERC20 tokens minted by a Farcaster user across multiple chains, such as Ethereum, Base, Zora, and Gold by using the [`getFarcasterUserERC20Mints`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusererc20mints) function:

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
    "blockchain": "polygon",
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

## Get Token Transfers Sent From A Farcaster User

You can fetch all token transfers sent by a given Farcaster user across multiple chains, such as Ethereum, Gold, Base, and Zora, by using the [`getFarcasterUserTokenSentFrom`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusertokensentfrom) functions:

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

## Get Token Transfers Received By A Farcaster User

You can fetch all token transfers received by a given Farcaster user across multiple chains, such as Ethereum, Gold, Base, and Zora by using the [`getFarcasterUserTokenReceivedBy`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#getfarcasterusertokenreceivedby) function:

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
    TokenBlockchain.Polygon,
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

## Get All Farcaster Users Whose Names Start With Certain Terms (auto-complete)

You can fetch all Farcaster users that starts with given words by providing the regex pattern `"^<given-words>"` to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mXuUlbGPnI" %}
show me all Farcaster users starting with "a"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Farcaster users
        # starting with "a"
<strong>        profileName: {_regex: "^a"},
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "dappName": "farcaster",
          "profileName": "atty"
        },
        {
          "dappName": "farcaster",
          "profileName": "anita-mpf"
        },
        {
          "dappName": "farcaster",
          "profileName": "amarraghu"
        }
        // Other Farcaster users starting with "a"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Whose Names Contain Certain Terms (auto-complete)

You can fetch all Farcaster users that contains given words by using the [`searchFarcasterUsers`](https://github.com/Airstack-xyz/airstack-frames-sdk/tree/main?tab=readme-ov-file#searchfarcasterusers) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  searchFarcasteUsers,
  SearchFarcasterUsersInput,
  SearchFarcastersOutput,
} from "@airstack/frames";

const input: SearchFarcasterUsersInput = {
  profileName: "a",
  limit: 10,
};
const {
  data,
  error,
  hasNextPage,
  hasPrevPage,
  getNextPage,
  getPrevPage,
}: SearchFarcastersOutput = await searchFarcasterUsers(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { searchFarcasteUsers } = require("@airstack/frames");

const input = {
  profileName: "a",
  limit: 10,
};
const { data, error, hasNextPage, hasPrevPage, getNextPage, getPrevPage } =
  await searchFarcasterUsers(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="Result" %}
```json
[
  {
    "profileName": "zachterrell",
    "fnames": ["zachterrell.eth", "zachterrell"],
    "userAssociatedAddresses": [
      "0xbce5a0d16dc2031dc53da79c34ddb366e76dc482",
      "0x5a492d1e15f2ae4b418e424ba9a1d112d6e9706a"
    ],
    "followerCount": 112210,
    "followingCount": 430,
    "fid": "457",
    "profileImage": {
      "extraSmall": "https://assets.airstack.xyz/image/social/u/+rRF4VjBM2b96BzHIZBcRKdFQ3MzIbCkEp6TV3KlQ=/extra_small.jpg",
      "small": "https://assets.airstack.xyz/image/social/u/+rRF4VjBM2b96BzHIZBcRKdFQ3MzIbCkEp6TV3KlQ=/small.jpg",
      "medium": "https://assets.airstack.xyz/image/social/u/+rRF4VjBM2b96BzHIZBcRKdFQ3MzIbCkEp6TV3KlQ=/medium.jpg",
      "large": "https://assets.airstack.xyz/image/social/u/+rRF4VjBM2b96BzHIZBcRKdFQ3MzIbCkEp6TV3KlQ=/large.jpg",
      "original": "https://assets.airstack.xyz/image/social/u/+rRF4VjBM2b96BzHIZBcRKdFQ3MzIbCkEp6TV3KlQ=/original_image.jpg"
    }
  }
]
```
{% endtab %}
{% endtabs %}

## Search All Farcaster Channels Whose Names Start With Certain Terms (auto-complete)

You can fetch all Farcaster channels that starts given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`FarcasterChannels`](../../api-references/api-reference/farcasterchannels-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OYXxn7sAP6" %}
Show me all Farcaster channels name that starts with "air"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  FarcasterChannels(
    input: { blockchain: ALL, filter: { name: { _regex: "^air" } } }
  ) {
    FarcasterChannel {
      name
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterChannels": {
      "FarcasterChannel": [
        {
          "name": "airdrop-dao"
        },
        {
          "name": "airdropkazet"
        },
        {
          "name": "airdrop1-deutsch"
        }
        // Other channels starting with "air"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Search All Farcaster Channels Whose Names Contain Certain Terms (auto-complete)

You can fetch search Farcaster channels by name by using the [`searchFarcasterChannels`](https://github.com/Airstack-xyz/airstack-frames-sdk#searchfarcasterchannels) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  searchFarcasterChannels,
  SearchFarcasterChannelsInput,
  SearchFarcasterChannelsOutput,
} from "@airstack/frames";

const input: SearchFarcasterChannelsInput = {
  channel: "airstack",
  createdAtTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 2,
};
const { data, error }: SearchFarcasterChannelsOutput =
  await searchFarcasterChannels(input);

if (error) throw new Error(error);

console.log(data);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { searchFarcasterChannels } = require("@airstack/frames");

const input = {
  channel: "airstack",
  createdAtTimestamp: {
    after: "2024-02-01T00:00:00Z",
    before: "2024-02-28T00:00:00Z",
  },
  limit: 2,
};
const { data, error } = await searchFarcasterChannels(input);

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
    "hosts": [
      {
        "profileName": "betashop.eth",
        "fnames": ["betashop", "betashop.eth", "jasongoldberg.eth"],
        "fid": "602",
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
        "followerCount": 59421,
        "followingCount": 2290
      }
    ],
    "warpcastUrl": "https://warpcast.com/~/channel/airstack"
  }
]
```
{% endtab %}
{% endtabs %}

## Next Steps

To further enrich and improve the user experience for your Farcaster Frames, you can use Airstack to fetch a user's onchain contacts, which will provide all the users that have onchain connection to you, e.g. Farcaster followers/following, attended the same POAP events, holder of the same token, etc.

<div align="center">

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="375"><figcaption><p>Builder.fi's Onchain Contacts</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/converse.png" alt="" width="375"><figcaption><p>Converse's Onchain Contacts</p></figcaption></figure>

</div>

To learn more how to build this into your application, check out [Onchain Graph](../../guides/onchain-graph.md) and [Onchain Contacts](../../guides/onchain-contacts.md).

## Developer Support

If you have any questions or need help regarding the activate kit for [Farcaster Frames](https://warpcast.notion.site/Farcaster-Frames-4bd47fe97dc74a42a48d3a234636d8c5), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Graph Guides](../../guides/onchain-graph.md)
* [Onchain Contact Guides](../../guides/onchain-contacts.md)
* [Search Farcaster Users](search-farcaster-users.md)
* [Farcaster Followers](farcaster-followers.md)
* [Farcaster Following](farcaster-following.md)
* [Get Token Balances](../../guides/lens/get-token-balances.md)
* [Token Mints](../../guides/token-mints.md)
* [Token Transfers](../../guides/recommend-users/token-transfers.md)
* [Balance Snapshots](../../guides/balance-snapshots.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api.md)
* [Snapshots API Reference](../../api-references/api-reference/snapshots-api.md)
* [Math Captcha For Farcaster Frames](https://github.com/limone-eth/farcaster-horizon-airstack/blob/main/app/api/captcha/validate/route.ts)
* [Airstack Frames SDK Reference](https://github.com/Airstack-xyz/airstack-frames-sdk)
* [Airstack No-Code Frames](../farcaster-frames/no-code-frames/)
