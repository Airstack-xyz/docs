---
description: Learn how to use Airstack in your React application.
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

# ⚛ React

## ⚛ React

In this tutorial, you will learn how to start integrating [Airstack](https://airstack.xyz) API into your React application.

This tutorial will only covering React app build with vite and create-react-app. Integration to React metaframeworks, e.g. Next.js, will be covered in other tutorials.

## Table Of Contents

* [Step 0: Pre-requisites](react.md#step-0-pre-requisites)
* [Step 1: Install Airstack Web SDK](react.md#step-1-install-airstack-web-sdk)
* [Step 2: Set Environment Variable](react.md#step-2-set-environment-variable)
* [Step 3: Initialize SDK](react.md#step-3-initialize-sdk)
* [Step 4: Call Your Query](react.md#step-4-call-your-query)

### Step 0: Pre-requisites

* Completed [Get API Key](../get-api-key.md)
* Git
* Node v.16+

### Step 1: Install Airstack Web SDK

Use a package manager to install the [Airstack Web SDK ](broken-reference/)into your React project:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/airstack-react
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/airstack-react
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @airstack/airstack-react
```
{% endtab %}
{% endtabs %}

### Step 2: Set Environment Variable

Create a new `.env` file:

```sh
touch .env
```

Add the [Airstack API key](../get-api-key.md) as the environment variable:

{% tabs %}
{% tab title="vite" %}
```sh
VITE_AIRSTACK_API_KEY=YOUR_AIRSTACK_API_KEY
```
{% endtab %}

{% tab title="create-react-app" %}
```bash
REACT_APP_AIRSTACK_API_KEY=YOUR_AIRSTACK_API_KEY
```
{% endtab %}
{% endtabs %}

### Step 3: Initialize SDK

Add `init` function from the SDK to initialize it with the [Airstack API key](../get-api-key.md):

{% tabs %}
{% tab title="vite (TS)" %}
{% code title="main.tsx" %}
```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { init } from "@airstack/airstack-react";
import Component from "./Component";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Component />
  </React.StrictMode>
);
```
{% endcode %}
{% endtab %}

{% tab title="vite (JS)" %}
{% code title="main.jsx" %}
```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { init } from "@airstack/airstack-react";
import Component from "./Component";

init(import.meta.env.VITE_AIRSTACK_API_KEY);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Component />
  </React.StrictMode>
);
```
{% endcode %}
{% endtab %}

{% tab title="create-react-app (TS)" %}
{% code title="index.tsx" %}
```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { init } from "@airstack/airstack-react";
import Component from "./Component";

init(process.env.REACT_APP_AIRSTACK_API_KEY);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Component />
  </React.StrictMode>
);
```
{% endcode %}
{% endtab %}

{% tab title="create-react-app (JS)" %}
{% code title="index.jsx" %}
```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { init } from "@airstack/airstack-react";
import Component from "./Component";

init(process.env.REACT_APP_AIRSTACK_API_KEY);

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Component />
  </React.StrictMode>
);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 4: Call Your Query

Once you have initialized the SDK, you can use the [`useQuery`](../../web-sdk-reference/hooks/usequery.md) to call the Airstack API.

Below you have been provided with Airstack query to fetch the 0x address, Lens, and Farcaster owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

{% hint style="info" %}
For more query examples, check out [**Guides**](broken-reference/) for various use cases you can build with Airstack.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-tsx" data-title="Component.tsx"><code class="lang-tsx">import { useQuery } from "@airstack/airstack-react";

interface QueryResponse {
  data: Data;
  loading: boolean;
  error: Error;
}

interface Data {
  Wallet: Wallet;
}

interface Error {
  message: string;
}

interface Wallet {
  socials: Social[];
  addresses: string[];
}

interface Social {
  dappName: "lens" | "farcaster";
  profileName: string;
}


const query = `
query MyQuery {
  Wallet(input: {identity: "vitalik.eth", blockchain: ethereum}) {
    socials {
      dappName
      profileName
    }
    addresses
  }
}
`;

const Component = () => {
<strong>  const { data, loading, error }: QueryResponse = useQuery(query, {}, { cache: false });
</strong>
  if (loading) {
    return &#x3C;p>Loading...&#x3C;/p>;
  }

  if (error) {
    return &#x3C;p>Error: {error.message}&#x3C;/p>;
  }

  // Render your component using the data returned by the query
  console.log(data);
}

export default Component;
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
{% code title="Component.jsx" %}
```jsx
import { useQuery } from "@airstack/airstack-react";

const query = `
query MyQuery {
  Wallet(input: {identity: "vitalik.eth", blockchain: ethereum}) {
    socials {
      dappName
      profileName
    }
    addresses
  }
}
`;

const Component = () => {
  const { data, loading, error } = useQuery(query, {}, { cache: false });

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }

  // Render your component using the data returned by the query
  console.log(data);
};

export default Component;
```
{% endcode %}
{% endtab %}
{% endtabs %}

The `data` variable will return and logged into your browser's console as follows:

```json
{
  "data": {
    "Wallet": {
      "socials": [
        {
          "dappName": "farcaster",
          "profileName": "vitalik.eth"
        },
        {
          "dappName": "lens",
          "profileName": "lens/@vitalik"
        }
      ],
      "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
    }
  }
}
```

### Developer Support

If you have any questions or need help regarding integrating [Airstack](https://airstack.xyz) into your React application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

Learn to build more with Airstack using our tutorials:

* [Onchain Graph](../../guides/onchain-graph.md)
* [Resolve Identities](../../guides/resolve-identities/)
* [Combinations](../../guides/combinations/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api/)
* [Web SDK Reference](broken-reference/)
