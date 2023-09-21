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

{% hint style="info" %}
PoapEvents event API allows users to retrieve information about a particular POAP event, including the event token holders, metadata, and resized image. \
In addition, PoapEvents API allows in-depth filtering of the PAOPs based on the date the event took place or the location.
{% endhint %}

### Inputs & Filters

```graphql
input PoapEventFilter {
  dappName: # POAP Dapp Name
  dappSlug: # POAP Dapp Version
  eventId: # POAP event ID 
  eventName:# POAP event name - exact match only
  country: # POAP event country
  city: # POAP event city
  startDate: # POAP event start date YYYY-MM-DD
  endDate: # POAP event end date YYYY-MM-DD
  isVirtualEvent: # Boolean - yes or no
}
```

### Outputs

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
