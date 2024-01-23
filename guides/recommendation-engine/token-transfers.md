---
description: Learn how to recommend users based on token transfers
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

Token transfers between two parties are a good indication that the parties involved know each other. Therefore, you can also use token transfers to build your recommendation engine.

To build such a recommendation engine, Airstack provides a [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API for you to fetch the users involved in a given user's all ERC20/721/1155 token transfers across Ethereum, Polygon, and Base.

The list of users, then can be compiled and returned to your application as recommendations.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Token Transfers Sent From A User on Ethereum](token-transfers.md#get-token-transfers-sent-from-a-user-on-ethereum)
* [Get Token Transfers Received By A User on Ethereum](token-transfers.md#get-token-transfers-received-by-a-user-on-ethereum)
* [Get Token Transfers Sent From A User on Polygon](token-transfers.md#get-token-transfers-sent-from-a-user-on-polygon)
* [Get Token Transfers Received By A User on Polygon](token-transfers.md#get-token-transfers-received-by-a-user-on-polygon)
* [Get Token Transfers Sent From A User on Base](token-transfers.md#get-token-transfers-sent-from-a-user-on-base)
* [Get Token Transfers Received By A User on Base](token-transfers.md#get-token-transfers-received-by-a-user-on-base)
* [Get Token Transfers Sent From A User on Multiple Chains](token-transfers.md#get-token-transfers-sent-from-a-user-on-multiple-chains)
* [Get Token Transfers Received By A User on Multiple Chains](token-transfers.md#get-token-transfers-received-by-a-user-on-multiple-chains)
* [Get The Most Recent Token Transfers Sent From A User](token-transfers.md#get-the-most-recent-token-transfers-sent-from-a-user-s)
* [Get The Most Recent Token Transfers Received By A User](token-transfers.md#get-the-most-recent-token-transfers-received-by-a-user)

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

## Get Token Transfers Sent From A User on Ethereum

You can fetch all token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), on Ethereum by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZGtUdm88Lv" %}
Show me token transfers sent from betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // this user received 25 USDC from betashop.eth
<strong>              "0x427a1c6dcaad92f886020a61e0b85be8e1c5ead5"
</strong>            ],
            "domains": [
              {
                "name": "mrwildenfree.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@mrwildenfree"
              },
              {
                "dappName": "farcaster",
                "profileName": "mrwildenfree"
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
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // This user received 25 USDC from betashop.eth
<strong>              "0x7ce0448b0fafab6cace981fffa967cf380a2cc33"
</strong>            ],
            "domains": [
              {
                "name": "jameshih.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "jameshih.eth"
              }
            ],
            "xmtp": null
          }
        },
        // Other token transfers from betashop.eth on Ethereum
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `to` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Received By A User on Ethereum

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), on Ethereum by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/d0vekk7Ako" %}
Show me token transfers received by betashop.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "ETHGlobal Pragma Lisbon"
          },
          "from": {
            "addresses": [
              // This user sent 1 ETHGlobal Pragma Lisbon NFT to betshop.eth
<strong>              "0x8f07bc36ff569312fdc41f3867d80bbd2fe94b76"
</strong>            ],
            "domains": [
              {
                "name": "jacob.willemsma.eth"
              },
              // Other ENS domains
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@jacobwillemsma"
              },
              {
                "dappName": "farcaster",
                "profileName": "wj"
              }
            ],
            "xmtp": null
          }
        },
        {
          "formattedAmount": 0.00005,
          "tokenType": "ERC20",
          "token": {
            "name": "Wrapped Ether"
          },
          "from": {
            "addresses": [
              // This user sent 0.00005 WETH to betashop.eth
              "0xe5bfab544eca83849c53464f85b7164375bdaac1"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        // Other Token Transfers received by betashop.eth on Ethereum
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `from` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Sent From A User on Polygon

You can fetch all token transfers sent from a given user, e.g. [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=ipeciura.eth\&rawInput=%23%E2%8E%B1ipeciura.eth%E2%8E%B1%28ipeciura.eth++ethereum+null%29\&inputType=ADDRESS), on Polygon by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/FLruoqln49" %}
Show me token transfers sent from ipeciura.eth on Polygon
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from ipeciura.eth
        from: {_eq: "ipeciura.eth"},
        # Only get token transfers with non-zero amount
        formattedAmount: {_gt: 0},
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "ipeciura.eth"]}
        }
      },
      blockchain: polygon,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC20",
          "token": {
            "name": "(PoS) Decentraland MANA"
          },
          "to": {
            "addresses": [
              // ipeciura.eth sent 1 (PoS) Decentraland MANA to this user
<strong>              "0x56845fd95c766ecb0ab450fe2d105a19440a6e35"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        {
          "formattedAmount": 0.002048431232076552,
          "tokenType": "ERC20",
          "token": {
            "name": "Wrapped Ether"
          },
          "to": {
            "addresses": [
              // ipeciura.eth sent 0.002048431232076552 WETH to this user
<strong>              "0x4a35582a710e1f4b2030a3f826da20bfb6703c09"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        // Other Polygon token transfers sent by ipeciura.eth
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `to` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Received By A User on Polygon

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), on Polygon by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/yO5BgLYsV0" %}
Show me token transfers received by betashop.eth on Polygon
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: polygon,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 520.95,
          "tokenType": "ERC20",
          "token": {
            "name": "@SNXPool.com"
          },
          "from": {
            "addresses": [
              // This user sent 520.95 @SNXPool.com to betashop.eth
<strong>              "0x50b728d8d964fd00c2d0aad81718b71311fef68a"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "Firefly Pass"
          },
          "from": {
            "addresses": [
              // This user sent 1 Firefly Pass NFT to betashop.eth
<strong>              "0x135c21b2da426760718e39da954974c4572ae9f6"
</strong>            ],
            "domains": [
              {
                "name": "lbae.eth"
              },
              // Other ENS domains
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@lwsnbaker"
              },
              {
                "dappName": "farcaster",
                "profileName": "lwsnbaker"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // Other Token Transfers received by betashop.eth on Polygon
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `from` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Sent From A User on Base

You can fetch all token transfers sent from a given user, e.g. [`jessepollak.eth`](https://explorer.airstack.xyz/token-balances?address=jessepollak.eth\&rawInput=%23%E2%8E%B1jessepollak.eth%E2%8E%B1%28jessepollak.eth++ethereum+null%29\&inputType=ADDRESS), on Base by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ubt2YJHZHj" %}
Show me token transfers sent from jessepollak.eth on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from jessepollak.eth
<strong>        from: {_eq: "jessepollak.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "jessepollak.eth"]}
        }
      },
      blockchain: base,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // jessepollak.eth sent 100 USDC to this user
<strong>              "0xdb6f1920a889355780af7570773609bd8cb1f498"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        {
          "formattedAmount": 99,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Base Coin"
          },
          "to": {
            "addresses": [
              // jessepollak.eth sent 99 USD Base Coin to this user
<strong>              "0x6cfc03917344d9403f77bbc8a62bf138da124715"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        // Other Base token transfers sent by jessepollak.eth
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `to` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Received By A User on Base

You can fetch all token transfers received by a given user, e.g. [`barmstrong.eth`](https://explorer.airstack.xyz/token-balances?address=barmstrong.eth\&rawInput=%23%E2%8E%B1barmstrong.eth%E2%8E%B1%28barmstrong.eth++ethereum+null%29\&inputType=ADDRESS), on Base by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/RIffwTnhus" %}
Show me token transfers received by barmstrong.eth on Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to barmstrong.eth
<strong>        to: {_eq: "barmstrong.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "barmstrong.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: base,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "BasePunks"
          },
          "from": {
            "addresses": [
              // This user sent 1 BasePunks NFT to barmstrong.eth
<strong>              "0xb3ce96c9b78e1f18ba250f909825cdfb50e9cafc"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "Bullcasso"
          },
          "from": {
            "addresses": [
              // This user sent 1 Bullcasso NFT to barmstrong.eth
<strong>              "0x3dc9c871672c3cccae681cc78df40394c49ebc09"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // Other Token Transfers received by barmstrong.eth on Base
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `from` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Sent From A User on Multiple Chains

You can fetch all token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, and Base, by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ikmf1dpeUy" %}
Show me all token transfers sent from betashop.eth on Ethereum, Polygon, and Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  # first query on Ethereum
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # second query on Polygon
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: polygon,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # third query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: base,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "Ethereum": {
      "TokenTransfer": [
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // betashop.eth sent 25 USDC to this user
<strong>              "0x427a1c6dcaad92f886020a61e0b85be8e1c5ead5"
</strong>            ],
            "domains": [
              {
                "name": "mrwildenfree.eth"
              }
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@mrwildenfree"
              },
              {
                "dappName": "farcaster",
                "profileName": "mrwildenfree"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // Other token transfers sent by betashop.eth on Ethereum
      ]
    },
    "Polygon": {
<strong>      "TokenTransfer": null // No tokens sent by betashop.eth on Polygon
</strong>    },
    "Base": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              "0x9bb4d44e6963260a1850926e8f6beb8d5803836f"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `to` response field can be compiled and returned as a recommendation in your application.

## Get Token Transfers Received By A User on Multiple Chains

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, and Base, by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/LQXsneqJLy" %}
Show me token transfers received by betashop.eth on Ethereum, Polygon, and Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  # first query on Ethereum
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: ethereum,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # second query on Polygon
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: polygon,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # third query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: base,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "Ethereum": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "ETHGlobal Pragma Lisbon"
          },
          "from": {
            "addresses": [
              // This user sent 1 ETHGlobal Pragma Lisbon NFT to betshop.eth
<strong>              "0x8f07bc36ff569312fdc41f3867d80bbd2fe94b76"
</strong>            ],
            "domains": [
              {
                "name": "jacob.willemsma.eth"
              },
              // Other ENS domains
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@jacobwillemsma"
              },
              {
                "dappName": "farcaster",
                "profileName": "wj"
              }
            ],
            "xmtp": null
          }
        },
        // Other Token Transfers received by betashop.eth on Ethereum
      ]
    },
    "Polygon": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "Firefly Pass"
          },
          "from": {
            "addresses": [
              // This user sent 1 Firefly Pass NFT to betashop.eth
              "0x135c21b2da426760718e39da954974c4572ae9f6"
            ],
            "domains": [
              {
                "name": "lbae.eth"
              },
              // Other ENS domains
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "lens/@lwsnbaker"
              },
              {
                "dappName": "farcaster",
                "profileName": "lwsnbaker"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // Other Token Transfers received by betashop.eth on Polygon
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No Tokens received by betashop.eth on Base
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `from` response field can be compiled and returned as a recommendation in your application.

## Get The Most Recent Token Transfers Sent From A User(s)

You can fetch all most recent token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, and Base, by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ROziBGPGJI" %}
Show me the most recent token transfers sent from betashop.eth on Ethereum, Polygon, and Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  # first query on Ethereum
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
        from: {_eq: "betashop.eth"},
        # Only get token transfers with non-zero amount
        formattedAmount: {_gt: 0},
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: ethereum,
      limit: 50,
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # second query on Polygon
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
        from: {_eq: "betashop.eth"},
        # Only get token transfers with non-zero amount
        formattedAmount: {_gt: 0},
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: polygon,
      limit: 50,
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  # third query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
        from: {_eq: "betashop.eth"},
        # Only get token transfers with non-zero amount
        formattedAmount: {_gt: 0},
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: base,
      limit: 50,
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      to {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
} 
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Ethereum": {
      "TokenTransfer": [
        {
          "formattedAmount": 125,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // betashop.eth sent 125 USDC to this user
<strong>              "0x3a23f943181408eac424116af7b7790c94cb97a5"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        },
        // Other most recent token transfers sent by betashop.eth on Ethereum
      ]
    },
    "Polygon": {
<strong>      "TokenTransfer": null // no tokens sent by betashop.eth on Polygon
</strong>    },
    "Base": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "to": {
            "addresses": [
              // betashop.eth sent 100 USDC to this user
<strong>              "0x9bb4d44e6963260a1850926e8f6beb8d5803836f"
</strong>            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          }
        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `to` response field can be compiled and returned as a recommendation in your application.

## Get The Most Recent Token Transfers Received By A User

You can fetch most recent token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, and Base, by using the [`TokenTransfers`](../../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/iMj7ZfdCAF" %}
Show me the most recent token transfers received by betashop.eth on Ethereum, Polygon, and Base
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  Ethereum: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: ethereum,
      limit: 50,
<strong>      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
</strong>    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Only get token transfers with non-zero amount
<strong>        formattedAmount: {_gt: 0},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: polygon,
      limit: 50,
<strong>      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
</strong>    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
        }
      }
    }
  }
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
        to: {_eq: "betashop.eth"},
        # Only get token transfers with non-zero amount
        formattedAmount: {_gt: 0},
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: base,
      limit: 50,
<strong>      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
</strong>    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
      from {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
        }
        xmtp {
          isXMTPEnabled
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
    "Ethereum": {
      "TokenTransfer": [
        {
          "formattedAmount": 1250,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
          "from": {
            "addresses": [
              // This user sent 1250 USDC to betashop.eth
<strong>              "0xe2b9be8c62dd8f3d416fc6ba766a17de7f81d0e9"
</strong>            ],
            "domains": [
              {
                "name": "airstack.eth"
              }
            ],
            "socials": null,
            "xmtp": null
          }
        },
        // Other most recent token transfers received by betashop.eth on Ethereum
      ]
    },
    "Polygon": {
      "TokenTransfer": [
        {
          "formattedAmount": 0.002298977414845877,
          "tokenType": "ERC20",
          "token": {
            "name": "Stable Coin"
          },
          "from": {
            "addresses": [
              // This user sent 0.002298977414845877 Stable Coin ERC20 token to betashop.eth
<strong>              "0xdfdf1c5052b6721871095d0df620e07927570f29"
</strong>            ],
            "domains": [
              {
                "name": "vsum.eth"
              },
              {
                "name": "bpmilne.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "bpmilne.eth"
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          }
        },
        // Other most recent token transfers received by betashop.eth on Polygon
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No tokens received by betashop.eth on Base
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

All the users returned from the `from` response field can be compiled and returned as a recommendation in your application.

## Developer Support

If you have any questions or need help regarding integrating or building recommendation engine with token transfer data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenTransfers API Reference](../../api-references/api-reference/tokentransfers-api.md)
* [On-Chain Graph](../onchain-graph.md)
