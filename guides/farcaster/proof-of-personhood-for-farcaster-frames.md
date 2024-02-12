---
description: >-
  Learn how to build a proof of personhood mechanism to determine user
  authenticity and deter any bot attacks on your Farcaster Frames.
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

# 🎭 Proof of Personhood for Farcaster Frames

You can embed proof of personhood checks to deter any bot attacks or spam on your  [Farcaster Frames](https://warpcast.notion.site/Farcaster-Frames-4bd47fe97dc74a42a48d3a234636d8c5).

Some criteria that you can check on each Farcaster user for proof of personhood are:

* Attended IRL POAP events
* More than X Farcaster Followers

## Table Of Contents

In this guide, you will learn to use [Airstack](https://airstack.xyz) to:

* [Check If Farcaster User Has Any Non-Virtual POAPs](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-any-non-virtual-poaps)
* [Check If Farcaster User Has X or More Followers on Farcaster](proof-of-personhood-for-farcaster-frames.md#check-if-farcaster-user-has-any-token-transfers)

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

### Get Started

**JavaScript/TypeScript/Python**

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
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

## Check If Farcaster User Has Any Non-Virtual POAPs

You can check if the Farcaster user has attended any non-virtual or in-real-life (IRL) POAP events by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Poaps`](../../api-references/api-reference/poaps-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/6Dci00KVsa" %}
Show me all POAPs owned by a Farcaster user and see if they are virtual or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query POAPsOwned($farcasterUser: Identity!) {
  Poaps(
    input: {
      filter: { owner: { _eq: $farcasterUser } }
      blockchain: ALL
      limit: 50
    }
  ) {
    Poap {
      mintOrder
      mintHash
      poapEvent {
        isVirtualEvent
      }
    }
    pageInfo {
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "farcasterUser": "fc_fid:3"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "mintOrder": 740,
          "mintHash": "0xdf9e6af3bab24eaac6e35a0474f97b4373eeff7bfff8fb4c964956860b4975e6",
          "poapEvent": {
<strong>            "isVirtualEvent": false // This event is non-virtual or IRL
</strong>          }
        },
        {
          "mintOrder": 5496,
          "mintHash": "0x8cbd346bbe318483e4c2134de97dc9e93cbc1e8a9774408d8ff20b747d8cd043",
          "poapEvent": {
<strong>            "isVirtualEvent": true // This event is virtual or online
</strong>          }
        },
        // more POAPs held by specified Farcaster user
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Check If Farcaster User Has X or More Followers on Farcaster

You can check if the Farcaster user has X or more Farcaster followers by providing the Farcaster user's `fid` from the [Frame Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) to the `$farcasterUser` variable using the [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/0XTXnQnkK2" %}
Check if Farcaster user has more than or equal to 100 followers on Farcaster
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($farcasterUser: Identity!) {
  Socials(
    input: {
      filter: {
        followerCount: {_gt: 100},
        dappName: {_eq: farcaster},
        identity: {_eq: $farcasterUser}
      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      followerCount
    }
  }
}
```
{% endtab %}

{% tab title="Variables" %}
```json
{
  "farcasterUser": "fc_fid:3"
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Socials": {
      "Social": [
        {
          // Follower number will be shown if above the threshold,
          // in this case above 100. Otherwise, will be shown null.
<strong>          "followerCount": 131001 
</strong>        }
      ]
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding adding proof of personhood to your Farcaster Frame, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Onchain Kit for Farcaster Frames](airstack-onchain-kit-for-farcaster-frames.md)
* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [Poaps API Reference](../../api-references/api-reference/poaps-api.md)
