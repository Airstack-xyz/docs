---
description: >-
  Learn how to build a universal resolver with Airstack APIs and integrate it
  into your React application.
---

# 🔄 Universal Resolver

{% embed url="https://www.youtube.com/watch?v=SvkGTU18MLI" %}

## Try Demo

{% embed url="https://universal-resolver.vercel.app/" %}
Universal Resolver Demo
{% endembed %}

## Step 0: Prerequisites

* [ ] Completed [Get API Key](../../get-started/get-api-key.md)
* [ ] Git
* [ ] Node v.16+

## Step 1: Clone Starter Code

To clone the starter code, simply copy the following code to clone in your preferred directory:

```sh
mkdir universal-resolver-starter
cd universal-resolver-starter
git init
git remote add -f origin https://github.com/Airstack-xyz/demos.git
git config core.sparseCheckout true
git sparse-checkout init
git sparse-checkout set universal-resolver-starter
git pull origin main
rm -rf .git
mv xmtp/universal-resolver-starter/* .
mv xmtp/universal-resolver-starter/.* .
rm -rf xmtp
```

You can find all the starter code in [GItHub](https://github.com/Airstack-xyz/demos/tree/main/xmtp/universal-resolver-starter).

## Step 2: Install Dependencies

Install the dependencies using your preferred package manager with one of the following commands:

{% tabs %}
{% tab title="npm" %}
```sh
npm install
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install
```
{% endtab %}
{% endtabs %}

Once the installation is complete, a new `node_modules` will be generated

You should also see that [Airstack React SDK](https://github.com/Airstack-xyz/airstack-web-sdk) has been added to your `package.json` as one of your dependency:

```json
{
  "dependencies": {
    "@airstack/airstack-react": "^0.3.3"
    // other dependencies
  }
  // other config
}
```

## Step 3: Set Environment Variables

Copy and rename `.env.example` to `.env.local`

```bash
cp .env.example .env.local
```

And, paste your [API key](../../get-started/get-api-key.md) to your environment variable `.env.local` file.

```shell
VITE_AIRSTACK_API_KEY=YOUR_API_KEY
```

## Step 4: Run App Locally

To run the app locally, simply run one of the following command:

{% tabs %}
{% tab title="npm" %}
```sh
npm run dev
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn dev
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm run dev
```
{% endtab %}
{% endtabs %}

Then, you can go to [http://localhost:5173](http://localhost:5173):

<figure><img src="../../.gitbook/assets/Screenshot 2023-07-19 at 10.55.46.png" alt=""><figcaption></figcaption></figure>

Here you have the UI all given to you out-of-the-box as part of the starter code.

However, the resolving has not been included as no Airstack query is found in the code.  You can see in `src/graphql/resolve.js` with it's empty query:

```jsx
const UNIVERSAL_RESOLVER = ``;

export default UNIVERSAL_RESOLVER;
```

In order to fill in this, the next steps will be dedicated to constructing your queries for the universal resolver.

## Step 5: Create a Query Using AI

{% hint style="info" %}
Use [Airstack AI's Best Practices](https://docs.airstack.xyz/airstack-docs-and-faqs/\~/changes/RCZ9VCCIc7gOrsTvT2BI/ai-assistant#best-practices) to get the best and most accurate result for your query.
{% endhint %}

[Airstack](https://airstack.xyz) provides an AI solution for you to build a GraphQL query efficiently. You can access [Airstack AI](../../get-started/airstack-ai.md) on the [API Studio](https://app.airstack.xyz/api-studio).

<figure><img src="https://lh5.googleusercontent.com/Hi5cokKJ_oVqjA_QLnjJYma64gSPVHaaiHaF6WmLMBOxbZsQunlSixQ7MH_yfrXfw6H9HPfzZ7z_-wMcLcICmsLQn92gkmTP-aKJl1fRpZ_3XN2zwVPuAJqiw2k7R6oByzouPU4AoxJr3YMFHESMB6A" alt=""><figcaption><p>Airstack AI</p></figcaption></figure>

For example, let's try the following prompt to resolve `vitalik.eth`:

```
For the vitalik.lens, show me the 0x address, web3 socials, and ENS domain
```

After clicking enter, the [Airstack AI](../../get-started/airstack-ai.md) will output a GraphQL query as follows:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(input: {identity: "vitalik.lens", blockchain: ethereum}) {
    addresses
    socials {
      dappName
      profileName
    }
    domains {
      name
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
      "addresses": [
        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
      ],
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
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

You can go to the [API Studio](https://app.airstack.xyz/api-studio) to test out the query yourself:

{% embed url="https://app.airstack.xyz/DTyOZg/Kcz253OnEV" %}
Resolve vitalik.eth Query (Playground)
{% endembed %}

## Step 6: Modify Your Query

Now that you have your basic query, you can build more complex queries on top of it to resolve any web3 identities to another.

For this, you can simply stay in the [API Studio](https://app.airstack.xyz/api-studio)[ ](https://app.airstack.xyz/explorer)to make the modifications.

To build a query that can universally resolve any web3 identities, it needs to satisfy 2 conditions:

### Condition #1: Accept any web3 identity as input

Airstack provides an [Identity API](https://docs.airstack.xyz/airstack-docs-and-faqs/reference/api-reference/airstack-identity-api) out-of-the-box that you can use to filter queries using web3 identities instead of an Ethereum address.

Thus, with [Airstack Identity API](../../api-references/api-reference/airstack-identity-api.md), you can instead replace the query input with other web3 identities such as ENS:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Wallet(input: {identity: "vitalik.eth", blockchain: ethereum}) {
    addresses
    socials {
      dappName
      profileName
    }
    domains {
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
      "addresses": [
        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
      ],
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
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Condition#2: Return all web3 identity as responses

The responses has already included everything that you need to resolve web3 identities. However, it is missing a primary ENS domain to highlight domains that is primary or not.

To add primary ENS domain, you will need this one field:

<table><thead><tr><th width="242">Field</th><th width="110">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>primaryDomains.name</code></td><td><code>string</code></td><td>primary ENS name</td></tr></tbody></table>

Adding the additional field, the last query with the changes highlighted will be as follows:

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query MyQuery {
  Wallet(
    input: {identity: "vitalik.lens", blockchain: ethereum}
  ) {
    addresses
    socials {
      dappName
      profileName
    }
    domains {
      name
    }
<strong>    primaryDomain {
</strong><strong>      name
</strong><strong>    }
</strong>  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Wallet": {
      "addresses": [
        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
      ],
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

## Step 7: Add Variables to Your Query

In production, you will likely need to have the input value change constantly. This is where you can add **variables** to your query to accept input values from your application.

Simply replace the input with variables, represented by `$` followed by the variable names:

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery($address: Identity!) {
  Wallet(input: {identity: $address, blockchain: ethereum}) {
    addresses
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
  "address": "vitalik.lens"
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Wallet": {
      "addresses": [
        "0xd8da6bf26964af9d7eed9e03e53415d37aa96045"
      ],
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

## Step 8: Call The Airstack Query

Now that you have the query constructed, all you need is just paste it directly into the codebase.

Simply go to `src/graphql/resolve.js` again and paste the query to `UNIVERSAL_RESOLVER` variable:

{% code overflow="wrap" %}
```jsx
const UNIVERSAL_RESOLVER = `
query MyQuery($address: Identity!) {
  Wallet(input: {identity: $address, blockchain: ethereum}) {
    addresses
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

export default UNIVERSAL_RESOLVER;
```
{% endcode %}

To call this query, use the `useQuery` hook from the Airstack React SDK to `src/App.jsx`:

<pre class="language-jsx"><code class="lang-jsx"><strong>import { useQuery } from "@airstack/airstack-react";
</strong><strong>import UNIVERSAL_RESOLVER from "./graphql/resolve";
</strong>import UniversalResolver from "./components/UniversalResolver";

function App() {
<strong>  const { data, loading } = useQuery(
</strong><strong>    UNIVERSAL_RESOLVER,
</strong><strong>    { address: "vitalik.lens" },
</strong><strong>    { cache: false }
</strong><strong>  );
</strong>
  return (
    &#x3C;UniversalResolver
<strong>      data={data}
</strong><strong>      loading={loading}
</strong>    />
  );
}

export default App;
</code></pre>

With this simple code, you essentially just integrated Airstack query that you constructed for universally resolving web3 identities.&#x20;

Since the code now hardcode `vitalik.lens` as the query input, the app will universally resolve `vitalik.lens` and have the UI look like this:

<figure><img src="../../.gitbook/assets/Screenshot 2023-07-19 at 11.11.21.png" alt=""><figcaption></figcaption></figure>

## Step 9: Toggle API Query to Button Click

At its current state, you have successfully loaded [Airstack](https://airstack.xyz) and its data responses to the UI.

However, the query call is not toggled to the button click and is not dependent on the user input.

This is because you only use `useQuery` which automatically calls the API for every render. To do a manual call, swap `useQuery` with `useLazyQuery` in `src/App.jsx` as follows:

<pre class="language-jsx" data-overflow="wrap"><code class="lang-jsx"><strong>import { useLazyQuery } from "@airstack/airstack-react";
</strong>import UNIVERSAL_RESOLVER from "./graphql/resolve";
import UniversalResolver from "./components/UniversalResolver";

function App() {
<strong>  const [resolveIdentity, { data, loading }] = useLazyQuery(
</strong>    UNIVERSAL_RESOLVER,
<strong>    {},
</strong>    { cache: false }
  );

  return (
    &#x3C;UniversalResolver
      data={data}
      loading={loading}
<strong>      onButtonClick={resolveIdentity}
</strong>    />
  );
}

export default App;
</code></pre>

The end result is as follows:

<figure><img src="../../.gitbook/assets/Screen Recording 2023-07-19 at 11.29.03.gif" alt=""><figcaption><p>Universal Resolver (Final)</p></figcaption></figure>

Congratulations! 🎉You’ve just learned how to use [Airstack](https://airstack.xyz) to build a universal resolver and integrate Airstack API into your React application.

## Developer Support

If you have any questions or need help regarding universal resolver, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Resolve ENS](../../guides/resolve-identities/ens.md)
* [Resolve Lens](../../guides/resolve-identities/lens.md)
* [Resolve Farcaster](../../guides/resolve-identities/farcaster.md)
* [Wallet API Reference](../../api-references/api-reference/wallet-api/)
* [Universal Resolver Final Code](https://github.com/Airstack-xyz/demos/tree/main/xmtp/universal-resolver-final)
