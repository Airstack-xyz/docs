---
description: >-
  Learn how to recommend Mints based on Lookalike audiences: users who have X
  are also Minting Y.
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

# 👛 Common Minters

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Trending Mints By Common Minters Mock Up</p></figcaption></figure>

## Table Of Contents

The algorithm for building trending mints will be as follows:

{% hint style="info" %}
Currently, there is no dedicated backend API for fetching trending mints directly into your application. Therefore, the following implementation will require you to run a **backend**.

\
For the **backend**, you will be required to run a cronjob to fetch token mints data from the Airstack API and have the data stored in a dedicated database.\
\
This turotial will walk you through the steps required to implement Trending Mints today and deliver immediate value to your users.\\

Concurrently Airstack is working on a dedicated Trending Mints API for lighter-weight integrations in the near future.
{% endhint %}

1. [Fetch All Recent Token Mints Minted By Onchain Graph Users](common-minters.md#step-1-fetch-all-recent-token-mints-minted-by-onchain-graph-users)
2. [Score, Sort, and Filter Token Mints](common-minters.md#step-2-score-sort-and-filter-token-mints)
3. [Run as a Cronjob](common-minters.md#step-3-run-as-a-cronjob)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Existing Implementation of [Onchain Graph](../onchain-graph.md)

## Get Started

To get started, install the [Airstack](https://airstack.xyz) SDK:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/node dayjs node-cron
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/node dayjs node-cron
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @airstack/node dayjs node-cron
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack python-cron
```
{% endtab %}
{% endtabs %}

## Step 1: Fetch All Recent Token Mints Minted By Common Minters

First, define the following parameters to fetch the token mints data:

#### **Intervals**

The interval that you would like to run your cron job. Using the interval, you can then set the variables for the query that will be shown below:

* `endTime` to the current unix timestamp
* `startTime` to the current unix timestamp minus the chosen interval duration.

In this tutorial, you'll use 1 hour as the default interval.

#### **Token Types**

Input all the token types that you would like to fetch from the mints data.

If you only prefer fungible token mints, then includes only `ERC20`. If you instead want to enable NFT mints only, then include both `ERC721` and `ERC1155`.

#### **Chains**

Choose the chain that you would like to fetch the token mints data.

Currently, Airstack supports Ethereum, Polygon, Base, and Zora.

#### **Limit**

The number of JSON object responses per API call, with a maximum allowable value of **200**.

***

As these parameters are going to be having constant values, you can create a new file to store these as constant variables that can be imported in the next steps:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="constant.ts" %}
```typescript
export const interval = 1; // 1 hour
export const tokenType = ["ERC20", "ERC721", "ERC1155"];
export const chains = ["ethereum", "polygon", "base", "zora"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="constant.js" %}
```javascript
export const interval = 1; // 1 hour
export const tokenType = ["ERC20", "ERC721", "ERC1155"];
export const chains = ["ethereum", "polygon", "base", "zora"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="constant.py" %}
```python
interval = 1 # 1 hour
token_type = ["ERC20", "ERC721", "ERC1155"]
chains = ["ethereum", "polygon", "base", "zora"]
limit = 200
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Fetching

First, you will need to fetch all the common minters that minted the same tokens as the given user.

To fetch all common minters, first fetch all the tokens minted by the user by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API and provide the user's address to the `$user` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/EsHzgXo047" %}
Show me all tokens minted by user on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery(
  $user: Identity!
  $tokenType: [TokenType!]
  $chain: TokenBlockchain!
  $limit: Int
) {
  TokenTransfers(
    input: {
      filter: {
        operator: { _eq: $user }
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        to: { _eq: $user }
        tokenType: { _in: $tokenType }
      }
      blockchain: $chain
      order: { blockTimestamp: DESC }
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "user": "0xeaf55242a90bb3289dB8184772b0B98562053559",
  "tokenType": ["ERC20", "ERC721", "ERC1155"],
  "chain": "ethereum",
  "limit": 200
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "tokenAddress": "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e",
          "token": {
            "name": "Venture Club Alpha"
          }
        },
        {
          "tokenAddress": "0xc9b09c916e22eb7b68037275fe035eb30d3989f7",
          "token": {
            "name": "This Week in Farcaster - June 24, 2023 - Sponsored by Purple"
          }
        },
        {
          "tokenAddress": "0xebb15487787cbf8ae2ffe1a6cca5a50e63003786",
          "token": {
            "name": "Hyperkiwification"
          }
        }
        // Other tokens minted by the given user on Ethereum
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

With this result, you can then format it to form an array of addresses using `formatUserMints` function:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/format.ts" %}
```typescript
export interface Data {
  TokenTransfers: TokenTransfer;
}

export interface TokenTransfer {
  TokenTransfer: TokenTransferDetails[];
}

export interface TokenTransferDetails {
  tokenAddress: string;
  operator?: Identity;
  to?: Identity;
  token?: Token;
}

export interface Identity {
  addresses: string;
}

export interface Token {
  name: string;
}

/**
 * @description Format user mints to an array of token addresses
 * @examples
 * const { data } = await fetchQuery(query, variables);
 * formatUserMints(data);
 *
 * @param {Object} data - All minted tokens by a user from Airstack API
 * @returns An array of minted token addresses
 */
export const formatUserMints = (data: Data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ tokenAddress }) => tokenAddress);
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="utils/format.js" %}
```javascript
/**
 * @description Format user mints to an array of token addresses
 * @examples
 * const { data } = await fetchQuery(query, variables);
 * formatUserMints(data);
 *
 * @param {Object} data - All minted tokens by a user from Airstack API
 * @returns An array of minted token addresses
 */
export const formatUserMints = (data) =>
  data?.TokenTransfers?.TokenTransfer?.map(({ tokenAddress }) => tokenAddress);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
```python
def format_user_mints(data):
  """
  Takes a data object and returns a list of tokenAddress values from the nested TokenTransfers.

  :param data: The data object containing TokenTransfers
  :return: List of tokenAddress values
  """
  if data and "TokenTransfers" in data and "TokenTransfer" in data.get("TokenTransfers", {}):
    return [mint.get("tokenAddress") for mint in data.get("TokenTransfers", {}).get("TokenTransfer", [])]
  else:
    return []
```
{% endtab %}
{% endtabs %}

And the output of `formatUserMints` function will look as follows:

```json
[
  "0x0f92612c5f539c89dc379e8aa42e3ba862a34b7e",
  "0xc9b09c916e22eb7b68037275fe035eb30d3989f7",
  "0xebb15487787cbf8ae2ffe1a6cca5a50e63003786",
  "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
  "0x9340204616750cb61e56437befc95172c6ff6606"
  // other minted token addresses
]
```

With the obtained array of minted token addresses, then you can use [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) again to fetch all the addresses that also minted the minted tokens as the given users. In other words, the common minters to the given user:

### Try Demo

{% embed url="https://app.airstack.xyz/query/fbSvCu1STn" %}
Show all minters that minted an array of given tokens on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($mintedToken: Address!, $chain: TokenBlockchain!, $limit: Int) {
  TokenTransfers(
    input: {
      filter: {
        from: { _eq: "0x0000000000000000000000000000000000000000" }
        tokenAddress: { _eq: $mintedToken }
      }
      blockchain: $chain
      order: { blockTimestamp: DESC }
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
      operator {
        addresses
      }
      to {
        addresses
      }
    }
  }
}
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  // For more than 1 minted token, use loop
  "mintedToken": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
  "chain": "ethereum",
  "limit": 200
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "tokenAddress": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
          "token": {
            "name": "NF.TD"
          },
          "operator": {
            "addresses": [
              // operator is the executor of the transaction
<strong>              "0xeaf9830bb7a38a3cebcaca3ff9f626c424f3fb55"
</strong>            ]
          },
          "to": {
            "addresses": [
              // to is the receiver of the token minted
<strong>              "0xeaf9830bb7a38a3cebcaca3ff9f626c424f3fb55"
</strong>            ]
          }
        },
        // Other mints showing operator and receiver of the minted tokens
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

From here, you can use the data to get all the minters by checking if the operator and receiver address is equal and further categorize the list of minters by individual minted tokens.

***

With the defined parameters, you can use the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API again to construct an Airstack query to fetch all recent tokens minted by all the **common minters** of a given user in a certain interval period by providing the individual minter 0x addresses from `formatCommonMinters` to the `$commonMinters` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/suHzE9HODw" %}
Show me minted tokens on Polygon by a common minter at certain timestamp
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int,
  $commonMinter: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers that are mints
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        to: {_eq: $commonMinter},
        operator: {_eq: $commonMinter},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variables" %}
```json
{
  "startTime": "2023-11-09T13:25:00Z",
  "endTime": "2023-11-09T13:30:00Z",
  "tokenType": ["ERC20", "ERC721", "ERC1155"],
  "chain": "polygon",
  "limit": 200,
  "commonMinter": "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          // The address of minted NFT
<strong>          "tokenAddress": "0xd3b4de0d85c44c57993b3b18d42b00de81809eea",
</strong>          "token": {
            "name": "Unveiling Airstack's Onchain Graph"
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Iterate

Once you have the query ready, you can combine them in one `main` function to be executed in a single flow.

Before defining the `main` function, create a separate function to fetch the list of all common minters. To fetch all the data, the query will be iterated multiple times using the [`fetchQueryWithPagination`](../../nodejs-sdk-reference/fetchquerywithpagination.md) function provided by the Airstack SDK to iterate over all the blockchains and the paginations.

{% tabs %}
{% tab title="TypeScript" %}
{% code title="functions/fetchCommonMinters.ts" %}
```typescript
import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import { chains, tokenType, limit } from "../constant";

config();

init(process.env.AIRSTACK_API_KEY);

const commonMintAddressesQuery = `
query MyQuery(
  $user: Identity!,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int!
) {
  TokenTransfers(
    input: {
      filter: {
        operator: {_eq: $user},
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_eq: $user},
        tokenType: {_in: $tokenType},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
}
`;

const commonMintersQuery = `
query MyQuery(
  $mintedToken: Address!,
  $chain: TokenBlockchain!,
  $limit: Int!
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        tokenAddress: {_eq: $mintedToken},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
      operator {
        addresses
      }
      to {
        addresses
      }
    }
  }
}
`;

/**
 * @description Fetches common minters associated with individual minted token by `user`
 * @example
 * const res = await fetchCommonMinters("0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB");
 *
 * @param {string} user – User's 0x address
 * @returns Common minters associated with individual minted token by `user`
 */
const fetchCommonMinters = async (user: string) => {
  let mintedTokensDataResponse;
  let mintsData = [];
  for (let chain of chains) {
    while (true) {
      if (!mintedTokensDataResponse) {
        mintedTokensDataResponse = await fetchQueryWithPagination(
          commonMintAddressesQuery,
          {
            user,
            tokenType,
            chain,
            limit,
          }
        );
      }
      // 1. Fetch all minted tokens by `user`
      const {
        data: mintedTokensData,
        error: mintedTokensError,
        hasNextPage: mintedTokensHasNextPage,
        getNextPage: mintedTokensGetNextPage,
      } = mintedTokensDataResponse ?? {};
      if (!mintedTokensError) {
        const mintedTokens = formatUserMints(mintedTokensData);
        if (mintedTokens.length === 0) break;
        for (let token of mintedTokens ?? []) {
          let commonMintersDataResponse;
          while (true) {
            if (!commonMintersDataResponse) {
              commonMintersDataResponse = await fetchQueryWithPagination(
                commonMintersQuery,
                {
                  mintedTokens: token,
                  chain,
                  limit,
                }
              );
            }
            // 2. Fetch all users that minted the same token minted by 'user'
            const {
              data: commonMintersData,
              error: commonMintersError,
              hasNextPage: commonMintersHasNextPage,
              getNextPage: commonMintersGetNextPage,
            } = commonMintersDataResponse;
            if (!commonMintersError) {
              const mint =
                commonMintersData?.TokenTransfers?.TokenTransfer?.[0];
              const { tokenAddress } = mint ?? {};
              delete mint?.operator;
              delete mint?.to;
              const mintsIndex = mintsData.findIndex(
                ({ tokenAddress: address }) => address === tokenAddress
              );
              const commonMintersList = (
                commonMintersData?.TokenTransfers?.TokenTransfer ?? []
              )
                ?.map(({ operator, to }) =>
                  operator?.addresses?.[0] === to?.addresses?.[0]
                    ? operator?.addresses?.[0]
                    : null
                )
                .filter(Boolean);
              if (commonMintersList.length !== 0) {
                if (mintsIndex === -1) {
                  mintsData = [
                    ...mintsData,
                    {
                      ...mint,
                      chain,
                      minters: commonMintersList?.filter((value, index) => {
                        return commonMintersList.indexOf(value) === index;
                      }),
                    },
                  ];
                } else {
                  mintsData[mintsIndex] = {
                    ...mintsData[mintsIndex],
                    minters: [
                      ...mintsData[mintsIndex]?.minters,
                      ...commonMintersList,
                    ].filter((value, index) => {
                      return (
                        [
                          ...mintsData[mintsIndex]?.minters,
                          ...commonMintersList,
                        ].indexOf(value) === index
                      );
                    }),
                  };
                }
              }

              if (!commonMintersHasNextPage) {
                break;
              } else {
                commonMintersDataResponse = await commonMintersGetNextPage();
              }
            } else {
              console.error("Error: ", commonMintersError);
              break;
            }
          }

          // reset common minters data for loop
          commonMintersDataResponse = null;
        }

        if (!mintedTokensHasNextPage) {
          break;
        } else {
          mintedTokensDataResponse = await mintedTokensGetNextPage();
        }
      } else {
        console.error("Error: ", mintedTokensError);
        break;
      }
    }

    // reset minted tokens data for loop
    mintedTokensDataResponse = null;
  }

  return mintsData;
};

export default fetchCommonMinters;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="functions/fetchCommonMinters.js" %}
```javascript
import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import { chains, tokenType, limit } from "../constant";

config();

init(process.env.AIRSTACK_API_KEY);

const commonMintAddressesQuery = `
query MyQuery(
  $user: Identity!,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int!
) {
  TokenTransfers(
    input: {
      filter: {
        operator: {_eq: $user},
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_eq: $user},
        tokenType: {_in: $tokenType},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
}
`;

const commonMintersQuery = `
query MyQuery(
  $mintedToken: Address!,
  $chain: TokenBlockchain!,
  $limit: Int!
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        tokenAddress: {_eq: $mintedToken},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
      operator {
        addresses
      }
      to {
        addresses
      }
    }
  }
}
`;

/**
 * @description Fetches common minters associated with individual minted token by `user`
 * @example
 * const res = await fetchCommonMinters("0xB59Aa5Bb9270d44be3fA9b6D67520a2d28CF80AB");
 *
 * @param {string} user – User's 0x address
 * @returns Common minters associated with individual minted token by `user`
 */
const fetchCommonMinters = async (user) => {
  let mintedTokensDataResponse;
  let mintsData = [];
  for (let chain of chains) {
    while (true) {
      if (!mintedTokensDataResponse) {
        mintedTokensDataResponse = await fetchQueryWithPagination(
          commonMintAddressesQuery,
          {
            user,
            tokenType,
            chain,
            limit,
          }
        );
      }
      // 1. Fetch all minted tokens by `user`
      const {
        data: mintedTokensData,
        error: mintedTokensError,
        hasNextPage: mintedTokensHasNextPage,
        getNextPage: mintedTokensGetNextPage,
      } = mintedTokensDataResponse ?? {};
      if (!mintedTokensError) {
        const mintedTokens = formatUserMints(mintedTokensData);
        if (mintedTokens.length === 0) break;
        for (let token of mintedTokens ?? []) {
          let commonMintersDataResponse;
          while (true) {
            if (!commonMintersDataResponse) {
              commonMintersDataResponse = await fetchQueryWithPagination(
                commonMintersQuery,
                {
                  mintedTokens: token,
                  chain,
                  limit,
                }
              );
            }
            // 2. Fetch all users that minted the same token minted by 'user'
            const {
              data: commonMintersData,
              error: commonMintersError,
              hasNextPage: commonMintersHasNextPage,
              getNextPage: commonMintersGetNextPage,
            } = commonMintersDataResponse;
            if (!commonMintersError) {
              const mint =
                commonMintersData?.TokenTransfers?.TokenTransfer?.[0];
              const { tokenAddress } = mint ?? {};
              delete mint?.operator;
              delete mint?.to;
              const mintsIndex = mintsData.findIndex(
                ({ tokenAddress: address }) => address === tokenAddress
              );
              const commonMintersList = (
                commonMintersData?.TokenTransfers?.TokenTransfer ?? []
              )
                ?.map(({ operator, to }) =>
                  operator?.addresses?.[0] === to?.addresses?.[0]
                    ? operator?.addresses?.[0]
                    : null
                )
                .filter(Boolean);
              if (commonMintersList.length !== 0) {
                if (mintsIndex === -1) {
                  mintsData = [
                    ...mintsData,
                    {
                      ...mint,
                      chain,
                      minters: commonMintersList?.filter((value, index) => {
                        return commonMintersList.indexOf(value) === index;
                      }),
                    },
                  ];
                } else {
                  mintsData[mintsIndex] = {
                    ...mintsData[mintsIndex],
                    minters: [
                      ...mintsData[mintsIndex]?.minters,
                      ...commonMintersList,
                    ].filter((value, index) => {
                      return (
                        [
                          ...mintsData[mintsIndex]?.minters,
                          ...commonMintersList,
                        ].indexOf(value) === index
                      );
                    }),
                  };
                }
              }

              if (!commonMintersHasNextPage) {
                break;
              } else {
                commonMintersDataResponse = await commonMintersGetNextPage();
              }
            } else {
              console.error("Error: ", commonMintersError);
              break;
            }
          }

          // reset common minters data for loop
          commonMintersDataResponse = null;
        }

        if (!mintedTokensHasNextPage) {
          break;
        } else {
          mintedTokensDataResponse = await mintedTokensGetNextPage();
        }
      } else {
        console.error("Error: ", mintedTokensError);
        break;
      }
    }

    // reset minted tokens data for loop
    mintedTokensDataResponse = null;
  }

  return mintsData;
};

export default fetchCommonMinters;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="fetch_common_minters.py" %}
```python
from airstack.execute_query import AirstackClient
from dotenv import load_dotenv
from constant import token_type, chains
from typing import List, Dict, Any

load_dotenv()

api_key = os.environ.get("AIRSTACK_API_KEY")

api_client = AirstackClient(api_key=api_key)

common_mint_addresses_query = """
query MyQuery(
  $user: Identity!,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!
) {
  TokenTransfers(
    input: {
      filter: {
        operator: {_eq: $user},
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        to: {_eq: $user},
        tokenType: {_in: $tokenType},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      tokenAddress
    }
  }
}
"""

common_minters_query = """
query MyQuery(
  $mintedToken: Address!,
  $chain: TokenBlockchain!
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        tokenAddress: {_eq: $mintedToken},
      },
      blockchain: $chain,
      order: {blockTimestamp: DESC}
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
      operator {
        addresses
      }
      to {
        addresses
      }
    }
  }
}
"""

async def fetch_common_minters(user: str) -> List[Dict[str, Any]]:
    minted_tokens_data_response = None
    mints_data = []
    for chain in chains:
        while True:
            if minted_tokens_data_response is None:
                # 1. Fetch all minted tokens by `user`
                execute_query_client = api_client.create_execute_query_object(
                    query=common_mint_addresses_query, variables={
                        "user": user,
                        "tokenType": token_type,
                        "chain": chain,
                        "limit": limit,
                    })
                minted_tokens_data_response = await execute_query_client.execute_paginated_query()
            if minted_tokens_data_response.error is None:
                minted_tokens = format_user_mints(
                    minted_tokens_data_response.data)
                if (len(minted_tokens) == 0):
                    break
                for token in minted_tokens:
                    common_minters_data_response = None
                    while True:
                        if (common_minters_data_response is None):
                            # 2. Fetch all common minters for each minted token
                            execute_query_client = api_client.create_execute_query_object(
                                query=common_minters_query, variables={
                                    "mintedToken": token,
                                    "chain": chain,
                                })
                            common_minters_data_response = await execute_query_client.execute_paginated_query()

                        if common_minters_data_response.error is None:
                            common_minters_data = common_minters_data_response.data

                            mint = common_minters_data.get(
                                'TokenTransfers', {}).get('TokenTransfer', [None])[0]
                            if mint:
                                token_address = mint.get('tokenAddress', '')
                                mint.pop('operator', None)
                                mint.pop('to', None)

                                # Find the index of the mint in mints_data
                                mints_index = next((index for index, m in enumerate(
                                    mints_data) if m.get('tokenAddress') == token_address), -1)

                                # Construct the commonMintersList
                                common_minters_list = []
                                for transfer in common_minters_data.get('TokenTransfers', {}).get('TokenTransfer', []):
                                    operator = transfer.get('operator', {})
                                    to = transfer.get('to', {})
                                    if operator.get('addresses', [None])[0] == to.get('addresses', [None])[0]:
                                        common_minters_list.append(
                                            operator.get('addresses', [None])[0])

                                # Filter out None and duplicates from commonMintersList
                                common_minters_list = list(dict.fromkeys(
                                    filter(None, common_minters_list)))

                                if common_minters_list:
                                    if mints_index == -1:
                                        mints_data.append({
                                            **mint,
                                            'chain': chain,
                                            'minters': common_minters_list
                                        })
                                    else:
                                        # Merging minters lists and removing duplicates
                                        existing_minters = mints_data[mints_index].get(
                                            'minters', [])
                                        combined_minters = list(dict.fromkeys(
                                            existing_minters + common_minters_list))
                                        mints_data[mints_index]['minters'] = combined_minters

                            if not common_minters_data_response.has_next_page:
                                break
                            else:
                                common_minters_data_response = await common_minters_data_response.get_next_page
                        else:
                            print(common_minters_data_response.error)
                            break

                # Iterate over all paginations
                if not minted_tokens_data_response.has_next_page:
                    break
                else:
                    minted_tokens_data_response = await minted_tokens_data_response.get_next_page
            else:
                print(minted_tokens_data_response.error)
                break

        minted_tokens_data_response = None

    return mints_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

When the result from `fetchCommonMinters` is logged, it will looks as follows:

```json
[
  {
    "tokenAddress": "0x2a9ea02e4c2dcd56ba20628fe1bd46bae2c62746",
    "token": { "name": "FarCon 2023 Tickets" },
    "chain": "ethereum",
    "minters": [
      "0xae2586e76c8a4d8dc1ff3d9ab70bec760ae143c2",
      "0x2152ad70e4b395169923e2c6e8b09cd81b50c498",
      "0xb8786d48c23bf7e5a0eff3089ba439d8e2fa6fe0"
    ]
  },
  {
    "tokenAddress": "0xd3b4de0d85c44c57993b3b18d42b00de81809eea",
    "token": { "name": "Unveiling Airstack's Onchain Graph" },
    "chain": "polygon",
    "minters": ["0x427a1c6dcaad92f886020a61e0b85be8e1c5ead5"]
  }
  // Other common minters and token details that they minted
]
```

Once the list of common minters is fetched and categorized by the tokens minted, you can then use the list of common minters 0x address as an input to fetch tokens that they are minting within the given interval:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import { interval, tokenType, chains, limit } from "./constant";
import fetchCommonMinters from "./functions/fetchCommonMinters";
import dayjs, { Dayjs } from "dayjs";

config();

init(process.env.AIRSTACK_API_KEY);

const query = `
query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int,
  $commonMinter: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $commonMinter},
        operator: {_eq: $commonMinter},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
    }
  }
}
`;

const main = async (user: string, currentTime: Dayjs) => {
  let response;
  let mintsData = [];
  const commonMintersByMintedTokens = await fetchCommonMinters(user);
  // Iterate over all onchain graph users
  for (let commonMinterDetail of commonMintersByMintedTokens) {
    const { tokenAddress, token, minters } = commonMinterDetail ?? {};
    for (let commonMinter of minters) {
      // Iterate over all blockchain
      for (let chain of chains) {
        while (true) {
          if (!response) {
            response = await fetchQueryWithPagination(query, {
              startTime: dayjs(currentTime?.subtract(interval, "h")).format(
                "YYYY-MM-DDTHH:mm:ss[Z]"
              ),
              endTime: currentTime?.format("YYYY-MM-DDTHH:mm:ss[Z]"),
              chain,
              limit,
              tokenType,
              commonMinter,
            });
          }

          const { data, error, getNextPage, hasNextPage } = response ?? {};
          if (!error) {
            for (let res of data?.TokenTransfers?.TokenTransfer ?? []) {
              const mintsIndex = mintsData.findIndex(
                ({ tokenAddress: address }) => address === res?.tokenAddress
              );
              if (mintsIndex !== -1) {
                const associatedMintsIndex = mintsData[
                  mintsIndex
                ]?.associatedMints?.findIndex(
                  ({ tokenAddress: address }) => address === tokenAddress
                );
                if (associatedMintsIndex !== -1) {
                  mintsData[mintsIndex].associatedMints[
                    associatedMintsIndex
                  ].minters = [
                    ...mintsData[mintsIndex].associatedMints[
                      associatedMintsIndex
                    ].minters,
                    commonMinter,
                  ];
                  mintsData[mintsIndex].associatedMints[
                    associatedMintsIndex
                  ].frequency += 1;
                } else {
                  mintsData[mintsIndex].associatedMints = [
                    ...mintsData[mintsIndex].associatedMints,
                    {
                      tokenAddress,
                      token,
                      minters: [commonMinter],
                      frequency: 1,
                    },
                  ];
                }
              } else {
                mintsData = [
                  ...mintsData,
                  {
                    ...res,
                    chain,
                    /*
                     * associated mints is the tokens that were minted in the past
                     * that is commonly minted by the given `user`. This is kept track
                     * in order to recommend user minted tokens that were minted by
                     * the same group of users that minted certain collections
                     *
                     * For example, minters of X also minted Y
                     */
                    associatedMints: [
                      {
                        tokenAddress,
                        token,
                        minters: [commonMinter],
                        frequency: 1,
                      },
                    ],
                  },
                ];
              }
            }
            // Iterate over all paginations
            if (!hasNextPage) {
              break;
            } else {
              response = await getNextPage();
            }
          } else {
            console.error("Error: ", error);
            break;
          }
        }

        // Resetting the loop
        response = null;
      }
    }
  }

  return mintsData;
};

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import { interval, tokenType, chains, limit } from "./constant";
import fetchCommonMinters from "./functions/fetchCommonMinters";
import dayjs from "dayjs";

config();

init(process.env.AIRSTACK_API_KEY);

const query = `
query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int,
  $commonMinter: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $commonMinter},
        operator: {_eq: $commonMinter},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit,
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
    }
  }
}
`;

const main = async (user, currentTime) => {
  let response;
  let mintsData = [];
  const commonMintersByMintedTokens = await fetchCommonMinters(user);
  // Iterate over all onchain graph users
  for (let commonMinterDetail of commonMintersByMintedTokens) {
    const { tokenAddress, token, minters } = commonMinterDetail ?? {};
    for (let commonMinter of minters) {
      // Iterate over all blockchain
      for (let chain of chains) {
        while (true) {
          if (!response) {
            response = await fetchQueryWithPagination(query, {
              startTime: dayjs(currentTime?.subtract(interval, "h")).format(
                "YYYY-MM-DDTHH:mm:ss[Z]"
              ),
              endTime: currentTime?.format("YYYY-MM-DDTHH:mm:ss[Z]"),
              chain,
              limit,
              tokenType,
              commonMinter,
            });
          }

          const { data, error, getNextPage, hasNextPage } = response ?? {};
          if (!error) {
            for (let res of data?.TokenTransfers?.TokenTransfer ?? []) {
              const mintsIndex = mintsData.findIndex(
                ({ tokenAddress: address }) => address === res?.tokenAddress
              );
              if (mintsIndex !== -1) {
                const associatedMintsIndex = mintsData[
                  mintsIndex
                ]?.associatedMints?.findIndex(
                  ({ tokenAddress: address }) => address === tokenAddress
                );
                if (associatedMintsIndex !== -1) {
                  mintsData[mintsIndex].associatedMints[
                    associatedMintsIndex
                  ].minters = [
                    ...mintsData[mintsIndex].associatedMints[
                      associatedMintsIndex
                    ].minters,
                    commonMinter,
                  ];
                  mintsData[mintsIndex].associatedMints[
                    associatedMintsIndex
                  ].frequency += 1;
                } else {
                  mintsData[mintsIndex].associatedMints = [
                    ...mintsData[mintsIndex].associatedMints,
                    {
                      tokenAddress,
                      token,
                      minters: [commonMinter],
                      frequency: 1,
                    },
                  ];
                }
              } else {
                mintsData = [
                  ...mintsData,
                  {
                    ...res,
                    chain,
                    /*
                     * associated mints is the tokens that were minted in the past
                     * that is commonly minted by the given `user`. This is kept track
                     * in order to recommend user minted tokens that were minted by
                     * the same group of users that minted certain collections
                     *
                     * For example, minters of X also minted Y
                     */
                    associatedMints: [
                      {
                        tokenAddress,
                        token,
                        minters: [commonMinter],
                        frequency: 1,
                      },
                    ],
                  },
                ];
              }
            }
            // Iterate over all paginations
            if (!hasNextPage) {
              break;
            } else {
              response = await getNextPage();
            }
          } else {
            console.error("Error: ", error);
            break;
          }
        }

        // Resetting the loop
        response = null;
      }
    }
  }

  return mintsData;
};

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="index.py" %}
```python
import os
from airstack.execute_query import AirstackClient
from dotenv import load_dotenv
from constant import interval, token_type, chains, limit
from fetch_common_minters import fetch_common_minters
from datetime import datetime, timedelta
from typing import List, Dict, Any

load_dotenv()

api_key = os.environ.get("AIRSTACK_API_KEY")

api_client = AirstackClient(api_key=api_key)

query = """
query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int,
  $commonMinter: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $commonMinter},
        operator: {_eq: $commonMinter},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit,
    }
  ) {
    TokenTransfer {
      tokenAddress
      token {
        name
      }
    }
  }
}
"""


async def main(user: str, current_time: datetime) -> List[Dict[str, Any]]:
    query_response = None
    mints_data = []

    common_minters_by_minted_tokens = await fetch_common_minters(user)
    for common_minter_detail in common_minters_by_minted_tokens:
        token_address = common_minter_detail.get("tokenAddress", "")
        token = common_minter_detail.get("token", {})
        minters = common_minter_detail.get("minters", [])
        # Iterate over all common minters
        for common_minter in minters[0:2]:
            # Iterate over all blockchain
            for chain in chains:
                while True:
                    if query_response is None:
                        execute_query_client = api_client.create_execute_query_object(
                            query=query, variables={
                                "startTime": (current_time + timedelta(hours=-interval)).strftime("%Y-%m-%dT%H:%M:%SZ"),
                                "endTime": current_time.strftime("%Y-%m-%dT%H:%M:%SZ"),
                                "tokenType": token_type,
                                "chain": chain,
                                "limit": limit,
                                "commonMinter": common_minter
                            })
                        query_response = await execute_query_client.execute_paginated_query()
                        if query_response.error is None:
                            data = query_response.data
                            token_transfers = data.get(
                                'TokenTransfers') if data.get(
                                'TokenTransfers') is not None else {}
                            token_transfer_list = token_transfers.get(
                                'TokenTransfer') if token_transfers.get(
                                'TokenTransfer') is not None else []

                            for res in token_transfer_list:
                                if not res:
                                    continue

                                # Extract tokenAddress from res, ensuring it's not None
                                res_token_address = res.get('tokenAddress')
                                if not res_token_address:
                                    continue

                                # Find index of the current token in the mints_data list
                                mints_index = next((index for index, m in enumerate(
                                    mints_data) if m.get('tokenAddress') == res_token_address), -1)
                                if mints_index != -1:
                                    # Find index of associated mint
                                    associated_mints = mints_data[mints_index].get(
                                        'associatedMints', [])
                                    associated_mints_index = next((index for index, m in enumerate(
                                        associated_mints) if m.get('tokenAddress') == token_address), -1)

                                    if associated_mints_index != -1:
                                        # Update existing associated mint
                                        mints_data[mints_index]['associatedMints'][associated_mints_index]['minters'].append(
                                            common_minter)
                                        mints_data[mints_index]['associatedMints'][associated_mints_index]['frequency'] += 1
                                    else:
                                        # Add new associated mint
                                        mints_data[mints_index].setdefault('associatedMints', []).append({
                                            'tokenAddress': token_address,
                                            'token': token,
                                            'minters': [common_minter],
                                            'frequency': 1
                                        })
                                else:
                                    # Add new entry to mints_data
                                    new_entry = res.copy()
                                    new_entry.update({
                                        'chain': chain,
                                        'associatedMints': [{
                                            'tokenAddress': token_address,
                                            'token': token,
                                            'minters': [common_minter],
                                            'frequency': 1
                                        }]
                                    })
                                    mints_data.append(new_entry)

                            # Iterate over all paginations
                            # if not query_response.has_next_page:
                            break
                            # else:
                            # query_response = await query_response.get_next_page
                        else:
                            print(query_response.error)
                            break
                    else:
                        print(query_response.error)
                        break

                # Resetting the loop
                query_response = None

    return mints_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

In this step, you'll also be keeping track of **associated minted tokens** to the tokens minted at the given interval. This is done such that you can know which token Y that is currently minted by the list of common minters that also minted token X the past.

In addition to the associated minted token addresses, you'll also be tracking the list of 0x addresses of the minters and the number of times (frequency) that token Y has been minted by the group of minters that also minted token X.

If you log the `main` function, it will return a response as follows:

```json
[
  {
    "tokenAddress": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    "token": { "name": "NFT.TD" },
    "chain": "ethereum",
    "associatedMints": [
      {
        "tokenAddress": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
        "token": { "name": "BoredApeYachtClub" },
        "minters": [
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe"
        ],
        "frequency": 5
      }
    ]
  },
  {
    "tokenAddress": "0xb63056fc3dab4f755d4d0380cf36e67c1164da64",
    "token": { "name": "Polysapien Open Edition" },
    "chain": "polygon",
    "associatedMints": [
      {
        "tokenAddress": "0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401",
        "token": { "name": "NameWrapper" },
        "minters": [
          "0x09ce2896b24e60cb3de34e2b826c2ef4545a7566",
          "0x95b7ce09add500052386318863d166326220d8e9",
          "0x575f10e1fe5f1ffa9d8b888b55bf59c7b8c01fa0",
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe",
          "0x9f06bbd82b7a26272d1eafa118cda2819b22d793",
          "0xeab05f8e538982516d119361d7e6c31e8fb6f7c8",
          "0x1d9a9b5e73259bbf0272c5228330ffd24bca82c0",
          "0x8135b0dd3eb53c1c391f4b228824ea60291793c9"
        ],
        "frequency": 10
      }
      // Other associated token mints, if any
    ]
  }
  // Other minted tokens data
]
```

## Step 2: Score, Sort, and Filter Token Mints

After fetching all the raw token mints data from [Airstack](https://airstack.xyz) API, next you can process the data to determine which will qualify as trending mints.

In this tutorial, there will be 3 steps to process the token mints data:

1. [Scoring](common-minters.md#scoring) – assigning a score to each minted tokens to determine how "popular" each tokens are in a certain period of time
2. [Sorting](common-minters.md#sorting) – sort the minted tokens data by the score, in descending order
3. [Filtering](common-minters.md#filtering) – filter out all non-trending tokens that does not qualify

These procedures are **NOT** a strict requirement and can be defined by yourself depending on the requirements you have for building the feature into application.

### Scoring

In this tutorial, you'll define the scoring function for a minted token to be **the sum of associated token's `frequency` value and the length of `minters`** **array** in the defined interval:

{% hint style="info" %}
You are not required to follow the scoring logic shown in this tutorial. Depending on your use cases, you are free to create your own custom scoring function or skip the scoring step all together if unnecessary.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/scoring.ts" %}
```typescript
import { TokenTransfer } from "./format";

/**
 * @description
 * Score all recent token mint data
 *
 * @example
 * const res = scoringFunction(data);
 *
 * @param {Object} data – Formatted minted tokens data from Airstack API
 * @returns scored mint data
 */
const scoringFunction = (data: TokenTransfer[]) => {
  let trendingMints = [];

  data.forEach((val) => {
    const { tokenAddress, chain, associatedMints } = val ?? {};
    const valIndex = trendingMints.findIndex((value) => {
      return value?.tokenAddress === tokenAddress && value?.chain === chain;
    });
    const addScore = associatedMints
      .map(({ minters, frequency }) => minters?.length * frequency)
      .reduce((a, b) => a + b, 0);
    if (valIndex !== -1) {
      trendingMints[valIndex] = {
        ...trendingMints[valIndex],
        // For each new mints, add score of 1
        score: trendingMints[valIndex]?.score + addScore,
      };
    } else {
      // For new token mints, assigned initial score of 1
      trendingMints.push({ ...val, score: addScore });
    }
  });

  return trendingMints;
};

export default scoringFunction;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="utils/scoring.js" %}
```javascript
/**
 * @description
 * Score all recent token mint data
 *
 * @example
 * const res = scoringFunction(data);
 *
 * @param {Object} data – Formatted minted tokens data from Airstack API
 * @returns scored mint data
 */
const scoringFunction = (data) => {
  let trendingMints = [];

  data.forEach((val) => {
    const { tokenAddress, chain } = val ?? {};
    const valIndex = trendingMints.findIndex((value) => {
      return value?.tokenAddress === tokenAddress && value?.chain === chain;
    });
    if (valIndex !== -1) {
      trendingMints[valIndex] = {
        ...trendingMints[valIndex],
        score: trendingMints[valIndex]?.score + 1,
      };
    } else {
      delete val?.tokenId;
      trendingMints.push({ ...val, score: 1 });
    }
  });

  return trendingMints;
};

export default scoringFunction;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/scoring.py" %}
```python
from typing import List, Dict, Any

def scoring_function(data: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
    """
    Score all recent token mint data

    :param data: Formatted minted tokens data
    :return: Scored mint data
    """
    trending_mints = []

    for val in data:
        token_address = val.get('tokenAddress')
        chain = val.get('chain')
        associated_mints = val.get('associatedMints', [])

        # Find index of the current token in the trending list
        val_index = next((index for index, value in enumerate(trending_mints)
                          if value.get('tokenAddress') == token_address and value.get('chain') == chain), -1)

        # Calculate additional score
        add_score = sum(len(minter.get('minters', 1)) * minter.get('frequency', 1) for minter in associated_mints)

        if val_index != -1:
            # Update score for existing entry
            trending_mints[val_index]['score'] += add_score
        else:
            # Create a new entry with initial score
            val['score'] = add_score
            trending_mints.append(val)

    return trending_mints

```
{% endcode %}
{% endtab %}
{% endtabs %}

Then, you can import the `scoringFunction` back to `main` to have the data from Airstack API scored:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
// same imports as above
import scoringFunction from "./utils/scoring";

const main = (user: string, currentTime: Dayjs) = > {
  // same as above
  const scoredData = scoringFunction(mintsData);
  return scoredData
}

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
// same imports as above
import { scoringFunction } from "./utils/scoring";

const main = (currentTime) = > {
  // same as above
  const scoredData = scoringFunction(mintsData);
  return scoredData
}

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="index.py" %}
```python
# same imports as above
from utils.scoring import scoring_function

async def main(current_time: datetime) -> List[Dict[str, Any]]:
  # same as above
  scored_data = scoring_function(mints_data)
  return scored_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

When the result of the newly modified `main` function is logged, it will have result that look as follows:

```json
[
  {
    "tokenAddress": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    "token": { "name": "NFT.TD" },
    "chain": "ethereum",
    "associatedMints": [
      {
        "tokenAddress": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
        "token": { "name": "BoredApeYachtClub" },
        "minters": [
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe"
        ],
        "frequency": 5
      }
    ],
    "score": 15 // 5 * 3 (minters?.length)
  },
  {
    "tokenAddress": "0xb63056fc3dab4f755d4d0380cf36e67c1164da64",
    "token": { "name": "Polysapien Open Edition" },
    "chain": "polygon",
    "associatedMints": [
      {
        "tokenAddress": "0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401",
        "token": { "name": "NameWrapper" },
        "minters": [
          "0x09ce2896b24e60cb3de34e2b826c2ef4545a7566",
          "0x95b7ce09add500052386318863d166326220d8e9",
          "0x575f10e1fe5f1ffa9d8b888b55bf59c7b8c01fa0",
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe",
          "0x9f06bbd82b7a26272d1eafa118cda2819b22d793",
          "0xeab05f8e538982516d119361d7e6c31e8fb6f7c8",
          "0x1d9a9b5e73259bbf0272c5228330ffd24bca82c0",
          "0x8135b0dd3eb53c1c391f4b228824ea60291793c9"
        ],
        "frequency": 10
      }
      // Other associated token mints, if any
    ],
    "score": 100 // 10 * 10 (minters?.length)
  }
  // Other scored token mints data
]
```

### Sorting

Once you have the token mints data scored, you can implement a very simple sorting function that sorts in descending order based on the `score` field value:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/sorting.ts" %}
```typescript
import { TokenTransfer } from "./format";

export interface TokenTransferWithScore extends TokenTransfer {
  score: number;
}

/**
 * @description
 * Sort all scored mints data by `score` field
 *
 * @example
 * const res = sortingFunction(scoredData);
 *
 * @param {Object} data – Minted tokens data with `score` field
 * @returns scored and sorted mint data
 */
const sortingFunction = (scoredData: TokenTransferWithScore) =>
  scoredData?.sort((a, b) => b.score - a.score);

export default sortingFunction;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="utils/sorting.js" %}
```javascript
/**
 * @description
 * Sort all scored mints data by `score` field
 *
 * @example
 * const res = sortingFunction(scoredData);
 *
 * @param {Object} data – Minted tokens data with `score` field
 * @returns scored and sorted mint data
 */
const sortingFunction = (scoredData) =>
  scoredData?.sort((a, b) => b.score - a.score);

export default sortingFunction;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/sorting.py" %}
```python
from typing import List, Dict, Any

def sorting_function(trending_mints: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
    return sorted(trending_mints, key=lambda x: x['score'], reverse=True)
```
{% endcode %}
{% endtab %}
{% endtabs %}

Then, you can import the `sortingFunction` back to `main` to have the scored data sorted:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
// same imports as above
import { sortingFunction } from "./utils/sorting";

const main = (user: string, currentTime: Dayjs) = > {
  // same as above
  const sortedData = sortingFunction(scoredData);
  return sortedData
}

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
// same imports as above
import sortingFunction from "./utils/sorting";

const main = () = > {
  // same as above
  const sortedData = sortingFunction(scoredData);
  return sortedData
}

export default main;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="index.py" %}
```python
# same imports as above
from utils.sorting import sorting_function

async def main(current_time: datetime) -> List[Dict[str, Any]]:
  # same as above
  sorted_data = sorting_function(scored_data)
  return sorted_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

When the result of the newly modified `main` function is logged, it will have result that look as follows:

```json
[
  {
    "tokenAddress": "0xb63056fc3dab4f755d4d0380cf36e67c1164da64",
    "token": { "name": "Polysapien Open Edition" },
    "chain": "polygon",
    "associatedMints": [
      {
        "tokenAddress": "0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401",
        "token": { "name": "NameWrapper" },
        "minters": [
          "0x09ce2896b24e60cb3de34e2b826c2ef4545a7566",
          "0x95b7ce09add500052386318863d166326220d8e9",
          "0x575f10e1fe5f1ffa9d8b888b55bf59c7b8c01fa0",
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe",
          "0x9f06bbd82b7a26272d1eafa118cda2819b22d793",
          "0xeab05f8e538982516d119361d7e6c31e8fb6f7c8",
          "0x1d9a9b5e73259bbf0272c5228330ffd24bca82c0",
          "0x8135b0dd3eb53c1c391f4b228824ea60291793c9"
        ],
        "frequency": 10
      }
      // Other associated token mints, if any
    ],
    "score": 100
  },
  {
    "tokenAddress": "0xad08067c7d3d3dbc14a9df8d671ff2565fc5a1ae",
    "token": { "name": "NFT.TD" },
    "chain": "ethereum",
    "associatedMints": [
      {
        "tokenAddress": "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D",
        "token": { "name": "BoredApeYachtClub" },
        "minters": [
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe"
        ],
        "frequency": 5
      }
    ],
    "score": 15
  }
  // Other scored and sorted token mints data
]
```

### Filtering

As the last step, you can then filter the scored and sorted mints data to determine which one would qualify as "trending mints" to be notified/shown to the user.

In this tutorial, you'll be using a very simple filtering function `filterFunction` that will filter out any result that have score below or equal to the `threshold` variable that you can set for the user:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/filter.ts" %}
```typescript
import { TokenTransferWithScore } from "./scoring";

/**
 * @description
 * Filter mints data by `score` field that reaches
 * certain `threshold` that would classify as trending mints
 *
 * @example
 * const res = filterFunction(sortedData, 50);
 *
 * @param {Object} data – Scored & sorted tokens data with `score` field
 * @returns list of trending mints
 */
const filterFunction = (data: TokenTransferWithScore, threshold: number) =>
  data?.filter((val) => val?.score >= threshold);

export default filterFunction;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="utils/fitler.js" %}
```javascript
/**
 * @description
 * Filter mints data by `score` field that reaches
 * certain `threshold` that would classify as trending mints
 *
 * @example
 * const res = filterFunction(sortedData, 50);
 *
 * @param {Object} data – Scored & sorted tokens data with `score` field
 * @returns list of trending mints
 */
const filterFunction = (data, threshold) =>
  data?.filter((val) => val?.score >= threshold);

export default filterFunction;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="utils/filter.py" %}
```python
from typing import List, Dict, Any


def filter_function(data: List[Dict[str, Any]], threshold: int) -> List[Dict[str, Any]]:
  """
  Filter mints data by `score` field that reaches a certain `threshold`
  that would classify as trending mints.

  :param data: Scored & sorted tokens data with `score` field
  :param threshold: The threshold score to filter the trending mints
  :return: List of trending mints
  """
  if data is None:
    return []

  return [val for val in data if val.get('score', 0) >= threshold]
```
{% endcode %}
{% endtab %}
{% endtabs %}

Then, you can import the `filterFunction` back to `main` to have the sorted and scored data filtered:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript" data-title="index.ts"><code class="lang-typescript">// same imports as above
import filterFunction from "./utils/filter";

const main = (user: string, currentTime: Dayjs) = > {
  // same as above
<strong>  const filteredData = filterFunction(sortedData, 50); // Only output result with score above 50
</strong>  return filteredData;
}

export default main;
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript" data-title=""><code class="lang-javascript">// same imports as above
import filterFunction from "./utils/filter";

const main = () = > {
  // same as above
<strong>  const filteredData = filterFunction(sortedData); // filter only result above 50
</strong>  return filteredData;
}

export default main;
</code></pre>
{% endtab %}

{% tab title="Python" %}
{% code title="index.py" %}
```python
# same imports as above
from utils.filter import filter_function

async def main(current_time: datetime) -> List[Dict[str, Any]]:
  # same as above
  filtered_data = filter_function(sorted_data, 50) # Only output result with score above 50
  return filtered_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

When the result of the newly modified `main` function is logged, it will have result that look as follows:

```json
[
  {
    "tokenAddress": "0xb63056fc3dab4f755d4d0380cf36e67c1164da64",
    "token": { "name": "Polysapien Open Edition" },
    "chain": "polygon",
    "associatedMints": [
      {
        "tokenAddress": "0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401",
        "token": { "name": "NameWrapper" },
        "minters": [
          "0x09ce2896b24e60cb3de34e2b826c2ef4545a7566",
          "0x95b7ce09add500052386318863d166326220d8e9",
          "0x575f10e1fe5f1ffa9d8b888b55bf59c7b8c01fa0",
          "0xf0bf94342c1c77c17575ac72b7c10d03c8e23d8a",
          "0xd50963c3f3dce3bf7449116062a6b3234240a366",
          "0xc38c8027fa54bfc3e8f093a7994c3c5f43f416fe",
          "0x9f06bbd82b7a26272d1eafa118cda2819b22d793",
          "0xeab05f8e538982516d119361d7e6c31e8fb6f7c8",
          "0x1d9a9b5e73259bbf0272c5228330ffd24bca82c0",
          "0x8135b0dd3eb53c1c391f4b228824ea60291793c9"
        ],
        "frequency": 10
      }
      // Other associated token mints, if any
    ],
    "score": 100
  }
]
```

## Step 3: Run as a Cronjob

With the code from above, now you can run this periodically non stop as a cron to fetch all the recent trending mints to be notified or recommended to your user.

From your user perspective, they will experience the feature in either **user interface** or **push notifications**.

### User Interface

For displaying all the trending token mints to your interface, it is best practice that you store the recent trending mints data from in your preferred database.

{% tabs %}
{% tab title="TypeScript" %}
{% code title="cron.ts" %}
```typescript
import cron from "node-cron";
import dayjs from "dayjs";
import main from "./main";

cron.schedule("0 * * * *", async () => {
  const currentTime = dayjs();
  const data = await main(user, currentTime);
  // Store `data` to your preferred DB
});
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="cron.js" %}
```javascript
import cron from "node-cron";
import dayjs from "dayjs";
import main from "./main";

cron.schedule("0 * * * *", async () => {
  const currentTime = dayjs();
  const data = await main(user, currentTime);
  // Store `data` to your preferred DB
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="cron.py" %}
```python
import pycron
from datetime import datetime
from index import main

@pycron.cron("* * * * *")
async def cron(current_time: datetime):
  print("Running cron job...")
  data = await main(user, current_time)
  # Store `data` to your preferred DB

if __name__ == '__main__':
  print("Starting cron job...")
  pycron.start()
```
{% endcode %}
{% endtab %}
{% endtabs %}

From there, you can fetch trending mints data from database directly through your application's frontend or backend to be served to your users' client.

### Push Notifications

For push notification, you simply need to push the message to your client using the information fetched from the Airstack API through the cron and will not require any additional storage:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="cron.ts" %}
```typescript
import cron from "node-cron";
import dayjs from "dayjs";
import main from "./main";
import { interval } from "./constant";

cron.schedule("0 * * * *", () => {
  const currentTime = dayjs();
  const [
    trendingMint1,
    trendingMint2,
    // Other trending mints in the array
  ] = await main(user, currentTime);
  const { token, associatedMints, score } = trendingMint1 ?? {};
  const { minters, token: associatedToken } = associatedMints?.[0];
  const message = `${minters?.length} user that minted ${associatedToken?.name} before have now also minted ${token?.name} in the last ${interval} minutes`;
  // Here make API call with `message` to the push service to notify
  // your app's client
});
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="cron.js" %}
```javascript
import cron from "node-cron";
import dayjs from "dayjs";
import main from "./main";
import { interval } from "./constant";

cron.schedule('0 * * * *', () => {
  const currentTime = dayjs();
  const [
    trendingMint1,
    trendingMint2,
    // Other trending mints in the array
  ] = await main(user, currentTime);
  const { token, associatedMints, score } = trendingMint1 ?? {};
  const { minters, token: associatedToken } = associatedMints?.[0];
  const message =
    `${minters?.length} user that minted ${associatedToken?.name} before have now also minted ${token?.name} in the last ${interval} minutes`;
  // Here make API call with `message` to the push service to notify
  // your app's client
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="cron.py" %}
```python
import pycron
from datetime import datetime
from index import main
from constant import interval

@pycron.cron("* * * * *")
async def cron(current_time: datetime):
  print("Running cron job...")
  data = await main(user, current_time)
  token = data[0].get("token", {})
  associated_mints = data[0].get("associated_mints", [])
  score = data[0].get("score", 0)
  minters = associated_mints[0].get("minters", [])
  associated_tokens = associated_mints[0].get("token", {})
  message =
    f"{len(minters)} user that minted {associatedToken.get('name', '')} before have now also minted {token.get('name', '')} in the last {interval} minutes"
  # Here make API call with `message` to the push service to notify
  # your app's client

if __name__ == '__main__':
  print("Starting cron job...")
  pycron.start()
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Developer Support

🎉 :partying\_face: Congratulations you've just integrated trending mints feature based on your user's common minters into your application!

If you have any questions or need help regarding integrating or building trending mints into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Global Trending Mints](global.md)
* [Trending Mints By Onchain Graph](../onchain-graph.md)
* [Token Mints](../token-mints.md)
* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
