---
description: >-
  Learn how to quickly build your first Farcaster Frame using Airstack Frog
  Recipes.
---

# 🚀 Quickstart

## Step 1: Install Dependencies

To get started, simply install all the necessary dependencies:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/frog hono dotenv
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/frog hono dotenv
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @airstack/frog hono dotenv
```
{% endtab %}

{% tab title="bun" %}
```bash
bun install @airstack/frog hono dotenv
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

For example, you would like to build a trending mints Frame, where you can recommend Farcaster users which token to mint and enable them to mint it directly from your Farcaster Frame.

In that case, create a new file under `src` folder and add the following code:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="src/index.tsx" %}
```typescript
import {
  Button,
  Frog,
  Audience,
  Criteria,
  TimeFrame,
  createAllowList,
  TokenBlockchain,
  getTrendingMints,
} from "@airstack/frog";
import { config } from "dotenv";

config();

// Instantiate new Frog instance with Airstack API key
export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
});

app.frame("/", async (c) => {
  let trendingMints;
  const { status, frameData } = c;
  const { fid } = frameData ?? {};
  if (status === "response") {
    // Only fetch trending mints in response frame
    const { data } = await getTrendingMints({
      audience: Audience.All,
      criteria: Criteria.UniqueWallets,
      timeFrame: TimeFrame.OneDay,
      limit: 1,
    });
    trendingMints = data?.[0];
  }
  return c.res({
    image: (
      <div style={{ color: 'white', display: 'flex', fontSize: 40 }}>
        {status === "initial"
          ? "Get Trending Mints!"
          : `Most Trending on Base: \n${trendingMints?.address}`}
      </div>
    ),
    intents: [
      status === "response" ? (
        // Display mint button to mint trending mints
        <Button.Mint target={`eip155:8453:${trendingMints?.address}`}>
          Mint
        </Button.Mint>
      ) : (
        <Button>Click Here</Button>
      ),
    ],
  });
});
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="src/index.jsx" %}
```javascript
const {
  Button,
  Frog,
  Audience,
  Criteria,
  TimeFrame,
  createAllowList,
  TokenBlockchain,
  getTrendingMints,
} = require("@airstack/frog");
const { config } = require("dotenv");

config();

// Instantiate new Frog instance with Airstack API key
export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY,
});

app.frame("/", async (c) => {
  let trendingMints;
  const { status, frameData } = c;
  const { fid } = frameData ?? {};
  if (status === "response") {
    // Only fetch trending mints in response frame
    const { data } = await getTrendingMints({
      audience: Audience.All,
      criteria: Criteria.UniqueWallets,
      timeFrame: TimeFrame.OneDay,
      limit: 1,
    });
    trendingMints = data?.[0];
  }
  return c.res({
    image: (
      <div style={{ color: 'white', display: 'flex', fontSize: 40 }}>
        {status === "initial"
          ? "Get Trending Mints!"
          : `Most Trending on Base: \n${trendingMints?.address}`}
      </div>
    ),
    intents: [
      status === "response" ? (
        // Display mint button to mint trending mints
        <Button.Mint target={`eip155:8453:${trendingMints?.address}`}>
          Mint
        </Button.Mint>
      ) : (
        <Button>Click Here</Button>
      ),
    ],
  });
});
```
{% endcode %}
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

Now that you have your 1st Farcaster Frame running, learn more about how Frog works in [here](https://frog.fm/concepts/overview).

In addition, you can also check out all the Airstack features available in Airstack Frog Recipes to enrich your Farcaster Frames with various onchain data offered:

* [Onchain Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Allow List](allow-list.md)

## Developer Support

If you have any questions or need help regarding integrating onchain data to your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Allow List](allow-list.md)
* [Integrate to Existing Frog Project](integrate-to-existing-frog-project.md)
