---
description: Learn how to fetch user data using Airstack Hubs API.
---

# 📱 User Data

## Get Specific User Data By FID

You can get user data by a specific FID by using Airstack Hubs API with the code below:

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
    // Fetch user data with `getUserDataByFid`
<strong>    const userDataRes = await client.getUserData(
</strong>      { fid: 6833, user_data_type: 1 },
      metadata
    );
    console.log(userDataRes?.value);
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
    const response = await axios.get(`${server}/v1/userDataByFid?fid=6833&#x26;user_data_type=1`, {
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
  "data": {
    "type": "MESSAGE_TYPE_USER_DATA_ADD",
    "fid": 6833,
    "timestamp": 83433831,
    "network": "FARCASTER_NETWORK_MAINNET",
    "userDataBody": {
      "type": "USER_DATA_TYPE_PFP",
      "value": "https://i.imgur.com/HG54Hq6.png"
    }
  },
  "hash": "0x327b8f47218c369ae01cc453cc23efc79f10181f",
  "hashScheme": "HASH_SCHEME_BLAKE3",
  "signature": "XITQZD7q...LdAlJ9Cg==",
  "signatureScheme": "SIGNATURE_SCHEME_ED25519",
  "signer": "0x0852...6e999cdd"
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating User data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Casts](casts.md)
* [Reactions](reactions.md)
* [Links](links.md)
* [FIDs](fids.md)
* [Storage](storage.md)
* [Username Proofs](username-proofs.md)
* [Verifications](verifcations.md)
* [Message](message.md)
* [Hub Events](hub-events.md)
* [Onchain Events](onchain-events.md)
