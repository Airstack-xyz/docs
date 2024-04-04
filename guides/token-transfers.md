---
description: >-
  Learn how to use Airstack to fetch all ERC20/721/1155 token transfers data,
  from or to user(s), across Ethereum, Base, Zora, and other Airstack-supported
  chains.
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

[Airstack](https://airstack.xyz) provides a [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API for you to fetch all ERC20/721/1155 token transfers data from either a user or multiple users on Ethereum, Base, Zora, and other Airstack-supported chains.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get Token Transfers Sent From A User on Ethereum](token-transfers.md#get-token-transfers-sent-from-a-user-on-ethereum)
* [Get Token Transfers Received By A User on Ethereum](token-transfers.md#get-token-transfers-received-by-a-user-on-ethereum)
* [Get Token Transfers Sent From A User on Base](token-transfers.md#get-token-transfers-sent-from-a-user-on-base)
* [Get Token Transfers Received By A User on Base](token-transfers.md#get-token-transfers-received-by-a-user-on-base)
* [Get Token Transfers Sent From A User on Zora](token-transfers.md#get-token-transfers-sent-from-a-user-on-zora)
* [Get Token Transfers Received By A User on Zora](token-transfers.md#get-token-transfers-received-by-a-user-on-zora)
* [Get Token Transfers Sent From A User on Multiple Chains](token-transfers.md#get-token-transfers-sent-from-a-user-on-multiple-chains)
* [Get Token Transfers Received By A User on Multiple Chains](token-transfers.md#get-token-transfers-received-by-a-user-on-multiple-chains)
* [Get The Most Recent Token Transfers Sent From A User](token-transfers.md#get-the-most-recent-token-transfers-sent-from-a-user-s)
* [Get The Most Recent Token Transfers Received By A User](token-transfers.md#get-the-most-recent-token-transfers-received-by-a-user)

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

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get Token Transfers Sent From A User on Ethereum

You can fetch all token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), on Ethereum by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

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
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        },
        {
          "formattedAmount": 25,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        }
        // Other token transfers from betashop.eth on Ethereum
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Received By A User on Ethereum

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), on Ethereum by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

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
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "ETHGlobal Pragma Lisbon"
          }
        },
        {
          "formattedAmount": 0.00005,
          "tokenType": "ERC20",
          "token": {
            "name": "Wrapped Ether"
          }
        }
        // Other Token Transfers received by betashop.eth on Ethereum
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Sent From A User on Base

You can fetch all token transfers sent from a given user, e.g. [`jessepollak.eth`](https://explorer.airstack.xyz/token-balances?address=jessepollak.eth\&rawInput=%23%E2%8E%B1jessepollak.eth%E2%8E%B1%28jessepollak.eth++ethereum+null%29\&inputType=ADDRESS), on Base by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

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
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        },
        {
          "formattedAmount": 99,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Base Coin"
          }
        }
        // Other Base token transfers sent by jessepollak.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Received By A User on Base

You can fetch all token transfers received by a given user, e.g. [`barmstrong.eth`](https://explorer.airstack.xyz/token-balances?address=barmstrong.eth\&rawInput=%23%E2%8E%B1barmstrong.eth%E2%8E%B1%28barmstrong.eth++ethereum+null%29\&inputType=ADDRESS), on Base by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

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
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "BasePunks"
          }
        },
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "Bullcasso"
          }
        }
        // Other Token Transfers received by barmstrong.eth on Base
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Sent From A User on Zora

You can fetch all token transfers sent from a given user, e.g. [`jacob.eth`](https://explorer.airstack.xyz/token-balances?address=fc\_fname%3Ajacob\&rawInput=%23%E2%8E%B1fc\_fname%3Ajacob%E2%8E%B1%28fc\_fname%3Ajacob++ethereum+null%29\&inputType=\&tokenType=\&activeView=\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=zora\&sortOrder=\&spamFilter=\&mintFilter=\&resolve6551=\&activeSocialInfo=), on Zora by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/nNTQ6Pg2h3" %}
Show me token transfers sent from jacob.eth on Zora
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from jacob.eth
<strong>        from: {_eq: "jacob.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "jessepollak.eth"]}
        }
      },
      blockchain: zora,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          }
        },
        {
          "formattedAmount": 99,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Base Coin"
          }
        }
        // Other Base token transfers sent by jessepollak.eth
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Received By A User on Zora

You can fetch all token transfers received by a given user, e.g. [`jacob.eth`](https://explorer.airstack.xyz/token-balances?address=fc\_fname%3Ajacob\&rawInput=%23%E2%8E%B1fc\_fname%3Ajacob%E2%8E%B1%28fc\_fname%3Ajacob++ethereum+null%29\&inputType=\&tokenType=\&activeView=\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=zora\&sortOrder=\&spamFilter=\&mintFilter=\&resolve6551=\&activeSocialInfo=), on Zora by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/IN0qktFA2I" %}
Show me token transfers received by jacob.eth on Zora
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to jacob.eth
<strong>        to: {_eq: "jacob.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "barmstrong.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: zora,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "BasePunks"
          }
        },
        {
          "formattedAmount": 1,
          "tokenType": "ERC721",
          "token": {
            "name": "Bullcasso"
          }
        }
        // Other Token Transfers received by barmstrong.eth on Base
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Token Transfers Sent From A User on Multiple Chains

You can fetch all token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NspXPukoq0" %}
Show me all token transfers sent from betashop.eth on Ethereum, Polygon, Base, and Zora
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
    }
  }
  # second query on Polygon
  Polygon: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
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
    }
  }
  # third query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
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
    }
  }
  # fourth query on Zora
  Zora: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
<strong>        from: {_eq: "betashop.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]}
        }
      },
      blockchain: zora,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
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
        },
        // Other token transfers sent by betashop.eth on Base
      ]
    },
    "Zora": {
<strong>      "TokenTransfer": null // No tokens sent by betashop.eth on Zora
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Token Transfers Received By A User on Multiple Chains

You can fetch all token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/EtO1a58dQQ" %}
Show me token transfers received by betashop.eth on Ethereum, Base, and Zora
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
    }
  }
  # second query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
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
    }
  }
  # third query on Zora
  Zora: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers to betashop.eth
<strong>        to: {_eq: "betashop.eth"},
</strong>        # Remove all minting/burning + self-transfer
<strong>        _nor: {
</strong>          from: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD", "betashop.eth"]},
          to: {_in: ["0x0000000000000000000000000000000000000000", "0x000000000000000000000000000000000000dEaD"]}
        }
      },
      blockchain: zora,
      limit: 50
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
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
        },
        // Other Token Transfers received by betashop.eth on Ethereum
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No Tokens received by betashop.eth on Base
</strong>    },
    "Zora": {
<strong>      "TokenTransfer": null // No Tokens received by betashop.eth on Zora
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Sent From A User(s)

You can fetch all most recent token transfers sent from a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/U8PmPyhVIS" %}
Show me the most recent token transfers sent from betashop.eth on Ethereum, Base, and Zora
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
        from: { _eq: "betashop.eth" }
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
            ]
          }
          to: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
              "betashop.eth"
            ]
          }
        }
      }
      blockchain: ethereum
      limit: 50
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
  # second query on Base
  Base: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
        from: { _eq: "betashop.eth" }
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
            ]
          }
          to: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
              "betashop.eth"
            ]
          }
        }
      }
      blockchain: base
      limit: 50
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
      }
    }
  }
  # third query on Zora
  Zora: TokenTransfers(
    input: {
      filter: {
        # Only get token transfers from betashop.eth
        from: { _eq: "betashop.eth" }
        # Remove all minting/burning + self-transfer
        _nor: {
          from: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
            ]
          }
          to: {
            _in: [
              "0x0000000000000000000000000000000000000000"
              "0x000000000000000000000000000000000000dEaD"
              "betashop.eth"
            ]
          }
        }
      }
      blockchain: zora
      limit: 50
      order: { blockTimestamp: DESC } # Order transfers by blocktimestamp in descending order
    }
  ) {
    TokenTransfer {
      formattedAmount
      tokenType
      token {
        name
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
    "Ethereum": {
      "TokenTransfer": [
        {
          "formattedAmount": 125,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
        },
        // Other most recent token transfers sent by betashop.eth on Ethereum
      ]
    },
    "Base": {
      "TokenTransfer": [
        {
          "formattedAmount": 100,
          "tokenType": "ERC20",
          "token": {
            "name": "USD Coin"
          },
        }
      ]
    },
    "Zora": {
      "TokenTransfer": null // no tokens sent by betashop.eth on Zora
    },
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Most Recent Token Transfers Received By A User

You can fetch most recent token transfers received by a given user, e.g. [`betashop.eth`](https://explorer.airstack.xyz/token-balances?address=betashop.eth\&rawInput=%23%E2%8E%B1betashop.eth%E2%8E%B1%28betashop.eth++ethereum+null%29\&inputType=ADDRESS), across multiple chains, such as Ethereum, Polygon, Base, and Zora, by using the [`TokenTransfers`](../api-references/api-reference/tokentransfers-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/ZjywjOVFCH" %}
Show me the most recent token transfers received by betashop.eth on Ethereum, Polygon, Base, and Zora
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
    }
  }
  Zora: TokenTransfers(
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
      blockchain: zora,
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
        },
        // Other most recent token transfers received by betashop.eth on Polygon
      ]
    },
    "Base": {
<strong>      "TokenTransfer": null // No tokens received by betashop.eth on Base
</strong>    },
    "Zora": {
<strong>      "TokenTransfer": null // No tokens received by betashop.eth on Zora
</strong>    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching token transfers data into your application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [TokenTransfers API Reference](../api-references/api-reference/tokentransfers-api.md)
* [On-Chain Graph](onchain-graph.md)
* [Token Mints Guides](token-mints.md)
