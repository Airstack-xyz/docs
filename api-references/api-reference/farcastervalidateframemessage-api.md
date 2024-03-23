---
description: >-
  Learn all the detailed references of Tokens API that provide ERC20/721/1155
  token detail information, including the input filters, supported chains, and
  output fields.
---

# FarcasterValidateFrameMessage API

The `FarcasterValidateFrameMessage` API helps you to validate any Frames Signature packet and provide additional social data enrichment on the caster and interactor of the Frames.

## Inputs

### filter

| Name           | Type     | Description                                                                                                                              |
| -------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `messageBytes` | `String` | Signed message bytes coming from the [Frames Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet). |

## Outputs

| Name              | Type                       | Description                                                                                                                                                       |
| ----------------- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `castedBy`        | [`Social`](socials-api.md) | **Nested Query** – Social profile details of the caster of the Frames.                                                                                            |
| `castedByFid`     | `Int`                      | FID of the caster of the Frames.                                                                                                                                  |
| `interactedBy`    | [`Social`](socials-api.md) | **Nested Query** – Social profile details of the interactor of the Frames                                                                                         |
| `interactedByFid` | `Int`                      | FID of the interactor of the Frames.                                                                                                                              |
| `isValid`         | `Boolean`                  | Indicates whether the signed message from the [Frames Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet) is valid or not. |
| `message`         | `FrameMessage`             | **Nested Query** – Formatted validated message details, including fids, buttonIndex, transactionHash, etc.                                                        |
| `messageByte`     | `String`                   | Signed message bytes coming from the [Frames Signature Packet](https://docs.farcaster.xyz/reference/frames/spec#frame-signature-packet).                          |
| `messageRaw`      | `Map`                      | Raw validated message details in JSON.                                                                                                                            |
