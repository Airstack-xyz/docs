---
description: >-
  Learn how to build a recommendation engine with Airstack APIs and integrate it
  into your React application.
---

# 🤝 Recommendation Engine

You will focus on building a recommendation engine based on addresses that have interacted with the user in **token transfers**.&#x20;

To get all the addresses, you will need the [`TokenTransfers`](broken-reference) API to fetch the historical token transfers from the user’s wallet.

## Prerequisites

* [ ] An Airstack account (Free)
* [ ] Node v.16+
* [ ] Basic knowledge of GraphQL

## Step 1: Create a Query Using AI

In this example, suppose the user is _0xd8da6bf26964af9d7eed9e03e53415d37aa96045_.&#x20;

Therefore, in the prompt, type the following text to get all the token transfers from 0xd8da6bf26964af9d7eed9e03e53415d37aa96045:

```
For 0xd8da6bf26964af9d7eed9e03e53415d37aa96045, get all token transfers
```

The [Airstack AI](broken-reference) will return the following query and response:

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  Wallet(input: {identity: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", blockchain: ethereum}) {
    tokenTransfers {
      id
      chainId
      blockchain
      from {
        identity
        blockchain
      }
      to {
        identity
        blockchain
      }
      type
      tokenAddress
      operator {
        identity
        blockchain
      }
      amount
      formattedAmount
      tokenId
      amounts
      tokenIds
      tokenType
      transactionHash
      blockTimestamp
      blockNumber
      tokenNft {
        id
        address
        tokenId
        blockchain
        chainId
        type
        totalSupply
        tokenURI
        contentType
        contentValue {
          image {
            original
          }
        }
        metaData {
          name
          description
          image
          attributes {
            trait_type
            value
            displayType
            maxValue
          }
          imageData
          backgroundColor
          youtubeUrl
          externalUrl
          animationUrl
        }
        rawMetaData
        lastTransferHash
        lastTransferBlock
        lastTransferTimestamp
      }
      token {
        id
        address
        chainId
        blockchain
        name
        symbol
        type
        totalSupply
        decimals
        logo {
          small
          medium
          large
          original
          external
        }
        contractMetaDataURI
        contractMetaData {
          name
          description
          image
          externalLink
          sellerFeeBasisPoints
          feeRecipient
        }
        rawContractMetaData
        baseURI
        lastTransferTimestamp
        lastTransferBlock
        lastTransferHash
        projectDetails {
          collectionName
          description
          externalUrl
          twitterUrl
          discordUrl
          imageUrl
        }
        tokenTraits
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
    "Wallet": {
      "tokenTransfers": [
        {
          "id": "13a95336fc0a2299660019be07cbc626c4b76d0c767beeba9bb7728a56f44ce0e115",
          "chainId": "1",
          "blockchain": "ethereum",
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
            "blockchain": "ethereum"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
            "blockchain": "ethereum"
          },
          "type": "TRANSFER",
          "tokenAddress": "0x027198c24cf9b973c9713c8b8be849a0f02471b4",
          "operator": {
            "identity": "0x0695586f690cbcd87376f621be6351913ebe6857",
            "blockchain": "ethereum"
          },
          "amount": "10000000000000000000000000",
          "formattedAmount": 5.5555555555555555e+23,
          "tokenId": "10000000000000000000000000",
          "amounts": null,
          "tokenIds": null,
          "tokenType": "ERC20",
          "transactionHash": "3a95336fc0a2299660019be07cbc626c4b76d0c767beeba9bb7728a56f44ce0e",
          "blockTimestamp": "2022-10-18T17:38:47Z",
          "blockNumber": 15776535,
          "tokenNft": null,
          "token": {
            "id": "10x027198c24cf9b973c9713c8b8be849a0f02471b4",
            "address": "0x027198c24cf9b973c9713c8b8be849a0f02471b4",
            "chainId": "1",
            "blockchain": "ethereum",
            "name": "THE Epiphany",
            "symbol": "EPI",
            "type": "ERC20",
            "totalSupply": "100000000000000000000000000",
            "decimals": 18,
            "logo": {
              "small": null,
              "medium": null,
              "large": null,
              "original": null,
              "external": null
            },
            "contractMetaDataURI": "",
            "contractMetaData": {
              "name": null,
              "description": null,
              "image": null,
              "externalLink": null,
              "sellerFeeBasisPoints": null,
              "feeRecipient": null
            },
            "rawContractMetaData": null,
            "baseURI": "",
            "lastTransferTimestamp": "2022-10-18T17:49:47Z",
            "lastTransferBlock": 15776590,
            "lastTransferHash": "4a6ea26de72348eb11bae0e16665b5c199fc4df57b16d434930652ce6e0a6863",
            "projectDetails": {
              "collectionName": null,
              "description": null,
              "externalUrl": null,
              "twitterUrl": null,
              "discordUrl": null,
              "imageUrl": null
            },
            "tokenTraits": null
          }
        },
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Use [Airstack AI's Best Practices](https://docs.airstack.xyz/airstack-docs-and-faqs/\~/changes/RCZ9VCCIc7gOrsTvT2BI/ai-assistant#best-practices) to get the best and most accurate result for your query.
{% endhint %}

These are a lot of data gained from single query, but not every field is needed. Thus, in the next step, you’ll modify the query to trim down some of the fields and only have the one necessary for building the contacts recommendation feature.

## Step 2: Modify Your Query

The fields that are most important for the recommendations engine feature are the `from`(sender) and `to`(receiver) fields

Therefore, you can reduce the query:

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  Wallet(
    input: {identity: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", blockchain: ethereum}
  ) {
    tokenTransfers {
      from {
        identity
      }
      to {
        identity
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
    "Wallet": {
      "tokenTransfers": [
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        },
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        },
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        },
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        },
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        }
      ]
    }
  }
} 
```
{% endtab %}
{% endtabs %}

From this response, either the sender or the receiver will have a different address to _0xd8da6bf26964af9d7eed9e03e53415d37aa96045_ that you can utilize and format as a list of contact recommendations for your user.

Despite getting the necessary fields, this query only fetches token transfers from Ethereum.

Instead of making a multiple requests to index each blockchains, you can use [Airstack](https://www.airstack.xyz/) to construct a [cross-chain query](broken-reference):

{% tabs %}
{% tab title="Query" %}
```graphql
query GetTokenTransfers {
  ethereum: Wallet(input: {identity: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", blockchain: ethereum}) {
    tokenTransfers {
      from {
        identity
      }
      to {
        identity
      }
    }
  }
  polygon: Wallet(input: {identity: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045", blockchain: polygon}) {
    tokenTransfers {
      from {
        identity
      }
      to {
        identity
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
    "ethereum": {
      "tokenTransfers": [
        {
          "from": {
            "identity": "0x1111111254eeb25477b68fb85ed929f73a960582"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        }
      ]
    },
    "polygon": {
      "tokenTransfers": [
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Step 3: Add Variables to Your Query

Adding variables to your constructed [Airstack](https://www.airstack.xyz/) query is very simple.

Simply replace the input with a variable called `$address`:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetTokenTransfers($address: Identity!) {
<strong>  ethereum: Wallet(input: {identity: $address, blockchain: ethereum}) {
</strong>    tokenTransfers {
      from {
        identity
      }
      to {
        identity
      }
    }
  }
<strong>  polygon: Wallet(input: {identity: $address, blockchain: polygon}) {
</strong>    tokenTransfers {
      from {
        identity
      }
      to {
        identity
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "ethereum": {
      "tokenTransfers": [
        {
          "from": {
            "identity": "0x1111111254eeb25477b68fb85ed929f73a960582"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        }
      ]
    },
    "polygon": {
      "tokenTransfers": [
        {
          "from": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          },
          "to": {
            "identity": "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Step 4: Create Your React Project (Optional)

First, create an empty React project if you don’t have any. Otherwise, you can skip this part and jump to [getting your API key](recommendation-engine.md#step-5-get-your-api-key) below.

First, use Vite to generate a new React project:

```sh
npm create vite@latest airstack-demo -- --template react
```

After the process is successfully completed, an **airstack-demo** folder containing the empty React project will be created. Now, you can enter the folder and download some dependencies:

{% tabs %}
{% tab title="npm" %}
```bash
cd airstack-demo && npm install
```
{% endtab %}

{% tab title="yarn" %}
{% code fullWidth="true" %}
```bash
cd airstack-demo && yarn install
```
{% endcode %}
{% endtab %}

{% tab title="pnpm" %}
```bash
cd airstack-demo && pnpm install
```
{% endtab %}
{% endtabs %}

## Step 5: Get Your API Key

First, login to your [Airstack](https://www.airstack.xyz/) account and go to [Settings](https://app.airstack.xyz/profile-settings/api-keys). Under the _Default Key_, you can find and copy your API key.

Then, create a `.env` file if you don’t have any by running the following command:

```bash
touch .env
```

And, paste your API key to your environment variable `.env` file.

```shell
VITE_AIRSTACK_API_KEY=YOUR_API_KEY
```

## Step 6: Integrate Airstack into Your React App

To use [Airstack](https://www.airstack.xyz/) in your React project, install the Airstack React SDK.

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/airstack-react
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/airstack-react
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm add @airstack/airstack-react
```
{% endtab %}
{% endtabs %}

Once the installation is complete, you should see that it has been added to your `package.json` as a dependency and is ready to be imported.

```json
{
  "dependencies": {
    "@airstack/airstack-react": "^0.3.3"
    // other dependencies
  }
  // other config
}
```

Now that all the basic setups are complete, let’s go to `src/App.jsx` file and add [Airstack](https://www.airstack.xyz/) to your React application.

First, let’s delete all the original template contents inside `src/App.jsx` and add new code for [Airstack](https://www.airstack.xyz/) integration.

To use the [Airstack](https://www.airstack.xyz/) React SDK, first, initialize the SDK by adding the following code in `App.jsx`.

```jsx
import { init, useQuery } from "@airstack/airstack-react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);
```

Then, add a basic component with `useQuery` hook added to call the query that you constructed in previous sections to build contact recommendations based on historical token transfers:

```jsx
import { init, useQuery } from "@airstack/airstack-react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

function App() {
  const query = `
  query GetTokenTransfers($address: Identity!) {
    ethereum: Wallet(input: {identity: $address, blockchain: ethereum}) {
      tokenTransfers {
        from {
          identity
        }
        to {
          identity
        }
      }
    }
    polygon: Wallet(input: {identity: $address, blockchain: polygon}) {
      tokenTransfers {
        from {
          identity
        }
        to {
          identity
        }
      }
    }
  }  
  `;
  const variables = {
    address: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
  };

  const { data, loading, error } = useQuery(query, variables, { cache: false });

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }

  return <>{JSON.stringify(data)}</>;
}

export default App;
```

With this simple code, you essentially just integrated Airstack APIs that fetch all the addresses that interacted with the _0xd8da6bf26964af9d7eed9e03e53415d37aa96045_ address in token transfers across both Ethereum and Polygon. The app will output the JSON result on the UI and will look like this:

<figure><img src="https://lh5.googleusercontent.com/515NJ-wt5r7W9Fyhcz4cNxS_Sx4rJeN5MG1hmH-eRWKJfvhJOu2S71XWc5KGQC2MGlQKMt_fSwquW7Cz3LnHMx9sjM08832jaQgu9HLRZ_YZN0Cj_eh9ghjBM7j26IwuGWr8uYO7-qXe3yWIjw94c3s" alt=""><figcaption></figcaption></figure>

At its current state, you have just successfully loaded Airstack and its data responses to the UI. However, it is a very bare minimum and the data is loaded still in its JSON stringified format.

As an improvement, let’s add an input box to provide the user address and some basic \`div\` elements to render the list of recommended contacts.

To do so, modify the components code as follows:

```jsx
import { init, useLazyQuery } from "@airstack/airstack-react";
import { Fragment, useMemo, useState } from "react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

function App() {
  const query = `
  query GetTokenTransfers($address: Identity!) {
    ethereum: Wallet(input: {identity: $address, blockchain: ethereum}) {
      tokenTransfers {
        from {
          identity
        }
        to {
          identity
        }
      }
    }
    polygon: Wallet(input: {identity: $address, blockchain: polygon}) {
      tokenTransfers {
        from {
          identity
        }
        to {
          identity
        }
      }
    }
  }  
  `;

  const [fetchTokenTransfers, { data, loading }] = useLazyQuery(
    query,
    {},
    { cache: false }
  );
  const [identity, setIdentity] = useState("");

  const recommendationsArray = useMemo(() => {
    const { ethereum, polygon } = data || {};
    const { tokenTransfers: ethereumTokenTransfers = [] } = ethereum || {};
    const { tokenTransfers: polygonTokenTransfers = [] } = polygon || {};
    return [...ethereumTokenTransfers, ...polygonTokenTransfers]
      .map((transfer) => {
        if (
          transfer.from.identity !== identity &&
          transfer.from.identity !==
            "0x0000000000000000000000000000000000000000"
        ) {
          return transfer.from.identity;
        } else if (
          transfer.to.identity !== identity &&
          transfer.to.identity !== "0x0000000000000000000000000000000000000000"
        ) {
          return transfer.to.identity;
        } else {
          return;
        }
      })
      .filter(Boolean);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [data]);

  return (
    <div
      style={{
        display: "flex",
        flexDirection: "column",
        alignItems: "center",
        justifyContent: "center",
        width: "100vw",
      }}
    >
      <form
        onSubmit={(e) => {
          e.preventDefault();
          fetchTokenTransfers({ address: identity });
        }}
      >
        <input
          value={identity}
          onChange={(e) => {
            e.preventDefault();
            setIdentity(e.target.value);
          }}
          placeholder="Enter Ethereum address"
          style={{
            width: "300px",
            height: "30px",
            borderRadius: "5px",
            padding: "5px",
          }}
        />
        <button type="submit">Resolve</button>
      </form>
      <h1>Recommendations</h1>
      <p>
        {loading ? (
          "Loading..."
        ) : (
          <Fragment>
            {recommendationsArray?.map((recommendation, key) => (
              <div
                key={key}
                style={{
                  margin: ".5rem",
                  padding: "1rem",
                  border: "1px solid white",
                }}
              >
                {recommendation}
              </div>
            ))}
          </Fragment>
        )}
      </p>
    </div>
  );
}

export default App;
```

The new UI looks as follows:

<figure><img src="https://lh6.googleusercontent.com/Hvs0lzl1hXZHTqpNupsQoWkMalCnxNdDjaBC3Uz_NcLpxMf1E3PztadqtDlUXaFM_7YA_hgX_oqO7I3MiifIYvnnCiyrHQEYcmlj7MN4w_30swlK5StxiVYxnRxR9LLk653h1-4regsmRfY0FTwXWvM" alt=""><figcaption></figcaption></figure>

As you can see, aside from having a component that renders the addresses that can be recommended based on the input address, you also added several new aspects.

First, since the app needs to call the API manually instead of every first render, it is essential to replace `useQuery` with `useLazyQuery`, which provides you with an additional function that is named `fetchTokenTransfers` to call the API on each button clicked.

In order to make sure that the `fetchTokenTransfers` is called accordingly to the user input, the input value in the input element is toggled to the `identity` React state such that each time the value in the input box changes the state will be changed.

Thus, when the user clicks the button, `fetchTokenTransfers` will always call the [Airstack](https://www.airstack.xyz/) API with the variables provided by the user input directly.

Once the `fetchTokenTransfers` is called and the `data` variable is changed to the latest response, the recommendations list will be generated and formatted based on the `data` variable. Here, `recommendationsArray` is going to have `data` as a dependency, and therefore the value will be updated when `data` is updated.

Thus, with the `recommendationsArray` value that contains the list of contact recommendations address, it can be rendered in the component wrapped in a simple `div` element. However, if you’re building for your application, feel free to modify the UI as you’d like.

Congratulations! 🎉You’ve just learned how to use [Airstack](https://www.airstack.xyz/) to build a simple contact recommendation feature for and integrate the APIs into your React application.
