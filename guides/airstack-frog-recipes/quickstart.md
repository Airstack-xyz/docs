---
description: >-
  Learn how to quickly build your first Farcaster Frame using Airstack Frog
  Recipes.
---

# 🚀 Quickstart

{% embed url="https://drive.google.com/file/d/1kSCk3Y7KtpPXHD7E_xjJbVn2YVKjplDz/view?usp=sharing" %}
Quickstart Demo
{% endembed %}

## Starter Code

You can find the Airstack Frames Starter Code using Airstack Frog Recipes in our GitHub [here](https://github.com/Airstack-xyz/airstack-frames-starter/tree/main).

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account

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
import { Button, Frog } from "@airstack/frog";
import { devtools } from "@airstack/frog/dev";
import { serveStatic } from "@hono/node-server/serve-static";
import { config } from "dotenv";

config();

// Instantiate new Frog instance with Airstack API key
export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
});

app.frame("/", async (c) => {
  const { status } = c;
  return c.res({
    image: (
      <div
        style={{
          color: "white",
          display: "flex",
          fontSize: 40,
        }}
      >
        {status === "initial" ? "Initial Frame" : "Response Frame"}
      </div>
    ),
    intents: [status === "initial" && <Button>Click Here</Button>],
  });
});

devtools(app, { serveStatic });
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Step 4: Configure TSConfig

Add the following fields into your `tsconfig.json`:

```json
{
  "compilerOptions": {
    // Other configurations
    "jsx": "react-jsx",
    "jsxImportSource": "@airstack/frog/jsx",
  }
}
```

If you don't have `tsconfig.json` in your project, then run the following command first:

```bash
npx tsc --init
```

## Step 5: Run `dev`

Add `dev` script to your `package.json` as shown below:

<pre class="language-json" data-title="package.json"><code class="lang-json">{
  "scripts": { 
<strong>    "dev": "npx @airstack/frog dev",
</strong>  },
}
</code></pre>

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

Then, go to `http://localhost:5173/dev` and you will be redirected to the Frog devtools as shown below:

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-23 at 15.05.15.png" alt=""><figcaption><p>Airstack Frog Devtools</p></figcaption></figure>

🥳 Congratulations, you've just built your 1st Farcaster Frames using Airstack Frog Recipes!

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
