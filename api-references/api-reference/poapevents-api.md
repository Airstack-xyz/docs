---
description: >-
  Learn all the detailed references of PoapEvents API that provide POAP events
  detailed information, including the input filters, supported chains, and
  output fields.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# PoapEvents API

PoapEvents event API allows users to retrieve information about a particular POAP event, including the event token holders, metadata, and resized images.

In addition, PoapEvents API allows in-depth filtering of the PAOPs based on the date the event took place or the location.

## Inputs

### filter

| Name             | Type                          | Description                                                                                                       |
| ---------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `dappName`       | `PoapDappName_Comparator_Exp` | <p>POAP Dapp Name:<br>- <code>poap_mainnet</code>: Ethereum POAPs<br>- <code>poap_gnosis</code>: Gnosis POAPs</p> |
| `dappSlug`       | `PoapDappSlug_Comparator_Exp` | POAP Dapp Version                                                                                                 |
| `eventId`        | `String_Comparator_Exp`       | POAP event ID                                                                                                     |
| `tokenMints`     | `Int_Comparator_Exp`          | Number of POAPs minted                                                                                            |
| `eventName`      | `Regex_String_Comparator_Exp` | POAP event name                                                                                                   |
| `country`        | `String_Comparator_Exp`       | POAP event country                                                                                                |
| `city`           | `String_Comparator_Exp`       | POAP event city                                                                                                   |
| `startDate`      | `String_Comparator_Exp`       | POAP event start date YYYY-MM-DD                                                                                  |
| `endDate`        | `String_Comparator_Exp`       | POAP event end date YYYY-MM-DD                                                                                    |
| `isVirtualEvent` | `Boolean_Comparator_Exp`      | Boolean - yes or no                                                                                               |

### blockchain

{% hint style="info" %}
For **PoapEvents** API, it will return POAPs data from both Ethereum and Gnosis mainnet.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description                                                                                                                             |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `ALL` | Both Ethereum & Gnosis Mainnet. To fetch POAPs data from individual chains, check [`dappName`](poapevents-api.md#filters) filter input. |

## Outputs

| Name             | Type                    | Description                                                                                            |
| ---------------- | ----------------------- | ------------------------------------------------------------------------------------------------------ |
| `id`             | `ID`                    | Airstack unique identifier for the data point.                                                         |
| `chainId`        | `String`                | Unique identifier for the blockchain.                                                                  |
| `blockchain`     | `EveryBlockchain`       | Blockchain where the marketplace data is calculated from.                                              |
| `dappName`       | `PoapDappName`          | This will return `poap` value.                                                                         |
| `dappSlug`       | `PoapDappSlug!`         | Indicate if the POAP is deployed on Ethereum mainnet (`poap_mainnet`) or Gnosis chain (`poap_gnosis`). |
| `dappVersion`    | `String`                | Indicate if the POAP is deployed on Ethereum mainnet (`mainnet`) or Gnosis chain (`gnosis`).           |
| `eventId`        | `String`                | POAP event ID.                                                                                         |
| `tokenMints`     | `Int`                   | Number of POAPs minted for this event ID.                                                              |
| `metadata`       | `Map`                   | POAP event metadata.                                                                                   |
| `contentType`    | `String`                | POAP Event token content (e.g. image/audio).                                                           |
| `contentValue`   | `String`                | POAP Event resized images.                                                                             |
| `eventName`      | `String`                | POAP event name.                                                                                       |
| `description`    | `String`                | POAP event description.                                                                                |
| `country`        | `String`                | The country where a POAP event took place, if any.                                                     |
| `city`           | `String`                | The city where a POAP event took place, if any.                                                        |
| `startDate`      | `Time`                  | The date when the POAP event started.                                                                  |
| `endDate`        | `Time`                  | The date when the POAP even ended.                                                                     |
| `isVirtualEvent` | `Boolean`               | Indicate if the POAP event is Virtual (Online) or not.                                                 |
| `eventURL`       | `String`                | The Event URL.                                                                                         |
| `poaps`          | [`Poaps`](poaps-api.md) | \*\*Nested query\*\* fetch information about the POAP's holders.                                       |
