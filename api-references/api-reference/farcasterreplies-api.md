---
description: >-
  Learn all the detailed references of FarcasterReplies API that provide
  Farcaster casts information, including the input filters and output fields.
---

# FarcasterReplies API

The FarcasterReplies API enables you to fetch a list of all replied based on various filters, including repliers, cast reply hash, cast reply's parent hash, and cast reply's URL.

## Inputs

### filter

| Name             | Type                          | Description                                                                                                                                                                                    |
| ---------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `hash`           | `String_Comparator_Exp`       | Filter by the reply cast hash.                                                                                                                                                                 |
| `parentCastedBy` | `Identity_Comparator_Exp`     | <p>Filter by parent caster's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p> |
| `paretHash`      | `String_Eq_In_Comparator_Exp` | Filter by parent cast hash.                                                                                                                                                                    |
| `parentUrl`      | `String_Eq_In_Comparator_Exp` | Filter by parent cast URL.                                                                                                                                                                     |
| `repliedBy`      | `Identity_Comparator_Exp`     | <p>Filter by replier's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p>       |

### blockchain

{% hint style="info" %}
For **FarcasterReplies** API, it will return all Farcaster replies.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

## Outputs

| Name                 | Type                                                     | Description                                                                          |
| -------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `castedAtTimestamp`  | `Time`                                                   | Time when the reply was casted.                                                      |
| `castedBy`           | [`Social`](socials-api.md)                               | **Nested Query** – Replier's details.                                                |
| `channel`            | [`FarcasterChannel`](farcasterchannels-api.md)           | **Nested Query** – Farcaster Channel where the reply is casted.                      |
| `embeds`             | `[Map]`                                                  | Embeds contained in the reply.                                                       |
| `fid`                | `String`                                                 | Replier's FID.                                                                       |
| `frame`              | [`FarcasterFrame`](../objects/farcasterframe.md)         | **Nested Query** – Farcaster Frame attached to the reply.                            |
| `hash`               | `String`                                                 | Reply cast hash.                                                                     |
| `id`                 | `ID`                                                     | Airstack unique identifier for the data point.                                       |
| `mentions`           | `[Mentions]`                                             | **Nested Query** – Farcaster profiles that like that are mentioned in the reply.     |
| `numberOfLikes`      | `Int`                                                    | Number of likes on the reply.                                                        |
| `numberOfRecasts`    | `Int`                                                    | Number of recasts on the reply.                                                      |
| `numberOfReplies`    | `Int`                                                    | Number of replies on the reply.                                                      |
| `parentCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Cast reply's parent details.                                      |
| `parentFid`          | `String`                                                 | Cast reply's parent FID.                                                             |
| `parentHash`         | `String`                                                 | Cast reply's parent hash.                                                            |
| `parentUrl`          | `String`                                                 | Cast reply's parent URL.                                                             |
| `quotedCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Quoted cast's details of the reply.                               |
| `rawText`            | `String`                                                 | Raw text contained in the reply.                                                     |
| `recastedBy`         | [`[Social]`](socials-api.md)                             | **Nested Queries** – List of all Farcaster profiles recasted the reply.              |
| `rootParentHash`     | `String`                                                 | The root hash of the reply.                                                          |
| `rootParentUrl`      | `String`                                                 | The root URL of the reply.                                                           |
| `socialCapitalValue` | [`SocialCapitalValue`](../objects/socialcapitalvalue.md) | **Nested Query** – The social capital value details of a given Farcaster cast reply. |
| `text`               | `String`                                                 | The text content of the reply.                                                       |
| `url`                | `String`                                                 | Warpcast's cast reply URL.                                                           |
