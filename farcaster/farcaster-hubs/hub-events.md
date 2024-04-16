---
description: Learn how to fetch Hub events data using Airstack Hubs API.
---

# 🎉 Hub Events

## Get Specific Event By ID

You can get an event by its Id to the Hub by using Airstack Hubs API with the code below:

{% tabs %}
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
    // Fetch event data with `getEvent`
<strong>    const eventRes = await client.getEvent(
</strong>      { id: 421699585773568 },
      metadata
    );
    console.log(eventRes.value);
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";

config();

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    const response = await axios.get(`${server}/v1/eventById?event_id=421699585773568`, {
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

{% tab title="Response" %}
```json
{
  "type": "HUB_EVENT_TYPE_MERGE_USERNAME_PROOF",
  "id": 350909155450880,
  "mergeUsernameProofBody": {
    "usernameProof": {
      "timestamp": 1695049760,
      "name": "nftonyp",
      "owner": "0x23b3c29900762a70def5dc8890e09dc9019eb553",
      "signature": "xp41PgeO...hJpNshw=",
      "fid": 20114,
      "type": "USERNAME_TYPE_FNAME"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get All Events From Hubs

You can get all events from the Hub by using Airstack Hubs API with the code below:

{% hint style="info" %}
Hubs prune events older than 3 days, so not all historical events can be fetched via this API.
{% endhint %}

{% tabs %}
{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";

config();

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    const response = await axios.get(`${server}/v1/events?from_event_id=350909155450880`, {
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
    const subscribeResult = await client.subscribe({
      eventTypes: [HubEventType.MERGE_MESSAGE],
      from_event_id: 350909155450880,
    });

    if (subscribeResult.isOk()) {
      const stream = subscribeResult.value;

      for await (const event of stream) {
        console.log(event);
      }
    }
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "nextPageEventId": 350909170294785,
  "events": [
    {
      "type": "HUB_EVENT_TYPE_MERGE_USERNAME_PROOF",
      "id": 350909155450880,
      "mergeUsernameProofBody": {
        "usernameProof": {
          "timestamp": 1695049760,
          "name": "nftonyp",
          "owner": "0x23b3c29900762a70def5dc8890e09dc9019eb553",
          "signature": "xp41PgeOz...9Jw5vT/eLnGphJpNshw=",
          "fid": 20114,
          "type": "USERNAME_TYPE_FNAME"
        }
      }
    },
    ...
  ]
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating Farcaster Hub events data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

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
* [Onchain Events](onchain-events.md)
