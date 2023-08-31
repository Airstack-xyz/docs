---
description: >-
  Learn how to use cursor pagination in the Airstack SDK using special and
  powerful pagination functions and variables offered by the Airstack SDK out of
  the box.
---

# Pagination in Airstack SDK

With the [Airstack](https://airstack.xyz) SDKs, you will not need to manually manage the cursor of each page yourself as they provide **special pagination functions and variables** that can help you simplify the process:

| React/Node    | Python          | Description                                      |
| ------------- | --------------- | ------------------------------------------------ |
| `hasNextPage` | `has_next_page` | Boolean indicating if the next page exists       |
| `hasPrevPage` | `has_prev_page` | Boolean indicating if  the previous page exists  |
| `getNextPage` | `get_next_page` | Get response data and error on the next page     |
| `getPrevPage` | `get_prev_page` | Get response data and error on the previous page |

{% hint style="info" %}
If you are using the SDK for paginating response data, `pageInfo.nextCursor` and `pageInfo.prevCursor` will **not be necessarily** added to your query as it will be added automatically by the SDK.\
\
Thus, you can just provide a query without any of the cursor field in your schema.
{% endhint %}

## Pre-requisites

* [Airstack API key](../../get-started/get-api-key.md)

### Install Airstack SDK

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

### Example

Here is sample implementation of using the **special functions and variables** mentioned above for paginating through all the data returned by the API:

{% tabs %}
{% tab title="React" %}
<pre class="language-jsx"><code class="lang-jsx">import { init, useQueryWithPagination } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `
  query MyQuery {
    TokenBalances(
      input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
    ) {
      TokenBalance {
        owner {
          identity
        }
      }
    }
  }
`;

const Component = () => {
<strong>  const { data, pagination } = useQueryWithPagination(query);
</strong>  const {
    hasNextPage, // Boolean – indicate if there's next page
    hasPrevPage, // Boolean - indicate if there's prev page
    getNextPage,
    getPrevPage
  } = pagination;

  /**
   * @description change cursor to the next page, this will
   * immediately be reflected on `data`
   */
  const handleNextPage = () => {
    getNextPage();
  };

  /**
   * @description change cursor to the prev page, this will
   * immediately be reflected on `data`
   */
  const handlePrevPage = () => {
    getPrevPage();
  };
  
  
  return (
    // your JSX component
  )
}

export default Component;
</code></pre>
{% endtab %}

{% tab title="Node" %}
```javascript
import { init, fetchQueryWithPagination } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `
  query MyQuery {
    TokenBalances(
      input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
    ) {
      TokenBalance {
        owner {
          identity
        }
      }
    }
  }
`;

// counting page indicator
let count = 0;
let response;

const main = async () => {
  while (true) {
    if (!response) {
      response = await fetchQueryWithPagination(query);
    }
    const { data, error, hasNextPage, getNextPage } = response;
    if (!error) {
      /**
       * Add more code to format the `response` data
       */
      console.log(`Data page ${++count}: `, data);
      if (!hasNextPage) {
        break;
      } else {
        response = await getNextPage();
      }
    } else {
      console.error("Error: ", error);
      break;
    }
  }
};

main();
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        identity
      }
    }
  }
}
"""


async def main():
    count = 0
    while True:
        execute_query_client = api_client.create_execute_query_object(
            query=query)
        query_response = await execute_query_client.execute_paginated_query()

        if query_response.error is None:

            count += 1
            print(f"Data page {count}: ", query_response.data)

            if not query_response.has_next_page:
                break
            else:
                query_response = await query_response.get_next_page
        else:
            print("Error: ", query_response.error)
            break

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding how to use cursor pagination with Airstack SDK, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [React SDK Reference](../../react-sdk-reference/overview.md)
  * [`useQueryWithPagination`](../../react-sdk-reference/hooks/usequerywithpagination.md)
  * [`useLazyQueryWithPagination`](../../react-sdk-reference/hooks/uselazyquerywithpagination.md)
* [NodeJS SDK Reference](../../nodejs-sdk-reference/overview.md)
  * [`fetchQueryWithPagination`](../../react-sdk-reference/functions/fetchquerywithpagination.md)
