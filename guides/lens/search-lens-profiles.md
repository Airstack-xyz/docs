---
description: >-
  Learn how to use Airstack to search for Lens profiles that fulfills the given
  filters or sort variables the API offered.
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

# 🔎 Search Lens Profiles

## 🔎 Search Lens Profiles

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Lens](https://lens.xyz) applications and integrating on-chain and off-chain data with [Lens](https://lens.xyz).

## Table Of Contents

In this guide you will learn how to use Airstack to:

* [Get All Lens Profiles](search-lens-profiles.md#get-all-lens-profiles)
* [Get All Lens Profiles Created In Specified Time](search-lens-profiles.md#get-all-lens-profiles-created-in-specified-time)
* [Get The Latest Lens Profiles Created](search-lens-profiles.md#get-the-latest-lens-profiles-created)
* [Get The Earliest Lens Profiles Created](search-lens-profiles.md#get-the-earliest-lens-profiles-created)
* [Get The Most Followed Lens Profiles](search-lens-profiles.md#get-the-most-followed-lens-profiles)
* [Get The Least Followed Lens Profiles](search-lens-profiles.md#get-the-least-followed-lens-profiles)
* [Get Lens Profiles That Is Following Others The Most](search-lens-profiles.md#get-lens-profiles-that-is-following-others-the-most)
* [Get Lens Profiles That Is Following Others The Least](search-lens-profiles.md#get-lens-profiles-that-is-following-others-the-least)

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

### Get All Lens Profiles

You can globally fetch all Lens profiles using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens`:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/atPGpXtsm7" %}
Show me all Lens profiles
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@yanuriyanto3",
          "userAssociatedAddresses": [
            "0x034522aa83ef2cd9a0a34e8c9f9c0e5aa55df88f"
          ]
        },
        {
          "profileName": "lens/@matchagood",
          "userAssociatedAddresses": [
            "0xfabb5411d31283973f6877a5120e138a6c386deb"
          ]
        },
        {
          "profileName": "lens/@996nft",
          "userAssociatedAddresses": [
            "0x40cdc92aa92b0522b6fa3d57117ef9bddd214946"
          ]
        }
        // Other Lens profiles
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjAwNjY3Yzg3NjhhZGI5ZDRhMjkxYTgyNzE4YmVjMWY5MmJkYzZmNTc5YjE2ZDU2YTZkMjIzZjk0NWQ2ZDcwOWIiLCJEYXRhVHlwZSI6InN0cmluZyJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get All Lens Profiles Created In Specified Time

You can fetch all Lens profiles that are created in a specified time using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and `profileCreatedAtBlockTimestamp` to the desired block timestamp range:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/Hl3mxK7tfU" %}
Show me all Lens profiles created between November 12 to 19, 2023 UTC
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        profileCreatedAtBlockTimestamp: {
          _gte: "2023-11-12T00:00:01Z"
          _lte: "2023-11-19T00:00:00Z"
        }
        dappName: { _eq: lens }
      }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      profileCreatedAtBlockNumber
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@sharmila",
          "userAssociatedAddresses": [
            "0x027fe3f132403c1b59ddaba14b576d15865f69c0"
          ],
          "profileCreatedAtBlockNumber": 50090941
        },
        {
          "profileName": "lens/@cryptobatman",
          "userAssociatedAddresses": [
            "0xffadbedd39470d969c483ea031c67e1dd1690be8"
          ],
          "profileCreatedAtBlockNumber": 49896947
        },
        {
          "profileName": "lens/@vaavi",
          "userAssociatedAddresses": [
            "0x78e6d347c3820550cb9bb26aea4b43d4df1b4989"
          ],
          "profileCreatedAtBlockNumber": 49944932
        }
        // Other Lens profiles created between November 12 to 19, 2023 UTC
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjcyN2JjMmNjYmZkOTFhN2EwMzEwMTI4YmUwZWM3MDMzZjNiMDY3M2Y4YmY5NTQxYmNlZDIxZGYyMTIwOTc0ZjMiLCJEYXRhVHlwZSI6InN0cmluZyJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get The Latest Lens Profiles Created

You can fetch all the latest Lens profiles created using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `profileCreatedAtBlockTimestamp` in descending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/RsnIUb5lGD" %}
Show me all the latest Lens profiles created
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { profileCreatedAtBlockTimestamp: DESC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      profileCreatedAtBlockNumber
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@joshkmatt",
          "userAssociatedAddresses": [
            "0xd987d775277dbb992e7d8831422c7ecd769caff6"
          ],
          "profileCreatedAtBlockNumber": 50206929
        },
        {
          "profileName": "lens/@vkuma",
          "userAssociatedAddresses": [
            "0xa8682e95c86d070f40c2f9a9b2b9b3ca8521d5d8"
          ],
          "profileCreatedAtBlockNumber": 50206199
        },
        {
          "profileName": "lens/@digge",
          "userAssociatedAddresses": [
            "0xfa6aa55f25c0cb728273d3ea4c261824821eb7aa"
          ],
          "profileCreatedAtBlockNumber": 50204562
        }
        // Other Lens profiles, ordered by the latest creation block number
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjExOTg4N2U4ZDM4ZWVkOTM0YWM4OTI0ZmMxNDg0NDVhZThmZWNkOWFhMDhmMDg1ZDllMjRhY2ViMzQ0MjNjMTkiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJwcm9maWxlQ3JlYXRlZEF0QmxvY2tUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNzAwMzk1MDY0IiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get The Earliest Lens Profiles Created

You can fetch all the latest Lens profiles created using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `profileCreatedAtBlockTimestamp` in ascending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/fSFKyUc0Yi" %}
Show me all the earliest Lens profiles created
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { profileCreatedAtBlockTimestamp: ASC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      profileCreatedAtBlockNumber
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@lensprotocol",
          "userAssociatedAddresses": [
            "0x05092cf69bdd435f7ba4b8ef97c9caecf2ba69ad"
          ],
          "profileCreatedAtBlockNumber": 28386421
        },
        {
          "profileName": "lens/@aaveaave",
          "userAssociatedAddresses": [
            "0x24a6d858342aaa20f1d058b8961cd3e712c2f859"
          ],
          "profileCreatedAtBlockNumber": 28435177
        },
        {
          "profileName": "lens/@aavegrants",
          "userAssociatedAddresses": [
            "0x396ac17c5e1e45999823c96c5137b56f1623f684"
          ],
          "profileCreatedAtBlockNumber": 28435442
        }
        // Other Lens profiles,
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjljODVjOWQ2ZTU4NTA2MmRlYmNkMmJhYzgyZTlkODlkNzIxY2UwMDdhNGFkYmJlNzQxNDk2YzFkMjA4OTljNDUiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJwcm9maWxlQ3JlYXRlZEF0QmxvY2tUaW1lc3RhbXAiOnsiVmFsdWUiOiIxNjUyODgxMzgwIiwiRGF0YVR5cGUiOiJEYXRlVGltZSJ9fSwiUGFnaW5hdGlvbkRpcmVjdGlvbiI6Ik5FWFQifQ==",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get The Most Followed Lens Profiles

You can fetch all the most followed Lens profiles using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `followerCount` in descending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/dIciLOsP1M" %}
Show me the most followed Lens profiles
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { followerCount: DESC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      followerCount
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@lensprotocol",
          "userAssociatedAddresses": [
            "0x05092cf69bdd435f7ba4b8ef97c9caecf2ba69ad"
          ],
          "followerCount": 86634
        },
        {
          "profileName": "lens/@stani",
          "userAssociatedAddresses": [
            "0x7241dddec3a6af367882eaf9651b87e1c7549dff"
          ],
          "followerCount": 77366
        },
        {
          "profileName": "lens/@aaveaave",
          "userAssociatedAddresses": [
            "0x24a6d858342aaa20f1d058b8961cd3e712c2f859"
          ],
          "followerCount": 73733
        }
        // other most followed lens profiles, sorted by number of followers in descending order
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6Ijk3YzM0YmViZmJiNzcxNmVkYTRkMzJkMWFiZGQwZWVkMGYyMTY2MDFiYjRjZDYyOGQyMjRmNzYwYTE5MmFiM2YiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJmb2xsb3dlckNvdW50Ijp7IlZhbHVlIjoiNDY4OSIsIkRhdGFUeXBlIjoiaW50NjQifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get The Least Followed Lens Profiles

You can fetch all the least followed Lens profiles using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `followerCount` in ascending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/kQMRSpEpSf" %}
Show me the least followed Lens profiles
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { followerCount: ASC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      followerCount
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@wattles",
          "userAssociatedAddresses": [
            "0x7844ef57c6f7dcdd78169ce8983b246005298a76"
          ],
          "followerCount": 0
        },
        {
          "profileName": "lens/@pastalavista",
          "userAssociatedAddresses": [
            "0xb411e2fca5c6cdd9132672b70e7d76b9ea75775c"
          ],
          "followerCount": 0
        },
        {
          "profileName": "lens/@oasisbigbrain",
          "userAssociatedAddresses": [
            "0xc56bd5a8dbd6997686aa0c9e68bd5eeaf1145d86"
          ],
          "followerCount": 0
        }
        // other least followed lens profiles, sorted by number of followers in ascending order
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjBkMzJlYjRhMGM3YmRhMDhlNTBmYTBkNjI1NDJlNDMyNTBjZTVkNjdjMTZmZmRiNGE5MGNkMDhjN2VhZTIxNGEiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJmb2xsb3dlckNvdW50Ijp7IlZhbHVlIjoiIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Lens Profiles That Is Following Others The Most

You can fetch all Lens profiles that is following other profiles the most using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `followingCount` in descending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/MJgKcgk9TV" %}
Show me Lens Profiles that is following others the most
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { followingCount: DESC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      followingCount
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@creatorfundincubator",
          "userAssociatedAddresses": [
            "0x84c0430f3564520dcde45c3dfd7ceb79372e4fa3"
          ],
          "followingCount": 39840
        },
        {
          "profileName": "lens/@ameerna17958863",
          "userAssociatedAddresses": [
            "0x33425c7f31a089ea00441cf46de2487f7899fdd8"
          ],
          "followingCount": 31767
        },
        {
          "profileName": "lens/@justinsunset",
          "userAssociatedAddresses": [
            "0x48b61678ea8748b81abc677d1bd6050878a86d27"
          ],
          "followingCount": 30021
        }
        // Other Lens profiles that is following others the most, sorted by followingCount in descending order
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6Ijc5NzQzMWY1MzIyMDZjZDVjNGJiYzViMWI1YmI1NDM1OTQ0NjQzNzJjZDViNWMxYjBiYjE1NTM0MTRlMTNjZWQiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJmb2xsb3dpbmdDb3VudCI6eyJWYWx1ZSI6IjE4MDQiLCJEYXRhVHlwZSI6ImludDY0In19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get Lens Profiles That Is Following Others The Least

You can fetch all Lens profiles that is following other profiles the least using the [`Socials`](../../api-references/api-reference/socials-api/) API by specifying `dappName` to `lens` and sorting `followingCount` in ascending order:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/noeIhdBx8W" %}
Show me Lens Profiles that is following others the least
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Socials(
    input: {
      filter: { dappName: { _eq: lens } }
      blockchain: ethereum
      limit: 200
      order: { followingCount: ASC }
    }
  ) {
    Social {
      profileName
      userAssociatedAddresses
      followingCount
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
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
          "profileName": "lens/@wattles",
          "userAssociatedAddresses": [
            "0x7844ef57c6f7dcdd78169ce8983b246005298a76"
          ],
          "followingCount": 0
        },
        {
          "profileName": "lens/@pastalavista",
          "userAssociatedAddresses": [
            "0xb411e2fca5c6cdd9132672b70e7d76b9ea75775c"
          ],
          "followingCount": 0
        },
        {
          "profileName": "lens/@oasisbigbrain",
          "userAssociatedAddresses": [
            "0xc56bd5a8dbd6997686aa0c9e68bd5eeaf1145d86"
          ],
          "followingCount": 0
        }
        // Other Lens profiles that is following others the least, sorted by followingCount in ascending order
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjBkNDg0OGI1N2JkZGE2N2RhOWRhYTA2MTk2ZDNhMmM5NTE4NzdiMmI2NzFiNjRiMmMyNjY0YWZiODkzMTYxOWEiLCJEYXRhVHlwZSI6InN0cmluZyJ9LCJmb2xsb3dpbmdDb3VudCI6eyJWYWx1ZSI6IiIsIkRhdGFUeXBlIjoic3RyaW5nIn19LCJQYWdpbmF0aW9uRGlyZWN0aW9uIjoiTkVYVCJ9",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding searching for Lens profiles, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [Socials API Reference](../../api-references/api-reference/socials-api/)
* [Resolve Lens Profiles](resolve-lens-profiles.md)
* [Lens Profile Details](lens-profile-details.md)
* [Lens Followers](lens-followers.md)
* [Lens Following](lens-following.md)
