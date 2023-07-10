# Domains API Examples

<details>

<summary>Get registration data and purchase price for balajis.eth</summary>

```graphql
query MyQuery {
  Domains(input: {filter: {name: {_eq: "balajis.eth"}}, blockchain: ethereum}) {
    Domain {
      registrationCost
      paymentTokenCostInUSDC
      registrationCostInUSDC
      registrationCostInNativeToken
      resolvedAddress
      isPrimary
      name
      owner
      paymentToken {
        address
        chainId
        decimals
        lastTransferBlock
        lastTransferHash
        name
        tokenBalances {
          amount
          blockchain
        }
      }
    }
  }
}
```

</details>

<details>

<summary>Show me all domains shnoodles.lens owns</summary>

```graphql
query domainsOwned {
  Domains(input: {filter: {owner: {_eq: "shnoodles.lens"}}, blockchain: ethereum}) {
    Domain {
      name
      blockchain
      dappName
    }
  }
}
```

</details>
