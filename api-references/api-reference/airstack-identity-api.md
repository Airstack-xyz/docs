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

# Airstack Identity API

Airstack Identity API has been integrated throughout all of the GraphQL queries!

You can filter your queries using the identity inputs by passing the following values instead of a blockchain address:

## **ENS** Domains

You can enter the [ENS domains](https://ens.domains), both on-chain or off-chain (e.g. [cb.id](https://cb.id) or [Namestone](https://namestone.xyz)), directly in the `owner` or `address` field:

| Sample Input                                                | Description                            |
| ----------------------------------------------------------- | -------------------------------------- |
| <mark style="color:red;">`vitalik.eth`</mark>               | Plain ENS domain ending in `.eth`      |
| <mark style="color:red;">`ens:vitalik.eth`</mark>           | ENS domain with optional `ens:` prefix |
| <mark style="color:red;">`leighton.pooltogether.eth`</mark> | Off-chain ENS domain from Namestone    |
| <mark style="color:red;">`yosephks.cb.id`</mark>            | Coinbase ID that is resolved off-chain |

## **Farcaster** ID and Name

You can enter Farcaster ID or name directly in the `owner` or `address` field:

| Sample Input                                        | Description                                                      |
| --------------------------------------------------- | ---------------------------------------------------------------- |
| <mark style="color:red;">`fc_fid:5650`</mark>       | Farcaster user ID with required `fc_fid` prefix                  |
| <mark style="color:red;">`fc_fname:vbuterin`</mark> | Farcaster user name (not ENS) with required `fc_fname` prefix    |
| <mark style="color:red;">`fc_fname:dwr.eth`</mark>  | Farcaster user name (ENS domain) with required `fc_fname` prefix |

Alternatively, you can enter the Farcaster profile name directly in the Socials `profileName` field.

{% hint style="info" %}
You can enter all fnames that Farcaster user has, including the one that is not actively used as a profile name.\
\
For example, a Farcaster user have multiple fnames: `fc_fname:varunsrin.eth` and `fc_fname:v`, then you should be able to use both as an input of any identity field.
{% endhint %}

### Example #1: Show me all NFTs and their images currently being held by Farcaster user name dwr.eth

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/a36uY0h17f" %}
Show me all NFTs and their images currently being held by Farcaster user name dwr.eth
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query ID1 {
  TokenBalances(input: {filter: {owner: {_eq: "fc_fname:dwr"}, tokenType: {_in: [ERC721, ERC1155]}}, blockchain: ethereum}) {
    TokenBalance {
      tokenNfts {
        address
        tokenId
        contentValue {
          image {
            medium
          }
        }
        token {
          name
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "tokenNfts": {
            "address": "0x0082578eedfd01ec97c36165469d012d6dc257cc",
            "tokenId": "7850128130671533894652765198397141493935721251300528902294133048226415902660",
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/dyFDo9ARD/t3yZOoy8Nq6oz4FNp4dRX6VXG4VO0/03HaEqfk4SWkzwaoqwPf9zzeAMN3X6nPI14uWP4b3n3U2HyNMwEoWkP9mdVxs7ucEyJZPx9uhZV7T6PY6eXVQs9f/RnbxASeXrJEfO1gNdnhQ3osADcjz/uOzleq7iMIjrU=/medium"
              }
            },
            "token": {
              "name": "Infinity"
            }
          }
        },
        {
          "tokenNfts": {
            "address": "0x0082578eedfd01ec97c36165469d012d6dc257cc",
            "tokenId": "14552839248952988946013811921776163857135064430444296228138892752085753508861",
            "contentValue": {
              "image": {
                "medium": "https://assets.airstack.xyz/image/nft/dyFDo9ARD/t3yZOoy8Nq6oz4FNp4dRX6VXG4VO0/03HlqVAl7WyJwqX9exhdRK1x9p4vwD/KxafdE0FN1R6gVHTIV4h3vBN2tVPR6XhPCjOqlr1nXo6vGM0Dd6BDaWeeib7G85tKSYloVz1ykZL7OMAENxdJQrmPx6mKXqkdOn0=/medium"
              }
            },
            "token": {
              "name": "Infinity"
            }
          }
        },
        {
          "tokenNfts": {
            "address": "0x2aaa54c5c7651a4e37f6b7095103381b9c0c31e6",
            "tokenId": "7",
            "contentValue": {
              "image": null
            },
            "token": {
              "name": "Remix Force"
            }
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Lens Profile Name and ID

You can enter Lens name directly in the `owner` or `address` field:

| Sample Input                                        | Description                                                          |
| --------------------------------------------------- | -------------------------------------------------------------------- |
| <mark style="color:red;">`vitalik.lens`</mark>      | Plain Lens profile name ending in `.lens`                            |
| <mark style="color:red;">`lens:vitalik.lens`</mark> | Lens profile name with optional `lens` prefix                        |
| <mark style="color:red;">`lens_id:100275`</mark>    | Lens profile ID in decimal format with optional `lens_id` prefix     |
| <mark style="color:red;">`lens_id:0x0187b3`</mark>  | Lens profile ID in hexadecimal format with optional `lens_id` prefix |

### Example#1: Show me all stani.lens token transfers

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/a76kjK1seY" %}
Show me all stani.lens token transfers
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query tokenTransfers {
  TokenTransfers(
    input: {
      filter: {
        _or: [
          { from: { _eq: "stani.lens" } },
          { to: { _eq: "stani.lens" } }
        ]
      },
      blockchain: ethereum
    }
  ) {
    TokenTransfer {
      amount
      tokenType
      tokenAddress
      tokenId
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenTransfers": {
      "TokenTransfer": [
        {
          "amount": "1",
          "tokenType": "ERC721",
          "tokenAddress": "0x054460490780a6eb15625d703db1754e0b78d846",
          "tokenId": "34389"
        },
        {
          "amount": "1",
          "tokenType": "ERC721",
          "tokenAddress": "0x054460490780a6eb15625d703db1754e0b78d846",
          "tokenId": "34411"
        },
        {
          "amount": "1",
          "tokenType": "ERC721",
          "tokenAddress": "0x054460490780a6eb15625d703db1754e0b78d846",
          "tokenId": "34399"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}
