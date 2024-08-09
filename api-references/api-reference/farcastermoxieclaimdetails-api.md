---
description: >-
  Learn all the detailed references of FarcasterMoxieClaimDetails API that
  provide Moxie claim details of Farcaster user, channel, or network
  information, including the input filters and output fields.
---

# FarcasterMoxieClaimDetails API

This API will provide you details of Moxie claim amounts of a Farcaster user.

## Inputs

### filters

| Name  | Type                          | Description      |
| ----- | ----------------------------- | ---------------- |
| `fid` | `String_Eq_In_Comparator_Exp` | FID of the user. |

### blockchain

{% hint style="info" %}
For **FarcasterMoxieClaimDetails** API, it will return user's Moxie claim details.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### order

| Name                   | Description                                                                  |
| ---------------------- | ---------------------------------------------------------------------------- |
| `availableClaimAmount` | Sort results by the amount of Moxie available that has not been claimed.     |
| `claimedAmount`        | Sort results by the amount of Moxie that has been claimed.                   |
| `processingAmount`     | Sort results by the amount of Moxie that is in the process of being claimed. |

## Outputs

| Name                        | Type                        | Description                                             |
| --------------------------- | --------------------------- | ------------------------------------------------------- |
| `availableClaimAmount`      | `String`                    | The amount of Moxie that is available for claim.        |
| `availableClaimAmountInWei` | `Float`                     | `availableClaimAmount` in wei (multiply by 10^18).      |
| `chainId`                   | `String`                    | Base chain ID.                                          |
| `claimedAmount`             | `String`                    | The amount of Moxie that has been successfully claimed. |
| `claimedAmountInWei`        | `Float`                     | `claimedAmount` in wei (multiply by 10^18).             |
| `fid`                       | `String`                    | The user FID inputted.                                  |
| `processingAmount`          | `String`                    | The amount of Moxie being processed for claimed.        |
| `processingAmountInWei`     | `Float`                     | `processingAmount` in wei (multiply by 10^18).          |
| `socials`                   | [`Socials`](socials-api.md) | Farcaster account details tied to the inputted FID.     |
| `tokenAddress`              | `String`                    | Moxie token address.                                    |
