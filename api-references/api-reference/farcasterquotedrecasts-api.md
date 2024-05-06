---
description: >-
  Learn all the detailed references of FarcasterQuotedRecasts API that provide
  Farcaster casts information, including the input filters and output fields.
---

# FarcasterQuotedRecasts API

The FarcasterQuotedRecasts API enables you to fetch a list of all quoted recasts based on various filters, including quoted recasters, quoted recast hash, quoted recast's parent hash, and quoted recast's URL.

## Inputs

### filters

| Name             | Type                      | Description                                                                                                                                                                                    |
| ---------------- | ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `parentCastedBy` | `Identity_Comparator_Exp` | <p>Filter by parent caster's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p> |
| `parentHash`     | `String_Eq_In_Comparator` | Filter by parent cast hash.                                                                                                                                                                    |
| `parentUrl`      | `String_Eq_In_Comparator` | Filter by parent cast URL.                                                                                                                                                                     |
| `recastedBy`     | `Identity_Comparator_Exp` | <p>Filter by recaster's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p>      |

### blockchain

{% hint style="info" %}
For **FarcasterQuotedRecasts** API, it will return all Farcaster quoted recasts.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

## Outputs

| Name                 | Type                                                     | Description                                                                              |
| -------------------- | -------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `castedAtTimestamp`  | `Time`                                                   | Time when the quoted recast was casted.                                                  |
| `castedBy`           | [`Social`](socials-api.md)                               | **Nested Query** – Quoted recaster details.                                              |
| `channel`            | [`FarcasterChannel`](farcasterchannels-api.md)           | **Nested Query** – Farcaster Channel where the quote recast is casted.                   |
| `embeds`             | `[Map]`                                                  | Embeds contained in the quote recasts.                                                   |
| `fid`                | `String`                                                 | Quoted recaster's FID.                                                                   |
| `frame`              | [`FarcasterFrame`](../objects/farcasterframe.md)         | **Nested Query** – Farcaster Frame attached to the quoted recast.                        |
| `hash`               | `String`                                                 | Quoted recast Hash.                                                                      |
| `id`                 | `ID`                                                     | Airstack unique identifier for the data point.                                           |
| `mentions`           | `[Mentions]`                                             | **Nested Query** – Farcaster profiles that like that are mentioned in the quoted recast. |
| `numberOfLikes`      | `Int`                                                    | Number of likes on the quoted recast.                                                    |
| `numberOfRecasts`    | `Int`                                                    | Number of recasts on the quoted recast.                                                  |
| `numberOfReplies`    | `Int`                                                    | Number of replies on the quoted recast.                                                  |
| `parentCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Quoted recast's parent details.                                       |
| `parentFid`          | `String`                                                 | Quoted recast's parent FID.                                                              |
| `parentHash`         | `String`                                                 | Quoted recast's parent hash.                                                             |
| `parentUrl`          | `String`                                                 | Quoted recast's parent URL.                                                              |
| `quotedCast`         | [`FarcasterCast`](farcastercasts-api.md)                 | **Nested Query** – Quoted recast's details of the quoted recast.                         |
| `rawText`            | `String`                                                 | Raw text contained in the quoted recast.                                                 |
| `recastedBy`         | [`[Social]`](socials-api.md)                             | **Nested Queries** – List of all Farcaster profiles recasted the quoted recast.          |
| `rootParentHash`     | `String`                                                 | The root hash of the quoted recast, if any.                                              |
| `rootParentUrl`      | `String`                                                 | The root URL of the quoted recast, if any.                                               |
| `socialCapitalValue` | [`SocialCapitalValue`](../objects/socialcapitalvalue.md) | **Nested Query** – The social capital value details of a given Farcaster quoted recast.  |
| `text`               | `String`                                                 | The text content of the quoted recast.                                                   |
| `url`                | `String`                                                 | Warpcast's Quoted Recast URL.                                                            |
