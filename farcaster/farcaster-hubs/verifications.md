---
description: Learn how to fetch verifications data using Airstack Hubs API.
---

# 🤖 Verifcations

## Get Verifications By FID

You can get all verifications by a specific FID by using Airstack Hubs API with the code below:

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
    // Fetch verification data with `getVerificationsByFid`
<strong>    const verificationRes = await client.getVerificationsByFid(
</strong>      { fid: 2 },
      metadata
    );
    console.log(verificationRes.value);
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
    const response = await axios.get(`${server}/v1/verificationsByFid?fid=2`, {
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
  "messages": [
    {
      "data": {
        "type": "MESSAGE_TYPE_VERIFICATION_ADD_ETH_ADDRESS",
        "fid": 2,
        "timestamp": 73244540,
        "network": "FARCASTER_NETWORK_MAINNET",
        "verificationAddEthAddressBody": {
          "address": "0x91031dcfdea024b4d51e775486111d2b2a715871",
          "ethSignature": "tyxj1...x1cYzhyxw=",
          "blockHash": "0xd74860c4bbf574d5ad60f03a478a30f990e05ac723e138a5c860cdb3095f4296"
        }
      },
      "hash": "0xa505331746ec8c5110a94bdb098cd964e43a8f2b",
      "hashScheme": "HASH_SCHEME_BLAKE3",
      "signature": "bln1zIZM.../4riB9IVBQ==",
      "signatureScheme": "SIGNATURE_SCHEME_ED25519",
      "signer": "0x78ff9...b6d62558c"
    }
  ],
  "nextPageToken": ""
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating verifications data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Casts](casts.md)
* [Reactions](reactions.md)
* [Links](links.md)
* [User Data](user-data.md)
* [FIDs](fids.md)
* [Storage](storage.md)
* [Username Proofs](username-proofs.md)
* [Message](message.md)
* [Hub Events](hub-events.md)
* [Onchain Events](onchain-events.md)
