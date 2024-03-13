---
description: >-
  Learn how to quickly build your first Farcaster Frame using Airstack Frog
  Recipes.
---

# 🚀 Quickstart

## Step 1: Install Dependencies

To get started, simply install `@airstack/frog` and `hono` as dependencies:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/frog hono
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/frog hono
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @airstack/frog hono
```
{% endtab %}

{% tab title="bun" %}
```bash
bun install @airstack/frog hono
```
{% endtab %}
{% endtabs %}

## Step 2: Add Airstack API Key

Get your [Airstack API key](../../get-started/get-api-key.md) and add it as an environment variable:

{% code title=".env" %}
```
AIRSTACK_API_KEY=YOUR_API_KEY
```
{% endcode %}

## Step 3: Build Your 1st Frame

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { config } from "dotenv";

config();

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Some code
```
{% endtab %}
{% endtabs %}

## Step 4: Add Scripts to `package.json`

Add `dev` script to your `package.json` as shown below:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "scripts": { 
<strong>    "dev": "npx @airstack/frog dev",
</strong>  },
}
</code></pre>

## Step 5: Run `dev`

To start the development server, simply run one of the following command to run the `dev` script:&#x20;

{% tabs %}
{% tab title="npm" %}
```bash
npm run dev
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn dev
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm dev
```
{% endtab %}

{% tab title="bun" %}
```bash
bun run dev
```
{% endtab %}
{% endtabs %}

## Next Steps

## Developer Support

If you have any questions or need help regarding integrating onchain data to your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Allow List](allow-list.md)
* [Integrate to Existing Frog Project](integrate-to-existing-frog-project.md)
