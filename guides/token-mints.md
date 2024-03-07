---
description: >-
  Learn how to use Airstack to fetch all ERC20/721/1155 token mints data, from
  or to user(s), across Ethereum, Polygon, Base, and Zora.
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

# 👛 Token Mints

All tokens minted are essentially token transfers from a [null address (0x00...00)](https://explorer.airstack.xyz/token-balances?address=0x0000000000000000000000000000000000000000&rawInput=%23%E2%8E%B10x0000000000000000000000000000000000000000%E2%8E%B1%280x0000000000000000000000000000000000000000+ADDRESS+ethereum+null%29&inputType=ADDRESS) to a user address that are executed by the receiving user itself. Thus, with [Airstack](https://airstack.xyz), you can use the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API to fetch all user's token mints by specifying the input as follows:

<table><thead><tr><th width="176">Input Filter</th><th>Value</th><th>Description</th></tr></thead><tbody><tr><td><code>operator</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Executor of the transaction.</td></tr><tr><td><code>from</code></td><td>0x0000000000000000000000000000000000000000</td><td>Sender in the ERC20/721/1155 token transfers.</td></tr><tr><td><code>to</code></td><td>user's 0x address, ENS, cb.id, Lens, or Farcaster</td><td>Receiver in the ERC20/721/1155 token transfers.</td></tr></tbody></table>

You also have the option to add `blockTimestamp` filter to your query to fetch all tokens that are minted during a specified period of time:

| Input Filter     | Value                                                                                                                                                                                                                                                                                                                                                                                                  | Description                                                                                                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `blockTimestamp` | <p>Specific time period to fetch token mints, use <code>\_gt</code> or <code>\_gte</code> and <code>\_lt</code> or <code>\_lte</code> filters to specify the timestamp period.<br><br>Look at example below for <a href="token-mints.md#get-nft-mints-by-a-user-in-a-specified-period">NFT</a> and <a href="token-mints.md#get-erc20-token-mints-by-a-user-in-a-specified-period">ERC20 </a>mints.</p> | <p>Allows entering blockTimestamp to filter transactions which happened in specific periods.<br><br>Time format should be following the unix timstamp format, e.g. 2023-01-01T00:00:00Z</p> |

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get NFT Mints By A User](token-mints.md#get-nft-mints-by-a-user)
- [Get ERC20 Token Mints By A User](token-mints.md#get-erc20-token-mints-by-a-user)
- [Get NFT Mints By A User in a Specified Period](token-mints.md#get-nft-mints-by-a-user)
- [Get ERC20 Token Mints By A User in a Specified Period](token-mints.md#get-erc20-token-mints-by-a-user-in-a-specified-period)
- [Get Current Balance of Minted NFTs](token-mints.md#get-current-balance-of-minted-nfts)
- [Get Current Balance of Minted ERC20 Tokens](token-mints.md#get-current-balance-of-minted-erc20-tokens)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

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

## Get NFT Mints By A User

You can fetch all NFTs minted by a user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/HiNkAcW8C7" %}
Show me all NFTs minted by betashop.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: ethereum,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: polygon,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: base,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
  Zora: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Get only NFTs (ERC721/1155)
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>      },
      blockchain: zora,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      tokenId
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "blockchain": "ethereum",
          "formattedAmount": 1,
          "tokenAddress": "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e",
          "tokenId": "8",
          "tokenNft": {
            "metaData": {
              "name": "Venture Club Alpha Membership NFT"
            },
            "contentValue": {
              "image": {
                "medium": "https://assets.uat.airstack.xyz/image/nft/li5d4XIGDPxtahI+EMjNmOiSymdFmCR0OsRC2p13nDaZQijmEYVpYlbV0t57GD8K/medium.jpg"
              }
            }
          },
          "tokenType": "ERC721"
        }
        // Other NFTs minted by betashop.eth on Ethereum
      ]
    },
    "Polygon": {
      "TokenTransfer": [
        {
          "blockchain": "polygon",
          "formattedAmount": 1,
          "tokenAddress": "0x0ff0095b74eca3644e62f91dbe952faf677efc12",
          "tokenId": "26723",
          "tokenNft": {
            "metaData": {
              "name": "ctrlaltf.lens's follower NFT"
            },
            "contentValue": {
              "image": {
                "medium": "https://assets.uat.airstack.xyz/image/nft/Ycq8Xr2vFpq0mTH7PnGk+oX4ZZDmBg9VWLv5LariQ3qQGuLBLG98xy7DN6D1F7lnfaKEpaNPsLerS7eamSbl6w==/medium.svg"
              }
            }
          },
          "tokenType": "ERC721"
        }
        // Other NFTs minted by betashop.eth on Polygon
      ]
    },
    "Base": {
      "TokenTransfer": [
        {
          "blockchain": "base",
          "formattedAmount": 1,
          "tokenAddress": "0xba5e05cb26b78eda3a2f8e3b3814726305dcac83",
          "tokenId": "118",
          "tokenNft": {
            "metaData": {
              "name": null
            },
            "contentValue": {
              "image": null
            }
          },
          "tokenType": "ERC1155"
        }
      ]
    },
    "Zora": {
<strong>      "TokenTransfer": null // No NFT mints by betashop.eth on Zora
</strong>    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Get ERC20 Token Mints By A User

You can fetch all ERC20 tokens minted by a user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/RsCkWHyHjV" %}
Show me all ERC20 tokens minted by ipeciura.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20},
</strong>      },
      blockchain: ethereum,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      token {
        name
      }
    }
  }
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "betashop.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20},
</strong>      },
      blockchain: polygon,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      token {
        name
      }
    }
  }
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20},
</strong>      },
      blockchain: base,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      token {
        name
      }
    }
  }
  Zora: TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20},
</strong>      },
      blockchain: zora,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      tokenAddress
      token {
        name
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
<strong>      "TokenTransfer": null // No ERC20 token minted on Ethereum
</strong>    },
    "Polygon": {
      "TokenTransfer": [
        {
          "blockchain": "polygon",
          "formattedAmount": 0.01697896348647009,
          "tokenAddress": "0xadbf1854e5883eb8aa7baf50705338739e558e5b",
          "token": {
            "name": "Uniswap V2"
          }
        },
        // Other ERC20 tokens minted by ipeciura.eth on Polygon
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No ERC20 token minted on Base
</strong>    },
    "Zora": {
<strong>      "TokenTransfer": null // No ERC20 token minted on Zora
</strong>    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Get NFT Mints By A User in a Specified Period

You can fetch all NFTs minted during a specified period of time by a user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across different chains, e.g. Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/1NeCdgZKG8" %}
Show me ERC721/1155 Polygon NFT mints by ipeciura.eth in 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC721/1155 NFTs
<strong>        tokenType: {_in: [ERC721, ERC1155]},
</strong>        # Specify time stamp (in 2023)
<strong>        blockTimestamp: {
</strong>          _gt: "2023-01-01T00:00:00Z", # (greater than - after Jan 1, 2023)
          _lt: "2024-01-01T00:00:00Z" # (less than - before Jan 1, 2024)
        }
      },
      blockchain: polygon,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      blockTimestamp
      tokenAddress
      tokenNft {
        metaData {
          name
        }
        contentValue {
          image {
            medium
          }
        }
      }
      tokenType
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "blockchain": "polygon",
          "formattedAmount": 1,
          "blockTimestamp": "2023-11-09T13:26:53Z",
          "tokenAddress": "0xd3b4de0d85c44c57993b3b18d42b00de81809eea",
          "tokenNft": {
            "metaData": {
              "name": "Unveiling Airstack's Onchain Graph #1"
            },
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/N3xsCM4U2UsyVojt4p4OUOsbwHHkyl1MPU9iEHe4mr0FqR+Vy4cMTaTFc03m2rVApXUy38ASkynABWL/M3lLaQ==/medium.jpg"
              }
            }
          },
          "tokenType": "ERC721"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get ERC20 Token Mints By A User in a Specified Period

You can fetch all ERC20 tokens minted during a specified period of time by a user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across different chains, e.g. Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/kQk6CRK9aT" %}
Show me all Polygon ERC20 token mints by ipeciura.eth in 2022
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20}
</strong>        # Specify time stamp (in 2022)
<strong>        blockTimestamp: {
</strong>          _gt: "2022-01-01T00:00:00Z", # (greater than - before Jan 1, 2022)
          _lt: "2023-01-01T00:00:00Z" # (less than - before Jan 1, 2023)
        }
      },
      blockchain: polygon,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      blockchain
      formattedAmount
      blockTimestamp
      tokenAddress
      token {
        name
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "blockchain": "polygon",
          "formattedAmount": 0.01697896348647009,
          "blockTimestamp": "2022-07-15T15:37:02Z",
          "tokenAddress": "0xadbf1854e5883eb8aa7baf50705338739e558e5b",
          "token": {
            "name": "Uniswap V2"
          }
        },
        {
          "blockchain": "polygon",
          "formattedAmount": 0.9908239030713007,
          "blockTimestamp": "2022-07-15T14:45:54Z",
          "tokenAddress": "0x9928340f9e1aaad7df1d95e27bd9a5c715202a56",
          "token": {
            "name": "Balancer B-stMATIC-STABLE RewardGauge Deposit"
          }
        }
        // Other ERC20 tokens minted by ipeciura.eth on Polygon in 2022
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Current Balance of Minted NFTs

### Fetching

You can fetch the current NFT balances of minted NFTs, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across different chains, e.g. Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/4dJEQDYR1K" %}
Show current NFT balances of Minted NFTs on Polygon by ipeciura.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC721/1155 NFTs
<strong>        tokenType: {_in: [ERC1155, ERC721]}
</strong>      },
      order: {blockTimestamp: DESC},
      blockchain: polygon
    }
  ) {
    TokenTransfer {
      blockTimestamp
      type
      transactionHash
      tokenNft {
        contentValue {
          image {
            medium
          }
        }
      }
      token {
        name
        address
        type
        tokenBalances(input: {filter: {owner: {_eq: "ipeciura.eth"}}}) {
          formattedAmount
        }
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "blockTimestamp": "2023-11-09T13:26:53Z",
          "type": "MINT",
          "transactionHash": "0x633ce3c1c2050b2477d9b8e3ee4a8de41bfb15a2b8adc3f59cc7ba002a782b2f",
          "tokenNft": {
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/N3xsCM4U2UsyVojt4p4OUOsbwHHkyl1MPU9iEHe4mr0FqR+Vy4cMTaTFc03m2rVApXUy38ASkynABWL/M3lLaQ==/medium.jpg"
              }
            }
          },
          "token": {
            "name": "Unveiling Airstack's Onchain Graph",
            "address": "0xd3b4de0d85c44c57993b3b18d42b00de81809eea",
            "type": "ERC721",
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1 // Shows that ipeciura.eth current balance for this NFT
</strong>              }
            ]
          }
        },
        // Other minted NFTs by ipeciura.eth on Polygon
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

### Formatting

To get the list of minted NFTs and their current balance in a flat array, use the following format function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
interface Token {
  name: string;
  type: string;
  address: string;
  tokenBalances: { formattedAmount?: string }[];
}

interface TokenTransfer {
  token: Token;
}

interface Data {
  TokenTransfers?: {
    TokenTransfer: TokenTransfer[];
  };
}

/**
 * @description Formats the NFT mints data.
 *
 * @example
 * import { fetchQuery } from "@airstack/node";
 *
 * const { data } = await fetchQuery(query, variables);
 * const res = formatFunction(data);
 *
 * @param {Data} data - The data result from NFT mints query.
 * @returns {Object[]} An array of formatted minted NFTs' current balance
 */
const formatFunction = (data: Data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ token }) => {
    const { name, type, address, tokenBalances } = token;
    const formattedAmount = tokenBalances[0]?.formattedAmount ?? 0;
    return { name, type, address, formattedAmount };
  })?.filter(
    (arr, index, self) =>
      index === self.findIndex((t) => t.address === arr.address)
  );
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
/**
 * @description Formats the NFT mints data.
 *
 * @example
 * import { fetchQuery } from "@airstack/node";
 *
 * const { data } = await fetchQuery(query, variables);
 * const res = formatFunction(data);
 *
 * @param {Data} data - The data result from NFT mints query.
 * @returns {Object[]} An array of formatted minted NFTs' current balance
 */
const formatFunction = (data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ token }) => {
    const { name, type, address, tokenBalances } = token;
    const formattedAmount = tokenBalances[0]?.formattedAmount ?? 0;
    return { name, type, address, formattedAmount };
  })?.filter(
    (arr, index, self) =>
      index === self.findIndex((t) => t.address === arr.address)
  );
```

{% endtab %}

{% tab title="Python" %}

```python
from typing import List, Dict, Optional, Any

class Token:
    def __init__(self, name: str, type: str, address: str, tokenBalances: List[Dict[str, Any]]):
        self.name = name
        self.type = type
        self.address = address
        self.tokenBalances = tokenBalances

class TokenTransfer:
    def __init__(self, token: Token):
        self.token = token

class Data:
    def __init__(self, TokenTransfers: Optional[Dict[str, List[TokenTransfer]]]):
        self.TokenTransfers = TokenTransfers

def format_function(data: Optional[Dict[str, Any]]) -> List[Dict[str, Any]]:
    """
    Format and flatten NFT mint data to show all minted NFTs
    and their current balance hold by a user
    """
    if not data or "TokenTransfers" not in data or not data["TokenTransfers"]:
        return []

    token_transfers = data["TokenTransfers"].get("TokenTransfer")
    if not token_transfers:
        return []

    unique = {}
    formatted_data = []

    for transfer in token_transfers:
        token = transfer.get("token", {})

        token_balances = token.get("tokenBalances", [])
        if token_balances:
            formatted_amount = token_balances[0].get("formattedAmount", 0)
        else:
            formatted_amount = 0

        if token.get("address") and token.get("address") not in unique:
            unique[token.get("address")] = True
            formatted_data.append({
                "name": token.get("name"),
                "type": token.get("type"),
                "address": token.get("address"),
                "formattedAmount": formatted_amount
            })

    return formatted_data
```

{% endtab %}
{% endtabs %}

With this format function, you will get the following result that you can directly add to your application:

```json
[
  {
    "name": "Unveiling Airstack's Onchain Graph",
    "type": "ERC721",
    "address": "0xd3b4de0d85c44c57993b3b18d42b00de81809eea",
    "formattedAmount": 1
  },
  {
    "name": "MIAMI ESPORTS",
    "type": "ERC721",
    "address": '0x15b120343928019be964a353cf4f7dae474fb579",
    "formattedAmount": 1
  }
]
```

## Get Current Balance of Minted ERC20 Tokens

### Fetching

You can fetch the current ERC20 token balances of minted ERC20 tokens that are still hold by a user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29&inputType=ADDRESS), across different chains, e.g. Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/prb5moxfQP" %}
Show current ERC20 token balances of Minted ERC20 tokens on Polygon by ipeciura.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenTransfers(
    input: {
      filter: {
        # Only get mints that are executed by the same user
<strong>        operator: {_eq: "ipeciura.eth"},
</strong>        # Mints are token transfers that has null address as `from`
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        # Set this to the user that receive the token mints
<strong>        to: {_eq: "ipeciura.eth"},
</strong>        # Get only ERC20 tokens
<strong>        tokenType: {_eq: ERC20}
</strong>      },
      order: {blockTimestamp: DESC},
      blockchain: polygon}
  ) {
    TokenTransfer {
      blockTimestamp
      type
      transactionHash
      token {
        name
        address
        tokenBalances(input: {filter: {owner: {_eq: "ipeciura.eth"}}}) {
          formattedAmount
        }
      }
    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "blockTimestamp": "2022-07-15T14:45:54Z",
          "type": "MINT",
          "transactionHash": "0x074efa071dd2be61286f411c091cf157246ccbe7fd6b24f6a8af86bcf52a0c98",
          "token": {
            "name": "Balancer B-stMATIC-STABLE RewardGauge Deposit",
            "address": "0x9928340f9e1aaad7df1d95e27bd9a5c715202a56",
            "tokenBalances": [
              {
<strong>                "formattedAmount": 0.9908239030713007 // ipeciura.eth currently hold this ERC20 token
</strong>              }
            ]
          }
        },
        {
          "blockTimestamp": "2022-07-15T15:37:02Z",
          "type": "MINT",
          "transactionHash": "0x985ff4181c7fec054fcf5eb20d841f4372745f80cf51a112444337166e7417f9",
          "token": {
            "name": "Uniswap V2",
            "address": "0xadbf1854e5883eb8aa7baf50705338739e558e5b",
<strong>            "tokenBalances": [] // Empty array indicates that the minted ERC20 token have 0 balance (no longer held)
</strong>          }
        },
        // Other ERC20 tokens minted by ipeciura.eth on Polygon
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

### Formatting

To get the list of minted ERC20 tokens and their current balance in a flat array, use the following format function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
interface Token {
  name: string;
  address: string;
  tokenBalances: { formattedAmount?: string }[];
}

interface TokenTransfer {
  token: Token;
}

interface Data {
  TokenTransfers?: {
    TokenTransfer: TokenTransfer[];
  };
}

/**
 * @description Formats the ERC20 token mints data.
 *
 * @example
 * import { fetchQuery } from "@airstack/node";
 *
 * const { data } = await fetchQuery(query, variables);
 * const res = formatFunction(data);
 *
 * @param {Data} data - The data result from ERC20 token mints query.
 * @returns {Object[]} An array of formatted minted ERC20 tokens' current balance
 */
const formatFunction = (data: Data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ token }) => {
    const { name, address, tokenBalances } = token;
    const formattedAmount = tokenBalances[0]?.formattedAmount ?? 0;
    return { name, address, formattedAmount };
  })?.filter(
    (arr, index, self) =>
      index === self.findIndex((t) => t.address === arr.address)
  );
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
/**
 * @description Formats the ERC20 token mints data.
 *
 * @example
 * import { fetchQuery } from "@airstack/node";
 *
 * const { data } = await fetchQuery(query, variables);
 * const res = formatFunction(data);
 *
 * @param {Data} data - The data result from ERC20 token mints query.
 * @returns {Object[]} An array of formatted minted ERC20 tokens' current balance
 */
const formatFunction = (data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ token }) => {
    const { name, address, tokenBalances } = token;
    const formattedAmount = tokenBalances[0]?.formattedAmount ?? 0;
    return { name, address, formattedAmount };
  })?.filter(
    (arr, index, self) =>
      index === self.findIndex((t) => t.address === arr.address)
  );
```

{% endtab %}

{% tab title="Python" %}

```python
from typing import List, Dict, Optional, Any

class Token:
    def __init__(self, name: str, address: str, tokenBalances: List[Dict[str, Any]]):
        self.name = name
        self.address = address
        self.tokenBalances = tokenBalances

class TokenTransfer:
    def __init__(self, token: Token):
        self.token = token

class Data:
    def __init__(self, TokenTransfers: Optional[Dict[str, List[TokenTransfer]]]):
        self.TokenTransfers = TokenTransfers

def format_function(data: Optional[Dict[str, Any]]) -> List[Dict[str, Any]]:
    """
    Format and flatten ERC20 token mint data to show all minted ERC20 tokens
    and their current balance hold by a user
    """
    if not data or "TokenTransfers" not in data or not data["TokenTransfers"]:
        return []

    token_transfers = data["TokenTransfers"].get("TokenTransfer")
    if not token_transfers:
        return []

    unique = {}
    formatted_data = []

    for transfer in token_transfers:
        token = transfer.get("token", {})

        token_balances = token.get("tokenBalances", [])
        if token_balances:
            formatted_amount = token_balances[0].get("formattedAmount", 0)
        else:
            formatted_amount = 0

        if token.get("address") and token.get("address") not in unique:
            unique[token.get("address")] = True
            formatted_data.append({
                "name": token.get("name"),
                "address": token.get("address"),
                "formattedAmount": formatted_amount
            })

    return formatted_data
```

{% endtab %}
{% endtabs %}

With this format function, you will get the following result that you can directly add to your application:

```json
[
  {
    "name": "Balancer B-stMATIC-STABLE RewardGauge Deposit",
    "address": "0x9928340f9e1aaad7df1d95e27bd9a5c715202a56",
    "formattedAmount": 0.9908239030713007
  },
  {
    "name": "Uniswap V2",
    "address": "0xadbf1854e5883eb8aa7baf50705338739e558e5b",
    "formattedAmount": 0
  }
  // Other minted ERC20 tokens
]
```

## Developer Support

If you have any questions or need help regarding fetching token mints data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [TokenTransfers API Reference](../api-references/api-reference/tokentransfers-api.md)
- [On-Chain Graph](onchain-graph.md)
- [TokenTransfers Guides](token-transfers.md)
