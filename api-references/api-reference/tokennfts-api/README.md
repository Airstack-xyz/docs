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

# TokenNFTs API

{% hint style="info" %}
The TokenNFTs API provides detailed data about a specific NFT token within an NFT Collection, either an ERC721 or ERC1155 contract. You can use this API to return token-specific metadata, including traits, and <mark style="background-color:green;">**resized images**</mark>.
{% endhint %}

### Inputs & Filters

```graphql
input TokenNftFilter {
  address: # Contract address (ERC721, ERC1155)
  metaData: # allows querying by NFT Token Name, or specific trait or attribute.
  tokenId: # Unique NFT token ID
}
```

### Outputs

<pre class="language-graphql"><code class="lang-graphql">type TokenNft {
address: Address! # NFT contract address on the blockchain
blockchain: Blockchain # Blockchain where the NFT token is deployed
chainId: String! # Unique blockchain identifier
contentType: String # Content type of the NFT token (image, video, audio, etc.)
contentValue: Media # NFT Media - resized images, animation, videos, etc.
id: # Airstack unique identifier for the NFT token
lastTransferBlock: # Block number of the token NFT's most recent transfer
lastTransferHash: # Hash of the token NFT's most recent transfer
lastTransferTimestamp: # Timestamp of the token NFT's most recent transfer
metaData: NftMetadata # Metadata associated with the NFT token
nftSaleTransactions: # **Nested query** allowing to retrieve token sale transactions
rawMetaData: # NFT token metadata as defined in the contract
token: # **Nested query** allowing to query contract level data
tokenBalances: # **Nested query** with token balance data &#x26; owner data
tokenId: IntString! # Unique NFT token ID
tokenTransfers: # **Nested query** with token transfers data
tokenURI: String # URI for the token NFT's resources
<strong>totalSupply: String # Total supply of the NFT token
</strong>type: TokenType # Type of NFT - ERC721 or ERC1155
}
</code></pre>

{% hint style="info" %}
Resized NFT images size guide:

* extra\_small: 125x125px
* small: 250x250px
* medium: 500x500px
* large: 750x750px
{% endhint %}
