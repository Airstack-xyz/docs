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

# PoapEvent API Examples

<details>

<summary>show  @DevCon4 details</summary>

```graphql
query MyQuery {
  PoapEvents(input: {filter: {eventId: {_eq: "6"}}, blockchain: ALL}) {
    PoapEvent {
      eventName
      eventId
      startDate
      endDate
      country
      city
      isVirtualEvent
      contentValue {
        image {
          extraSmall
          large
          medium
          original
          small
        }
      }
    }
  }
}
```

</details>

<details>

<summary>show me all events which took place in USA in 2022</summary>

```graphql
query MyQuery {
  PoapEvents(input: {filter: {country: {_eq: "USA"}, startDate: {_gte: "2022-01-01"}, endDate: {_lte: "2022-12-31"}}, blockchain: ALL, limit: 10}) {
    PoapEvent {
      eventName
      eventId
      startDate
      endDate
      country
      city
      isVirtualEvent
      contentValue {
        image {
          extraSmall
          large
          medium
          original
          small
        }
      }
    }
  }
}
```

</details>
