---
description: Learn how to use Airstack in your frontend TypeScript/JavaScript application.
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

# 🌐 JavaScript (Browser)

## 🌐 JavaScript (Browser)

In this tutorial, you will learn how to start integrating [Airstack](https://airstack.xyz) API into your frontend TypeScript/JavaScript application.

## Table Of Contents

* [Step 0: Pre-requisites](javascript-browser.md#step-0-pre-requisites)
* [Step 1: Install Airstack Web SDK](javascript-browser.md#step-1-install-airstack-web-sdk)
* [Step 2: Set Environment Variable](javascript-browser.md#step-2-set-environment-variable)
* [Step 3: Initialize SDK](javascript-browser.md#step-3-initialize-sdk)
* [Step 4: Call Your Query](javascript-browser.md#step-4-call-your-query)

### Step 0: Pre-requisites

* Completed [Get API Key](../get-api-key.md)
* Git
* Node v.16+

### Step 1: Install Airstack Web SDK

Use a package manager to install the Airstack Web SDK into your JS/TS project:

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

```sh
AIRSTACK_API_KEY=YOUR_AIRSTACK_API_KEY
```

{% hint style="info" %}
Depending on the JavaScript framework that you are using, you might need to specify a specific naming convention for your environment variables, e.g. vue needs `VUE_APP_AIRSTACK_API_KEY`
{% endhint %}

### Step 3: Initialize SDK

You can use [`init`](../../nodejs-sdk-reference/init.md) from the SDK to initialize it with the [Airstack API key](../get-api-key.md):

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { init } from "@airstack/airstack-react";

init(process.env.AIRSTACK_API_KEY);
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { init } from "@airstack/airstack-react";

init(process.env.AIRSTACK_API_KEY);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 4: Call Your Query

Once you have initialized the SDK, you can use the [`fetchQuery`](../../web-sdk-reference/functions/fetchquery.md) to call the Airstack API.

Below you have been provided with Airstack query to fetch the 0x address, Lens, and Farcaster owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

{% hint style="info" %}
For more query examples, check out [**Guides**](broken-reference/) for various use cases you can build with Airstack.
{% endhint %}

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { fetchQuery } from "@airstack/airstack-react";

interface QueryResponse {
  data: Data;
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

const GET_VITALIK_LENS_FARCASTER_ENS = `
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

const main = async () => {
  const { data, error }: QueryResponse = await fetchQuery(query);

  if (error) {
    throw new Error(error.message);
  }

  console.log(data);
};

main();
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { fetchQuery } from "@airstack/airstack-react";

const GET_VITALIK_LENS_FARCASTER_ENS = `
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

const main = async () => {
  const { data, error } = await fetchQuery(query);

  if (error) {
    throw new Error(error.message);
  }

  console.log(data);
};

main();
```
{% endcode %}
{% endtab %}
{% endtabs %}

The `data` variable will return and logged into your terminal as follows:

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

If you have any questions or need help regarding integrating [Airstack](https://airstack.xyz) into your frontend TS/JS application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

Learn to build more with Airstack using our tutorials:

* [Onchain Graph](../../guides/onchain-graph.md)
* [Resolve Identities](../../guides/resolve-identities/)
* [Combinations](../../guides/combinations/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api/)
* [Web SDK Reference](broken-reference/)
