---
description: >-
  Learn how to fetch user's Fan Token portfolio balances across all their
  wallets and vesting contracts. In addition, you will also learn how you can
  setup Fan Token gating for your project/application.
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

# 🧩 Moxie Fan Token Balances

## Table Of Contents

* [Get User's Fan Token Balances By Wallet Address](moxie-fan-token-balances.md#get-users-fan-token-balances-by-wallet-address)
* [Get User's Fan Token Balances By FID](moxie-fan-token-balances.md#get-users-fan-token-balances-by-fid)
* [Check If Certain User Hold Certain Fan Token](moxie-fan-token-balances.md#check-if-certain-user-hold-certain-fan-token) (Fan Token Gating)

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

## Get User's Fan Token Balances By Wallet Address

You can get a user's Fan Token portfolio balances by using the [`MoxieUserPortfolio`](../api-references/api-reference/moxieuserportfolios.md) API and inputting the user's wallet address in `walletAddress` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LSFYWtiYxc" %}
Show me the Moxie Fan token portfolio balances of 0xd7029bdea1c17493893aafe29aad69ef892b8ff2
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieUserPortfolios(
    input: {
      filter: {
        walletAddress: {
          _eq: "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
        }
      }
    }
  ) {
    MoxieUserPortfolio {
      fanTokenSymbol
      fanTokenAddress
      fanTokenName
      amount: totalUnlockedAmount
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieUserPortfolios": {
      "MoxieUserPortfolio": [
        {
          "fanTokenSymbol": "fid:3",
          "fanTokenAddress": "0xd83b15a9c8dc73f1a5ba279ea7815abf97a91fff",
          "fanTokenName": "dwr.eth",
          "amount": 13020342330530892
        },
        // Other Fan Tokens in portfolio by 0xd7029bdea1c17493893aafe29aad69ef892b8ff2
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get User's Fan Token Balances By FID

You can get a user's Fan Token portfolio balances by using the [`MoxieUserPortfolio`](../api-references/api-reference/moxieuserportfolios.md) API and inputting the user's FID in `fid` input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZdLg1vJBVV" %}
Show me the Moxie Fan token portfolio balances of FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  MoxieUserPortfolios(
    input: {
      filter: {
        fid: {_eq: "3"}
      }
    }
  ) {
    MoxieUserPortfolio {
      fanTokenAddress
      fanTokenName
      fanTokenSymbol
      amount: totalUnlockedAmount
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieUserPortfolios": {
      "MoxieUserPortfolio": [
        {
          "fanTokenAddress": "0xbe2f22ae9f1bff46a9d814395cf70bfae8ed3a86",
          "fanTokenName": "betashop.eth",
          "fanTokenSymbol": "fid:602",
          "amount": 10000000000000000000
        },
        // Other Fan Tokens owned by FID 3
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If Certain User Hold Certain Fan Token

You can check if a certain user hold a certain Fan Token in their portfolio balance by using the [`MoxieUserPortfolio`](../api-references/api-reference/moxieuserportfolios.md) API and inputting the user's wallet to the `walletAddress` input and the Fan Token symbol to the `fanTokenSymbol` input:

{% hint style="info" %}
Alternatively, you can also use `fid` and `fanTokenAddress` to specify the user and Fan Token input, respectively.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/odTEChTFUe" %}
Check If 0xd7029bdea1c17493893aafe29aad69ef892b8ff2 hold Fan Token FID 3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetPortfolioInfo {
  MoxieUserPortfolios(
    input: {
      filter: {
        walletAddress: {
          # user's wallet address
<strong>          _eq: "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
</strong>        },
        fanTokenSymbol: {
          # Fan Token to check, symbol will be based on types:
          # - User: fid:&#x3C;FID>
          # - Channel: cid:&#x3C;CHANNEL-ID>
          # - Network: id:farcaster
<strong>          _eq: "fid:3"
</strong>        }
      }
    }
  ) {
    MoxieUserPortfolio {
      amount: totalUnlockedAmount
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "MoxieUserPortfolios": {
      "MoxieUserPortfolio": [
        // If not `null`, then it indicates that the user hold the FT
        {
          "amount": 13020342330530892
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster user's Moxie Fan Token portfolio data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [MoxieUserPortfolio API References](../api-references/api-reference/moxieuserportfolios.md)
