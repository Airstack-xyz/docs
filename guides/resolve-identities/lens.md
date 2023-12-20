---
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

Learn how to use Airstack to universally resolve and reverse resolve Lens handles to other web3 identities (Farcaster, ENS, Ethereum address). Airstack supports both Lens v1, and Lens v2 profile names in the API inputs, you can freely use stani.lens or lens/@stani.

## Table Of Contents

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
    input: {filter: {identity: {_in: ["0x4b70d04124c2996de29e0caa050a49822faec6cc", "betashop.eth", "fc_fname:vbuterin"]}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      dappName
      profileName
      userId
      profileImageContentValue{
        image{
          medium
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
    "Socials": {
      "Social": [
        {
          "dappName": "lens",
          "profileName": "lens/@betashop9",
          "userId": "0xeaf55242a90bb3289db8184772b0b98562053559",
          "profileImageContentValue": {
            "image": {
              "medium": "https://assets.airstack.xyz/image/social/ZZh4MAMUy3nBc21Ujz5IMsHFulAJlQYwXE27nrWEHoA=/medium.png"
            }
          }
        },
        {
          "dappName": "lens",
          "profileName": "lens/@prashantbagga",
          "userId": "0x4b70d04124c2996de29e0caa050a49822faec6cc",
          "profileImageContentValue": {
            "image": {
              "medium": "https://assets.airstack.xyz/image/social/kXSdPRb6NpBizLxqlmvRCeDuBUOX7B6R1DWSQ3qHBqk=/medium.jpg"
            }
          }
        },
        {
          "dappName": "lens",
          "profileName": "lens/@vitalik",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "profileImageContentValue": {
            "image": {
              "medium": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/medium.jpg"
            }
          }
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
  Socials(
    input: {filter: {identity: {_in: ["lens/@prashantbagga", "betashop9.lens", "lens/@vitalik"]}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      userAddress
      dappName
      profileName
      userAddressDetails {
        domains {
          name
          isPrimary
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
    "Socials": {
      "Social": [
        {
          "userAddress": "0xeaf55242a90bb3289db8184772b0b98562053559",
          "dappName": "lens",
          "profileName": "lens/@betashop9",
          "userAddressDetails": {
            "domains": [
              {
                "name": "jasongoldberg.eth",
                "isPrimary": false
              },
              {
                "name": "betashop.eth",
                "isPrimary": true
              }
            ]
          }
        },
        {
          "userAddress": "0x4b70d04124c2996de29e0caa050a49822faec6cc",
          "dappName": "lens",
          "profileName": "lens/@prashantbagga",
          "userAddressDetails": {
            "domains": [
              {
                "name": "prxshant.eth",
                "isPrimary": true
              },
              {
                "name": "prashantbagga.eth",
                "isPrimary": false
              }
            ]
          }
        },
        {
          "userAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "dappName": "lens",
          "profileName": "lens/@vitalik",
          "userAddressDetails": {
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              {
                "name": "7860000.eth",
                "isPrimary": false
              },
              {
                "name": "offchainexample.eth",
                "isPrimary": false
              },
              {
                "name": "brianshaw.eth",
                "isPrimary": false
              },
              {
                "name": "vbuterin.stateofus.eth",
                "isPrimary": false
              },
              {
                "name": "quantumsmartcontracts.eth",
                "isPrimary": false
              },
              {
                "name": "Vitalik.eth",
                "isPrimary": false
              },
              {
                "name": "openegp.eth",
                "isPrimary": false
              },
              {
                "name": "vitalik.cannafam.eth",
                "isPrimary": false
              },
              {
                "name": "VITALIK.eth",
                "isPrimary": false
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

If you have any questions or need help regarding resolving Lens handle(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
