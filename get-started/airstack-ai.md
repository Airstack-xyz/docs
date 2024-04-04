---
description: >-
  Learn how to use the Airstack AI effectively to quickly build queries for your
  use cases using natural languages.
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

# 🤖 Airstack AI

The [Airstack](https://www.airstack.xyz/) AI is a very powerful tool for you to build [Airstack](https://www.airstack.xyz/) queries efficiently within seconds.

In this tutorial, you will learn:

* [Best Practices to use Airstack AI](airstack-ai.md#best-practices)
* [Try Examples](airstack-ai.md#try-examples)

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Generating Queries With Airstack AI</p></figcaption></figure>

{% hint style="info" %}
Currently, Airstack AI is only available in Airstack API studio. In the future, we'll have this offered as an API service.
{% endhint %}

## Best Practices

To get the best out of the feature, here are some best practices to follow:

### Tip #1: Be Specific And Clear On Inputs

Start with the input and make it clear what's the input. For example:

* For Farcaster user [dwr](https://explorer.airstack.xyz/token-balances?address=fc\_fname%3Adwr\&blockchain=ethereum\&rawInput=%23%E2%8E%B1fc\_fname%3Adwr%E2%8E%B1%28fc\_fname%3Adwr++ethereum+null%29\&inputType=ADDRESS), show their web3 socials and NFTs
* For [vitalik.eth](https://explorer.airstack.xyz/token-balances?address=vitalik.eth\&blockchain=ethereum\&rawInput=%23%E2%8E%B1vitalik.eth%E2%8E%B1%28vitalik.eth++ethereum+null%29\&inputType=ADDRESS), show their POAPs

### Tip #2: Use Airstack Token Selector

Use `@` to choose from a list of 10,000 popular Tokens, NFTs, and POAP events

<figure><img src="https://lh4.googleusercontent.com/a-lBiQ8MbApGBks7ibZTqhbDnRY2OzUaxtgHtvywW2-gJZ1Nbl12SfKr-jsvdQ8-9YVp8T0PSpdHRQTE-3vwJguLPzXNaLp5n03jq2xqkG7IabLgdFU9o-8oFHEIehM05eGRLptMj2XjfvwXJrCCj_Y" alt=""><figcaption></figcaption></figure>

If you are looking for a project that is not yet in our auto-complete @mention list, please [let us know](https://t.me/+uW6ypo49TcZmMGEx) in the developer Telegram group. In meantime, you can always enter a contract address.

### Tip #3: Use _/help_

Use _/help_ to get an intro and learn more about the [Airstack](https://www.airstack.xyz/) API and what data is available for you.

## Try Examples

{% hint style="info" %}
Make sure to type `@` manually to generate the chooser. Do not just copy/paste.
{% endhint %}

### **Combinations**

* Get all attendees of @Devcon5 who also attended @EthCC\[6] - Attendee and their ENS
* Get all holders of @moonbirds who also hold @BAYC
* Get all holders of @nouns who also have >100 @USDC

### **ERC6551 Token Bound Accounts**

* Show the most recent 10 6551 accounts on ethereum, base, and zora
* Show me all @Sapienz ERC6551 accounts and their balances
* Show all ERC65111 accounts owned by @BoredApeYachtClub and the token balances
* Show lens/@sapienz\_0 ERC6551 accounts
* Show me 6551 accounts created on July 12, 2023 on Ethereum

### **XMTP**

* Check if betashop.eth has XMTP
* Show all holders of @BoredApeYachtClub and if they have XMTP
* Show all holders of @EthCC6 POAP and if they have XMTP

### **Token Balances, Identities, Transactions**

* For shnoodles.eth show all ENS, NFTs and web3 socials
* Show all NFTs owned by lens/@stani
* Show all transactions by balajis.eth within last 30 days
* Show me all NFTs held by vitalik.eth on Base and the images of those NFTs

### **Web3 Socials**

* For vitalik.eth, show all of their ENS names, and their web3 socials -- this will return all of his ENS names, his Farcaster, and his Lens
* For Lens user lens/@stani show all of their NFTS
* For Farcaster user betashop show all his NFTs and their images
* Get the Ethereum connected address for Farcaster user betashop, along with the profile creation date and recovery address
* Get all of the ENS Domains owned by vitalik.eth

### **Token Holders + Identities**

* Show all @orangeDAO holders and their ENS and web3 socials
* For @BAYC holders show all of their Lens usernames
* Get tokens in the @BoredApeYachtClub and the traits of each NFT
* Get the holders of @y00ts and their ENS names and Lens if they have one
* Get the token holders of @purpleDAO and the Farcaster profile name of each holder, their Farcaster registration date, FID number, their connected address, and all NFTs they own

### **POAPs**

* Get all attendees of @devcon2 and their web3 socials and ENS
* For Farcaster user worthalter - show all their POAPs and web3 socials and ENS
* Get all events attended by Patricio.eth
* Show all holders of @orangeDAO and their POAPs

### **Token & Contract Data**

* Get the token metadata for @Matic
* Get the token transfer history of the @Mutant Apes NFT Collection on Ethereum
* Show me @Moonbirds NFT id 5555 image
* Give me recent transfers of @DAI token

**Please try asking the Airstack AI Assistant all sorts of questions and let us know when it falls short. We're training to be smarter and smarter**
