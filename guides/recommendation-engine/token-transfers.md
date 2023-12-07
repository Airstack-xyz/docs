---
description: >-
  Learn how to construct queries to build a recommendation engine based on token
  transfers.
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

# 💸 Token Transfers

Frequent token transfers between two parties could be a good indication that the parties involved know each other. Therefore, you can also use token transfers to build your contact recommendation feature.

To build such a contact recommendation feature, Airstack provides a [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api/) API for you to fetch all ERC20 token transfer data from either a user or multiple users on Ethereum, Polygon, and Base.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Token Transfers Of A User(s) on Ethereum](token-transfers.md#get-token-transfers-of-a-user-s-on-ethereum)
- [Get Token Transfers Of A User(s) on Polygon](token-transfers.md#get-token-transfers-of-a-user-s-on-polygon)
- [Get Token Transfers Of A User(s) on Base](token-transfers.md#get-token-transfers-of-a-user-s-on-base)
- [Get Token Transfers Of A User(s) on Multiple Chains](token-transfers.md#get-token-transfers-of-a-user-s-on-multiple-chains)
- [Get The Most Recent Token Transfers Of A User(s)](token-transfers.md#get-the-most-recent-token-transfers-of-a-user-s)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
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

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Token Transfers Of A User(s) on Ethereum

### Try Demo

{% embed url="https://app.airstack.xyz/query/Cxy3fiJAKd" %}
Show me token transfers from and to dwr.eth and fc_fname:vitalik.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2022-05-06T05:10:47Z",
          "token": {
            "symbol": "Karafuru 3D",
            "name": "Karafuru 3D",
            "decimals": 0
          },
          "from": {
            "addresses": ["0x0000000000000000000000000000000000000000"],
            "socials": [
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": "lens/@shatter"
              },
              {
                "dappName": "lens",
                "profileName": ""
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "dappling-starter-cra-ei1tt5.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "👨🏿‍🤝‍👨🏻.eth"
              },
              {
                "isPrimary": false,
                "name": "asdkjfas.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "baez.eth"
              },
              {
                "isPrimary": false,
                "name": "[701636f5d42ce2b973edc83cbb7ab390f844dfbd4d8d506df4780d12c4ba318b].com"
              },
              {
                "isPrimary": false,
                "name": "smartcontractsaudit.eth"
              },
              {
                "isPrimary": false,
                "name": "[c887159df7c26ccac34df4c89ddc49c27708f30b2521e1454f6a1743aaa49fc8].crobit.eth"
              },
              {
                "isPrimary": false,
                "name": "nextjs-3eqpjx.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "nikito3.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "wally.dev-poap.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "MINT"
        }
        // Other token transfers on Ethereum
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Token Transfers Of A User(s) on Polygon

### Try Demo

{% embed url="https://app.airstack.xyz/query/KcoFgsDUJR" %}
Show me token transfers from and to dwr.eth and fc_fname:vitalik.eth on Polygon
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2022-11-25T12:22:36Z",
          "token": {
            "symbol": "VITALIK",
            "name": "Cult Vitalik",
            "decimals": 0
          },
          "from": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "quantumexchange.eth"
              },
              {
                "isPrimary": false,
                "name": "7860000.eth"
              },
              {
                "isPrimary": false,
                "name": "offchainexample.eth"
              },
              {
                "isPrimary": false,
                "name": "brianshaw.eth"
              },
              {
                "isPrimary": false,
                "name": "vbuterin.stateofus.eth"
              },
              {
                "isPrimary": false,
                "name": "quantumsmartcontracts.eth"
              },
              {
                "isPrimary": false,
                "name": "Vitalik.eth"
              },
              {
                "isPrimary": false,
                "name": "openegp.eth"
              },
              {
                "isPrimary": false,
                "name": "vitalik.cannafam.eth"
              },
              {
                "isPrimary": false,
                "name": "VITALIK.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "TRANSFER"
        }
        // Other Polygon token transfers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Token Transfers Of A User(s) on Base

### Try Demo

{% embed url="https://app.airstack.xyz/query/HyCEvCtw1e" %}
Show me token transfers from and to dwr.eth and fc_fname:vitalik.eth on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
    }
  }
}
```

{% endtab %}

{% tab title="Second Tab" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2023-09-28T21:07:57Z",
          "token": {
            "symbol": "SWCFIT21",
            "name": "I Called Congress - FIT21",
            "decimals": 0
          },
          "from": {
            "addresses": ["0x0000000000000000000000000000000000000000"],
            "socials": [
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": "lens/@shatter"
              },
              {
                "dappName": "lens",
                "profileName": ""
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "dappling-starter-cra-ei1tt5.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "👨🏿‍🤝‍👨🏻.eth"
              },
              {
                "isPrimary": false,
                "name": "asdkjfas.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "baez.eth"
              },
              {
                "isPrimary": false,
                "name": "[701636f5d42ce2b973edc83cbb7ab390f844dfbd4d8d506df4780d12c4ba318b].com"
              },
              {
                "isPrimary": false,
                "name": "smartcontractsaudit.eth"
              },
              {
                "isPrimary": false,
                "name": "[c887159df7c26ccac34df4c89ddc49c27708f30b2521e1454f6a1743aaa49fc8].crobit.eth"
              },
              {
                "isPrimary": false,
                "name": "nextjs-3eqpjx.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "nikito3.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "wally.dev-poap.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "MINT"
        }
        // Other Base token transfers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Token Transfers Of A User(s) on Multiple Chains

### Try Demo

{% embed url="https://app.airstack.xyz/query/ARv7hFj3ki" %}
Show me token transfers from and to dwr.eth and fc_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetTokenTransfers {
  # first query on Ethereum
  ethereum: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
      blockchain
    }
  }
  # second query on Polygon
  polygon: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
      blockchain
    }
  }
  # third query on Polygon
  base: TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenTransfer {
      amount
      formattedAmount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          name
          dappName
        }
      }
      type
      blockchain
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
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2022-05-06T05:10:47Z",
          "token": {
            "symbol": "Karafuru 3D",
            "name": "Karafuru 3D",
            "decimals": 0
          },
          "from": {
            "addresses": ["0x0000000000000000000000000000000000000000"],
            "socials": [
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": "lens/@shatter"
              },
              {
                "dappName": "lens",
                "profileName": ""
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "dappling-starter-cra-ei1tt5.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "👨🏿‍🤝‍👨🏻.eth"
              },
              {
                "isPrimary": false,
                "name": "asdkjfas.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "baez.eth"
              },
              {
                "isPrimary": false,
                "name": "[701636f5d42ce2b973edc83cbb7ab390f844dfbd4d8d506df4780d12c4ba318b].com"
              },
              {
                "isPrimary": false,
                "name": "smartcontractsaudit.eth"
              },
              {
                "isPrimary": false,
                "name": "[c887159df7c26ccac34df4c89ddc49c27708f30b2521e1454f6a1743aaa49fc8].crobit.eth"
              },
              {
                "isPrimary": false,
                "name": "nextjs-3eqpjx.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "nikito3.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "wally.dev-poap.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "MINT",
          "blockchain": "ethereum"
        }
        // Other Ethereum token transfers
      ]
    },
    "polygon": {
      "TokenTransfer": [
        {
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2022-11-25T12:22:36Z",
          "token": {
            "symbol": "VITALIK",
            "name": "Cult Vitalik",
            "decimals": 0
          },
          "from": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "quantumexchange.eth"
              },
              {
                "isPrimary": false,
                "name": "7860000.eth"
              },
              {
                "isPrimary": false,
                "name": "offchainexample.eth"
              },
              {
                "isPrimary": false,
                "name": "brianshaw.eth"
              },
              {
                "isPrimary": false,
                "name": "vbuterin.stateofus.eth"
              },
              {
                "isPrimary": false,
                "name": "quantumsmartcontracts.eth"
              },
              {
                "isPrimary": false,
                "name": "Vitalik.eth"
              },
              {
                "isPrimary": false,
                "name": "openegp.eth"
              },
              {
                "isPrimary": false,
                "name": "vitalik.cannafam.eth"
              },
              {
                "isPrimary": false,
                "name": "VITALIK.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "TRANSFER",
          "blockchain": "polygon"
        }
        // Other Polygon token transfers
      ]
    },
    "base": {
      "TokenTransfer": [
        {
          "amount": "1",
          "formattedAmount": 1,
          "blockTimestamp": "2023-09-28T21:07:57Z",
          "token": {
            "symbol": "SWCFIT21",
            "name": "I Called Congress - FIT21",
            "decimals": 0
          },
          "from": {
            "addresses": ["0x0000000000000000000000000000000000000000"],
            "socials": [
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": ""
              },
              {
                "dappName": "lens",
                "profileName": "lens/@shatter"
              },
              {
                "dappName": "lens",
                "profileName": ""
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "dappling-starter-cra-ei1tt5.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "👨🏿‍🤝‍👨🏻.eth"
              },
              {
                "isPrimary": false,
                "name": "asdkjfas.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "baez.eth"
              },
              {
                "isPrimary": false,
                "name": "[701636f5d42ce2b973edc83cbb7ab390f844dfbd4d8d506df4780d12c4ba318b].com"
              },
              {
                "isPrimary": false,
                "name": "smartcontractsaudit.eth"
              },
              {
                "isPrimary": false,
                "name": "[c887159df7c26ccac34df4c89ddc49c27708f30b2521e1454f6a1743aaa49fc8].crobit.eth"
              },
              {
                "isPrimary": false,
                "name": "nextjs-3eqpjx.dappling.eth"
              },
              {
                "isPrimary": false,
                "name": "nikito3.dev-poap.eth"
              },
              {
                "isPrimary": false,
                "name": "wally.dev-poap.eth"
              }
            ]
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "dappName": "ens"
              },
              {
                "name": "7860000.eth",
                "dappName": "ens"
              },
              {
                "name": "offchainexample.eth",
                "dappName": "ens"
              },
              {
                "name": "brianshaw.eth",
                "dappName": "ens"
              },
              {
                "name": "vbuterin.stateofus.eth",
                "dappName": "ens"
              },
              {
                "name": "quantumsmartcontracts.eth",
                "dappName": "ens"
              },
              {
                "name": "Vitalik.eth",
                "dappName": "ens"
              },
              {
                "name": "openegp.eth",
                "dappName": "ens"
              },
              {
                "name": "vitalik.cannafam.eth",
                "dappName": "ens"
              },
              {
                "name": "VITALIK.eth",
                "dappName": "ens"
              }
            ]
          },
          "type": "MINT",
          "blockchain": "base"
        }
        // Other Base token transfers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Of A User(s)

### Try Demo

{% embed url="https://app.airstack.xyz/query/oYrGbtsJOv" %}
Show me the most recent token transfers from and to dwr.eth and fc_fname:vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: {
          from: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
          to: { _in: ["dwr.eth", "fc_fname:vbuterin"] }
        }
      }
      blockchain: ethereum
      limit: 50
      order: { blockTimestamp: DESC }
    }
  ) {
    TokenTransfer {
      amount
      blockTimestamp
      token {
        symbol
        name
        decimals
      }
      from {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      to {
        addresses
        socials {
          dappName
          profileName
        }
        domains {
          isPrimary
          name
        }
      }
      type
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "amount": "1458330000000000",
          "blockTimestamp": "2023-12-05T07:57:59Z",
          "token": {
            "symbol": "Antspepe",
            "name": "Antspepe",
            "decimals": 8
          },
          "from": {
            "addresses": ["0x9720a34e34096c4921eef16ba7936ab801d0c489"],
            "socials": null,
            "domains": null
          },
          "to": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vitalik.eth"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ],
            "domains": [
              {
                "isPrimary": false,
                "name": "quantumexchange.eth"
              },
              {
                "isPrimary": false,
                "name": "7860000.eth"
              },
              {
                "isPrimary": false,
                "name": "offchainexample.eth"
              },
              {
                "isPrimary": false,
                "name": "brianshaw.eth"
              },
              {
                "isPrimary": false,
                "name": "vbuterin.stateofus.eth"
              },
              {
                "isPrimary": false,
                "name": "quantumsmartcontracts.eth"
              },
              {
                "isPrimary": false,
                "name": "Vitalik.eth"
              },
              {
                "isPrimary": false,
                "name": "openegp.eth"
              },
              {
                "isPrimary": false,
                "name": "vitalik.cannafam.eth"
              },
              {
                "isPrimary": false,
                "name": "VITALIK.eth"
              }
            ]
          },
          "type": "TRANSFER"
        }
        // More recent Ethereum token transfers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating or building recommendation engine with token transfers data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api/)
- [On-Chain Graph](../onchain-graph.md)
