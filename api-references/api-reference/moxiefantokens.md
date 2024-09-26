---
description: >-
  Learn all the detailed references of MoxieFanTokens API that provide Fan
  Tokens information on the Moxie protocol, including the input filters and
  output fields.
---

# MoxieFanTokens

The `MoxieFanTokens` API helps you to fetch data regarding specific Fan Tokens on the Moxie protocol.

## Inputs

### filter

| Name              | Type     | Description                                                                                                                                                                                                                                                            |
| ----------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `fanTokenAddress` | `String` | Filter by the Fan Token contract address on Base.                                                                                                                                                                                                                      |
| `fanTokenSymbol`  | `String` | <p>Filter by the Fan Token symbol.<br><br>Depending on the Fan Token type, the symbols follow certain formats:<br>- User type: <code>fid:&#x3C;FID></code><br>- Channel type: <code>cid:&#x3C;CHANNEL-ID></code><br>- Network type: <code>network:farcaster</code></p> |
| `uniqueHolders`   | `String` | Filter by the number of unique holder.                                                                                                                                                                                                                                 |

## Outputs

| Name                  | Type                                            | Description                                                                            |
| --------------------- | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| `fanTokenAddress`     | `String`                                        | Fan Token contract address.                                                            |
| `fanTokenName`        | `String`                                        | Fan Token name.                                                                        |
| `fanTokenSymbol`      | `String`                                        | Fan Token symbol.                                                                      |
| `totalSupply`         | `Float`                                         | Total supply of the Fan Token.                                                         |
| `tlv`                 | `Float`                                         | reserve for fan token.                                                                 |
| `uniqueHolders`       | `Int`                                           | The number of unique Fan Token holders.                                                |
| `currentPrice`        | `Float`                                         | Current price of the Fan Token in Moxie.                                               |
| `currentPriceInWei`   | `Float`                                         | Current price of the Fan Token in Moxie (in wei, multiply 10^18).                      |
| `totalUnlockedAmount` | `Float`                                         | total amount in moxie unlocked for this Fan Token across all the users.                |
| `totalLockedAmount`   | `Float`                                         | total amount in moxie locked for this Fan Token across all the users.                  |
| `socials`             | [`Socials`](socials-api.md)                     | **Nested Query** – Social profile details of the subject if it's a user-type Fan Token |
| `channel`             | [`FarcasterChannels`](farcasterchannels-api.md) | **Nested Query** – Channle details of the subject if it's a channel-type Fan Token     |
| `lockedTvl`           | `Float`                                         | total amount in moxie of a particular Fan Token locked across all the users.           |
| `unlockedTvl`         | `Float`                                         | total amount in moxie of a particular Fan Token unlocked across all the users          |
