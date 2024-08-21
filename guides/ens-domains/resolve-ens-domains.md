---
description: >-
  Learn how to resolve ENS and Offchain Domains (Namestone & cb.id) to 0x
  address, Farcaster, and Reverse Resolution.
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

# 🆔 Resolve ENS Domains

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get ENS and Offchain Domains (Namestone & cb.id) of user(s)](resolve-ens-domains.md#get-ens-and-offchain-domains-namestone-and-cb.id-of-user-s)
* [Get All 0x addresses of ENS Domain(s)](resolve-ens-domains.md#get-all-0x-addresses-of-ens-domain-s)
* [Get All Farcaster accounts owned by 0x address or ENS](resolve-ens-domains.md#get-all-farcaster-accounts-owned-by-ens)
* [Get the 0x address and Farcaster from a given cb.id (Offchain)](resolve-ens-domains.md#get-the-0x-address-and-farcaster-from-a-given-cb.id-offchain)
* [Get the 0x address and Farcaster from a given Namestone Subdomain (Offchain)](resolve-ens-domains.md#get-the-0x-address-and-farcaster-from-a-given-namestone-subdomain-offchain)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

## Get ENS and Offchain Domains (Namestone & cb.id) of user(s)

You can fetch the ENS and offchain domains (Namestone & cb.id) of user(s) by using the [`Domains`](../../api-references/api-reference/domains-api.md) API and providing either the 0x address, Lens, or Farcaster to `resolvedAddress`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/PEqpmpWvDg" %}
Show me the ENS and offchain domains (Namestone & cb.id) of users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENSNameStoneCBID {
  Domains(
    input: {
      filter: {
        resolvedAddress: {
          _in: [
            "0xc7486219881C780B676499868716B27095317416"
            "lens/@yosephks"
            "fc_fname:yosephks.eth"
          ]
        }
      }
      blockchain: ethereum
    }
  ) {
    Domain {
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
    "Domains": {
      "Domain": [
        {
          "name": "yosephks.eth"
        },
        {
          "name": "yosephks.cb.id"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All 0x addresses of ENS Domain(s)

You can resolve an array of ENS domain(s) to their 0x addresses:

### Try Demo

{% embed url="https://app.airstack.xyz/query/42ywXPi3yg" %}
Show 0x addresses of vitalik.eth and naderdabit.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: {
      filter: { name: { _in: ["vitalik.eth", "naderdabit.eth"] } }
      blockchain: ethereum
    }
  ) {
    Domain {
      resolvedAddress
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
    "Domains": {
      "Domain": [
        {
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "name": "vitalik.eth"
        },
        {
          "resolvedAddress": "0xb2ebc9b3a788afb1e942ed65b59e9e49a1ee500d",
          "name": "naderdabit.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster accounts owned by ENS

You can resolve any ENS to their Farcaster accounts, which comprise of Farcaster:

### Try Demo

{% embed url="https://app.airstack.xyz/query/BXb4j3H7bI" %}
Show Farcaster accounts owned by vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetWeb3SocialsOfLens {
  Socials(
    input: {
      filter: {
        identity: { _in: ["vitalik.eth"] }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
      profileTokenId
      profileTokenIdHex
      userId
      userAssociatedAddresses
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
          "dappName": "farcaster",
          "profileName": "vitalik.eth",
          "profileTokenId": "53546877787533711135172528420529478392632952428887988304966222706809455509504",
          "profileTokenIdHex": "0x0766275746572696e000000000000000000000000000000000000000000000000",
          "userId": "5650",
          "userAssociatedAddresses": [
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the 0x address and Farcaster from a given cb.id (Offchain)

You can get the 0x address and Farcaster of cb.ids:

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
    input: {
      filter: { name: { _in: ["yosephks.cb.id"] } }
      blockchain: ethereum
    }
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

## Get the 0x address and Farcaster from a given Namestone Subdomain (Offchain)

You can get the 0x address and Farcaster of [Namestone](https://namestone.xyz/) subdomains:

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
    input: {
      filter: { name: { _in: ["namestone.testbrand.eth"] } }
      blockchain: ethereum
    }
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

### Developer Support

If you have any questions or need help regarding resolving identities for ENS domain(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Resolve Identities](../resolve-identities/)
  * [ENS](../resolve-identities/ens.md)
  * [Farcaster](../resolve-identities/farcaster.md)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
