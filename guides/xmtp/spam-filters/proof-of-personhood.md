---
description: >-
  Learn how you can build a proof of personhood mechanism through on-chain and
  off-chain data indexed by Airstack to determine if a user can be considered a
  spammer in your XMTP messaging app.
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

# 🎭 Proof of Personhood

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

In this tutorial, you will learn how to create a proof of personhood mechanism through on-chain and off-chain data to determine if a user is a spammer or not.

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to create proof of personhood mechanism by:

* [Token Transfers](proof-of-personhood.md#token-transfers)
* [Token Balances](proof-of-personhood.md#token-balances)
* [Has Primary ENS](proof-of-personhood.md#has-primary-ens)
* [Has Lens Profile](proof-of-personhood.md#has-lens-profile)
* [Has Farcaster Account](proof-of-personhood.md#has-farcaster-account)
* [Has Non-Virtual POAPs](proof-of-personhood.md#has-non-virtual-poaps)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [XMTP](https://xmtp.org)

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

<figure><img src="../../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Best Practice

While choosing a specific criteria will significantly decrease the number of spam appearing on your user's XMTP inbox, It is best practice that you **combine** the multiple criterion given here to build your **spam filter**.

This is done to provide **multiple layers of filtration** that will make it nearly impossible for spammers to have their messages slide into your users' XMTP inbox.

## Token Transfers

You can build proof of personhood by checking if there are any token transfers history from the given user:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has any token transfers is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if a user is a real person.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/dvRSbWiNKG" %}
Show me token transfers by vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  ethereum: TokenTransfers(
    input: {filter: {from: {_in: ["vitalik.eth"]}}, blockchain: ethereum}
  ) {
    TokenTransfer {
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      transactionHash
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  polygon: TokenTransfers(
    input: {filter: {from: {_in: ["vitalik.eth"]}}, blockchain: polygon}
  ) {
    TokenTransfer {
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      transactionHash
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "ethereum": {
<strong>      "TokenTransfer": [ // If TokenTransfer array is not empty, then there are token transfers 
</strong>        {
          "from": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth"
              },
              {
                "name": "7860000.eth"
              },
              {
                "name": "offchainexample.eth"
              },
              {
                "name": "brianshaw.eth"
              },
              {
                "name": "vbuterin.stateofus.eth"
              },
              {
                "name": "quantumsmartcontracts.eth"
              },
              {
                "name": "Vitalik.eth"
              },
              {
                "name": "openegp.eth"
              },
              {
                "name": "vitalik.cannafam.eth"
              },
              {
                "name": "VITALIK.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "",
                "userId": "5650",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              },
              {
                "dappName": "lens",
                "profileName": "vitalik.lens",
                "profileTokenId": "100275",
                "profileTokenIdHex": "",
                "userId": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ]
          },
          "to": {
            "addresses": [
              "0xd8b75eb7bd778ac0b3f5ffad69bcc2e25bccac95"
            ],
            "domains": [
              {
                "name": "toastmybread.eth"
              },
              {
                "name": "daerbymtsaot.eth"
              }
            ],
            "socials": null
          },
          "transactionHash": "0xd2e0d4e8aae125a7edae14c7dab106c1620be136b239e7f9dbd60861034b0c25"
        },
        // other token transfers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If the `ethereum.TokenTransfer` or the `polygon.TokenTransfer` array has non-zero length, then the user have a history of transferring token to other users and thus can be **considered non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Token Balances

You can build proof of personhood by checking if there are any ERC20/721/1155 tokens  hold by the given user:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has any token balances is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if a user is a real person.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/5teoS4ZS1X" %}
show me vitalik.eth token balances on Ethereum and Polygon
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Ethereum: TokenBalances(
    input: {filter: {owner: {_in: ["vitalik.eth"]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      tokenAddress
      tokenId
      amount
      tokenType
      token {
        name
        symbol
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
  Polygon: TokenBalances(
    input: {filter: {owner: {_in: ["vitalik.eth"]}}, blockchain: polygon, limit: 50}
  ) {
    TokenBalance {
      tokenAddress
      tokenId
      amount
      tokenType
      token {
        name
        symbol
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
<strong>      "TokenBalance": [ // Shows that hold some tokens in Ethereum
</strong>        {
          "tokenAddress": "0x75c97384ca209f915381755c582ec0e2ce88c1ba",
          "tokenId": "",
          "amount": "47077057573",
          "tokenType": "ERC20",
          "token": {
            "name": "FINE",
            "symbol": "FINE"
          }
        },
        {
          "tokenAddress": "0xd56736e79093d31be093ba1b5a5fe32e054b9592",
          "tokenId": "",
          "amount": "57415038814950477",
          "tokenType": "ERC20",
          "token": {
            "name": "Nucleus",
            "symbol": "NUCLEUS"
          }
        },
        {
          "tokenAddress": "0xa362cd1e14705f0bdd9347fccffe0d67a7cf6e2b",
          "tokenId": "",
          "amount": "400000000000000000000",
          "tokenType": "ERC20",
          "token": {
            "name": "Chloe Clem",
            "symbol": "CHLOE"
          }
        },
        // more tokens
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If the `Ethereum.TokenBalance` or the `Polygon.TokenBalance` array has non-zero length, then the user have some ERC20/721/1155 tokens hold in either Ethereum or Polygon and thus can be **considered non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Has Primary ENS

You can build proof of personhood by checking if the given user hold any primary ENS:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has primary ENS is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if a user is a real person.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/6re2GWPzw5" %}
show me if 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 has any primary ENS
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Domains(input: {filter: {owner: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}, isPrimary: {_eq: true}}, blockchain: ethereum}) {
    Domain {
      name
      owner
      isPrimary
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
<strong>          "name": "vitalik.eth", // This is the primary ENS
</strong>          "owner": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "isPrimary": true
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If the `Domain` array has non-zero length, then the user has primary ENS domain for their account, in the example it is `vitalik.eth`, and thus can be **considered non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Has Lens Profile

You can build proof of personhood by checking if the given user hold any Lens profile:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has Lens profile is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if a user is a real person.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/aoZPofhxXD" %}
show me if 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 has any Lens profile
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ethereum}
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
<strong>          "profileName": "vitalik.lens", // This is the Lens profile
</strong>          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3"
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If the `Social` array has non-zero length, then the user has Lens profile(s) for their account, in the example it is `vitalik.lens`, and thus can be **considered non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Has Farcaster Account

You can build proof of personhood by checking if the given user hold any Farcaster account:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has Farcaster account is **NOT** a foolproof method to remove spammers from your user's inbox.

Thus, be sure to combine multiple criterion to provide multiple layer of filtration to prove if a user is a real person.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/4GPOVP5rUE" %}
show me if 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 has any Farcaster profile
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"]}}, blockchain: ethereum}
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
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
<strong>          "profileName": "vitalik.eth", // This is the Farcaster account
</strong>          "userId": "5650",
          "userAssociatedAddresses": [
            "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ]
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

If the `Social` array has non-zero length, then the user has Farcaster account(s) for their account, in the example it is `vitalik.eth`, and thus can be **considered non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Has Non-Virtual POAPs

You can build proof of personhood by checking if the given user ever attended and obtained any non-virtual POAPs:

{% hint style="success" %}
:first\_place: **BEST PRACTICES**

It is important to note that **ONLY** checking if a user has non-virtual POAPs is **NOT** a foolproof method to remove spammers from your user's inbox.

While attending non-virtual POAPs provide a strong indication that a user is a real person, it is still best practice to combine multiple criterion to provide multiple layer of filtration to avoid any unwanted spamming.
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/EjEAFk2Xv7" %}
show me all POAPs owned by vitalik.eth and see if they are virtual or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwnedByVitalik {
  Poaps(
    input: {filter: {owner: {_in: ["vitalik.eth"]}}, blockchain: ALL, limit: 50}
  ) {
    Poap {
      mintOrder
      mintHash
      poapEvent {
        isVirtualEvent
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "mintOrder": 5,
          "mintHash": "0x4974ddc3ada2100b8e4cb2e17fb993324f71e0676cdd33dfff107899fca16e90",
          "poapEvent": {
<strong>            "isVirtualEvent": false // This is not virtual event
</strong>          }
        },
        {
          "mintOrder": 12,
          "mintHash": "0xdbb3a298401c221e6596557cecd0c0c8944e0dd538b7d81b6d97449b5911ce98",
          "poapEvent": {
<strong>            "isVirtualEvent": true // This is virtual event
</strong>          }
        },
        // more POAPs
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All you need to check is whether there is at least 1 POAP with the `isVirtualEvent` field shown as `false`. If yes, then the given user have ever attended a non-virtual POAP event and thus can be **considered a non-spammer**.

Otherwise, the user should be **considered a potential spammer**.

## Developer Support

If you have any questions or need help regarding creating a proof of personhood to classify spammers on your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenTransfers API Reference](../../../api-references/api-reference/tokentransfers-api/)
* [TokenBalances API Reference](../../../api-references/api-reference/tokenbalances-api/)
* [Poaps API Reference](../../../api-references/api-reference/poaps-api/)
* [Domains API Reference](../../../api-references/api-reference/domains-api/)
* [Socials API Reference](../../../api-references/api-reference/socials-api/)
* [Known Senders](known-senders.md)
* [High Probability of Connection](high-probability-of-connection.md)
