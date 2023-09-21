---
description: >-
  Learn how to enable users to access certain features only if they have a Lens
  profile or a combination of Lens + other criteria such as a specific POAP or
  NFT.
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

# 🚪 Token Gating

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to build token gating systems in your dapp that grant feature access to Lens profile(s) based on specific on-chain criteria.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Gating only user(s) that have Lens Profile](token-gating.md#gating-only-user-s-that-have-lens-profile)
* [Gating only user(s) that have Lens Profile and NFT](token-gating.md#gating-only-user-s-that-have-lens-profile-and-nft)
* [Gating only user(s) that have Lens Profile and POAP](token-gating.md#gating-only-user-s-that-have-lens-profile-and-poap)

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

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Gating only user(s) that have Lens Profile

You can implement token gating by checking whether users have Lens profile:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/TpGSzOq9tx" %}
Show the Lens profile of bradorbradley.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenGatingLens {
  Socials(
    input: {filter: {identity: {_eq: "bradorbradley.eth"}, dappName: {_eq: lens}}, blockchain: ethereum}
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
          "profileName": "westlakevillage.lens",
          "profileTokenId": "99755",
          "profileTokenIdHex": "0x0185ab",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "brad.lens",
          "profileTokenId": "116598",
          "profileTokenIdHex": "0x01c776",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "bradorbradley.lens",
          "profileTokenId": "36",
          "profileTokenIdHex": "0x024",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "profileName": "hanimourra.lens",
          "profileTokenId": "116239",
          "profileTokenIdHex": "0x01c60f",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the length of the `data.Socials.Social` array is 0, then it implies that the user has no Lens profile.

Otherwise, the user does and can be given access to a the desired feature.

## Gating only user(s) that have Lens Profile and NFT

You can implement token gating by checking whether users have both Lens profile and the given NFT:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/VERJn9Uhbd" %}
Show the NFT balance of 0x8eC94086A724cbEC4D37097b8792cE99CaDCd520 on specific NFTs
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_in: ["0x977e43ab3eb8c0aece1230ba187740342865ee78", "0x9d90669665607f08005cae4a7098143f554c59ef"]}, owner: {_eq: "0x8eC94086A724cbEC4D37097b8792cE99CaDCd520"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": "0x0185ab",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": "0x01c776",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": "0x024",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": "0x01c60f",
                "userAssociatedAddresses": [
                  "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                ]
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

If the length of the `data.TokenBalances.TokenBalance` array is 0, then it implies that the user has no given NFT held.

Otherwise, the user have at least one of the given NFT and then can have the `owner.socials` to be checked further to confirm if the user has any Lens profile.

If `owner.socials` has length 0, then similarly the user has no Lens profile.

Otherwise, the user has Lens profile and can be given access to a the desired feature.

## Gating only user(s) that have Lens Profile and POAP

You can implement token gating by checking whether users have both Lens profile and the given POAP:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/izTMNkr9m9" %}
Show if 0x4455951fa43b17bd211e0e8ae64d22fb47946ade hold some given specific POAPs and have Lens profile
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(
    input: {filter: {eventId: {_in: ["127462", "141910"]}, owner: {_eq: "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"}}, blockchain: ALL}
  ) {
    Poap {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
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
          "owner": {
            "socials": [
              {
                "profileName": "0x131.lens",
                "profileTokenId": "73916",
                "profileTokenIdHex": "0x0120bc",
                "userAssociatedAddresses": [
                  "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"
                ]
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

If the length of the `data.Poaps.Poap` array is 0, then it implies that the user has no given POAP held.

Otherwise, the user have at least one of the given POAP and then can have the `owner.socials` to be checked further to confirm if the user has any Lens profile.

If `owner.socials` has length 0, then similarly the user has no Lens profile.

Otherwise, the user has that Lens profile and can be given access to a the desired feature.

## Developer Support

If you have any questions or need help regarding token gating, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
