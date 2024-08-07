---
description: >-
  Learn how to get POAP balances of user(s), including images and metadata, on
  Gnosis and Ethereum.
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

# ⚡ POAP

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 applications and integrating POAP balances data from Gnosis and Ethereum.

In this guide you will learn how to use Airstack to [get All POAPs owned by user(s)](poap.md#get-all-poaps-owned-by-user-s).

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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get All POAPs Owned By User(s)

You can fetch all POAPs tokens owned by any user(s):

### Try Demo

{% embed url="https://app.airstack.xyz/query/ag9LjFzi4r" %}
Show POAPs owned by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByLensProfiles {
  Poaps(
    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
      }
      blockchain: ALL
    }
  ) {
    Poap {
      eventId
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
          }
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
          "eventId": "80393",
          "owner": {
            "socials": [
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "You met Cryptocomical at Lisbon 2022",
            "eventURL": "https://cards.io",
            "startDate": "2022-10-30T00:00:00Z",
            "endDate": "2022-11-15T00:00:00Z",
            "country": "Portugal",
            "city": "Lisbon",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/SAZVBaiZmZwlX6c7OxyUGw==/small.png"
              }
            }
          }
        },
        {
          "eventId": "47553",
          "owner": {
            "socials": [
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              }
            ]
          },
          "poapEvent": {
            "eventName": "Proof of rAAVE - ETHCC Paris 2022",
            "eventURL": "https://aave.com",
            "startDate": "2022-07-01T00:00:00Z",
            "endDate": "2022-07-01T00:00:00Z",
            "country": "",
            "city": "",
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/extra_small.png",
                "large": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/large.png",
                "medium": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/medium.png",
                "original": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/original_image.png",
                "small": "https://assets.airstack.xyz/image/poap/FpLVsf5or7YFwItupydyOg==/small.png"
              }
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

## Developer Support

If you have any questions or need help regarding fetching POAP balances of user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Balance Snapshots Guides](broken-reference)
* [Holder Snapshots Guides](broken-reference)
* [POAP Guides](../poap/)
* [POAPs In Common Guides](../tokens-in-common/poaps.md)
* [POAP Holders Guides](../token-holders/poap.md)
* [POAPs API Reference](../../api-references/api-reference/poaps-api.md)
