---
description: >-
  Learn how to fetch Lens followers data and its various use cases and
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

# 🫂 Lens Followers

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to fetch various use cases of Lens followers data.

In this guide you will learn how to use Airstack to:

- [Get Lens Followers of Lens Profile(s)](lens-followers.md#get-lens-followers-of-lens-profile-s)
- [Check A Given Lens Profile is A Follower of User(s) on Lens](lens-followers.md#check-a-given-lens-profile-is-a-follower-of-user-s-on-lens)
- [Get The Most Recent Lens Followers of Lens Profile(s)](lens-followers.md#get-the-most-recent-lens-followers-of-lens-profile-s)
- [Get The Earliest Lens Followers of Lens Profile(s)](lens-followers.md#get-the-earliest-lens-followers-of-lens-profile-s)
- [Get Lens Followers of Lens Profile(s) that has ENS Domain](lens-followers.md#get-lens-followers-of-lens-profile-s-that-has-ens-domain)
- [Get Lens Followers of Lens Profile(s) that has XMTP Enabled](lens-followers.md#get-lens-followers-of-lens-profile-s-that-has-xmtp-enabled)
- [Get Lens Profiles that have a certain amount of Followers](lens-followers.md#get-lens-profiles-that-have-a-certain-amount-of-followers)
- [Get Lens and Farcaster Followers of Lens Profile(s)](lens-followers.md#get-lens-and-farcaster-followers-of-lens-profile-s)
- [Get Lens Followers of Lens Profile(s) that Hold ERC20 Token(s)](lens-followers.md#get-lens-followers-of-lens-profile-s-that-hold-erc20-token-s)
- [Get Lens Followers of Lens Profile(s) that Hold ERC721/1155 NFT(s)](lens-followers.md#get-lens-followers-of-lens-profile-s-that-hold-erc721-1155-nft-s)
- [Get Lens Followers of Lens Profile(s) that Hold POAP(s)](lens-followers.md#get-lens-followers-of-lens-profile-s-that-hold-poap-s)
- [Get Lens Followers of Lens Profile(s) that Hold Certain Amount of ERC20 Token(s)](lens-followers.md#get-lens-followers-of-lens-profile-s-that-hold-certain-amount-of-erc20-token-s)

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

## Get Lens Followers of Lens Profile(s)

You can get the list of Lens followers of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-1)[^1] or Lens profile ID (decimal[^2] or hex[^3]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/6VKviRcwXH" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: lens }
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
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "19989",
          "followerTokenId": "15527",
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Check A Given Lens Profile is A Follower of User(s) on Lens

You can use Airstack to check if user(s) is following a given Lens profile on Lens protocol.

This can be done by providing the given Lens profile on the `Wallet` top-level query's `identity` input and the user(s) in the [`socialFollowers`](../../api-references/api-reference/socialfollowers-api.md):

### Try Demo

{% embed url="https://app.airstack.xyz/query/KI3tFacXLb" %}
Show me if lens/@shnoodles is a follower of a group of users on Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query isFollowing {
  Wallet(input: {identity: "lens/@shnoodles", blockchain: ethereum}) {
    socialFollowers( # Check if lens/@shnoodles is a follower of these user identities on Lens
<strong>      input: {filter: {identity: {_in: ["0xeaf55242a90bb3289dB8184772b0B98562053559", "betashop.eth", "yosephks.cb.id", "lens/@deepesh", "lens_id:100275", "fc_fname:dawufi", "fc_fid:602"]}, dappName: {_eq: lens}}}
</strong>    ) {
      Follower {
        dappName
        dappSlug
        followingProfileId
        followerProfileId
        followingAddress {
          addresses
          socials {
            profileName
          }
          domains {
            name
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
    "Wallet": {
      "socialFollowers": {
        "Follower": [
          {
            "dappName": "lens",
            "dappSlug": "lens_polygon",
            "followingProfileId": "94472",
            "followerProfileId": "27561",
            "followingAddress": {
              "addresses": [
                "0xeaf55242a90bb3289db8184772b0b98562053559"
              ],
              "socials": [
                {
                  "profileName": "betashop.eth"
                },
                {
<strong>                  "profileName": "lens/@betashop9" // lens/@shnoodles is a follower of lens/@betashop9 on Lens
</strong>                }
              ],
              "domains": [
                {
                  "name": "jasongoldberg.eth"
                },
                {
                  "name": "betashop.eth"
                }
              ]
            }
          },
          {
            "dappName": "lens",
            "dappSlug": "lens_polygon",
            "followingProfileId": "122312",
            "followerProfileId": "27561",
            "followingAddress": {
              "addresses": [
                "0x0964256674e42d61f0ff84097e28f65311786ccb"
              ],
              "socials": [
                {
<strong>                  "profileName": "lens/@dawufi" // lens/@shnoodles is a follower of lens/@dawufi on Lens
</strong>                },
                {
                  "profileName": "dawufi"
                }
              ],
              "domains": [
                {
                  "name": "cantos.testbrand.eth"
                },
                {
                  "name": "dawufi.eth"
                }
              ]
            }
          }
        ]
      }
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

If [`lens/@shnoodles`](https://explorer.airstack.xyz/token-balances?address=lens%2F%40shnoodles&blockchain=ethereum&rawInput=%23%E2%8E%B1lens%2F%40shnoodles%E2%8E%B1%28lens%2F%40shnoodles++ethereum+null%29&inputType=ADDRESS) is a follower of any of the users on Lens, then it will appear as a response in the `Follower` array as shown in the [sample response](lens-followers.md#response-1).

## Get The Most Recent Lens Followers of Lens Profile(s)

You can get the list of most recent Lens followers of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-4)[^4] or Lens profile ID (decimal[^5] or hex[^6]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/IHBMwpSwoy" %}
Show me the most recent Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: lens }
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
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x58ff0410a60153edc925cfca1a42d104d669d3d9"],
            "socials": [
              {
                "profileName": "lens/@oglab",
                "profileTokenId": "97497",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "97497",
          "followerTokenId": "66673",
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Earliest Lens Followers of Lens Profile(s)

You can get the list of the earliest Lens followers of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-7)[^7] or Lens profile ID (decimal[^8] or hex[^9]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/5wOzfKr33V" %}
Show me the earliest Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        dappName: { _eq: lens }
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
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x2c5667c6b4d442f8a9efb555ed6b465fff56710d"],
            "socials": [
              {
                "profileName": "lens/@tataraghoul303",
                "profileTokenId": "8560",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@meowed",
                "profileTokenId": "96953",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "96953",
          "followerTokenId": "28394",
          "followingAddress": {
            "addresses": ["0x7241dddec3a6af367882eaf9651b87e1c7549dff"],
            "domains": null,
            "socials": [
              {
                "profileName": "lens/@stani",
                "profileTokenId": "5",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@lilgho",
                "profileTokenId": "110056",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@lensofficial",
                "profileTokenId": "47319",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "5"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Lens Followers of Lens Profile(s) that has ENS Domain

You can get the list of Lens followers of Lens profile(s) and check if they have any ENS domain by inputting either their 0x address, [Lens profile name](#user-content-fn-10)[^10] or Lens profile ID (decimal[^11] or hex[^12]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/etLz3ybvlu" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that have ENS Domain
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowers(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["lens/@stani", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
<strong>        domains { # Show all domains owned by followers
</strong>          name
          isPrimary
        }
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "19989",
          "followerTokenId": "15527",
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // other followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

You can use the `followerAddress.domains` that will return an array to see if there's any domain owned by the follower.

If the length of the `followerAddress.domains` array is non-zero, then the follower has at least one ENS domain. Otherwise, it does not have any ENS domain.

## Get Lens Followers of Lens Profile(s) that has XMTP Enabled

You can get the list of Lens followers of Lens profile(s) and check if they have XMTP enabled by inputting either their 0x address, [Lens profile name](#user-content-fn-13)[^13] or Lens profile ID (decimal[^14] or hex[^15]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/KqHp79HKd5" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that have XMTP enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowers(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["lens/@stani", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        xmtp {
<strong>          isXMTPEnabled # Indicate if followers have XMTP enabled
</strong>        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "followerProfileId": "19989",
          "followerTokenId": "15527",
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

You can use the `followerAddress.xmtp.isXMTPEnabled` field to check if the follower has XMTP enabled or not.

If `followerAddress.xmtp.isXMTPEnabled` field returns `true`, then the follower has XMTP enabled. Otherwise, it will return `null` and thus has not enabled XMTP.

## Get Lens Profiles that have a certain amount of Followers

You can get all the Lens profiles that have a certain amount of followers, e.g. more than or equal to 1000 follower counts on Lens:

### Try Demo

{% embed url="https://app.airstack.xyz/query/TN0zL4LkpY" %}
Show me all Lens profiles that have more than or equal to 1000 followers
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {filter: {followerCount: {_gte: 1000}, dappName: {_eq: lens}}, blockchain: ethereum, limit: 200}
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
<strong>      followerCount # Show the total follower count of each follower
</strong>    }
  }
}
</code></pre>

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "lens/@metaverseplayer",
          "profileTokenId": "971",
          "profileTokenIdHex": "0x03cb",
          "followerCount": 1000
        },
        {
          "profileName": "lens/@thisisvoya",
          "profileTokenId": "36332",
          "profileTokenIdHex": "0x08dec",
          "followerCount": 1486
        },
        {
          "profileName": "lens/@timjeffries",
          "profileTokenId": "30824",
          "profileTokenIdHex": "0x07868",
          "followerCount": 1108
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

To get the individual follower count of each Lens profile, use `followerCount` field that will return an integer type.

## Get Lens and Farcaster Followers of Lens Profile(s)

You can get the list of Lens and Farcaster followers of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-16)[^16] or Lens profile ID (decimal[^17] or hex[^18]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/EIet2oTIB8" %}
Show me the Lens and Farcaster followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
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
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x3a134eb077cda063dec6f507e969556ddcb4e1f9"],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "florist",
                "profileTokenId": "13915",
                "profileTokenIdHex": "",
                "userId": "13915",
                "userAssociatedAddresses": [
                  "0x3a134eb077cda063dec6f507e969556ddcb4e1f9"
                ]
              }
            ]
          },
          "followerProfileId": "13915",
          "followingAddress": {
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
                "profileName": "lens/@betashop9",
                "profileTokenId": "94472",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "602"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Lens Followers of Lens Profile(s) that Hold ERC20 Token(s)

You can get the list of Lens followers of Lens profile(s) and that hold certain ERC20 token(s), e.g. [USDC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48&tokenType=&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29&inputType=TOKEN&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-19)[^19] or Lens profile ID (decimal[^20] or hex[^21]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/EozH1sSEkw" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also hold USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["lens/@stani", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]}, tokenType: {_eq: ERC20}}}
        ) {
<strong>          formattedAmount # Indicate if there's any USDC, otherwise `null`
</strong>        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 0.507491
              }
            ]
          },
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Lens Followers of Lens Profile(s) that Hold ERC721/1155 NFT(s)

You can get the list of Lens followers of Lens profile(s) and that hold certain ERC721 or ERC1155 NFT(s), e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D&tokenType=&rawInput=%23%E2%8E%B1BoredApeYachtClub%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+NFT_COLLECTION+ethereum+null%29&inputType=NFT_COLLECTION&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-22)[^22] or Lens profile ID (decimal[^23] or hex[^24]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/YY1o5tZHx7" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also hold BAYC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
        dappName: { _eq: lens }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
          formattedAmount # Indicate if there's any BAYC, otherwise `null`
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 1
              }
            ]
          },
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Lens Followers of Lens Profile(s) that Hold POAP(s)

You can get the list of Lens followers of Lens profile(s) and that hold certain POAP(s), e.g. [EthCC\[6\] Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=&address=141910&tokenType=&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29&inputType=POAP&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=gnosis), by inputting either their 0x address, [Lens profile name](#user-content-fn-25)[^25] or Lens profile ID (decimal[^26] or hex[^27]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/Yr5VUSbjgp" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also hold EthCC\[6] POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
        dappName: { _eq: lens }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        poaps(input: { filter: { eventId: { _eq: "141910" } } }) {
          mintHash
          mintOrder
        }
      }
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": ["0x82cb4081ddba4ecb2eb45ad4b9ee7405351738b7"],
            "socials": [
              {
                "profileName": "lens/@elsu83877827",
                "profileTokenId": "19989",
                "profileTokenIdHex": ""
              }
            ],
            "poaps": null
          },
          "followingAddress": {
            "addresses": ["0x8ec94086a724cbec4d37097b8792ce99cadcd520"],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        }
        // more followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get Lens Followers of Lens Profile(s) that Hold Certain Amount of ERC20 Token(s)

You can get the list of Lens followers of Lens profile(s) and that hold certain amount of ERC20 token(s), e.g. more than or equal to 1000 [USDC](https://explorer.airstack.xyz/token-holders?activeView=&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48&tokenType=&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29&inputType=TOKEN&tokenFilters=&activeViewToken=&activeViewCount=&blockchainType=&sortOrder=&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-28)[^28] or Lens profile ID (decimal[^29] or hex[^30]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/6tkOHX8r23" %}
Show me the Lens followers of lens/@stani, lens_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also hold at least 1000 USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
    input: {
      filter: {
        identity: {
          _in: [
            "lens/@stani"
            "lens_id:0x024"
            "vitalik.eth"
            "0xeaf55242a90bb3289dB8184772b0B98562053559"
          ]
        }
        dappName: { _eq: lens }
      }
      blockchain: ALL
      limit: 200
    }
  ) {
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
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
      followingAddress {
        addresses
        domains {
          name
        }
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
      }
      followingProfileId
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xc00bdafae2ddfca38c87b77c567d5f1891aa35fc"
            ],
            "socials": [
              {
                "profileName": "lens/@takminn",
                "profileTokenId": "87700",
                "profileTokenIdHex": ""
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1099.566913 // hold more than 1000 USDC
</strong>              }
            ]
          },
          "followingAddress": {
            "addresses": [
              "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
            ],
            "domains": [
              {
                "name": "tiktoktopmoments.eth"
              },
              {
                "name": "crimsonchin.eth"
              },
              {
                "name": "levelsdigital.eth"
              },
              {
                "name": "saintevie.eth"
              },
              {
                "name": "bradorbradley.eth"
              }
            ],
            "socials": [
              {
                "profileName": "lens/@westlakevillage",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@brad",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@bradorbradley",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "lens/@hanimourra",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "36"
        },
        // more followers
      ]
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Lens Followers data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
- [Wallet API Reference](../../api-references/api-reference/wallet-api/)
- [Lens Following](broken-reference/)
- [Farcaster Followers](../farcaster/farcaster-followers.md)
- [Farcaster Following](../farcaster/farcaster-following.md)

[^1]: `profileName`
[^2]: `profileTokenId`
[^3]: `profileTokenIdHex`
[^4]: `profileName`
[^5]: `profileTokenId`
[^6]: `profileTokenIdHex`
[^7]: `profileName`
[^8]: `profileTokenId`
[^9]: `profileTokenIdHex`
[^10]: `profileName`
[^11]: `profileTokenId`
[^12]: `profileTokenIdHex`
[^13]: `profileName`
[^14]: `profileTokenId`
[^15]: `profileTokenIdHex`
[^16]: `profileName`
[^17]: `profileTokenId`
[^18]: `profileTokenIdHex`
[^19]: `profileName`
[^20]: `profileTokenId`
[^21]: `profileTokenIdHex`
[^22]: `profileName`
[^23]: `profileTokenId`
[^24]: `profileTokenIdHex`
[^25]: `profileName`
[^26]: `profileTokenId`
[^27]: `profileTokenIdHex`
[^28]: `profileName`
[^29]: `profileTokenId`
[^30]: `profileTokenIdHex`
