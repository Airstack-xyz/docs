---
description: >-
  Learn how to get recommended followers for Farcaster users based on on-chain
  insights from token transfers, POAPs, NFTS, and token holder combinations.
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

# 📞 Recommendation Engines

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Recommendation Follows For Farcaster User(s) Based on Token Transfers](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-token-transfers)
- [Get Recommendation Follows For Farcaster User(s) Based on POAPs](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-poaps)
- [Get Recommendation Follows For Farcaster User(s) Based on NFTs](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-nfts)
- [Get Recommendation Follows For Farcaster User(s) Based on NFTs and POAPs Commonly Held](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-nfts-and-poaps-commonly-held)
- [Get Recommendation Follows For Farcaster User(s) Based on Farcaster Followers](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-farcaster-followers)
- [Get Recommendation Follows For Farcaster User(s) Based on Lens Following](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-lens-following)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL

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

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Get Recommendation Follows For Farcaster User(s) Based on Token Transfers

To get recommendations by token transfers, simply fetch all token transfer that is received and sent from the Farcaster user(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/fAtWCdyFrU" %}
Show recommendations by token transfers for Farcaster user name varunsrin.eth and id 5650
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetRecommendationsByTokenTransfers {
  # first query on Ethereum
  ethereum: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
          to: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
        }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          userId
          profileName
        }
      }
    }
  }
  # second query on Polygon
  polygon: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
          to: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
        }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          userId
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          userId
          profileName
        }
      }
    }
  }
  # third query on Base
  base: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
          to: { _in: ["fc_fname:dwr", "fc_fid:5650"] }
        }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          userId
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
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
            "addresses": ["0xf8e8f48d0e72e41a21acc8897d4012126d04de78"],
            "socials": [
              {
                "userId": "9837",
                "profileName": "shah"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "userId": "5650",
                "profileName": "vbuterin"
              }
            ]
          }
        }
        // Other Ethereum Token Transfers
      ]
    },
    "polygon": {
      "TokenTransfer": [
        {
          "from": {
            "addresses": ["0xf8e8f48d0e72e41a21acc8897d4012126d04de78"],
            "socials": [
              {
                "userId": "3811",
                "profileName": "betashop.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "userId": "10",
                "profileName": "varunsrin.eth"
              }
            ]
          }
        }
        // Other Polygon Token Transfers
      ]
    },
    "base": {
      "TokenTransfer": [
        {
          "from": {
            "addresses": ["0x919ba4118e5b566b35b62364613bb8f6bd8e2c61"],
            "socials": null,
            "domains": null
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "userId": "5650",
                "profileName": "vitalik.eth"
              }
            ]
          }
        }
        // Other Base Token Transfers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

### Get Recommendation Follows For Farcaster User(s) Based on POAPs

To get recommendations by POAPs, first fetch all POAPs that is owned by the Farcaster user(s):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/9gNjxdNWsP" %}
Show POAPs owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query POAPsOwnedByFarcasterUser {
  Poaps(
    input: {
      filter: { owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] } }
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
          "eventId": "6584",
          "poapEvent": {
            "eventName": "Pudgy Penguin owner as of August 2021",
            "eventURL": "https://www.stonercats.com/",
            "startDate": "2021-08-30T00:00:00Z",
            "endDate": "2021-09-30T00:00:00Z",
            "country": "United States",
            "city": "Ethereum",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/100/6584/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/100/6584/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/100/6584/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/100/6584/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/100/6584/small.png"
              }
            }
          }
        },
        {
          "eventId": "14498",
          "poapEvent": {
            "eventName": "ConstitutionDAO Contributor",
            "eventURL": "https://www.constitutiondao.com/",
            "startDate": "2021-11-18T00:00:00Z",
            "endDate": "2021-11-18T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/1/14498/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/1/14498/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/1/14498/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/1/14498/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/1/14498/small.png"
              }
            }
          }
        },
        {
          "eventId": "6481",
          "poapEvent": {
            "eventName": "Fractional Early Adopter",
            "eventURL": "https://fractional.art/",
            "startDate": "2021-08-27T00:00:00Z",
            "endDate": "2021-08-27T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/1/6481/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/1/6481/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/1/6481/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/1/6481/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/1/6481/small.png"
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

{% embed url="https://app.airstack.xyz/query/LTs9rbCXWJ" %}
Show follow recommendations based on POAP event IDs 6584, 14498, and 6481
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetAllAddressesSocialsAndENSOfPOAP {
  Poaps(
    input: {
      filter: { eventId: { _in: ["6584", "14498", "6481"] } }
      blockchain: ALL
      limit: 10
    }
  ) {
    Poap {
      owner {
        identity
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
            "identity": "0xd8f3676a7567842295170b3f6ae4bd466a878374",
            "socials": [
              {
                "profileName": "cryptoraketeros",
                "userId": "15366"
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

The follow recommendation will provide a list of Farcaster users with the Farcaster user name from the `profileName` field and Farcaster user ID from the `userId` field.

### Get Recommendation Follows For Farcaster User(s) Based on NFTs

To get recommendations by NFTs, first fetch all NFTs that is owned by the Farcaster user(s) on Ethereum, Polygon, and Base:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/ZpYY2KkXDv" %}
Show NFTs on Ethereum, Polygon, and Base owned by Farcaster user name varunsrin.eth and id 5650
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetNFTs {
  ethereum: TokenBalances(
    input: {
      filter: {
        owner: { _in: ["fc_fname:varunsrin.eth", "fc_fid:5650"] }
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
        owner: { _in: ["fc_fname:varunsrin.eth", "fc_fid:5650"] }
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
        owner: { _in: ["fc_fname:varunsrin.eth", "fc_fid:5650"] }
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
          "tokenAddress": "0x00000000000061ad8ee190710508a818ae5325c3"
        },
        {
          "tokenAddress": "0x37fb80ef28008704288087831464058a4a3940ae"
        },
        {
          "tokenAddress": "0x5a3c077906abf9a46a9de87bb3c6d075a8a67851"
        }
      ]
      // Other Ethereum NFT Collections
    },
    "polygon": {
      "TokenBalance": [
        {
          "tokenAddress": "0xd18359edd97ff13609c1978452b05b43213222d7"
        },
        {
          "tokenAddress": "0x27562885f784616be44a0dc801ff18ed4551ba3d"
        },
        {
          "tokenAddress": "0x579720c63ed37fd4bd60a44fc27a25d6f169e95e"
        }
      ]
      // Other Polygon NFT Collections
    },
    "base": {
      "TokenBalance": [
        {
          "tokenAddress": "0x7f9f222d2c492bf3c876ecb03a148884b90020f8"
        },
        {
          "tokenAddress": "0xfd0c723375dd37085dded2c6bab99ee590edec7a"
        },
        {
          "tokenAddress": "0x2790e25a247efa375bd313692356a9ffb0d5639f"
        }
        // Other Base NFT Collections
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

With the response, you can compile the many `tokenAddress` as an array that can be forwarded as an input for the next query.

The next query for recommending follows based on NFTs will be fetching all the holders of the NFT with the `eventId`s in the array:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/DfxElQUlKx" %}
Show follow recommendations based on NFTs
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
            "0x00000000000061ad8ee190710508a818ae5325c3"
            "0x37fb80ef28008704288087831464058a4a3940ae"
            "0x5a3c077906abf9a46a9de87bb3c6d075a8a67851"
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
          socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
            "0xd18359edd97ff13609c1978452b05b43213222d7"
            "0x27562885f784616be44a0dc801ff18ed4551ba3d"
            "0x579720c63ed37fd4bd60a44fc27a25d6f169e95e"
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
          socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
            "0xd18359edd97ff13609c1978452b05b43213222d7"
            "0x27562885f784616be44a0dc801ff18ed4551ba3d"
            "0x579720c63ed37fd4bd60a44fc27a25d6f169e95e"
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
          socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
      "TokenNft": [
        {
          "tokenBalances": [
            {
              "owner": {
                "identity": "0x0734d56da60852a03e2aafae8a36ffd8c12b32f1",
                "socials": [
                  {
                    "profileName": "0age",
                    "userId": "4262"
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
                "identity": "0xb16266ba63506cb4de9b2328bada30064e196a3b",
                "socials": null // Have no Farcaster, can be filtered out
              }
            }
          ]
        }
      ]
    },
    "polygon": {
      "TokenNft": [
        {
          "tokenBalances": [
            {
              "owner": {
                "identity": "0x87fdd2217f4d8531b5d091d1b290eb64d26f415b",
                "socials": null
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

The follow recommendation will provide a list of Farcaster users with the Farcaster user name from the `profileName` field and Farcaster user ID from the `userId` field.

### Get Recommendation Follows For Farcaster User(s) Based on NFTs and POAPs Commonly Held

#### Fetching

You can fetch follow recommendations based on the common holder of a specific NFT and a specific POAP by providing the token contract address and the POAP event ID:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/8xD3r6s6lU" %}
Show common holders of Nouns NFT and EthCC POAP that also have Farcaster
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfNounsAndEthCC {
  TokenBalances(
<strong>    input: {filter: {tokenAddress: {_eq: "0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03"}}, blockchain: ethereum, limit: 200}
</strong>  ) {
    TokenBalance {
      owner {
<strong>        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
</strong>          owner {
            socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
                      "profileName": "worthalter",
                      "userId": "9456",
                      "userAssociatedAddresses": [
                        "0xc19628f57a46389b0ac0fc113de273f91b07faca",
                        "0xf6b6f07862a02c85628b3a9688beae07fea9c863"
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

All the common holders' Farcaster details will be returned inside the innermost `owner.socials` field.

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
            if 'owner' in item and 'poaps' in item['owner'] and item['owner']['poaps'] is not None:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'socials' in poap['owner']:
                        result.append(poap['owner']['socials'])

    result = [item for sublist in result for item in sublist]

    return result
```

{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "worthalter",
    "userId": "9456",
    "userAssociatedAddresses": [
      "0xc19628f57a46389b0ac0fc113de273f91b07faca",
      "0xf6b6f07862a02c85628b3a9688beae07fea9c863"
    ]
  }
  // ...other token holders
]
```

### Get Recommendation Follows For Farcaster User(s) Based on Farcaster Followers

#### Fetching

You can fetch follow recommendations for Farcaster user(s) by simply showing them all the Farcaster accounts that followed the given user(s):

**Try Demo**

{% embed url="https://app.airstack.xyz/query/d56c7N9Sy6" %}
Show me all Farcaster followers of fname dwr.eth and their Farcaster account details
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
        identity: { _in: ["fc_fname:dwr.eth"] }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
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
              "0xb83d63d179838b439f3fa9f35d87f29fcec7b343",
              "0x7bdee41767e9aac283b86dd73133dd34d735cfac"
            ],
            "socials": [
              {
<strong>                "profileName": "vipulgoyal", // One of Farcaster follower that can be recommended
</strong>                "userId": "11282",
                "userAssociatedAddresses": [
                  "0xb83d63d179838b439f3fa9f35d87f29fcec7b343",
                  "0x7bdee41767e9aac283b86dd73133dd34d735cfac"
                ]
              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f"
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
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f"
                ]
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

With the response, you can get all `followerAddress.socials` and compile them into an array of Farcaster accounts that you can recommend for the given user(s) to follow.

### Get Recommendation Follows For Farcaster User(s) Based on Lens Following

#### Fetching

You can fetch follow recommendations for Farcaster user(s) by simply showing them all their Lens following and show their corresponding Farcaster accounts, if any:

**Try Demo**

{% embed url="https://app.airstack.xyz/query/mjaRr2aBoY" %}
Show me all Lens following of fc_fname:dwr.eth and their Farcaster account details
{% endembed %}

**Code**

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: { _in: ["fc_fname:dwr.eth"] }
        dappName: { _eq: lens }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
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
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x66da63b03feca7dd44a5bb023bb3645d3252fa32"
            ],
            "socials": [
              {
<strong>                "profileName": "keeks", // One of Lens following that can be recommended
</strong>                "userId": "3283",
                "userAssociatedAddresses": [
                  "0xadc0b2321bb9779bf2a565c51456fa300517bed5",
                  "0x66da63b03feca7dd44a5bb023bb3645d3252fa32"
                ]
              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
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
                  "0xff3174d9f52f8c24f9c884600ac80c2b2dda4556",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea"
                ]
              }
            ]
          }
        },
        {
          "followingAddress": {
            "addresses": [
              "0xcd3b95032441457aa6382fd1312753353368be41"
            ],
<strong>            "socials": null // This Lens following have no Farcaster, should be filtered out
</strong>          },
          "followerAddress": {
            "addresses": [
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
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
                  "0xff3174d9f52f8c24f9c884600ac80c2b2dda4556",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea"
                ]
              }
            ]
          }
        },
        // more Lens following
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

All the Farcaster accounts that can be used for recommendation will be returned in `followingAddress.socials`.

#### Formatting

To get the list of all following in a flat array and filter out all those that don't have any Farcaster account, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}

```javascript
/**
 * @description Formats the given data.
 * @example
 * // For React
 * const { data } = useQuery(query);
 * formatFunction(data);
 *
 * // For Node
 * const { data } = await fetchQuery(query);
 * formatFunction(data);
 *
 * @param {Object} data – data result from the Airstack API call
 * @returns – an array of Farcaster and their user details for recommendataion
 */
const formatFunction = (data) =>
  data?.SocialFollowings?.Following?.map(({ followingAddress }) =>
    followingAddress?.xmtp?.length ? followingAddress?.socials : null
  )
    .filter(Boolean)
    .flat(1)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```

{% endtab %}

{% tab title="Python" %}

```python
"""
Formats the given data.

Examples:
  query_response = await execute_query_client.execute_query()
  formatted_data = format_function(query_response.data)

Args:
  data (dict): Data result from the Airstack API call.

Returns:
  list: An array of Farcaster and their user details for recommendation.
"""
def format_function(data):
    result = []

    if data and 'SocialFollowings' in data and 'Following' in data['SocialFollowings']:
        for following in data['SocialFollowings']['Following']:
            if 'followingAddress' in following and following['followingAddress']['xmtp'] is not None and len(following['followingAddress']['xmtp']) > 0 and following['followingAddress']['socials'] is not None:
                result.extend(following['followingAddress']['socials'])

    return result
```

{% endtab %}
{% endtabs %}

The formatted data will have data structure that look as follows:

```json
[
  {
    "profileName": "keeks",
    "userId": "3283",
    "userAssociatedAddresses": [
      "0xadc0b2321bb9779bf2a565c51456fa300517bed5",
      "0x66da63b03feca7dd44a5bb023bb3645d3252fa32"
    ]
  }
  // more recommended users
]
```

### Developer Support

If you have any questions or need help regarding building a recommendation engine, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

- [Contact Recommendation](../recommendation-engine/)
  - [Token Transfers](../recommendation-engine/token-transfers.md)
  - [POAPs](../recommendation-engine/poaps.md)
  - [NFTs](../recommendation-engine/nfts.md)
- [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api/)
- [POAPs API Reference](../../api-references/api-reference/poaps-api/)
- [TokenNft API Reference](../../api-references/api-reference/tokennfts-api/)
