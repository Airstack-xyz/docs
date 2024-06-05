---
description: Learn how to fetch storage data using Airstack Hubs API.
---

# 📦 Storage

## Get Storage Limits By FID

You can get the detailed storage limit of various categories (cast storage, follow storage, recation storage, etc.) that a specific FID has by using Airstack Hubs API with the code below:

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
    // Fetch Hubs info data with `getCurrentStorageLimitsByFid`
<strong>    const storageRes = await client.getCurrentStorageLimitsByFid(
</strong>      { fid: 6833 },
      metadata
    );
    console.log(storageRes.value);
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
    const response = await axios.get(`${server}/v1/storageLimitsByFid?fid=6833`, {
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
<pre class="language-json"><code class="lang-json">{
  "limits": [
    {
<strong>      "storeType": "STORE_TYPE_CASTS", // Cast Storage
</strong>      "name": "\b\u0004�",
      "limit": 75000,
      "used": 25488,
      "earliestTimestamp": 108097112,
      "earliestHash": "K/krBlhQPy2ps3S+16OvvdPOjhM="
    },
    {
<strong>      "storeType": "STORE_TYPE_LINKS", // Follow Storage
</strong>      "name": ",�J",
      "limit": 37500,
      "used": 3391,
      "earliestTimestamp": 108090342,
      "earliestHash": "W3FsMBBZUMHJZvLeOjsBFgcLrKA="
    },
    {
<strong>      "storeType": "STORE_TYPE_REACTIONS", // Reaction Storage
</strong>      "name": "D@\u0002L��",
      "limit": 37500,
      "used": 37500,
      "earliestTimestamp": 85728693,
      "earliestHash": "31caeSXVnC1wV2qZMRgw6ttP2Xw="
    },
    {
<strong>      "storeType": "STORE_TYPE_USER_DATA", // User Profile Storage
</strong>      "name": "Q!\u0011�0\u0013",
      "limit": 750,
      "used": 4,
      "earliestTimestamp": 69403216,
      "earliestHash": "XzbR+DhAEAQAQpanel46whd4mVw="
    },
    {
<strong>      "storeType": "STORE_TYPE_USERNAME_PROOFS", // Username Proofs Storage
</strong>      "name": "Q!\u00114\u0003\u0004��N8T",
      "limit": 75,
      "used": 2,
      "earliestTimestamp": 80755811,
      "earliestHash": "UiDlOTI7DUyvm77kMWKg8pW3zx0="
    },
    {
<strong>      "storeType": "STORE_TYPE_VERIFICATIONS", // Connected/Verifid Accounts Storage
</strong>      "name": "TDH\u0014��L��",
      "limit": 375,
      "used": 5,
      "earliestTimestamp": 77306045,
      "earliestHash": "oxQVvW/ln0rgRYjr4UmM7FNHJgU="
    }
  ],
  "units": 15
}
</code></pre>
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating storage data using Airstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Casts](casts.md)
* [Reactions](reactions.md)
* [Links](links.md)
* [User Data](user-data.md)
* [FIDs](fids.md)
* [Username Proofs](username-proofs.md)
* [Verifications](verifcations.md)
* [Message](message.md)
* [Hub Events](hub-events.md)
* [Onchain Events](onchain-events.md)
