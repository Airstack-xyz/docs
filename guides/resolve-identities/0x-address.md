---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve 0x
  addresses to Solana addresses, web3 socials (Lens & Farcaster) and ENS Domains
  (including offchain Namestone & cb.id).
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

# 🔑 0x Address

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get All Solana Addresses Connected To 0x Address](0x-address.md#get-all-solana-addresses-connected-to-0x-address)
* [Get All 0x Addresses Connected To Solana Address](0x-address.md#get-all-0x-addresses-connected-to-solana-address)
* [Get All Web3 Social Accounts (Lens, Farcaster) and ENS Domains Resolved From An Array of 0x Addresses](0x-address.md#get-all-web3-social-accounts-lens-farcaster-and-ens-domains-resolved-from-an-array-of-0x-addresses)
* [Get All The 0x addresses from a given ENS name(s)](0x-address.md#get-all-the-0x-addresses-from-a-given-ens-name-s)
* [Get All The 0x addresses from a given Namestone Subdomain or cb.id (Offchain)](0x-address.md#get-all-the-0x-addresses-from-a-given-namestone-subdomain-or-cb.id-offchain)
* [Get All 0x addresses of Farcaster user(s)](0x-address.md#get-all-0x-addresses-of-farcaster-user-s)
* [Get All 0x addresses of Lens profile(s)](0x-address.md#get-all-0x-addresses-of-lens-profile-s)

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

## Get All Solana Addresses Connected To 0x Address

You can get all the solana addresses connected to a given 0x addresss by using the [`Wallet`](broken-reference) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/9vdc9156fG" %}
Show me all the Solana address connected to 0xe0235804378c31948e81441f656d826ee5998bc6
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Wallet(
    input: {identity: "0xe0235804378c31948e81441f656d826ee5998bc6", blockchain: ethereum}
  ) {
    farcaster: socials(input: {filter: {dappName: {_eq: farcaster}}}) {
<strong>      connectedAddresses { # Fetch all SOL connected addresses from Farcaster (if any)
</strong>        address
        chainId
        blockchain
        timestamp
      }
    }
    domains {
<strong>      multiChainAddresses { # Fetch all SOL address registered with ENS (if any)
</strong>        address
        symbol
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Wallet": {
      "farcaster": [
        {
<strong>          "connectedAddresses": [ // No SOL address connected in FC
</strong>            {
              "address": "0xe0235804378c31948e81441f656d826ee5998bc6",
              "chainId": "1",
              "blockchain": "ethereum",
              "timestamp": "2023-07-04T18:54:04Z"
            }
          ]
        }
      ],
      "domains": [
        {
          "multiChainAddresses": [
            {
              // This is the SOL address registered by user in ENS
<strong>              "address": "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV",
</strong>              "symbol": "SOL"
            },
            {
              "address": "0xe0235804378c31948E81441f656D826eE5998Bc6",
              "symbol": "ETH"
            }
          ]
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All 0x Addresses Connected To Solana Address

You can get all the 0x addresses connected to a given solana addresss by using the [`Wallet`](broken-reference) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lGD94GtqKH" %}
Show the 0x addresses of solana address GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV"
      blockchain: ethereum
    }
  ) {
    addresses
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Wallet": {
      "addresses": ["0xe0235804378c31948e81441f656d826ee5998bc6"]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Web3 Social Accounts (Lens, Farcaster) and ENS Domains Resolved From An Array of 0x addresses

You can resolve an array of 0x addresses to their web3 socials and ENS Domains (including offchain domains, e.g. Namestone & cb.id) using the [`Socials`](../../api-references/api-reference/socials-api.md) and [`Domains`](../../api-references/api-reference/domains-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/5O1VK0utN9" %}
Show web3 socials (Lens, Farcaster) and ENS resolved from 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  # Get All Web3 socials (Lens/Farcaster)
<strong>  Socials(
</strong>    input: {
      filter: {
        identity: {
          _in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
        }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      userAssociatedAddresses
      dappName
      profileName
    }
  }
  # Get All ENS domains, including offchain Namestone &#x26; cb.id
<strong>  Domains(
</strong>    input: {
      filter: {
        resolvedAddress: {
          _in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]
        }
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      resolvedAddress
      name
      isPrimary
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
          "userAssociatedAddresses": [
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "dappName": "farcaster",
<strong>          "profileName": "vitalik.eth" // Farcaster fname
</strong>        },
        {
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ],
          "dappName": "lens",
<strong>          "profileName": "lens/@vitalik" // Lens Profile
</strong>        }
      ]
    },
    "Domains": {
      "Domain": [
        {
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "name": "quantumdapps.eth",
          "isPrimary": false
        },
        {
          "resolvedAddress": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "name": "vitalik.eth",
<strong>          "isPrimary": true // This indicates that this is the primary ENS
</strong>        },
        // Other ENS domains
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All The 0x addresses from a given ENS name(s)

You can get the 0x addresses of ENS names by using the [`Domains`](../../api-references/api-reference/domains-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/2Tq7thalCi" %}
show me all the 0x addresses of vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetUserDetailsFromENS {
  Domains(
    input: {
      filter: {
        name: {
          # Add more ENS Domains into the array
<strong>          _in: ["vitalik.eth"]
</strong>        }
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      resolvedAddress
      name
    }
  }
}
</code></pre>
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
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All The 0x addresses from a given Namestone Subdomain or cb.id (Offchain)

You can get the 0x addresses of offchain domains (Namestone/cb.id) by using the [`Domains`](../../api-references/api-reference/domains-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ofFAk8Ex0b" %}
Show me all the 0x addresses of yosephks.cb.id
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCbDotID {
  Domains(
    input: {
      filter: {
        name: {
          # Add more namestone subdomains/cb.ids into the array
<strong>          _in: ["yosephks.cb.id"]
</strong>        }
      },
      blockchain: ethereum
    }
  ) {
    Domain {
      resolvedAddress
      name
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Domains": {
      "Domain": [
        {
          "resolvedAddress": "0xc7486219881c780b676499868716b27095317416",
          "name": "yosephks.cb.id"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All 0x addresses of Lens profile(s)

You can resolve an array of Lens profile(s) to their 0x addresses by using [`Socials`](../../api-references/api-reference/socials-api.md) API:

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/UEnpGZrGhp" %}
Show 0x addresses of lens/@nader and Lens profile id 0x0187b3
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfLens {
  Socials(
    input: {
      filter: {
        identity: { _in: ["lens/@nader", "lens_id:0x0187b3"] }
        dappName: { _eq: lens }
      }
      blockchain: ethereum
    }
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
          "profileName": "lens/@nader",
          "profileTokenId": "10402",
          "profileTokenIdHex": "0x028a2",
          "userAssociatedAddresses": [
            "0xb2ebc9b3a788afb1e942ed65b59e9e49a1ee500d"
          ]
        },
        {
          "profileName": "lens/@vitalik",
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

## Get All 0x addresses of Farcaster user(s)

You can resolve an array of Farcaster user(s) to their 0x addresses by using [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/rQQVTG8laa" %}
Show 0x addresses of Farcaster user name dwr.eth and user ID 1
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetAddressesOfFarcasters {
  Socials(
    input: {
      filter: {
        identity: {
          # Add more Farcaster fname or fid into the array
<strong>          _in: ["fc_fname:dwr.eth", "fc_fid:1"]
</strong>        }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      userAssociatedAddresses
    }
  }
}
</code></pre>
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

## Developer Support

If you have any questions or need help regarding resolving 0x address(es), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [ENS Domains Guide](../ens-domains/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
