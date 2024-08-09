---
description: >-
  Learn how to fetch user's Moxie claim details, which includes data such as the
  amount of Moxie that user has claimed, is claimable by user, or still in the
  claiming process.
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

# 🔏 Farcaster Moxie Claim Details

## Table Of Contents

* [Get The Amount Of Moxie Claimable By Certain User](farcaster-moxie-claim-details.md#get-the-amount-of-moxie-claimable-by-certain-user)
* [Get The Amount Of Moxie Claimed By User](farcaster-moxie-claim-details.md#get-the-amount-of-moxie-claimed-by-user)
* [Get The Amount Of Moxie In Process Of Being Claimed By Certain User](farcaster-moxie-claim-details.md#get-the-amount-of-moxie-in-process-of-being-claimed-by-certain-user)

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

## Get The Amount Of Moxie Claimable By Certain User

You can use [`FarcasterMoxieClaimDetails`](../api-references/api-reference/farcastermoxieclaimdetails-api.md) to fetch the amount of Moxie claimable by a certain Farcaster user by providing the FID of the user to the `fid` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/Eqk4qTh5Wo" %}
Show me the amount of Moxie available for claim for FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieClaimDetails(
    input: {
      filter: {
        fid: {
<strong>          _eq: "602" # Specify user's FID here
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    FarcasterMoxieClaimDetails {
      availableClaimAmount
      availableClaimAmountInWei
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieClaimDetails": {
      "FarcasterMoxieClaimDetails": [
        {
          "availableClaimAmount": 30239.014380375822,
          "availableClaimAmountInWei": "30239014380375823000000"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Amount Of Moxie Claimed By User

You can use [`FarcasterMoxieClaimDetails`](../api-references/api-reference/farcastermoxieclaimdetails-api.md) to fetch the amount of Moxie claimed by a certain Farcaster user by providing the FID of the user to the `fid` input filter:

### Try Demo

{% embed url="https://app.airstack.xyz/query/d8fxrTivTH" %}
Show me the amount of Moxie claimed by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieClaimDetails(
    input: {
      filter: {
        fid: {
<strong>          _eq: "602" # Specify user's FID here
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    FarcasterMoxieClaimDetails {
      claimedAmount
      claimedAmountInWei
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieClaimDetails": {
      "FarcasterMoxieClaimDetails": [
        {
          "claimedAmount": 360579.2465510323,
          "claimedAmountInWei": "360579246551032300000000"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Amount Of Moxie In Process Of Being Claimed By Certain User

You can use [`FarcasterMoxieClaimDetails`](../api-references/api-reference/farcastermoxieclaimdetails-api.md) to fetch the amount of Moxie in the process of being claimed by a certain Farcaster user by providing the FID of the user to the `fid` input filter:

{% hint style="info" %}
If you have Moxie stuck in the claimed process for a very long time, feel free to reach out to the Airstack team at [/airstack Warpcast channel](https://warpcast.com/\~/channel/airstack).
{% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/mVXWdMA58f" %}
Show me the amount of Moxie in process of being claimed by FID 602
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  FarcasterMoxieClaimDetails(
    input: {
      filter: {
        fid: {
<strong>          _eq: "602" # Specify user's FID here
</strong>        }
      },
      blockchain: ALL
    }
  ) {
    FarcasterMoxieClaimDetails {
      processingAmount
      processingAmountInWei
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "FarcasterMoxieClaimDetails": {
      "FarcasterMoxieClaimDetails": [
        {
          "processingAmount": 0,
          "processingAmountInWei": "0"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching Moxie claim details of Farcaster users, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [FarcasterMoxieClaimDetails API Reference](../api-references/api-reference/farcastermoxieearningstats.md)
* [Moxie developer docs](https://developer.moxie.xyz)
