---
description: >-
  Learn how to get recommended followers for Lens profiles based on on-chain
  insights from token transfers, POAPs, NFTS, and token holder combinations.
---

# 📞 Recommendation Engines

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to recommend followers for Lens profiles based on on-chain insights from token transfers, POAPs, NFTS, and token holder combinations.

In this guide you will learn how to use Airstack to:

* [Get Recommendation Follows For Lens Profile(s) Based on Token Transfers](recommendation-engines.md#get-recommendation-follows-for-lens-profile-s-based-on-token-transfers)
* [Get Recommendation Follows For Lens Profile(s) Based on POAPs](recommendation-engines.md#get-recommendation-follows-for-lens-profile-s-based-on-poaps)
* [Get Recommendation Follows For Lens Profile(s) Based on NFTs](recommendation-engines.md#get-recommendation-follows-for-lens-profile-s-based-on-nfts)
* [Get Recommendation Follows For Lens Profile(s) Based on NFTs and POAPs Commonly Held](recommendation-engines.md#get-recommendation-follows-for-lens-profile-s-based-on-nfts-and-poaps-commonly-held)

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

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Recommendation Follows For Lens Profile(s) Based on Token Transfers

To get recommendations by token transfers, simply fetch all token transfer that is received and sent from the Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/NUbezIUVe9" %}
Show recommendations by token transfers for bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetRecommendationsByTokenTransfers {
  ethereum: TokenTransfers(
    input: {filter: {_or: {from: {_in: ["bradorbradley.lens"]}, to: {_in: ["bradorbradley.lens"]}}}, blockchain: ethereum, limit: 50}
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: {filter: {dappName: {_in: lens}}}) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: {filter: {dappName: {_in: lens}}}) {
          userId
          profileName
        }
      }
    }
  }
  polygon: TokenTransfers(
    input: {filter: {_or: {from: {_in: ["bradorbradley.lens"]}, to: {_in: ["bradorbradley.lens"]}}}, blockchain: polygon, limit: 50}
  ) {
    TokenTransfer {
      from {
        addresses
        socials(input: {filter: {dappName: {_in: lens}}}) {
          userId
          profileName
        }
      }
      to {
        addresses
        socials(input: {filter: {dappName: {_in: lens}}}) {
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
              "0x41e54704acc2787b2b987b56111ca934ad3e3210"
            ],
            "socials": [
              {
                "userId": "0x41e54704acc2787b2b987b56111ca934ad3e3210",
                "profileName": "noahmason.lens"
              }
            ]
          },
          "to": {
            "addresses": [
              "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
            ],
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "bradorbradley.lens"
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
              "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
            ],
            "socials": [
              {
                "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                "profileName": "bradorbradley.lens"
              }
            ]
          },
          "to": {
            "addresses": [
              "0xc8970a269384463c0033d7e71135e41581146006"
            ],
            "socials": [
              {
                "userId": "0xc8970a269384463c0033d7e71135e41581146006",
                "profileName": "grams.lens"
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

## Get Recommendation Follows For Lens Profile(s) Based on POAPs

To get recommendations by POAPs, first fetch all POAPs that is owned by the Lens profile(s):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/WndOFRhLKU" %}
Show POAPs owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}}, blockchain: ALL}
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

### Try Demo

{% embed url="https://app.airstack.xyz/query/y1vbz7WOI3" %}
Show follow recommendations based on POAP event IDs 47553, 80122, and 37964
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAllAddressesSocialsAndENSOfPOAP {
  Poaps(
    input: {filter: {eventId: {_in: ["47553", "80122", "37964"]}}, blockchain: ALL, limit: 200}
  ) {
    Poap {
      owner {
        identity
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
                "profileName": "hermet.lens",
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

## Get Recommendation Follows For Lens Profile(s) Based on NFTs

To get recommendations by NFTs, first fetch all NFTs that is owned by the Lens profile(s) on both Ethereum and Polygon:

### Try Code

{% embed url="https://app.airstack.xyz/query/ik0ptBHZU3" %}
Show NFTs on Ethereum and Polygon owned by bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTs {
  ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      tokenAddress
    }
  }
  polygon: TokenBalances(
    input: {filter: {owner: {_in: ["bradorbradley.lens"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: polygon, limit: 200}
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
    }
  }
}
```
{% endtab %}
{% endtabs %}

With the response, you can compile the many `tokenAddress` as an array that can be forwarded as an input for the next query.

The next query for recommending follows based on NFTs will be fetching all the holders of the NFT with the `eventId`s in the array:

### Try Demo

{% embed url="https://app.airstack.xyz/query/JIrNUBryfO" %}
Show follow recommendations based on NFTs for bradorbradley.lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNFTHoldersAndImages {
  ethereum: TokenNfts(
    input: {filter: {address: {_in: ["0xf5806ba5635911aa1d2b7b794172d55c731ca860", "0xa4ed1befd956d4f444e7010955c3c06ffbd46ac7", "0x9d90669665607f08005cae4a7098143f554c59ef"]}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: {filter: {dappName: {_eq: lens}}}) {
            profileName
            userId
          }
        }
      }
    }
  }
  polygon: TokenNfts(
    input: {filter: {address: {_in: ["0xa71d227ffc872a4627e832f87b49ef46b29dd050", "0x5ef718b8360ef2b82fb971b50350913e2bad4783", "0xdeb053d41521672af06ba75d61be3364977461af"]}}, blockchain: polygon}
  ) {
    TokenNft {
      tokenBalances {
        owner {
          identity
          socials(input: {filter: {dappName: {_eq: lens}}}) {
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
                "identity": "0xc75ad9f1b13c964d6bf9396cf78e6165f79cf947",
                "socials": [
                  {
                    "profileName": "yully.lens",
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
                    "profileName": "moonlfg.lens",
                    "userId": "0xcc0dc98ce0db0b73d27f9c496119c02d34f145b3"
                  }
                ]
              }
            }
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

The follow recommendation will provide a list of Lens profiles name from the `profileName` field.

## Get Recommendation Follows For Lens Profile(s) Based on NFTs and POAPs Commonly Held

### Fetching

You can fetch follow recommendations based on the common holder of a specific NFT and a specific POAP by providing the token contract address and the POAP event ID:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/lHTqRfk5wb" %}
Show common holders of Sound.xyz NFT and Aave Booth Permissionless POAP that also have Lens Profile
{% endembed %}

#### Code

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
                      "profileName": "westlakevillage.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "brad.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "bradorbradley.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "hanimourra.lens",
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
    "profileName": "westlakevillage.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "brad.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "bradorbradley.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "hanimourra.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
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
