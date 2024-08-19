---
description: >-
  Learn how to use Airstack to universally resolve and reverse resolve Solana
  addresses to other web3 identities (Farcaster, ENS, Lens, 0x address).
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

# 🆔 Resolve Identities

## Table Of Contents

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get All 0x Addresses Connected To Solana Address](resolve-identities.md#get-all-0x-addresses-connected-to-solana-address)
* [Get All Solana Addresses Connected To 0x Address](resolve-identities.md#get-all-solana-addresses-connected-to-0x-address)
* [Get All Web3 Social Accounts (Farcaster, Lens) and ENS Domains Resolved From Solana Address](resolve-identities.md#get-all-web3-social-accounts-farcaster-lens-and-ens-domains-resolved-from-solana-address)
* [Get All The Solana addresses from a given ENS name](resolve-identities.md#get-all-the-solana-addresses-from-a-given-ens-name)
* [Get All The Solana addresses from a given Namestone Subdomain or cb.id (Offchain)](resolve-identities.md#get-all-the-solana-addresses-from-a-given-namestone-subdomain-or-cb.id-offchain)
* [Get All Solana addresses of Farcaster user](resolve-identities.md#get-all-solana-addresses-of-farcaster-user)

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

## Get All Web3 Social Accounts (Farcaster, Lens) and ENS Domains Resolved From Solana Address

You can resolve a Solana addresses to their web3 socials and ENS Domains (including offchain domains, e.g. Namestone & cb.id) using the [`Wallet`](broken-reference) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/3fqSzSZhnP" %}
Show the Farcaster, Lens, and ENS of solana address GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV
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
    farcaster: socials(input: { filter: { dappName: { _eq: farcaster } } }) {
      profileName
    }
    lens: socials(input: { filter: { dappName: { _eq: lens } } }) {
      profileName
    }
    domains {
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
    "Wallet": {
      "farcaster": [
        {
          "profileName": "alexjcomeau.eth"
        }
      ],
      "lens": [
        {
          "profileName": "lens/@alexj"
        }
      ],
      "domains": [
        {
          "name": "alexjcomeau.eth"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All The Solana addresses from a given ENS name

You can get the Solana addresses of ENS names by using the [`Domains`](../../api-references/api-reference/domains-api.md) API and checking through the registered `multichainAddresses` field that has `symbol` equal to **SOL**:

### Try Demo

{% embed url="https://app.airstack.xyz/query/UHJdGOCe1r" %}
Show me alexjcomeau.eth's multichain SOL address
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetUserDetailsFromENS {
  Domains(
    input: {
      filter: { name: { _eq: "alexjcomeau.eth" } }
      blockchain: ethereum
    }
  ) {
    Domain {
      multiChainAddresses {
        address
        symbol
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Domains": {
      "Domain": [
        {
          "multiChainAddresses": [
            {
              "address": "0xe0235804378c31948E81441f656D826eE5998Bc6",
              "symbol": "ETH"
            },
            {
              // This is the SOL address registered by user in ENS
<strong>              "address": "GJQUFnCu7ZJHxtxeaeskjnqyx8QFAN1PsiGuShDMPsqV",
</strong>              "symbol": "SOL"
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

## Get All The Solana addresses from a given Namestone Subdomain or cb.id (Offchain)

You can get the 0x addresses of offchain domains (Namestone/cb.id) by using the [`Domains`](../../api-references/api-reference/domains-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/E2MMhhm4ql" %}
Show me yosephks.cb.id's multichain SOL address
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(
    input: { filter: { name: { _eq: "yosephks.cb.id" } }, blockchain: ethereum }
  ) {
    Domain {
      multiChainAddresses {
        address
        symbol
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Domains": {
      "Domain": [
        {
          "multiChainAddresses": [
            {
              "address": "bc1qetgl5rx3uuhxek7erfc3wh97m3xkshx8pkdpr5",
              "symbol": "BTC"
            },
            {
              "address": "DETcngVSTXVbetWmRo9kdQe7xD9F19uqWk",
              "symbol": "DOGE"
            },
            {
              "address": "0xc7486219881C780B676499868716B27095317416",
              "symbol": "ETH"
            },
            {
              "address": "ltc1qwsqc8y09w59yqvlwy9c4fkqxu5md33vsr2uak6",
              "symbol": "LTC"
            },
            {
              // This is the SOL address connected to yosephks.cb.id (offchain)
<strong>              "address": "HyrNmmmce9W3rDdTQcZHyYvhuxPN6AaY3mVJcS9f4AZw",
</strong>              "symbol": "SOL"
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

## Developer Support

If you have any questions or need help regarding resolving Solana address(es), please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [ENS Domains Guide](../ens-domains/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Domains API Reference](../../api-references/api-reference/domains-api.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
