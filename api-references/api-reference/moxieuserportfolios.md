---
description: >-
  Learn all the detailed references of MoxieUserPortfolios API that provide
  Farcaster user's Fan Token portfolio on the Moxie protocol, including the
  input filters and output fields.
---

# MoxieUserPortfolios

The `MoxieUserPortfolios API` helps you to fetch a Farcaster user's Moxie Fan Token portfolio.

## Inputs

### filter

| Name              | Type     | Description                                                                                                                                                                                                                                                        |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `fanTokenAddress` | `String` | Filter by Fan Token contract address on Base.                                                                                                                                                                                                                      |
| `fanTokenSymbol`  | `String` | <p>Filter by Fan token symbol.<br><br>Depending on the Fan Token type, the symbols follow certain formats:<br>- User type: <code>fid:&#x3C;FID></code><br>- Channel type: <code>cid:&#x3C;CHANNEL-ID></code><br>- Network type: <code>network:farcaster</code></p> |
| `fid`             | `String` | Filter by user's FID.                                                                                                                                                                                                                                              |
| `walletAddress`   | `String` | Filter by user's wallet address.                                                                                                                                                                                                                                   |

### blockchain

{% hint style="info" %}
For **Socials** API, it will return Lens & Farcaster data from all onchain and offchain sources, not specifically Ethereum.

You just need to specify the input for the query to work.
{% endhint %}

| Enum       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |

### order

| Name                             | Description                                                                                                                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `farRank`                        | Sort by Farcaster profile's [farrank](../../social-capital-value-and-social-capital-scores.md).                              |
| `farScore`                       | Sort by Farcaster profile's [farcaster score](../../social-capital-value-and-social-capital-scores.md).                      |
| `followerCount`                  | Sort by the number of users following the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order.         |
| `followingCount`                 | Sort by the number of users being followed by the Lens/Farcaster profile on Lens/Farcaster in ascending or descending order. |
| `profileCreatedAtBlockTimestamp` | Sort by Lens/Farcaster profile creation block timestamp in ascending or descending order.                                    |
| `socialCapitalRank`              | Sort by Farcaster profile's [social capital rank](../../social-capital-value-and-social-capital-scores.md).                  |
| `socialCapitalScore`             | Sort by Farcaster profile's [social capital score](../objects/socialcapitalvalue.md).                                        |
| `updatedAt`                      | Sort by Lens/Farcaster profile last update timestamp in ascending or descending order.                                       |

## Outputs

| Name                        | Type                                            | Description                                                                        |
| --------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------- |
| `fid`                       | `String`                                        | FID of the holder                                                                  |
| `beneficiaryVestingAddress` | `[BeneficiaryVestingAddress]`                   | FID of the caster of the Frames.                                                   |
| `totalLockedAmount`         | `Float`                                         | total quantity of a particular Fan Token locked by the FID.                        |
| `totalUnlockedAmount`       | `Float`                                         | total quantity of a particular Fan Token unlocked by the FID.                      |
| `lockedTvl`                 | `Float`                                         | total amount in moxie of a particular Fan Token locked by the FID.                 |
| `unlockedTvl`               | `Float`                                         | total amount in moxie of a particular Fan Token unlocked by the FID.               |
| `fanTokenAddress`           | `String`                                        | Fan token contract address.                                                        |
| `fanTokenName`              | `String`                                        | Fan Token name.                                                                    |
| `fanTokenSymbol`            | `String`                                        | Fan Token symbol.                                                                  |
| `tokenLockedTvl`            | `Float`                                         | total quantity of a particular Fan Token locked in the Moxie protocol.             |
| `tokenUnlockedTvl`          | `Float`                                         | total quantity of a particular Fan Token unlocked in the Moxie protocol.           |
| `walletAddress`             | `[String]`                                      | List of all wallet address.                                                        |
| `walletFanTokens`           | `[WalletFanToken]`                              | List of holding of this Fan Token across different wallet address for by this FID. |
| `holderSocial`              | [`[Social]`](socials-api.md)                    | **Nested Query** – Social details for the FID who requesting portfolio details.    |
| `fanTokenSocial`            | [`[Social]`](socials-api.md)                    | **Nested Query** – Social details Fan token owner if it is a user Fan Token.       |
| `fanTokenChannel`           | [`FarcasterChannels`](farcasterchannels-api.md) | **Nested Query** – Social details Fan token owner if it is a channel Fan Token.    |
