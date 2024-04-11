---
description: Learn how to submit and validate message data using Airstack Hubs API.
---

# 📧 Message

## Submit Message

You can submit a signed protobuf-serialized message to the Hub by using Airstack Hubs API with the code below:

{% tabs %}
{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";

config();

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    // Encode the message into a Buffer (of bytes)
    const messageBytes = Buffer.from(Message.encode(castAdd).finish());
    const response = await axios.post(`${server}/v1/submitMessage`,
      messageBytes,
      {
        headers: {
          "Content-Type": "application/octet-stream",
          // Provide API key here
<strong>          "x-airstack-hubs": process.env.AIRSTACK_API_KEY as string,
</strong>        },
      }
    );
  
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
</strong>    console.log(`Connected to ${hubRpcEndpoint}`);

    const message; // Any valid message constructed with a Builder

    const submitResult = await client.submitMessage(metadata);
    console.log(submitResult);
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "type": "MESSAGE_TYPE_CAST_ADD",
    "fid": 2,
    "timestamp": 48994466,
    "network": "FARCASTER_NETWORK_MAINNET",
    "castAddBody": {
      "embedsDeprecated": [],
      "mentions": [],
      "parentCastId": {
        "fid": 226,
        "hash": "0xa48dd46161d8e57725f5e26e34ec19c13ff7f3b9"
      },
      "text": "Cast Text",
      "mentionsPositions": [],
      "embeds": []
    }
  },
  "hash": "0xd2b1ddc6c88e865a33cb1a565e0058d757042974",
  "hashScheme": "HASH_SCHEME_BLAKE3",
  "signature": "3msLXzxB4eEYe...dHrY1vkxcPAA==",
  "signatureScheme": "SIGNATURE_SCHEME_ED25519",
  "signer": "0x78ff9a...58c"
}
```
{% endtab %}
{% endtabs %}

## Validate Message

You can validate a signed protobuf-serialized message with the Hub by using Airstack Hubs API with the code below:

{% hint style="info" %}
If you are validating message for Frames, check out [Airstack Frames Validator](../farcaster-frames/frames-validator.md).
{% endhint %}

{% tabs %}
{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";

config();

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    let message = "";
    const response = await axios.post(`${server}/v1/validateMessage`,
      {
        headers: {
          "Content-Type": "application/octet-stream",
          // Provide API key here
<strong>          "x-airstack-hubs": process.env.AIRSTACK_API_KEY as string,
</strong>        },
        data: message;
      }
    );
  
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
    // validate message data with `validateMessage`
<strong>    const submitResult = await client.validateMessage(
</strong>      message,
      metadata
    );
    console.log(submitResult);
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "valid": true,
  "message": {
    "data": {
      "type": "MESSAGE_TYPE_FRAME_ACTION",
      "fid": 2,
      "timestamp": 48994466,
      "network": "FARCASTER_NETWORK_MAINNET",
      "frameActionBody": {
        "url": "https://fcpolls.com/polls/1",
        "buttonIndex": 2,
        "inputText": "",
        "castId": {
          "fid": 226,
          "hash": "0xa48dd46161d8e57725f5e26e34ec19c13ff7f3b9"
        }
      }
    },
    "hash": "0xd2b1ddc6c88e865a33cb1a565e0058d757042974",
    "hashScheme": "HASH_SCHEME_BLAKE3",
    "signature": "3msLXzxB4eEYe...dHrY1vkxcPAA==",
    "signatureScheme": "SIGNATURE_SCHEME_ED25519",
    "signer": "0x78ff9a...58c"
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating submitting and validating message data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Hub State](hub-state.md)
* [Casts](casts.md)
* [Reactions](reactions.md)
* [Links](links.md)
* [User Data](user-data.md)
* [FIDs](fids.md)
* [Storage](storage.md)
* [Username Proofs](username-proofs.md)
* [Verifications](verifcations.md)
* [Hub Events](hub-events.md)
* [Onchain Events](onchain-events.md)
