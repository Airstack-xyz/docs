---
description: >-
  Learn how to use Airstack RegEx Filter to search for POAPs that fulfill the
  given criteria
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

# 🔎 Search POAPs

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [POAP](https://poap.xyz/) applications and for integrating POAP on-chain data indexed directly from both Ethereum and Gnosis.

Using the `_regex` operator in [`PoapEvents`](../../api-references/api-reference/poapevents-api.md) API, now you can search for POAPs easily with any regex script and build your very own POAP search engine into your application.

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Search All POAPs Starting With Given Words](search-poaps.md#search-all-poaps-starting-with-given-words)
* [Search All POAPs In Case Insensitive Manner](search-poaps.md#search-all-poaps-in-case-insensitive-manner)
* [Search All POAPs That Does Not Contain Given Words](search-poaps.md#search-all-poaps-that-does-not-contain-given-words)

Keep in mind that you can combine the regex from each of these use cases into your query to build more complex search feature that suits your app use cases.

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

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

## Search All POAPs Starting With Given Words

You can search all POAP events that start with given word by using the [`PoapEvents`](../../api-references/api-reference/poapevents-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/uZmYxfgiw3" %}
Search all POAPs that starts with ETHGlobal
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  PoapEvents(
    input: {
      filter: {
<strong>        eventName: {_regex: "^ETHGlobal"} # ^ to indicate starts with in regex
</strong>      },
      blockchain: ALL
    }
  ) {
    PoapEvent {
      eventName
      eventId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "PoapEvents": {
      "PoapEvent": [
        {
          "eventName": "ETHGlobal HackMoney2021 Staked Hacker",
          "eventId": "5470"
        },
        {
          "eventName": "ETHGlobal Paris 2023 Speaker",
          "eventId": "146356"
        },
        {
          "eventName": "ETHGlobal Paris 2023 Volunteer",
          "eventId": "146354"
        },
        // Other POAP events that starts with "ETHGlobal"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Search All POAPs In Case Insensitive Manner

You can search all POAP events that contain a given word in a case-insensitive manner by using the [`PoapEvents`](../../api-references/api-reference/poapevents-api.md) API:

{% hint style="info" %}
When doing case-insensitive search, it is **best practice** that you use `(?i)devcon` pattern instead of others (e.g. `[Dd]evcon`).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/tsxSA2mZ3V" %}
Search all POAPs that contain devcon (case insensitive)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  PoapEvents(
    input: {
      filter: {
        # Regex to search all POAP containing 'devcon' in case insensitive manner
<strong>        eventName: {_regex: "(?i)devcon"}
</strong>      },
      blockchain: ALL
    }
  ) {
    PoapEvent {
      eventName
      eventId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "PoapEvents": {
      "PoapEvent": [
        {
          "eventName": "I met sandramaria.eth at Devcon 6",
          "eventId": "73929"
        },
        {
          "eventName": "Devcon VI in Bogotá",
          "eventId": "60695"
        },
        {
          "eventName": "I met 0xmono.eth at Devcon 6",
          "eventId": "75713"
        },
        // Other POAP events that contain devcon (case insensitive)
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Search All POAPs That Does Not Contain Given Words

You can search all POAP events that **DOES NOT** contain a given word in by using the [`PoapEvents`](../../api-references/api-reference/poapevents-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/kZHnQVtAb6" %}
Search all POAPs that does not contain "Devconnect"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  PoapEvents(
    input: {
      filter: {
        # This regex excludes all POAPs that contain "Devconnect" in their name
<strong>        eventName: {_regex: "^(?!Devconnect).*"}
</strong>      },
      blockchain: ALL
    }
  ) {
    PoapEvent {
      eventName
      eventId
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "PoapEvents": {
      "PoapEvent": [
        {
          "eventName": "ByDyorSide Beta Tester",
          "eventId": "149953"
        },
        {
          "eventName": "CH90 MOD Sharing  24th May 2022",
          "eventId": "47455"
        },
        {
          "eventName": "Reunión BIAS / IAS Comms",
          "eventId": "21524"
        },
        // Other POAP events that does not contain "Devconnect"
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding search POAP events using RegEx, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [POAP Balances](poap-balances.md)
* [POAP Holders](poap-holders.md)
* [POAP Events](poap-events.md)
* [POAP Token Gating](token-gating.md)
* [PoapEvents API Reference](../../api-references/api-reference/poapevents-api.md)
