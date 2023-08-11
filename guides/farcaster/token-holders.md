---
description: >-
  Learn how to get all Farcaster users who own a specific token, NFT, or POAP,
  or a min amount of that token. Get combinations of NFTs or POAPs + Farcaster,
  e.g. Has POAP1 and POAP2 and has Farcaster
---

# 🥇 Token Holders

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

In this tutorial, you will learn how to fetch all Farcaster users who own a specific ERC20 token, NFT (ERC721 and ERC1155), or POAPs.

In addition, you will also learn how to fetch common Farcaster users that hold two different assets at the same time, e.g. Farcaster users that hold both [EthLisbon](https://explorer.airstack.xyz/token-holders?address=76949\&blockchain=gnosis\&rawInput=%23%E2%8E%B1ETHLisbon+2022%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+76949%29\&inputType=POAP) and [EthCC\[6\]](https://explorer.airstack.xyz/token-holders?address=141910\&blockchain=gnosis\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29\&inputType=POAP) POAP.

In this guide you will learn how to use Airstack to:

* [Get Holders of an ERC20 Token That Has Farcaster](token-holders.md#get-holders-of-an-erc20-token-that-has-farcaster)
* [Get Holders of NFT That Has Farcaster](token-holders.md#get-holders-of-nft-that-has-farcaster)
* [Get Holders of POAP That Has Farcaster](token-holders.md#get-holders-of-nft-that-has-farcaster)
* [Get Holders That Held Specific Amount of ERC20 Token](token-holders.md#get-holders-that-held-specific-amount-of-erc20-token)
* [Get Common Holders of 2 ERC20 Tokens That Has Farcaster](token-holders.md#get-common-holders-of-2-erc20-tokens-that-has-farcaster)
* [Get Common Holders of Two POAPs That Has Farcaster](token-holders.md#get-common-holders-of-two-poaps-and-that-has-farcaster)
* [Get Common Holders of A Token (ERC20 or NFT) and A POAP That Has Farcaster](token-holders.md#get-common-holders-of-two-poaps-and-that-has-farcaster)

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

## Get Holders of an ERC20 Token That Has Farcaster

### Fetching

You can get all holders of an ERC20 token that has Farcaster:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/dErT36be21" %}
Show all token holders of USD Coin on Polygon that has Farcaster
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174"}}, blockchain: polygon, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
```json
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "vbuterin",
                "userId": "5650",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

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
    "profileName": "vbuterin",
    "userId": "5650",
    "userAssociatedAddresses": [
      "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
      "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
    ]
  },
  // ... other token holders
]
```

## Get Holders of NFT That Has Farcaster

### Fetching

You can get all holders of NFT that has Farcaster:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Qyl0sm2u8E" %}
Show all token holders of BoredApeYachtClub that has Farcaster
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(
          input: {filter: {dappName: {_eq: farcaster}}}
        ) {
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
```graphql
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "durtis",
                "userId": "11926",
                "userAssociatedAddresses": [
                  "0xcb0b3404b7d5db8622511427c09a1bba450d3c0f",
                  "0xe5ca890a0ef2f128eb3267e4711c6bf3306ec024"
                ]
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

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
    "profileName": "durtis",
    "userId": "11926",
    "userAssociatedAddresses": [
      "0xcb0b3404b7d5db8622511427c09a1bba450d3c0f",
      "0xe5ca890a0ef2f128eb3267e4711c6bf3306ec024"
    ]
  }
  // ... other token holders
]
```

## Get Holders of POAP That Has Farcaster

### Fetching

You can get all holders of POAP that has Farcaster:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/BsPwpo9zYu" %}
Show all POAP holders of EthCC\[6] - Attendee that has Farcaster
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(input: {filter: {eventId: {_eq: "141910"}}, blockchain: ALL, limit: 50}) {
    Poap {
      owner {
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
```graphql
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "megankaspar",
                "userId": "7361",
                "userAssociatedAddresses": [
                  "0xadf37a0d500c748edb1c689a1be26472e583dcb5",
                  "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
                  "0xfaedb341b0faced023099d7b0ccd23c2ec5ed7a5"
                ]
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'Poaps' in data and 'Poap' in data['Poaps']:
        for item in data['Poaps']['Poap']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

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
    "profileName": "megankaspar",
    "userId": "7361",
    "userAssociatedAddresses": [
      "0xadf37a0d500c748edb1c689a1be26472e583dcb5",
      "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
      "0xfaedb341b0faced023099d7b0ccd23c2ec5ed7a5"
    ]
  },
  // ... other token holders
]
```

## Get Holders That Held Specific Amount of ERC20 Token

### Fetching

You can get all holders of an ERC20 token that have a minimum amount held in their balances which also have Farcaster:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/c1yxMwkl5B" %}
Show all user that has at least 10 USD Coin with their Farcaster
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: {_eq: "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"}, 
        formattedAmount: {_gte: 10}
      }, 
      blockchain: ethereum, 
      limit: 200
    }
  ) {
    TokenBalance {
      owner {
        identity
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
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
```graphql
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xfc84d2742d0f1a66d76d51a2980f48a770a9456e",
            "socials": [
              {
                "profileName": "yusuf",
                "userId": "1081",
                "userAssociatedAddresses": [
                  "0x3da8748ca04694d8f889fe457db96c2e7e7f381b",
                  "0xfc84d2742d0f1a66d76d51a2980f48a770a9456e",
                  "0x302882ea764225633c74838d702bb591774955a3"
                ]
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

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

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
    "profileName": "durtis",
    "userId": "11926",
    "userAssociatedAddresses": [
      "0xcb0b3404b7d5db8622511427c09a1bba450d3c0f",
      "0xe5ca890a0ef2f128eb3267e4711c6bf3306ec024"
    ]
  }
  // ... other token holders
]
```

## Get Common Holders of 2 ERC20 Tokens That Has Farcaster

### Fetching

You can fetch the common holders of two given ERC20, e.g. [USDT](https://explorer.airstack.xyz/token-holders?address=0xdac17f958d2ee523a2206206994597c13d831ec7\&blockchain=ethereum\&rawInput=%23%E2%8E%B1Tether+USD%E2%8E%B1%280xdac17f958d2ee523a2206206994597c13d831ec7+TOKEN+ethereum+null%29+\&inputType=ADDRESS) and [USDC](https://explorer.airstack.xyz/token-holders?address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&blockchain=ethereum\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29+\&inputType=ADDRESS):

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfUSDTAndUSDC {
<strong>  TokenBalances(input: {filter: {tokenAddress: {_eq: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"}}, blockchain: ethereum, limit: 200}) {
</strong>    TokenBalance {
      owner {
<strong>        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xdAC17F958D2ee523a2206206994597C13D831ec7"}}, limit: 200}) {
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
            "tokenBalances": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "vbuterin",
                      "userId": "5606",
                      "userAssociatedAddresses": [
                        "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
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
            "tokenBalances": [] // Have USDT, but no USDC
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
    owner?.tokenBalances?.map(({ owner }) =>
      owner?.socials?.length > 0 ? owner?.socials : null
    )
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner']:
                for token_balance in item['owner']['tokenBalances']:
                    if 'owner' in token_balance and 'socials' in token_balance['owner'] and len(token_balance['owner']['socials']) > 0:
                        result.append(token_balance['owner']['socials'])

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
    "profileName": "vbuterin",
    "userId": "5606",
    "userAssociatedAddresses": [
      "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
      "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
    ]
  }
  // ... other token holders
]
```

## Get Common Holders of Two POAPs and That Has Farcaster

### Fetching

You can fetch the common holders of two given POAP event IDs, e.g. [EthGlobal Lisbon 2023 Partner Attendee POAP](https://explorer.airstack.xyz/token-holders?address=127462\&blockchain=\&rawInput=%23%E2%8E%B1127462%E2%8E%B1%28127462++++ID\_POAP%29\&inputType=POAP) & [EthCC\[6\] Attendee POAP](https://explorer.airstack.xyz/token-holders?address=141910\&blockchain=gnosis\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29+\&inputType=POAP):

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/tjIzrCbvsd" %}
Get Common Holders Of EthGlobal Lisbon and EthCC POAPs
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfEthGlobalLisbonAndEthCC {
<strong>  Poaps(input: {filter: {eventId: {_eq: "127462"}}, blockchain: ALL, limit: 200}) {
</strong>    Poap {
      owner {
<strong>        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: "141910"}}}) {
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
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "vbuterin",
                      "userId": "5606",
                      "userAssociatedAddresses": [
                        "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
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
            "poaps": null // Have EthGlobal Lisbon, but no EthCC[6]
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

If user has any Farcaster, then `socials` will have non-`null` value and `profileName` and `userId` will show both the Farcaster name and ID, respectively.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) =>
      owner?.socials?.length > 0 ? owner?.socials : null
    )
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    
    if data and 'Poaps' in data and 'Poap' in data['Poaps']:
        for poap in data['Poaps']['Poap']:
            if 'owner' in poap and 'poaps' in poap['owner']:
                for owner_poap in poap['owner']['poaps']:
                    if 'owner' in owner_poap and 'socials' in owner_poap['owner'] and len(owner_poap['owner']['socials']) > 0:
                        result.append(owner_poap['owner']['socials'])

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
    "profileName": "schmidsi",
    "userId": "14088",
    "userAssociatedAddresses": [
      "0xcc8fd0d88abf88d006fea4487e59e2b39f281095",
      "0x546457bbddf5e09929399768ab5a9d588cb0334d"
    ]
  },
  // ...other token holders
]
```

## Get Common Holders of A Token (ERC20 or NFT) and A POAP That Has Farcaster

### Fetching

You can fetch the common holder of a token and a POAP by providing the token contract address and the POAP event ID:

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

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Combinations](../lens/recommendation-engines.md)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Mutliple POAPs](../combinations/multiple-poaps.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
  * [Social Combinations](../combinations/socials-stats.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
