---
description: Learn how to check if Lens profiles have XMTP enabled on their account or not.
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

# 📬 Has XMTP

## 📬 Has XMTP

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

[XMTP](https://xmtp.org) is an Ethereum-based messaging protocol.

Developers building with [XMTP](https://xmtp.org) can use [Airstack](https://airstack.xyz) to enable [Lens](https://lens.xyz) users to seamlessly message Ethereum, ENS, Lens, and Farcaster users with XMTP-enabled addresses.

[Airstack](https://airstack.xyz) is utilized to:

1. check if users have [XMTP](https://xmtp.org) enabled (required in order to receive [XMTP](https://xmtp.org) messages)
2. resolve [Lens](https://lens.xyz) profile names or IDs to 0x Ethereum addresses and vice versa.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Check Lens Profile Has XMTP Enabled](has-xmtp.md#check-lens-profile-has-xmtp-enabled)
* [Bulk Check Lens Profiles Have XMTP Enabled](has-xmtp.md#bulk-check-lens-profiles-have-xmtp-enabled)
* [Get All 0x addresses of Lens profile(s)](has-xmtp.md#get-all-0x-addresses-of-lens-profile-s)
* [Get All Lens Profiles Owned by 0x address](has-xmtp.md#get-all-lens-profiles-owned-by-0x-address)
* [Get Lens Followers of Lens Profile(s) that has XMTP Enabled](has-xmtp.md#get-lens-followers-of-lens-profile-s-that-has-xmtp-enabled)
* [Get Lens Following of Lens Profile(s) that has XMTP Enabled](has-xmtp.md#get-lens-following-of-lens-profile-s-that-has-xmtp-enabled)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

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

### Check Lens Profile Has XMTP Enabled

You can check if a Lens profile has XMTP enabled or not:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/38TR2Gxuqf" %}
Check Lens Profile has XMTP enabled (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(
    input: { blockchain: ALL, filter: { owner: { _eq: "lens/@vitalik" } } }
  ) {
    XMTP {
      isXMTPEnabled
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
          "isXMTPEnabled": true
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Bulk Check Lens Profiles Have XMTP Enabled

You can bulk check a list of Lens profiles have XMTP enabled or not:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/nNmMc2WyKr" %}
Bulk Check If an Array of Lens Profiles Have XMTP (Demo)
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query BulkFetchLensProfilesHaveXMTP {
  XMTPs(
    input: {
      blockchain: ALL
      filter: {
        owner: { _in: ["lens/@hoobastank", "lens/@22776", "lens_id:0x0187b3"] }
      }
      limit: 100
    }
  ) {
    XMTP {
      isXMTPEnabled
      owner {
        socials(input: { filter: { dappName: { _eq: lens } } }) {
          profileName
          profileTokenId
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
                "profileTokenId": "77099",
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
                "profileTokenId": "38680",
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
                "profileName": "lens/@vitalik",
                "profileTokenId": "100275",
                "profileTokenIdHex": "0x0187b3",
                "userAssociatedAddresses": [
                  "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
                ]
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

### Get All 0x addresses of Lens profile(s)

You can resolve an array of Lens profile(s) to their 0x addresses for XMTP messaging and displayed in your XMTP application:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/rmLiJWfGxh" %}
Show 0x addresses of lens/@nader and Lens profile id 100275
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetAddressesOfLens {
  Socials(
    input: {
      filter: {
        identity: { _in: ["lens/@nader", "lens_id:0x0187b3"] }
        dappName: { _eq: lens }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
      userAssociatedAddresses
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
          "profileName": "lens/@nader",
          "profileTokenId": "10402",
          "profileTokenIdHex": "0x028a2",
          "userAssociatedAddresses": [
            "0xb2ebc9b3a788afb1e942ed65b59e9e49a1ee500d"
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
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All Lens Profiles Owned by 0x address

You can resolve any 0x address to their Farcaster accounts for XMTP messaging and displayed in your XMTP application:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/WAIEP13Nr5" %}
Show Lens owned by Ethereum address 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetLensOfEthereumAddress {
  Socials(
    input: {
      filter: {
        identity: { _in: ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"] }
        dappName: { _eq: lens }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      profileTokenId
      profileTokenIdHex
      userAssociatedAddresses
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json

  "data": {
    "Socials": {
      "Social": [
        {
          "profileName": "lens/@vitalik",
          "profileTokenId": "100275",
          "profileTokenIdHex": "0x0187b3",
          "userAssociatedAddresses": [
            "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Lens Followers of Lens Profile(s) that has XMTP Enabled

You can all the Lens followers of Lens profile(s) and see whether they have their XMTP enabled for messaging:

#### Try Demo

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  SocialFollowers(
    input: {filter: {dappName: {_eq: lens}, identity: {_in: ["lens/@stani", "lens_id:0x024", "vitalik.eth", "0xeaf55242a90bb3289dB8184772b0B98562053559"]}}, blockchain: ALL, limit: 200}
  ) {
    Follower {
      followerAddress {
        addresses
        socials {
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
        socials {
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
// Some code
```
{% endtab %}
{% endtabs %}

### Get Lens Following of Lens Profile(s) that has XMTP Enabled

You can all the Lens following of Lens profile(s) and see whether they have their XMTP enabled for messaging:

#### Try Demo

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  SocialFollowings(
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
    Following {
      followingAddress {
        addresses
        socials {
          profileName
          profileTokenId
          profileTokenIdHex
        }
        xmtp {
          isXMTPEnabled # Indicate if following have XMTP enabled
        }
      }
      followingProfileId
      followerAddress {
        addresses
        domains {
          name
        }
        socials {
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
// Some code
```
{% endtab %}
{% endtabs %}

### Developer Support

### Developer Support

If you have any questions or need help regarding checking if Lens profile(s) have XMTP enabled, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Has XMTP](../xmtp/)
  * [Check Lens Profile](../xmtp/check-single-user.md#by-lens-profile)
  * [Bulk Check Lens Profiles](../xmtp/check-multiple-users.md#bulk-check-lens-profiles-have-xmtp)
* [Lens Followers](broken-reference/)
* [Lens Followings](broken-reference/)
* [Lens Resolver](broken-reference)
* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
* [SocialFollowers API Reference](../../api-references/api-reference/socialfollowers-api.md)
* [SocialFollowings API Reference](../../api-references/api-reference/socialfollowings-api.md)
