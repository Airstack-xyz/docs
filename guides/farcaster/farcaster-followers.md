---
description: >-
  Learn how to fetch Farcaster followers data and its various use cases and
  combinations.
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

# 🫂 Farcaster Followers

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

In this tutorial, you will learn how to fetch various use cases of Farcaster followers data.

In this guide you will learn how to use Airstack to:

* [Get Farcaster Followers of Farcaster User(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s)
* [Get Farcaster Followers of Farcaster User(s) that has XMTP Enabled](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s-that-has-xmtp-enabled)
* [Get Farcaster Users that have a certain amount of Followers](farcaster-followers.md#get-farcaster-users-that-have-a-certain-amount-of-followers)
* [Get Farcaster Followers of Farcaster User(s) that Hold ERC20 Token(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s-that-hold-erc20-token-s)
* [Get Farcaster Followers of Farcaster User(s) that Hold ERC721/1155 NFT(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s-that-hold-erc20-token-s)
* [Get Farcaster Followers of Farcaster User(s) that Hold POAP(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s-that-hold-poap-s)
* [Get Farcaster Followers of Farcaster User(s) that Hold Certain Amount of ERC20 Token(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s-that-hold-certain-amount-of-erc20-token-s)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);
  
  if (data) {
    return <p>Data: {JSON.stringify(data)}</p>;
  }

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }
};
```
{% endtab %}

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

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Farcaster Followers of Farcaster User(s)

You can get the list of Farcaster followers of Farcaster user(s) by inputting [0x address](#user-content-fn-1)[^1], [ENS domain](#user-content-fn-2)[^2], [Farcaster Name](#user-content-fn-3)[^3], or [Farcaster ID](#user-content-fn-4)[^4]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NwB5fVxenM" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
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
          "followerAddress": {
            "addresses": [
              "0x08f2ca269fc4ca7ec4f86c434dc13d120a116271"
            ],
            "socials": [
              {
                "profileName": "dian",
                "userId": "9739",
                "userAssociatedAddresses": [
                  "0x08f2ca269fc4ca7ec4f86c434dc13d120a116271"
                ]
              }
            ]
          },
          "followerProfileId": "9739",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "0x91031dcfdea024b4d51e775486111d2b2a715871"
            ],
            "domains": [
              {
                "name": "varunsrin.eth"
              }
            ],
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871"
                ]
              }
            ]
          },
          "followingProfileId": "2"
        },
        // more Farcaster followers
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Followers of Farcaster User(s) that has XMTP Enabled

You can get the list of Farcaster followers of Farcaster user(s) and check if each followers have XMTP enabled or not by inputting [0x address](#user-content-fn-5)[^5], [ENS domain](#user-content-fn-6)[^6], [Farcaster Name](#user-content-fn-7)[^7], or [Farcaster ID](#user-content-fn-8)[^8]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pRTpTuyHa8" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 and check if XMTP is enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xd28c2e920bb2e4f159c31ac052b2ed57390996e5",
              "0xabc4f7d130d0c6be07b7ed23a315e3f60c4d5661"
            ],
            "socials": [
              {
                "profileName": "schneller",
                "userId": "12305",
                "userAssociatedAddresses": [
                  "0xd28c2e920bb2e4f159c31ac052b2ed57390996e5",
                  "0xabc4f7d130d0c6be07b7ed23a315e3f60c4d5661"
                ]
              }
            ],
            "xmtp": [
              {
<strong>                "isXMTPEnabled": true // XMTP is enabled
</strong>              }
            ]
          },
          "followerProfileId": "12305",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "0x91031dcfdea024b4d51e775486111d2b2a715871"
            ],
            "domains": [
              {
                "name": "varunsrin.eth"
              }
            ],
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871"
                ]
              }
            ]
          },
          "followingProfileId": "2"
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Farcaster Users that have a certain amount of Followers

You can get the list of all Farcaster users that have a certain amount of followers, e.g. at least 1000 followers:

### Try Demo

{% embed url="https://app.airstack.xyz/query/BK9pLZ9OgA" %}
Show me all Farcaster users that have at least 1000 followers
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {followerCount: {_gte: 1000}, dappName: {_eq: farcaster}}, blockchain: ethereum, limit: 200}
  ) {
    Social {
      profileName
      userId
      userAssociatedAddresses
      followerCount
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
          "profileName": "pfista",
          "userId": "451",
          "userAssociatedAddresses": [
            "0xd2344f892241e3a83c7eca8e586093714be8ca7f",
            "0x8a95d5f9ac0eebd5880199ea8fcd0fae845bbbf2",
            "0x92382f5c6ad39829a07537d18807462960025725"
          ],
          "followerCount": 7620
        },
        {
          "profileName": "jayme",
          "userId": "373",
          "userAssociatedAddresses": [
            "0x1ca66c990e86b750ea6b2180d17fff89273a5c0d",
            "0x9eab9d856a3a667dc4cd10001d59c679c64756e7"
          ],
          "followerCount": 9554
        },
        {
          "profileName": "macbudkowski",
          "userId": "1048",
          "userAssociatedAddresses": [
            "0x66cc8f831f2a05fde7573f18ba6abfef8ad59d34",
            "0x3e6c23cdaa52b1b6621dbb30c367d16ace21f760"
          ],
          "followerCount": 5766
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Followers of Farcaster User(s) that Hold ERC20 Token(s)

You can get the list of Farcaster followers of Farcaster user(s) that also hold ERC20 token(s), e.g. [USDC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&tokenType=\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29\&inputType=TOKEN\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting [0x address](#user-content-fn-9)[^9], [ENS domain](#user-content-fn-10)[^10], [Farcaster Name](#user-content-fn-11)[^11], or [Farcaster ID](#user-content-fn-12)[^12]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/XgT9Kp1oMO" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also holds USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]}, tokenType: {_eq: ERC20}}}
        ) {
          formattedAmount
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
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
          "followerAddress": {
            "addresses": [
              "0xd3c3228fa91ff3b2b146f4b4a1f6222e640ed559",
              "0x7954aa7d63fc32203f23177e502871c4d1e7307b"
            ],
            "socials": [
              {
                "profileName": "kevinli",
                "userId": "11370",
                "userAssociatedAddresses": [
                  "0xd3c3228fa91ff3b2b146f4b4a1f6222e640ed559",
                  "0x7954aa7d63fc32203f23177e502871c4d1e7307b"
                ]
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 1494.378076 // hold USDC
              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "0x91031dcfdea024b4d51e775486111d2b2a715871"
            ],
            "domains": [
              {
                "name": "varunsrin.eth"
              }
            ],
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871"
                ]
              }
            ]
          },
          "followingProfileId": "2"
        },
        // more followers
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Farcaster Followers of Farcaster User(s) that Hold ERC721/1155 NFT(s)

You can get the list of Farcaster followers of Farcaster user(s) that also hold ERC721/1155 NFT(s), e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D\&tokenType=\&rawInput=%23%E2%8E%B1BoredApeYachtClub%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+NFT\_COLLECTION+ethereum+null%29\&inputType=NFT\_COLLECTION\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting [0x address](#user-content-fn-13)[^13], [ENS domain](#user-content-fn-14)[^14], [Farcaster Name](#user-content-fn-15)[^15], or [Farcaster ID](#user-content-fn-16)[^16]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/MbCjfpghtO" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also holds BAYC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
 query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"]}, tokenType: {_in: [ERC721, ERC1155]}}}
        ) {
          formattedAmount
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0x2bd2b6e88b6653bb3615961090d421ac58e44be4",
              "0x980cd99311f3b3264f5dfdce33d204a8d6d492e4"
            ],
            "socials": [
              {
                "profileName": "if",
                "userId": "8151",
                "userAssociatedAddresses": [
                  "0x2bd2b6e88b6653bb3615961090d421ac58e44be4",
                  "0x980cd99311f3b3264f5dfdce33d204a8d6d492e4"
                ]
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1 // Hold 1 BAYC
</strong>              }
            ]
          },
          "followerProfileId": "8151",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "socials": [
              {
                "profileName": "vitalik.eth",
                "userId": "5650",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "followingProfileId": "5650"
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Farcaster Followers of Farcaster User(s) that Hold POAP(s)

You can get the list of Farcaster followers of Farcaster user(s) that also hold POAP(s), e.g. [EthCC\[6\] – Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=\&address=141910\&tokenType=\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29\&inputType=POAP\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=gnosis), by inputting [0x address](#user-content-fn-17)[^17], [ENS domain](#user-content-fn-18)[^18], [Farcaster Name](#user-content-fn-19)[^19], or [Farcaster ID](#user-content-fn-20)[^20]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/UJstxtiyUe" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that holds EthCC\[6] POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
          mintHash
          mintOrder
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xe79a8659fc91f07f80688a5fba034093a9a5412b",
              "0xb32792a81085001713e571f3b5794409e587a258"
            ],
            "socials": [
              {
                "profileName": "martinbreiten.eth",
                "userId": "2792",
                "userAssociatedAddresses": [
                  "0xe79a8659fc91f07f80688a5fba034093a9a5412b",
                  "0xb32792a81085001713e571f3b5794409e587a258"
                ]
              }
            ],
            "poaps": [
              {
                // This indicate that the follower has the EthCC[6] POAP
<strong>                "mintHash": "0x97902dfac8b17aedd67a736d9a57b437e7da450e058ff58a377ba24e230990e2",
</strong><strong>                "mintOrder": 1235
</strong>              }
            ]
          },
          "followerProfileId": "2792",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followingProfileId": "3"
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Farcaster Followers of Farcaster User(s) that Hold Certain Amount of ERC20 Token(s)

You can get the list of Farcaster followers of Farcaster user(s) that also hold a certain amount of ERC20 token(s), e.g. at least 1000 [USDC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&tokenType=\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29\&inputType=TOKEN\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting [0x address](#user-content-fn-21)[^21], [ENS domain](#user-content-fn-22)[^22], [Farcaster Name](#user-content-fn-23)[^23], or [Farcaster ID](#user-content-fn-24)[^24]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/F14x5wQet9" %}
Show me Farcaster followers of fc\_fname:dwr.eth, fc\_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also holds at least 1000 USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["fc_fname:dwr.eth", "fc_fid:602", "varunsrin.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: farcaster}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]}, tokenType: {_eq: ERC20}, formattedAmount: {_gte: 1000}}}
        ) {
          formattedAmount
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
          "followerAddress": {
            "addresses": [
              "0xd3c3228fa91ff3b2b146f4b4a1f6222e640ed559",
              "0x7954aa7d63fc32203f23177e502871c4d1e7307b"
            ],
            "socials": [
              {
                "profileName": "kevinli",
                "userId": "11370",
                "userAssociatedAddresses": [
                  "0xd3c3228fa91ff3b2b146f4b4a1f6222e640ed559",
                  "0x7954aa7d63fc32203f23177e502871c4d1e7307b"
                ]
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1494.378076 // hold at least 1000 USDC
</strong>              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "0x91031dcfdea024b4d51e775486111d2b2a715871"
            ],
            "domains": [
              {
                "name": "varunsrin.eth"
              }
            ],
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871"
                ]
              }
            ]
          },
          "followingProfileId": "2"
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster Followers data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)

[^1]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^2]: e.g. `varunsrin.eth`

[^3]: e.g. `fc_fname:dwr.eth`

[^4]: e.g. `fc_fid:602`

[^5]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^6]: e.g. `varunsrin.eth`

[^7]: e.g. `fc_fname:dwr.eth`

[^8]: e.g. `fc_fid:602`

[^9]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^10]: e.g. `varunsrin.eth`

[^11]: e.g. `fc_fname:dwr.eth`

[^12]: e.g. `fc_fid:602`

[^13]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^14]: e.g. `varunsrin.eth`

[^15]: e.g. `fc_fname:dwr.eth`

[^16]: e.g. `fc_fid:602`

[^17]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^18]: e.g. `varunsrin.eth`

[^19]: e.g. `fc_fname:dwr.eth`

[^20]: e.g. `fc_fid:602`

[^21]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`

[^22]: e.g. `varunsrin.eth`

[^23]: e.g. `fc_fname:dwr.eth`

[^24]: e.g. `fc_fid:602`
