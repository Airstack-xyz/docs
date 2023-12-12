---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# PoapEvents API

PoapEvents event API allows users to retrieve information about a particular POAP event, including the event token holders, metadata, and resized image.

In addition, PoapEvents API allows in-depth filtering of the PAOPs based on the date the event took place or the location.

## Inputs

### Filters

| Name             | Type                          | Description                                                                                                       |
| ---------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `dappName`       | `PoapDappName_Comparator_Exp` | <p>POAP Dapp Name:<br>- <code>poap_mainnet</code>: Ethereum POAPs<br>- <code>poap_gnosis</code>: Gnosis POAPs</p> |
| `dappSlug`       | `PoapDappSlug_Comparator_Exp` | POAP Dapp Version                                                                                                 |
| `eventId`        | `String_Comparator_Exp`       | POAP event ID                                                                                                     |
| `tokenMints`     | `Int_Comparator_Exp`          | Number of POAPs minted                                                                                            |
| `eventName`      | `String_Comparator_Exp`       | POAP event name - exact match only                                                                                |
| `country`        | `String_Comparator_Exp`       | POAP event country                                                                                                |
| `city`           | `String_Comparator_Exp`       | POAP event city                                                                                                   |
| `startDate`      | `String_Comparator_Exp`       | POAP event start date YYYY-MM-DD                                                                                  |
| `endDate`        | `String_Comparator_Exp`       | POAP event end date YYYY-MM-DD                                                                                    |
| `isVirtualEvent` | `Boolean_Comparator_Exp`      | Boolean - yes or no                                                                                               |

### Blockchain

{% hint style="info" %}
For **PoapEvents** API, it will return POAPs data from both Ethereum and Gnosis mainnet.

You just need to specify the input to `ALL` for the query to work.
{% endhint %}

| Enum  | Description                                                                                                                             |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `ALL` | Both Ethereum & Gnosis Mainnet. To fetch POAPs data from individual chains, check [`dappName`](poapevents-api.md#filters) filter input. |

## Outputs

```graphql
type PoapEvent {
  id: # Airstack unique element identifier
  chainId: # Blockchain ID 
  blockchain: # Blockchain name
  dappName: 
  dappSlug: 
  dappVersion: 
  eventId: # POAP Event ID
  metadata: # POAP Event metadata
  contentType: # POAP Event token content (e.g. image/audio)
  contentValue: # POAP Event resized images
  eventName: # POAP Event name
  description: # POAP Event description
  country: # POAP Event country
  city: # POAP Event city
  startDate:
  endDate: 
  isVirtualEvent: 
  eventURL: # POAP Event website
  poaps: # **Nested query** fetch information about the e
}
```
