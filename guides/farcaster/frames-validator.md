---
description: >-
  Learn how to validate your Farcaster Frames Signature Packet using the
  Airstack Validation API.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# ✅ Frames Validator

Airstack provides an easy-to-use [Validation API](../../api-references/api-reference/airstack-validation-api.md) to validate your Frames signature packet in your Farcaster Frames.

You can access the [Validation API](../../api-references/api-reference/airstack-validation-api.md) in various ways, depending on the stacks that you are using:

* [Airstack Frames SDK](frames-validator.md#airstack-frames-sdk): suitable for those using JS/TS in their tech stacks, e.g. Frames.js, Next.js, etc.
* [Frog Framework](frames-validator.md#frog-framework): suitable for those using Frog to build their Frames
* [Direct API Call](frames-validator.md#direct-api-call): suitable for those building Frames with other tech stacks than JS/TS.

On the bottom, you can also find the test results on the performance of our API [here](frames-validator.md#testing-on-airstack-validation-api).

## Airstack Frames SDK

{% embed url="https://drive.google.com/file/d/1w0AZ2b7Jj83-ylDUtCRuVwmjaNpxEmbU/view?usp=sharing" %}
Validate Frames Demo
{% endembed %}

You can validate [Frames Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) for your Farcaster Frames by using the [`validateFramesMessage`](https://github.com/Airstack-xyz/airstack-frames-sdk?tab=readme-ov-file#validateframesmessage) function:

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import {
  validateFramesMessage,
  ValidateFramesMessageInput,
  ValidateFramesMessageOutput,
} from "@airstack/frames";

try {
  // Your Frames Signature Packet from the request body
  const body: ValidateFramesMessageInput = {
    untrustedData: {
      fid: 289309,
      url: "https://sample.frames",
      messageHash: "0xabc",
      timestamp: 1709198011100,
      network: 1,
      buttonIndex: 1,
      castId: {
        fid: 289309,
        hash: "0x0000000000000000000000000000000000000001",
      },
    },
    trustedData: {
      messageBytes:
        "0a61080d109dd41118d0c9c72f20018201510a3168747470733a2f2f70656c6963616e2d666f6e642d64697374696e63746c792e6e67726f6b2d667265652e6170702f6f6710011a1a089dd4111214000000000000000000000000000000000000000112146357261fa893e4be85f78178babaca876f9a1fac18012240d1ed649964018377641a78638f0c19d3c346c1eb1a47e856c0fcd87d3fc72ff98172f939fc18ffdd16af746144279e6debb3f4913f491c69d22f6703e554510a280132200295183aaa021cad737db7ddbc075964496ece1c0bcc1009bdae6d1799c83cd4",
    },
  };
  const res: ValidateFramesMessageOutput =
    await validateFramesMessage(body);
} catch (error) {
  console.error(error);
}
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { validateFramesMessage } = require("@airstack/frames");

try {
  // Your Frames Signature Packet from the request body
  const body = {
    untrustedData: {
      fid: 289309,
      url: "https://sample.frames",
      messageHash: "0xabc",
      timestamp: 1709198011100,
      network: 1,
      buttonIndex: 1,
      castId: {
        fid: 289309,
        hash: "0x0000000000000000000000000000000000000001",
      },
    },
    trustedData: {
      messageBytes:
        "0a61080d109dd41118d0c9c72f20018201510a3168747470733a2f2f70656c6963616e2d666f6e642d64697374696e63746c792e6e67726f6b2d667265652e6170702f6f6710011a1a089dd4111214000000000000000000000000000000000000000112146357261fa893e4be85f78178babaca876f9a1fac18012240d1ed649964018377641a78638f0c19d3c346c1eb1a47e856c0fcd87d3fc72ff98172f939fc18ffdd16af746144279e6debb3f4913f491c69d22f6703e554510a280132200295183aaa021cad737db7ddbc075964496ece1c0bcc1009bdae6d1799c83cd4",
    },
  };
  const res = await validateFramesMessage(body);
} catch (error) {
  console.error(error);
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "isValid": true,
  "message": {
    "data": {
      "type": 13,
      "fid": 289309,
      "timestamp": 99738832,
      "network": 1,
      "castAddBody": undefined,
      "castRemoveBody": undefined,
      "reactionBody": undefined,
      "verificationAddAddressBody": undefined,
      "verificationRemoveBody": undefined,
      "userDataBody": undefined,
      "linkBody": undefined,
      "usernameProofBody": undefined,
      "frameActionBody": {
        "url": [
          104, 116, 116, 112, 115, 58, 47, 47, 112, 101, 108, 105, 99, 97, 110,
          45, 102, 111, 110, 100, 45, 100, 105, 115, 116, 105, 110, 99, 116,
          108, 121, 46, 110, 103, 114, 111, 107, 45, 102, 114, 101, 101, 46, 97,
          112, 112, 47, 111, 103
        ],
        "buttonIndex": 1,
        "castId": {
          "fid": 289309,
          "hash": [
            211, 29, 52, 211, 77, 52, 211, 77, 52, 211, 77, 52, 211, 77, 52,
            211, 77, 52, 211, 77, 52, 211, 77, 52, 211, 77, 52, 211, 77, 52, 211
          ]
        },
        "inputText": [],
        "state": [],
        "transactionId": []
      }
    },
    "hash": [
      211, 30, 183, 231, 189, 186, 213, 246, 188, 247, 119, 184, 109, 239, 57,
      127, 191, 53, 239, 198, 218, 109, 167, 26, 243, 190, 159, 245, 173, 95,
      105
    ],
    "hashScheme": 1,
    "signature": [
      209, 237, 100, 153, 100, 1, 131, 119, 100, 26, 120, 99, 143, 12, 25, 211,
      195, 70, 193, 235, 26, 71, 232, 86, 192, 252, 216, 125, 63, 199, 47, 249,
      129, 114, 249, 57, 252, 24, 255, 221, 22, 175, 116, 97, 68, 39, 158, 109,
      235, 179, 244, 145, 63, 73, 28, 105, 210, 47, 103, 3, 229, 84, 81, 10
    ],
    "signatureScheme": 1,
    "signer": [
      211, 29, 54, 247, 157, 124, 221, 166, 154, 211, 109, 92, 105, 222, 247,
      237, 214, 251, 117, 214, 220, 211, 190, 125, 235, 142, 61, 233, 231, 30,
      213, 205, 27, 113, 205, 116, 211, 214, 221, 105, 238, 157, 215, 191, 125,
      115, 205, 220, 119
    ],
    "dataBytes": undefined
  }
}
```
{% endtab %}
{% endtabs %}

## Frog Framework

If you are using the [Frog](https://frog.fm/) Framework, validation logic is embedded so you can instead set the hubs config to validate with the [Airstack Validation API](../../api-references/api-reference/airstack-validation-api.md) by providing the Hub API URL and the [Airstack API key](../../get-started/get-api-key.md):

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Frog } from "frog";
 
const app = new Frog({
  hub: {
    apiUrl: "https://hubs.airstack.xyz",
    fetchOptions: {
      headers: {
        "x-airstack-hubs": "YOUR_AIRSTACK_API_KEY",
      }
    }
  }});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { Frog } = require("frog");
 
const app = new Frog({
  hub: {
    apiUrl: "https://hubs.airstack.xyz",
    headers: {
      "x-airstack-hubs": "YOUR_AIRSTACK_API_KEY",
    }
  }
});
```
{% endtab %}
{% endtabs %}

## Direct API Call

Alternatively, you can also call the [validation API](../../api-references/api-reference/airstack-validation-api.md) by making a **POST** request directly to the `https://hubs.airstack.xyz` and adding the [Airstack API key](../../get-started/get-api-key.md) to the `x-airstack-hubs` field in the header.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import fetch from "node-fetch";

const validateMessageResponse = await fetch(
  "https://hubs.airstack.xyz/v1/validateMessage",
  {
    method: "POST",
    headers: {
      "Content-Type": "application/octet-stream",
      "x-airstack-hubs": "YOUR_AIRSTACK_API_KEY",
    },
    body: new Uint8Array(
      body.trustedData.messageBytes.match(/.{1,2}/g)!.map(
        (byte: string) => parseInt(byte, 16)
      )
    ),
  }
);
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const fetch = require("node-fetch");

const validateMessageResponse = await fetch(
  "https://hubs.airstack.xyz/v1/validateMessage",
  {
    method: "POST",
    headers: {
      "Content-Type": "application/octet-stream",
      "x-airstack-hubs": "YOUR_AIRSTACK_API_KEY",
    },
    body: new Uint8Array(
      body.trustedData.messageBytes.match(/.{1,2}/g)!.map(
        (byte: string) => parseInt(byte, 16)
      )
    ),
  }
);
```
{% endtab %}
{% endtabs %}

## Testing on Airstack Validation API

We also ran some tests on the [Validation API](../../api-references/api-reference/airstack-validation-api.md) with results shown below:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Airstack Validation API Testing Result</p></figcaption></figure>

## Developer Support

If you have any questions or need help regarding validating your Farcaster Frames, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Onchain Kit for Farcaster Frames](airstack-onchain-kit-for-farcaster-frames.md)
* [Math Captcha For Farcaster Frames](https://github.com/limone-eth/farcaster-horizon-airstack/blob/main/app/api/captcha/validate/route.ts)
* [Airstack Validation API Reference](../../api-references/api-reference/airstack-validation-api.md)
* [Airstack Frames SDK Reference](https://github.com/Airstack-xyz/airstack-frames-sdk)
* [Farcaster Frames Guides](../farcaster-frames.md)
