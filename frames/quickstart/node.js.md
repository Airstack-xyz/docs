---
description: >-
  Learn how to quickly build your first Farcaster Frame using Airstack Frog
  Recipes in Node.js.
---

# 🗼 Node.js

{% embed url="https://drive.google.com/file/d/1kSCk3Y7KtpPXHD7E_xjJbVn2YVKjplDz/view?usp=sharing" %}
Quickstart Demo
{% endembed %}

## Automatic Installation

You can find the Airstack Frames Starter Code using Airstack Frog Recipes in our GitHub [here](https://github.com/Airstack-xyz/airstack-frames-starter/tree/main).

To clone the repository, run the following command:

{% tabs %}
{% tab title="Git (HTTPS)" %}
```bash
git clone https://github.com/Airstack-xyz/airstack-frames-starter.git
```
{% endtab %}

{% tab title="Git (SSH)" %}
```bash
git clone git@github.com:Airstack-xyz/airstack-frames-starter.git
```
{% endtab %}

{% tab title="GitHub CLI" %}
```bash
gh repo clone Airstack-xyz/airstack-frames-starter
```
{% endtab %}
{% endtabs %}

## Manual Installation

### Pre-requisites

* An [Airstack](https://airstack.xyz/) account
* An existing Node.js project

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

Then, get your [Airstack API key](../../get-started/get-api-key.md) and add it as an environment variable:

{% code title=".env" %}
```
AIRSTACK_API_KEY=YOUR_API_KEY
```
{% endcode %}

### Step 2: Build Your 1st Frame

To create your 1st Farcaster Frame, create a new file under `api` folder and add the following code with a new `Frog` instance from `@airstack/frog` instantiated:

{% tabs %}
{% tab title="TypeScript" %}
{% code title="api/index.tsx" %}
```typescript
import { Button, Frog } from "@airstack/frog";
import { devtools } from "@airstack/frog/dev";
import { serveStatic } from "@hono/node-server/serve-static";
import { config } from "dotenv";

config();

// Instantiate new Frog instance with Airstack API key
export const app = new Frog({
  apiKey: process.env.AIRSTACK_API_KEY as string,
  assetsPath: "/",
  basePath: "/api",
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
export const GET = handle(app);
export const POST = handle(app);
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

Then, go to `http://localhost:5173/dev` and you will be redirected to the Frog devtools as shown below:

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-23 at 15.05.15.png" alt=""><figcaption><p>Airstack Frog Devtools</p></figcaption></figure>

🥳 Congratulations, you've just built your 1st Farcaster Frames using Airstack Frog Recipes!

## Bonus: Deployment

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

Once deployed successfully, you can go to the Vercel Dashboard to have your [Airstack API key](../../get-started/get-api-key.md) added to your environment variables.

| Key                | Value                                                     |
| ------------------ | --------------------------------------------------------- |
| `AIRSTACK_API_KEY` | [Your Airstack API key](../../get-started/get-api-key.md) |

Then, access the live Farcaster Frame from a custom vercel link  with the following format `https://<FRAME_VERCEL_PROJECT>.vercel.app/api`, which you can use to test with the Farcaster official validator.

<figure><img src="../../.gitbook/assets/Screenshot 2024-03-26 at 16.33.03.png" alt=""><figcaption><p>Testing Frames in Farcaster's Official Frames Validator</p></figcaption></figure>

🥳 Congratulations, you've just deployed your 1st Farcaster Frames built usng Airstack Frog Recipes into Vercel!

## Next Steps

Now that you have your 1st Farcaster Frame running, learn more about how Frog works in [here](https://frog.fm/concepts/overview).

In addition, you can also check out all the Airstack features available in Airstack Frog Recipes to enrich your Farcaster Frames with various onchain data offered:

* [Onchain Data](../airstack-frog-recipes-and-middleware/onchain-data.md)
* [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Allow List](../airstack-frog-recipes-and-middleware/allow-list.md)

## Developer Support

If you have any questions or need help regarding integrating onchain data to your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Onchain Data](../airstack-frog-recipes-and-middleware/onchain-data.md)
* [Farcaster Hubs](../airstack-frog-recipes-and-middleware/farcaster-hubs.md)
* [Allow List](../airstack-frog-recipes-and-middleware/allow-list.md)
* [Captcha Verification](../airstack-frog-recipes-and-middleware/captcha-verification.md)
* [Airstack Frog Middleware](../airstack-frog-recipes-and-middleware/airstack-frog-middleware.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
