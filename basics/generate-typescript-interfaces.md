---
description: >-
  Learn how to generate your own TypeScript interfaces for your TS-based
  Airstack-powered application.
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

# 💡 Generate TypeScript Interfaces

## Table Of Contents

In this guide you will learn how to:

* [Step 1: Install Packages](generate-typescript-interfaces.md#step-1-install-packages)
* [Step 2: Create `codegen.ts`](generate-typescript-interfaces.md#step-2-create-codegen.ts)
* [Step 3: Modify `package.json` Scripts](generate-typescript-interfaces.md#step-3-modify-package.json-scripts)
* [Step 4: Mark All GraphQL Queries](generate-typescript-interfaces.md#step-4-mark-all-graphql-queries)
* [Step 5: Generate TypeScript Interfaces](generate-typescript-interfaces.md#step-5-generate-typescript-interfaces)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* Basic knowledge of GraphQL

## Step 1: Install Packages

First, you need to install the required packages using your favorite package manager:

{% tabs %}
{% tab title="npm" %}
{% code overflow="wrap" %}
```bash
npm install --save-dev @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @parcel/watcher
```
{% endcode %}
{% endtab %}

{% tab title="yarn" %}
{% code overflow="wrap" %}
```bash
yarn add --dev @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @parcel/watcher
```
{% endcode %}
{% endtab %}

{% tab title="pnpm" %}
{% code overflow="wrap" %}
```bash
pnpm install --dev @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @parcel/watcher
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Step 2: Create `codegen.ts`

Once you have the necessary packages, create a `codegen.ts` file in your project root folder and copy the following configurations:

<pre class="language-typescript" data-title="codegen.ts"><code class="lang-typescript">import type { CodegenConfig } from "@graphql-codegen/cli";

const config: CodegenConfig = {
  overwrite: true,
  schema: "https://api.airstack.xyz/gql",
  documents: "src/**/*.tsx",
  generates: {
    // Output type file
<strong>    "src/graphql/types.ts": {
</strong>    // add to plugins: @graphql-codegen/typescript and @graphql-codegen/typescript-operations
<strong>      plugins: ["typescript", "typescript-operations"],
</strong>      config: {
        avoidOptionals: true,
        skipTypename: true,
      },
    },
  },
};

export default config;
</code></pre>

GraphQL Code Generator relies on a configuration file `codegen.ts` to manage all possible options, input, and output document types.

Here, the types will be compiled and outputted in a single file `src/graphql/types.ts`. You can modify this depending on your project needs.

## Step 3: Modify `package.json` Scripts

Once you have the `codegen.ts` file ready, add the following scripts to your project's `package.json`:

{% code title="package.json" %}
```json
{
  "scripts": {
    "generate": "npx graphql-codegen",
    "prestart": "yarn generate",
    "predev": "yarn generate"
    // Other scripts
  }
  // ...
}
```
{% endcode %}

Here, you'll have 3 new scripts:

<table><thead><tr><th width="156">Scripts</th><th>Description</th></tr></thead><tbody><tr><td><code>generate</code></td><td>Generate TypeScript interfaces of GraphQL queries in the project.</td></tr><tr><td><code>prestart</code></td><td>This script run before building the project. Here it will run the <code>generate</code> script.</td></tr><tr><td><code>predev</code></td><td>This script run before running development mode of the project. Here it will run the <code>generate</code> script.</td></tr></tbody></table>

With these scripts, you're almost ready to generate all the necessary typescript interfaces for your typescript application.

Optionally, you can also run the `generate` script concurrently in **watch mode** with your development script and add it to your `package.json`. In this example, it will be the `dev` script, but some frameworks might use other development scripts such as `start` instead:

```json
{
  "scripts": {
    "dev": "concurrently \"vite\" \"yarn generate --watch\""
    // Other scripts
  }
  // ...
}
```

This way you'll be able to easily modify your query during development without worrying any type changes, as `@graphql-codegen/cli` will handle all the type changes live.

## Step 4: Mark All GraphQL Queries

Now that all is setup, your last step before generating the TypeScript interfaces will be to add `/* GraphQL */` on each of the GraphQL queries you would like to have the types generated:

{% hint style="info" %}
Keep in mind to have the name of each query **UNIQUE** to each other as TypeScript interfaces will be generated based on the query name. Otherwise, the `@graphql-codegen/cli` will throw error.

For example, the query below has name `FetchWeb3Identity`. Therefore, the types generated will be:

* `FetchWeb3IdentityQuery`: response data type interface
* `FetchWeb3IdentityVariables`: variable type interface
{% endhint %}

```typescript
const query = /* GraphQL */ `
  query FetchWeb3Identity {
    Wallet(input: { identity: "vitalik.eth", blockchain: ethereum }) {
      socials {
        dappName
        profileName
      }
      addresses
    }
  }
`;
```

## Step 5: Generate TypeScript Interfaces

Finally, you can run the `generate` scripts to generate the necessary typescript interfaces:

{% tabs %}
{% tab title="npm" %}
```bash
npm run generate
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn generate
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm run generate
```
{% endtab %}
{% endtabs %}

Once successfully completed, you will see a new file `src/graphql/types.ts` generated that contains all the necessary TypeScript interfaces for your GraphQL queries.

From `src/graphql/types.ts`, you can import all the necessary types for your Airstack query:

{% tabs %}
{% tab title="Vanilla (TS)" %}
```typescript
import { fetchQuery } from "@airstack/airstack-react";
import { FetchWeb3IdentityQuery } from "./src/graphql/types";

const { data, loading, error } = fetchQuery<FetchWeb3IdentityQuery>(
  query,
  {},
  { cache: false }
);
```
{% endtab %}

{% tab title="React (TS)" %}
```tsx
import { useQuery } from "@airstack/airstack-react";
import {
  FetchWeb3IdentityQuery,
  FetchWeb3IdentityVariables,
} from "./src/graphql/types";

const { data, loading, error } = useQuery<
  FetchWeb3IdentityQuery,
  FetchWeb3IdentityVariables
>(query, {}, { cache: false });
```
{% endtab %}

{% tab title="Node (TS)" %}
```typescript
import { fetchQuery } from "@airstack/node";
import { FetchWeb3IdentityQuery } from "./src/graphql/types";

interface QueryResponse {
  data: FetchWeb3IdentityQuery | null;
  error: Error | null;
}

interface Error {
  message: string;
}

const { data, error }: QueryResponse = fetchQuery(query, {}, { cache: false });
```
{% endtab %}
{% endtabs %}

Alternatively, if you added **watch** mode to run concurrently with your development or production build script, you can simply run those scripts and the types will be generated upon development or build time.

{% tabs %}
{% tab title="npm" %}
```bash
# For development
npm run dev

# For production build
npm run build
```
{% endtab %}

{% tab title="yarn" %}
```bash
# For development
yarn dev

# For production build
yarn build
```
{% endtab %}

{% tab title="pnpm" %}
```bash
# For development
pnpm run dev

# For production build
pnpm run build
```
{% endtab %}
{% endtabs %}

### Developer Support

If you have any questions or need help regarding generating TypeScript interfaces into your Airstack query, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

### More Resources

* [API Overview](../api-references/overview/)
* [API References](../api-references/api-reference/)
* [Variables](../web-sdk-reference/objects/variables.md)
* [Pagination](pagination-in-airstack-sdk.md)
* [Direct API Call](../quickstart/direct-api-call.md)
* [Multiple Queries Execution](multiple-queries-execution.md)
