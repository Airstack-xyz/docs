---
description: >-
  Learn how to resolve Lens profiles to 0x address, ENS, Farcaster, and XMTP and
  Reverse Resolution.
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

# 🆔 Resolve Lens Profiles

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to resolve a Lens profile name or ID to other [web3 identities](#user-content-fn-1)[^1] and vice versa.

In this guide you will learn how to use Airstack to:

* [Get All 0x addresses of Lens profile(s)](resolve-lens-profiles.md#get-all-0x-addresses-of-lens-profile-s)
* [Get all ENS domains owned by Lens profile(s)](resolve-lens-profiles.md#get-all-ens-domains-owned-by-lens-profile-s)
* [Get All Web3 Social Accounts (Lens, Farcaster) owned by 0x address or ENS](resolve-lens-profiles.md#get-all-web3-social-accounts-lens-farcaster-owned-by-0x-address-or-ens)
* [Get Farcaster of Lens Profile(s)](resolve-lens-profiles.md#get-farcaster-of-lens-profile-s)
* [Check If XMTP is Enabled for Lens profile(s)](resolve-lens-profiles.md#check-if-xmtp-is-enabled-for-lens-profile-s)

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

## Get All 0x addresses of Lens profile(s)

You can resolve an array of Lens profile(s) to their 0x addresses:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Qig0cA3dEv" %}
Show 0x addresses of nader.lens and Lens profile id 0x0187b3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfLens {
  Socials(
    input: {filter: {identity: {_in: ["nader.lens", "lens_id:0x0187b3"]}, dappName: {_eq: lens}}, blockchain: ethereum}
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

## Get all ENS domains owned by Lens profile(s)

You can resolve an array of Lens profile(s) to their ENS domains:

### Try Demo

{% embed url="https://app.airstack.xyz/query/UEnpGZrGhp" %}
Show all ENS domains owned by bradorbradley.lens and lens profile id 0x0187b3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENSOfLens {
  Domains(input: {filter: {owner: {_in: ["bradorbradley.lens", "lens_id:0x0187b3"]}}, blockchain: ethereum}) {
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
          "name": "v‍i‍t‍a‍l‍i‍k‍.eth"
        },
        {
          "name": "bradorbradley.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Web3 Social Accounts (Lens, Farcaster) owned by 0x address or ENS

You can resolve any 0x address or ENS to their web3 socials, which comprise of Lens and Farcaster:

### Try Demo

{% embed url="https://app.airstack.xyz/query/IWRDgNkqlQ" %}
Show web3 socials (Lens, Farcaster) owned by bradorbradley.eth and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetWeb3SocialsOfLens {
  Socials(input: {filter: {identity: {_in: ["bradorbradley.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ethereum}) {
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
          "dappName": "lens",
          "profileName": "westlakevillage.lens",
          "profileTokenId": "99755",
          "profileTokenIdHex": "0x0185ab",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
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
        },
        {
          "dappName": "lens",
          "profileName": "brad.lens",
          "profileTokenId": "116598",
          "profileTokenIdHex": "0x01c776",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "dappName": "lens",
          "profileName": "bradorbradley.lens",
          "profileTokenId": "36",
          "profileTokenIdHex": "0x024",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
          "userAssociatedAddresses": [
            "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
          ]
        },
        {
          "dappName": "lens",
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3",
          "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ]
        },
        {
          "dappName": "lens",
          "profileName": "hanimourra.lens",
          "profileTokenId": "116239",
          "profileTokenIdHex": "0x01c60f",
          "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
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

## Get Farcaster of Lens profile(s)

You can resolve an array of Lens profile(s) to their Farcaster name and ID, if any:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Z1f9IjxWEg" %}
Show Farcasters owned by betashop9.lens and lens profile id 0x0187b3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetFarcastersOfLens {
  Socials(
    input: {filter: {identity: {_in: ["betashop9.lens", "lens_id:0x0187b3"]}, dappName: {_eq: farcaster}}, blockchain: ethereum}
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
          "profileName": "betashop.eth",
          "userId": "602",
          "userAssociatedAddresses": [
            "0x66bd69c7064d35d146ca78e6b186e57679fba249",
            "0xeaf55242a90bb3289db8184772b0b98562053559"
          ]
        },
        {
          "profileName": "vitalik.eth",
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

## Check If XMTP is Enabled for Lens profile(s)

You can check if an array of Lens profile(s) have their XMTP enabled or not:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Jj3kxBbxP3" %}
Show if XMTP is enabled for stani.lens and Lens profile id 0x0187b3
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLensOfFarcasters {
  XMTPs(
    input: {blockchain: ALL, filter: {owner: {_in: ["stani.lens", "lens_id:0x0187b3"]}}}
  ) {
    XMTP {
      isXMTPEnabled
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
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "stani.lens",
                "profileTokenId": "5",
                "profileTokenIdHex": "0x05",
                "userAssociatedAddresses": [
                  "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
                ]
              },
              {
                "profileName": "lilgho.lens",
                "profileTokenId": "110056",
                "profileTokenIdHex": "0x01ade8",
                "userAssociatedAddresses": [
                  "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
                ]
              },
              {
                "profileName": "lensofficial.lens",
                "profileTokenId": "47319",
                "profileTokenIdHex": "0x0b8d7",
                "userAssociatedAddresses": [
                  "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
                ]
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
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
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding resolving identities for Lens profile(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Resolve Identities](../resolve-identities/)
  * [Lens](../resolve-identities/lens.md)
* [Has XMTP](../xmtp/)
  * [Check Lens Profile](../xmtp/check-single-user.md#by-lens-profile)
  * [Bulk Check Lens Profiles](../xmtp/check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
* [Lens Resolver](../../use-cases/lens/universal-resolver.md)
* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)

[^1]: 0x addresses, ENS domains, Lens, XMTP
