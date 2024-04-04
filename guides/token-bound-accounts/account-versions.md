---
description: >-
  Learn how to use Airstack to fetch certain versions of token-bound (ERC6551)
  accounts.
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

# 💎 Account Versions

Currently, [Airstack](https://airstack.xyz) indexes all the official ERC6551 registry contracts on Ethereum and Base:

<table><thead><tr><th width="210">Version</th><th>Registry Address</th></tr></thead><tbody><tr><td>0.2.0</td><td><a href="https://etherscan.io/address/0x02101dfB77FDE026414827Fdc604ddAF224F0921"><code>0x02101dfB77FDE026414827Fdc604ddAF224F0921</code></a></td></tr><tr><td>0.3.0</td><td><a href="https://etherscan.io/address/0x284be69BaC8C983a749956D7320729EB24bc75f9"><code>0x284be69BaC8C983a749956D7320729EB24bc75f9</code></a></td></tr><tr><td>0.3.1</td><td><a href="https://etherscan.io/address/0x000000006551c19487814612e58fe06813775758"><code>0x000000006551c19487814612e58FE06813775758</code></a></td></tr></tbody></table>

{% hint style="info" %}
If you are deploying ERC6551 using a custom registry contract, please reach out to us to add it to the [Airstack](https://airstack.xyz) API by joining our [Telegram](https://t.me/+1k3c2FR7z51mNDRh).
{% endhint %}

By default, ERC721 NFTs can have multiple versions of ERC6551 accounts controlled by it.

Using the `registry` filter that is provided in the [`Accounts`](../../api-references/api-reference/accounts-api.md) API, you can fetch only certain versions of ERC6551 accounts that your dapp supports.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get A Certain Version of ERC6551 Accounts](account-versions.md#get-a-certain-version-of-erc6551-accounts)
* [Get A Certain Version of ERC6551 Accounts By NFT Collection](account-versions.md#get-a-certain-version-of-erc6551-accounts-by-nft-collection)
* [Get A Certain Version of ERC6551 Accounts Owned By A User](account-versions.md#get-a-certain-version-of-erc6551-accounts-owned-by-a-user)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL
* Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

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

## Get A Certain Version of ERC6551 Accounts

You can use the [`Accounts`](../../api-references/api-reference/accounts-api.md) API to fetch certain version of the ERC6551 accounts, e.g. all v.0.3.1 ERC6551 accounts:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Y3vJiopHG3" %}
Show me all ERC6551 with registry version 0.3.1 on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Accounts(
    input: {
      blockchain: ethereum,
      filter: {
        # set registry to v.0.3.1 registry address
<strong>        registry: {_eq: "0x000000006551c19487814612e58FE06813775758"}
</strong>      },
      limit: 200,
      order: {createdAtBlockTimestamp: DESC}
    }
  ) {
    Account {
      address {
        addresses
      }
      nft {
        address
        tokenId
        token {
          name
        }
        contentValue {
          image {
            medium
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
    "Accounts": {
      "Account": [
        {
          "address": {
            "addresses": [
              // This is the ERC6551 account address
<strong>              "0xcdcbeb24ddc70c869ad7279569a6cfdb9f978d67"
</strong>            ]
          },
          // NFT address, token ID, name, images
<strong>          "nft": {
</strong>            "address": "0xda4128a4fc209df510e9e4483acd059d84620419",
            "tokenId": "14753",
            "token": {
              "name": "Spooky Boys Mansion Party"
            },
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/unphxhbTvBWE8tj4Zl7uHPwvSpFxr0XTTudcSwOrqcxZ1h/Uifxg/1nGez6IlPgkQOn3Aa124ipSq50cn/5KAw==/medium.png"
              }
            }
          }
        },
        // Other v0.3.1 ERC6551 accounts on Ethereum
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get A Certain Version of ERC6551 Accounts By NFT Collection

You can use the [`Accounts`](../../api-references/api-reference/accounts-api.md) API to fetch certain version of the ERC6551 accounts of an NFT Collection, e.g. all v.0.3.1 ERC6551 accounts of [Sapienz NFT](https://explorer.airstack.xyz/token-holders?activeView=\&address=0x26727ed4f5ba61d3772d1575bca011ae3aef5d36\&tokenType=\&rawInput=%23%E2%8E%B1Sapienz%E2%8E%B1%280x26727ed4f5ba61d3772d1575bca011ae3aef5d36+NFT\_COLLECTION+ethereum+null%29\&inputType=NFT\_COLLECTION\&activeTokenInfo=\&activeSnapshotInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&spamFilter=\&mintFilter=\&activeSocialInfo=):

### Try Demo

{% embed url="https://app.airstack.xyz/query/kYjqzOl1DU" %}
Show me all v0.3.1 ERC6551 accounts on @Sapienz NFT
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Accounts(
    input: {
      blockchain: ethereum,
      filter: {
        # set registry to v.0.3.1 registry address
<strong>        registry: {_eq: "0x000000006551c19487814612e58FE06813775758"},
</strong>        # ERC721 NFT Collection address
<strong>        tokenAddress: {_eq: "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36"}
</strong>      },
      limit: 200,
      order: {createdAtBlockTimestamp: DESC}
    }
  ) {
    Account {
      address {
        addresses
      }
      nft {
        tokenId
        contentValue {
          image {
            medium
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
    "Accounts": {
      "Account": [
        {
          "address": {
            "addresses": [
              // ERC6551 account address
<strong>              "0xfcdb9fa208a44dcbf44bee781a843e3a110e5a6d"
</strong>            ]
          },
          // Sapienz NFT's token ID &#x26; image that owned the ERC6551 account
<strong>          "nft": {
</strong>            "tokenId": "5016",
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/5kd+4a7IJ5/FRcsd89DlfK94FmP9mMPaWE6lJEEFcv8k7Uysylj9VPfwtBG/4Coqfgt+T8NjIvnl3Ens3pPcpg==/medium.aaf"
              }
            }
          }
        },
        // Other v0.3.1 ERC6551 accounts on Sapienz NFT
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get A Certain Version of ERC6551 Accounts Owned By A User

You can use the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api.md) API to fetch certain version of the ERC6551 accounts owned by a user, e.g. all v.0.3.1 ERC6551 accounts owned by [`0xjw.eth`](https://explorer.airstack.xyz/token-balances?address=0xjw.eth\&rawInput=%23%E2%8E%B10xjw.eth%E2%8E%B1%280xjw.eth+ADDRESS+ethereum+null%29\&inputType=ADDRESS):

### Try Demo

{% embed url="https://app.airstack.xyz/query/67x9qr9ggJ" %}
Show all v0.3.1 ERC6551 accounts owned by 0xjw.eth on Ethereum
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  TokenBalances(
    input: {
      filter: {
        # Owner of ERC6551 accounts
<strong>        owner: {_eq: "0xjw.eth"},
</strong>        # Filter out only ERC721 NFTs
<strong>        tokenType: {_eq: ERC721}
</strong>      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    TokenBalance {
      tokenNfts {
        erc6551Accounts(
          input: {
            filter: {
              # set registry to v.0.3.1
<strong>              registry: {_eq: "0x000000006551c19487814612e58FE06813775758"}
</strong>            }
          }
        ) {
          address {
            addresses
          }
          nft {
            address
            tokenId
            token {
              name
            }
            contentValue {
              image {
                medium
              }
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    // This is the ERC6551 account address of the NFT
<strong>                    "0xb5307cb1ae1385f64de8442d5d48ff86479f4f8c"
</strong>                  ]
                },
                "nft": {
                  "address": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
                  "tokenId": "0",
                  "token": {
                    "name": "Sapienz"
                  },
                  "contentValue": {
                    "image": {
                      "medium": "https://assets.airstack.xyz/image/nft/5kd+4a7IJ5/FRcsd89DlfK94FmP9mMPaWE6lJEEFcv89IrV05wFbp2CuATN2RB0a/medium.gif"
                    }
                  }
                }
              }
            ]
          }
        },
        {
          "tokenNfts": {
<strong>            "erc6551Accounts": [] // This NFT has no ERC6551 accounts
</strong>          }
        },
        // Other ERC721 NFTs that is owned by 0xjw.eth
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching certain versions of ERC6551 token bound accounts data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Accounts API Reference](../../api-references/api-reference/accounts-api.md)
* [TokenBalances API References](../../api-references/api-reference/tokenbalances-api.md)
