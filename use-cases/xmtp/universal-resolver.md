---
description: >-
  Learn how to build a universal resolver with Airstack APIs and integrate it
  into your React application.
---

# 🔄 Universal Resolver

## Prerequisites

* [ ] An Airstack account (Free)
* [ ] Node v.16+
* [ ] Basic knowledge of GraphQL

## Step 1: Create a Query Using AI

Airstack provides an AI solution for you to build a GraphQL query efficiently. You can access Airstack AI on the [Explorer page](https://app.airstack.xyz/explorer).

<figure><img src="https://lh5.googleusercontent.com/Hi5cokKJ_oVqjA_QLnjJYma64gSPVHaaiHaF6WmLMBOxbZsQunlSixQ7MH_yfrXfw6H9HPfzZ7z_-wMcLcICmsLQn92gkmTP-aKJl1fRpZ_3XN2zwVPuAJqiw2k7R6oByzouPU4AoxJr3YMFHESMB6A" alt=""><figcaption><p>Airstack AI</p></figcaption></figure>

For example, if you like to get the web3 identity of a user, e.g. all the web3 identities of the _0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045_ address, then you can simply type:

```
For the 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 address, get all web3 identities
```

After clicking enter, the Airstack AI will output a GraphQL query that will return the web3 identities of the given address:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(input: {identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", blockchain: ethereum}) {
    socials {
      dappName
      profileName
    }
  }
}
```
{% endtab %}

{% tab title="Responses" %}
```json
{
  "data": {
    "Wallet": {
      "socials": [
        {
          "dappName": "farcaster",
          "profileName": "vbuterin"
        },
        {
          "dappName": "lens",
          "profileName": "vitalik.lens"
        }
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

## Step 2: Modify Your Query

Now that you have your basic query, you can build more complex queries on top of it to resolve any web3 identities to another. For this, you can simply stay in the [Airstack Explorer ](https://app.airstack.xyz/explorer)to make the modifications.

Since Airstack provides GraphQL API to query blockchain data, it is very easy and developer friendly to see what inputs and outputs are available to you.

<figure><img src="https://lh6.googleusercontent.com/eJkTP4IErLJDKotaW0FR0nf_zZ4YtNqHqe5awonKTRFzQxtQyDBpeFsPQ0sdtsngjOFPxW3KJqOPytc3pzLRXuZEJuLvjtzPw406KnZV7CzZIVtKycDEyLkhVtVHxKF81_dQwa4E1ZFJ0Okl1Vxs0kE" alt=""><figcaption></figcaption></figure>

For example, here you can see on the sidebar of [Airstack Explorer](https://app.airstack.xyz/explorer) from the previous query, the Wallet API has identity as one of the inputs and a variety of response fields to be chosen from.

### Identity

Airstack provides an [Identity API](https://docs.airstack.xyz/airstack-docs-and-faqs/reference/api-reference/airstack-identity-api) out-of-the-box that you can use to filter queries using web3 identities instead of an Ethereum address.

Thus, with Airstack Identity API, you can instead replace the query input with other web3 identities such as ENS:

```graphql
query MyQuery {
  Wallet(input: {identity: "vitalik.eth", blockchain: ethereum}) {
    socials {
      dappName
      profileName
    }
  }
}
```

### Responses

In GraphQL, you have the option to choose which response field to receive. Thus, you will only receive the necessary data for your application. In addition, there will not be any unexpected field returned from your query that can potentially break your application.

To modify the responses, it is also very simple to do from the [Airstack Explorer](https://app.airstack.xyz/explorer). All you need to do is simply tick the field that you want. If a field has its own subfield then you can select the subfield to be received in your responses.

For example, let’s take the query from the Step 1 section above and add domain data to the responses to know whether the user queried has any ENS domain available under its web3 identity. Then, you can check on several additional fields in your responses:

```graphql
{
  domains {
    name
  }
  primaryDomains {
    name
  }
}
```

Here, `domains.name` will provide an array of results of all the ENS names that are held under the given wallet address, while `primaryDomains.name` will provide the primary ENS name that is set to this wallet address.

The query end result will look like this:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(
    input: {identity: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", blockchain: ethereum}
  ) {
    socials {
      dappName
      profileName
    }
    domains {
      name
    }
    primaryDomain {
      name
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
      "socials": [
        {
          "dappName": "farcaster",
          "profileName": "vbuterin"
        },
        {
          "dappName": "lens",
          "profileName": "vitalik.lens"
        }
      ],
      "domains": [
        {
          "name": "quantumexchange.eth"
        },
        {
          "name": "7860000.eth"
        },
        {
          "name": "brianshaw.eth"
        },
        {
          "name": "vbuterin.stateofus.eth"
        },
        {
          "name": "quantumsmartcontracts.eth"
        },
        {
          "name": "openegp.eth"
        },
        {
          "name": "vitalik.cannafam.eth"
        },
        {
          "name": "quantumdapps.eth"
        },
        {
          "name": "vitalik.soy.eth"
        },
        {
          "name": "satoshinart.eth"
        }
      ],
      "primaryDomain": {
        "name": "satoshinart.eth"
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Step 3: Add Variables to Your Query

In production, you will likely need to have the input value change constantly. This is where you can add **variables** to your query to accept input values from your application.

To add variables to your query, simply replace the input with variables, represented by `$` followed by the variable names. For example, an `address` variable would be written as `$variable` in the GraphQL query.

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($address: Identity!) {
    Wallet(input: {identity: $address, blockchain: ethereum}) {
      socials {
        dappName
        profileName
      }
    domains {
      name
    }
    primaryDomain {
      name
    }	
  }
} 
```
{% endtab %}

{% tab title="Variable" %}
```json
{
  "address": "ADDRESS_INPUT"
}
```
{% endtab %}
{% endtabs %}

## Step 4: Create Your React Project (Optional)

First, create an empty React project if you don’t have any. Otherwise, you can skip this part and jump to [getting your API key](universal-resolver.md#step-5-get-your-api-key) below.

First, use Vite to generate a new React project:

```
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

First, login to your Airstack account and go to [Settings](https://app.airstack.xyz/profile-settings/api-keys). Under the _Default Key_, you can find and copy your API key.

Then, create a `.env` file if you don’t have any by running the following command:

```bash
touch .env
```

And, paste your API key to your environment variable `.env` file.

```shell
VITE_AIRSTACK_API_KEY=YOUR_API_KEY
```

## Step 6: Integrate Airstack into Your React App

To use Airstack in your React project, install the Airstack React SDK.

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

Now that all the basic setups are complete, let’s go to `src/App.jsx` file and add Airstack to your React application.

First, let’s delete all the original template contents inside `src/App.jsx` and add new code for Airstack integration.

To use the Airstack React SDK, first, initialize the SDK by adding the following code in `App.jsx`.

```jsx
import { init, useQuery } from "@airstack/airstack-react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);
```

Then, add a basic component with `useQuery` hook added to call the query that you constructed in previous sections to resolve web3 identities:

{% code overflow="wrap" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

function App() {
  const query = `
    query MyQuery($address: Identity!) {
        Wallet(input: {identity: $address, blockchain: ethereum}) {
          socials {
            dappName
            profileName
          }
        domains {
          name
        }
        primaryDomain {
          name
        }	
      }
    } 
  `;
  const variables = {
    address: "vitalik.eth",
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
{% endcode %}

With this simple code, you essentially just integrated Airstack APIs that resolve the `vitalik.eth` identity address input into its Farcaster, Lens Profile, and other ENS names that exist. The app will output the JSON result on the UI and will look like this:

<figure><img src="https://lh5.googleusercontent.com/X5a3X8Yll6epuIVApykJyUk64e0IUmej6Opl40awY-YPs0REEdj5IttHPcfPuTgjHUNS7GmLgxv5WzlcmCzriLR7OeLZpEOZO1srpn3VpDfm7vBOXA2RSnAkXgbCuYqMdb-ag-4wACcGs7f6F9Zo7xg" alt=""><figcaption></figcaption></figure>

At its current state, you have successfully loaded Airstack and its data responses to the UI. However, it is a very bare minimum and the data is loaded still in its JSON stringified format.

As an improvement, let’s add an input box so that your user can input any Ethereum address, Farcaster, Lens Profile, or ENS and a button that the user can click to call the query manually. Once the button is clicked, the app will render the resolved identities in a table format.

To do so, modify the components code as follows:

{% code overflow="wrap" %}
```jsx
import { init, useLazyQuery } from "@airstack/airstack-react";
import { Fragment, useState } from "react";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

function App() {
  const query = `
  query MyQuery($address: Identity!) {
    Wallet(input: {identity: $address, blockchain: ethereum}) {
      socials {
        dappName
        profileName
      }
      domains {
        name
      }
      primaryDomain {
        name
      }  
    }
  }  
  `;

  const [resolveWeb3Identities, { data, loading }] = useLazyQuery(
    query,
    {},
    { cache: false }
  );
  const [identity, setIdentity] = useState("");

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
          resolveWeb3Identities({ address: identity });
        }}
      >
        <input
          value={identity}
          onChange={(e) => {
            e.preventDefault();
            setIdentity(e.target.value);
          }}
          placeholder="Enter Ethereum address, ENS, Lens, or Farcaster"
          style={{
            width: "300px",
            height: "30px",
            borderRadius: "5px",
            padding: "5px",
          }}
        />
        <button type="submit">Resolve</button>
      </form>
      <p>
        {loading ? (
          "Loading..."
        ) : (
          <div
            style={{
              width: "100vw",
              display: "flex",
              flexDirection: "column",
              alignItems: "center",
              justifyContent: "center",
            }}
          >
            <b>Socials</b>
            <table style={{ border: "1px solid white" }}>
              <thead>
                <tr style={{ border: "1px solid white" }}>
                  <th style={{ border: "1px solid white", padding: "5px" }}>
                    Name
                  </th>
                  <th style={{ border: "1px solid white", padding: "5px" }}>
                    Dapp
                  </th>
                </tr>
              </thead>
              <tbody>
                {data?.Wallet?.socials?.map((social, key) => (
                  <Fragment key={key}>
                    <tr style={{ border: "1px solid white" }}>
                      <td style={{ border: "1px solid white", padding: "5px" }}>
                        {social.profileName}
                      </td>
                      <td style={{ border: "1px solid white", padding: "5px" }}>
                        {social.dappName}
                      </td>
                    </tr>
                  </Fragment>
                ))}
              </tbody>
            </table>
            <b>Domains</b>
            <table style={{ border: "1px solid white" }}>
              <thead>
                <tr style={{ border: "1px solid white" }}>
                  <th style={{ border: "1px solid white", padding: "5px" }}>
                    Name
                  </th>
                  <th style={{ border: "1px solid white", padding: "5px" }}>
                    Dapp
                  </th>
                </tr>
              </thead>
              <tbody>
                {data?.Wallet?.domains?.map((domain, key) => (
                  <Fragment key={key}>
                    <tr style={{ border: "1px solid white" }}>
                      <td style={{ border: "1px solid white", padding: "5px" }}>
                        {domain.name}
                      </td>
                      <td style={{ border: "1px solid white", padding: "5px" }}>
                        ENS
                      </td>
                    </tr>
                  </Fragment>
                ))}
              </tbody>
            </table>
          </div>
        )}
      </p>
    </div>
  );
}

export default App;
```
{% endcode %}

With the new UI looking as follows:

<figure><img src="https://lh4.googleusercontent.com/UlCW3dZG7ubKRWitS0Ht1yLa16Wm1j4d3tRReqkACcTup5rSumB6sp4XWVT8kRCBu6u2dtOBHTbXOqzpjHnrPtU0rr8pW92nWnjg-T1EwYh3YpXwjlmBJDyTQBLBTzy3JKFZNjkTf6x1DGJqeufxWw4" alt=""><figcaption></figcaption></figure>

As you can see, aside from having a component that renders the data in a table format, you also added several new aspects.

First, since the app needs to call the API manually instead of every first render, it is essential to replace `useQuery` with `useLazyQuery`, which provides you with an additional function that is named as \`resolveWeb3Identities\` to call the API on each button clicked.

In order to make sure that the `resolveWeb3Identities` is called accordingly to the user input, the input value in the input element is toggled to the `identity` React state such that each time the value in the input box changes the state will be changed.

Thus, when the user clicks the button, `resolveWeb3Identities` will always call the [Airstack](https://www.airstack.xyz/) API with the variables provided by the user input directly and the user will be able to resolve any web3 identities that they provide into the input box.

Congratulations! 🎉You’ve just learned how to use Airstack as a universal web3 resolver and integrate the APIs into your React application.
