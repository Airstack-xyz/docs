---
description: >-
  Learn how to integrate Airstack APIs to your Node.js application using the
  Airstack Node SDK.
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

# 🗼 Node.js

## 🗼 Node.js

In this tutorial, you will learn how to start integrating [Airstack](https://airstack.xyz) API into your [Node.js](https://nodejs.org/en) application.

## Table Of Contents

* [Step 0: Pre-requisites](node.md#step-0-pre-requisites)
* [Step 1: Install Airstack Node SDK](node.md#step-1-install-airstack-node-sdk)
* [Step 2: Set Environment Variable](node.md#step-2-set-environment-variable)
* [Step 3: Initialize SDK](node.md#step-3-initialize-sdk)
* [Step 4: Call Your Query](node.md#step-4-call-your-query)

### Step 0: Pre-requisites

* Completed [Get API Key](../get-api-key.md)
* Git
* Node v.16+

### Step 1: Install Airstack Node SDK

Use a package manager to install the [Airstack Node SDK](../../nodejs-sdk-reference/overview.md) into your [Node.js](https://nodejs.org/en) project:

{% tabs %}
{% tab title="npm" %}
```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @airstack/node
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

If you are using Node version 20.6.0+, then you can simply import the environment variable to you Node.js app. Thus, directly proceed to the next step.

If you are using Node version earlier than 20.6.0, then you need to install the `dotenv` package:

{% tabs %}
{% tab title="npm" %}
```bash
npm install dotenv
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add dotenv
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install dotenv
```
{% endtab %}
{% endtabs %}

and import the package to be able to inject the environment variable to your application:

{% tabs %}
{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { config } from "dotenv";

config();
```
{% endcode %}
{% endtab %}

{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { config } from "dotenv";

config();
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 3: Initialize SDK

You can use [`init`](../../nodejs-sdk-reference/init.md) from the SDK to initialize it with the [Airstack API key](../get-api-key.md):

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { init } from "@airstack/node";

init(process.env.AIRSTACK_API_KEY);
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { init } from "@airstack/node";

init(process.env.AIRSTACK_API_KEY);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 4: Call Your Query

Once you have initialized the SDK, you can use the [`fetchQuery`](../../web-sdk-reference/functions/fetchquery.md) to call the Airstack API.

Below you have been provided with Airstack query to fetch the 0x address and Farcaster owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { fetchQuery } from "@airstack/node";

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
  dappName: "farcaster";
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
import { fetchQuery } from "@airstack/node";

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
        }
      ],
      "addresses": ["0xd8da6bf26964af9d7eed9e03e53415d37aa96045"]
    }
  }
}
```

### Developer Support

If you have any questions or need help regarding integrating [Airstack](https://airstack.xyz) into your [Node.js ](https://nodejs.org/en)application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

Learn to build more with Airstack using our tutorials:

* [Resolve Identities](../../guides/resolve-identities/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Node SDK Reference](broken-reference)
