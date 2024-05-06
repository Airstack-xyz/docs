---
description: >-
  Learn all the detailed references of FarcasterReactions API that provide
  Farcaster casts information, including the input filters and output fields.
---

# FarcasterReactions API

The FarcasterReactions API enables you to fetch a list of all reactions, which includes likes, replies, and recasts, on Farcaster based on various filters, including reactor's, cast hash, cast's URL, and Frame URL, if any.

## Inputs

### filters

| Name        | Type                                                                 | Description                                                                                                                                                                                   |
| ----------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `castHash`  | `String_Eq_In_Comparator_Exp`                                        | Filter by cast hash.                                                                                                                                                                          |
| `castUrl`   | `String_Eq_In_Comparator_Exp`                                        | Filter by cast URL.                                                                                                                                                                           |
| `criteria`  | [`FarcasterReactionCriteria!`](../enum/farcasterreactioncriteria.md) | Filter by different criteria selection, including `like`, `recast`, and `replied`.                                                                                                            |
| `frameUrl`  | `String_Eq_In_Comparator_Exp`                                        | Filter by Frames URL.                                                                                                                                                                         |
| `reactedBy` | `Identity_Comparator_Exp`                                            | <p>Filter by cast reactor's web3 identities, either 0x address, Farcaster, ENS, or Lens.<br><br>For more details, check out <a href="airstack-identity-api.md">Airstack Identity API</a>.</p> |

### blockchain

{% hint style="info" %}
For **FarcasterReactions** API, it will return all Farcaster reactions.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description |
| ----- | ----------- |
| `ALL` | -           |

## Outputs



| Name                 | Type                                                                | Description                                                                                      |
| -------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `Criteria`           | [`FarcasterReactionCriteria`](../enum/farcasterreactioncriteria.md) | The chosen criteria to filter the reaction selection, including `like`, `recast`, and `replied`. |
| `Reaction.cast`      | [`FarcasterCast`](farcastercasts-api.md)                            | **Nested Query** – Farcaster cast details with the specified reaction criteria.                  |
| `Reaction.castHash`  | `String`                                                            | Farcaster cast hash                                                                              |
| `Reaction.reactedBy` | [`Social`](socials-api.md)                                          | **Nested Query** – Reactor's Farcaster profile details                                           |
