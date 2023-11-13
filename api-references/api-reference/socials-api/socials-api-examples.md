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

# Socials API Examples

<details>

<summary>Get the Ethereum connected address for Farcaster user betashop, along with the profile creation date and recovery address</summary>

```graphql
query MyQuery {
  Socials(
    input: {
      filter: { profileName: { _eq: "fc_fname:betashop" } }
      blockchain: ethereum
    }
  ) {
    Social {
      userAssociatedAddresses
      userCreatedAtBlockNumber
      userCreatedAtBlockTimestamp
      userRecoveryAddress
    }
  }
}
```

</details>

<details>

<summary>Check if this Ethereum address "0x74232bf61e994655592747e20bdf6fa9b9476f79" has a Farcaster account</summary>

```graphql
query MyQuery {
  Socials(
    input: {
      filter: {
        userAssociatedAddresses: {
          _eq: "0x74232bf61e994655592747e20bdf6fa9b9476f79"
        }
      }
      blockchain: ethereum
    }
  ) {
    Social {
      profileCreatedAtBlockNumber
      profileCreatedAtBlockTimestamp
      profileName
      profileTokenId
      profileTokenUri
      userId
    }
  }
}
```

</details>

<details>

<summary>For Farcaster user betashop, lens/@shnoodles, vitalik.eth show their user id and registration date</summary>

```graphql
query MyQuery {
  betashop: Socials(
    input: {
      filter: { identity: { _eq: "fc_fname:betashop" } }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      userId
      userCreatedAtBlockTimestamp
    }
  }
  shnoodlesLens: Socials(
    input: {
      filter: { identity: { _eq: "lens/@shnoodles" } }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      userId
      userCreatedAtBlockTimestamp
    }
  }
  vitalikEth: Socials(
    input: {
      filter: { identity: { _eq: "vitalik.eth" } }
      blockchain: ethereum
    }
  ) {
    Social {
      dappName
      userId
      userCreatedAtBlockTimestamp
    }
  }
}
```

</details>
