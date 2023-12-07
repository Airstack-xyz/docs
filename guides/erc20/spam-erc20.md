---
description: >-
  Learn how to use Airstack to filter out spam ERC20s and check whether an ERC20
  token is a spam or not.
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

# 🧹 Spam ERC20

[Airstack](https://airstack.xyz) provides easy-to-use ERC20 token APIs for enriching Web3 applications with onchain and offchain ERC20 token data from Ethereum, Polygon, and Base.

When indexing any new token, Airstack takes into consideration contract deployer address history, and other factors to determine if the token might be spam.

Those contracts are the marked spam and can be filtered out. If you think that a given contract was incorrectly labeled as spam, please reach out to us [support@airstack.xyz](mailto:support@airstack.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Non-Spam ERC20 Token Balances Of Users](spam-erc20.md#get-non-spam-erc20-token-balances-of-user-s)
- [Check If ERC20 Token(s) Are Spam Or Not](spam-erc20.md#check-if-erc20-token-s-are-spam-or-not)
- [Show All Non-Spam ERC20 Tokens](spam-erc20.md#show-all-non-spam-erc20-tokens)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
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

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Non-Spam ERC20 Token Balances Of User(s)

### Fetching

First, you can fetch the ERC20 token balances of user(s) and show each ERC20 token whether they are a spam or not using the `token.isSpam` field from the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API.

With the query below, provide an array of users' 0x addresses, ENS domains, cb.ids, Lens profiles, Farcaster fnames/fids to `owner` input:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/gd2fCLJFjJ" %}
Show users' ERC20 token balances and check if each ERC20 token is a spam or not
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Ethereum: TokenBalances(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik.eth"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
    }
  ) {
    TokenBalance {
      tokenAddress
      token {
        isSpam
        name
        symbol
      }
    }
  }
  Polygon: TokenBalances(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik.eth"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: polygon
    }
  ) {
    TokenBalance {
      tokenAddress
      token {
        isSpam
        name
        symbol
      }
    }
  }
  Base: TokenBalances(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik.eth"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
    }
  ) {
    TokenBalance {
      tokenAddress
      token {
        isSpam
        name
        symbol
      }
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "tokenAddress": "0xbaaa3b3b00def381c0794d0253a4a1ef3b2a9fbc",
          "token": {
<strong>            "isSpam": false, // Not a spam ERC20 token
</strong>            "name": "Monkeys George",
            "symbol": "Monkeys George"
          }
        },
        {
          "tokenAddress": "0xfe7377b49567387345b44fc603f0fa3d294e62c7",
          "token": {
<strong>            "isSpam": false, // A Spam ERC20 token
</strong>            "name": "THE PEOPLEs MONEY",
            "symbol": "TPM"
          }
        },
        // Other Ethereum ERC20 tokens
      ]
    },
    "Polygon": {
      "TokenBalance": [
        {
          "tokenAddress": "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
          "token": {
<strong>            "isSpam": false, // Not a spam ERC20 token
</strong>            "name": "(PoS) Tether USD",
            "symbol": "USDT"
          }
        },
        {
          "tokenAddress": "0xb27cea9e38eeb387d3b0672287ca304fbff00b7c",
          "token": {
<strong>            "isSpam": true, // A spam ERC20 token
</strong>            "name": "Lens Rewards",
            "symbol": "Claim via lens-rewards.xyz"
          }
        },
        // Other Polygon ERC20 tokens
      ]
    },
    "Base": {
      "TokenBalance": [
        {
          "tokenAddress": "0xac1bd2486aaf3b5c0fc3fd868558b082a531b2b4",
          "token": {
<strong>            "isSpam": false, // Not a spam ERC20 token
</strong>            "name": "Toshi",
            "symbol": "TOSHI"
          }
        },
        {
          "tokenAddress": "0xa11ab779517a0edfabb1b5d9db1cd3167b268858",
          "token": {
<strong>            "isSpam": true, // A spam ERC20 token
</strong>            "name": "In 𝕏 S",
            "symbol": "𝕏S"
          }
        },
      ],
      // Other Base ERC20 tokens
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

### Formatting

Using the GraphQL response, you can filter out and aggregate the ERC20 tokens across Ethereum, Polygon, and Base by providing the response as to the `data` input of the `filterSpamERC20Tokens` function:

{% tabs %}
{% tab title="TypeScript" %}

```typescript
interface ERC20 {
  tokenAddress: string;
  token: {
    isSpam: boolean;
    name: string;
    symbol: string;
  };
}

interface ERC20WithBlockchain extends ERC20 {
  blockchain: "ethereum" | "polygon" | "base";
}

interface TokenBalancesResponse {
  Ethereum: {
    TokenBalance?: ERC20[];
  };
  Polygon: {
    TokenBalance?: ERC20[];
  };
  Base: {
    TokenBalance?: ERC20[];
  };
}

const filterSpamERC20Tokens = (
  data: TokenBalancesResponse
): ERC20WithBlockchain[] | undefined => {
  try {
    const { Ethereum, Polygon, Base } = data ?? {};
    const ethErc20Tokens =
      Ethereum?.TokenBalance?.map((erc20Token: ERC20) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "ethereum",
            }
      ) ?? [];
    const polygonErc20Tokens =
      Polygon?.TokenBalance?.map((erc20Token: ERC20) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "polygon",
            }
      ) ?? [];
    const baseErc20Tokens =
      Base?.TokenBalance?.map((erc20Token: ERC20) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "base",
            }
      ) ?? [];
    return [
      ...ethErc20Tokens,
      ...polygonErc20Tokens,
      ...baseErc20Tokens,
    ]?.filter(Boolean) as ERC20WithBlockchain[];
  } catch (e) {
    console.error(e);
  }
};
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const filterSpamERC20Tokens = (data) => {
  try {
    const { Ethereum, Polygon, Base } = data ?? {};
    const ethErc20Tokens =
      Ethereum?.TokenBalance?.map((erc20Token) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "ethereum",
            }
      ) ?? [];
    const polygonErc20Tokens =
      Polygon?.TokenBalance?.map((erc20Token) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "polygon",
            }
      ) ?? [];
    const baseErc20Tokens =
      Base?.TokenBalance?.map((erc20Token) =>
        erc20Token?.token?.isSpam
          ? null
          : {
              ...erc20Token,
              blockchain: "base",
            }
      ) ?? [];
    return [
      ...ethErc20Tokens,
      ...polygonErc20Tokens,
      ...baseErc20Tokens,
    ]?.filter(Boolean);
  } catch (e) {
    console.error(e);
  }
};
```

{% endtab %}

{% tab title="Python" %}

```python
from typing import List, Dict, Any, Optional
import traceback

def filter_spam_erc20_tokens(data: Optional[Dict[str, Any]]) -> List[Dict[str, Any]]:
    try:
        ethereum = data.get('Ethereum', {}) if data else {}
        polygon = data.get('Polygon', {}) if data else {}
        base = data.get('Base', {}) if data else {}

        eth_erc20_tokens = [
            {**erc20_token, 'blockchain': 'ethereum'} for erc20_token in ethereum.get('TokenBalance', [])
            if erc20_token and not erc20_token.get('token', {}).get('isSpam')
        ]

        polygon_erc20_tokens = [
            {**erc20_token, 'blockchain': 'polygon'} for erc20_token in polygon.get('TokenBalance', [])
            if erc20_token and not erc20_token.get('token', {}).get('isSpam')
        ]

        base_erc20_tokens = [
            {**erc20_token, 'blockchain': 'base'} for erc20_token in polygon.get('TokenBalance', [])
            if erc20_token and not erc20_token.get('token', {}).get('isSpam')
        ]

        return [erc20_token for erc20_token in eth_erc20_tokens + polygon_erc20_tokens + base_erc20_tokens if erc20_token]
    except Exception:
        error = traceback.print_exc()
        raise Exception(error)
```

{% endtab %}
{% endtabs %}

The formatted data will combine Ethereum, Polygon, and Base NFTs hold by the user and filtered out all spam NFTs:

```json
[
  {
    "tokenAddress": "0xbaaa3b3b00def381c0794d0253a4a1ef3b2a9fbc",
    "token": {
      "isSpam": false,
      "name": "Monkeys George",
      "symbol": "Monkeys George"
    },
    "blockchain": "ethereum"
  },
  // Other Ethereum non-spam ERC20 tokens
  {
    "tokenAddress": "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
    "token": { "isSpam": false, "name": "(PoS) Tether USD", "symbol": "USDT" },
    "blockchain": "polygon"
  },
  // Other Polygon non-spam ERC20 tokens
  {
    "tokenAddress": "0xac1bd2486aaf3b5c0fc3fd868558b082a531b2b4",
    "token": { "isSpam": false, "name": "Toshi", "symbol": "TOSHI" },
    "blockchain": "base"
  }
  // Other Base non-spam ERC20 tokens
]
```

## Check If ERC20 Token(s) Are Spam Or Not

You can use [Airstack](https://airstack.xyz) to check if ERC20 token(s) are spam or not by using [`Tokens`](../../api-references/api-reference/tokens-api/) API and providing the ERC20 token address(es) to `address` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lbqjtIyFrJ" %}
Check if Wrapped Ether, USD Coin, and MATIC are spam ERC20 tokens or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Tokens(
    input: {
      filter: {
        type: { _eq: ERC20 }
        address: {
          _in: [
            "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"
            "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"
            "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"
          ]
        }
      }
      blockchain: ethereum
    }
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Tokens": {
      "Token": [
        {
<strong>          "isSpam": false, // not Spam ERC20 tokens
</strong>          "address": "0x7d1afa7b718fb893db30a3abc0cfc608aacfebb0",
          "name": "Matic Token",
          "symbol": "MATIC",
          "type": "ERC20"
        },
        {
<strong>          "isSpam": false, // not Spam ERC20 tokens
</strong>          "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
          "name": "USD Coin",
          "symbol": "USDC",
          "type": "ERC20"
        },
        {
<strong>          "isSpam": false, // not Spam ERC20 tokens
</strong>          "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "name": "Wrapped Ether",
          "symbol": "WETH",
          "type": "ERC20"
        }
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Show All Non-Spam ERC20 Tokens

You can use [Airstack](https://airstack.xyz) to fetch all existing non-spam ERC20 tokens across Ethereum, Polygon, and Base:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ikS5WzdNMc" %}
Show all non-spam ERC20s on Ethereum, Polygon, and Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Ethereum: Tokens(
    input: {
      filter: { isSpam: { _eq: false }, type: { _eq: ERC20 } }
      blockchain: ethereum
    }
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
  Polygon: Tokens(
    input: {
      filter: { isSpam: { _eq: false }, type: { _eq: ERC20 } }
      blockchain: polygon
    }
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
  Base: Tokens(
    input: {
      filter: { isSpam: { _eq: false }, type: { _eq: ERC20 } }
      blockchain: base
    }
  ) {
    Token {
      isSpam
      address
      name
      symbol
      type
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
    "Ethereum": {
      "Token": [
        {
          "isSpam": false,
          "address": "0x000000000000073760fc462304360d9e887e4ef4",
          "name": "Fides",
          "symbol": "FIDES",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x0000000000000af8fe6e4de40f4804c90fa8ea8f",
          "name": "EPS API",
          "symbol": "EPSAPI",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x00000000000045166c45af0fc6e4cf31d9e14b9a",
          "name": "TopBidder",
          "symbol": "BID",
          "type": "ERC20"
        }
        // Other non-spam Ethereum ERC20 tokens
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEweDAwMDExOTZmMzM5NjNiMWUzYTkzODM2ODg3YTgwODBkZjM4ODg4ODgiLCJEYXRhVHlwZSI6InN0cmluZyJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "Token": [
        {
          "isSpam": false,
          "address": "0x0000000000000000000000000000000000001010",
          "name": "Matic Token",
          "symbol": "MATIC",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x0000000000004946c0e9f43f4dee607b0ef1fa1c",
          "name": "Chi Gastoken by 1inch",
          "symbol": "CHI",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x000000000000bd3c2d7cba506f2b058f3589ac60",
          "name": "U",
          "symbol": "U",
          "type": "ERC20"
        }
        // Other non-spam Polygon ERC20 tokens
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEzNzB4MDAwMTc0YTQzOWMyYzdkYjZmNWExMzEyNWE3NmE3NjdlMDJkOTgyZiIsIkRhdGFUeXBlIjoic3RyaW5nIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "Token": [
        {
          "isSpam": false,
          "address": "0x0000c16643e2b6849218cb33224ed3837bd85249",
          "name": "ruse",
          "symbol": "ruse",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x0001bd3cecb64f58843a135303d160246d02454e",
          "name": "Archly DEX Volatile AMM - ERN/HOP",
          "symbol": "vAMM-ERN/HOP",
          "type": "ERC20"
        },
        {
          "isSpam": false,
          "address": "0x0003787e89033f89d3659b01d9c2f0e48b190e5a",
          "name": "155",
          "symbol": "15",
          "type": "ERC20"
        }
        // Other non-spam Base ERC20 tokens
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjEzNzB4MDAwMTc0YTQzOWMyYzdkYjZmNWExMzEyNWE3NmE3NjdlMDJkOTgyZiIsIkRhdGFUeXBlIjoic3RyaW5nIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
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

- [NFT Details](../nft/nft-details.md)
- [NFT Balances](../nft/nft-balances.md)
- [NFT Holders](../nft/nft-holders.md)
- [Combinations (Common Holders)](../combinations/)
  - [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  - [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
- [Tokens In Common](../tokens-in-common/)
  - [NFTs](../tokens-in-common/nfts.md)
- [Tokens API Reference](../../api-references/api-reference/tokens-api/)
- [TokenNfts API Reference](../../api-references/api-reference/tokennfts-api/)
