---
description: >-
  Learn how to use Airstack to get token-bound account results sorted in
  ascending or descending order by the creation block timestamp.
---

# ⏫ Sort Results

## Get The Latest Token Bound Accounts Created

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {order: {createdAtBlockTimestamp: DESC}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
        blockchain
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
      updatedAtBlockNumber
      updatedAtBlockTimestamp
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Accounts": {
      "Account": [
        {
          "id": "a7ed516e2a9c49f1f18a7a62adabf08181f6ca6b1d2dc35a890df257ebeb02ff",
          "standard": "ERC6551",
          "blockchain": "ethereum",
          "tokenAddress": "0x51e613727fdd2e0b91b51c3e5427e9440a7957e4",
          "tokenId": "13015035",
          "address": {
            "identity": "0x0d6eb86ed9231e4ff679367c24480e79e452834d",
            "blockchain": "ethereum"
          },
          "registry": "0x02101dfb77fde026414827fdc604ddaf224f0921",
          "implementation": "0x2d25602551487c3f3354dd80d76d54383a243358",
          "salt": "0",
          "createdAtBlockNumber": 17681775,
          "createdAtBlockTimestamp": "2023-07-13T02:57:59Z",
          "creationTransactionHash": "0xe017bbb147e3e6c345e41920aafd0cf5304863d93f606f2d192679f0299ca242",
          "deployer": "0x90bade35da052450b01e99b38cbb550bc3f1dd58",
          "updatedAtBlockNumber": 17681775,
          "updatedAtBlockTimestamp": "2023-07-13T02:57:59Z"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Get The Earliest Token Bound Accounts Created

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Accounts(
    input: {order: {createdAtBlockTimestamp: DESC}, blockchain: ethereum, limit: 200}
  ) {
    Account {
      id
      standard
      blockchain
      tokenAddress
      tokenId
      address {
        identity
        blockchain
      }
      registry
      implementation
      salt
      createdAtBlockNumber
      createdAtBlockTimestamp
      creationTransactionHash
      deployer
      updatedAtBlockNumber
      updatedAtBlockTimestamp
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Accounts": {
      "Account": [
        {
          "id": "37130b5894303a6d57d28bed00daa5583acc9fd0e2e475be825f65af765b76e4",
          "standard": "ERC6551",
          "blockchain": "ethereum",
          "tokenAddress": "0x26727ed4f5ba61d3772d1575bca011ae3aef5d36",
          "tokenId": "0",
          "address": {
            "identity": "0x5416e5dc14caa0950b2a24ede1eb0e97c360bcf5",
            "blockchain": "ethereum"
          },
          "registry": "0x02101dfb77fde026414827fdc604ddaf224f0921",
          "implementation": "0x2d25602551487c3f3354dd80d76d54383a243358",
          "salt": "0",
          "createdAtBlockNumber": 17213826,
          "createdAtBlockTimestamp": "2023-05-08T05:53:59Z",
          "creationTransactionHash": "0xf998cb400eebe89aa1d369792daf4c202be74dcd88981b34eb49d510b7837332",
          "deployer": "0xa75b7833c78eba62f1c5389f811ef3a7364d44de",
          "updatedAtBlockNumber": 17213826,
          "updatedAtBlockTimestamp": "2023-05-08T05:53:59Z"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}
