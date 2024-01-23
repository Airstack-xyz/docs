---
description: >-
  Learn how to recommend  Mints that are trending amongst the user's onchain
  connections (social follows, POAPs in common, NFTs in common, token transfers,
  and more).
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

# 🕸 Onchain Graph

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Trending Mints By Onchain Graph Mock Up</p></figcaption></figure>

## Table Of Contents

In this tutorial, you'll learn how to build trending mints based on your user's onchain graph for your web3 application using either JavaScript/TypeScript or Python:

{% hint style="info" %}
Currently, there is no dedicated backend API for fetching trending mints directly into your application. Therefore, the following implementation will require you to run a **backend**.

\
For the **backend**,  you will be required to run a cronjob to fetch token mints data from the Airstack API and have the data stored in a dedicated database.\
\
This turotial will walk you through the steps required to implement Trending Mints today and deliver immediate value to your users.\


Concurrently Airstack is working on a dedicated Trending Mints API for lighter-weight integrations in the near future.
{% endhint %}

1. [Fetch All Recent Token Mints Minted By Onchain Graph Users](onchain-graph.md#step-1-fetch-all-recent-token-mints-minted-by-onchain-graph-users)
2. [Score, Sort, and Filter Token Mints](onchain-graph.md#step-2-score-sort-and-filter-token-mints)
3. [Run as a Cronjob](onchain-graph.md#step-3-run-as-a-cronjob)

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

## Step 1: Fetch All Recent Token Mints Minted By Onchain Graph Users

First, define the following parameters to fetch the token mints data:

#### **Intervals**

The interval that you would like to run your cron job. Using the interval, you can then set the variables for the query that will be shown below:

* `endTime` to the current unix timestamp
* `startTime` to the current unix timestamp minus the chosen interval duration.

In this tutorial, we'll use 1 hour as the default interval.

#### **Token Types**

Input all the token types that you would like to fetch from the mints data.

If you only prefer fungible token mints, then includes only `ERC20`. If you instead want to enable NFT mints only, then include both `ERC721` and `ERC1155`.

#### **Chains**

Choose the chain that you would like to fetch the token mints data.

Currently, Airstack supports Ethereum, Polygon, and Base. Zora will be supported very soon.

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
export const chains = ["ethereum", "polygon", "base"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="constant.js" %}
```javascript
export const interval = 1; // 1 hour
export const tokenType = ["ERC20", "ERC721", "ERC1155"];
export const chains = ["ethereum", "polygon", "base"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="constant.py" %}
```python
interval = 1 # 1 hour
token_type = ["ERC20", "ERC721", "ERC1155"]
chains = ["ethereum", "polygon", "base"]
limit = 200
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Fetching

As defined in the pre-requisites, you are **expected** to have a working implementation on onchain graph. If you have not, please follow [this tutorial](../onchain-graph.md) before continuing.

Assuming that you have the onchain graph setup and stored on your backend, you can fetch the full list of the onchain graph users' addresses to be provided as a variable to the query that will be shown next.

Along with the defined parameters, you can use [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API to construct an Airstack query to fetch all recent tokens minted by all the **onchain graph users** of a given user in a certain interval period:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Rg8XFNEPS2" %}
Show me minted tokens on Polygon by an onchain graph user at certain timestamp
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
  $onchainGraphUser: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers that are mints
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        # Ensuring that all token mints amount are non-zero
<strong>        formattedAmount: {_gt: 0},
</strong>        to: {_eq: $onchainGraphUser},
        operator: {_eq: $onchainGraphUser},
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
  "onchainGraphUser": "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab"
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

To fetch all the data, the query will be iterated multiple times using the [`fetchQueryWithPagination`](../../nodejs-sdk-reference/fetchquerywithpagination.md) function provided by the Airstack SDK to iterate over all the blockchains and the paginations.

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript" data-title="index.ts"><code class="lang-typescript">import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import {
  interval,
  tokenType,
  chains,
  limit
} from "./constant";
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
  $onchainGraphUser: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $onchainGraphUser},
        operator: {_eq: $onchainGraphUser},
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
  /**
   * You should fetch onchain graph users based on the `user` variable
   * with their onchain score from your DB here.
   * 
   * For this tutorial, this will simply user a constant variable.
   */
<strong>  const onchainGraphUsers = [
</strong>    {
      address: "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab",
      onchainScore: 96,
    },
    {
      address: "0xf6b6f07862a02c85628b3a9688beae07fea9c863",
      onchainScore: 64,
    },
    {
      address: "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879",
      onchainScore: 60,
    },
    // Other onchain graph users
<strong>  ];
</strong>  // Iterate over all onchain graph users
  for (let onchainGraphUser of onchainGraphUsers) {
    const { address, onchainScore } = onchainGraphUser ?? {};
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
            onchainGraphUser,
          });
        }

        const { data, error, getNextPage, hasNextPage } = response ?? {};
        if (!error) {
          // aggregate all the token mints to `mintsData` variable
          mintsData = [
            ...mintsData,
            ...(data?.TokenTransfers?.TokenTransfer?.map((mint) => ({
              ...mint,
              minter: {
                address,
                onchainScore,
              },
              chain,
            })) ?? []),
          ];
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
  
  return mintsData;
}

export default main;
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript

import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";

config();

init(process.env.AIRSTACK_API_KEY);

const query = `
query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int,
  $onchainGraphUser: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $onchainGraphUser},
        operator: {_eq: $onchainGraphUser},
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
  /**
   * You should fetch onchain graph users based on the `user` variable
   * with their onchain score from your DB here.
   * 
   * For this tutorial, this will simply user a constant variable.
   */
  const onchainGraphUsers = [
    {
      address: "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab",
      onchainScore: 96,
    },
    {
      address: "0xf6b6f07862a02c85628b3a9688beae07fea9c863",
      onchainScore: 64,
    },
    {
      address: "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879",
      onchainScore: 60,
    },
    // Other onchain graph users
  ];
  // Iterate over all onchain graph users
  for (let onchainGraphUser of onchainGraphUsers) {
    const { address, onchainScore } = onchainGraphUser ?? {};
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
            onchainGraphUser,
          });
        }

        const { data, error, getNextPage, hasNextPage } = response ?? {};
        if (!error) {
          mintsData = [
            ...mintsData,
            ...(data?.TokenTransfers?.TokenTransfer?.map((mint) => ({
              ...mint,
              minter: {
                address,
                onchainScore,
              },
              chain,
            })) ?? []),
          ];
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
  
  return mintsData;
}

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
from utils.format import format_function
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
  $onchainGraphUser: Identity
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        formattedAmount: {_gt: 0},
        to: {_eq: $onchainGraphUser},
        operator: {_eq: $onchainGraphUser},
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
  """
  You should fetch onchain graph users based on the `user` variable
  with their onchain score from your DB here.
  
  For this tutorial, this will simply user a constant variable.
  """
  onchainGraphUsers = [
    {
      address: "0xb59aa5bb9270d44be3fa9b6d67520a2d28cf80ab",
      onchainScore: 96,
    },
    {
      address: "0xf6b6f07862a02c85628b3a9688beae07fea9c863",
      onchainScore: 64,
    },
    {
      address: "0xcbfbcbfca74955b8ab75dec41f7b9ef36f329879",
      onchainScore: 60,
    },
    # Other onchain graph users
  ];
  # Iterate over all on chain graph users
  for onchainGraphUser in onchainGraphUsers:
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
            "onchainGraphUser": onchainGraphUser.get("address", "")
          })
          query_response = await execute_query_client.execute_paginated_query()
          if query_response.error is None:
            data = query_response.data
            if data and 'TokenTransfers' in data and data['TokenTransfers'] is not None:
              token_transfers = data['TokenTransfers'].get(
                'TokenTransfer')
              if token_transfers:
                for mint in token_transfers:
                  new_mint = mint.copy()
                  new_mint['minter'] = {
                    'address': onchainGraphUser.get("address", ""),
                    'onchainScore': onchainGraphUser.get("onchainScore", "")
                  }
                  new_mint['chain'] = chain
                  mints_data.append(new_mint)
                        
            # Iterate over all paginations
            if not query_response.has_next_page:
              break
            else:
              query_response = await query_response.get_next_page
  
      # Resetting the loop
      query_response = None

  return mints_data
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Step 2: Score, Sort, and Filter Token Mints

After fetching all the raw token mints data from [Airstack](https://airstack.xyz) API, next you can process the data to determine which will qualify as trending mints.

In this tutorial, there will be 3 steps to process the token mints data:

1. [Scoring](onchain-graph.md#scoring) – assigning a score to each minted tokens to determine how "popular" each tokens are in a certain period of time
2. [Sorting](onchain-graph.md#sorting) – sort the minted tokens data by the score, in descending order
3. [Filtering](onchain-graph.md#filtering) – filter out all non-trending tokens that does not qualify

These procedures are **NOT** a strict requirement and can be defined by yourself depending on the requirements you have for building the feature into application.

### Scoring

In this tutorial, we'll define the scoring function for a minted token to be **the sum of the multiplication of the minter's onchain graph score and a token's mint frequency by the minter** in the defined interval:

{% hint style="info" %}
You are not required to follow the scoring logic shown in this tutorial. Depending on your use cases, you are free to create your own custom scoring function or skip the scoring step all together if unnecessary.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/scoring.ts" %}
```typescript
export interface Data {
  TokenTransfers: TokenTransfer;
}

export interface TokenTransfer {
  TokenTransfer: TokenTransferDetails[];
}

export interface TokenTransferDetails {
  tokenAddress: string;
  operator: Identity;
  to: Identity;
  token: Token;
}

export interface Identity {
  addresses: string;
}

export interface Token {
  name: string;
}

/**
 * @description
 * Score all recent token mint data by how much people mint the token.
 * Each minting represent a score of 1.
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
    const { tokenAddress, chain, minter } = val ?? {};
    const { address, onchainScore } = minter ?? {};
    const valIndex = trendingMints.findIndex((value) => {
      return value?.tokenAddress === tokenAddress && value?.chain === chain;
    });
    if (valIndex !== -1) {
      const minterIndex = trendingMints?.[valIndex]?.minters?.findIndex(
        (minter) => {
          return (
            minter?.address === address && minter?.onchainScore === onchainScore
          );
        }
      );
      trendingMints[valIndex] = {
        ...trendingMints[valIndex],
        // For each new mints, add score of the minter's onchain score
        score: trendingMints[valIndex]?.score + onchainScore,
        minters: [
          ...trendingMints[valIndex]?.minters,
          // only add the minter to the list it is a new minter
          minterIndex !== -1 ? null : minter,
        ]?.filter(Boolean),
      };
    } else {
      delete val?.minter;
      // For new token mints, assigned initial score of the minter's onchain score
      trendingMints.push({ ...val, score: onchainScore, minters: [minter] });
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
 * Score all recent token mint data by how much people mint the token.
 * Each minting represent a score of 1.
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
    const { tokenAddress, chain, minter } = val ?? {};
    const { address, onchainScore } = minter ?? {};
    const valIndex = trendingMints.findIndex((value) => {
      return value?.tokenAddress === tokenAddress && value?.chain === chain;
    });
    if (valIndex !== -1) {
      const minterIndex = trendingMints?.[valIndex]?.minters?.findIndex(
        (minter) => {
          return (
            minter?.address === address && minter?.onchainScore === onchainScore
          );
        }
      );
      trendingMints[valIndex] = {
        ...trendingMints[valIndex],
        // For each new mints, add score of the minter's onchain score
        score: trendingMints[valIndex]?.score + onchainScore,
        minters: [
          ...trendingMints[valIndex]?.minters,
          // only add the minter to the list it is a new minter
          minterIndex !== -1 ? null : minter,
        ]?.filter(Boolean),
      };
    } else {
      delete val?.minter;
      // For new token mints, assigned initial score of the minter's onchain score
      trendingMints.push({ ...val, score: onchainScore, minters: [minter] });
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
def scoring_function(data):
    """
    Score all recent token mint data by how much people mint the token.
    Each minting represents a score of 1.

    :param data: Formatted minted tokens data from Airstack API
    :return: Scored mint data
    """
    trending_mints = []

    for val in data:
        token_address = val.get('tokenAddress')
        chain = val.get('chain')
        minter = val.get('minter', {})
        address = minter.get('address')
        onchain_score = minter.get('onchainScore', 0)

        val_index = next((index for (index, value) in enumerate(trending_mints)
                          if value.get('tokenAddress') == token_address and value.get('chain') == chain), -1)

        if val_index != -1:
            minter_index = next((index for (index, m) in enumerate(trending_mints[val_index].get('minters', []))
                                 if m.get('address') == address and m.get('onchainScore') == onchain_score), -1)

            trending_mints[val_index]['score'] += onchain_score
            if minter_index == -1:
                trending_mints[val_index]['minters'].append(minter)
        else:
            val.pop('minter', None)
            trending_mints.append({**val, 'score': onchain_score, 'minters': [minter]})

    # Filter out None values in minters
    for item in trending_mints:
        item['minters'] = [m for m in item['minters'] if m]

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
    "tokenAddress": "0xd4416b13d2b3a9abae7acd5d6c2bbdbe25686401",
    "token": { "name": "NameWrapper" },
    "chain": "ethereum",
    "score": 52,
    "minters": [
      {
        "address": "0x171dd9a138b796e3b307086b136a810bae44a185",
        "onchainScore": 52
      }
    ]
  },
  {
    "tokenAddress": "0x9a74559843f7721f69651eca916b780ef78bd060",
    "token": { "name": "Poglin: Battle For Havens Destiny" },
    "chain": "ethereum",
    "score": 52,
    "minters": [
      {
        "address": "0x171dd9a138b796e3b307086b136a810bae44a185",
        "onchainScore": 52
      }
    ]
  },
  // Other scored token mints data
]
```

### Sorting

Once you have the token mints data scored, you can implement a very simple sorting function that sorts in descending order based on the `score` field value:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/sorting.ts" %}
```typescript
import { TokenTransferDetails } from "./scoring";

export interface TokenTransferWithScore extends TokenTransferDetails {
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
    "tokenAddress": "0xc347075b60ff7f07eea970636ea9a8f95d7e7da9",
    "token": { "name": "Shadowink" },
    "chain": "ethereum",
    "score": 510,
    "minters": [
      {
        "address": "0xc94327cbb9eef801a4bbe3459766bb712c5ad979",
        "onchainScore": 51
      }
    ]
  },
  {
    "tokenAddress": "0xdda213a564ec3ba7a6e82c529ddd1aa37b4d0fb4",
    "token": { "name": "Moonie Punks" },
    "chain": "ethereum",
    "score": 459,
    "minters": [
      {
        "address": "0xc94327cbb9eef801a4bbe3459766bb712c5ad979",
        "onchainScore": 51
      }
    ]
  },
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
const filterFunction = (
  data: TokenTransferWithScore,
  threshold: number
) => data?.filter((val) => val?.score >= threshold);

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
    "tokenAddress": "0xc347075b60ff7f07eea970636ea9a8f95d7e7da9",
    "token": { "name": "Shadowink" },
    "chain": "ethereum",
    "score": 510,
    "minters": [
      {
        "address": "0xc94327cbb9eef801a4bbe3459766bb712c5ad979",
        "onchainScore": 51
      }
    ]
  },
  {
    "tokenAddress": "0xdda213a564ec3ba7a6e82c529ddd1aa37b4d0fb4",
    "token": { "name": "Moonie Punks" },
    "chain": "ethereum",
    "score": 459,
    "minters": [
      {
        "address": "0xc94327cbb9eef801a4bbe3459766bb712c5ad979",
        "onchainScore": 51
      }
    ]
  },
  // Other scored, sorted, and filtered token mints data by onchain graph users
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

cron.schedule('0 * * * *', async () => {
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

cron.schedule('0 * * * *', async () => {
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

cron.schedule('0 * * * *', () => {
  const currentTime = dayjs();
  const [
    trendingMint1,
    trendingMint2,
    // Other trending mints in the array
  ] = await main(user, currentTime);
  const { token, minters, score } = trendingMint1 ?? {};
  const message = 
    `${minters?.[0]?.address} and ${minters?.length - 2} have minted ${token?.name} in the last ${interval} minutes`;
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
  const { token, score } = trendingMint1 ?? {};
  const message = 
    `${minters?.[0]?.address} and ${minters?.length - 2} have minted ${token?.name} in the last ${interval} minutes`;
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

@pycron.cron("* * * * *")
async def cron(current_time: datetime):
  print("Running cron job...")
  data = await main(user, current_time)
  token = data[0].get("token", {})
  score = data[0].get("score", 0)
  message =
    f"{token.get('name', '')} have been minted more than {score} times the last {interval} minutes";
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

🎉 :partying\_face: Congratulations you've just integrated trending mints feature based on your user's onchain graph into your application!

If you have any questions or need help regarding integrating or building trending mints into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Global Trending Mints](global.md)
* [Trending Mints With Common Minters](common-minters.md)
* [Token Mints](../token-mints.md)
* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
