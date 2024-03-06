---
description: >-
  Learn all the detailed reference of validation API to validate Frames
  Signature Packet.
---

# Airstack Validation API

## HTTPS URL

https://hubs.airstack.xyz/v1/validateMessage

## Method

POST

## Input

| Field  | Type                         | Required | Description             |
| ------ | ---------------------------- | -------- | ----------------------- |
| `body` | `ValidateFramesMessageInput` | true     | Frames Signature Packet |

## Usage

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { validateFramesMessage } from "@airstack/frames";

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
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { validateFramesMessage } = require("@airstack/frames");

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
```
{% endtab %}
{% endtabs %}

## Response

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

