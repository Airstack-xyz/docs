---
description: >-
  Learn how to use Airstack by making direct API call without Airstack SDK. In
  this tutorial, you will learn how to integrate in Node.js. However, this
  should also work in any other tech stacks.
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

# 🎯 Direct API Call

## 🎯 Direct API Call

In this tutorial, you will learn how to start integrating [Airstack](https://airstack.xyz) API by making direct API call without the Airstack SDKs.

While this method is most useful when you already have an app that is using languages not supported by the Airstack SDKs, such as JavaScript/TypeScript and Python, this get starting guide will be using Node.js as an example.

All the concepts here for integrating Airstack GraphQL API should be translatable to any tech stacks you are using.

#### GraphQL API Library

You can call the Airstack API by using a 3rd party GraphQL API library with the following details:

<table><thead><tr><th>Input</th><th width="95">Fields</th><th width="112" data-type="checkbox">Required</th><th>Value</th></tr></thead><tbody><tr><td><code>Authorization</code></td><td>header</td><td>true</td><td><a href="../get-api-key.md">Airstack API Key</a></td></tr><tr><td><code>query</code></td><td>body</td><td>true</td><td>Airstack Queries</td></tr><tr><td><code>variables</code></td><td>body</td><td>false</td><td>Variables to the Queries</td></tr></tbody></table>

In this tutorial, you'll be shown example with [`graphql-request`](https://www.npmjs.com/package/graphql-request) for GraphQL API library implementation.

#### REST API Library

You can also call the Airstack API by using a traditional 3rd party REST API library, such as `node-fetch`, with the following details:

<table><thead><tr><th>Input</th><th width="95">Fields</th><th width="112" data-type="checkbox">Required</th><th>Value</th></tr></thead><tbody><tr><td><code>Content-Type</code></td><td>header</td><td>true</td><td><code>"application/json"</code></td></tr><tr><td><code>Authorization</code></td><td>header</td><td>true</td><td><a href="../get-api-key.md">Airstack API Key</a></td></tr><tr><td><code>query</code></td><td>body</td><td>true</td><td>Airstack Queries</td></tr><tr><td><code>variables</code></td><td>body</td><td>false</td><td>Variables to the Queries</td></tr></tbody></table>

In this tutorial, you'll be shown example with [`node-fetch`](https://www.npmjs.com/package/node-fetch) for GraphQL API library implementation.

## Table Of Contents

* [Step 0: Pre-requisites](direct-api-call.md#step-0-pre-requisites)
* [Step 1: Install 3rd Party Library](direct-api-call.md#step-1-install-3rd-party-library)
* [Step 2: Set Environment Variable](direct-api-call.md#step-2-set-environment-variable)
* [Step 3: Call Your Query](direct-api-call.md#step-3-call-your-query)

### Step 0: Pre-requisites

* Completed [Get API Key](../get-api-key.md)
* Git
* Node v.16+

### Step 1: Install 3rd Party Library

Use a package manager to install the [Airstack Node SDK](broken-reference) into your [Node.js](https://nodejs.org/en) project:

{% tabs %}
{% tab title="graphql-request" %}
**npm**

```sh
npm install graphql-request graphql
```

**yarn**

```bash
yarn add graphql-request graphql
```

**pnpm**

```bash
pnpm install graphql-request graphql
```
{% endtab %}

{% tab title="node-fetch" %}
**npm**

```sh
npm install node-fetch
```

**yarn**

```bash
yarn add node-fetch
```

**pnpm**

```bash
pnpm install node-fetch
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
{% tab title="TypeScript" %}
{% code title="index.ts" %}
```typescript
import { config } from "dotenv";

config();
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="index.js" %}
```javascript
import { config } from "dotenv";

config();
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 3: Call Your Query

Once you have installed one of the 3rd party SDK, you can directly make calls to the Airstack API.

Below you have been provided with Airstack query to fetch the 0x address, Lens, and Farcaster owned by [`vitalik.eth`](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS):

{% hint style="info" %}
For more query examples, check out [**Guides**](broken-reference) for various use cases you can build with Airstack.
{% endhint %}

{% tabs %}
{% tab title="graphql-request (TS)" %}
<pre class="language-typescript"><code class="lang-typescript">import { gql, GraphQLClient } from "graphql-request";

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

const AIRSTACK_API_URL = "https://api.airstack.xyz/graphql";
const AIRSTACK_API_KEY = process.env.AIRSTACK_API_KEY;

const query = gql`
  query MyQuery {
    Wallet(input: { identity: "vitalik.eth", blockchain: ethereum }) {
      socials {
        dappName
        profileName
      }
      addresses
    }
  }
`;

if (!AIRSTACK_API_KEY) throw new Error("AIRSTACK_API_KEY not set");

const graphQLClient = new GraphQLClient(AIRSTACK_API_URL, {
  headers: {
<strong>    Authorization: AIRSTACK_API_KEY, // Add API key to Authorization header
</strong>  },
});

const main = async () => {
  try {
    const data = await graphQLClient.request&#x3C;Data>(query);
    console.log(data);
  } catch (e) {
    throw new Error((e as Error)?.message)
  }
}

main();
</code></pre>
{% endtab %}

{% tab title="graphql-request (JS)" %}
<pre class="language-javascript"><code class="lang-javascript">import { gql, GraphQLClient } from "graphql-request";

const AIRSTACK_API_URL = "https://api.airstack.xyz/graphql";
const AIRSTACK_API_KEY = process.env.AIRSTACK_API_KEY;

const query = gql`
  query MyQuery {
    Wallet(input: { identity: "vitalik.eth", blockchain: ethereum }) {
      socials {
        dappName
        profileName
      }
      addresses
    }
  }
`;

if (!AIRSTACK_API_KEY) throw new Error("AIRSTACK_API_KEY not set");

const graphQLClient = new GraphQLClient(AIRSTACK_API_URL, {
  headers: {
<strong>    Authorization: AIRSTACK_API_KEY, // Add API key to Authorization header
</strong>  },
});

const main = async () => {
  try {
    const data = await graphQLClient.request(query);
    console.log(data);
  } catch (e) {
    throw new Error(e?.message)
  }
}

main();
</code></pre>
{% endtab %}

{% tab title="node-fetch (TS)" %}
<pre class="language-typescript" data-title="index.ts"><code class="lang-typescript">import fetch from "node-fetch";

interface QueryResponse {
  data: Data;
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

const AIRSTACK_API_URL = "https://api.airstack.xyz/graphql";
const AIRSTACK_API_KEY = process.env.AIRSTACK_API_KEY;

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
  try {
    if (!AIRSTACK_API_KEY) throw new Error("AIRSTACK_API_KEY not set");

    const res = await fetch(AIRSTACK_API_URL, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
<strong>        Authorization: AIRSTACK_API_KEY, // Add API key to Authorization header
</strong>      },
      body: JSON.stringify({ query }),
    });
    const json = (await res?.json()) as QueryResponse;
    const data = json?.data;

    console.log(data);
  } catch (e) {
    throw new Error((e as Error)?.message);
  }
};

main();
</code></pre>
{% endtab %}

{% tab title="node-fetch (JS)" %}
<pre class="language-javascript" data-title="index.js"><code class="lang-javascript">import fetch from "node-fetch";

const AIRSTACK_API_URL = "https://api.airstack.xyz/graphql";
const AIRSTACK_API_KEY = process.env.AIRSTACK_API_KEY;

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
  try {
    if (!AIRSTACK_API_KEY) throw new Error("AIRSTACK_API_KEY not set");
    
    const res = await fetch(
      AIRSTACK_API_URL,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
<strong>          Authorization: AIRSTACK_API_KEY // Add API key to Authorization header
</strong>        },
        body: JSON.stringify({ query }),
      }
    );
    const json = await res?.json();
    const data = json?.data;
  
    console.log(data);
  } catch (e) {
    throw new Error(e?.message);
  }
}

main();
</code></pre>
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

If you have any questions or need help regarding integrating [Airstack](https://airstack.xyz) using direct API call into your web3 application, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

Learn to build more with Airstack using our tutorials:

* [Onchain Graph](../../guides/onchain-graph.md)
* [Resolve Identities](../../guides/resolve-identities/)
* [Combinations](../../guides/combinations/)
* [Wallet API Reference](../../api-references/api-reference/wallet-api.md)
* [Node SDK Reference](broken-reference)
