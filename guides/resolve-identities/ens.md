---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve ENS to
  other web3 identities (Farcaster, Lens, 0x address).
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

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [ENS](https://ens.domains) applications and for integrating on-chain and off-chain data from ENS.

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get ENS from a given user(s)](ens.md#get-ens-from-a-given-user-s)
* [Get the 0x address, Lens, and Farcaster from a given ENS name(s)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-ens-name-s)
* [Get the 0x address, Lens, and Farcaster from a given cb.id (Offchain)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-cb.id-offchain)
* [Get the 0x address, Lens, and Farcaster from a given Namestone Subdomain (Offchain)](ens.md#get-the-0x-address-lens-and-farcaster-from-a-given-namestone-subdomain-offchain)

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

You can get all the ENS names of a given user, both primary and non-primary names, by providing either 0x addresses, Lens profile, or Farcaster:

### Try Demo

{% embed url="https://app.airstack.xyz/query/SkTlH3Lh3I" %}
Show me the ENS of 0x4b70d04124c2996de29e0caa050a49822faec6cc, lens/@stani, fc\_fname:vbuterin
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
            "0x4b70d04124c2996de29e0caa050a49822faec6cc"
            "lens/@stani"
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
        {
          "name": "brianshaw.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "vbuterin.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "klock.eth",
          "isPrimary": false,
          "resolvedAddress": "0xdb5802f5579b97be83bd48acad271187f7813148"
        },
        {
          "name": "quantumsmartcontracts.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "legendgames.eth",
          "isPrimary": false,
          "resolvedAddress": "0xdb5802f5579b97be83bd48acad271187f7813148"
        },
        {
          "name": "vitalik.ethid.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "elddem.eth",
          "isPrimary": false,
          "resolvedAddress": "0xdb5802f5579b97be83bd48acad271187f7813148"
        },
        {
          "name": "v‍i‍t‍a‍l‍i‍k‍.eth",
          "isPrimary": false,
          "resolvedAddress": "0xc341683d1ee57b13ab90a1fc00f38dc6f7afdd14"
        },
        {
          "name": "happybirthdayvitalik.eth",
          "isPrimary": false,
          "resolvedAddress": "0x000001f568875f378bf6d170b790967fe429c81a"
        },
        {
          "name": "openegp.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "satoshichained.eth",
          "isPrimary": false,
          "resolvedAddress": "0xdb5802f5579b97be83bd48acad271187f7813148"
        },
        {
          "name": "[96ac36fc321a672a62d6674124081d3998301fd403505f4eae1f0a1f824391f1].addr.reverse",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "quantumdapps.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "slasha.vitalik.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "v-buterin.eth",
          "isPrimary": false,
          "resolvedAddress": "0x2b3a83b27139d0501ed2345716121189e7994964"
        },
        {
          "name": "vitalik-b.eth",
          "isPrimary": false,
          "resolvedAddress": "0x2b3a83b27139d0501ed2345716121189e7994964"
        },
        {
          "name": "ethereumgoodprivacy.eth",
          "isPrimary": false,
          "resolvedAddress": "0x6b0c50a96e8ff1e740b4518253078ec79c3a139c"
        },
        {
          "name": "prxshant.eth",
          "isPrimary": true,
          "resolvedAddress": "0x4b70d04124c2996de29e0caa050a49822faec6cc"
        },
        {
          "name": "denarii.eth",
          "isPrimary": false,
          "resolvedAddress": "0x8a8b5318d3a59fa6d1d0a83a1b0506f2796b5670"
        },
        {
          "name": "@LudwigEth #ens.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "prashantbagga.eth",
          "isPrimary": false,
          "resolvedAddress": "0x4b70d04124c2996de29e0caa050a49822faec6cc"
        },
        {
          "name": "vitalik.rpl.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "vitalik.xeenon.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "vitalik.eth",
          "isPrimary": true,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "vitalik-buterin.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "pool.denarii.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "quantum-ethereum.eth",
          "isPrimary": false,
          "resolvedAddress": "0x6b0c50a96e8ff1e740b4518253078ec79c3a139c"
        },
        {
          "name": "qthereum.eth",
          "isPrimary": false,
          "resolvedAddress": "0x6b0c50a96e8ff1e740b4518253078ec79c3a139c"
        },
        {
          "name": "denarius.eth",
          "isPrimary": false,
          "resolvedAddress": "0xdb5802f5579b97be83bd48acad271187f7813148"
        },
        {
          "name": "tfw_cheese_under_my_toe_nails_and_it_stanks_but_in_a_good_way_in_fact_so_good_i_wanna_smear_it_on_a_toast_and_eat_it.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "list.denarii.eth",
          "isPrimary": false,
          "resolvedAddress": ""
        },
        {
          "name": "ephemeralgoodprivacy.eth",
          "isPrimary": false,
          "resolvedAddress": "0x6b0c50a96e8ff1e740b4518253078ec79c3a139c"
        },
        {
          "name": "vitalik.takoyaki.eth",
          "isPrimary": false,
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        {
          "name": "@LudwigEth.eth",
          "isPrimary": false,
          "resolvedAddress": ""
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

## Developer Support

If you have any questions or need help regarding resolving ENS name(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [ENS Domains Guide](../ens-domain/)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
