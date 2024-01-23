---
description: Learn how to recommend  Mints that are trending globally.
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

# 🌎 Global

<div data-full-width="true">

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Global Trending Mints Mock Up</p></figcaption></figure>

</div>

## Table Of Contents

In this tutorial, you'll learn how to build trending mints for your web3 application using either JavaScript/TypeScript or Python:

{% hint style="info" %}
Currently, there is no dedicated backend API for fetching trending mints directly into your application. Therefore, the following implementation will require you to run a **backend**.

\
For the **backend**,  you will be required to run a cronjob to fetch token mints data from the Airstack API and have the data stored in a dedicated database.\
\
This turotial will walk you through the steps required to implement Trending Mints today and deliver immediate value to your users.\


Concurrently Airstack is working on a dedicated Trending Mints API for lighter-weight integrations in the near future.
{% endhint %}

The algorithm for building trending mints will be as follows:

1. [Fetch All Recent Token Mints](global.md#step-1-fetch-all-recent-token-mints)
2. [Score, Sort, and Filter Token Mints](global.md#step-2-score-sort-and-filter-token-mints)
3. [Run as a Cronjob](global.md#step-3-run-as-a-cronjob)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

To get started, install the Airstack SDK and other necessary 3rd party packages:

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

## Step 1: Fetch All Recent Token Mints

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

The number of JSON object returned in responses per API call, with a maximum allowable value of **200**.

***

As these parameters are going to be having constant values, you can create a new file to store these as constant variables that can be imported in the next steps:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="constant.ts" %}
```typescript
export const interval = 1; // Every 1 hour
export const tokenType = ["ERC20", "ERC721", "ERC1155"];
export const chains = ["ethereum", "polygon", "base"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="constant.js" %}
```javascript
export const interval = 1; // Every 1 hour
export const tokenType = ["ERC20", "ERC721", "ERC1155"];
export const chains = ["ethereum", "polygon", "base"];
export const limit = 200;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="constant.py" %}
```python
interval = 1 # Every 1 hr
token_type = ["ERC20", "ERC721", "ERC1155"]
chains = ["ethereum", "polygon", "base"]
limit = 200
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Fetching

Using the defined parameters, construct an Airstack query to fetch all recent tokens minted in a certain interval period, e.g.  January 1 to 7, 2024:

### Try Demo

{% embed url="https://app.airstack.xyz/query/84msEupmWo" %}
Show me all recently minted tokens on Ethereum from Jan 1 to 7
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int
) {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers that are mints
<strong>        from: {_eq: "0x0000000000000000000000000000000000000000"},
</strong>        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
        # Ensuring that all token mints amount are non-zero
<strong>        formattedAmount: {_gt: 0}
</strong>      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      operator {
        addresses
      }
      to {
        addresses
      }
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
  "startTime": "2024-01-01T00:00:00Z",
  "endTime": "2024-01-07T23:59:59Z",
  "tokenType": ["ERC20", "ERC721", "ERC1155"],
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
          "tokenAddress": "0x63b420fb3294ba1d300ce5d3ba4bbca0f4fe5e3b",
          "operator": {
            "addresses": [
              "0x4b9c19ae685c58b535a1bab0fdd925875e71ec50"
            ]
          },
          "to": {
            "addresses": [
<strong>              "0x4b9c19ae685c58b535a1bab0fdd925875e71ec50" // receiver of the 
</strong>            ]
          }
          "token": {
            "name": "Scramble Finance"
          }
        },
        {
          "tokenAddress": "0xc771aa9f1c8d83d185498b03dae3053256cf5b90",
          "operator": {
            "addresses": [
              "0x00d5ad0d58d57e07de36da33481aa85d9bfb4128"
            ]
          },
          "to": {
            "addresses": [
              "0x00d5ad0d58d57e07de36da33481aa85d9bfb4128"
            ]
          },
          "token": {
            "name": "Milady Meals"
          }
        },
        // Other recently minted NFTs
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Formatting

Once you fetch the token mints data, you will need to next format the data to remove all **airdropped mints**.

Those are mints that have different `operator.addresses` (executor of the transaction) to the value of `to.addresses` (token receiver):

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/format.ts" %}
```typescript
export interface Data {
  TokenTransfers: TokenTransfer[];
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
 * @description Remove all airdrop minted tokens from the API result
 * @examples
 * const { data } = await fetchQuery(query, variables);
 * formatFunction(data);
 *
 * @param {Object} data - Recently minted tokens from Airstack API
 * @returns All recently minted tokens, excluding all airdropped mints
 */
const formatFunction = (data: Data) =>
  data?.TokenTransfers?.TokenTransfer?.map((transfer: TokenTransfer) => {
    const { operator, to } = transfer ?? {};
    const isMint = operator?.addresses?.[0] === to?.addresses?.[0];
    delete transfer?.operator;
    delete transfer?.to;
    // excluding all airdrop mints (operator != to)
    return isMint ? transfer : null;
  })?.filter(Boolean);
  
export default formatFunction;
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="utils/format.js" %}
```javascript
/**
 * @description Remove all airdrop minted tokens from the API result
 * @examples
 * const { data } = await fetchQuery(query, variables);
 * formatFunction(data);
 *
 * @param {Object} data - Recently minted tokens from Airstack API
 * @returns All recently minted tokens, excluding all airdropped mints
 */
const formatFunction = (data) =>
  data?.TokenTransfers?.TokenTransfer?.map((transfer) => {
    const { operator, to } = transfer ?? {};
    const isMint = operator?.addresses?.[0] === to?.addresses?.[0];
    delete transfer?.operator;
    delete transfer?.to;
    // excluding all airdrop mints (operator != to)
    return isMint ? transfer : null;
  })?.filter(Boolean);
  
export default formatFunction;
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
```python
from typing import List, Optional


class Token:
    def __init__(self, name: str):
        self.name = name


class Identity:
    def __init__(self, addresses: str):
        self.addresses = addresses


class TokenTransfer:
    def __init__(self, tokenAddress: str, operator: Identity, to: Identity, token: Token):
        self.tokenAddress = tokenAddress
        self.operator = operator
        self.to = to
        self.token = token


class Data:
    def __init__(self, TokenTransfers: List[TokenTransfer]):
        self.TokenTransfers = TokenTransfers

def format_function(data: Data) -> List[Optional[TokenTransfer]]:
    """
    Remove all airdrop minted tokens from the API result.

    This function processes the Data object to filter out and exclude all airdrop mints
    where the operator is not the same as the recipient.

    :param data: Recently minted tokens from an API
    :return: A list of TokenTransfer objects, excluding all airdrop mints
    """
    result = []
    token_transfers = data.get('TokenTransfers', {}).get('TokenTransfer', [])

    if token_transfers is not None:
        for transfer in token_transfers:
            operator_address = transfer.get('operator', {}).get('addresses')
            to_address = transfer.get('to', {}).get('addresses')
            if operator_address and to_address and operator_address == to_address:
                # Excluding all airdrop mints (operator != to)
                transfer.pop('operator', None)
                transfer.pop('to', None)
                result.append(transfer)

    return result
```
{% endtab %}
{% endtabs %}

### Iterate

Once you have the query and its formatting function ready, you can combine them in one `main` function to be executed in a single flow.

To fetch all the data, the query will be iterated multiple times using the [`fetchQueryWithPagination`](../../nodejs-sdk-reference/fetchquerywithpagination.md) function provided by the Airstack SDK to iterate over all the blockchains and the paginations.

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { init, fetchQueryWithPagination } from "@airstack/node";
import { config } from "dotenv";
import formatFunction from "./utils/format";
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
  $limit: Int
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      operator {
        addresses
      }
      to {
        addresses
      }
      token {
        name
      }
    }
  }
}
`;

const main = async (currentTime: Dayjs) => {
  let response;
  let mintsData = [];
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
        });
      }

      const { data, error, getNextPage, hasNextPage } = response ?? {};
      if (!error) {
        mintsData = [
          ...mintsData,
          ...(formatFunction(data)?.map((mint) => ({
            ...mint,
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
import formatFunction from "./utils/format";
import {
  interval,
  tokenType,
  chains,
  limit
} from "./constant";
import dayjs from "dayjs";

config();

init(process.env.AIRSTACK_API_KEY);

const query = `
query MyQuery(
  $startTime: Time,
  $endTime: Time,
  $tokenType: [TokenType!],
  $chain: TokenBlockchain!,
  $limit: Int
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      operator {
        addresses
      }
      to {
        addresses
      }
      token {
        name
      }
    }
  }
}
`;

const main = async (currentTime) => {
  let response;
  let mintsData = [];
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
        });
      }

      const { data, error, getNextPage, hasNextPage } = response ?? {};
      if (!error) {
        mintsData = [
          ...mintsData,
          ...(formatFunction(data)?.map((mint) => ({
            ...mint,
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
  $limit: Int
) {
  TokenTransfers(
    input: {
      filter: {
        from: {_eq: "0x0000000000000000000000000000000000000000"},
        blockTimestamp: {_gte: $startTime, _lte: $endTime},
        tokenType: {_in: $tokenType},
      }
      blockchain: $chain,
      order: {blockTimestamp: DESC},
      limit: $limit
    }
  ) {
    TokenTransfer {
      tokenAddress
      operator {
        addresses
      }
      to {
        addresses
      }
      token {
        name
      }
    }
  }
}
"""

async def main(current_time: datetime) -> List[Dict[str, Any]]:
  query_response = None
  mints_data = []
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
        })
        query_response = await execute_query_client.execute_paginated_query()
        if query_response.error is None:
          formatted_data = format_function(query_response.data)
          if formatted_data is not None:
            for mint in formatted_data:
              mint.update({'chain': chain})
            mints_data.extend(formatted_data)
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

1. [Scoring](global.md#scoring) – assigning a score to each minted tokens to determine how "popular" each tokens are in a certain period of time
2. [Sorting](global.md#sorting) – sort the minted tokens data by the score, in descending order
3. [Filtering](global.md#filtering) – filter out all non-trending tokens that does not qualify

These procedures are **NOT** a strict requirement and can be defined by yourself depending on the requirements you have for building the feature into application.

### Scoring

In this tutorial, we'll define the scoring function for a minted token to be **the sum of a token's mint frequency** in the defined interval, with each mint carrying the score of 1:

{% hint style="info" %}
You are not required to follow the scoring logic shown in this tutorial. Depending on your use cases, you are free to create your own custom scoring function or skip the scoring step all together if unnecessary.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
{% code title="utils/scoring.ts" %}
```typescript
import { TokenTransferDetails } from "./format";

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
const scoringFunction = (data: TokenTransferDetails[]) => {
  let trendingMints = [];

  data.forEach((val) => {
    const { tokenAddress, chain } = val ?? {};
    const valIndex = trendingMints.findIndex((value) => {
      return value?.tokenAddress === tokenAddress && value?.chain === chain;
    });
    if (valIndex !== -1) {
      trendingMints[valIndex] = {
        ...trendingMints[valIndex],
        // For each new mints, add score of 1
        score: trendingMints[valIndex]?.score + 1,
      };
    } else {
      // For new token mints, assigned initial score of 1
      trendingMints.push({ ...val, score: 1 });
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
    Score and sort recent trending mint data by how much people mint the token.

    :param data: Recently minted tokens data from Airstack API
    :return: Scored and sorted mint data, showing recent trending mints
    """
    trending_mints = []
    for val in data:
        token_address = val.get('tokenAddress')
        chain = val.get('chain')
        val_index = next((i for i, v in enumerate(trending_mints) if v.get('tokenAddress') == token_address and v.get('chain') == chain), -1)

        if val_index != -1:
            trending_mints[val_index]['score'] += 1
        else:
            trending_mints.append({**val, 'score': 1})

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

const main = (currentTime: Dayjs) = > {
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
    "tokenAddress": "0x63b420fb3294ba1d300ce5d3ba4bbca0f4fe5e3b",
    "token": { "name": "Scramble Finance" },
    "chain": "ethereum",
    "score": 5
  },
  {
    "tokenAddress": "0xf19308f923582a6f7c465e5ce7a9dc1bec6665b1",
    "token": { "name": "TITAN X" },
    "chain": "ethereum",
    "score": 11
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
import { TokenTransferDetails } from "./format";

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

const main = (currentTime: Dayjs) = > {
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
    "tokenAddress": "0x8746e359a61f0fc0c8f750cf1a07f3129a53edfb",
    "token": { "name": "Rarity Jump Genesis" },
    "chain": "ethereum",
    "score": 73
  },
  {
    "tokenAddress": "0xe333cd2f6e26a949ce1f3fb15d7bfac2871cc9e4",
    "token": { "name": "Crazy frens V2" },
    "chain": "ethereum",
    "score": 35
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

const main = (currentTime: Dayjs) = > {
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
<strong>  const filteredData = filterFunction(sortedData, 50); // Only output result with score above 50
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
    "tokenAddress": "0x8746e359a61f0fc0c8f750cf1a07f3129a53edfb",
    "token": { "name": "Rarity Jump Genesis" },
    "chain": "ethereum",
    "score": 73
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
import main from "./index";

cron.schedule('0 * * * *', async () => {
  const currentTime = dayjs();
  const data = await main(currentTime);
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
import main from "./index";

cron.schedule('0 * * * *', async () => {
  const currentTime = dayjs();
  const data = await main(currentTime);
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
  data = await main(current_time)
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
import main from "./index";
import { interval } from "./constant";

cron.schedule('0 * * * *', () => {
  const currentTime = dayjs();
  const [
    trendingMint1,
    trendingMint2,
    // Other trending mints in the array
  ] = await main(currentTime);
  const { token, score } = trendingMint1 ?? {};
  const message = 
    `${token?.name} have been minted more than ${score} times the last ${interval} minutes`;
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
import main from "./index";
import { interval } from "./constant";

cron.schedule('0 * * * *', () => {
  const currentTime = dayjs();
  const [
    trendingMint1,
    trendingMint2,
    // Other trending mints in the array
  ] = await main(currentTime);
  const { token, score } = trendingMint1 ?? {};
  const message = 
    `${token?.name} have been minted more than ${score} times the last ${interval} minutes`;
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
  data = await main(current_time)
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

🎉 :partying\_face: Congratulations you've just integrated trending mints feature into your application!

If you have any questions or need help regarding integrating or building trending mints into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Trending Mints With Onchain Graph](onchain-graph.md)
* [Trending Mints With Common Minters](common-minters.md)
* [Token Mints Guides](../token-mints.md)
* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
