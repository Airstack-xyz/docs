---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Farcaster
  name or ID to other web3 identities (Lens, ENS, Ethereum address).
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

# 🟪 Farcaster

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating on-chain (Optimism) and off-chain (Farcaster hubs) data from Farcaster.

### Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Farcaster from a given user(s)](farcaster.md#get-farcaster-from-a-given-user-s)
* [Get the Ethereum address, Lens, and ENS from a given Farcaster(s)](farcaster.md#get-the-ethereum-address-lens-and-ens-from-a-given-farcaster-s)

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

## Get Farcaster from a given user(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/tZ3gMPNL7p" %}
Show Farcaster name of prxshant.eth and lens/@vitalik
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcaster {
  Socials(
    input: {filter: {identity: {_in: ["prxshant.eth", "lens/@vitalik"]}, dappName: {_eq: farcaster}}, blockchain: ethereum}
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
          "profileName": "vitalik.eth",
          "dappName": "farcaster"
        },
        {
          "profileName": "prxshant.eth",
          "dappName": "farcaster"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum address, Lens, and ENS from a given Farcaster(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/W43n5Ow33G" %}
Show the 0x addresses, ENS, and Lens of fc\_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressOfFarcasters {
  Socials(input: {filter: {identity: {_in: $address}}, blockchain: ethereum}) {
    Social {
      userAddress
      dappName
      profileName
    }
  }
  Domains(input: {filter: {owner: {_in: $address}}, blockchain: ethereum}) {
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
          "userAddress": "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
          "dappName": "farcaster",
          "profileName": "vitalik.eth"
        },
        {
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "dappName": "lens",
          "profileName": "lens/@vitalik"
        }
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
        // More ENS domains
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding resolving Farcaster user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Domains API Reference](../../api-references/api-reference/domains-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
