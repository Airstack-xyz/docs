---
description: >-
  Learn how to use the Allow List Middleware to check whether a user is allowed
  to use your Frames.js Frames based on their onchain data.
---

# 🎭 Allow List

## Get Started

First, install the Airstack Frames SDK:

{% tabs %}
{% tab title="npm" %}
```bash
npm install @airstack/frames
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add @airstack/frames
```
{% endtab %}

{% tab title="pnpm" %}
```bash
pnpm install @airstack/frames
```
{% endtab %}

{% tab title="bun" %}
```bash
bun install @airstack/frames
```
{% endtab %}
{% endtabs %}

## Check User's Farcaster Followers Count

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user has certain number of followers by using the `NUMBER_OF_FARCASTER_FOLLOWERS` and specifying the criteria using one of the [comparison operator](../../api-references/overview/working-with-graphql.md#comparison-operators) offered by Airstack:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
      allowList({
        apiKey: process.env.AIRSTACK_API_KEY as string,
        criteria: {
          or: [
            // Only allow those with greater than 100 followers
<strong>            [AllowListCriteria.NUMBER_OF_FARCASTER_FOLLOWERS, { _gte: 100 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow those with greater than 100 followers
<strong>            [AllowListCriteria.NUMBER_OF_FARCASTER_FOLLOWERS, { _gte: 100 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Check If User Is Followed By Certain Farcaster User

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user is followed by a certain Farcaster user by using the `FARCASTER_FOLLOWED_BY` and specifying the `fid` of that certain Farcaster user:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY as string,
        criteria: {
          or: [
            // Only allow those who are followed by fid 1
<strong>            [AllowListCriteria.FARCASTER_FOLLOWED_BY, { fid: 1 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow those who are followed by fid 1
<strong>            [AllowListCriteria.FARCASTER_FOLLOWED_BY, { fid: 1 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Check If User Is Following Certain Farcaster User

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user is following a certain Farcaster user by using the `FARCASTER_FOLLOWED_BY` and specifying the `fid` of that certain Farcaster user:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
      allowList({
<strong>        apiKey: process.env.AIRSTACK_API_KEY as string,
</strong>        criteria: {
          or: [
            // Only allow those who are following fid 1
<strong>            [AllowListCriteria.FARCASTER_FOLLOWING, { fid: 1 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow those who are following fid 1
<strong>            [AllowListCriteria.FARCASTER_FOLLOWING, { fid: 1 }],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Check If User Is Following The Caster

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user is following the caster of the Frame by using the `FARCASTER_FOLLOWING_CASTER`:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
      allowList({
        apiKey: process.env.AIRSTACK_API_KEY as string,
        criteria: {
          or: [
            // Only allow those who are following the caster
<strong>            [AllowListCriteria.FARCASTER_FOLLOWING_CASTER],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow those who are following the caster
<strong>            [AllowListCriteria.FARCASTER_FOLLOWING_CASTER],
</strong>          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Check If User Hold Certain Token

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user is holding certain token on Ethereum, Base, Zora, or Gold by using the `TOKEN_HOLD` and specify the token's `address` and `chain`:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
  TokenBlockchain,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY as string,
        criteria: {
          or: [
            // Only allow holders of this token on Base
<strong>            [AllowListCriteria.TOKEN_HOLD, {
</strong>              chain: TokenBlokchain.Base,
              address: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
            }],
          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
  type TokenBlockchain,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow holders of this token on Base
<strong>            [AllowListCriteria.TOKEN_HOLD, {
</strong>              chain: TokenBlokchain.Base,
              address: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
            }],
          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Check If User Minted Certain Token

You can use the [`allowListFramesjsMiddleware`](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1) to check if a user has minted certain token on Ethereum, Base, Zora, or Gold by using the `TOKEN_HOLD` and specify the token's `address` and `chain`:

{% tabs %}
{% tab title="TypeScript" %}
<pre class="language-typescript"><code class="lang-typescript">import { createFrames, Button } from "frames.js/next";
import {
  AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
  TokenBlockchain,
} from "@airstack/frames";

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
      allowList({
        apiKey: process.env.AIRSTACK_API_KEY as string,
        criteria: {
          or: [
            // Only allow minters of this token on Base
<strong>            [AllowListCriteria.TOKEN_MINT, {
</strong>              chain: TokenBlokchain.Base,
              address: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
            }],
          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript">const { createFrames, Button } = require("frames.js/next");
const {
  type AllowListCriteriaEnum as AllowListCriteria,
  allowListFramesjsMiddleware as allowList,
  type TokenBlockchain,
} = require("@airstack/frames");

const frames = createFrames();

const handleRequest = frames(
  async (ctx) => {
    // Use `ctx.isAllowed` to check if user is allowed or not
    // based on the Farcaster followers count
<strong>    console.log(ctx.isAllowed);
</strong>    return {
      image: (&#x3C;div>&#x3C;/div>),
      buttons: [],
    };
  },
  {
    middleware: [
      // Allow List Middleware
<strong>      allowList({
</strong>        apiKey: process.env.AIRSTACK_API_KEY,
        criteria: {
          or: [
            // Only allow minters of this token on Base
<strong>            [AllowListCriteria.TOKEN_MINT, {
</strong>              chain: TokenBlokchain.Base,
              address: "0x4c17ff12d9a925a0dec822a8cbf06f46c6268553",
            }],
          ],
        },
      }),
    ],
  }
);
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
true
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding building Farcaster Frames with Airstack Frames.js Middleware, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Allow List Frames.js Middleware Reference](https://www.npmjs.com/package/@airstack/frames#allow-list-middleware-1)
