---
description: >-
  Learn how to use Airstack to fetch the ENS domain details, such as manager,
  text records, and multichain addresses.
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

# ✍ ENS Domain Details

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get ENS Domain Manager](ens-domain-details.md#get-ens-domain-manager)
* [Get ENS Domain Text Record](ens-domain-details.md#get-ens-domain-text-record)
* [Get ENS Domain Multichain Addresses](ens-domain-details.md#get-ens-domain-multichain-addresses)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
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

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get ENS Domain Manager

You can use the [`Domains`](../../api-references/api-reference/domains-api.md) API to fetch the manager of an ENS domain.In `Domains` API, there is `manager` field that provide a 0x address of the ENS domain manager, but if you want to fetch more information on the manager, e.g. other ENS owned by the manager, socials (Lens & Farcaster), XMTP, token balances, etc., then use `managerDetails`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/wNEW9SEFuy" %}
Show me the manager and its details of ENS domain vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: {
      filter: {
        name: {_eq: "vitalik.eth"}
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      manager
      managerDetails {
        addresses
        domains {
          isPrimary
          name
        }
        socials {
          profileName
          dappName
        }
        xmtp {
          isXMTPEnabled
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
    "Domains": {
      "Domain": [
        {
          "manager": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "managerDetails": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "quantumdapps.eth"
              },
              {
                "isPrimary": true,
                "name": "vitalik.eth"
              },
              // Other ENS domains
            ],
            "socials": [
              {
                "profileName": "vitalik.eth",
                "dappName": "farcaster"
              },
              {
                "profileName": "lens/@vitalik",
                "dappName": "lens"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
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

## Get ENS Domain Text Record

You can use the [`Domains`](../../api-references/api-reference/domains-api.md) API to fetch an ENS domain text records using the `texts` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/vYotiSkv2s" %}
Show me all text records of ENS domain ens.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: {
      filter: {
        name: {_eq: "ens.eth"}
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      texts {
        key
        value
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
    "Domains": {
      "Domain": [
        {
          "texts": [
            {
              "key": "url",
              "value": "https://ens.domains/"
            },
            {
              "key": "avatar",
              "value": "https://i.imgur.com/ga6y0c0.jpg"
            },
            {
              "key": "com.github",
              "value": "ensdomains"
            },
            {
              "key": "com.twitter",
              "value": "ensdomains"
            },
            {
              "key": "snapshot",
              "value": "ipns://storage.snapshot.page/registry/0xb6E040C9ECAaE172a89bD561c5F73e1C48d28cd9/ens.eth"
            }
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get ENS Domain Multichain Addresses

You can use the [`Domains`](../../api-references/api-reference/domains-api.md) API to resolve the multichain addresses of an ENS domain, that is fetching user's address on different chain, especially non-EVM ones:

### Try Demo

{% embed url="https://app.airstack.xyz/query/6nSZ4QG6U1" %}
Show me all the multichain addresses of ENS domain barmstrong.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: {
      filter: {
        name: {_eq: "barmstrong.eth"}
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      multiChainAddresses {
        address
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
    "Domains": {
      "Domain": [
        {
          "multiChainAddresses": [
            {
              // BTC address
<strong>              "address": "3BFFsrzv9npUkP53XEwf8kKLPHSu8yniaa",
</strong>              "symbol": "BTC"
            },
            {
              // ETH address
<strong>              "address": "0x5b76f5B8fc9D700624F78208132f91AD4e61a1f0",
</strong>              "symbol": "ETH"
            }
          ]
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding resolving identities for ENS domain(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Resolve ENS Domain](resolve-ens-domains.md)
* [ENS Profile Image](profile-image.md)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
