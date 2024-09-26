---
description: Learn how to fetch data about Moxie Fan Tokens in the Moxie protocol.
---

# 🪭 Moxie Fan Tokens

## Table Of Contents

* [Get The Current Price Of A Fan Token](moxie-fan-tokens.md#get-the-current-price-of-a-fan-token)
* [Get The Total Supply Of A Fan Token](moxie-fan-tokens.md#get-the-total-supply-of-a-fan-token)
* [Get The Number Of Unique Holders Of A Certain Fan Token](moxie-fan-tokens.md#get-the-number-of-unique-holders-of-a-certain-fan-token)
* [Get The Current Total Locked Value Of A Certain Fan Token](moxie-fan-tokens.md#get-the-current-total-locked-value-of-a-certain-fan-token)
* [Get All Fan Tokens With More Than X Holders](moxie-fan-tokens.md#get-all-fan-tokens-with-more-than-x-holders)
* [Get Trending Fan Tokens](moxie-fan-tokens.md#get-trending-fan-tokens)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) API key. To access this, you'll need to hold **at least 1 /airstack Channel Fan Token**.
* Basic knowledge of GraphQL

## Get Started

**JavaScript/TypeScript/Python**

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

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## Get The Current Price Of A Fan Token

You can get the current price of a Fan Token by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and inputting the Fan Token symbol in `fanTokenSymbol` input:

{% hint style="info" %}
Alternatively, you can also use `fanTokenAddress` to specify the Fan Token input data.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/oV3DkhGKUL" %}
Get The current price of Fan Token FID 3 (in Moxie)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      filter: {
        fanTokenSymbol: {
          # Fan Token to check, symbol will be based on types:
          # - User: fid:<FID>
          # - Channel: cid:<CHANNEL-ID>
          # - Network: id:farcaster
          _eq: "fid:3"
        }
      }
    }
  ) {
    MoxieFanToken {
      currentPrice
      currentPriceInWei
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "currentPrice": 333.2940312828,
          "currentPriceInWei": 333294031282823600000
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Total Supply Of A Fan Token

You can get the total supply of a Fan Token by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and inputting the Fan Token symbol in `fanTokenSymbol` input:

{% hint style="info" %}
Alternatively, you can also use `fanTokenAddress` to specify the Fan Token input data.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/RO2CVDeeuB" %}
Show me the total supply of Fan Token FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      filter: {
        fanTokenSymbol: {
          # Fan Token to check, symbol will be based on types:
          # - User: fid:<FID>
          # - Channel: cid:<CHANNEL-ID>
          # - Network: id:farcaster
          _eq: "fid:3"
        }
      }
    }
  ) {
    MoxieFanToken {
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
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "totalSupply": 2.9452860156560017e+22
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Number Of Unique Holders Of A Certain Fan Token

You can get the number of unique holders of a Fan Token by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and inputting the Fan Token symbol in `fanTokenSymbol` input:

{% hint style="info" %}
Alternatively, you can also use `fanTokenAddress` to specify the Fan Token input data.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/4xErETbJY0" %}
Show me the number of unique holders of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      filter: {
        fanTokenSymbol: {
          # Fan Token to check, symbol will be based on types:
          # - User: fid:<FID>
          # - Channel: cid:<CHANNEL-ID>
          # - Network: id:farcaster
          _eq: "fid:3"
        }
      }
    }
  ) {
    MoxieFanToken {
      uniqueHolders
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "uniqueHolders": 516
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Current Total Locked Value Of A Certain Fan Token

You can get the current total locked value of a Fan Token by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and inputting the Fan Token symbol in `fanTokenSymbol` input:

{% hint style="info" %}
Alternatively, you can also use `fanTokenAddress` to specify the Fan Token input data.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/HoHFzUkEms" %}
Show me the current TVL of Fan Token FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      filter: {
        fanTokenSymbol: {
          # Fan Token to check, symbol will be based on types:
          # - User: fid:<FID>
          # - Channel: cid:<CHANNEL-ID>
          # - Network: id:farcaster
          _eq: "fid:3"
        }
      }
    }
  ) {
    MoxieFanToken {
      tlv
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "tlv": 7.863170089196643e+24
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Fan Tokens With More Than X Holders

You can get all the Fan Tokens with more than certain number of holders by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and inputting the amount filter criteria in the `uniqueHolders` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/m199KgZ5ro" %}
Show me all Fan Tokens with more than 100 holders
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      filter: {
        uniqueHolders: {_gte: "100"}
      }
    }
  ) {
    MoxieFanToken {
      fanTokenName
      fanTokenSymbol
      uniqueHolders
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "fanTokenName": "vlady",
          "fanTokenSymbol": "fid:239748",
          "uniqueHolders": 140
        },
        // Other Fan Tokens w/ > 100 holders
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Trending Fan Tokens

You can get the trending Fan Tokens list as shown in the Airstack Explorer [here](https://www.airstack.xyz/fan-tokens) by using the [`MoxieFanTokens`](../api-references/api-reference/moxiefantokens.md) API and sorting the data by `uniqueHolders` variable:

### Try Demo

{% embed url="https://app.airstack.xyz/query/SpXBHyy0e5" %}
Show me trending Fan Tokens in the Moxie protocol
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieFanTokens(
    input: {
      order: {uniqueHolders: DESC},
      filter: {}
    }
  ) {
    MoxieFanToken {
      fanTokenName
      fanTokenSymbol
      uniqueHolders
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieFanTokens": {
      "MoxieFanToken": [
        {
          "fanTokenName": "network:farcaster",
          "fanTokenSymbol": "id:farcaster",
          "uniqueHolders": 2097
        },
        {
          "fanTokenName": "betashop.eth",
          "fanTokenSymbol": "fid:602",
          "uniqueHolders": 1142
        },
        {
          "fanTokenName": "/airstack",
          "fanTokenSymbol": "cid:airstack",
          "uniqueHolders": 776
        },
        // Other trending Fan Tokens
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster user's Moxie Fan Token data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [MoxieFanTokens API References](../api-references/api-reference/moxiefantokens.md)
