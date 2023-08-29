---
description: >-
  Learn how to use Airstack to traverse the ERC6551 tree, either upwards or
  downwards.
---

# 🌲 Traverse ERC6551 Tree

With [ERC6551](https://eips.ethereum.org/EIPS/eip-6551) accounts, NFT now has the capability to have an account on their own and perform writing actions to the blockchain as any other accounts on EVM. Thus, this also allows NFTs for the first time to own other assets, e.g. ERC20, ERC721, and ERC1155s.

Since an NFT that has [ERC6551](https://eips.ethereum.org/EIPS/eip-6551) accounts can own another NFT that also can own another ERC6551 account, this creates a very unique, but complex, ownership structure of NFTs.

Using [Airstack](https://airstack.xyz), you can efficiently traverse the [ERC6551](https://eips.ethereum.org/EIPS/eip-6551) ownership tree, both upwards and downwards in just a single API call.

There are various use cases for traversing the ERC6551 ownership tree, which include but is not limited to:

1. [ERC6551 Token Gating](token-gating.md)
2. ERC6551 Ownership Visualization
3. ERC6551-Compatible Wallet
4. etc.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Traverse Up ERC6551 Tree By ERC6551 Account Address](traverse-erc6551-tree.md#traverse-up-erc6551-tree-by-erc6551-account-address)
* [Traverse Up ERC6551 Tree By NFT](traverse-erc6551-tree.md#traverse-up-erc6551-tree-by-nft)
* [Traverse Down ERC6551 Tree By EOA Address](traverse-erc6551-tree.md#traverse-down-erc6551-tree-by-eoa-address)
* [Traverse Down ERC6551 Tree By NFT](traverse-erc6551-tree.md#traverse-down-erc6551-tree-by-nft)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

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

## Traverse Up ERC6551 Tree By ERC6551 Account Address

Suppose that you know the ERC6551 account address `X` and would like to get the NFT that own the given ERC6551 account and the TBA that hold the mentioned NFT as shown in the chart below.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-16 at 23.10.08.png" alt=""><figcaption><p>ERC6551 Tree Example Chart</p></figcaption></figure>

Then, you can use the `Accounts` API to do so with the following query by providing the ERC6551 account address `X` to the `address` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/epmTCtAefH" %}
Traverse Down ERC6551 Tree By ERC6551 Account Address (Demo)
{% endembed %}

### Code

{% hint style="info" %}
To query the upper-level NFTs & TBAs in the ERC6551 ownership tree, simply add more `tokenBalances` nesting inside the most nested `nft` field.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {filter: {address: {_eq: "0xd8dc5794dd43aa9d7495f05bf110614ed32e950f"}}, blockchain: polygon}
  ) {
    Account {
      nft {
        address
        tokenId
        tokenBalances {
          owner {
            addresses
            accounts {
              nft {
                address
                tokenId
                tokenBalances {
                  owner {
                    addresses
                    accounts {
                      nft {
                        address
                        tokenId
                        tokenBalances {
                          owner {
                            addresses
                            accounts {
                              nft {
                                address
                                tokenId
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
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
    "Accounts": {
      "Account": [
        {
          "nft": {
            "address": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
            "tokenId": "13",
            "tokenBalances": [
              {
                "owner": {
                  "addresses": [
                    "0x9b220ed6f11f0223d1e01140fbb0e1b22c713a1a"
                  ],
                  "accounts": [
                    {
                      "nft": {
                        "address": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                        "tokenId": "10",
                        "tokenBalances": [
                          {
                            "owner": {
                              "addresses": [
                                "0x6da658f5840fecc688a4bd007ef6b314d9138135"
                              ],
                              "accounts": []
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Traverse Up ERC6551 Tree By NFT

Suppose that you know the NFT `X` and would like to get the TBA that own the given NFT and the NFT that hold by the mentioned TBA, and the higher-level layer TBAs & NFTs and so on, as shown in the chart below.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-16 at 22.43.30.png" alt=""><figcaption><p>ERC6551 Tree Example Chart</p></figcaption></figure>

Then, you can use the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API to do so with the following query by providing the NFT `X`'s token address and token ID to the `tokenAddress`  and `tokenId` filter input, respectively:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/p5BSiPbwGb" %}
Traverse Up ERC6551 Tree By NFT (Demo)
{% endembed %}

### Code

{% hint style="info" %}
To query the upper-level NFTs & TBAs in the ERC6551 ownership tree, simply add more `tokenBalances` nesting inside the most nested `nft` field.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb"}, tokenId: {_eq: "14"}}, blockchain: polygon}
  ) {
    TokenBalance {
      owner {
        addresses
        accounts {
          nft {
            address
            tokenId
            tokenBalances {
              owner {
                addresses
                accounts {
                  nft {
                    address
                    tokenId
                    tokenBalances {
                      owner {
                        addresses
                        accounts {
                          nft {
                            address
                            tokenId
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": [
              "0xd8dc5794dd43aa9d7495f05bf110614ed32e950f"
            ],
            "accounts": [
              {
                "nft": {
                  "address": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                  "tokenId": "13",
                  "tokenBalances": [
                    {
                      "owner": {
                        "addresses": [
                          "0x9b220ed6f11f0223d1e01140fbb0e1b22c713a1a"
                        ],
                        "accounts": [
                          {
                            "nft": {
                              "address": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                              "tokenId": "10",
                              "tokenBalances": [
                                {
                                  "owner": {
                                    "addresses": [
                                      "0x6da658f5840fecc688a4bd007ef6b314d9138135"
                                    ],
                                    "accounts": []
                                  }
                                }
                              ]
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Traverse Down ERC6551 Tree By EOA Address

Suppose that you know the user's EOA wallet address `X` and would like to get all the NFTs and TBAs own, either directly or indirectly, by the EOA as shown in the chart below.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-16 at 18.34.55.png" alt=""><figcaption><p>ERC6551 Tree Example Chart</p></figcaption></figure>

Then, you can use the [`TokenBalances`](../../api-references/api-reference/tokenbalances-api/) API to do so with the following query by providing the EOA address `X` to the `owner` filter input:

### Try Demo

{% embed url="https://app.airstack.xyz/query/05VOJGobmd" %}
Traverse Down ERC6551 Tree By EOA address (Demo)
{% endembed %}

### Code

{% hint style="info" %}
To query the lower-level NFTs & TBAs in the ERC6551 ownership tree, simply add more `tokenBalances` nesting inside the most nested `address` field.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {blockchain: polygon, filter: {owner: {_eq: "0x6da658f5840fecc688a4bd007ef6b314d9138135"}}}
  ) {
    TokenBalance {
      tokenAddress
      tokenId
      owner {
        addresses
      }
      tokenNfts {
        erc6551Accounts {
          address {
            addresses
            tokenBalances {
              tokenAddress
              tokenId
              tokenNfts {
                erc6551Accounts {
                  address {
                    addresses
                    tokenBalances {
                      tokenAddress
                      tokenId
                      tokenNfts {
                        erc6551Accounts {
                          address {
                            addresses
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
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
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenAddress": "0x3b8ac663c56267f697239f087d4c8207dca7d14c",
          "tokenId": "0",
          "owner": {
            "addresses": [
              "0x6da658f5840fecc688a4bd007ef6b314d9138135"
            ]
          },
          "tokenNfts": {
            "erc6551Accounts": []
          }
        },
        {
          "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
          "tokenId": "10",
          "owner": {
            "addresses": [
              "0x6da658f5840fecc688a4bd007ef6b314d9138135"
            ]
          },
          "tokenNfts": {
            "erc6551Accounts": [
              {
                "address": {
                  "addresses": [
                    "0x9b220ed6f11f0223d1e01140fbb0e1b22c713a1a"
                  ],
                  "tokenBalances": [
                    {
                      "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                      "tokenId": "13",
                      "tokenNfts": {
                        "erc6551Accounts": [
                          {
                            "address": {
                              "addresses": [
                                "0xd8dc5794dd43aa9d7495f05bf110614ed32e950f"
                              ],
                              "tokenBalances": [
                                {
                                  "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                                  "tokenId": "14",
                                  "tokenNfts": {
                                    "erc6551Accounts": []
                                  }
                                }
                              ]
                            }
                          }
                        ]
                      }
                    },
                    {
                      "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                      "tokenId": "12",
                      "tokenNfts": {
                        "erc6551Accounts": []
                      }
                    },
                    {
                      "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                      "tokenId": "9",
                      "tokenNfts": {
                        "erc6551Accounts": [
                          {
                            "address": {
                              "addresses": [
                                "0x333d0ddaf1ecf507f54641055f732914c7f00f18"
                              ],
                              "tokenBalances": [
                                {
                                  "tokenAddress": "0xc61ba42d6315a019b01727defd9c441bb650345b",
                                  "tokenId": "17",
                                  "tokenNfts": {
                                    "erc6551Accounts": []
                                  }
                                },
                                {
                                  "tokenAddress": "0xc61ba42d6315a019b01727defd9c441bb650345b",
                                  "tokenId": "16",
                                  "tokenNfts": {
                                    "erc6551Accounts": []
                                  }
                                }
                              ]
                            }
                          }
                        ]
                      }
                    },
                    {
                      "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                      "tokenId": "11",
                      "tokenNfts": {
                        "erc6551Accounts": []
                      }
                    }
                  ]
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Traverse Down ERC6551 Tree By NFT

Suppose that you know the user's EOA wallet address `X` and would like to get all the NFTs and TBAs own, either directly or indirectly, by the EOA as shown in the chart below.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-16 at 18.50.09.png" alt=""><figcaption><p>ERC6551 Tree Example Chart</p></figcaption></figure>

Then, you can use the [`Accounts`](../../api-references/api-reference/accounts-api/) API to do so with the following query by providing the NFT `X`'s token address and token ID to the `tokenAddress`  and `tokenId` filter input, respectively:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/bCkovDe5fb" %}
Traverse Down ERC6551 Tree By NFT (Demo)
{% endembed %}

### Code

{% hint style="info" %}
To query the lower-level NFTs & TBAs in the ERC6551 ownership, simply add more `tokenBalances` nesting inside the most nested `address` field.
{% endhint %}

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {blockchain: polygon, filter: {tokenAddress: {_eq: "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb"}, tokenId: {_eq: "10"}}}
  ) {
    Account {
      address {
        addresses
        tokenBalances {
          tokenAddress
          tokenId
          tokenNfts {
            erc6551Accounts {
              address {
                addresses
                tokenBalances {
                  tokenAddress
                  tokenId
                  tokenNfts {
                    erc6551Accounts {
                      address {
                        addresses
                      }
                    }
                  }
                }
              }
            }
          }
        }
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
    "Accounts": {
      "Account": [
        {
          "address": {
            "addresses": [
              "0x9b220ed6f11f0223d1e01140fbb0e1b22c713a1a"
            ],
            "tokenBalances": [
              {
                "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                "tokenId": "13",
                "tokenNfts": {
                  "erc6551Accounts": [
                    {
                      "address": {
                        "addresses": [
                          "0xd8dc5794dd43aa9d7495f05bf110614ed32e950f"
                        ],
                        "tokenBalances": [
                          {
                            "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                            "tokenId": "14",
                            "tokenNfts": {
                              "erc6551Accounts": []
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              },
              {
                "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                "tokenId": "12",
                "tokenNfts": {
                  "erc6551Accounts": []
                }
              },
              {
                "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                "tokenId": "9",
                "tokenNfts": {
                  "erc6551Accounts": [
                    {
                      "address": {
                        "addresses": [
                          "0x333d0ddaf1ecf507f54641055f732914c7f00f18"
                        ],
                        "tokenBalances": [
                          {
                            "tokenAddress": "0xc61ba42d6315a019b01727defd9c441bb650345b",
                            "tokenId": "17",
                            "tokenNfts": {
                              "erc6551Accounts": []
                            }
                          },
                          {
                            "tokenAddress": "0xc61ba42d6315a019b01727defd9c441bb650345b",
                            "tokenId": "16",
                            "tokenNfts": {
                              "erc6551Accounts": []
                            }
                          }
                        ]
                      }
                    }
                  ]
                }
              },
              {
                "tokenAddress": "0x99d3fd2f1cf2e99c43f95083b98033d191f4eabb",
                "tokenId": "11",
                "tokenNfts": {
                  "erc6551Accounts": []
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding traversing the ERC6551 tree, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Accounts API Reference](../../api-references/api-reference/accounts-api/)
* [ERC6551 Token Gating](token-gating.md)
