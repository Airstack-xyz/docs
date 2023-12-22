---
description: >-
  Learn all the detailed references of TokenTransfers API that provide
  ERC20/721/1155 token transfer information, including the input filters,
  supported chains, and output fields.
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

# TokenTransfers API

The TokenTransfers API delivers information on token transfers between wallets & contracts.

## Inputs

### Filters

| Name              | Type                       | Description                                                                              |
| ----------------- | -------------------------- | ---------------------------------------------------------------------------------------- |
| `blockTimestamp`  | `Time_Comparator_Exp`      | Allows entering blockTimestamp to filter transactions which happened in specific periods |
| `formattedAmount` | `Float_Comparator_Exp`     | Allows filtering based on Balance amount in decimals, e.g. show me Balances above 200    |
| `from`            | `Identity_Comparator_Exp`  | Identity: blockchain address, domain name, social identity                               |
| `to`              | `Identity_Comparator_Exp`  | Identity: blockchain address, domain name, social identity                               |
| `tokenAddress`    | `Address_Comparator_Exp`   | Token contract address (ERC20, ERC721, ERC1155)                                          |
| `tokenId`         | `String_Comparator_Exp`    | Unique NFT token ID                                                                      |
| `tokenType`       | `TokenType_Comparator_Exp` | ERC20, ERC721, ERC1155                                                                   |
| `transactionHash` | `String_Comparator_Exp`    | Transaction hash of the Token Transfers                                                  |

### Blockchain

| Name       | Description      |
| ---------- | ---------------- |
| `ethereum` | Ethereum mainnet |
| `polygon`  | Polygon mainnet  |
| `base`     | Base mainnet     |

## Outputs

```graphql
type TokenTransfer {
amount: String # Amount transferred in the token transfer
amounts: [String] # ERC1155 batch transfer specific amounts
blockchain: Blockchain # Blockchain where the token transfer took place
blockNumber: Int # Block number of the token transfer
blockTimestamp: Time # Timestamp of the token transfer
chainId: String # Unique blockchain identifier
from:  # **Nested query** - wallet-related information, including address, domains, social profile, other token balances, and transfer history.
id: ID # Airstack unique identifier for the token transfer
operator:  # **Nested query** - operator (contract) wallet-related information, including address, domains, social profile, other token balances, and transfer history.
to:  # **Nested query** - wallet-related information, including address, domains, social profile, other token balances, and transfer history.
token: Token # **Nested query** - token contract level information
tokenAddress: Address # Token contract address
tokenId: String # Token ID associated with the token transfer, if applicable
tokenIds: [String] # List of token IDs associated with the token transfer, if applicable
tokenNft: TokenNft # **Nested query** - individual NFT token level data
tokenType: TokenType # Token type (ERC20, ERC721, or ERC1155)
transactionHash: String! # Transaction hash of the token transfer
type: String # Type of the token transfer
}
```

{% hint style="warning" %}
The TokenTransfers API only covers transfers of the "transfer" type and excludes "sale" transactions for NFT contracts.
{% endhint %}
