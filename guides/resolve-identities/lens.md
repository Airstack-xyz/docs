---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Lens
  handles to other web3 identities (Farcaster, ENS, Ethereum address).
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

# 🌿 Lens

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [ENS](https://ens.domains) applications and for integrating on-chain and off-chain data from ENS.

### Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get ENS from a given user(s)](lens.md#get-ens-from-a-given-user-s)
* [Get the 0x address, Lens, and Farcaster from a given ENS name(s)](lens.md#get-the-0x-address-lens-and-farcaster-from-a-given-ens-name-s)

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

## Get Lens Handles from a given user(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/isK75uiZ3e" %}
Show me the Lens handles of 0x4b70d04124c2996de29e0caa050a49822faec6cc, betashop.eth, fc\_fname:vbuterin
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLens {
  Socials(
    input: {filter: {identity: {_in: ["0x4b70d04124c2996de29e0caa050a49822faec6cc", "betashop.eth", "fc_fname:vbuterin"]}, dappSlug: {_eq: lens_polygon}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      dappName
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "lens/@prashantbagga",
          "dappName": "lens"
        },
        {
          "profileName": "lens/@betashop9",
          "dappName": "lens"
        },
        {
          "profileName": "lens/@vitalik",
          "dappName": "lens"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum address, Farcaster, and ENS from a given Lens profile(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/OUcCwLSIpH" %}
Show me the 0x address, Farcaster, and ENS of lens/@prashantbagga, lens/@betashop9, lens/@vitalik
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressOfLens {
  Socials(input: {filter: {identity: {_in: ["lens/@prashantbagga", "lens/@betashop9", "lens/@vitalik"]}, dappName: { _eq: farcaster}}, blockchain: ethereum}) {
    Social {
      userAddress
      dappName
      profileName
    }
  }
  Domains(input: {filter: {owner: {_in: ["lens/@prashantbagga", "lens/@betashop9", "lens/@vitalik"]}}, blockchain: ethereum}) {
    Domain {
      dappName
      name
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "userAddress": "0x66bd69c7064d35d146ca78e6b186e57679fba249",
          "dappName": "farcaster",
          "profileName": "betashop.eth"
        },
        {
          "userAddress": "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
          "dappName": "farcaster",
          "profileName": "vitalik.eth"
        },
        {
          "userAddress": "0xaa11ebefbb4723debabbfc85932c11f358e68306",
          "dappName": "farcaster",
          "profileName": "prxshant.eth"
        },
      ]
    },
    "Domains": {
      "Domain": [
        {
          "dappName": "ens",
          "name": "skynft.eth"
        },
        {
          "dappName": "ens",
          "name": "quantumexchange.eth"
        },
        {
          "dappName": "ens",
          "name": "vitalik.daohall.eth"
        },
        // more domains
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding resolving Lens handle(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Domains API Reference](../../api-references/api-reference/domains-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
