---
description: Learn how to check if Lens profiles have XMTP enabled on their account or not.
---

# 📬 Has XMTP

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

[XMTP](https://xmtp.org) is an Ethereum-based messaging protocol.

Developers building with [XMTP](https://xmtp.org) can use [Airstack](https://airstack.xyz) to enable [Lens](https://lens.xyz) users to seamlessly message Ethereum, ENS, Lens, and Farcaster users with XMTP-enabled addresses.&#x20;

[Airstack](https://airstack.xyz) is utilized to:&#x20;

1. check if users have [XMTP](https://xmtp.org) enabled (required in order to receive [XMTP](https://xmtp.org) messages)
2. resolve [Lens](https://lens.xyz) profile names or IDs to 0x Ethereum addresses and vice versa.&#x20;

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Check Lens Profile Has XMTP Enabled](has-xmtp.md#check-lens-profile-has-xmtp-enabled)
* [Bulk Check Lens Profiles Have XMTP Enabled](has-xmtp.md#bulk-check-lens-profiles-have-xmtp-enabled)
* [Get All 0x addresses of Lens profile(s)](has-xmtp.md#get-all-0x-addresses-of-lens-profile-s)
* [Get All Lens Profiles Owned by 0x address](has-xmtp.md#get-all-lens-profiles-owned-by-0x-address)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
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

  // Render your component using the data returned by the query
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

## Check Lens Profile Has XMTP Enabled

You can check if a Lens profile has XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/38TR2Gxuqf" %}
Check Lens Profile has XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "vitalik.lens"}}}) {
    XMTP {
      isXMTPEnabled
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Bulk Check Lens Profiles Have XMTP Enabled

You can bulk check a list of Lens profiles have XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/query/4wzNFy5ceS" %}
Bulk Check Lens Profiles Have XMTP (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query BulkFetchLensProfilesHaveXMTP {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: ["hoobastank.lens", "22776.lens", "barisadiguzel.lens"]}}, limit: 100}) {
    XMTP {
      isXMTPEnabled
      owner {
        socials {
          dappName
          profileName
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
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "hoobastank.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "22776.lens"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "dappName": "lens",
                "profileName": "barisadiguzel.lens"
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

## Get All 0x addresses of Lens profile(s)

You can resolve an array of Lens profile(s) to their 0x addresses:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/Qig0cA3dEv" %}
Show 0x addresses of nader.lens and Lens profile id 100275
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfLens {
  Socials(
    input: {filter: {identity: {_in: ["nader.lens", "lens_id:100275"]}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
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
          "profileName": "nader.lens",
          "profileTokenId": "10402",
          "profileTokenIdHex": "0x028a2",
          "userAssociatedAddresses": [
            "0xb2ebc9b3a788afb1e942ed65b59e9e49a1ee500d"
          ]
        },
        {
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3",
          "userAssociatedAddresses": [
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

## Get All Lens Profiles Owned by 0x address

You can resolve any 0x address to their Farcaster accounts:

### Try Demo

{% embed url="https://app.airstack.xyz/query/WAIEP13Nr5" %}
Show Lens owned by Ethereum address 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLensOfEthereumAddress {
  Socials(
    input: {filter: {identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: lens}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
      userAssociatedAddresses
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json

  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3",
          "userAssociatedAddresses": [
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

## Developer Support

If you have any questions or need help regarding checking if Lens profile(s) have XMTP enabled, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Has XMTP](../has-xmtp/)
  * [Check Lens Profile](../has-xmtp/check-single-user.md#by-lens-profile)
  * [Bulk Check Lens Profiles](../has-xmtp/check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
* [Lens Resolver](../../use-cases/lens/universal-resolver.md)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
