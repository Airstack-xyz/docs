---
description: >-
  Learn how to get all Lens profiles who own a specific token, NFT, or POAP, or
  a min amount of that token. Get combinations of NFTs or POAPs + Lens, e.g. Has
  POAP1 and POAP2 and has Lens profile
---

# 🥇 Token Holders

## Topics

* [Get Holders of an ERC20 Token That Has Lens Profile](token-holders.md#get-holders-of-an-erc20-token-that-has-lens-profile)
* [Get Holders of NFT That Has Lens Profile](token-holders.md#get-holders-of-nft-that-has-lens-profile)
* [Get Holders of POAP That Has Lens Profile](token-holders.md#get-holders-of-poap-that-has-lens-profile)
* [Get Holders That Held Specific Amount of ERC20 Token](token-holders.md#get-holders-that-held-specific-amount-of-erc20-token)
* [Get Common Holders of 2 ERC20 Tokens That Have Lens Profile](token-holders.md#get-common-holders-of-2-erc20-tokens-that-have-lens-profile)
* [Get Common Holders of Two POAPs That Has Lens Profile](token-holders.md#get-common-holders-of-two-poaps-and-that-has-lens-profile)
* [Get Common Holders of A Token (ERC20 or NFT) and A POAP That Has a Lens Profile](token-holders.md#get-common-holders-of-a-token-erc20-or-nft-and-a-poap-that-has-lens-profile)

## Get Holders of an ERC20 Token That Has Lens Profile

### Fetching

You can get all holders of an ERC20 token that has a Lens Profile:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/jwb232gKHF" %}
Show all token holders of USD Coin on Polygon that has Lens profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x2791bca1f2de4661ed88a30c99a7a9449aa84174"}}, blockchain: polygon, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
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
          "owner": {
            "socials": [
              {
                "profileName": "lyska.lens",
                "userId": "0x79eb30ff39ad0d2879ee1c0358170972e808cf7a",
                "userAssociatedAddresses": [
                  "0x79eb30ff39ad0d2879ee1c0358170972e808cf7a"
                ]
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "lyska.lens",
    "userId": "0x79eb30ff39ad0d2879ee1c0358170972e808cf7a",
    "userAssociatedAddresses": [
      "0x79eb30ff39ad0d2879ee1c0358170972e808cf7a"
    ]
  },
  // ... other token holders
]
```

## Get Holders of NFT That Has Lens Profile

### Fetching

You can get all holders of NFT that has Lens Profile:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/gIOrRVPFVK" %}
Show all token holders of StandWithCrypto that has Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {filter: {tokenAddress: {_eq: "0x9d90669665607f08005cae4a7098143f554c59ef"}}, blockchain: ethereum, limit: 200}
  ) {
    TokenBalance {
      owner {
        socials(
          input: {filter: {dappName: {_eq: lens}}}
        ) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```graphql
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "choccy.lens",
                "userId": "0x9a41abee1477745ab8004ce129ad60f1231ef85b",
                "userAssociatedAddresses": [
                  "0x9a41abee1477745ab8004ce129ad60f1231ef85b"
                ]
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "choccy.lens",
    "userId": "0x9a41abee1477745ab8004ce129ad60f1231ef85b",
    "userAssociatedAddresses": [
      "0x9a41abee1477745ab8004ce129ad60f1231ef85b"
    ]
  }
  // ... other token holders
]
```

## Get Holders of POAP That Has Lens Profile

### Fetching

You can get all holders of POAP that has Lens Profile:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/6JBkrFm6Xn" %}
Show all POAP holders of EthCC\[6] - Attendee that has Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Poaps(input: {filter: {eventId: {_eq: "141910"}}, blockchain: ALL, limit: 50}) {
    Poap {
      owner {
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```graphql
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "socials": [
              {
                "profileName": "0x131.lens",
                "userId": "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
                "userAssociatedAddresses": [
                  "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"
                ]
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'Poaps' in data and 'Poap' in data['Poaps']:
        for item in data['Poaps']['Poap']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result)) 

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "0x131.lens",
    "userId": "0x4455951fa43b17bd211e0e8ae64d22fb47946ade",
    "userAssociatedAddresses": [
      "0x4455951fa43b17bd211e0e8ae64d22fb47946ade"
    ]
  },
  // ... other token holders
]
```

## Get Holders That Held Specific Amount of ERC20 Token

### Fetching

You can get all holders of an ERC20 token that have a minimum amount held in their balances which also have Lens Profile:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/xW4iILym4e" %}
Show all user that has at least 10 USD Coin with their Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  TokenBalances(
    input: {
      filter: {
        tokenAddress: {_eq: "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"}, 
        formattedAmount: {_gte: 10}
      }, 
      blockchain: ethereum, 
      limit: 200
    }
  ) {
    TokenBalance {
      owner {
        identity
        socials(input: {filter: {dappName: {_eq: lens}}}) {
          profileName
          userId
          userAssociatedAddresses
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```graphql
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "identity": "0xd034fd34eaee5ec2c413c51936109e12873f4da5",
            "socials": [
              {
                "profileName": "monkeyflower.lens",
                "userId": "0xd034fd34eaee5ec2c413c51936109e12873f4da5",
                "userAssociatedAddresses": [
                  "0xd034fd34eaee5ec2c413c51936109e12873f4da5"
                ]
              }
            ]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.socials?.length > 0 ? owner?.socials : null
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'socials' in item['owner'] and len(item['owner']['socials']) > 0:
                result.append(item['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "monkeyflower.lens",
    "userId": "0xd034fd34eaee5ec2c413c51936109e12873f4da5",
    "userAssociatedAddresses": [
      "0xd034fd34eaee5ec2c413c51936109e12873f4da5"
    ]
  }
  // ... other token holders
]
```

## Get Common Holders of 2 ERC20 Tokens That Have Lens Profile

### Fetching

You can fetch the common holders of two given ERC20, e.g. [ApeCoin](https://explorer.airstack.xyz/token-holders?address=0x4d224452801aced8b2f0aebe155379bb5d594381\&rawInput=0x4d224452801aced8b2f0aebe155379bb5d594381\&inputType=ADDRESS\&tokenType=ERC20) and [USDC](https://explorer.airstack.xyz/token-holders?address=0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48\&blockchain=ethereum\&rawInput=%23%E2%8E%B1USD+Coin%E2%8E%B1%280xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48+TOKEN+ethereum+null%29+\&inputType=ADDRESS):

#### Try Demo

{% embed url="https://app.airstack.xyz/DTyOZg/tRf4wsoyrD" %}
Show all common holders of ApeCoin and USDC that has Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query GetCommonHoldersOfApeCoinAndUSDC {
  TokenBalances(input: {filter: {tokenAddress: {_eq: "0x4d224452801aced8b2f0aebe155379bb5d594381"}}, blockchain: ethereum, limit: 200}) {
    TokenBalance {
      owner {
        tokenBalances(input: {filter: {tokenAddress: {_eq: "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"}}, limit: 200}) {
          owner {
            socials(input: {filter: {dappName: {_eq: lens}}}) {
              profileName
              userId
              userAssociatedAddresses
            }
          }
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
          "owner": {
            "tokenBalances": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "cryptoetc.lens",
                      "userId": "0x188c30e9a6527f5f0c3f7fe59b72ac7253c62f28",
                      "userAssociatedAddresses": [
                        "0x188c30e9a6527f5f0c3f7fe59b72ac7253c62f28"
                      ]
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "tokenBalances": [] // Have USDT, but no USDC
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' Lens profile details will be returned inside the innermost `owner.socials` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.tokenBalances?.map(({ owner }) =>
      owner?.socials?.length > 0 ? owner?.socials : null
    )
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'tokenBalances' in item['owner']:
                for token_balance in item['owner']['tokenBalances']:
                    if 'owner' in token_balance and 'socials' in token_balance['owner'] and len(token_balance['owner']['socials']) > 0:
                        result.append(token_balance['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "cryptoetc.lens",
    "userId": "0x188c30e9a6527f5f0c3f7fe59b72ac7253c62f28",
    "userAssociatedAddresses": [
      "0x188c30e9a6527f5f0c3f7fe59b72ac7253c62f28"
    ]
  }
  // ... other token holders
]
```

## Get Common Holders of Two POAPs and That Has Lens Profile

### Fetching

You can fetch the common holders of two given POAP event IDs, e.g. [EthGlobal Lisbon 2023 Partner Attendee POAP](https://explorer.airstack.xyz/token-holders?address=127462\&blockchain=\&rawInput=%23%E2%8E%B1127462%E2%8E%B1%28127462++++ID\_POAP%29\&inputType=POAP) & [EthCC\[6\] Attendee POAP](https://explorer.airstack.xyz/token-holders?address=141910\&blockchain=gnosis\&rawInput=%23%E2%8E%B1EthCC%5B6%5D+-+Attendee%E2%8E%B1%280x22c1f6050e56d2876009903609a2cc3fef83b415+POAP+gnosis+141910%29+\&inputType=POAP):

#### Try Demo

{% embed url="https://app.airstack.xyz/query/1bf6GUDcES" %}
Get Common Holders Of EthGlobal Lisbon and EthCC POAPs and Their Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfEthGlobalLisbonAndEthCC {
<strong>  Poaps(input: {filter: {eventId: {_eq: "127462"}}, blockchain: ALL, limit: 200}) {
</strong>    Poap {
      owner {
<strong>        poaps(input: {blockchain: ALL, filter: {eventId: {_eq: "141910"}}}) {
</strong>          owner {
            socials(input: {filter: {dappName: {_eq: lens}}}) {
              profileName
              userId
              userAssociatedAddresses
            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Poaps": {
      "Poap": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "schmidsi.lens",
                      "userId": "0x546457bbddf5e09929399768ab5a9d588cb0334d",
                      "userAssociatedAddresses": [
                        "0x546457bbddf5e09929399768ab5a9d588cb0334d"
                      ]
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have EthGlobal Lisbon, but no EthCC[6]
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' Lens profile details will be returned inside the innermost `owner.socials` field.

If user has any Lens profile, then `socials` will have non-`null` value and `profileName` will show the Lens profile name.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.Poaps?.Poap?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) =>
      owner?.socials?.length > 0 ? owner?.socials : null
    )
  )
    .filter(Boolean)
    .flat(2)
    .filter((address, index, array) => array.indexOf(address) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    
    if data and 'Poaps' in data and 'Poap' in data['Poaps']:
        for poap in data['Poaps']['Poap']:
            if 'owner' in poap and 'poaps' in poap['owner']:
                for owner_poap in poap['owner']['poaps']:
                    if 'owner' in owner_poap and 'socials' in owner_poap['owner'] and len(owner_poap['owner']['socials']) > 0:
                        result.append(owner_poap['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result
```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "schmidsi.lens",
    "userId": "0x546457bbddf5e09929399768ab5a9d588cb0334d",
    "userAssociatedAddresses": [
      "0x546457bbddf5e09929399768ab5a9d588cb0334d"
    ]
  },
  // ...other token holders
]
```

## Get Common Holders of A Token (ERC20 or NFT) and A POAP That Has a Lens Profile

### Fetching

You can fetch the common holder of a token and a POAP by providing the token contract address and the POAP event ID:

#### Try Demo

{% embed url="https://app.airstack.xyz/query/lHTqRfk5wb" %}
Show common holders of Sound.xyz NFT and Aave Booth Permissionless POAP that also have Lens Profile
{% endembed %}

#### Code

{% tabs %}
{% tab title="Query" %}
<pre class="language-graphql"><code class="lang-graphql">query GetCommonHoldersOfNounsAndEthCC {
  TokenBalances(
<strong>    input: {filter: {tokenAddress: {_eq: "0x977e43ab3eb8c0aece1230ba187740342865ee78"}}, blockchain: ethereum, limit: 200}
</strong>  ) {
    TokenBalance {
      owner {
<strong>        poaps(input: {filter: {eventId: {_eq: "43882"}}}) {
</strong>          owner {
            socials(input: {filter: {dappName: {_eq: lens}}}) {
              profileName
              userId
              userAssociatedAddresses
            }
          }
        }
      }
    }
  }
}
</code></pre>
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "TokenBalances": {
      "TokenBalance": [
        {
          "owner": {
            "poaps": [
              {
                "owner": {
                  "socials": [
                    {
                      "profileName": "westlakevillage.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "brad.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "bradorbradley.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    },
                    {
                      "profileName": "hanimourra.lens",
                      "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
                      "userAssociatedAddresses": [
                        "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
                      ]
                    }
                  ]
                }
              }
            ]
          }
        },
        {
          "owner": {
            "poaps": null // Have Nouns, but no EthCC[6] POAP
          }
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

All the common holders' Lens profile details will be returned inside the innermost `owner.socials` field.

### Formatting

To get the list of all holders in a flat array, use the following format function:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const formatFunction = (data) =>
  data?.TokenBalances?.TokenBalance?.map(({ owner }) =>
    owner?.poaps?.map(({ owner }) => owner?.socials)
  )
    .filter(Boolean)
    .flat(2)
    .filter((social, index, array) => array.indexOf(social) === index) ?? [];
```
{% endtab %}

{% tab title="Python" %}
```python
def format_function(data):
    result = []
    if data is not None and 'TokenBalances' in data and 'TokenBalance' in data['TokenBalances']:
        for item in data['TokenBalances']['TokenBalance']:
            if 'owner' in item and 'poaps' in item['owner']:
                for poap in item['owner']['poaps']:
                    if 'owner' in poap and 'socials' in poap['owner']:
                        result.append(poap['owner']['socials'])

    result = [item for sublist in result for item in sublist]
    result = [item for sublist in result for item in sublist]
    result = list(set(result))

    return result

```
{% endtab %}
{% endtabs %}

The final result will the the list of all common holders in an array:

```json
[
  {
    "profileName": "westlakevillage.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "brad.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "bradorbradley.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  {
    "profileName": "hanimourra.lens",
    "userId": "0x8ec94086a724cbec4d37097b8792ce99cadcd520",
    "userAssociatedAddresses": [
      "0x8ec94086a724cbec4d37097b8792ce99cadcd520"
    ]
  },
  // ...other token holders
]
```

## Developer Support

If you have any questions or need help regarding fetching holders or attendees of multiple POAPs, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Combinations](recommendation-engines.md)
  * [Multiple ERC20s or NFTs](../combinations/multiple-erc20s-or-nfts.md)
  * [Mutliple POAPs](../combinations/multiple-poaps.md)
  * [Combinations of ERC20s, NFTs, and POAPs](../combinations/erc20s-nfts-and-poaps.md)
  * [Social Combinations](../combinations/socials-stats.md)
* [TokenBalances API Reference](../../api-references/api-reference/tokenbalances-api/)
* [POAPs API Reference](../../api-references/api-reference/poaps-api/)
