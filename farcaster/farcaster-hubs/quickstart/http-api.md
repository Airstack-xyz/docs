---
description: >-
  Learn how to integrate Airstack Hubs HTTP API into your Farcaster app within
  minutes.
---

# 📠 HTTP API

## Step 1: Install Dependencies

First, install the necessary dependencies to your project:

{% tabs %}
{% tab title="npm" %}
```sh
npm i axios dotenv
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add axios dotenv
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install axios dotenv
```
{% endtab %}

{% tab title="bun" %}
```sh
bun install axios dotenv
```
{% endtab %}
{% endtabs %}

## Step 2: Connect to HTTP API

Then, you can connect and call to Airstack Hubs HTTP API by using library such as `axios` and provide the URL with the [Airstack API key](../../../get-started/get-api-key.md) added to the headers field `x-airstack-hubs`:

{% tabs %}
{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";

config();

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    const response = await axios.get(`${server}/v1/info?dbstats=1`, {
      headers: {
        "Content-Type": "application/json",
        // Provide API key here
<strong>        "x-airstack-hubs": process.env.AIRSTACK_API_KEY as string,
</strong>      },
    });
  
    console.log(response);
  
    console.log(json);
  } catch (e) {
    console.error(e);
  }
}

main();
</code></pre>
{% endtab %}
{% endtabs %}

## Next Steps

Once you have the HTTP API integrated, you can start exploring all the APIs offered:

* [Hub State](../hub-state.md)
* [Casts](../casts.md)
* [Reactions](../reactions.md)
* [Links](../links.md)
* [User Data](../user-data.md)
* [FIDs](../fids.md)
* [Storage](../storage.md)
* [Username Proofs](../username-proofs.md)
* [Verifications](../verifcations.md)
* [Message](../message.md)
* [Hub Events](../hub-events.md)
* [Onchain Events](../onchain-events.md)

## Developer Support

If you have any questions or need help regarding integrating Airstack HTTP API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Hubs HTTP API Swagger](https://app.gitbook.com/o/RSmZxCZGQwSEBar1KeT7/s/9qgSPEovTA9myKKaerOr/\~/changes/804/farcaster-hubs-api/overview#http-api)
* [Hubs HTTP API Reference](https://docs.farcaster.xyz/reference/hubble/httpapi/httpapi)
