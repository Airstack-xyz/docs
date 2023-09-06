---
description: >-
  Learn how to use Airstack to get all social followers or following that have
  XMTP enabled.
---

# 🎉 Social Follows

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

In this tutorial, you will learn how to check whether an array of user followers or following have XMTP enabled or not.

{% hint style="info" %}
Currently, Airstack **ONLY** returns Farcaster follows data. Lens follows data will be coming very soon.
{% endhint %}

In this guide, you will learn how to use [Airstack](https://airstack.xyz) to check if holders of a given NFT or POAP have XMTP enabled:

* [Get All Followers of User(s) that have XMTP Enabled](social-follows.md#get-all-followers-of-user-s-that-have-xmtp-enabled)
* [Get All Following of User(s) that have XMTP Enabled](social-follows.md#get-all-following-of-user-s-that-have-xmtp-enabled)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [XMTP](https://xmtp.org)

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

## Get All Followers of User(s) that have XMTP Enabled

You can all the followers of user(s) and see whether they have their XMTP enabled for messaging by providing [0x address](#user-content-fn-1)[^1], ENS[^2], [Lens profile](#user-content-fn-3)[^3], or Farcaster[^4]:&#x20;

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/RRiQCiLfbC" %}
Show all followers of vitalik.eth and 0xeaf55242a90bb3289dB8184772b0B98562053559 if they have XMTP enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowers(
    input: {filter: {identity: {_in: ["vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        domains {
          name
          resolvedAddress
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
        }
        xmtp {
          isXMTPEnabled
        }
      }
      followerProfileId
      followerTokenId
      followingAddress {
        addresses
        domains {
          name
          resolvedAddress
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
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
<pre class="language-json"><code class="lang-json">{
  "data": {
    "SocialFollowers": {
      "Follower": [
        {
          "followerAddress": {
            "addresses": [
              "0xac7a709f09eac9769c567a24a858fe65b44850af",
              "0x3a497a2a8b35b934bf271938502ca24e444e689e"
            ],
            "domains": [
              {
                "name": "pfizergang.eth",
                "resolvedAddress": "0x3a497a2a8b35b934bf271938502ca24e444e689e"
              }
            ],
            "socials": [
              {
                "profileName": "pfizergang.lens",
                "profileTokenId": "66242",
                "profileTokenIdHex": "0x0102c2",
                "userId": "0x3a497a2a8b35b934bf271938502ca24e444e689e",
                "userAssociatedAddresses": [
                  "0x3a497a2a8b35b934bf271938502ca24e444e689e"
                ]
              },
              {
                "profileName": "pfizergang",
                "profileTokenId": "16250",
                "profileTokenIdHex": "0x03f7a",
                "userId": "16250",
                "userAssociatedAddresses": [
                  "0xac7a709f09eac9769c567a24a858fe65b44850af",
                  "0x3a497a2a8b35b934bf271938502ca24e444e689e"
                ]
              }
            ],
            "xmtp": [
              {
<strong>                "isXMTPEnabled": true // Indicate that XMTP is enabled
</strong>              }
            ]
          },
          "followerProfileId": "16250",
          "followerTokenId": "",
          "followingAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ],
            "domains": [
              {
                "name": "jasongoldberg.eth",
                "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
              },
              {
                "name": "betashop.eth",
                "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
              }
            ],
            "socials": [
              {
                "profileName": "betashop.eth",
                "profileTokenId": "602",
                "profileTokenIdHex": "0x025a",
                "userId": "602",
                "userAssociatedAddresses": [
                  "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              },
              {
                "profileName": "betashop9.lens",
                "profileTokenId": "94472",
                "profileTokenIdHex": "0x017108",
                "userId": "0xeaf55242a90bb3289db8184772b0b98562053559",
                "userAssociatedAddresses": [
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              }
            ]
          },
          "followingProfileId": "602"
        },
        // more followers
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get All Following of User(s) that have XMTP Enabled

You can all the following of user(s) and see whether they have their XMTP enabled for messaging by providing [0x address](#user-content-fn-5)[^5], ENS[^6], [Lens profile](#user-content-fn-7)[^7], or Farcaster[^8]:

### Try Demo

{% embed url="https://app.airstack.xyz/query/AQS3vZZW9b" %}
Show all following of vitalik.eth and 0xeaf55242a90bb3289dB8184772b0B98562053559 if they have XMTP enabled
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
    input: {filter: {identity: {_in: ["0xeaf55242a90bb3289dB8184772b0B98562053559", "vitalik.eth", "bradorbradley.lens", "fc_fname:dwr.eth"]}}, blockchain: ALL, limit: 200}
  ) {
    Following {
      followingAddress {
        addresses
        domains {
          name
          resolvedAddress
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
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
          resolvedAddress
        }
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
          userId
          userAssociatedAddresses
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
              "0xed03d7c78f164b66233853a07ee80b7911f7ea2e",
              "0x8457b099bc9fa96a45d2c39c67871713cc50ccb7",
              "0x4b2df5354934fce9179828d3cd600c93e4962482",
              "0x471e9b5f89f827045e6d9ec2083a310c27459157"
            ],
            "domains": [
              {
                "name": "snapstore.eth",
                "resolvedAddress": "0xed03d7c78f164b66233853a07ee80b7911f7ea2e"
              },
              {
                "name": "lludo.eth",
                "resolvedAddress": "0x471e9b5f89f827045e6d9ec2083a310c27459157"
              },
              {
                "name": "ludo.landry.eth",
                "resolvedAddress": "0x471e9b5f89f827045e6d9ec2083a310c27459157"
              },
              {
                "name": "landry.eth",
                "resolvedAddress": "0x471e9b5f89f827045e6d9ec2083a310c27459157"
              },
              {
                "name": "onetown.eth",
                "resolvedAddress": "0xed03d7c78f164b66233853a07ee80b7911f7ea2e"
              },
              {
                "name": "monkeypod.eth",
                "resolvedAddress": "0x471e9b5f89f827045e6d9ec2083a310c27459157"
              }
            ],
            "socials": [
              {
                "profileName": "onetown.lens",
                "profileTokenId": "107023",
                "profileTokenIdHex": "0x01a20f",
                "userId": "0xed03d7c78f164b66233853a07ee80b7911f7ea2e",
                "userAssociatedAddresses": [
                  "0xed03d7c78f164b66233853a07ee80b7911f7ea2e"
                ]
              },
              {
                "profileName": "ludo",
                "profileTokenId": "165",
                "profileTokenIdHex": "0x0a5",
                "userId": "165",
                "userAssociatedAddresses": [
                  "0xed03d7c78f164b66233853a07ee80b7911f7ea2e",
                  "0x8457b099bc9fa96a45d2c39c67871713cc50ccb7",
                  "0x4b2df5354934fce9179828d3cd600c93e4962482",
                  "0x471e9b5f89f827045e6d9ec2083a310c27459157"
                ]
              },
              {
                "profileName": "lludo.lens",
                "profileTokenId": "106216",
                "profileTokenIdHex": "0x019ee8",
                "userId": "0x471e9b5f89f827045e6d9ec2083a310c27459157",
                "userAssociatedAddresses": [
                  "0x471e9b5f89f827045e6d9ec2083a310c27459157"
                ]
              }
            ],
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "followingProfileId": "165",
          "followerAddress": {
            "addresses": [
              "0x66bd69c7064d35d146ca78e6b186e57679fba249",
              "0xeaf55242a90bb3289db8184772b0b98562053559"
            ],
            "domains": [
              {
                "name": "jasongoldberg.eth",
                "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
              },
              {
                "name": "betashop.eth",
                "resolvedAddress": "0xeaf55242a90bb3289db8184772b0b98562053559"
              }
            ],
            "socials": [
              {
                "profileName": "betashop.eth",
                "profileTokenId": "602",
                "profileTokenIdHex": "0x025a",
                "userId": "602",
                "userAssociatedAddresses": [
                  "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              },
              {
                "profileName": "betashop9.lens",
                "profileTokenId": "94472",
                "profileTokenIdHex": "0x017108",
                "userId": "0xeaf55242a90bb3289db8184772b0b98562053559",
                "userAssociatedAddresses": [
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              }
            ]
          },
          "followerProfileId": "602",
          "followerTokenId": ""
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

If you have any questions or need help regarding checking XMTP for holders of a given NFT or POAP, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
* [Has XMTP For Lens Developers](../lens/has-xmtp.md)
* [Has XMTP For Farcaster Developers](../farcaster/has-xmtp.md)

[^1]: e.g. `0xeaf55242a90bb3289dB8184772b0B98562053559`

[^2]: e.g. `vitalik.eth`

[^3]: e.g. Lens profile name `bradorbradley.lens` or  Lens profile id, either in decimal or hex, `lens_id:0x24`

[^4]: e.g. Farcaster name `fc_fname:dwr.eth` or Farcaster id `fc_fid:3`

[^5]: e.g. `0xeaf55242a90bb3289dB8184772b0B98562053559`

[^6]: e.g. `vitalik.eth`

[^7]: e.g. Lens profile name `bradorbradley.lens` or  Lens profile id, either in decimal or hex, `lens_id:0x24`

[^8]: e.g. Farcaster name `fc_fname:dwr.eth` or Farcaster id `fc_fid:3`
