---
description: >-
  Learn how to integrate the Airstack Frog Recipes into your existing Frog
  project in less than 5 minutes.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🐸 Frog

Integrating the Airstack Frog Recipes into your existing Frog project is very simple and takes less than 5 minutes.&#x20;

## Step 1: Install Airstack Frog Recipes

First, install the Airstack Frog Recipes to your project:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/frog
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/frog
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @airstack/frog
```
{% endtab %}

{% tab title="bun" %}
```bash
bun install @airstack/frog
```
{% endtab %}
{% endtabs %}

## Step 2: Change Imports

Then, simply by change all the `frog` imports to `@airstack/frog`.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
// Initially: import { Frog } from "frog";
import { Frog } from "@airstack/frog";
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Initially: const { Frog } = require("frog");
const { Frog } = require("@airstack/frog");
```
{% endtab %}
{% endtabs %}

The Airstack Frog Recipes is built on top of the Frog framework and contains all the functionality offered by the Frog framework, therefore any functions available in `frog` should also be available in `@airstack/frog`.

## Step 3: Add Airstack API Key

Once all imports are converted, add an [Airstack API key](../../../get-started/get-api-key.md) to your Frog instance's `apiKey`(required) field:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Frog } from "@airstack/frog";

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { Frog } = require("@airstack/frog");

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY,
});
```
{% endtab %}
{% endtabs %}

## Step 4: TypeScript JSX Configuration

Ensure that the `jsxImportSource` in your `tsconfig.json` is as follows:

<pre class="language-json" data-title="tsconfig.json"><code class="lang-json">{
  "compilerOptions": {
    // other options
<strong>    "jsxImportSource": "@airstack/frog/jsx",
</strong>  }
}
</code></pre>

## Step 5: Modify Scripts In `package.json`

Lastly, you can replace `frog dev` with `npx @airstack/frog dev`:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "scripts": { 
<strong>    "dev": "npx @airstack/frog dev",
</strong>  },
}
</code></pre>

:partying\_face: Congratulations! You just integrated Airstack Frog Recipe to your Frog Framework!

## Developer Support

If you have any questions or need help regarding integrating Airstack Frog Recipe to your existing Frog project, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](../airstack-frog-recipes-and-middleware/onchain-data.md)
* [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Allow List](../airstack-frog-recipes-and-middleware/allow-list.md)
