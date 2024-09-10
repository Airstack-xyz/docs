---
description: >-
  Learn how to fetch Farcaster followers data and its various use cases and
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

# 🫂 Farcaster Followers

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with Farcaster.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get Farcaster Followers of Farcaster User(s)](farcaster-followers.md#get-farcaster-followers-of-farcaster-user-s)
- [Check A Given Farcaster User is A Follower of User(s) on Farcaster](farcaster-followers.md#check-a-given-farcaster-user-is-a-follower-of-user-s-on-farcaster)
- [Get Farcaster Users that have a certain amount of Followers](farcaster-followers.md#get-farcaster-users-that-have-a-certain-amount-of-followers)
- [Get All Farcaster Followers That Also Have A Power Badge](farcaster-followers.md#get-all-farcaster-followers-that-also-have-a-power-badge)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

### Get Started

**JavaScript/TypeScript/Python**

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

**Other Programming Languages**

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## Get Farcaster Followers of Farcaster User(s)

You can get the list of Farcaster followers of Farcaster user(s) by inputting [0x address](#user-content-fn-1)[^1], ENS domain, [Farcaster Name](#user-content-fn-2)[^2], or Farcaster ID:

### Try Demo

{% embed url="https://app.airstack.xyz/query/NwB5fVxenM" %}
Show me Farcaster followers of fc_fname:dwr.eth, fc_fid:602, varunsrin.eth, and 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  SocialFollowers(
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
    Follower {
      followerAddress {
        addresses
        socials(input: { filter: { dappName: { _eq: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
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
            "addresses": ["0x08f2ca269fc4ca7ec4f86c434dc13d120a116271"],
            "socials": [
              {
                "profileName": "dian",
                "userId": "9739",
                "userAssociatedAddresses": [
                  "0x08f2ca269fc4ca7ec4f86c434dc13d120a116271"
                ]
              }
            ]
          },
          "followerProfileId": "9739",
          "followerTokenId": "",
          "followingAddress": {
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
          "followingProfileId": "2"
        }
        // more Farcaster followers
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Check A Given Farcaster User is A Follower of User(s) on Farcaster

You can use Airstack to check if user(s) is following a given Farcaster user on Farcaster.

This can be done by providing the given [Farcaster name](#user-content-fn-3)[^3] or ID on the `Wallet` top-level query's `identity` input and the user(s) in the [`socialFollowers`](../../api-references/api-reference/socialfollowers-api.md):

### Try Demo

{% embed url="https://app.airstack.xyz/query/wBxhAKzKPR" %}
Show me if fc_fname:ipeciura.eth is a follower of a group of users on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query isFollowing {
  Wallet(input: {identity: "fc_fname:ipeciura.eth", blockchain: ethereum}) {
    socialFollowers( # Check if fc_fname:ipeciura.eth is a follower of these user identities on Lens
<strong>      input: {filter: {identity: {_in: ["0xeaf55242a90bb3289dB8184772b0B98562053559", "betashop.eth", "yosephks.cb.id", "lens/@deepesh", "lens_id:100275", "fc_fname:dawufi", "fc_fid:602"]}, dappName: {_eq: farcaster}}}
</strong>    ) {
      Follower {
        dappName
        dappSlug
        followingProfileId
        followerProfileId
        followingAddress {
          addresses
          socials {
            dappName
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
<strong>            "dappName": "farcaster", // follows on Farcaster
</strong>            "dappSlug": "farcaster_optimism",
            "followingProfileId": "602",
            "followerProfileId": "2602",
            "followingAddress": {
              "addresses": [
                "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                "0xeaf55242a90bb3289db8184772b0b98562053559"
              ],
              "socials": [
                {
                  "dappName": "farcaster",
<strong>                  "profileName": "betashop.eth" // ipeciura.eth is follower of betashop.eth
</strong>                },
                {
                  "dappName": "lens",
                  "profileName": "lens/@betashop9"
                }
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
<strong>            "dappName": "farcaster", // follows on Farcaster
</strong>            "dappSlug": "farcaster_optimism",
            "followingProfileId": "15971",
            "followerProfileId": "2602",
            "followingAddress": {
              "addresses": [
                "0xc6582cd12debdc9cbe4d972615589aba586550e7",
                "0xc7486219881c780b676499868716b27095317416"
              ],
              "socials": [
                {
                  "dappName": "farcaster",
<strong>                  "profileName": "yosephks.eth" // ipeciura.eth is follower of yosephks.eth
</strong>                },
                {
                  "dappName": "lens",
                  "profileName": "lens/@yosephks"
                }
              ],
              "domains": [
                {
                  "name": "yosephks.eth" 
                },
                {
                  "name": "yosephks.cb.id"
                }
              ]
            }
          },
          {
<strong>            "dappName": "farcaster", // follows on Farcaster
</strong>            "dappSlug": "farcaster_optimism",
            "followingProfileId": "6806",
            "followerProfileId": "2602",
            "followingAddress": {
              "addresses": [
                "0xe1b1e3bbf4f29bd7253d6fc1e2ddc9cacb0a546a",
                "0x0964256674e42d61f0ff84097e28f65311786ccb"
              ],
              "socials": [
                {
                  "dappName": "lens",
                  "profileName": "lens/@dawufi"
                },
                {
                  "dappName": "farcaster",
<strong>                  "profileName": "dawufi" // ipeciura.eth is follower of dawufi.eth
</strong>                }
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
          },
          // other followers
        ]
      }
    }
  }
}
</code></pre>

{% endtab %}
{% endtabs %}

If [`ipeciura.eth`](https://explorer.airstack.xyz/token-balances?address=fc_fname%3Aipeciura.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1fc_fname%3Aipeciura.eth%E2%8E%B1%28fc_fname%3Aipeciura.eth++ethereum+null%29&inputType=ADDRESS) is a follower of any of the users on Lens, then it will appear as a response in the `Follower` array as shown in the [sample response](farcaster-followers.md#response-1).

## Get Farcaster Users that have a certain amount of Followers

You can get the list of all Farcaster users that have a certain amount of followers, e.g. at least 1000 followers:

### Try Demo

{% embed url="https://app.airstack.xyz/query/BK9pLZ9OgA" %}
Show me all Farcaster users that have at least 1000 followers
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { followerCount: { _gte: 1000 }, dappName: { _eq: farcaster } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      profileName
      userId
      userAssociatedAddresses
      followerCount
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
          "profileName": "pfista",
          "userId": "451",
          "userAssociatedAddresses": [
            "0xd2344f892241e3a83c7eca8e586093714be8ca7f",
            "0x8a95d5f9ac0eebd5880199ea8fcd0fae845bbbf2",
            "0x92382f5c6ad39829a07537d18807462960025725"
          ],
          "followerCount": 7620
        },
        {
          "profileName": "jayme",
          "userId": "373",
          "userAssociatedAddresses": [
            "0x1ca66c990e86b750ea6b2180d17fff89273a5c0d",
            "0x9eab9d856a3a667dc4cd10001d59c679c64756e7"
          ],
          "followerCount": 9554
        },
        {
          "profileName": "macbudkowski",
          "userId": "1048",
          "userAssociatedAddresses": [
            "0x66cc8f831f2a05fde7573f18ba6abfef8ad59d34",
            "0x3e6c23cdaa52b1b6621dbb30c367d16ace21f760"
          ],
          "followerCount": 5766
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Farcaster Followers data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
