---
description: >-
  Learn how to fetch profile images of various web3 domains and socials, such as
  ENS, Farcaster, and Lens.
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

# 🖼 Profile Image

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching Web3 applications with profile images from various protocols.

## Table Of Contents

In this guide you will learn how to use Airstack to:

- [Get ENS Profile Image](profile-image.md#get-ens-profile-image) (in various sizes)
- [Get Lens Profile Image](profile-image.md#get-lens-profile-image) (in various sizes)
- [Get Farcaster Profile Image](profile-image.md#get-farcaster-profile-image)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
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

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get ENS Profile Image

You can provide the domain name, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29&inputType=ADDRESS), into the `name` input filter and fetch the ENS profile image from the response with the `tokenNft.contentValue.image` field:

{% hint style="info" %}
The images returned will already be resized by Airstack and can be used directly within your application:

- extra_small: 125x125px
- small: 250x250px
- medium: 500x500px
- large: 750x750px
  {% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/PN6u9k0NQ4" %}
Show me ENS profile image of vitalik.eth
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Domains(
    input: { filter: { name: { _eq: "vitalik.eth" } }, blockchain: ethereum }
  ) {
    Domain {
      tokenNft {
        contentValue {
          image {
            extraSmall
            small
            medium
            large
            original
          }
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
    "Domains": {
      "Domain": [
        {
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3msKuOthSDfoAV6MR5Ue/EiPbgVeU3k+Cvz8sVZWCzXs4SBrIQIAUkye/EmeyMSxWG6wPc0VufPWGCZpP7bR7XGa9jXJgSZ032qABqGFbsvRt6dukJ42iQpcEUa6WVPeM=/extra_small.svg",
                "small": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3msKuOthSDfoAV6MR5Ue/EiPbgVeU3k+Cvz8sVZWCzXs4SBrIQIAUkye/EmeyMSxWG6wPc0VufPWGCZpP7bR7XGa9jXJgSZ032qABqGFbsvRt6dukJ42iQpcEUa6WVPeM=/small.svg",
                "medium": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3msKuOthSDfoAV6MR5Ue/EiPbgVeU3k+Cvz8sVZWCzXs4SBrIQIAUkye/EmeyMSxWG6wPc0VufPWGCZpP7bR7XGa9jXJgSZ032qABqGFbsvRt6dukJ42iQpcEUa6WVPeM=/medium.svg",
                "large": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3msKuOthSDfoAV6MR5Ue/EiPbgVeU3k+Cvz8sVZWCzXs4SBrIQIAUkye/EmeyMSxWG6wPc0VufPWGCZpP7bR7XGa9jXJgSZ032qABqGFbsvRt6dukJ42iQpcEUa6WVPeM=/large.svg",
                "original": "https://assets.airstack.xyz/image/nft/nNBFvZ6wvuIHqDzTFi5pM/pM0Q1IAUgJRNTJrw7f4s3msKuOthSDfoAV6MR5Ue/EiPbgVeU3k+Cvz8sVZWCzXs4SBrIQIAUkye/EmeyMSxWG6wPc0VufPWGCZpP7bR7XGa9jXJgSZ032qABqGFbsvRt6dukJ42iQpcEUa6WVPeM=/original_image.svg"
              }
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

## Get Lens Profile Image

You can provide the Lens handle, e.g. [`lens/@vitalik`](https://explorer.airstack.xyz/token-balances?address=lens%2F%40vitalik&blockchain=ethereum&rawInput=%23%E2%8E%B1lens%2F%40vitalik%E2%8E%B1%28lens%2F%40vitalik++ethereum+null%29&inputType=ADDRESS), into the `profileName` input filter and fetch the Lens profile image from the response with the `profileImage` giving the original profile image and `profileImageContentValue` providing the resized versions:

{% hint style="info" %}
The images returned will already be resized by Airstack and can be used directly within your application:

- extra_small: 125x125px
- small: 250x250px
- medium: 500x500px
- large: 750x750px
  {% endhint %}

### Try Demo

{% embed url="https://app.airstack.xyz/query/FBm7aw5m16" %}
Show me Lens profile image of lens/@vitalik
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { profileName: { _eq: "lens/@vitalik" }, dappName: { _eq: lens } }
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
          "profileImage": "ipfs://QmQP1DyNH8upeBxKJYtfCDdUj3mRcZep8zhJTLe3ePXB7M",
          "profileImageContentValue": {
            "image": {
              "extraSmall": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/extra_small.jpg",
              "large": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/large.jpg",
              "medium": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/medium.jpg",
              "original": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/original_image.jpg",
              "small": "https://assets.airstack.xyz/image/social/WA1TRm9gbDHIiCUF6iXICUfjUq/5gWZ5lBaDpcgYv0Y=/small.jpg"
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

## Get Farcaster Profile Image

You can provide the user's fname, e.g. [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth&blockchain=ethereum&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29&inputType=ADDRESS), into the `profileName` input filter and fetch the Farcaster profile image from the response with the `profileImage` field:

### Try Demo

{% embed url="https://app.airstack.xyz/query/S3zEhemN0K" %}
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
          "profileImage": "https://i.imgur.com/gF9Yaeg.jpg"
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

- [Socials API Reference](../api-references/api-reference/socials-api/)
- [Domains API Reference](../api-references/api-reference/domains-api/)
- [TokenNfts API Reference](../api-references/api-reference/tokennfts-api/)
- [Farcaster Users Details](farcaster/farcaster-users-details.md)
- [Lens Profile Details](lens/lens-profile-details.md)
