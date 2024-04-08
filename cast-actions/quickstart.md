---
description: >-
  Learn how to quickly build your first Farcaster Cast Actions using Airstack
  Frog Recipes in Node.js.
---

# 🚀 Quickstart

In this tutorial, you will learn how to create a simple [upthumbed](https://github.com/horsefacts/upthumbs)-like cast action called **GM**, where with each click you "GM" a person.

You have the option to directly clone the project through [automatic installation](quickstart.md#automatic-installation) or manually follow it step-by-step through [manual installation](quickstart.md#manual-installation).

## Automatic Installation

You can find the Airstack Farcaster Cast Actions Starter Code using Airstack Frog Recipes in our GitHub [here](https://github.com/Airstack-xyz/farcaster-cast-actions-starter).

To clone the repository, run the following command:

{% tabs %}
{% tab title="Git (HTTPS)" %}
```bash
git clone https://github.com/Airstack-xyz/farcaster-cast-actions-starter.git
```
{% endtab %}

{% tab title="Git (SSH)" %}
```bash
git clone git@github.com:Airstack-xyz/farcaster-cast-actions-starter.git
```
{% endtab %}

{% tab title="GitHub CLI" %}
```bash
gh repo clone Airstack-xyz/farcaster-cast-actions-starter
```
{% endtab %}
{% endtabs %}

## Manual Installation

### Step 1: Install Dependencies & Environment Variables

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

Then, get your [Airstack API key](../get-started/get-api-key.md) and add it as an environment variable:

{% code title=".env.local" %}
```bash
AIRSTACK_API_KEY=YOUR_API_KEY
```
{% endcode %}

### Step 2: Build Your 1st Cast Actions

To create your 1st Farcaster cast actions, create a new file `index.tsx` under `api` folder and add the following code to create a new `Frog` instance from `@airstack/frog`:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.tsx" %}
```typescript
import { Frog } from "@airstack/frog";
import { devtools } from "@airstack/frog/dev";
import { serveStatic } from "@airstack/frog/serve-static";
import { handle } from "@airstack/frog/vercel";
import { config } from "dotenv";

config();

export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
  basePath: "/api",
});

devtools(app, { serveStatic });

export const GET = handle(app);
export const POST = handle(app);
```
{% endcode %}
{% endtab %}
{% endtabs %}

Once you have the Frog instance instantiated, define a POST route `/gm` that will be the **post\_url** of your cast actions. For the cast action, you only need to return the `message` text in the 1st response (in JSON format) with the response status code in the 2nd response:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="index.tsx" %}
```typescript
import {
  getFarcasterUserDetails,
  validateFramesMessage,
} from "@airstack/frog";


app.hono.post("/gm", async (c) => {
  const body = await c.req.json();

  // validate the POST body
  const { isValid, message } = await validateFramesMessage(body);
  const interactorFid = message?.data?.fid;
  const castFid = message?.data.frameActionBody.castId?.fid as number;
  if (isValid) {
    // Check if trying to `GM` themselves
    if (interactorFid === castFid) {
      return c.json({ message: "Nice try." }, 400);
    }

    // Fetch user profile name
    const { data, error } = await getFarcasterUserDetails({
      fid: castFid,
    });

    if (error) {
      return c.json({ message: "Error. Try Again." }, 500);
    }

    let message = `GM ${data?.profileName}!`;
    if (message.length > 30) {
      message = "GM!";
    }

    return c.json({ message });
  } else {
    return c.json({ message: "Unauthorized" }, 401);
  }
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Step 3: Configure TSConfig

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

## Run Development Server

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

Then, you can access the cast actions by making POST request to the `http://localhost:5173/api/gm`.

🥳 Congratulations, you've just built your 1st Farcaster Cast Actions using Airstack Frog Recipes!

## Bonus: Deployment & Add Database

To keep track the number of time user **GM** someone, you can add a simple Redis DB to store the number. For this, Vercel provide [Vercel KV](https://vercel.com/docs/storage/vercel-kv/quickstart#create-a-kv-database) that you can easily use in your project. To setup a new KV database, you will first need to setup and deploy your project on Vercel.

To deploy, first add some of the following additional scripts to your `package.json`:

{% code title="package.json" %}
```json
{
  "scripts": { 
    "build": "npx @airstack/frog vercel-build",
    "deploy": "vercel --prod",
    "deploy:preview": "vercel",
  },
}
```
{% endcode %}

Then, compile the poject by running the following command:

{% tabs %}
{% tab title="npm" %}
```bash
npm run build
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn build
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm build
```
{% endtab %}

{% tab title="bun" %}
```bash
bun run build
```
{% endtab %}
{% endtabs %}

If run successfully, you should have a new `.vercel` folder generated. With this you can deploy the compiled build into Vercel by running:&#x20;

{% hint style="info" %}
Before running the command, make sure that you have `vercel` CLI within your machine and have it connected to your account by running `vercel auth`.
{% endhint %}

{% tabs %}
{% tab title="npm" %}
```bash
npm run deploy
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn deploy
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm deploy
```
{% endtab %}

{% tab title="bun" %}
```bash
bun run deploy
```
{% endtab %}
{% endtabs %}

Once deployed successfully, you can go to the Vercel Dashboard to create a new Vercel KV database (follow steps [here](https://vercel.com/docs/storage/vercel-kv/quickstart#create-a-kv-database)) have your [Airstack API key](../get-started/get-api-key.md) added to your environment variables.

| Key                | Value                                                  |
| ------------------ | ------------------------------------------------------ |
| `AIRSTACK_API_KEY` | [Your Airstack API key](../get-started/get-api-key.md) |

Then, access the live Farcaster cast actions from a custom vercel link  with the following format `https://<CAST_ACTIONS_VERCEL_PROJECT>.vercel.app/api/gm`, which you can use to test with the Farcaster official [cast action playground.](https://warpcast.com/\~/developers/cast-actions)

🥳 Congratulations, you've just deployed your 1st Farcaster cast actions built usng Airstack Frog Recipes into Vercel!

## Next Steps

Now that you have your 1st Farcaster cast actions running, you can also learn more on how to use Airstack Frog Recipes to build your 1st Farcaster Frame [here](../guides/airstack-frog-recipes-and-middleware/quickstart/) to company your cast actions.

In addition, you can also check out all the Airstack features available in Airstack Frog Recipes to enrich your Farcaster cast actions or Farcaster Frames with various onchain data offered:

* [Onchain Data](../frames/airstack-frog-recipes-and-middleware/onchain-data.md)
* [Farcaster Hubs](../frames/airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Allow List](../frames/airstack-frog-recipes-and-middleware/allow-list.md)

## Developer Support

If you have any questions or need help regarding integrating onchain data to your Farcaster cast actions using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Farcaster Frames Quickstart](../guides/airstack-frog-recipes-and-middleware/quickstart/)
* [Airstack Frog Recipes & Middleware](../frames/airstack-frog-recipes-and-middleware/)
* [Frog Framework](https://frog.fm)
