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

# Accounts API Examples

<details>

<summary>Show the most recent 10 6551 accounts on ethereum, polygon, and base</summary>

```graphql
query MyQuery {
  ethereumAccounts: Accounts(
    input: {
      filter: { standard: { _eq: ERC6551 } }
      blockchain: ethereum
      order: { createdAtBlockTimestamp: DESC }
      limit: 10
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
    }
  }
  polygonAccounts: Accounts(
    input: {
      filter: { standard: { _eq: ERC6551 } }
      blockchain: polygon
      order: { createdAtBlockTimestamp: DESC }
      limit: 10
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
    }
  }
  baseAccounts: Accounts(
    input: {
      filter: { standard: { _eq: ERC6551 } }
      blockchain: base
      order: { createdAtBlockTimestamp: DESC }
      limit: 10
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
    }
  }
}
```

</details>

<details>

<summary>Show me all @Sapienz ERC6551 accounts and their balances</summary>

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: {
        tokenAddress: { _eq: "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36" }
        standard: { _eq: ERC6551 }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    Account {
      address {
        addresses
        tokenBalances {
          formattedAmount
          tokenAddress
          token {
            name
          }
        }
      }
      salt
      tokenAddress
      tokenId
      createdAtBlockTimestamp
      createdAtBlockNumber
    }
  }
}
```

</details>

<details>

<summary>Show all ERC65111 accounts owned by @BoredApeYachtClub and the token balances</summary>

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: {
        standard: { _eq: ERC6551 }
        tokenAddress: { _eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D" }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    Account {
      address {
        addresses
      }
      tokenAddress
      tokenId
      address {
        tokenBalances {
          tokenAddress
          amount
          token {
            name
          }
        }
      }
    }
  }
}
```

</details>

<details>

<summary>Show lens/@sapienz_0 ERC6551 accounts</summary>

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: { address: { _eq: "lens/@sapienz_0" } }
      blockchain: ethereum
      limit: 50
    }
  ) {
    Account {
      address {
        addresses
        primaryDomain {
          name
        }
      }
      salt
      tokenAddress
      tokenId
      createdAtBlockTimestamp
      createdAtBlockNumber
    }
  }
}
```

</details>

<details>

<summary>Show me 6551 accounts created on July 12, 2023 on Ethereum</summary>

```graphql
query MyQuery {
  Accounts(
    input: {
      filter: {
        standard: { _eq: ERC6551 }
        createdAtBlockTimestamp: {
          _gte: "2023-07-12T00:00:00Z"
          _lt: "2023-07-13T00:00:00Z"
        }
      }
      blockchain: ethereum
      limit: 50
    }
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
    }
  }
}
```

</details>
