---
description: >-
  Learn how to use Airstack to get token-bound account results sorted in
  ascending or descending order by the creation block timestamp.
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

# ⏫ Sort Results

[Airstack](https://airstack.xyz) provides easy-to-use APIs for enriching ERC6551 dapps and integrating on-chain and off-chain data.

In this tutorial, you will learn how to sort an array of global ERC6551 accounts deployed, either in ascending or descending order by the creation block timestamp. This is particularly useful for fetching the latest or earliest token-bound accounts created.

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

- [Get The Latest Token Bound Accounts Created](sort-results.md#get-the-latest-token-bound-accounts-created)
- [Get The Earliest Token Bound Accounts Created](sort-results.md#get-the-earliest-token-bound-accounts-created)

## Pre-requisites

- An [Airstack](https://airstack.xyz/) account (free)
- Basic knowledge of GraphQL
- Basic knowledge of [ERC6551](https://eips.ethereum.org/EIPS/eip-6551)

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

## Get The Latest Token Bound Accounts Created

You can get the all the latest token-bound accounts created in descending order by `createdAtBlockTimestamp` by setting the value to enum `DESC`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/TvC0S5uOBi" %}
Get The Latest Token Bound Accounts Created (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      order: { createdAtBlockTimestamp: DESC }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
        blockchain
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
      updatedAtBlockNumber
      updatedAtBlockTimestamp
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Accounts": {
      "Account": [
        {
          "id": "a7ed516e2a9c49f1f18a7a62adabf08181f6ca6b1d2dc35a890df257ebeb02ff",
          "standard": "ERC6551",
          "blockchain": "ethereum",
          "tokenAddress": "0x51e613727fdd2e0b91b51c3e5427e9440a7957e4",
          "tokenId": "13015035",
          "address": {
            "identity": "0x0d6eb86ed9231e4ff679367c24480e79e452834d",
            "blockchain": "ethereum"
          },
          "registry": "0x02101dfb77fde026414827fdc604ddaf224f0921",
          "implementation": "0x2d25602551487c3f3354dd80d76d54383a243358",
          "salt": "0",
          "createdAtBlockNumber": 17681775,
          "createdAtBlockTimestamp": "2023-07-13T02:57:59Z",
          "creationTransactionHash": "0xe017bbb147e3e6c345e41920aafd0cf5304863d93f606f2d192679f0299ca242",
          "deployer": "0x90bade35da052450b01e99b38cbb550bc3f1dd58",
          "updatedAtBlockNumber": 17681775,
          "updatedAtBlockTimestamp": "2023-07-13T02:57:59Z"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Get The Earliest Token Bound Accounts Created

You can get the all the earliest token-bound accounts created in descending order by `createdAtBlockTimestamp` by setting the value to enum `ASC`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/eb5iRohtrn" %}
Get The Earliest Token Bound Accounts Created (Demo)
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}

```graphql
query MyQuery {
  Accounts(
    input: {
      order: { createdAtBlockTimestamp: ASC }
      blockchain: ethereum
      limit: 200
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
        blockchain
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
      updatedAtBlockNumber
      updatedAtBlockTimestamp
    }
  }
}
```

{% endtab %}

{% tab title="Response" %}

```json
{
  "data": {
    "Accounts": {
      "Account": [
        {
          "id": "37130b5894303a6d57d28bed00daa5583acc9fd0e2e475be825f65af765b76e4",
          "standard": "ERC6551",
          "blockchain": "ethereum",
          "tokenAddress": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
          "tokenId": "0",
          "address": {
            "identity": "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5",
            "blockchain": "ethereum"
          },
          "registry": "0x02101dfb77fde026414827fdc604ddaf224f0921",
          "implementation": "0x2d25602551487c3f3354dd80d76d54383a243358",
          "salt": "0",
          "createdAtBlockNumber": 17213826,
          "createdAtBlockTimestamp": "2023-05-08T05:53:59Z",
          "creationTransactionHash": "0xf998cb400eebe89aa1d369792daf4c202be74dcd88981b34eb49d510b7837332",
          "deployer": "0xa75b7833c78eba62f1c5389f811ef3a7364d44de",
          "updatedAtBlockNumber": 17213826,
          "updatedAtBlockTimestamp": "2023-05-08T05:53:59Z"
        }
      ]
    }
  }
}
```

{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding sorting ERC6551 token bound accounts data by creation block timestamp, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

- [Accounts API Reference](../../api-references/api-reference/accounts-api/)
