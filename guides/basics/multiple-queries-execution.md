---
description: >-
  Learn how you can do multiple queries execution using Airstack GraphQL API to
  fetch data more efficiently in a single request.
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

# 🎆 Multiple Queries Execution

In GraphQL, you are able to call multiple queries in a single request.

As [Airstack](https://airstack.xyz) is a GraphQL API, it inherits this feature that enables you to call multiple API in one request:

### Try Demo

{% embed url="https://app.airstack.xyz/query/54kSuTrcCa" %}
Show me all followers and followings of users in Lens and Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
<strong>  SocialFollowers( # 1st query fetching social followers
</strong>    input: {filter: {identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}}, blockchain: ALL, limit: 200, order: {followerSince: DESC}}
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          isPrimary
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
<strong>  SocialFollowings( # 2nd query fetching social followings
</strong>    input: {filter: {identity: {_in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "vitalik.eth", "lens/@vitalik", "fc_fname:vitalik"]}}, blockchain: ALL, limit: 200, order: {followingSince: DESC}}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          isPrimary
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
```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0x7c8428173b00bb04bd35692b1fdf6408e1b1108a"
            ],
            "domains": null,
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "mxr"
              }
            ],
            "xmtp": null
          }
        },
        // Lens and Farcaster follower of vitalik 
      ]
    },
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x71f4b32ce1303d8537696e1311df8f485ae96a18",
              "0x0fce5d938ef7ebd0ce0306406ebb40668014693a"
            ],
            "domains": [
              {
                "isPrimary": true,
                "name": "quanm2831.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "quanm2831"
              }
            ],
            "xmtp": null
          }
        },
        // Lens and Farcaster following of vitalik 
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

This allow you to not only fetch data that you need more efficiently, but also allow you to do the same query with different inputs when the [`_in`](../../api-references/overview/working-with-graphql.md) operator is not available for the API input.

This is particularly useful for building cross-chain queries:

### Try Demo

{% embed url="https://app.airstack.xyz/query/gla6KJCgqy" %}
Show ERC20 tokens on Ethereum, Polygon, and Base owned by users
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query ERC20OwnedByLensProfiles {
<strong>  Ethereum: TokenBalances( # first query fetch Ethereum ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      amount
      tokenAddress
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
<strong>  Polygon: TokenBalances( # second query fetch Polygon ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: polygon
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      amount
      tokenAddress
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
<strong>  Base: TokenBalances( # third query fetch Base ERC20 balance
</strong>    input: {
      filter: {
        owner: {
          _in: [
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            "vitalik.eth"
            "lens/@vitalik"
            "fc_fname:vitalik"
          ]
        }
        tokenType: { _eq: ERC20 }
      }
      blockchain: base
      limit: 50
    }
  ) {
    TokenBalance {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      amount
      tokenAddress
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
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Ethereum": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              // Other ENS domains
            ]
            "socials": [
              {
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x1612",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              },
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "amount": "45934484403886362668",
          "tokenAddress": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
          "token": {
            "name": "Wrapped Ether",
            "symbol": "WETH"
          }
        }
        // Other Ethereum ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Polygon": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              // Other ENS domains
            ]
            "socials": [
              {
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x1612",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              },
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "amount": "457218374987121000000",
          "tokenAddress": "0x086373fad3447f7f86252fb59d56107e9e0faafa",
          "token": {
            "name": "Yup",
            "symbol": "YUP"
          }
        }
        // Other Polygon ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    },
    "Base": {
      "TokenBalance": [
        {
          "owner": {
            "addresses": [
              "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
            ],
            "domains": [
              {
                "name": "quantumexchange.eth",
                "isPrimary": false
              },
              // Other ENS domains
            ]
            "socials": [
              {
                "profileName": "vitalik.eth",
                "profileTokenId": "5650",
                "profileTokenIdHex": "0x1612",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              },
              {
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "amount": "310194000000000000000000",
          "tokenAddress": "0xac1bd2486aaf3b5c0fc3fd868558b082a531b2b4",
          "token": {
            "name": "Toshi",
            "symbol": "TOSHI"
          }
        }
        // Other Base ERC20s
      ],
      "pageInfo": {
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6ImVmYmMyM2UwZGZkYmFiY2Y0MjFjNzRmNmE5ODlkMWNhMjdhMTJlYjRjZWUyNmM5NmViNzZhMzZhMTk3MzA0ZjUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJsYXN0VXBkYXRlZFRpbWVzdGFtcCI6eyJWYWx1ZSI6IjE2OTE0Mjk2NjIiLCJEYXRhVHlwZSI6IkRhdGVUaW1lIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding adding variables into your Airstack query, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [API Overview](../../api-references/overview/cursor-pagination/overview.md)
* [Variables Guides](variables.md)
* [Direct API Call](../../get-started/quickstart/direct-api-call.md)
* [SocialFollowings API References](../../api-references/api-reference/socialfollowings-api.md)
* [SocialFollowers API References](../../api-references/api-reference/socialfollowers-api.md)
* [TokenBalances API References](../../api-references/api-reference/tokenbalances-api.md)
