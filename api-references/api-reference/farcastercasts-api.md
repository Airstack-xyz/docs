---
description: >-
  Learn all the detailed references of FarcasterCasts API that provide Farcaster
  casts information, including the input filters and output fields.
---

# FarcasterCasts API

The FarcasterCasts API enables you to fetch a list of all casts based on various filters, including casters, cast hash, cast's parent hash, cast's URL, casting time, and whether they contain frames, embeds, or any mentions in them.

## Inputs

### filters

| Name                | Type                          | Description                                                                                                          |
| ------------------- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `castedBy`          | `Identity_Comparator_Exp`     | Filter by caster. You can input any type of identity supported by [Airstack Identity API](airstack-identity-api.md). |
| `castedAtTimestamp` | `Time_Comparator_Exp`         | Filter by the time when Frames is casted.                                                                            |
| `rootParentUrl`     | `String_Eq_In_Comparator_Exp` | Filter By Farcaster channel's URL.                                                                                   |
| `frameUrl`          | `String_Eq_In_Comparator_Exp` | Filter By Frame URL.                                                                                                 |
| `hasEmbeds`         | `Boolean_Comparator_Exp`      | Filter by whether a cast has any embeds or not.                                                                      |
| `hasFrames`         | `Boolean_Comparator_Exp`      | Filter by whether a cast has any Frames or not.                                                                      |
| `hasMentions`       | `Boolean_Comparator_Exp`      | Filter by whether a cast has any mentions or not.                                                                    |
| `hash`              | `String_Eq_In_Comparator_Exp` | Filter by Farcaster cast hash.                                                                                       |
| `url`               | `String_Eq_In_Comparator_Exp` | Filter by Warpcast's Cast URL.                                                                                       |

### blockchain

{% hint style="info" %}
For **FarcasterCasts** API, it will return all Farcaster casts.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

## Outputs

| Name                 | Type                                                     | Description                                                                     |
| -------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `castedAtTimestamp`  | `Time`                                                   | Time when the cast was casted.                                                  |
| `castedBy`           | [`Social`](socials-api.md)                               | **Nested Query** – Caster details.                                              |
| `channel`            | [`FarcasterChannel`](farcasterchannels-api.md)           | **Nested Query** – Farcaster Channel where the cast is casted.                  |
| `embeds`             | `[Map]`                                                  | Embeds contained in the casts.                                                  |
| `fid`                | `String`                                                 | Caster's FID.                                                                   |
| `frame`              | [`FarcasterFrame`](../objects/farcasterframe.md)         | **Nested Query** – Farcaster Frame attached to the cast.                        |
| `hash`               | `String`                                                 | Cast Hash.                                                                      |
| `id`                 | `ID`                                                     | Airstack unique identifier for the data point.                                  |
| `mentions`           | `[Mentions]`                                             | **Nested Query** – Farcaster profiles that like that are mentioned in the cast. |
| `numberOfLikes`      | `Int`                                                    | Number of likes on the cast.                                                    |
| `numberOfRecasts`    | `Int`                                                    | Number of recasts on the cast.                                                  |
| `numberOfReplies`    | `Int`                                                    | Number of replies on the cast.                                                  |
| `parentCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Cast's parent details.                                       |
| `parentFid`          | `String`                                                 | Cast's parent FID.                                                              |
| `parentHash`         | `String`                                                 | Cast's parent hash.                                                             |
| `parentUrl`          | `String`                                                 | Cast's parent URL.                                                              |
| `quotedCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Quoted cast's details.                                       |
| `rawText`            | `String`                                                 | Raw text contained in the cast.                                                 |
| `recastedBy`         | [`[Social]`](socials-api.md)                             | **Nested Queries** – List of all Farcaster profiles recasted the cast.          |
| `rootParentUrl`      | `String`                                                 | The cast's associated Farcaster Channel URL.                                    |
| `socialCapitalValue` | [`SocialCapitalValue`](../objects/socialcapitalvalue.md) | **Nested Query** – The social capital value details of a given Farcaster cast.  |
| `text`               | `String`                                                 | The text content of the cast.                                                   |
| `url`                | `String`                                                 | Warpcast's Cast URL.                                                            |
