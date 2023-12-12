---
description: >-
  Learn how to resolve Farcaster(s) fname/username/FID to 0x address, ENS, Lens,
  and XMTP and Reverse Resolution.
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

# 🆔 Resolve Farcaster Users

## 🆔 Resolve Farcaster Users

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and for integrating onchain and offchain data with Farcaster.

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get All 0x addresses of Farcaster user(s)](resolve-farcaster-users.md#get-all-0x-addresses-of-farcaster-user-s)
* [Get all ENS domains owned by Farcaster user(s)](resolve-farcaster-users.md#get-all-ens-domains-owned-by-farcaster-user-s)
* [Get All Web3 Social Accounts (Farcaster, Lens) owned by 0x address or ENS](resolve-farcaster-users.md#get-all-web3-social-accounts-farcaster-lens-owned-by-0x-address-or-ens)
* [Get Lens Profile of Farcaster user(s)](resolve-farcaster-users.md#get-lens-profile-of-farcaster-user-s)
* [Check If XMTP is Enabled for Farcaster user(s)](resolve-farcaster-users.md#check-if-xmtp-is-enabled-for-farcaster-user-s)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

### Get Started

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

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Get All 0x addresses of Farcaster user(s)

You can resolve an array of Farcaster user(s) to their 0x addresses:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/rQQVTG8laa" %}
Show 0x addresses of Farcaster user name dwr.eth and user ID 1
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfFarcasters {
  Socials(
    input: {
      filter: { identity: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] } }
      blockchain: ethereum
    }
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

### Get all ENS domains owned by Farcaster user(s)

You can resolve an array of Farcaster user(s) to their ENS domains:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/6TtsANTcnd" %}
Show all ENS domains owned by Farcaster user name dwr.eth and user id 1
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetENSOfFarcasters {
  Domains(
    input: {
      filter: { owner: { _in: ["fc_fname:dwr.eth", "fc_fid:1"] } }
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
          "name": "noun124.eth"
        },
        {
          "name": "dwr.mirror.xyz"
        },
        {
          "name": "warpcast.eth"
        },
        {
          "name": "danromero.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All Web3 Social Accounts (Farcaster, Lens) owned by 0x address or ENS

You can resolve any 0x address or ENS to their web3 socials, which comprise of Farcaster and Lens:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/FHYr4PHYwu" %}
Show web3 social accounts (Farcaster, Lens) owned by dwr.eth and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetWeb3SocialsOfFarcasters {
  Socials(
    input: {
      filter: {
        identity: {
          _in: ["dwr.eth", "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
        }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
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
          "profileName": "vbuterin"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@vitalik"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@danromero"
        },
        {
          "dappName": "farcaster",
          "profileName": "dwr.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Lens Profile of Farcaster user(s)

You can resolve an array of Farcaster user(s) to their Lens profile, if any:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/7IYVTJBsZi" %}
Show Lens profiles owned by Farcaster user name dwr.eth and user id 5650
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLensOfFarcasters {
  Socials(
    input: {
      filter: {
        identity: { _in: ["fc_fname:dwr.eth", "fc_fid:5650"] }
        dappName: { _eq: lens }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
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
          "profileName": "lens/@vitalik"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@danromero"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Check If XMTP is Enabled for Farcaster user(s)

You can check if an array of Farcaster user(s) have their XMTP enabled or not:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/KHPF47JvdY" %}
Show if XMTP is enabled for Farcaster user name varunsrin.eth and user id 5650
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetXMTPOfFarcasters {
  XMTPs(
    input: {
      blockchain: ALL
      filter: { owner: { _in: ["fc_fname:varunsrin.eth", "fc_fid:5650"] } }
    }
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
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
                "profileName": "vbuterin"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "varunsrin.eth"
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

If you have any questions or need help regarding resolving identities for Farcaster user(s), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Resolve Identities](../resolve-identities/)
  * [Farcaster](../resolve-identities/farcaster.md)
* [Farcaster Resolver](broken-reference)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api.md)
