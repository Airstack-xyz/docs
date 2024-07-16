---
description: Learn how to fetch Farcaster casts data using Airstack Hubs API.
---

# 💬 Casts

## Write Casts

You can create a new casts to the Hub by using Airstack Hubs API with the code below:

{% hint style="info" %}
For this, you will first need to [create your own signer](https://docs.farcaster.xyz/developers/guides/accounts/create-account-key) to call the submit message API.\
\
For details on creating casts with mentions, replies, embeds, and other details, check [here](https://docs.farcaster.xyz/developers/guides/writing/casts).
{% endhint %}

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-typescript"><code class="lang-typescript">import {
  hexStringToBytes,
  getSSLHubRpcClient,
  Metadata,
  makeCastAdd,
  FarcasterNetwork,
} from "@farcaster/hub-nodejs";
import { config } from "dotenv";

config();

const SIGNER_PRIVATE_KEY = '0x...'; // Your signer's private key
const FID = 1; // Your fid
const ed25519Signer = new NobleEd25519Signer(SIGNER_PRIVATE_KEY);
const dataOptions = {
  fid: FID,
  network: FC_NETWORK,
};
const FC_NETWORK = FarcasterNetwork.MAINNET;

const client = getSSLHubRpcClient("hubs-grpc.airstack.xyz");

// Construct the cast
<strong>const cast = await makeCastAdd(
</strong>  {
    text: 'This is a cast!', // Text can be up to 320 bytes long
    embeds: [],
    embedsDeprecated: [],
    mentions: [],
    mentionsPositions: [],
  },
  dataOptions,
  ed25519Signer
);

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
    if (cast.isOk()) {
      // Broadcast the cast/message to the Farcaster network
<strong>      const submitResult = await client.submitMessage(
</strong>        castReplyResult.value,
        metadata
      );
      if (submitResult.isOk()) {
        console.log(`Reply posted successfully`);
      } else {
        console.error(`Error posting reply: ${submitResult.error}`);
      }
    } else {
      const error = cast.error;
      // Handle the error case
      console.error(`Error posting reply: ${error}`);
    }
    // After everything, close the RPC connection
    client.close();
  }
});
</code></pre>
{% endtab %}

{% tab title="axios" %}
<pre class="language-typescript"><code class="lang-typescript">import axios from "axios";
import { config } from "dotenv";
import {
  makeCastAdd,
  FarcasterNetwork,
} from "@farcaster/hub-nodejs";
// Require `npm install @farcaster/core`
import { Message } from "@farcaster/core";

config();

const SIGNER_PRIVATE_KEY = '0x...'; // Your signer's private key
const FID = 1; // Your fid
const ed25519Signer = new NobleEd25519Signer(SIGNER_PRIVATE_KEY);
const dataOptions = {
  fid: FID,
  network: FC_NETWORK,
};
const FC_NETWORK = FarcasterNetwork.MAINNET;

// Construct the cast
const cast = await makeCastAdd(
  {
    text: 'This is a cast!', // Text can be up to 320 bytes long
    embeds: [],
    embedsDeprecated: [],
    mentions: [],
    mentionsPositions: [],
  },
  dataOptions,
  ed25519Signer
);

const main = async () => {
  const server = "https://hubs.airstack.xyz";
  try {
    // Encode the message into a Buffer (of bytes)
    const messageBytes = Buffer.from(Message.encode(cast).finish());
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

## Check If FID Casted A Given Cast Hash

You can check if a specific FID a specific cast with a given cast hash by using Airstack Hubs API with the code below:

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-javascript"><code class="lang-javascript">import {
  Metadata,
  getSSLHubRpcClient,
} from "@farcaster/hub-nodejs";

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
    const castHashHex = 'd2b1ddc6c88e865a33cb1a565e0058d757042974';
    const castHashBytes = hexStringToBytes(castHashHex)._unsafeUnwrap(); // Safety: castHashHex is known and can't error
    
    // Fetch cast data with `getCast`
    const castResult = await client.getCast({ fid: 2, hash: castHashBytes }, metadata);
    console.log(castResult.value);
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
    const response = await axios.get(`${server}/v1/castById?fid=2&#x26;hash=0xd2b1ddc6c88e865a33cb1a565e0058d757042974`, {
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

## Get Casts Authored By FID

You can get all the casts authored by a specific FID by using Airstack Hubs API with the code below:

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-javascript"><code class="lang-javascript">import {
  Metadata,
  getSSLHubRpcClient,
} from "@farcaster/hub-nodejs";

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
    // Fetch casts data with `getCastsByFid`
    const castsResult = await client.getCastsByFid({ fid: 2 }, metadata);
    console.log(castsResult.value);
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
    const response = await axios.get(`${server}/v1/castsByFid?fid=2`, {
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
        "type": "MESSAGE_TYPE_CAST_ADD",
        "fid": 2,
        "timestamp": 48994466,
        "network": "FARCASTER_NETWORK_MAINNET",
        "castAddBody": {... },
          "text": "Cast Text",
          "mentionsPositions": [],
          "embeds": []
        }
      },
      "hash": "0xd2b1ddc6c88e865a33cb1a565e0058d757042974",
      "hashScheme": "HASH_SCHEME_BLAKE3",
      "signature": "3msLXzxB4eEYeF0Le...dHrY1vkxcPAA==",
      "signatureScheme": "SIGNATURE_SCHEME_ED25519",
      "signer": "0x78ff9a768cf1...2eca647b6d62558c"
    }
  ]
  "nextPageToken": ""
}
```
{% endtab %}
{% endtabs %}

## Get Cast That Mention FID

You can get all the casts that mentioned a specific FID by using Airstack Hubs API with the code below:

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-javascript"><code class="lang-javascript">import {
  Metadata,
  getSSLHubRpcClient,
} from "@farcaster/hub-nodejs";

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
    // Fetch casts data with `getCastsByMention`
    const castsResult = await client.getCastsByMention({ fid: 2 }, metadata);
    console.log(castsResult.value);
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
    const response = await axios.get(`${server}/v1/castsByMention?fid=6833`, {
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
        "type": "MESSAGE_TYPE_CAST_ADD",
        "fid": 2,
        "timestamp": 62298143,
        "network": "FARCASTER_NETWORK_MAINNET",
        "castAddBody": {
          "embedsDeprecated": [],
          "mentions": [15, 6833],
          "parentCastId": {
            "fid": 2,
            "hash": "0xd5540928cd3daf2758e501a61663427e41dcc09a"
          },
          "text": "cc  and ",
          "mentionsPositions": [3, 8],
          "embeds": []
        }
      },
      "hash": "0xc6d4607835197a8ee225e9218d41e38aafb12076",
      "hashScheme": "HASH_SCHEME_BLAKE3",
      "signature": "TOaWrSTmz+cyzPMFGvF...OeUznB0Ag==",
      "signatureScheme": "SIGNATURE_SCHEME_ED25519",
      "signer": "0x78ff9a768c...647b6d62558c"
    }
  ],
  "nextPageToken": ""
}
```
{% endtab %}
{% endtabs %}

## Get Cast By Parent Cast Or URL

You can get all casts by parent cast's FID and Hash OR by the parent's URL by using Airstack Hubs API with the code below:

{% tabs %}
{% tab title="@farcaster/hub-nodejs" %}
<pre class="language-typescript"><code class="lang-typescript">import {
  Metadata,
  getSSLHubRpcClient,
} from "@farcaster/hub-nodejs";

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
    const castHashHex = 'a48dd46161d8e57725f5e26e34ec19c13ff7f3b9';
    const castHashBytes = hexStringToBytes(castHashHex)._unsafeUnwrap(); // Safety: castHashHex is known

    // Fetch casts data with `getCastsByParent`
    const castsResult = await client.getCastsByParent({ parentCastId: { fid: 2, hash: castHashBytes } });
    console.log(castsResult.value);
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
    const response = await axios.get(`${server}/v1/castsByParent?fid=226&#x26;hash=0xa48dd46161d8e57725f5e26e34ec19c13ff7f3b9`, {
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
        "type": "MESSAGE_TYPE_CAST_ADD",
        "fid": 226,
        "timestamp": 48989255,
        "network": "FARCASTER_NETWORK_MAINNET",
        "castAddBody": {
          "embedsDeprecated": [],
          "mentions": [],
          "parentCastId": {
            "fid": 226,
            "hash": "0xa48dd46161d8e57725f5e26e34ec19c13ff7f3b9"
          },
          "text": "Cast's Text",
          "mentionsPositions": [],
          "embeds": []
        }
      },
      "hash": "0x0e501b359f88dcbcddac50a8f189260a9d02ad34",
      "hashScheme": "HASH_SCHEME_BLAKE3",
      "signature": "MjKnOQCTW42K8+A...tRbJfia2JJBg==",
      "signatureScheme": "SIGNATURE_SCHEME_ED25519",
      "signer": "0x6f1e8758...7f04a3b500ba"
    }
  ],
  "nextPageToken": ""
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding integrating Casts data using AIrstack Hubs API into your Farcaster app, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

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
