---
description: Learn how to use Airstack to check if multiple users have XMTP or not.
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

# 👬 Check Multiple Users

## 👬 Check Multiple Users

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to check if multiple users have XMTP enabled:

- [Bulk Check Multiple Users Has XMTP](check-multiple-users.md#bulk-check-multiple-users-have-xmtp)
- [Bulk Check Lens Profiles Have XMTP](check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
- [Bulk Check Farcasters Have XMTP](check-multiple-users.md#bulk-check-farcasters-have-xmtp)

### Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL
- Basic knowledge of [XMTP](https://xmtp.org)

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

### **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

### Bulk Check Multiple Users Have XMTP

To check multiple users if they have XMTP is similar to checking for single users.

Swap `_eq` comparator with `_in` to allow array inputs:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/wXLtoDe4F8" %}
Bulk Check Multiple Users Have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  XMTPs(
    input: {
      blockchain: ALL
      filter: {
        owner: {
          _in: [
            "0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa"
            "shanemac.eth"
            "lens/@vitalik"
          ]
        }
      }
    }
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        addresses
        domains {
          name
        }
        socials {
          dappName
          profileName
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
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "addresses": ["0xa64af7f78de39a238ecd4fff7d6d410dbace2df0"],
            "domains": [
              {
                "name": "shanemac.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "shanemac"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@shanemac"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"],
            "domains": [
              {
                "name": "satoshinart.eth"
              }
            ],
            "socials": [
              {
                "dappName": "farcaster",
                "profileName": "vbuterin"
              },
              {
                "dappName": "lens",
                "profileName": "lens/@vitalik"
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "addresses": ["0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa"],
            "domains": [],
            "socials": null
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
The response will return fewer amount of element in the `XMTP` array if some of the `owner` input has no XMTP.

Therefore, you can use `owner` field response to double check if in more details if a specific user has XMTP or not.
{% endhint %}

### Bulk Check Lens Profiles Have XMTP

You can bulk check Lens profiles have XMTP by providing either Lens profile name[^1] or ID into the `owner` input array:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/0QqNIEIXLH" %}
Bulk Check Lens Profiles Have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query BulkFetchLensandXMTP {
  XMTPs(
    input: {
      blockchain: ALL
      filter: {
        owner: {
          _in: ["lens/@hoobastank", "lens_id:0x09718", "lens/@barisadiguzel"]
        }
      }
      limit: 100
    }
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenIdHex
          userAssociatedAddresses
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
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "lens/@hoobastank",
                "profileTokenIdHex": "0x012d2b",
                "userAssociatedAddresses": [
                  "0xf81c128e1c660d08c1f33ccd5a06d040a37245eb"
                ]
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "lens/@22776",
                "profileTokenIdHex": "0x09718",
                "userAssociatedAddresses": [
                  "0x53af8473b558dc42275abcfcaaf3ce2fcbd2c727"
                ]
              }
            ]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "lens/@barisadiguzel",
                "profileTokenIdHex": "0x0e8b6",
                "userAssociatedAddresses": [
                  "0x32483c2ef44de655781ce54e822130ff9d34c0c8"
                ]
              }
            ]
          }
        }
      ],
      "pageInfo": {
        "nextCursor": "",
        "prevCursor": ""
      }
    }
  }
}
```

{% endtab %}
{% endtabs %}

### Bulk Check Farcasters Have XMTP

You can bulk check Farcasters have XMTP by providing either Farcaster name[^2] or ID into the `owner` input array:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/v4nVRtJsw5" %}
Bulk Query Farcasters Have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query BulkFetchFarcasterHaveXMTP {
  XMTPs(
    input: {
      blockchain: ALL
      filter: {
        owner: {
          _in: ["fc_fname:vitalik.eth", "fc_fname:varunsrin.eth", "fc_fid:602"]
        }
      }
      limit: 100
    }
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        socials(input: { filter: { dappName: { _in: farcaster } } }) {
          profileName
          userId
          userAssociatedAddresses
        }
        addresses
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
    "XMTPs": {
      "XMTP": [
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "vitalik.eth",
                "userId": "5650",
                "userAssociatedAddresses": [
                  "0xadd746be46ff36f10c81d6e3ba282537f4c68077",
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
              }
            ],
            "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "varunsrin.eth",
                "userId": "2",
                "userAssociatedAddresses": [
                  "0x182327170fc284caaa5b1bc3e3878233f529d741",
                  "0x91031dcfdea024b4d51e775486111d2b2a715871",
                  "0x4114e33eb831858649ea3702e1c9a2db3f626446"
                ]
              }
            ],
            "addresses": ["0x182327170fc284caaa5b1bc3e3878233f529d741"]
          }
        },
        {
          "isXMTPEnabled": true,
          "owner": {
            "socials": [
              {
                "profileName": "betashop.eth",
                "userId": "602",
                "userAssociatedAddresses": [
                  "0x66bd69c7064d35d146ca78e6b186e57679fba249",
                  "0xeaf55242a90bb3289db8184772b0b98562053559"
                ]
              }
            ],
            "addresses": ["0xeaf55242a90bb3289db8184772b0b98562053559"]
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding checking XMTP for multiple users with various [web3 identities](#user-content-fn-3)[^3], please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

- [XMTPs API Reference](../../api-references/api-reference/xmtps-api.md)
- [Has XMTP For Lens Developers](../lens/has-xmtp.md)
- [Has XMTP For Farcaster Developers](../farcaster/has-xmtp.md)

1. e.g. `lens_id:0x09718`
2. e.g. `fc_fid:5650`

[^1]: e.g. lens/@vitalik
[^2]: e.g. `fc_fname:vitalik.eth`
[^3]: 0x address, ENS, Lens, Farcaster
