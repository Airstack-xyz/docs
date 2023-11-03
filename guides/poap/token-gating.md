---
description: >-
  Learn how to enable users to access certain features only if they hold certain
  POAP(s) or have common POAPs with a given user.
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

# 🚪 Token Gating

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [POAP](https://poap.xyz/) applications and for integrating POAP on-chain data indexed directly from both Ethereum and Gnosis.

In this tutorial, you will learn how to build a [POAP](https://poap.xyz) token gating system with various conditions.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Gating Only User(s) That Have A Given POAP](token-gating.md#gating-only-user-s-that-have-a-given-poap)
- [Gating Only User(s) That Have Multiple Given POAP(s)](token-gating.md#gating-only-user-s-that-have-multiple-given-poap-s)
- [Gating Only User(s) That Have Common POAP(s) With A Given User](token-gating.md#gating-only-user-s-that-have-common-poap-s-with-a-given-user)

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

## Gating Only User(s) That Have A Given POAP

You can gate certain feature access only to users that have attended a certain POAP event, e.g. [EthCC\[6\] - Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=&address=141910&tokenType=&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29&inputType=POAP&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&activeSocialInfo=&blockchain=gnosis):

### Try Demo

{% embed url="https://app.airstack.xyz/query/ktnZJrrbPy" %}
Show me if molpy.eth attended EthCC\[6] - Attendee
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { eventId: { _eq: "141910" }, owner: { _eq: "molpy.eth" } }
      blockchain: ALL
    }
  ) {
    Poap {
      mintHash
      mintOrder
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
<strong>      "Poap": [ // non-empty array indicate that user attended EthCC[6]
</strong>        {
          "mintHash": "0x66865ff2cf4391cd72447fd2955b1a22f80ee0876188225ec3375168cb1238ef",
          "mintOrder": 934
        }
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

If `Poaps.Poap` returned a non-empty array, filled with `mintHash` and `mintOrder` data, it indicates that the POAP has been minted under the given user account and thus showing that the user have attended the POAP event.

In such case, the user shall be **granted feature access**. Otherwise, **no access** shall be given.

## Gating Only User(s) That Have Multiple Given POAP(s)

You can gate certain feature access only to users that have attended multiple given POAP events, e.g. [EthCC\[6\] - Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=&address=141910&tokenType=&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29&inputType=POAP&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&activeSocialInfo=&blockchain=gnosis) and [EthDenver 2023 POAP](https://explorer.airstack.xyz/token-holders?activeView=&address=103093&tokenType=&rawInput=%23%E2%8E%B1ETHDenver+2023%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+103093%29&inputType=POAP&activeTokenInfo=&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&activeSocialInfo=&blockchain=gnosis):&#x20;

{% hint style="info" %}
If you would like to add more POAP events to check, then you can simply add another `owner.poaps` nesting under the innermost `poaps` field with the newly added POAPs `eventId` as an input .\
\
To see more details and example demos, check out [Combinations of Multiple POAPs](../combinations/multiple-poaps.md#common-holders-of-more-than-2-poaps).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/gXZpd3F7eB" %}
Show me if sponnet.eth attended both EthCC\[6] - Attendee and ETHDenver 2023
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Poaps(
    input: {
      filter: { eventId: { _eq: "141910" }, owner: { _eq: "sponnet.eth" } }
      blockchain: ALL
    }
  ) {
    Poap {
      owner {
        poaps(input: { filter: { eventId: { _eq: "103093" } } }) {
          mintHash
          mintOrder
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
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
                "mintHash": "0xa263abd088630e45d1e06e3b15afaf29d1454c64f73eb312367371eee7fdf1c6",
                "mintOrder": 914
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

If the innermost `owner.poaps` return a non-empty array, then it implies that the user has attended the given multiple events and thus **feature access can be given** to the user.

Otherwise, **no feature access** shall be given to the user.

## Gating Only User(s) That Have Common POAP(s) With A Given User

You can gate certain feature access only to users that have attended the same POAP event as a given user.

In other words, have common POAP(s) with the given user, e.g. checking if [`nich.eth` ](https://explorer.airstack.xyz/token-balances?address=nich.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1nich.eth%E2%8E%B1%28nich.eth++ethereum+null%29&inputType=ADDRESS)can be given any access by checking any common POAP with another user [`sponnet.eth`](https://explorer.airstack.xyz/token-balances?address=sponnet.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1sponnet.eth%E2%8E%B1%28sponnet.eth++ethereum+null%29&inputType=ADDRESS):&#x20;

{% hint style="info" %}
If you would like to add more users, then you can simply add another `poapEvent.poaps` nesting under the innermost `poaps` field with the newly added POAPs `owner` as an input .\
\
To see more details and example demos, check out [POAPs in Common](../tokens-in-common/poaps.md).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/BHhGV5dFqG" %}
show me the POAPs in common attended by both sponnet.eth and nich.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Poaps(
<strong>    input: {filter: {owner: {_eq: "sponnet.eth"}}, blockchain: ALL, limit: 200}
</strong>  ) {
    Poap {
      poapEvent {
<strong>        poaps(input: {filter: {owner: {_eq: "nich.eth"}}, limit: 200}) {
</strong>          poapEvent {
            eventId
            eventName
          }
          mintHash
          mintOrder
        }
      }
    }
    pageInfo {
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
    "Poaps": {
      "Poap": [
        {
          "poapEvent": {
<strong>            "poaps": [ // Both sponnet.eth and nich.eth attended EthCC[6]
</strong>              {
                "poapEvent": {
                  "eventId": "141910",
                  "eventName": "EthCC[6] - Attendee"
                },
                "mintHash": "0x1388589a75469c8da2df798a7464469dac2b9547fb3bba0795174cbc2c2c802d",
                "mintOrder": 568
              }
            ]
          }
        },
        {
          "poapEvent": {
            "poaps": []
          }
        },
        // more POAPs
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

If [`nich.eth`](https://explorer.airstack.xyz/token-balances?address=nich.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1nich.eth%E2%8E%B1%28nich.eth++ethereum+null%29&inputType=ADDRESS) have at least 1 common POAP with [`sponnet.eth`](https://explorer.airstack.xyz/token-balances?address=sponnet.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1sponnet.eth%E2%8E%B1%28sponnet.eth++ethereum+null%29&inputType=ADDRESS) as shown in the [sample response](token-gating.md#response-2), then [`nich.eth`](https://explorer.airstack.xyz/token-balances?address=nich.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1nich.eth%E2%8E%B1%28nich.eth++ethereum+null%29&inputType=ADDRESS) can be given access to the gated feature.

Otherwise, **no feature access** shall be given to [`nich.eth`](https://explorer.airstack.xyz/token-balances?address=nich.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1nich.eth%E2%8E%B1%28nich.eth++ethereum+null%29&inputType=ADDRESS).

## Developer Support

If you have any questions or need help building token gating with POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Poaps API Reference](../../api-references/api-reference/poaps-api/)
- [Poaps API Examples](../../api-references/api-reference/poaps-api/poaps-api-examples.md)
- [Combinations](../combinations/)
  - [Multiple POAPs](../combinations/multiple-poaps.md)
  - [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
- [Tokens In Common](../tokens-in-common/)
  - [POAPs](../tokens-in-common/poaps.md)
