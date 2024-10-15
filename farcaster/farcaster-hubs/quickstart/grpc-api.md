---
description: >-
  Learn how to integrate Airstack Hubs GRPC API into your Farcaster app within
  minutes.
---

# ⚡ GRPC API

## Step 1: Install Dependencies

First, install the necessary dependencies to your project:

{% tabs %}
{% tab title="npm" %}
```sh
npm i @farcaster/hub-nodejs dotenv
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add @farcaster/hub-nodejs dotenv
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm install @farcaster/hub-nodejs dotenv
```
{% endtab %}

{% tab title="bun" %}
```sh
bun install @farcaster/hub-nodejs dotenv
```
{% endtab %}
{% endtabs %}

## Step 2: Connect to GRPC API

Then, you can connect and call to Airstack Hubs GRPC API by using the `@farcaster/hub-nodejs` library offered by the Farcaster team by providing the GRPC URL with the [Airstack API key](../../../get-api-key.md) added to the metadata field `x-airstack-hubs`:

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-typescript"><code class="lang-typescript">import {
  getSSLHubRpcClient,
  Metadata
} from '@farcaster/hub-nodejs';
import { config } from "dotenv";

config();

// Instantiate the gRPC client for a secure connection
const client = getSSLHubRpcClient('hubs-grpc.airstack.xyz');

client.$.waitForReady(Date.now() + 5000, async (e) => {
  if (e) {
    console.error(`Failed to connect to the gRPC server:`, e);
    process.exit(1);
  } else {
    console.log(`Connected to the gRPC server`);
    const metadata = new Metadata();
    // add API key here
<strong>    metadata.add("x-airstack-hubs", process.env.AIRSTACK_API_KEY as string);
</strong>
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}
{% endtabs %}

## Next Steps

Once you have the GRPC API integrated, you can start exploring all the APIs offered:

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

If you have any questions or need help regarding integrating Airstack GRPC API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Hubs GRPC API Reference](https://docs.farcaster.xyz/reference/hubble/grpcapi/grpcapi)
