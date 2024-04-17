---
description: >-
  Learn how to use Airstack to fetch onchain and offchain data of a wallet,
  which includes, ENS, Lens, Farcaster, XMTP, token balances, etc.
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

# 💰 Wallet

[Airstack](https://airstack.xyz) provides easy-to-use [Wallet API](../api-references/api-reference/wallet-api.md) for enriching Web3 applications with onchain and offchain data of a wallet, which includes, ENS, Lens, Farcaster, XMTP, token balances, etc.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get All User's ENS Domain](wallet.md#get-all-users-ens-domain)
- [Get All User's Lens Profiles](wallet.md#get-all-users-lens-profiles)
- [Get All User's Farcaster Account](wallet.md#get-all-users-farcaster-account)
- [Check If A User Have XMTP Enabled](wallet.md#check-if-a-user-have-xmtp-enabled)
- [Get User's Token Balances](wallet.md#get-users-token-balances)
- [Get User's Token Transfers](wallet.md#get-users-token-transfers)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

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

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get All User's ENS Domain

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to fetch all user's ENS domains (including primary and non-primary):

{% hint style="info" %}
If a user have more than **200 ENS domains** resolved to the address, then it is recommended that you use the [**Domains**](../api-references/api-reference/domains-api.md) API directly instead.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/X7ltaCyncL" %}
Show me all 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045's ENS domains
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    domains(input: { limit: 200 }) {
      name
      isPrimary
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
      "domains": [
        {
          "name": "quantumexchange.eth",
          "isPrimary": false
        },
        {
          "name": "7860000.eth",
          "isPrimary": false
        },
        {
          "name": "vitalik.eth",
          "isPrimary": true
        }
        // More ENS domain
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All User's Lens Profiles

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to fetch all user's Lens profiles:

{% hint style="info" %}
If a user have more than **200 Lens profiles** owned by the address, then it is recommended that you use the [**Socials**](../api-references/api-reference/socials-api.md) API directly instead.

For more details, follow this guide [here](lens/lens-profile-details.md#get-lens-profile-details-by-0x-address).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/XR5DcbYEa4" %}
Show me all 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045's Lens profiles
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    socials(input: { limit: 200, filter: { dappName: { _eq: lens } } }) {
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
    "Wallet": {
      "socials": [
        {
          "profileName": "lens/@vitalik"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All User's Farcaster Account

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to fetch all user's Farcaster accounts:

{% hint style="info" %}
If a user have more than **200 Farcaster accounts** owned by the address, then it is recommended that you use the [**Socials**](../api-references/api-reference/socials-api.md) API directly instead.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/jKBmz9k6MM" %}
Show me all 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045's Farcaster accounts
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    socials(input: { limit: 200, filter: { dappName: { _eq: farcaster } } }) {
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
    "Wallet": {
      "socials": [
        {
          "profileName": "vitalik.eth"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Check If A User Have XMTP Enabled

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to check if a user has XMTP enabled:

### Try Demo

{% embed url="https://app.airstack.xyz/query/p5J8Adtr3I" %}
Check if 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 have XMTP enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    xmtp {
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
    "Wallet": {
      "xmtp": [
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

## Get User's Token Balances

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to fetch all user's token (ERC20/721/1155) balances across Ethereum, Base, Zora, and other [Airstack supported chains](overview.md#supported-chains):

{% hint style="info" %}
For fetching token balances data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).

If a user have more than **200 tokens on a chain**, then it is recommended that you use the [**TokenBalances**](../api-references/api-reference/tokenbalances-api.md) API directly instead.

For more details, follow this guide [here](token-balances/).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/wv3y0UQiKL" %}
Show me token balances of 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    tokenBalances {
      tokenAddress
      tokenId
      tokenType
      token {
        name
        symbol
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
    "Wallet": {
      "tokenBalances": [
        {
          "tokenAddress": "0x00000000366b8a1ec86d6c8e00c861bf2245d946",
          "tokenId": "",
          "tokenType": "ERC20",
          "token": {
            "name": "iPhone 15",
            "symbol": "iPhone15",
            "logo": {
              "external": null,
              "small": null,
              "medium": null,
              "large": null,
              "original": null
            }
          },
          "tokenNfts": null
        }
        // Other Ethereum tokens
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get User's Token Transfers

You can use [`Wallet`](../api-references/api-reference/wallet-api.md) API to fetch all user's token (ERC20/721/1155) transfers across Ethereum, Base, Zora, and other [Airstack supported chains](overview.md#supported-chains):

{% hint style="info" %}
For fetching token transfers data from multiple chains, check out [Cross-Chain Queries](basics/cross-chain-queries.md).

If a user have more than **200 token transfers on a chain**, then it is recommended that you use the [**TokenTransfers**](../api-references/api-reference/tokentransfers-api.md) API directly instead.

For more details, follow this guide [here](token-transfers.md#get-token-transfers-of-a-user-s-on-multiple-chains).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/VsXIPb6a9f" %}
show me all token transfers from or to 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Wallet(
    input: {
      identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      blockchain: ethereum
    }
  ) {
    tokenTransfers {
      from {
        addresses
      }
      to {
        addresses
      }
      tokenAddress
      tokenId
      tokenType
      token {
        name
        symbol
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
    "Wallet": {
      "tokenTransfers": [
        {
          "from": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            ]
          },
          "to": {
            "addresses": ["0xd8b75eb7bd778ac0b3f5ffad69bcc2e25bccac95"],
            ]
          },
          "tokenAddress": "0xc791c23da1161f8259c9094b65cf13f9d891a892",
          "tokenId": "606",
          "tokenType": "ERC721",
          "token": {
            "name": "Mars Mining Company",
            "symbol": "MARS",
          },
        }
        // Other Ethereum token transfers
      ],
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching balance snapshots data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Wallet API References](../api-references/api-reference/wallet-api.md)
- [Domains API References](../api-references/api-reference/domains-api.md)
- [Socials API References](../api-references/api-reference/socials-api.md)
- [TokenBalances API References](../api-references/api-reference/tokenbalances-api.md)
- [TokenTransfers API References](../api-references/api-reference/tokentransfers-api.md)
- [ENS Domain Guides](ens-domains/)
- [Lens Guides](lens/)
- [Farcaster Guides](../farcaster/farcaster/)
- [Token Balances Guides](token-balances/)
- [Token Transfers Guides](token-transfers.md)
- [Balance Snapshots Guides](balance-snapshots.md)
- [Holder Snapshots Guides](holder-snapshots.md)
