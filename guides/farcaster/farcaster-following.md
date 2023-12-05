---
description: >-
  Learn how to fetch Farcaster following data and its various use cases and
  combinations.
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

# 💐 Farcaster Following

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

# Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Farcaster Following of Farcaster User(s)](farcaster-following.md#get-farcaster-following-of-farcaster-user-s)
- [Get The Most Recent Farcaster Following of Farcaster User(s)](farcaster-following.md#get-the-most-recent-farcaster-following-of-farcaster-user-s)
- [Get The Earliest Farcaster Following of Farcaster User(s)](farcaster-following.md#get-the-earliest-farcaster-following-of-farcaster-user-s)
- [Get Farcaster Following of Farcaster User(s) that has ENS Domain](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-has-ens-domain)
- [Get Farcaster Following of Farcaster User(s) that has XMTP Enabled](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-has-xmtp-enabled)
- [Get Farcaster Users that have a certain amount of Following](farcaster-following.md#get-farcaster-users-that-have-a-certain-amount-of-following)
- [Get Farcaster and Lens Following of Lens Profile(s)](farcaster-following.md#get-farcaster-and-lens-following-of-lens-profile-s)
- [Get Farcaster Following of Farcaster User(s) that Hold ERC20 Token(s)](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-hold-erc20-token-s)
- [Get Farcaster Following of Farcaster User(s) that Hold ERC721/1155 NFT(s)](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-hold-erc721-1155-nft-s)
- [Get Farcaster Following of Farcaster User(s) that Hold POAP(s)](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-hold-poap-s)
- [Get Farcaster Following of Farcaster User(s) that Hold Certain Amount of ERC20 Token(s)](farcaster-following.md#get-farcaster-following-of-farcaster-user-s-that-hold-certain-amount-of-erc20-token-s)

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

## Get Farcaster Following of Farcaster User(s)

You can get the list of Farcaster following of Farcaster user(s) by inputting [0x address](#user-content-fn-1)[^1], ENS domain, [Farcaster Name](#user-content-fn-2)[^2], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/x3vTTRer22" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followingProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
              "0x223f2db258234f7fa164a9e4c0929318feb3b550"
            ],
            "socials": [
              {
                "profileName": "vm",
                "userId": "325",
                "userAssociatedAddresses": [
                  "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              }
            ]
          },
          "followingProfileId": "325",
          "followerAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followerProfileId": "3"
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Most Recent Farcaster Following of Farcaster User(s)

You can get the list of most recent Farcaster following of Farcaster user(s) by inputting [0x address](#user-content-fn-3)[^3], ENS domain, [Farcaster Name](#user-content-fn-4)[^4], or Farcaster ID:

### Try Demo

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
      }
      blockchain: ALL
      limit: 200
      order: { followerSince: DESC }
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```
// Some code
```

{% endtab %}
{% endtabs %}

## Get The Earliest Farcaster Following of Farcaster User(s)

You can get the list of the earliest Farcaster following of Farcaster user(s) by inputting [0x address](#user-content-fn-5)[^5], ENS domain, [Farcaster Name](#user-content-fn-6)[^6], or Farcaster ID:

### Try Demo

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
      }
      blockchain: ALL
      limit: 200
      order: { followerSince: ASC }
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```
// Some code
```

{% endtab %}
{% endtabs %}

## Get Farcaster Following of Farcaster User(s) that has ENS Domain

You can get the list of Farcaster following of Farcaster user(s) and check if they have any ENS domain by inputting [0x address](#user-content-fn-7)[^7], ENS domain, [Farcaster Name](#user-content-fn-8)[^8], or Farcaster ID:

### Try Demo

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: farcaster}, identity: {_in: ["lens/@stani", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
<strong>        domains { # Show all domains owned by followers
</strong>          name
          isPrimary
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
      followingTokenId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: farcaster}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

You can use the `followerAddress.domains` that will return an array to see if there's any domain owned by the follower.

If the length of the `followerAddress.domains` array is non-zero, then the follower has at least one ENS domain. Otherwise, it does not have any ENS domain.

## Get Farcaster Following of Farcaster User(s) that has XMTP Enabled

You can get the list of Farcaster following of Farcaster user(s) and check if each following have XMTP enabled or not by inputting [0x address](#user-content-fn-9)[^9], ENS domain, [Farcaster Name](#user-content-fn-10)[^10], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Gpgb0KvNyb" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 and if XMTP is enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        dappName: { _eq: farcaster }
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      followingProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
              "0x223f2db258234f7fa164a9e4c0929318feb3b550"
            ],
            "socials": [
              {
                "profileName": "vm",
                "userId": "325",
                "userAssociatedAddresses": [
                  "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "followingProfileId": "325",
          "followerAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followerProfileId": "3"
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster Users that have a certain amount of Following

You can get the list of Farcaster users that have a certain amount of following, e.g. at least 1000 following:

### Try Demo

{% embed url="https://app.airstack.xyz/query/BwveccRyyQ" %}
Show me all Farcaster users that have at least 1000 following
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { followingCount: { _gte: 1000 }, dappName: { _eq: farcaster } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      profileName
      userId
      userAssociatedAddresses
      followingCount
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
          "profileName": "jayme",
          "userId": "373",
          "userAssociatedAddresses": [
            "0x1ca66c990e86b750ea6b2180d17fff89273a5c0d",
            "0x9eab9d856a3a667dc4cd10001d59c679c64756e7"
          ],
          "followingCount": 1443
        },
        {
          "profileName": "aydasayer",
          "userId": "14993",
          "userAssociatedAddresses": [
            "0xf009162e2495dd1dd54b5470e03004010f931a9f",
            "0x022fe87e83f3d6662862ed51ef74de33ed16ff25"
          ],
          "followingCount": 1245
        },
        {
          "profileName": "reiri",
          "userId": "17626",
          "userAssociatedAddresses": [
            "0x5f948ad313f280eb71d038f72d932f3b335458de",
            "0x688d0cef9d2ad572e2b889ea595a042a1821e5ff"
          ],
          "followingCount": 1054
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster and Lens Following of Lens Profile(s)

You can get the list of Farcaster and Lens following of Farcaster user(s) by inputting [0x address](#user-content-fn-11)[^11], ENS domain, [Farcaster Name](#user-content-fn-12)[^12], or [Farcaster ID](#user-content-fn-13)[^13]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/p7QkRzuSeX" %}
Show me the Farcaster and Lens following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
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
      followingProfileId
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
              "0x223f2db258234f7fa164a9e4c0929318feb3b550"
            ],
            "domains": [
              {
                "name": "victorma.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vm",
                "profileTokenId": "325",
                "profileTokenIdHex": "",
                "userId": "325",
                "userAssociatedAddresses": [
                  "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              },
              {
                "dappName": "lens",
                "profileName": "lens/@victorma",
                "profileTokenId": "118319",
                "profileTokenIdHex": "",
                "userId": "0x223f2db258234f7fa164a9e4c0929318feb3b550",
                "userAssociatedAddresses": [
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              }
            ]
          },
          "followingProfileId": "325",
          "followerAddress": {
            "addresses": [
              "0xff3174d9f52f8c24f9c884600ac80c2b2dda4556",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0x74232bf61e994655592747e20bdf6fa9b9476f79"
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0xff3174d9f52f8c24f9c884600ac80c2b2dda4556",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79"
                ]
              }
            ]
          },
          "followerProfileId": "3"
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster Following of Farcaster User(s) that Hold ERC20 Token(s)

You can get the list of Farcaster following of Farcaster user(s) and check if each following hold ERC20 token(s), e.g. [USDC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48&tokenType=&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29&inputType=TOKEN&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting [0x address](#user-content-fn-14)[^14], ENS domain, [Farcaster Name](#user-content-fn-15)[^15], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/DiPL2syB2M" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also hold USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {
            filter: {
              tokenAddress: {
                _in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]
              }
              tokenType: { _eq: ERC20 }
            }
          }
        ) {
          formattedAmount
        }
      }
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
              "0x223f2db258234f7fa164a9e4c0929318feb3b550"
            ],
            "socials": [
              {
                "profileName": "vm",
                "userId": "325",
                "userAssociatedAddresses": [
                  "0x5beaf49e8ee203ef1023e9b1b716c07016f399a1",
                  "0x223f2db258234f7fa164a9e4c0929318feb3b550"
                ]
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 12
              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followerProfileId": "3"
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Farcaster Following of Farcaster User(s) that Hold ERC721/1155 NFT(s)

You can get the list of Farcaster following of Farcaster user(s) and check if each following hold ERC721/1155 NFT(s), e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&tokenType=&rawInput=%23%E2%8E%B1BoredApeYachtClub%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+NFT_COLLECTION+ethereum+null%29&inputType=NFT_COLLECTION&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting [0x address](#user-content-fn-16)[^16], ENS domain, [Farcaster Name](#user-content-fn-17)[^17], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/jmiIrPMM8o" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also hold BAYC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {
            filter: {
              tokenAddress: {
                _in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"]
              }
              tokenType: { _in: [ERC721, ERC1155] }
            }
          }
        ) {
          formattedAmount
        }
      }
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0xb5b2da92350efdc6250b74bccf24f4ec9e5bb0ce",
              "0x695626a661831a618ae846482fe8abf6c8d4529c"
            ],
            "socials": [
              {
                "profileName": "ivanhodl",
                "userId": "14240",
                "userAssociatedAddresses": [
                  "0xb5b2da92350efdc6250b74bccf24f4ec9e5bb0ce",
                  "0x695626a661831a618ae846482fe8abf6c8d4529c"
                ]
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1 // following have BAYC
</strong>              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ],
            "domains": [
              {
                "name": "jasongoldberg.eth"
              },
              {
                "name": "betashop.eth"
              }
            ],
            "socials": [
              {
                "profileName": "betashop.eth",
                "userId": "602",
                "userAssociatedAddresses": [
                  "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              }
            ]
          },
          "followerProfileId": "602"
        },
        // more following
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Get Farcaster Following of Farcaster User(s) that Hold POAP(s)

You can get the list of Farcaster following of Farcaster user(s) and check if each following hold POAP(s), e.g. [EthCC\[6\] – Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=&address=141910&tokenType=&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29&inputType=POAP&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=gnosis), by inputting [0x address](#user-content-fn-18)[^18], ENS domain, [Farcaster Name](#user-content-fn-19)[^19], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mAGfmFiMnB" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also hold EthCC\[6] - Attendee POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        poaps(input: { filter: { eventId: { _eq: "141910" } } }) {
          mintHash
          mintOrder
        }
      }
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x6d7af15b43f897403004c5e14a70022f5625d801",
              "0xd383eeeb6912036f36d0d1878e2dec6dd3eb80ba"
            ],
            "socials": [
              {
                "profileName": "rdl",
                "userId": "652",
                "userAssociatedAddresses": [
                  "0x6d7af15b43f897403004c5e14a70022f5625d801",
                  "0xd383eeeb6912036f36d0d1878e2dec6dd3eb80ba"
                ]
              }
            ],
            "poaps": [
              // Hold an EthCC[6] - Attendee POAP
              {
<strong>                "mintHash": "0xccd861db65ffb63f20d458bcbc1d1a0282e4bd7a606ef1226121abe21429d00e",
</strong><strong>                "mintOrder": 732
</strong>              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0x4114e33eb831858649ea3702e1c9a2db3f626446",
              "0x182327170fc284caaa5b1bc3e3878233f529d741",
              "0x91031dcfdea024b4d51e775486111d2b2a715871"
            ],
            "domains": [
              {
                "name": "varunsrin.eth"
              }
            ],
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446",
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871"
                ]
              }
            ]
          },
          "followerProfileId": "2"
        },
        // more following
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Get Farcaster Following of Farcaster User(s) that Hold Certain Amount of ERC20 Token(s)

You can get the list of Farcaster following of Farcaster user(s) and check if each following hold a certain amount of ERC20 token(s), e.g. at least 1000 [USDC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48&tokenType=&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29&inputType=TOKEN&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting [0x address](#user-content-fn-20)[^20], ENS domain, [Farcaster Name](#user-content-fn-21)[^21], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/w9JazJ5kna" %}
Show me Farcaster following of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 that also hold at least 1000 USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowings(
    input: {
      filter: {
        identity: {
          _in: [
            "fc_fname:dwr.eth"
            "fc_fid:602"
            "varunsrin.eth"
            "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
          ]
        }
        dappName: { _eq: farcaster }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        tokenBalances(
          input: {
            filter: {
              tokenAddress: {
                _in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]
              }
              tokenType: { _eq: ERC20 }
              formattedAmount: { _gte: 1000 }
            }
          }
        ) {
          formattedAmount
        }
      }
      followerAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x30a838dfc03968b5e0a64796547db42f64aa4de5",
              "0xf0fbaaa7ece80ac41508e442929b81a4c8c8543b"
            ],
            "socials": [
              {
                "profileName": "0xjoyce",
                "userId": "1660",
                "userAssociatedAddresses": [
                  "0x30a838dfc03968b5e0a64796547db42f64aa4de5",
                  "0xf0fbaaa7ece80ac41508e442929b81a4c8c8543b"
                ]
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 3283.180666
              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0x74232bf61e994655592747e20bdf6fa9b9476f79",
              "0xb877f7bb52d28f06e60f557c00a56225124b357f",
              "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
              "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
            ],
            "domains": [
              {
                "name": "dwr.eth"
              },
              {
                "name": "noun124.eth"
              },
              {
                "name": "dwr.mirror.xyz"
              },
              {
                "name": "danromero.eth"
              }
            ],
            "socials": [
              {
                "profileName": "dwr.eth",
                "userId": "3",
                "userAssociatedAddresses": [
                  "0x74232bf61e994655592747e20bdf6fa9b9476f79",
                  "0xb877f7bb52d28f06e60f557c00a56225124b357f",
                  "0xa14b4c95b5247199d74c5578531b4887ca5e4909",
                  "0xd7029bdea1c17493893aafe29aad69ef892b8ff2"
                ]
              }
            ]
          },
          "followerProfileId": "3"
        }
        // more following
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster Followings data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowings API](../../api-references/api-reference/socialfollowings-api.md)

1. e.g. `varunsrin.eth`
2. e.g. `fc_fid:602`

[^1]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^2]: e.g. `fc_fname:dwr.eth`
[^3]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^4]: e.g. `fc_fname:dwr.eth`
[^5]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^6]: e.g. `fc_fname:dwr.eth`
[^7]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^8]: e.g. `fc_fname:dwr.eth`
[^9]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^10]: e.g. `fc_fname:dwr.eth`
[^11]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^12]: e.g. `fc_fname:dwr.eth`
[^13]: e.g. `fc_fid:602`
[^14]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^15]: e.g. `fc_fname:dwr.eth`
[^16]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^17]: e.g. `fc_fname:dwr.eth`
[^18]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^19]: e.g. `fc_fname:dwr.eth`
[^20]: e.g. `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`
[^21]: e.g. `fc_fname:dwr.eth`
