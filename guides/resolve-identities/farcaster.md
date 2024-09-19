---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Farcaster
  name or ID to other web3 identities (ENS, Ethereum address, Solana address).
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

* [Get Farcaster profiles of a given Solana address](farcaster.md#get-farcaster-profiles-of-a-given-solana-address)
* [Get Solana addresses of Farcaster user](farcaster.md#get-all-solana-addresses-of-farcaster-user)
* [Get Farcaster Accounts Of ENS Domain(s)](farcaster.md#get-farcaster-accounts-of-ens-domain-s)
* [Get the Ethereum address and ENS from a given Farcaster(s)](farcaster.md#get-the-ethereum-addresses-and-ens-from-a-given-farcaster-username-s)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
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

## Get Farcaster profiles of a given Solana address

You can resolve Solana address to get all the Farcaster profiles owned by the given Solana address by using the [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ewk0zjfkll" %}
Show me the Farcaster profile of solana address 9t92xZy9q5SyfKBH4rZwEDFXjaZKgzj5PgviPhKtBjyy
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        identity: { _eq: "9t92xZy9q5SyfKBH4rZwEDFXjaZKgzj5PgviPhKtBjyy" }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      fid: userId
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
          "profileName": "v",
          "fid": "2",
          "userAssociatedAddresses": [
            "0x4114e33eb831858649ea3702e1c9a2db3f626446",
            "0x91031dcfdea024b4d51e775486111d2b2a715871",
            "0x182327170fc284caaa5b1bc3e3878233f529d741",
            "0xf86a7a5b7c703b1fd8d93c500ac4cc75b67477f0",
            "9t92xZy9q5SyfKBH4rZwEDFXjaZKgzj5PgviPhKtBjyy"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Solana addresses of Farcaster user

You can resolve a Farcaster user to their Solana addresses by using [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4oizUq1TtG" %}
Show me the Solana connected address of Farcaster user v
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: farcaster }, profileName: { _eq: "v" } }
      blockchain: ethereum
    }
  ) {
    Social {
      connectedAddresses {
        address
        blockchain
        chainId
        timestamp
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
          "connectedAddresses": [
            {
              "address": "9t92xZy9q5SyfKBH4rZwEDFXjaZKgzj5PgviPhKtBjyy",
              "blockchain": "solana",
              "chainId": "900",
              "timestamp": "2024-02-16T22:13:14Z"
            },
            {
              "address": "0x91031dcfdea024b4d51e775486111d2b2a715871",
              "blockchain": "ethereum",
              "chainId": "1",
              "timestamp": "2023-04-28T17:42:20Z"
            },
            {
              "address": "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "blockchain": "ethereum",
              "chainId": "1",
              "timestamp": "2023-07-26T20:46:33Z"
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

## Get Farcaster Accounts Of ENS Domain(s)

You can resolve ENS domain(s) to their Farcaster accounts by using [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/hNUNtazgv1" %}
Show Farcaster name of prxshant.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcaster {
  Socials(
    input: {
      filter: {
        identity: { _in: ["prxshant.eth"] }
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

## Get the Ethereum addresses and ENS from a given Farcaster username(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/W43n5Ow33G" %}
Show the 0x addresses and ENS of fc\_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcaster {
  Socials(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        profileName: { _in: ["betashop.eth", "dwr.eth"] }
      }
      blockchain: ethereum
    }
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
