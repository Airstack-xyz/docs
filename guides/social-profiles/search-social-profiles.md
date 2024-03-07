---
description: >-
  Learn how to use Airstack to search for social profiles across Lens and
  Farcaster that fulfills the given regex patterns, filters or sort variables
  the API offered.
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

# 🔍 Search Social Profiles

[Airstack](https://airstack.xyz) provides easy-to-use [Socials](../../api-references/api-reference/socials-api.md) API for searching social profiles across Lens and Farcaster easily by using RegEx patterns.

With this, you can build your very own social profile search engine easily and plug it into your application.

For demo on social profile search engine, check out Airstack Explorer [here](https://explorer.airstack.xyz).

<div align="center" data-full-width="true">

<figure><img src="../../.gitbook/assets/Explorer - Regex Social Search - Vitalik Dawufi Betashop - Stops at Explorer.gif" alt=""><figcaption><p>Airstack Explorer Social Profile Search Engine</p></figcaption></figure>

</div>

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get All Social Profiles Starting With Given Words](search-social-profiles.md#get-all-social-profiles-starting-with-given-words)
- [Get All Social Profiles Containing Given Words](search-social-profiles.md#get-all-social-profiles-containing-given-words)
- [Get All Social Profiles That Has Certain Number of Letters](search-social-profiles.md#get-all-social-profiles-that-has-certain-number-of-letters)
- [Get All Lens Profiles Starting With Given Words](search-social-profiles.md#get-all-lens-profiles-starting-with-given-words)
- [Get All Lens Profiles Containing Given Words](search-social-profiles.md#get-all-lens-profiles-containing-given-words)
- [Get All Lens Profiles That Has Certain Number of Letters](search-social-profiles.md#get-all-lens-profiles-that-has-certain-number-of-letters)
- [Get All Farcaster Users Starting With Given Words](search-social-profiles.md#get-all-farcaster-users-starting-with-given-words)
- [Get All Farcaster Users Containing Given Words](search-social-profiles.md#get-all-farcaster-users-containing-given-words)
- [Get All Farcaster Users That Has Certain Number of Letters](search-social-profiles.md#get-all-farcaster-users-that-has-certain-number-of-letters)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account
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

## Get All Social Profiles Starting With Given Words

You can fetch all Lens and Farcaster users that starts with given words by providing an array of regex patterns containing:

- `"^<given-words>"` for Farcaster search
- `"^lens/@<given-words>"` for Lens search

to the <mark style="color:red;">**`_regex_in`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/z7uWnFzcXD" %}
show me all Lens and Farcaster profiles starting with "a"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search Lens &#x26; Farcaster profiles
        # starting with "a"
<strong>        profileName: {_regex_in: ["^a", "^lens/@a"]},
</strong>      },
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
          "dappName": "lens",
          "profileName": "lens/@anastasia1337"
        },
        {
          "dappName": "farcaster",
          "profileName": "anita-mpf"
        }
        // Other Lens & Farcaster profiles starting with "a"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Social Profiles Containing Given Words

You can fetch all Lens and Farcaster users that contains given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/IBrXUm2Rm1" %}
show me all Lens and Farcaster profiles containing with "abc"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search Lens &#x26; Farcaster profiles
        # containing "abc"
<strong>        profileName: {_regex: "abc"}
</strong>      },
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
          "dappName": "lens",
          "profileName": "lens/@abcoathup"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@phabc"
        },
        {
          "dappName": "farcaster",
          "profileName": "abcabc"
        }
        // Other Lens and Farcaster profiles containing "abc"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Social Profiles That Has Certain Number of Letters

You can fetch all Lens and Farcaster users that starts with given words by providing an array of regex patterns containing:

- `"^.{min_number_of_letters, max_number_of_letters}$"` for Farcaster search
- `"^lens/@.{min_number_of_letters, max_number_of_letters}$"` for Lens search

to the <mark style="color:red;">**`_regex_in`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API, where the minimum should always be less than or equal to the maximum:

### Try Demo

{% embed url="https://app.airstack.xyz/query/szgoFIqPfe" %}
Show me all Lens and Farcaster profiles that has 3 letters or less
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern search Lens &#x26; Farcaster profiles
        # that have 1-3 letters in its profile name
<strong>        profileName: {_regex_in: ["^.{1,3}$", "^lens/@.{1,3}$"]}
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
        // Other Lens & Farcaster profiles that has 3 letters or less
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Lens Profiles Starting With Given Words

You can fetch all Lens profiles that starts with given words by providing the regex pattern `"^lens/@<given-words>"` to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/uzlh7IZa4c" %}
show me all Lens profiles starting with "a"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Lens profile
        # starting with "a"
<strong>        profileName: {_regex: "^lens/@a"},
</strong>        dappName: {_eq: lens}
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
          "dappName": "lens",
          "profileName": "lens/@anastasia1337"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@amg888"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@alexdark"
        }
        // Other Lens profiles starting with "a"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Lens Profiles Containing Given Words

You can fetch all Lens profiles that contains given words by providing `"<given-words>"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API:

### Try Demo

{% embed url="https://app.airstack.xyz/query/wBDukQXH3E" %}
show me all Lens profiles containing with "abc"
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern will search all Lens profiles
        # containing "abc"
<strong>        profileName: {_regex: "abc"},
</strong>        dappName: {_eq: lens}
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
          "dappName": "lens",
          "profileName": "lens/@abcoathup"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@phabc"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@abc888"
        }
        // Other Lens profiles containing with "abc"
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get All Lens Profiles That Has Certain Number of Letters

You can fetch all Lens profiles that has certain number of letters in its profile name by providing `"^.{min_number_of_letters, max_number_of_letters}$"` directly to the <mark style="color:red;">**`_regex`**</mark> operator in [`Socials`](../../api-references/api-reference/socials-api.md) API, where the minimum should always be less than or equal to the maximum:

### Try Demo

{% embed url="https://app.airstack.xyz/query/EocCe2Et9c" %}
show me all Lens profiles that has 3 letters or less
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        # This regex pattern search all Lens profiles that have 1-3
        # letters in its profile name
<strong>        profileName: {_regex: "^lens/@.{1,3}$"},
</strong>        dappName: {_eq: lens}
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
          "dappName": "lens",
          "profileName": "lens/@emi"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@dev"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@bak"
        }
        // Other lens profiles with less than 3 letters
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

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

If you have any questions or need help regarding searching for social profiles on Lens and Farcaster, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Socials API Reference](../../api-references/api-reference/socials-api.md)
- [Resolve Lens Profiles](../lens/resolve-lens-profiles.md)
- [Lens Profile Details](../lens/lens-profile-details.md)
- [Lens Followers](../lens/lens-followers.md)
- [Lens Following](../lens/lens-following.md)
- [Resolve Farcaster Users](../farcaster/resolve-farcaster-users.md)
- [Farcaster Users Details](../farcaster/farcaster-users-details.md)
- [Farcaster Followers](../farcaster/farcaster-followers.md)
- [Farcaster Following](../farcaster/farcaster-following.md)
