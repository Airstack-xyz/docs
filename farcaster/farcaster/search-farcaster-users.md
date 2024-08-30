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

* [Get All Farcaster Users Sorted By Social Capital Scores](search-farcaster-users.md#get-all-farcaster-users-sorted-by-social-capital-scores)
* [Get All Farcaster Users With Social Capital Scores > X](search-farcaster-users.md#get-all-farcaster-users-with-social-capital-scores-greater-than-x)
* [Get All Farcaster Users Sorted By Social Capital Rank](search-farcaster-users.md#get-all-farcaster-users-sorted-by-social-capital-rank)
* [Get All Farcaster Users With Social Capital Rank < X](search-farcaster-users.md#get-all-farcaster-users-with-social-capital-rank-less-than-x)
* [Get All Farcaster Users Starting With Given Words](search-farcaster-users.md#get-all-farcaster-users-starting-with-given-words)
* [Get All Farcaster Users Containing Given Words](search-farcaster-users.md#get-all-farcaster-users-containing-given-words)
* [Get All Farcaster Users That Has Certain Number of Letters](search-farcaster-users.md#get-all-farcaster-users-that-has-certain-number-of-letters)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

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

## Get All Farcaster Users Sorted By Social Capital Scores

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users sorted by [social capital scores](../../social-capital-value-and-social-capital-scores.md) by adding `socialCapitalScore` to the `order` field and set it to `DESC` value to sort in descending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/QAr0y5Iowm" %}
Show me all Farcaster users sorted by social capital scores
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster}
      },
      blockchain: ethereum,
<strong>      order: {socialCapitalScore: DESC}, # Add this to sort by SCS
</strong>      limit: 200
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      farcasterScore {
        farScore
      }
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
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "farcasterScore": {
            "farScore": 279.70459538175
          }
        }
        // Other highly influential user on Farcaster network
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users With Social Capital Scores > X

You can use the `Socials` API to fetch all Farcaster users with [social capital scores](../../social-capital-value-and-social-capital-scores.md) above certain number by using the `socialCapitalScore` input field and provide the **X** value to the `_gt` filter (for other comparators, check out [here](../../api-references/overview/working-with-graphql.md)).

In the example below, **X** is 50:

{% hint style="info" %}
If you would like to also sort the result by social capital score, simply add `socialCapitalScore` to the `order` field and set the value to `DESC`.\
\
To learn more how to do it, click [here](search-farcaster-users.md#get-all-farcaster-users-sorted-by-social-capital-scores).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/hR1DZjUvy3" %}
Show me all Farcaster users with social capital scores of at least 50
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {_eq: farcaster},
<strong>        socialCapitalScore: {_gt: 50} # greater than to 50
</strong>      },
      blockchain: ethereum,
      limit: 200
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      farcasterScore {
        farScore
      }
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
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "farcasterScore": {
            "farScore": 279.70459538175
          }
        }
        // Other Farcaster users w/ SCS > 50
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users Sorted By Social Capital Rank

{% embed url="https://www.youtube.com/embed/PymaxiF9I7k?start=83&end=219" %}
Get Farcaster Users Sorted By Social Capital Rank
{% endembed %}

You can use the [`Socials`](../../api-references/api-reference/socials-api.md) API to fetch all Farcaster users sorted by[ social capital rank](../../social-capital-value-and-social-capital-scores.md) by adding `socialCapitalRank` to the `order` field and set it to `ASC` value to sort in ascending order:

### Try Demo

{% embed url="https://app.airstack.xyz/query/lnGIDqKyQN" %}
Show me all Farcaster users sorted by social capital rank
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      # Order by social capital rank in ascending order
<strong>      order: {socialCapitalRank: ASC},
</strong>      filter: {
        dappName: {
          _eq: farcaster
        }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      farcasterScore {
        farScore
      }
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
          "profileName": "dwr.eth",
          "fid": "3",
          "custodyAddress": "0x6b0bda3f2ffed5efc83fa8c024acff1dd45793f1",
          "connectedAddresses": [
            {
              "address": "0x8fc5d6afe572fefc4ec153587b63ce543f6fa2ea",
              "blockchain": "ethereum"
            },
            {
              "address": "0xd7029bdea1c17493893aafe29aad69ef892b8ff2",
              "blockchain": "ethereum"
            }
          ],
          "farcasterScore": {
            "farRank": 1
          }
        }
        // Other Farcaster users ranked by social capital rank
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Farcaster Users With Social Capital Rank < X

{% embed url="https://www.youtube.com/embed/PymaxiF9I7k?start=340" %}
Get Farcaster Users With Social Capital Rank < X
{% endembed %}

You can use the `Socials` API to fetch all Farcaster users with [social capital rank](../../social-capital-value-and-social-capital-scores.md) below certain value by using the `socialCapitalRank` input field and the `_lte` operator. If you prefer other comparator for your logic, you can use other available comparator that suits you need (check more [here](../../api-references/overview/working-with-graphql.md)).

{% hint style="info" %}
If you would like to also sort the result by social capital rank, simply add `socialCapitalRank` to the `order` field and set the value to `ASC`.\
\
To learn more how to do it, click [here](search-farcaster-users.md#get-all-farcaster-users-sorted-by-social-capital-rank).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/gesg8XwOZA" %}
Show all Farcaster users with social captial rank below or equal to 100
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Socials(
    input: {
      filter: {
        dappName: {
          _eq: farcaster
        },
        socialCapitalRank: {
<strong>          _lte: 100 # less than or equal to 100
</strong>        }
      },
      blockchain: ethereum
    }
  ) {
    Social {
      profileName
      fid: userId
      custodyAddress: userAddress
      connectedAddresses {
        address
        blockchain
      }
      farcasterScore {
        farScore
      }
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
          "profileName": "corbin.eth",
          "fid": "358",
          "custodyAddress": "0x3bcadeaef7f8e3bf5ec9ca5212cc9c72556c43ed",
          "connectedAddresses": [
            {
              "address": "0x869ec00fa1dc112917c781942cc01c68521c415e",
              "blockchain": "ethereum"
            }
          ],
          "farcasterScore": {
            "farRank": 82
          }
        }
        // Other user with social capital rank below or equal to 100
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

If you have any questions or need help regarding searching for Farcaster users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Socials API Reference](../../api-references/api-reference/socials-api.md)
* [Social Capital Value & Social Capital Scores](../../social-capital-value-and-social-capital-scores.md)
* [Resolve Farcaster Users](resolve-farcaster-users.md)
* [Farcaster Users Details](farcaster-users-details.md)
* [Farcaster Followers](farcaster-followers.md)
* [Farcaster Following](farcaster-following.md)
