---
description: >-
  Learn how to use Airstack to search for Farcaster users that fulfills the
  given filters or sort variables the API offered.
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

# 🔎 Search Farcaster Users

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get All Farcaster Users Starting With Given Words](search-farcaster-users.md#get-all-farcaster-users-starting-with-given-words)
- [Get All Farcaster Users Containing Given Words](search-farcaster-users.md#get-all-farcaster-users-containing-given-words)
- [Get All Farcaster Users That Has Certain Number of Letters](search-farcaster-users.md#get-all-farcaster-users-that-has-certain-number-of-letters)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
- Basic knowledge of GraphQL

## Get Started

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

## Get All Farcaster Users Starting With Given Words

You can fetch all Farcaster users that starts with given words by providing the regex pattern `"^<given-words>"` to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/mXuUlbGPnI" %}
show me all Farcaster users starting with "a"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Farcaster users
        # starting with "a"
<strong>        profileName: {_regex: "^a"},
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
    }
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
          "dappName": "farcaster",
          "profileName": "atty"
        },
        {
          "dappName": "farcaster",
          "profileName": "anita-mpf"
        },
        {
          "dappName": "farcaster",
          "profileName": "amarraghu"
        }
        // Other Farcaster users starting with "a"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Farcaster Users Containing Given Words

You can fetch all Farcaster users that contains given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/yuolzNgcAR" %}
show me all Farcaster users containing with "abc"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Farcaster users
        # containing "abc"
<strong>        profileName: {_regex: "abc"},
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
    }
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
          "dappName": "farcaster",
          "profileName": "abcabc"
        },
        {
          "dappName": "farcaster",
          "profileName": "krabchinski"
        },
        {
          "dappName": "farcaster",
          "profileName": "861213abcc"
        }
        // Other Farcaster users containing with "abc"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Farcaster Users That Has Certain Number of Letters

You can fetch all Farcaster users that has certain number of letters in its profile name by providing `"^.{min_number_of_letters, max_number_of_letters}$"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API, where the minimum should always be less than or equal to the maximum:

### Try Demo

{% embed url="https://app.airstack.xyz/query/9bEUw4msj1" %}
show me all Farcaster users that has 3 letters or less
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern search all Farcaster users that have 1-3
        # letters in its profile name
<strong>        profileName: {_regex: "^.{1,3}$"}
</strong>        dappName: {_eq: farcaster}
      },
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      profileName
    }
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
          "dappName": "farcaster",
          "profileName": "977"
        },
        {
          "dappName": "farcaster",
          "profileName": "nem"
        },
        {
          "dappName": "farcaster",
          "profileName": "vw"
        }
        // Other Farcaster users with less than 3 letters
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding searching for Farcaster users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Socials API Reference](../../api-references/api-reference/socials-api.md)
- [Resolve Farcaster Users](resolve-farcaster-users.md)
- [Farcaster Users Details](farcaster-users-details.md)
- [Farcaster Followers](farcaster-followers.md)
- [Farcaster Following](farcaster-following.md)
