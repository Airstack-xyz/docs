---
description: Learn how to use Airstack to check if a single user has XMTP or not.
---

# 🧍 Check Single User

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching [XMTP](https://xmtp.org) applications and integrating on-chain and off-chain data with [XMTP](https://xmtp.org).

In this tutorial, you will learn how to check whether a given user has XMTP enabled or not using various web3 identities, such as 0x address, ENS, cb.id, Lens, and Farcaster.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to check if a single user has XMTP enabled:

* [By 0x address](check-single-user.md#by-0x-address)
* [By ENS](check-single-user.md#by-ens)
* [By cb.id](check-single-user.md#by-cb.id)
* [By Lens profile name or ID](check-single-user.md#by-lens-profile-name-or-id)
* [By Farcaster name or ID](check-single-user.md#by-farcaster-name-or-id)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL
* Basic knowledge of [XMTP](https://xmtp.org)

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
#### React

```sh
npm install @airstack/airstack-react
```

#### Node

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
#### React

```sh
yarn add @airstack/airstack-react
```

#### Node

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
#### React

```sh
pnpm install @airstack/airstack-react
```

#### Node

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack asyncio
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

## By 0x Address

Simply use the following query to check if a user has XMTP or not with this example of a user that **has an XMTP**:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ZT1QLdVkIx" %}
Check Single User Has XMTP With 0x Address (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "0xa91c2d10a993d14f842d23b97f2ab3fdf6b5b9aa"}}}) {
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

Here's an example of user the **does not have any XMTP**:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/6oVZYGDzXG" %}
Check Single User Does Not Have XMTP With 0x Address (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5"}}}) {
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
      "XMTP": null
    }
  }
}
```
{% endtab %}
{% endtabs %}

## By ENS

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input to any ENS names, either on-chain or off-chain (e.g. Namestone[^1]):

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/1n0kL621qf" %}
Check Single User Has XMTP With ENS (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "vitalik.eth"}}}) {
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

## By cb.id

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input to any cb.id:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/jr6QAbmRGb" %}
Show me if yosephks.cb.id has XMTP enabled or not
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "yosephks.cb.id"}}}) {
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

## By Lens Profile Name or ID

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input `owner` to any [Lens profile name](#user-content-fn-2)[^2]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ngQHil80AU" %}
Check Single User Has XMTP With Lens Profile Name (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "vitalik.lens"}}}) {
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

Alternatively, you can also swap the address input `owner` to any Lens profile ID, either in decimal[^3] or hex[^4] form:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/QO9CcBlwQY" %}
Check Single User Has XMTP With Lens Profile ID (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "lens_id:100275"}}}) {
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

## By Farcaster Name or ID

With [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can simply swap the address input `owner` to any [Farcaster Name](#user-content-fn-5)[^5]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/ju22glZstP" %}
Check Single User Has XMTP With Farcaster Name (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "fc_fname:vitalik.eth"}}}) {
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

Alternatively, you can also swap the address input `owner` to any [Farcaster ID](#user-content-fn-6)[^6]:

### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/g2m7nSXVBQ" %}
Check Single User Has XMTP With Farcaster ID (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  XMTPs(input: {blockchain: ALL, filter: {owner: {_eq: "fc_fid:5650"}}}) {
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

## Developer Support

If you have any questions or need help regarding checking XMTP for a single user with various [web3 identities](#user-content-fn-7)[^7], please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [XMTPs API Reference](../../api-references/api-reference/xmtps-api/)
* [Has XMTP For Lens Developers](../lens/has-xmtp.md)
* [Has XMTP For Farcaster Developers](../farcaster/has-xmtp.md)
* [Universal Resolver](../../use-cases/xmtp/universal-resolver.md)

[^1]: ENS that is resolved off-chain, e.g. <mark style="color:red;">`leighton.pooltogether.eth`</mark>             &#x20;

[^2]: e.g. vitalik.lens

[^3]: e.g. `lens_id:100275`

[^4]: e.g. `lens_id:0x19af`

[^5]: e.g. `fc_fname:vitalik.eth`

[^6]: e.g. `fc_fid:5650`

[^7]: 0x address, ENS, Lens, Farcaster
