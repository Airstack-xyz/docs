---
description: >-
  Learn all the detailed references of FarcasterNotaEarningStats API that
  provide Nota historical earning of a user or channel information, including
  the input filters and output fields.
---

# FarcasterNotaEarningStats

This API will help you caluclate the NOTA (offchain) Earnings that is received by an entity, that could either be a user or channel type.

Nota earnings calculation will stop on July 29th, 2024 and future earnings will be in MOXIE. To get Moxie earnings, use [`FarcasterMoxieEarningStats`](farcastermoxieearningstats.md) instead.

## Inputs

### filters

| Name         | Type                                                                                       | Description                                       |
| ------------ | ------------------------------------------------------------------------------------------ | ------------------------------------------------- |
| `entityId`   | `String`                                                                                   | The name of the user or channel.                  |
| `entityType` | [`FarcasterNotaEarningStatsEntityType`](../enum/farcasternotaearningstatsentitytype.md)`!` | The type of fan token, either `USER` or `CHANNEL` |
| `timeFrame`  | [`FarcasterNotaEarningStatsTimeFrame`](../enum/farcasternotaearningstatstimeframe.md)`!`   | either `TODAY`, `WEEKLY`, or `LIFETIME`.          |

### blockchain

{% hint style="info" %}
For **FarcasterChannelParticipants** API, it will return all Farcaster channels.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

### order

| Name               | Description                                         |
| ------------------ | --------------------------------------------------- |
| `allEarnings`      | Sort results by the NOTA earnings from all sources. |
| `castEarnings`     | Sort results by the NOTA earnings from casts.       |
| `frameDevEarnings` | Sort results by the NOTA earnings from Frame devs.  |
| `otherEarnings`    | Sort results by other NOTA earnings.                |

## Outpus

| Name                          | Type                                                                                     | Description                                                                                              |
| ----------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `entityId`                    | `String`                                                                                 | The name of the user or channel.                                                                         |
| `entityType`                  | [`FarcasterNOTAEarningStatsEntityType!`](../enum/farcasternotaearningstatsentitytype.md) | The type of fan token, either `USER` or `CHANNEL`                                                        |
| `allEarningsAmount`           | `Float`                                                                                  | The total earnings of NOTA by the entity.                                                                |
| `allEarningsAmountInWei`      | `String`                                                                                 | The total earnings of NOTA by the entity in wei (multiply 10^18 from the original amount).               |
| `castEarningsAmount`          | `Float`                                                                                  | The earnings of NOTA earned from casts by the entity.                                                    |
| `castEarningsAmountInWei`     | `String`                                                                                 | The earnings of NOTA earned from casts by the entity in wei (multiply 10^18 from the original amount)    |
| `frameDevEarningsAmount`      | `Float`                                                                                  | The earnings of NOTA earned from frames by the entity.                                                   |
| `frameDevEarningsAmountInWei` | `String`                                                                                 | The earnings of NOTA earned from frames by the entity  in wei (multiply 10^18 from the original amount). |
| `otherEarningsAmount`         | `Float`                                                                                  | The earnings of NOTA earned from others by the entity.                                                   |
| `otherEarningsAmountInWei`    | `String`                                                                                 | The earnings of NOTA earned from others by the entity in wei (multiply 10^18 from the original amount).  |
| `channel`                     | [`FarcasterChannels`](farcasterchannels-api.md)                                          | Channel details if the entity is `CHANNEL` type.                                                         |
| `socials`                     | [`Socials`](socials-api.md)                                                              | User details if the entiy is `USER` type.                                                                |
| `timeframe`                   | [`FarcasterNotaEarningStatsTimeFrame`](../enum/farcasternotaearningstatstimeframe.md)    | either `TODAY`, `WEEKLY`, or `LIFETIME`, depending on the input chosen.                                  |
| `startTimestamp`              | `Time!`                                                                                  | The time when the entity start earning NOTA.                                                             |
| `endTimestamp`                | `Time!`                                                                                  | The time when the entity last earn NOTA.                                                                 |
