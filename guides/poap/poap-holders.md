---
description: >-
  Learn how to fetch holders of a given POAP(s) and how to fetch common holders
  of multiple given POAP(s) in a single API call using the Airstack API.
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

# 🗝 POAP Holders

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [POAP](https://poap.xyz/) applications and for integrating POAP on-chain data indexed directly from both Ethereum and Gnosis.

In this tutorial, you will learn how to fetch [POAP](https://poap.xyz/) holders of a given [POAP(s)](https://poap.xyz/) and how to fetch common holders of multiple given [POAPs](https://poap.xyz/).

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get POAP Holders Of A Given POAP(s)](poap-holders.md#get-poap-holders-of-a-given-poap-s)
* [Get POAP Common Holders Of Multiple POAPs](poap-holders.md#get-poap-common-holders-of-multiple-poaps)

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

## Get POAP Holders Of A Given POAP(s)

You can fetch all POAP holders of a given POAP(s) by specifying the `eventId` of the POAP, e.g. `14190` for [EthCC\[6\] – Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=\&address=141910\&tokenType=\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29\&inputType=POAP\&activeTokenInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=gnosis):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/00c05PCjs5" %}
show me all holders of EthCC\[6] - Attendee POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(input: {filter: {eventId: {_in: ["141910"]}}, blockchain: ALL, limit: 200}) {
    Poap {
      owner {
        addresses
        domains {
           name
        }
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
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
    "Poaps": {
      "Poap": [
                {
          "owner": {
            "addresses": [
              "0x7123ba74a7da85704c05021c421a8316930218e6"
            ],
            "domains": [
              {
                "name": "molpy.eth"
              }
            ],
            "socials": null,
            "xmtp": null
          }
        },
        {
          "owner": {
            "addresses": [
              "0x49ea3ca642e40a26cecc9789fc7da37c90cef943"
            ],
            "domains": [
              {
                "name": "delivan.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "delivan.lens",
                "profileTokenId": "121757",
                "profileTokenIdHex": "",
                "userAssociatedAddresses": [
                  "0x49ea3ca642e40a26cecc9789fc7da37c90cef943"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        {
          "owner": {
            "addresses": [
              "0x3e3127478fdcca6dbb3f010882b91520e2b88fce"
            ],
            "domains": [
              {
                "name": "ppolo.eth"
              }
            ],
            "socials": null,
            "xmtp": null
          }
        },
        // other EthCC[6] POAP holders
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get POAP Common Holders Of Multiple POAPs

You can fetch the common holders of multiple POAPs by providing the `eventId`s of each POAPs on every level of the `Poaps` API.

The more POAPs you would like to use as an input, the more `Poaps.owner` nesting you will need:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/nF1cGPr8uJ" %}
show me common holders of EthCC\[6] - Attendee and ETHDenver 2023 POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
<strong>  Poaps(input: {filter: {eventId: {_eq: "141910"}}, blockchain: ALL, limit: 200}) {
</strong>    Poap {
      owner {
<strong>        poaps(input: {filter: {eventId: {_eq: "103093"}}}) {
</strong>          owner {
            addresses
            domains {
              name
            }
            socials {
              dappName
              profileName
              profileTokenId
              profileTokenIdHex
              userAssociatedAddresses
            }
            xmtp {
              isXMTPEnabled
            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
<strong>                "owner": { // Hold both EthCC[6] and EthDenver POAP
</strong>                  "addresses": [
                    "0x5cb4ea65e7622f3746680451b36c3641e096ab99"
                  ],
                  "domains": [
                    {
                      "name": "ponnet.eth"
                    },
                    {
                      "name": "sponn.eth"
                    },
                    {
                      "name": "sponnet.eth"
                    },
                    {
                      "name": "avadao.eth"
                    }
                  ],
                  "socials": [
                    {
                      "dappName": "lens",
                      "profileName": "sponnet.lens",
                      "profileTokenId": "107203",
                      "profileTokenIdHex": "",
                      "userAssociatedAddresses": [
                        "0x5cb4ea65e7622f3746680451b36c3641e096ab99"
                      ]
                    }
                  ],
                  "xmtp": null
                }
              }
            ]
          }
        },
        {
          "owner": {
<strong>            "poaps": null // Holder of EthCC[6] POAP, but does not hold EthDenver POAP
</strong>          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching POAP balances, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Poaps API Reference](../../api-references/api-reference/poaps-api/)
* [Poaps API Examples](../../api-references/api-reference/poaps-api/poaps-api-examples.md)
* [Combinations](../combinations/)
  * [Multiple POAPs](../combinations/multiple-poaps.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
* [Tokens In Common](../tokens-in-common/)
  * [POAPs](../tokens-in-common/poaps.md)
