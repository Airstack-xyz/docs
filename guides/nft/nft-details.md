---
description: >-
  Learn how to fetch ERC721 & ERC1155 NFT details data, which includes name,
  symbol, NFT images (original & resized), metadata from a specific NFT or an
  NFT collection.
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

# 📑 NFT Details

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 social applications with NFT data from Ethereum and Polygon.

In this tutorial, you will learn how to fetch ERC721 & ERC1155 NFT details data, which includes name, symbol, NFT images (original & resized), metadata from a specific NFT or an NFT collection.

In this guide you will learn how to use Airstack to:

* [Get NFT Total Supply](nft-details.md#get-nft-total-supply)
* [Get NFT Name & Symbol](nft-details.md#get-nft-name-and-symbol)
* [Get NFT Metadata of Specific NFT](nft-details.md#get-nft-metadata-of-specific-nft)
* [Get NFT Metadata of An NFT Collection](nft-details.md#get-nft-metadata-of-an-nft-collection)
* [Get NFT Images of Specific NFT](nft-details.md#get-nft-images-of-specific-nft)
* [Get NFT Images of An NFT Collection](nft-details.md#get-nft-images-of-an-nft-collection)
* [Get Specific NFT in An NFT Collection](nft-details.md#get-specific-nft-in-an-nft-collection)
* [Get All NFT In An NFT Collection](nft-details.md#get-all-nft-in-an-nft-collection)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

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

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get NFT Collection Total Supply

You can fetch an NFT collection's total supply by using the `totalSupply` field from  [`Tokens`](../../api-references/api-reference/tokens-api/) API by providing NFT collection `address` as an input:

### Try Code

{% embed url="https://app.airstack.xyz/query/g25Jc90pM6" %}
Show me total supply of @BoredApeYachtClub
{% endembed %}

### Demo

{% tabs %}
{% tab title="Query" %}
```graphql
query TotalSupply {
  Tokens(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum}
  ) {
    Token {
      totalSupply
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Tokens": {
      "Token": [
        {
          "totalSupply": "10000"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Name & Symbol

You can fetch an NFT collection's name and symbol by using the `name`  and `symbol` fields from  [`Tokens`](../../api-references/api-reference/tokens-api/) API by providing NFT collection `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/dwqV0IflxF" %}
Show me the name and symbol of @BoredApeYachtClub
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Tokens(
    input: { filter: { address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum}
  ) {
    Token {
      name
      symbol
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Tokens": {
      "Token": [
        {
          "name": "BoredApeYachtClub",
          "symbol": "BAYC"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Metadata of Specific NFT

You can fetch an NFT's metadata (both raw and formatted version) by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by specifying NFT collection `address` and the NFT `tokenId` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/1n8J9jyZJA" %}
Show me the NFT Metadata of @BoredApeYachtClub token ID 0
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}, tokenId: {_eq: "0"}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenId
      rawMetaData
      metaData {
        name
        description
        image
        attributes {
          trait_type
          value
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
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenId": "0",
          "rawMetaData": {
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ"
          },
          "metaData": {
            "name": "",
            "description": "",
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ",
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ]
          }
        }
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Metadata of An NFT Collection

You can fetch all NFTs' metadata (both raw and formatted version) in an NFT collection by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by providing NFT collection `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ciNT0PDEmZ" %}
Show me the NFT Metadata of @BoredApeYachtClub
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum}) {
    TokenNft {
      tokenId
      rawMetaData
      metaData {
        name
        description
        image
        attributes {
          trait_type
          value
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
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenId": "0",
          "rawMetaData": {
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ"
          },
          "metaData": {
            "name": "",
            "description": "",
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ",
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ]
          }
        },
        // other NFTs
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get NFT Images of Specific NFT

You can fetch an NFT's image (both original and resized version) by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by specifying NFT collection `address` and the NFT `tokenId` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lTC6ELEi3D" %}
Show me the the original NFT image of @BoredApeYachtClub token ID 0 with all their resized versions
{% endembed %}

{% hint style="info" %}
Resized NFT images size guide:

* `extra_small`: 125x125px
* `small`: 250x250px
* `medium`: 500x500px
* `large`: 750x750px
{% endhint %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}, tokenId: {_eq: "0"}}, blockchain: ethereum}
  ) {
    TokenNft {
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original # Original size of the NFT image
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
    "TokenNfts": {
      "TokenNft": [
        {
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/extra_small.png",
              "small": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/small.png",
              "medium": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/medium.png",
              "large": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/large.png",
              "original": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/original_image.png"
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

## Get NFT Images of An NFT Collection

You can fetch all NFTs' images (both original and resized version) in an NFT collection by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by providing NFT collection `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/c423sskw2o" %}
Show me the the original NFT image of @BoredApeYachtClub with all their resized versions
{% endembed %}

{% hint style="info" %}
Resized NFT images size guide:

* `extra_small`: 125x125px
* `small`: 250x250px
* `medium`: 500x500px
* `large`: 750x750px
{% endhint %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenId
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original # Original size of the NFT image
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
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenId": "0",
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/extra_small.png",
              "small": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/small.png",
              "medium": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/medium.png",
              "large": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/large.png",
              "original": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/original_image.png"
            }
          }
        },
        // other NFTs
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Specific NFT in An NFT Collection

You can fetch all individual NFTs' details in an NFT collection by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by providing NFT collection `address` and the NFT `tokenId` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/pZZfhEm5Hm" %}
Show me @BoredApeYachtClub token ID 0 and their details
{% endembed %}

{% hint style="info" %}
Resized NFT images size guide:

* `extra_small`: 125x125px
* `small`: 250x250px
* `medium`: 500x500px
* `large`: 750x750px
{% endhint %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}, tokenId: {_eq: "0"}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenURI
      rawMetaData
      metaData {
        animationUrl
        attributes {
          displayType
          maxValue
          trait_type
          value
        }
        backgroundColor
        description
        externalUrl
        image
        imageData
        name
        youtubeUrl
      }
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original # Original size of the NFT image
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
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenURI": "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/0",
          "rawMetaData": {
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ"
          },
          "metaData": {
            "animationUrl": "",
            "attributes": [
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "backgroundColor": "",
            "description": "",
            "externalUrl": "",
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ",
            "imageData": "",
            "name": "",
            "youtubeUrl": ""
          },
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/extra_small.png",
              "small": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/small.png",
              "medium": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/medium.png",
              "large": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/large.png",
              "original": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/original_image.png"
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

## Get All NFT In An NFT Collection

You can fetch all individual NFTs' details in an NFT collection by using the  [`TokenNfts`](../../api-references/api-reference/tokennfts-api/) API by providing NFT collection `address` as an input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/aYwwg56aGU" %}
Show me @BoredApeYachtClub and their details
{% endembed %}

{% hint style="info" %}
Resized NFT images size guide:

* `extra_small`: 125x125px
* `small`: 250x250px
* `medium`: 500x500px
* `large`: 750x750px
{% endhint %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenNfts(
    input: {filter: {address: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum}
  ) {
    TokenNft {
      tokenId
      tokenURI
      rawMetaData
      metaData {
        animationUrl
        attributes {
          displayType
          maxValue
          trait_type
          value
        }
        backgroundColor
        description
        externalUrl
        image
        imageData
        name
        youtubeUrl
      }
      contentValue {
        image {
          extraSmall
          small
          medium
          large
          original # Original size of the NFT image
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
    "TokenNfts": {
      "TokenNft": [
        {
          "tokenId": "0",
          "tokenURI": "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/0",
          "rawMetaData": {
            "attributes": [
              {
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ"
          },
          "metaData": {
            "animationUrl": "",
            "attributes": [
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Earring",
                "value": "Silver Hoop"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Background",
                "value": "Orange"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Fur",
                "value": "Robot"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Clothes",
                "value": "Striped Tee"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Mouth",
                "value": "Discomfort"
              },
              {
                "displayType": null,
                "maxValue": null,
                "trait_type": "Eyes",
                "value": "X Eyes"
              }
            ],
            "backgroundColor": "",
            "description": "",
            "externalUrl": "",
            "image": "ipfs://QmRRPWG96cmgTn2qSzjwr2qvfNEuhunv6FNeMFGa9bx6mQ",
            "imageData": "",
            "name": "",
            "youtubeUrl": ""
          },
          "contentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/extra_small.png",
              "small": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/small.png",
              "medium": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/medium.png",
              "large": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/large.png",
              "original": "https://assets.airstack.xyz/image/nft/D+0AQrSsYYaYPkhHjNpMvNeST+YQ2SI7M2u7bB7tjmI6xynU3spMwkuRVDM/9nYZ/original_image.png"
            }
          }
        },
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweGJjNGNhMGVkYTc2NDdhOGFiN2MyMDYxYzJlMTE4YTE4YTkzNmYxM2QxMDQxIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching NFT details data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [NFT Balances](nft-balances.md)
* [NFT Holders](nft-holders.md)
* [Combinations (Common Holders)](../combinations/)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [NFTs](../tokens-in-common/nfts.md)
* [Tokens API Reference](../../api-references/api-reference/tokens-api/)
* [TokenNfts API Reference](../../api-references/api-reference/tokennfts-api/)
