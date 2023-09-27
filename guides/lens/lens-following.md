---
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

# 💐 Lens Following

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

In this tutorial, you will learn how to fetch various use cases of Lens Following data.

In this guide you will learn how to use Airstack to:

* [Get Lens Following of Lens Profile(s)](lens-following.md#get-lens-following-of-lens-profile-s)
* [Check If User(s) is Following A Given Lens Profile on Lens](lens-following.md#check-if-user-s-is-following-a-given-lens-profile-on-lens)
* [Get The Most Recent Lens Following of Lens Profile(s)](lens-following.md#get-the-most-recent-lens-following-of-lens-profile-s)
* [Get The Earliest Lens Following of Lens Profile(s)](lens-following.md#get-the-most-recent-lens-following-of-lens-profile-s)
* [Get Lens Following of Lens Profile(s) that has ENS Domain](lens-following.md#get-lens-following-of-lens-profile-s-that-has-ens-domain)
* [Get Lens Following of Lens Profile(s) that has XMTP Enabled](lens-following.md#get-lens-profiles-that-have-a-certain-amount-of-following)
* [Get Lens Profiles that have a certain amount of Following](lens-following.md#get-lens-profiles-that-have-a-certain-amount-of-following)
* [Get Lens and Farcaster Following of Lens Profile(s)](lens-following.md#get-lens-and-farcaster-following-of-lens-profile-s)
* [Get Lens Following of Lens Profile(s) that Hold ERC20 Token(s)](lens-following.md#get-lens-following-of-lens-profile-s-that-hold-erc20-token-s)
* [Get Lens Following of Lens Profile(s) that Hold ERC721/1155 NFT(s)](lens-following.md#get-lens-following-of-lens-profile-s-that-hold-erc721-1155-nft-s)
* [Get Lens Following of Lens Profile(s) that Hold POAP(s)](lens-following.md#get-lens-following-of-lens-profile-s-that-hold-poap-s)
* [Get Lens Following of Lens Profile(s) that Hold Certain Amount of ERC20 Token(s)](lens-following.md#get-lens-following-of-lens-profile-s-that-hold-certain-amount-of-erc20-token-s)

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

## Get Lens Following of Lens Profile(s)

You can get the list of Lens following of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-1)[^1] or Lens profile ID (decimal[^2] or hex[^3]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/XTj0vyFSrL" %}
Show me the Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "3568",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Check If User(s) is Following A Given Lens Profile on Lens

You can use Airstack to check if user(s) is following a given Lens profile on Lens protocol.

This can be done by providing the given Lens profile on the `Wallet` top-level query's `identity` input and the user(s) in the [`socialFollowings`](../../api-references/api-reference/socialfollowings-api.md):

### Try Demo

{% embed url="https://app.airstack.xyz/query/PycMq2SXq7" %}
Show me if a group of users is following shnoodles.lens on Lens
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query isFollowing {
  Wallet(input: {identity: "shnoodles.lens", blockchain: ethereum}) {
    socialFollowings( # Check if these user identities is following shnoodles.lens on Lens
<strong>      input: {filter: {identity: {_in: ["0xeaf55242a90bb3289dB8184772b0B98562053559", "betashop.eth", "yosephks.cb.id", "deepesh.lens", "lens_id:100275", "fc_fname:dawufi", "fc_fid:602"]}, dappName: {_eq: lens}}}
</strong>    ) {
      Following {
        dappName
        dappSlug
        followingProfileId
        followerProfileId
        followerAddress {
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
      "socialFollowings": {
        "Following": [
          {
            "dappName": "lens",
            "dappSlug": "lens_polygon",
            "followingProfileId": "27561",
            "followerProfileId": "122312",
            "followerAddress": {
              "addresses": [
                "0x0964256674e42d61f0ff84097e28f65311786ccb"
              ],
              "socials": [
                {
<strong>                  "profileName": "dawufi.lens" // dawufi.lens is following ipeciura.eth's on Lens
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

If any of the user is following [`shnoodles.lens`](https://explorer.airstack.xyz/token-balances?address=shnoodles.lens\&blockchain=ethereum\&rawInput=%23%E2%8E%B1shnoodles.lens%E2%8E%B1%28shnoodles.lens+ADDRESS+ethereum+null%29\&inputType=ADDRESS\&tokenType=\&activeView=\&activeTokenInfo=\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=) on Lens, then it will appear as a response in the `Following` array as shown in the [sample response](lens-following.md#response-1).

## Get The Most Recent Lens Following of Lens Profile(s)

You can get the list of most recent Lens following of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-4)[^4] or Lens profile ID (decimal[^5] or hex[^6]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/bck3CGCmqN" %}
Show me the most recent Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200, order: {followingSince: DESC}}
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x6db4d0682d7d7bb68bd00c774dbdd05b91925c13"
            ],
            "socials": [
              {
                "profileName": "unlonely.lens",
                "profileTokenId": "120779",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "120779",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "68"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Earliest Lens Following of Lens Profile(s)

You can get the list of the earliest Lens following of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-7)[^7] or Lens profile ID (decimal[^8] or hex[^9]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/K0IH6CK0kT" %}
Show me the earliest Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200, order: {followingSince: ASC}}
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
        socials(input: {filter: {dappName: {_eq: lens}}}) {
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
```json
{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x6db4d0682d7d7bb68bd00c774dbdd05b91925c13"
            ],
            "socials": [
              {
                "profileName": "unlonely.lens",
                "profileTokenId": "120779",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "120779",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "68"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Following of Lens Profile(s) that has ENS Domain

You can get the list of Lens following of Lens profile(s) and check if they have any ENS domain by inputting either their 0x address, [Lens profile name](#user-content-fn-10)[^10] or Lens profile ID (decimal[^11] or hex[^12]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/EZRlf8y3Ar" %}
Show me all the Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 and their ENS domains
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
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
      followingProfileId
      followerAddress {
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
      followerProfileId
      followerTokenId
    }
  }
}
</code></pre>
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
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "domains": [
              {
                "name": "abertog.eth",
                "isPrimary": true
              },
              {
                "name": "bertomeu.eth",
                "isPrimary": false
              }
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followingProfileId": "3568",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

You can use the `followingAddress.domains` that will return an array to see if there's any domain owned by the following.

If the length of the `followingAddress.domains` array is non-zero, then the following has at least one ENS domain. Otherwise, it does not have any ENS domain.

## Get Lens Following of Lens Profile(s) that has XMTP Enabled

You can get the list of Lens following of Lens profile(s) and check if they have XMTP enabled by inputting either their 0x address, [Lens profile name](#user-content-fn-13)[^13] or Lens profile ID (decimal[^14] or hex[^15]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/pJYpYdO13X" %}
Show me all the Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 and check if their XMTP is enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        xmtp {
<strong>          isXMTPEnabled # Indicate if following have XMTP enabled
</strong>        }
      }
      followingProfileId
      followerAddress {
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
      followerProfileId
      followerTokenId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ],
            "xmtp": [
              {
<strong>                "isXMTPEnabled": true // have XMTP enabled
</strong>              }
            ]
          },
          "followingProfileId": "3568",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Lens Profiles that have a certain amount of Following

You can get all the Lens profiles that have a certain amount of following, e.g. more than or equal to 1000 following counts on Lens:

### Try Demo

{% embed url="https://app.airstack.xyz/query/cw55sMgRzw" %}
Show me all Lens profiles that have more than or equal to 1000 following
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {filter: {followingCount: {_gte: 1000}, dappName: {_eq: lens}}, blockchain: ethereum, limit: 200}
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
<strong>      followingCount # Total following count
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
          "profileName": "thedogist.lens",
          "profileTokenId": "45164",
          "profileTokenIdHex": "0x0b06c",
          "followingCount": 10507
        },
        {
          "profileName": "9xkiwi.lens",
          "profileTokenId": "961",
          "profileTokenIdHex": "0x03c1",
          "followingCount": 4930
        },
        {
          "profileName": "eth666.lens",
          "profileTokenId": "17693",
          "profileTokenIdHex": "0x0451d",
          "followingCount": 1165
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens and Farcaster Following of Lens Profile(s)

You can get the list of Lens and Farcaster following of Lens profile(s) by inputting either their 0x address, [Lens profile name](#user-content-fn-16)[^16] or Lens profile ID (decimal[^17] or hex[^18]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/KxjSJmLWUQ" %}
Show all Lens and Farcaster following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials {
          dappName
          profileName
          profileTokenId
          profileTokenIdHex
          userId
        }
      }
      followingProfileId
      followerAddress {
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
      followerProfileId
      followerTokenId
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
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "dappName": "lens",
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": "",
                "userId": "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
              },
              {
                "dappName": "lens",
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": "",
                "userId": "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
              }
            ]
          },
          "followingProfileId": "3568",
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get Lens Following of Lens Profile(s) that Hold ERC20 Token(s)

You can get the list of Lens following of Lens profile(s) that also hold certain ERC20 token(s), e.g. [USDC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&tokenType=\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29\&inputType=TOKEN\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-19)[^19] or Lens profile ID (decimal[^20] or hex[^21]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/MMp5HQgKaD" %}
Show me Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also holds USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]}, tokenType: {_eq: ERC20}}}
        ) {
<strong>          formattedAmount # Indicate if following have any USDC
</strong>        }
      }
      followerAddress {
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
      followerProfileId
      followerTokenId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 13.487059 // Hold some USDC
</strong>              }
            ]
          },
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Lens Following of Lens Profile(s) that Hold ERC721/1155 NFT(s)

You can get the list of Lens following of Lens profile(s) that also hold certain ERC721 or ERC1155 NFT(s), e.g. [BAYC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D\&tokenType=\&rawInput=%23%E2%8E%B1BoredApeYachtClub%E2%8E%B1%280xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D+NFT\_COLLECTION+ethereum+null%29\&inputType=NFT\_COLLECTION\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-22)[^22] or Lens profile ID (decimal[^23] or hex[^24]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/fXbfAVAjOZ" %}
Show me Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also holds BAYC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql"> query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}, dappName: {_eq: lens}}, blockchain: ALL}
  ) {
    Following {
      followingAddress {
        addresses
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"]}, tokenType: {_in: [ERC721, ERC1155]}}}
        ) {
<strong>          formattedAmount # Indicate if following have any BAYC
</strong>        }
      }
      followerAddress {
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
      followerProfileId
      followerTokenId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowings": {
      "Following": [
        {
          "followingAddress": {
            "addresses": [
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ],
            "tokenBalances": [
              {
<strong>                "formattedAmount": 1 // Hold 1 BAYC NFT
</strong>              }
            ]
          },
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Lens Following of Lens Profile(s) that Hold POAP(s)

You can get the list of Lens following of Lens profile(s) that also hold certain POAP(s), e.g. [EthCC \[6\] Attendee POAP](https://explorer.airstack.xyz/token-holders?activeView=\&address=141910\&tokenType=\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29\&inputType=POAP\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=gnosis), by inputting either their 0x address, [Lens profile name](#user-content-fn-25)[^25] or Lens profile ID (decimal[^26] or hex[^27]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/kMJRUHXQlA" %}
Show me Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also holds EthCC\[6] – Attendee POAP
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        poaps(input: {filter: {eventId: {_eq: "141910"}}}) {
          mintHash
          mintOrder
        }
      }
      followerAddress {
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
      followerProfileId
      followerTokenId
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
              "0x008b1ffe244bf31896c833bff4ad9009e7a0f4eb"
            ],
            "socials": [
              {
                "profileName": "yummy.lens",
                "profileTokenId": "47982",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "abertog.lens",
                "profileTokenId": "3568",
                "profileTokenIdHex": ""
              }
            ],
<strong>            "poaps": [ // attended the EthCC[6] POAP event
</strong>              {
                "mintHash": "0x8b0e8d3ca7ca205e797660469925f6ed5a7b2d539421d7331ff19081f7ec48e2",
                "mintOrder": 789
              }
            ]
          },
          "followerAddress": {
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
                "profileName": "westlakevillage.lens",
                "profileTokenId": "99755",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "brad.lens",
                "profileTokenId": "116598",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "bradorbradley.lens",
                "profileTokenId": "36",
                "profileTokenIdHex": ""
              },
              {
                "profileName": "hanimourra.lens",
                "profileTokenId": "116239",
                "profileTokenIdHex": ""
              }
            ]
          },
          "followerProfileId": "36",
          "followerTokenId": "245"
        },
        // more following
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get Lens Following of Lens Profile(s) that Hold Certain Amount of ERC20 Token(s)

You can get the list of Lens following of Lens profile(s) that also hold certain amount of ERC20 token(s), e.g. more than or equal to 1000 [USDC](https://explorer.airstack.xyz/token-holders?activeView=\&address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&tokenType=\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29\&inputType=TOKEN\&tokenFilters=\&activeViewToken=\&activeViewCount=\&blockchainType=\&sortOrder=\&blockchain=ethereum), by inputting either their 0x address, [Lens profile name](#user-content-fn-28)[^28] or Lens profile ID (decimal[^29] or hex[^30]):

### Try Demo

{% embed url="https://app.airstack.xyz/query/5r5iFwz8n8" %}
Show me Lens following of stani.lens, lens\_id:0x024, vitalik.eth, 0xeaf55242a90bb3289dB8184772b0B98562053559 that also holds at least 1000 USDC
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["stani.lens", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}, dappName: {_eq: lens}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        tokenBalances(
          input: {filter: {tokenAddress: {_in: ["0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48"]}, tokenType: {_eq: ERC20}, formattedAmount: {_gte: 1000}}}
        ) {
          formattedAmount
        }
      }
      followerAddress {
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
      followerProfileId
      followerTokenId
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
              "0x6c1e581e4ea169c28b9ca03f17f7370e030548ac"
            ],
            "socials": [
              {
                "profileName": "camillionaire",
                "profileTokenId": "12637",
                "profileTokenIdHex": "0x0315d"
              },
              {
                "profileName": "camillionaire.lens",
                "profileTokenId": "27299",
                "profileTokenIdHex": "0x06aa3"
              }
            ],
            "tokenBalances": [
              {
                "formattedAmount": 2746.94
              }
            ]
          },
          "followerAddress": {
            "addresses": [
              "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
            ],
            "domains": null,
            "socials": [
              {
                "profileName": "stani.lens",
                "profileTokenId": "5",
                "profileTokenIdHex": "0x05"
              },
              {
                "profileName": "lilgho.lens",
                "profileTokenId": "110056",
                "profileTokenIdHex": "0x01ade8"
              },
              {
                "profileName": "lensofficial.lens",
                "profileTokenId": "47319",
                "profileTokenIdHex": "0x0b8d7"
              }
            ]
          },
          "followerProfileId": "5",
          "followerTokenId": "22"
        },
        // more following
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Lens Following data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
* [Wallet API Reference](../../api-references/api-reference/wallet-api/)
* [Lens Followers](broken-reference)
* [Farcaster Followers](../farcaster/farcaster-followers.md)
* [Farcaster Following](../farcaster/farcaster-following.md)

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
