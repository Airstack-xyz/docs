---
description: Learn how to fetch Hub state data using Airstack Hubs API.
---

# 🔃 Hub State

## Get Hubs Info

You can get hubs info using Airstack Hubs API with the code below:

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

{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-typescript"><code class="lang-typescript">import {
  Metadata,
  getSSLHubRpcClient,
} from "@farcaster/hub-nodejs";
import { config } from "dotenv";

config();

const client = getSSLHubRpcClient("hubs-grpc.airstack.xyz");

client.$.waitForReady(Date.now() + 5000, async (e) => {
  if (e) {
    console.error(`Failed to connect to the gRPC server:`, e);
    process.exit(1);
  } else {
    console.log(`Connected to the gRPC server`);
    const metadata = new Metadata();
    // Provide API key here
<strong>    metadata.add("x-airstack-hubs", process.env.AIRSTACK_API_KEY as string);
</strong>
    // Fetch Hubs info data with `getInfo`
<strong>    const hubsInfoRes = await client.getInfo({ dbStats: true }, metadata);
</strong>    console.log(hubsInfoRes);
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "version": "1.5.5",
  "isSyncing": false,
  "nickname": "Farcaster Hub",
  "rootHash": "fa349603a6c29d27041225261891bc9bc846bccb",
  "dbStats": {
    "numMessages": 4191203,
    "numFidEvents": 20287,
    "numFnameEvents": 20179
  },
  "peerId": "12D3KooWNr294AH1fviDQxRmQ4K79iFSGoRCWzGspVxPprJUKN47",
  "hubOperatorFid": 6833
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating Hub State data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Casts](casts.md)
* [Reactions](reactions.md)
* [Links](links.md)
* [User Data](user-data.md)
* [FIDs](fids.md)
* [Storage](storage.md)
* [Username Proofs](username-proofs.md)
* [Verifications](verifcations.md)
* [Message](message.md)
* [Hub Events](hub-events.md)
* [Onchain Events](onchain-events.md)
