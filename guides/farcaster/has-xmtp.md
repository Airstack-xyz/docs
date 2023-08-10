---
description: >-
  Learn how to check if Farcaster name or ID has enabled XMTP and resolve
  Farcaster name or ID for XMTP messaging.
---

# 📬 Has XMTP

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

[XMTP](https://xmtp.org) is an Ethereum-based messaging protocol.

Developers building with [XMTP](https://xmtp.org) can use [Airstack](https://airstack.xyz) to enable [Farcaster](https://farcaster.xyz) users to seamlessly message Ethereum, ENS, Lens, and Farcaster users with XMTP-enabled addresses.&#x20;

[Airstack](https://airstack.xyz) is utilized to:&#x20;

1. check if users have [XMTP](https://xmtp.org) enabled (required in order to receive [XMTP](https://xmtp.org) messages)
2. resolve [Farcaster](https://farcaster.xyz) usernames to 0x Ethereum addresses and vice versa.&#x20;

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Check if Farcaster User Has XMTP Enabled](has-xmtp.md#check-farcaster-has-xmtp-enabled)
* [Bulk Check Farcaster Users Have XMTP Enabled](has-xmtp.md#bulk-check-farcasters-have-xmtp-enabled)
* [Get all 0x addresses of Farcaster user(s)](has-xmtp.md#get-all-0x-addresses-of-farcaster-user-s)
* [Get all Farcasters owned by 0x address](has-xmtp.md#get-all-farcasters-owned-by-0x-address)

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

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

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
import { init, fetchQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = "YOUR_QUERY"; // Replace with GraphQL Query

const { data, error } = fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = "YOUR_QUERY" # Replace with GraphQL Query

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

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your JSON endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Check Farcaster Has XMTP Enabled

You can check if a Farcaster has XMTP enabled or not by providing either the Farcaster name or ID:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/tHpIYqruY8" %}
Check Farcaster has XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "fc_fname:vbuterin"}}}) {
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

## Bulk Check Farcasters Have XMTP Enabled

You can bulk check a list of Farcasters have XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/uf0qK2hOaJ" %}
Check Farcaster Name and ID have XMTP enabled (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_in: ["fc_fname:betashop.eth", "fc_fid:5650"]}}}) {
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
          "isXMTPEnabled": true
        },
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

## Get all 0x addresses of Farcaster user(s)

You can resolve an array of Farcaster user(s) to their 0x addresses:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/rQQVTG8laa" %}
Show 0x addresses of Farcaster user name dwr.eth and user ID 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfFarcasters {
  Socials(
    input: {filter: {identity: {_in: ["fc_fname:dwr.eth", "fc_fid:1"]}}, blockchain: ethereum}
  ) {
    Social {
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
          "userAssociatedAddresses": [
            "0x8773442740c17c9d0f0b87022c722f9a136206ed",
            "0x86924c37a93734e8611eb081238928a9d18a63c0"
          ]
        },
        {
          "userAssociatedAddresses": [
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
          ]
        },
        {
          "userAssociatedAddresses": [
            "0xb877f7bb52d28f06e60f557c00a56225124b357f",
            "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
            "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
            "0x74232bf61e994655592747e20bdf6fa9b9476f79"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get all Farcasters owned by 0x address

You can resolve any 0x address to their Farcaster accounts:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/LZDbcSC7LK" %}
Show Farcasters owned by Ethereum address 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcastersOfEthereumAddress {
  Socials(
    input: {filter: {identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, dappName: {_eq: farcaster}}, blockchain: ethereum}
  ) {
    Social {
      profileName
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
          "profileName": "vitalik.eth",
          "userId": "5650",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077"
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

If you have any questions or need help regarding checking if Farcaster(s) have XMTP enabled, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Has XMTP](../has-xmtp/)
  * [Check Farcaster](../has-xmtp/check-single-user.md#by-farcaster-name-or-id)
  * [Bulk Check Farcasters](has-xmtp.md#bulk-check-farcasters-have-xmtp-enabled)
* [Resolve Farcaster Users](resolve-farcaster-users.md)
* [Farcaster Resolver Demo](../../use-cases/farcaster/universal-resolver.md)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [Domains API Reference](../../api-references/api-reference/domains-api/)
