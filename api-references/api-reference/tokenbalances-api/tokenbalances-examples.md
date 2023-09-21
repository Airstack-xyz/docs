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

# TokenBalances Examples

<details>

<summary>Get all tokens held by wallet address 0x540cb04ebab67e05a620b97bb367ac5e4ed68f09</summary>

```graphql
query QB5 {
  TokenBalances(input: {filter: { owner: {_eq: "0x540cb04ebab67e05a620b97bb367ac5e4ed68f09"}}, limit: 10, blockchain: ethereum}) {
    TokenBalance {
      amount
      chainId
      id
      lastUpdatedBlock
      lastUpdatedTimestamp
      owner {
        addresses
      }
      tokenAddress
      tokenId
      tokenType
      token {
        name
        symbol
      }
    }
  }
}
```

</details>

<details>

<summary>Get all tokens held by vitalik.eth</summary>

```graphql
query QB5 {
  TokenBalances(input: {filter: { owner: {_eq: "vitalik.eth"}}, limit: 10, blockchain: ethereum}) {
    TokenBalance {
      amount
      chainId
      id
      lastUpdatedBlock
      lastUpdatedTimestamp
      owner {
        addresses
      }
      tokenAddress
      tokenId
      tokenType
      token {
        name
        symbol
      }
    }
  }
}
```

</details>

<details>

<summary>For this list of ENS addresses, show me all NFTs they own, including images: dwr.eth, vitalik.eth, balajis.eth</summary>

```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["dwr.eth","vitalik.eth","balajis.eth"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum}
  ) {
    TokenBalance {
      amount
      tokenId
      tokenAddress
      tokenNfts {
        contentValue {
          image {
            medium
          }
        }
      }
      token {
        name
        symbol
      }
    }
  }
}
```

</details>

<details>

<summary>For this list of Farcaster users, show how many all NFTs they own, including images: dwr, vitalik, balajis</summary>

```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {owner: {_in: ["fc_fname:v","fc_fname:dwr","fc_fname:balajis"]}, tokenType: {_in: [ERC1155, ERC721]}}, blockchain: ethereum, limit: 50}
  ) {
    TokenBalance {
      tokenAddress
      amount
      tokenType
      owner {
        addresses
        socials {
          profileName
          userAssociatedAddresses
        }
      }
    }
  }
}
```

</details>

<details>

<summary>Show all wallets holding @BAYC and their Lens usernames</summary>

```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0xBC4CA0EdA7647A8aB7C2061c2E118A18a936f13D"}}, blockchain: ethereum, limit: 10}
  ) {
    TokenBalance {
      owner {
        addresses
        socials {
          profileName
        }
      }
    }
  }
}
```

</details>

<details>

<summary>Get all the token holders and their ENS names for Nouns DAO</summary>

```graphql
{
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x9C8fF314C9Bc7F6e59A9d9225Fb22946427eDC03"}}, limit: 10, blockchain: ethereum}
  ) {
    TokenBalance {
      owner {
        addresses
        primaryDomain {
          name
        }
        domains {
          name
        }
      }
    }
  }
}
```

</details>

<details>

<summary>For the Ethereum token Matic, show me all token holders and the NFTs they hold</summary>

```graphql
{
  TokenBalances(
    input: {blockchain: ethereum, filter: {tokenAddress: {_eq: "0x7D1AfA7B718fb893dB30A3aBc0Cfc608AaCfeBB0"}}, limit: 10}
  ) {
    TokenBalance {
      owner {
        identity
        addresses
        tokenBalances {
          tokenNfts {
            tokenId
            contentType
          }
        }
      }
    }
  }
}

```

</details>
