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

## Table Of Contents

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

## Get Farcaster name from any other identity

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
    input: {
      filter: {
        identity: { _in: ["prxshant.eth", "lens/@vitalik"] }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      userId
      profileImage
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
          "userId": "5650",
          "profileImage": "https://i.imgur.com/gF9Yaeg.jpg"
        },
        {
          "profileName": "prxshant.eth",
          "userId": "4280",
          "profileImage": "https://pbs.twimg.com/profile_images/1379754227602399235/jNz1xW74_400x400.jpg"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get the Ethereum addresses, Lens, and ENS from a given Farcaster username(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/W43n5Ow33G" %}
Show the 0x addresses, ENS, and Lens of fc\_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcaster {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, 
    profileName: {_in: ["betashop.eth", "dwr.eth"]}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      userId
      profileImage
      userAssociatedAddresses
      userAssociatedAddressDetails {
        domains {
          name
          isPrimary
          resolvedAddress
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
          "profileName": "dwr.eth",
          "userId": "3",
          "profileImage": "https://res.cloudinary.com/merkle-manufactory/image/fetch/c_fill,f_png,w_256/https://lh3.googleusercontent.com/MyUBL0xHzMeBu7DXQAqv0bM9y6s4i4qjnhcXz5fxZKS3gwWgtamxxmxzCJX7m2cuYeGalyseCA2Y6OBKDMR06TWg2uwknnhdkDA1AA",
          "userAssociatedAddresses": [
            "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ],
          "userAssociatedAddressDetails": [
            {
              "domains": null
            },
            {
              "domains": [
                {
                  "name": "dwr.eth",
                  "isPrimary": true,
                  "resolvedAddress": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                },
                {
                  "name": "dwr.mirror.xyz",
                  "isPrimary": false,
                  "resolvedAddress": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                },
                {
                  "name": "danromero.eth",
                  "isPrimary": false,
                  "resolvedAddress": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                }
              ]
            },
            {
              "domains": null
            },
            {
              "domains": [
                {
                  "name": "vault.dwr.eth",
                  "isPrimary": false,
                  "resolvedAddress": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea"
                }
              ]
            },
            {
              "domains": [
                {
                  "name": "noun124.eth",
                  "isPrimary": true,
                  "resolvedAddress": "0xb877f7bb52d28f06e60f557c00a56225124b357f"
                }
              ]
            },
            {
              "domains": null
            }
          ]
        },
        {
          "profileName": "betashop.eth",
          "userId": "602",
          "profileImage": "https://i.seadn.io/gae/L6orcQneNpLm3BYUN1cDsOAalQdWtWPPEUAMuQzpqNzH1hgQMDom79OBfDC8JI7oPlRoZX_ie_CkBxIqxNeTOo8IiGo0iSwkvUhgIFo?w=500&auto=format",
          "userAssociatedAddresses": [
            "0x66bd69c7064d35d146ca78e6b186e57679fba249",
            "0xeaf55242a90bb3289db8184772b0b98562053559"
          ],
          "userAssociatedAddressDetails": [
            {
              "domains": null
            },
            {
              "domains": [
                {
                  "name": "jasongoldberg.eth",
                  "isPrimary": false,
                  "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
                },
                {
                  "name": "betashop.eth",
                  "isPrimary": true,
                  "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
                }
              ]
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

## Developer Support

If you have any questions or need help regarding resolving Farcaster user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
