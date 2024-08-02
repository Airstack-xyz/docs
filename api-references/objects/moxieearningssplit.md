---
description: >-
  MoxieEarningsSplit type provides detailed information on how the Moxie
  earnings on a cast is split between caster, member fans (if any), channel fans
  (if any), and the Farcaster network.
---

# MoxieEarningsSplit

## Fields

| Name                  | Value                                  | Description                                          |
| --------------------- | -------------------------------------- | ---------------------------------------------------- |
| `earnerType`          | [`EarnerType!`](../enum/earnertype.md) | The type of earner that earn Moxie.                  |
| `earningsAmount`      | `Float`                                | How much Moxie is earned on the cast.                |
| `earningsAmountInWei` | `String`                               | How much Moxie is earned on the cast in wei (10^18). |
