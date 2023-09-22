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

## Token Transfers

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
```json
{
  "data": {
    "ethereum": {
      "TokenTransfer": [
        {
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
```
{% endtab %}
{% endtabs %}

## Token Balances

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
```json
{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
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
```
{% endtab %}
{% endtabs %}

## Has Primary ENS

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
```json
{
  "data": {
    "Domains": {
      "Domain": [
        {
          "name": "vitalik.eth",
          "owner": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
          "isPrimary": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Has Lens Profile

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
```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "vitalik.lens",
          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Has Farcaster Account

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
```json
{
  "data": {
    "Socials": {
      "Social": [
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

## Has Non-Virtual POAPs

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
            "isVirtualEvent": false // This is not virtual event
          }
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

## Developer Support

If you have any questions or need help regarding creating a proof of persoonhood to classify spammers on your XMTP messaging app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [SocialFollowers API Reference](../../../api-references/api-reference/socialfollowers-api.md)
* [Wallet API Reference](../../../api-references/api-reference/wallet-api/)
* [Proof of Personhood](proof-of-personhood.md)
