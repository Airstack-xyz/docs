---
description: Learn how to use Airstack API to fetch Farcaster profile image data.
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

# 🖼️ Farcaster Profile Image

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [Farcaster](https://farcaster.xyz) applications and integrating on-chain and off-chain data with [Farcaster](https://farcaster.xyz).

In this guide you will learn how to use Airstack to [get Farcaster profile image](farcaster-profile-image.md#get-farcaster-profile-image).

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

## Get Farcaster Profile Image

You can provide the user's fname, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29&inputType=ADDRESS), into the `profileName` input filter and fetch the Farcaster profile image from the response with the `profileImage` giving the original profile image and `profileImageContentValue` providing the resized versions:

{% hint style="info" %}
The images returned will already be resized by Airstack and can be used directly within your application:

- extra_small: 125x125px
- small: 250x250px
- medium: 500x500px
- large: 750x750px
  {% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/DGytdRwbjx" %}
Show Farcaster profile image of Farcaster user vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        profileName: { _eq: "vitalik.eth" }
        dappName: { _eq: farcaster }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileImage
      profileImageContentValue {
        image {
          extraSmall
          large
          medium
          original
          small
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
    "Socials": {
      "Social": [
        {
          "profileImage": "https://i.imgur.com/gF9Yaeg.jpg",
          "profileImageContentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/social/t7c6r/zEOQzL8ozyEr1fFmzEH3KbNAYrWqlNPs9+OwU=/extra_small.jpg",
              "large": "https://assets.airstack.xyz/image/social/t7c6r/zEOQzL8ozyEr1fFmzEH3KbNAYrWqlNPs9+OwU=/large.jpg",
              "medium": "https://assets.airstack.xyz/image/social/t7c6r/zEOQzL8ozyEr1fFmzEH3KbNAYrWqlNPs9+OwU=/medium.jpg",
              "original": "https://assets.airstack.xyz/image/social/t7c6r/zEOQzL8ozyEr1fFmzEH3KbNAYrWqlNPs9+OwU=/original_image.jpg",
              "small": "https://assets.airstack.xyz/image/social/t7c6r/zEOQzL8ozyEr1fFmzEH3KbNAYrWqlNPs9+OwU=/small.jpg"
            }
          }
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching profile images data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Socials API Reference](../../api-references/api-reference/socials-api.md)
- [Farcaster Users Details](farcaster-users-details.md)
