---
description: >-
  FarcasterFrame type provides detailed Farcaster Frames information related to
  the casts information queried.
---

# FarcasterFrame

## Fields

| Name                | Type                                                         | Description                                          |
| ------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| `buttons`           | [`[FrameButton!]`](framebutton.md)                           | All the buttons that are returned by the Frame.      |
| `castedAtTimestamp` | `Time`                                                       | The casting time of the casts where the Frame is in. |
| `casts`             | [`[FarcasterCast!]`](../api-reference/farcastercasts-api.md) | **Nested Query** – Casts that contains the Frame.    |
| `frameHash`         | `String`                                                     | Frame Hash.                                          |
| `frameUrl`          | `String`                                                     | Frame URL.                                           |
| `id`                | `ID`                                                         | Airstack unique identifier for the data point.       |
| `imageAspectRatio`  | `String`                                                     | Frame's image aspect ratio.                          |
| `imageUrl`          | `String`                                                     | Frame's image URL.                                   |
| `inputText`         | `String`                                                     | Frame's input text.                                  |
| `postUrl`           | `String`                                                     | Frame's Post URL.                                    |
| `state`             | `String`                                                     | Frames state.                                        |
