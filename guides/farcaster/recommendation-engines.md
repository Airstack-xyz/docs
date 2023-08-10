---
description: >-
  Learn how to get recommended followers for Farcaster users based on on-chain
  insights from token transfers, POAPs, NFTS, and token holder combinations.
---

# 📞 Recommendation Engines

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

In this tutorial, you will learn how to recommend followers for Farcaster users based on on-chain insights from token transfers, POAPs, NFTS, and token holder combinations.

In this guide you will learn how to use Airstack to:

* [Get Recommendation Follows For Farcaster User(s) Based on Token Transfers](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-token-transfers)
* [Get Recommendation Follows For Farcaster User(s) Based on POAPs](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-poaps)
* [Get Recommendation Follows For Farcaster User(s) Based on NFTs](recommendation-engines.md#get-recommendation-follows-for-farcaster-user-s-based-on-nfts)

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

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }

  // Render your component using the data returned by the query
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

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your JSON endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Recommendation Follows For Farcaster User(s) Based on Token Transfers

To get recommendations by token transfers, simply fetch all token transfer that is received and sent from the Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/CJuhCzxDH0" %}
Show recommendations by token transfers for Farcaster user name varunsrin.eth and id 5650
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetRecommendationsByTokenTransfers {
  # first query on Ethereum
  ethereum: TokenTransfers(
    input: {filter: {_or: {from: {_in: ["fc_fname:dwr", "fc_fid:5650"]}, to: {_in: ["fc_fname:dwr", "fc_fid:5650"]}}}, blockchain: ethereum, limit: 50}
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: {_in: farcaster}}}) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: {_in: farcaster}}}) {
          userId
          profileName
        }
      }
    }
  }
  # second query on Polygon
  polygon: TokenTransfers(
    input: {filter: {_or: {from: {_in: ["fc_fname:dwr", "fc_fid:5650"]}, to: {_in: ["fc_fname:dwr", "fc_fid:5650"]}}}, blockchain: polygon, limit: 50}
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: { filter: { dappName: {_in: farcaster}}}) {
          userId
          profileName
        }
        domains {
          dappName
        }
      }
      to {
        addresses
        socials(input: { filter: { dappName: {_in: farcaster}}}) {
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
            "addresses": [
              "0xf8e8f48d0e72e41a21acc8897d4012126d04de78"
            ],
            "socials": [
              {
                "userId": "9837",
                "profileName": "shah"
              }
            ]
          },
          "to": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "socials": [
              {
                "userId": "5650",
                "profileName": "vbuterin"
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
            "addresses": [
              "0xf8e8f48d0e72e41a21acc8897d4012126d04de78"
            ],
            "socials": [
              {
                "userId": "3811",
                "profileName": "betashop.eth"
              }
            ]
          },
          "to": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "socials": [
              {
                "userId": "10",
                "profileName": "varunsrin.eth"
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

## Get Recommendation Follows For Farcaster User(s) Based on POAPs

To get recommendations by POAPs, first fetch all POAPs that is owned by the Farcaster user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/9gNjxdNWsP" %}
Show POAPs owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByFarcasterUser {
  Poaps(
    input: {filter: {owner: {_in: ["fc_fname:dwr.eth", "fc_fid:1"]}}, blockchain: ALL}
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

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/LTs9rbCXWJ" %}
Show follow recommendations based on POAP event IDs 6584, 14498, and 6481
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAllAddressesSocialsAndENSOfPOAP {
  Poaps(input: {filter: {eventId: {_in: ["6584", "14498", "6481"]}}, blockchain: ALL, limit: 10}) {
    Poap {
      owner {
        identity
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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

## Get Recommendation Follows For Farcaster User(s) Based on NFTs

To get recommendations by NFTs, first fetch all NFTs that is owned by the Farcaster user(s) on both Ethereum and Polygon:

### Try Code

{% embed url="https://app.airstack.xyz/DTyOZg/FZcNb4cU1W" %}
Show NFTs on Ethereum and Polygon owned by Farcaster user name varunsrin.eth and id 5650
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTs {
  ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["fc_fname:varunsrin.eth", "fc_fid:5650"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      tokenAddress
    }
  }
  polygon: TokenBalances(
    input: {filter: {owner: {_in: ["fc_fname:varunsrin.eth", "fc_fid:5650"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 200}
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
    }
  }
}
```
{% endtab %}
{% endtabs %}

With the response, you can compile the many `tokenAddress` as an array that can be forwarded as an input for the next query.

The next query for recommending follows based on NFTs will be fetching all the holders of the NFT with the `eventId`s in the array:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/j1IKcsBzht" %}
Show follow recommendations based on NFTs
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTHoldersAndImages {
  ethereum: TokenNfts(
    input: {filter: {address: {_in: ["0x00000000000061ad8ee190710508a818ae5325c3", "0x37fb80ef28008704288087831464058a4a3940ae", "0x5a3c077906abf9a46a9de87bb3c6d075a8a67851"]}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: {filter: {dappName: {_eq: farcaster}}}) {
            profileName
            userId
          }
        }
      }
    }
  }
  polygon: TokenNfts(
    input: {filter: {address: {_in: ["0xd18359edd97ff13609c1978452b05b43213222d7", "0x27562885f784616be44a0dc801ff18ed4551ba3d", "0x579720c63ed37fd4bd60a44fc27a25d6f169e95e"]}}, blockchain: polygon}
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
// Some code
```
{% endtab %}
{% endtabs %}

The follow recommendation will provide a list of Farcaster users with the Farcaster user name from the `profileName` field and Farcaster user ID from the `userId` field.

## Get Recommendation Follows For Farcaster User(s) Based on NFTs and POAPs Commonly Held

### Fetching

You can fetch follow recommendations based on the common holder of a specific NFT and a specific POAP by providing the token contract address and the POAP event ID:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/8xD3r6s6lU" %}
Show common holders of Nouns NFT and EthCC POAP that also have Farcaster
{% endembed %}

#### Code

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

### Formatting

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
  },
  // ...other token holders
]
```

## Developer Support

If you have any questions or need help regarding building a recommendation engine, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Contact Recommendation](../contact-recommendation/)
  * [Token Transfers](../contact-recommendation/token-transfers.md)
  * [POAPs](../contact-recommendation/poaps.md)
  * [NFTs](../contact-recommendation/nfts.md)
* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
* [TokenNft API Reference](../../api-references/api-reference/tokennfts-api/)
