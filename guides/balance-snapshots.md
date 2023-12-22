---
description: >-
  Learn how to use the Airstack API fetch token balances time-series data of
  user(s) and their variations on Base.
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

# 📸 Balance Snapshots

[Airstack](https://airstack.xyz) provides easy-to-use [Snapshots API](../api-references/api-reference/snapshots-api.md)s for fetching token balances at a specific point in time. This feature set is currently available for the Ethereum and Base blockchain and will be available soon for Polygon.&#x20;

The [Snapshots API](../api-references/api-reference/snapshots-api.md) use timestamp, date, or block number as an input to specify the time:

| Name          | Type   | Description                                                                                     |
| ------------- | ------ | ----------------------------------------------------------------------------------------------- |
| `blockNumber` | `Int`  | Allows filtering based on specific block number (integer), 14562584                             |
| `date`        | `Date` | Allows filtering based on specific date with YYYY-MM-DD format, e.g. 2023-10-25                 |
| `timestamp`   | `Int`  | Epoch Unix timestamp to fetch the specified time snapshots of balances/holders, e.g. 1702559139 |

and returns a list of tokens held by a user at the speficied timestamp, date, or block number.

Each object returned will be followed by a range of block number (`startBlockNumber` and `endBlockNumber`) and timestamp (`startBlockTimestamp` and `endBlockTimestamp`) that shows when was the token was first and last held:

| Name                  | Type   | Description                                                                                             |
| --------------------- | ------ | ------------------------------------------------------------------------------------------------------- |
| `endBlockNumber`      | `Int`  | Block number when the token was **last** held. If still hold to present, it will return `-1`.           |
| `endBlockTimestamp`   | `Time` | Timestamp when the token was **last** held. If still hold to present, it will return present timestamp. |
| `startBlockNumber`    | `Int`  | Block number when the token was **first** held by owner.                                                |
| `startBlockTimestamp` | `Time` | Timestamp when the token was **first** held.                                                            |

It is important to keep in mind that the `startBlockNumber` and `startBlockTimestamp` can have a different value to the input, but will always have time value less than or equal to the specified time.

For more details, check out the [Snapshots API references](../api-references/api-reference/snapshots-api.md).

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get User's Token Balances Of Specified Time on Ethereum](balance-snapshots.md#get-users-token-balances-of-specified-time-on-base) (`date`, `timestamp` or `blockNumber`)
* [Get User's Token Balances at a Specified Time on Base](balance-snapshots.md#get-users-token-balances-of-specified-time-on-base) (`date`, `timestamp` or `blockNumber`)
* [Get User's Token Balances Of Specified Time on Ethereum and Base](balance-snapshots.md#get-users-token-balances-of-specified-time-on-ethereum-and-base) (`date`, `timestamp` or `blockNumber`)
* [Get User's Historical Balance Of Specified ERC20 Token](balance-snapshots.md#get-users-historical-balances-of-specified-erc20-token)
* [Get User's Historical Balance Of Specified NFT Collection](balance-snapshots.md#get-users-historical-balance-of-specified-nft-collection)
* [Get User's Historical Balance Of Specified NFT](balance-snapshots.md#get-users-historical-balance-of-specified-nft)
* [Get User's Historical Balance Of Certain Token Type(s)](balance-snapshots.md#get-users-historical-balance-of-certain-token-type-s)

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

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get User's Token Balances Of Specified Time on Ethereum

You can fetch all the tokens ever held by user at a specified time on Ethereum by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and time input to either `date`, `timestamp` or `blockNumber`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/6mXI6CHjL8" %}
Show me balance snapshots of users on Ethereum on Aug 18, 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>      },
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "tokenAddress": "0xa1f92f70dce96c7cd32aafd93cd4bff7debdf853",
          "tokenId": "162",
          "tokenType": "ERC721",
          "token": {
            "name": "Cosmic Corpse Society",
            "symbol": "CCS",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": null
            }
          },
          "startBlockNumber": 15537991,
          "startBlockTimestamp": "2022-09-15T08:44:23Z",
<strong>          "endBlockNumber": -1, // -1 indicate asset is no longer hold at present date
</strong>          "endBlockTimestamp": "2023-12-22T12:04:57Z"
        },
        // Other Ethereum tokens held specifically on Aug 18, 2023
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjBjMWVlOTZmYjkyMzc1OWM3MGRlMWQ2ZmRlZGFiZTE5IiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get User's Token Balances Of Specified Time on Base

You can fetch all the tokens ever held by user at a specified time on Base by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and time input to either `date`, `timestamp` or `blockNumber`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4gYLbXKAgh" %}
Show me balance snapshots of users on Base on Aug 18, 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>      },
      blockchain: base,
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "tokenAddress": "0x6e30433b8c65e76fa095e85260a244b3c3bc1865",
          "tokenId": "273",
          "tokenType": "ERC721",
          "token": {
            "name": "Base Echo",
            "symbol": "$",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/large.png",
                "medium": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/medium.png",
                "original": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/original_image.png",
                "small": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/small.png"
              }
            }
          },
          "startBlockNumber": 2790948,
          "startBlockTimestamp": "2023-08-18T15:07:23Z",
<strong>          "endBlockNumber": -1, // -1 indicate asset is no longer hold at present date
</strong>          "endBlockTimestamp": "2023-12-06T20:35:37Z"
        },
        // Other Base tokens held specifically on Aug 18, 2023
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjRkZGUxMzhhNGExNTVhYmRjMWMzYTkwMTM0MmM4NGViIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get User's Token Balances Of Specified Time on Ethereum and Base

You can fetch all the tokens ever held by user at a specified time across multiple chains, e.g. Ethereum and Base, by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and time input to either `date`, `timestamp` or `blockNumber`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Et9sxm16nq" %}
Show me balance snapshots of users on Base on Aug 18, 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Ethereum: Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>      },
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
  Base: Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
<strong>        date: {_eq: "2023-08-18"} # Specifying date to Aug 18, 2023
</strong>      },
      blockchain: base,
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "Snapshot": [
        {
          "tokenAddress": "0xa1f92f70dce96c7cd32aafd93cd4bff7debdf853",
          "tokenId": "162",
          "tokenType": "ERC721",
          "token": {
            "name": "Cosmic Corpse Society",
            "symbol": "CCS",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": null
            }
          },
          "startBlockNumber": 15537991,
          "startBlockTimestamp": "2022-09-15T08:44:23Z",
<strong>          "endBlockNumber": -1, // -1 indicate asset is no longer hold at present date
</strong>          "endBlockTimestamp": "2023-12-22T12:04:57Z"
        },
        // Other Ethereum tokens held specifically on Aug 18, 2023
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjBjMWVlOTZmYjkyMzc1OWM3MGRlMWQ2ZmRlZGFiZTE5IiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    },
    "Base": {
      "Snapshot": [
        {
          "tokenAddress": "0x6e30433b8c65e76fa095e85260a244b3c3bc1865",
          "tokenId": "273",
          "tokenType": "ERC721",
          "token": {
            "name": "Base Echo",
            "symbol": "$",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/large.png",
                "medium": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/medium.png",
                "original": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/original_image.png",
                "small": "https://assets.airstack.xyz/image/nft/8453/8ptb4/bkF79jKGc/p1otcqwQSv+qMZ6PvIpudahqBysb50fH7Q/xnptwoac3JTkzk0pHtn5XHPWr/LWAZCp5eA==/small.png"
              }
            }
          },
          "startBlockNumber": 2790948,
          "startBlockTimestamp": "2023-08-18T15:07:23Z",
<strong>          "endBlockNumber": -1, // -1 indicate asset is no longer hold at present date
</strong>          "endBlockTimestamp": "2023-12-06T20:35:37Z"
        },
        // Other Base tokens held specifically on Aug 18, 2023
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjRkZGUxMzhhNGExNTVhYmRjMWMzYTkwMTM0MmM4NGViIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get User's Historical Balances Of Specified ERC20 Token

You can fetch user's historical token balances of a specified ERC20 token and their time series data by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and specifying `tokenAddress` with the ERC20 token address:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OlVll6ze2e" %}
Show me users' historical balances of ERC20 token @USD Coin on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
        # Optional – specify type to ERC20
<strong>        tokenType: {_in: [ERC20]},
</strong>        # specify ERC20 token addresss
<strong>        tokenAddress: {_eq: "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913"}
</strong>      },
      blockchain: base,
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "tokenAddress": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
          "tokenType": "ERC20",
          "formattedAmount": 1.53,
          "token": {
            "name": "USD Coin",
            "symbol": "USDC",
            "isSpam": false
          },
<strong>          "startBlockNumber": 6466579, // the blocknumber when this use start to have 1.53 USD
</strong>          "startBlockTimestamp": "2023-11-11T17:08:25Z",
<strong>          "endBlockNumber": 6711733, // the blocknumber when this user have USDC balance changes
</strong>          "endBlockTimestamp": "2023-11-17T09:20:13Z"
        },
        {
          "tokenAddress": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
          "tokenType": "ERC20",
          "formattedAmount": 1.1,
          "token": {
            "name": "USD Coin",
            "symbol": "USDC",
            "isSpam": false
          },
          "startBlockNumber": 6291593,
          "startBlockTimestamp": "2023-11-07T15:55:33Z",
          "endBlockNumber": 6421677,
          "endBlockTimestamp": "2023-11-10T16:11:41Z"
        },
        {
          "tokenAddress": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
          "tokenType": "ERC20",
          "formattedAmount": 1.11,
          "token": {
            "name": "USD Coin",
            "symbol": "USDC",
            "isSpam": false
          },
          "startBlockNumber": 6421677,
          "startBlockTimestamp": "2023-11-10T16:11:41Z",
          "endBlockNumber": 6466579,
          "endBlockTimestamp": "2023-11-11T17:08:25Z"
        },
        // Time series data of USD Coin holdings
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjRkZGUxMzhhNGExNTVhYmRjMWMzYTkwMTM0MmM4NGViIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get User's Historical Balance Of Specified NFT Collection

You can fetch user's historical token balances of a specified NFT collection and their time series data by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and specifying `tokenAddress` with the ERC721/1155 NFT collection address:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4UPeT8F2h0" %}
Show me users' historical balances of NFT @Taiko x Base on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
        # Optional – specify type to only ERC721/1155
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>        # Specify NFT contract address
<strong>        tokenAddress: {_eq: "0x344bd884B47dfc988F7e47851d576E0AC083A16F"}
</strong>      },
    blockchain: base,
    limit: 200
  }
  ) {
    Snapshot {
      tokenAddress
      tokenId
      tokenType
      formattedAmount
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "tokenAddress": "0x344bd884b47dfc988f7e47851d576e0ac083a16f",
          "tokenId": "1001",
          "tokenType": "ERC721",
          "formattedAmount": 1,
          "token": {
            "name": "Taiko x Base",
            "symbol": "TAI",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": null
            }
          },
          "startBlockNumber": 4903658,
          "startBlockTimestamp": "2023-10-06T12:51:03Z",
          "endBlockNumber": -1,
          "endBlockTimestamp": "2023-12-07T03:57:47Z"
        }
      ],
      "pageInfo": {
        "hasNextPage": false,
        "hasPrevPage": false,
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get User's Historical Balance Of Specified NFT

You can fetch user's historical token balances of a specified ERC721/1155 NFT and their time series data by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and specifying `tokenAddress` with the ERC721 NFT token address and `tokenId` with the corresponding token ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/9itdEZxLBo" %}
Show me users' historical balances of NFT @Taiko x Base token ID 1001 on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
        # Optional – specify type ERC721/1155
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>        # specify NFT token address &#x26; ID
<strong>        tokenAddress: {_eq: "0x344bd884B47dfc988F7e47851d576E0AC083A16F"}
</strong><strong>        tokenId: {_eq: "1001"}
</strong>      },
    blockchain: base,
    limit: 200
  }
  ) {
    Snapshot {
      tokenAddress
      tokenId
      tokenType
      formattedAmount
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
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```graphql
{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "tokenAddress": "0x344bd884b47dfc988f7e47851d576e0ac083a16f",
          "tokenId": "1001",
          "tokenType": "ERC721",
          "formattedAmount": 1,
          "token": {
            "name": "Taiko x Base",
            "symbol": "TAI",
            "isSpam": false
          },
          "tokenNft": {
            "contentValue": {
              "image": null
            }
          },
          "startBlockNumber": 4903658,
          "startBlockTimestamp": "2023-10-06T12:51:03Z",
          "endBlockNumber": -1,
          "endBlockTimestamp": "2023-12-07T03:57:47Z"
        }
      ],
      "pageInfo": {
        "hasNextPage": false,
        "hasPrevPage": false,
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get User's Historical Balance Of Certain Token Type(s)

You can fetch user's historical token balances of specified token type(s), that is ERC20/721/1155, and their time series data by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing user(s) 0x address, ENS, cb.id, Lens, or Farcaster to `owner` input and specifying `tokenType` with the token type (ERC20/721/1155):

### Try Demo

{% embed url="https://app.airstack.xyz/query/jcYvpL20dS" %}
Show me users' historical ERC20 token balances
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Snapshots(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "vitalik.eth",
            "lens/@vitalik",
            "fc_fname:vitalik"
          ]
        },
        tokenType: {_in: [ERC20]}
      },
      blockchain: base,
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
    pageInfo {
      hasNextPage
      hasPrevPage
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
    "Snapshots": {
      "Snapshot": [
        {
          "tokenAddress": "0x52c45d3068c937cb1e6b4a7f2c2a66b85056dd24",
          "tokenId": null,
          "tokenType": "ERC20",
          "token": {
            "name": "SHARD",
            "symbol": "SHARD",
            "isSpam": false
          },
          "tokenNft": null,
          "startBlockNumber": 2652268,
          "startBlockTimestamp": "2023-08-15T10:04:43Z",
          "endBlockNumber": 2706080,
          "endBlockTimestamp": "2023-08-16T15:58:27Z"
        },
        // Other historical ERC20s token balances ever held
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjRkZGUxMzhhNGExNTVhYmRjMWMzYTkwMTM0MmM4NGViIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching balance snapshots data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Holder Snapshots Guides](holder-snapshots.md)
* [Token Balances Guides](token-balances/)
* [Token Holders Guides](token-holders/)
* [Combinations Guides](combinations/)
* [Tokens In Common Guides](tokens-in-common/)
* [Snapshots API](../api-references/api-reference/snapshots-api.md)
