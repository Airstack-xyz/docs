---
description: >-
  Learn how to get recommended followers for Lens profiles based on on-chain
  insights from token transfers, POAPs, NFTS, token holder combinations, and
  Lens & Farcaster social followers/following.
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

# 📞 Recommendation Users

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get Recommendation Follows For Lens Profile(s) Based on Token Transfers](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-token-transfers)
* [Get Recommendation Follows For Lens Profile(s) Based on POAPs](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-poaps)
* [Get Recommendation Follows For Lens Profile(s) Based on NFTs](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-nfts)
* [Get Recommendation Follows For Lens Profile(s) Based on NFTs and POAPs Commonly Held](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-nfts-and-poaps-commonly-held)
* [Get Recommendation Follows For Lens Profile(s) Based on Lens Followers](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-lens-followers)
* [Get Recommendation Follows For Lens Profile(s) Based on Farcaster Followers](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-farcaster-followers)
* [Get Recommendation Follows For Lens Profile(s) Based on Farcaster Following](recommendation-users.md#get-recommendation-follows-for-lens-profile-s-based-on-farcaster-following)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

### Get Started

**JavaScript/TypeScript/Python**

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
**React**

```sh
npm install @airstack/airstack-react
```

**Node**

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
**React**

```sh
yarn add @airstack/airstack-react
```

**Node**

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
**React**

```sh
pnpm install @airstack/airstack-react
```

**Node**

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
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

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
import { init, fetchQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const { data, error } = fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = "YOUR_QUERY" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Get Recommendation Follows For Lens Profile(s) Based on Token Transfers

To get recommendations by token transfers, simply fetch all token transfers that is received and sent from the Lens profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/ldjkYhPw98" %}
Show recommendations by token transfers for lens/@bradorbradley
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetRecommendationsByTokenTransfers {
  ethereum: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["lens/@bradorbradley"] }
          to: { _in: ["lens/@bradorbradley"] }
        }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
    }
  }
  polygon: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["lens/@bradorbradley"] }
          to: { _in: ["lens/@bradorbradley"] }
        }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
    }
  }
  base: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["lens/@bradorbradley"] }
          to: { _in: ["lens/@bradorbradley"] }
        }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: lens } } }) {
          userId
          profileName
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "ethereum": {
      "TokenTransfer": [
        {
          "from": {
            "addresses": ["0x41e54704acc2787b2b987b56111ca934ad3e3210"],
            "socials": [
              {
                "userId": "0x41e54704acc2787b2b987b56111ca934ad3e3210",
                "profileName": "lens/@noahmason"
              }
            ]
          },
          "to": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "lens/@bradorbradley"
              }
            ]
          }
        }
      ]
    },
    "polygon": {
      "TokenTransfer": [
        {
          "from": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "lens/@bradorbradley"
              }
            ]
          },
          "to": {
            "addresses": ["0xc8970a269384463c0033d7e71135e41581146006"],
            "socials": [
              {
                "userId": "0xc8970a269384463c0033d7e71135e41581146006",
                "profileName": "lens/@grams"
              }
            ]
          }
        }
      ]
    },
    "base": {
      "TokenTransfer": [
        {
          "from": {
            "addresses": ["0x32bea430631db4e416900db332f5431d265a954a"],
            "socials": null
          },
          "to": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "lens/@bradorbradley"
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Recommendation Follows For Lens Profile(s) Based on POAPs

To get recommendations by POAPs, first fetch all POAPs that is owned by the Lens profile(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/WndOFRhLKU" %}
Show POAPs owned by lens/@bradorbradley
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {
      filter: { owner: { _in: ["lens/@bradorbradley"] } }
      blockchain: ALL
    }
  ) {
    Poap {
      eventId
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
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
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "eventId": "47553",
          "poapEvent": {
            "eventName": "Proof of rAAVE - ETHCC Paris 2022",
            "eventURL": "https://aave.com",
            "startDate": "2022-07-01T00:00:00Z",
            "endDate": "2022-07-01T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/47553/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/47553/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/47553/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/47553/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/47553/small.png"
              }
            }
          }
        },
        {
          "eventId": "80122",
          "poapEvent": {
            "eventName": "You have met Patricio in November 2022 (IRL)",
            "eventURL": "http://poap.xyz",
            "startDate": "2022-11-01T00:00:00Z",
            "endDate": "2022-11-30T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/80122/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/80122/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/80122/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/80122/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/80122/small.png"
              }
            }
          }
        },
        {
          "eventId": "37964",
          "poapEvent": {
            "eventName": "AaveFam #18",
            "eventURL": "https://www.aave.com",
            "startDate": "2022-04-08T00:00:00Z",
            "endDate": "2022-04-08T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/37964/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/37964/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/37964/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/37964/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/37964/small.png"
              }
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

With the response, you can compile the `eventId`s as an array that can be forwarded as an input for the next query.

The next query for recommending follows based on POAPs will be fetching all the holders of the POAP with the `eventId`s in the array:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/y1vbz7WOI3" %}
Show follow recommendations based on POAP event IDs 47553, 80122, and 37964
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAllAddressesSocialsAndENSOfPOAP {
  Poaps(
    input: {
      filter: { eventId: { _in: ["47553", "80122", "37964"] } }
      blockchain: ALL
      limit: 200
    }
  ) {
    Poap {
      owner {
        identity
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          userId
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "identity": "0x6877fdeac0d0d5d61b4ca3bbd42501a2ff02c144",
            "socials": [
              {
                "profileName": "lens/@hermet",
                "userId": "0x6877fdeac0d0d5d61b4ca3bbd42501a2ff02c144"
              }
            ]
          }
        },
        {
          "owner": {
            "identity": "0x0000000000000000000000000000000000000000",
            "socials": null
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The follow recommendation will provide a list of Lens profiles with the Lens profile name from the `profileName` field.

### Get Recommendation Follows For Lens Profile(s) Based on NFTs

To get recommendations by NFTs, first fetch all NFTs that is owned by the Lens profile(s) on both Ethereum, Polygon, and Base:

#### Try Code

{% embed url="https://app.airstack.xyz/query/ky5KSrfQ24" %}
Show NFTs on Ethereum, Polygon, and Base owned by lens/@bradorbradley
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTs {
  ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
  polygon: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: polygon
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
  base: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["lens/@bradorbradley"] }
        tokenType: { _in: [ERC1155, ERC721] }
      }
      blockchain: base
      limit: 200
    }
  ) {
    TokenBalance {
      tokenAddress
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "ethereum": {
      "TokenBalance": [
        {
          "tokenAddress": "0xf5806ba5635911aa1d2b7b794172d55c731ca860"
        },
        {
          "tokenAddress": "0xa4ed1befd956d4f444e7010955c3c06ffbd46ac7"
        },
        {
          "tokenAddress": "0x9d90669665607f08005cae4a7098143f554c59ef"
        }
      ]
    },
    "polygon": {
      "TokenBalance": [
        {
          "tokenAddress": "0xa71d227ffc872a4627e832f87b49ef46b29dd050"
        },
        {
          "tokenAddress": "0x5ef718b8360ef2b82fb971b50350913e2bad4783"
        },
        {
          "tokenAddress": "0xdeb053d41521672af06ba75d61be3364977461af"
        }
      ]
    },
    "base": {
      "TokenBalance": null
    }
  }
}
```
{% endtab %}
{% endtabs %}

With the response, you can compile the many `tokenAddress` as an array that can be forwarded as an input for the next query.

The next query for recommending follows based on NFTs will be fetching all the holders of the NFT with the `eventId`s in the array:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/hiizdKMhjl" %}
Show follow recommendations based on NFTs for lens/@bradorbradley
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTHoldersAndImages {
  ethereum: TokenNfts(
    input: {
      filter: {
        address: {
          _in: [
            "0xf5806ba5635911aa1d2b7b794172d55c731ca860"
            "0xa4ed1befd956d4f444e7010955c3c06ffbd46ac7"
            "0x9d90669665607f08005cae4a7098143f554c59ef"
          ]
        }
      }
      blockchain: ethereum
    }
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: { filter: { dappName: { _eq: lens } } }) {
            profileName
            userId
          }
        }
      }
    }
  }
  polygon: TokenNfts(
    input: {
      filter: {
        address: {
          _in: [
            "0xa71d227ffc872a4627e832f87b49ef46b29dd050"
            "0x5ef718b8360ef2b82fb971b50350913e2bad4783"
            "0xdeb053d41521672af06ba75d61be3364977461af"
          ]
        }
      }
      blockchain: polygon
    }
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: { filter: { dappName: { _eq: lens } } }) {
            profileName
            userId
          }
        }
      }
    }
  }
  base: TokenNfts(
    input: {
      filter: {
        address: {
          _in: [
            "0xa71d227ffc872a4627e832f87b49ef46b29dd050"
            "0x5ef718b8360ef2b82fb971b50350913e2bad4783"
            "0xdeb053d41521672af06ba75d61be3364977461af"
          ]
        }
      }
      blockchain: base
    }
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: { filter: { dappName: { _eq: lens } } }) {
            profileName
            userId
          }
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "ethereum": {
      "TokenNft": null
    },
    "polygon": {
      "TokenNft": [
        {
          "tokenBalances": [
            {
              "owner": {
                "identity": "0xc75ad9f1b13c964d6bf9396cf78e6165f79cf947",
                "socials": [
                  {
                    "profileName": "lens/@yully",
                    "userId": "0xc75ad9f1b13c964d6bf9396cf78e6165f79cf947"
                  }
                ]
              }
            }
          ]
        },
        {
          "tokenBalances": [
            {
              "owner": {
                "identity": "0xcc0dc98ce0db0b73d27f9c496119c02d34f145b3",
                "socials": [
                  {
                    "profileName": "lens/@moonlfg",
                    "userId": "0xcc0dc98ce0db0b73d27f9c496119c02d34f145b3"
                  }
                ]
              }
            }
          ]
        }
      ]
    },
    "base": {
      "TokenNft": null
    }
  }
}
```
{% endtab %}
{% endtabs %}

The follow recommendation will provide a list of Lens profiles name from the `profileName` field.

### Get Recommendation Follows For Lens Profile(s) Based on NFTs and POAPs Commonly Held

#### Fetching

You can fetch follow recommendations based on the common holder of a specific NFT and a specific POAP by providing the token contract address and the POAP event ID:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/lHTqRfk5wb" %}
Show common holders of Sound.xyz NFT and Aave Booth Permissionless POAP that also have Lens Profile
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfNounsAndEthCC {
  TokenBalances(
<strong>    input: {filter: {tokenAddress: {_eq: "0x977e43ab3eb8c0aece1230ba187740342865ee78"}}, blockchain: ethereum, limit: 200}
</strong>  ) {
    TokenBalance {
      owner {
<strong>        poaps(input: {filter: {eventId: {_eq: "43882"}}}) {
</strong>          owner {
            socials(input: {filter: {dappName: {_eq: lens}}}) {
              profileName
              userId
              userAssociatedAddresses
            }
          }
        }
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "lens/@westlakevillage",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "lens/@brad",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "lens/@bradorbradley",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "lens/@hanimourra",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have Nouns, but no EthCC[6] POAP
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' Lens profile details will be returned inside the innermost `owner.socials` field.

#### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) => owner?.socials)
  )
    .filter(Boolean)
    .flat(2)
    .filter((social, index, array) => array.indexOf(social) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'socials' in poap['owner']:
                        result.append(poap['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result

```
{% endtab %}
{% endtabs %}

The final result will be the list of all common holders in an array:

```json
[
  {
    "profileName": "lens/@westlakevillage",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"]
  },
  {
    "profileName": "lens/@brad",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"]
  },
  {
    "profileName": "lens/@bradorbradley",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"]
  },
  {
    "profileName": "lens/@hanimourra",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"]
  }
  // ...other token holders
]
```

### Get Recommendation Follows For Lens Profile(s) Based on Lens Followers

#### Fetching

You can fetch follow recommendations based on the Lens followers of Lens profile(s) by fetching the list of Lens followers of the given Lens profile(s):

**Try Demo**

{% embed url="https://app.airstack.xyz/query/djxR2UrtIB" %}
Show me all Lens followers of lens/@stani, lens\_id:0x24, betashop.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: lens }
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x24"
            "betashop.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
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
            "addresses": ["0xcde3725b25d6d9bc78cf0941cc15fd9710c764b9"],
            "socials": [
              {
                "profileName": "lens/@nicolo",
                "profileTokenId": "9",
                "profileTokenIdHex": "0x09"
              }
            ]
          },
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": "0x0185ab"
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": "0x01c776"
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024"
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": "0x01c60f"
              }
            ]
          }
        }
        // more recommendation from Lens followers
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Recommendation Follows For Lens Profile(s) Based on Farcaster Followers

#### Fetching

You can fetch follow recommendations based on the Farcaster followers of Lens profile(s) by fetching the list of Farcaster followers of the given Lens profile(s):

{% hint style="info" %}
If the address that owns the given Lens profile NFT does not own any Farcaster account, then no Farcaster followers will exist and API response will be `null`.
{% endhint %}

**Try Demo**

{% embed url="https://app.airstack.xyz/query/8Dff1C8SHM" %}
Show me all Farcaster followers of lens/@stani, lens\_id:0x24, betashop.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 and their Lens profiles
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: { _in: ["lens/@vitalik"] }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
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
              "0x33aa7c91c9afe6215ece55d520791e32f2c420e9",
              "0x18d997237770f3f4b1f722eba605e938dc28ab67",
              "0xa905af4f03337551bf8e5de2e008249958c78e9b"
            ],
            "socials": [
              {
<strong>                "profileName": "lens/@kamilmouthon", // the follower's Lens profile handle
</strong>                "profileTokenId": "57006",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth"
              },
              {
                "name": "7860000.eth"
              },
              {
                "name": "offchainexample.eth"
              },
              {
                "name": "brianshaw.eth"
              },
              {
                "name": "vbuterin.stateofus.eth"
              },
              {
                "name": "quantumsmartcontracts.eth"
              },
              {
                "name": "Vitalik.eth"
              },
              {
                "name": "openegp.eth"
              },
              {
                "name": "vitalik.cannafam.eth"
              },
              {
                "name": "VITALIK.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": ""
              }
            ]
          }
        },
        // more Farcaster followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Get Recommendation Follows For Lens Profile(s) Based on Farcaster Following

#### Fetching

You can fetch follow recommendations based on the Farcaster following of Lens profile(s) by fetching the list of Farcaster followers of the given Lens profile(s):

{% hint style="info" %}
If the address that owns the given Lens profile NFT does not own any Farcaster account, then no Farcaster following will exist and API response will be `null`.
{% endhint %}

**Try Demo**

{% embed url="https://app.airstack.xyz/query/ASCSVYV3SO" %}
Show all Farcaster following by lens/@vitalik and show their Lens profiles
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: { _in: ["lens/@vitalik"] }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x4ce34af3378a00c640125e4dbf4c9e64dff4c93b",
              "0x849151d7d0bf1f34b70d5cad5149d28cc2308bf1",
              "0x6e0d9c6dd8a08509bb625caa35dc61a991406f62",
              "0xe73f9c181b571cac2bf3173634d04a9921b7ffcf"
            ],
            "socials": [
              {
                "profileName": "lens/@jessepollak",
                "profileTokenId": "110917",
                "profileTokenIdHex": "0x01b145"
              }
            ]
          },
          "followerProfileId": "5650",
          "followerAddress": {
            "addresses": [
              "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth"
              },
              {
                "name": "7860000.eth"
              },
              {
                "name": "offchainexample.eth"
              },
              {
                "name": "brianshaw.eth"
              },
              {
                "name": "vbuterin.stateofus.eth"
              },
              {
                "name": "quantumsmartcontracts.eth"
              },
              {
                "name": "Vitalik.eth"
              },
              {
                "name": "openegp.eth"
              },
              {
                "name": "vitalik.cannafam.eth"
              },
              {
                "name": "VITALIK.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3"
              }
            ]
          },
          "followingProfileId": "99"
        }
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding building a recommendation engine, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Recommendation Engine](../recommend-users/)
  * [Token Transfers](../recommend-users/token-transfers.md)
  * [POAPs](../recommend-users/poaps.md)
  * [NFTs](../recommend-users/nfts.md)
* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
* [POAPs API Reference](../../api-references/api-reference/poaps-api.md)
* [TokenNft API Reference](../../api-references/api-reference/tokennfts-api.md)
