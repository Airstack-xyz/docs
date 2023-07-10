# POAPs API Examples

<details>

<summary>Show who attended @ETHLisbon 2022 and their social profiles</summary>

```graphql
query AttendeesAndSocialProfiles {
  Poaps(
    input: {filter: {eventId: {_eq: "76949"}}, blockchain: ALL, limit: 10}
  ) {
    Poap {
      owner {
        identity
        primaryDomain {
          name
        }
        socials {
          profileName
          userAddress
          userAssociatedAddresses
        }
      }
    }
  }
}
```

</details>

<details>

<summary>what events did patricio.eth attended?</summary>

```graphql
query POAPsPatricioEth {
  Poaps(
    input: {filter: {owner: {_eq: "patricio.eth"}}, blockchain: ALL, limit: 10}) 
    {
    Poap {
      eventId
      poapEvent {
        eventName
        eventURL
        startDate
        endDate
        country
        city
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
}
```

</details>
