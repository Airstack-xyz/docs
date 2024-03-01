---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve ENS,
  Namestone, and cb.id to other web3 identities (Farcaster, Lens, 0x address,
  Solana address).
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

# 🔷 ENS

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [ENS](https://ens.domains) applications and for integrating on-chain ENS data and off-chain data from Namestone and cb.id.

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get ENS from a given user(s)](ens.md#get-ens-from-a-given-user-s)
* [Get the 0x address, Lens, and Farcaster from a given ENS name(s)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-ens-name-s)
* [Get the 0x address, Lens, and Farcaster from a given cb.id (Offchain)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-cb.id-offchain)
* [Get the 0x address, Lens, and Farcaster from a given Namestone Subdomain (Offchain)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-namestone-subdomain-offchain)
* [Get All The Solana addresses from a given ENS name](ens.md#get-all-the-solana-addresses-from-a-given-ens-name)
* [Get All The Solana addresses from a given Namestone Subdomain or cb.id (Offchain)](ens.md#get-all-the-solana-addresses-from-a-given-namestone-subdomain-or-cb.id-offchain)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

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

## Get ENS from a given user(s)

You can get all the ENS names and offchain domains (Namestone & cb.id) of a given user, both primary and non-primary names, by providing either 0x addresses, Solana addresses, Farcaster or Lens profile:

### Try Demo

{% embed url="https://app.airstack.xyz/query/SkTlH3Lh3I" %}
Show me the ENS of 0x4b70d04124c2996de29e0caa050a49822faec6cc, GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV, lens/@stani, fc\_fname:vbuterin
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENS {
  Domains(
    input: {
      filter: {
        owner: {
          _in: [
            "0x4b70d04124c2996de29e0caa050a49822faec6cc",
            "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV",
            "lens/@stani",
            "fc_fname:vbuterin"
          ]
        }
      }
      blockchain: ethereum
    }
  ) {
    Domain {
      name
      isPrimary
      resolvedAddress
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
          "name": "skynft.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "quantumexchange.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "vitalik.daohall.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        // Other ENS domains
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the 0x address, Lens, and Farcaster from a given ENS name(s)

You can get the 0x address, Lens, and Farcaster of ENS names:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LAFfZPgdlt" %}
show me the 0x address, Lens, Farcaster of vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetUserDetailsFromENS {
  Domains(
    input: {filter: {name: {_in: ["vitalik.eth"]}}, blockchain: ethereum}
  ) {
    Domain {
      resolvedAddress
      resolvedAddressDetails {
        socials {
          profileName
          dappName
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
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "resolvedAddressDetails": {
            "socials": [
              {
                "profileName": "vitalik.eth",
                "dappName": "farcaster"
              },
              {
                "profileName": "lens/@vitalik",
                "dappName": "lens"
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

## Get the 0x address, Lens, and Farcaster from a given cb.id (Offchain)

You can get the 0x address, Lens, and Farcaster of cb.ids:

### Try Demo

{% embed url="https://app.airstack.xyz/query/OLqqmKXrqj" %}
Show me the 0x address, Lens, and Farcaster of yosephks.cb.id
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCbDotID {
  Domains(
    input: {filter: {name: {_in: ["yosephks.cb.id"]}}, blockchain: ethereum}
  ) {
    Domain {
      resolvedAddress
      resolvedAddressDetails {
        socials {
          profileName
          dappName
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
          "resolvedAddress": "0xc7486219881c780b676499868716b27095317416",
          "resolvedAddressDetails": {
            "socials": [
              {
                "profileName": "lens/@yosephks",
                "dappName": "lens"
              },
              {
                "profileName": "yosephks.eth",
                "dappName": "farcaster"
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

## Get the 0x address, Lens, and Farcaster from a given Namestone Subdomain (Offchain)

You can get the 0x address, Lens, and Farcaster of [Namestone](https://namestone.xyz/) subdomains:

### Try Demo

{% embed url="https://app.airstack.xyz/query/tYYXAwIXUu" %}
Show me the 0x address, Lens, and Farcaster of namestone.testbrand.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetNamestone {
  Domains(
    input: {filter: {name: {_in: ["namestone.testbrand.eth"]}}, blockchain: ethereum}
  ) {
    Domain {
      resolvedAddress
      resolvedAddressDetails {
        socials {
          profileName
          dappName
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
          "resolvedAddress": "0x57632ba9a844af0ab7d5cdf98b0056c8d87e3a85",
          "resolvedAddressDetails": {
            "socials": [
              {
                "profileName": "heeroyuy",
                "dappName": "farcaster"
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

## Get All The Solana addresses from a given ENS name

You can get the Solana addresses of ENS names by using the [`Domains`](../../api-references/api-reference/domains-api.md) API and checking through the registered `multichainAddresses` field that has `symbol` equal to **SOL**:

### Try Demo

{% embed url="https://app.airstack.xyz/query/UHJdGOCe1r" %}
Show me alexjcomeau.eth's multichain SOL address
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetUserDetailsFromENS {
  Domains(
    input: {
      filter: {
        name: {_eq: "alexjcomeau.eth"}
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
              "address": "0xe0235804378c31948E81441f656D826eE5998Bc6",
              "symbol": "ETH"
            },
            {
              // This is the SOL address registered by user in ENS
<strong>              "address": "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV",
</strong>              "symbol": "SOL"
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

## Get All The Solana addresses from a given Namestone Subdomain or cb.id (Offchain)

You can get the 0x addresses of offchain domains (Namestone/cb.id) by using the [`Domains`](../../api-references/api-reference/domains-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/E2MMhhm4ql" %}
Show me yosephks.cb.id's multichain SOL address
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: {
      filter: {
        name: {_eq: "yosephks.cb.id"}
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
              "address": "bc1qetgl5rx3uuhxek7erfc3wh97m3xkshx8pkdpr5",
              "symbol": "BTC"
            },
            {
              "address": "DETcngVSTXVbetWmRo9kdQe7xD9F19uqWk",
              "symbol": "DOGE"
            },
            {
              "address": "0xc7486219881C780B676499868716B27095317416",
              "symbol": "ETH"
            },
            {
              "address": "ltc1qwsqc8y09w59yqvlwy9c4fkqxu5md33vsr2uak6",
              "symbol": "LTC"
            },
            {
              // This is the SOL address connected to yosephks.cb.id (offchain)
<strong>              "address": "HyrNmmmce9W3rDdTQcZHyYvhuxPN6AaY3mVJcS9f4AZw",
</strong>              "symbol": "SOL"
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

## Developer Support

If you have any questions or need help regarding resolving ENS name(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [ENS Domains Guide](../ens-domains/)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
