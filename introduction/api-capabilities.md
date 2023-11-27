---
description: Learn all the current features available on Airstack and its future roadmap.
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

# 🚦 API Capabilities

## Table Of Contents

* [Technology](api-capabilities.md#technology)
* [Rate Limits](api-capabilities.md#rate-limits)
* [Features](api-capabilities.md#features)
  * [Blockchains, Tokens, and Dapps](api-capabilities.md#blockchains-tokens-and-dapps)
  * [Domains Specific](api-capabilities.md#domains-specific)
  * [Lens Specific](api-capabilities.md#lens-specific)
  * [Farcaster Specific](api-capabilities.md#farcaster-specific)
  * [ERC-6551 Specific](api-capabilities.md#erc-6551-specific)

## Technology

* [Airstack](https://app.airstack.xyz) **indexes onchain and offchain data** (see tables below for summary of what’s indexed as of 15 Nov 2023). Whenever possible we default to onchain as the source of truth.
* **Onchain** data is indexed through a blend of Substreams, RPC nodes, and Subgraphs. New transactions from the chains and projects we index appear in Airstack within seconds of onchain finalization. We use a blend of technologies depending on the use case.
  * We index onchain data optimistically without waiting for block confirmation
  * If our indexing layer detects a reorg, then we will redo those blocks
* **Offchain** data sources include Farcaster hubs, XMTP, and token/NFT Metadata.
* **Data is currently organized and stored in Airstack’s hosted infra**, using Kafka, MongoDB, Postgres, Clickhouse, Redis, S3.
  * This allows for blazing fast queries with responses in milliseconds
  * We currently invest considerably to maintain and optimize this infra, so our clients don’t need to worry about it
  * In the future we plan to decentralize this and make it permissionless to access.
* **We download and store/resize all the images and metadata in our CDN.**
  * We do this for all NFT, POAPs (ipfs, arweave, etc). And also for any NFTs with audio or media files.
  * We offer 5 image sizes from our CDN that are easy to integrate in any app without further treatment
  * We also index and store all the NFT metadata, traits
  * Also all Farcaster, Lens, ENS profiles, images, bios (edited)
* Airstack’s data is uniquely organized to allow for **composability**, combining data from any of the Airstack data sources and APIs in a single query/response.
* Airstack provides **GraphQL APIs** for the data, with the unique ability to **query cross-chain, cross-project, and combine onchain and offchain data in a single query & response**
* **Airstack AI** engine writes GraphQL queries for the user, allowing for complex natural language queries such as “show me all attendees of @ethcc6 and their Lens profiles and if they have XMTP” or “show POAPs in common for stani.lens and betashop9.lens”

## Rate Limits

We currently offer rate limit of 3000/min and burst of 300/second.

## Features

### Blockchains, Tokens, and Dapps\*

<table><thead><tr><th>Feature</th><th width="107">Ethereum</th><th width="100">Polygon</th><th width="85">Base</th><th width="89">Gnosis (POAP)</th><th width="105">Optimism</th><th>Offchain</th></tr></thead><tbody><tr><td><strong>ERC-20</strong></td><td>✅</td><td>✅</td><td>Nov 2023</td><td></td><td></td><td>Metadata</td></tr><tr><td><strong>ERC-721</strong></td><td>✅</td><td>✅</td><td>Nov 2023</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td><strong>ERC-1155</strong></td><td>✅</td><td>✅</td><td>Nov 2023</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td><strong>ERC-6551</strong></td><td>✅</td><td>✅</td><td>Nov 2023</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Token Balances</td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Token Holders</td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Transfer History</td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td></td></tr><tr><td>Mints</td><td>Nov 2023</td><td>Nov 2023</td><td>Nov 2023</td><td></td><td></td><td></td></tr><tr><td>NFT Marketplace, Collection, &#x26; Sales Data</td><td>✅</td><td>✅</td><td>Nov 2023</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>POAP</td><td>✅</td><td></td><td></td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Domains</td><td>ENS Primary All</td><td></td><td></td><td></td><td></td><td>Metadata Images</td></tr><tr><td>Lens Profiles, bios, images</td><td></td><td>✅</td><td></td><td></td><td></td><td></td></tr><tr><td>Lens Social Graph</td><td></td><td>✅</td><td></td><td></td><td></td><td></td></tr><tr><td>Farcaster Profiles, bios, images</td><td></td><td></td><td></td><td></td><td>✅</td><td>✅ Hubs</td></tr><tr><td>Farcaster social graph</td><td></td><td></td><td></td><td></td><td></td><td>✅ Hubs</td></tr><tr><td>XMTP</td><td></td><td></td><td></td><td></td><td></td><td>✅</td></tr><tr><td>Tokens in common, holders in common</td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td></td></tr><tr><td>Rec Engines</td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td></td></tr><tr><td>Spam filters </td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td></td></tr><tr><td>Snapshot<br>time series data </td><td>✅</td><td>✅</td><td>Nov 2023</td><td>✅</td><td></td><td></td></tr><tr><td>Contracts / dapps </td><td>Dec 2023</td><td>Dec 2023</td><td>Dec 2023</td><td></td><td></td><td></td></tr><tr><td>Uniswap swaps</td><td>Dec 2023</td><td>Dec 2023</td><td>Dec 2023</td><td></td><td></td><td></td></tr><tr><td>AI Natural language queries</td><td>✅</td><td>✅</td><td>Oct 2023</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td>Webhooks</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td></tr><tr><td>API for Airstack AI</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td></tr></tbody></table>

### Domains Specific

| Feature                                                  | Availability         |
| -------------------------------------------------------- | -------------------- |
| Fetch all ENS owned by a user (including subdomains)     | :white\_check\_mark: |
| Fetch Primary ENS of a user                              | :white\_check\_mark: |
| Fetch ENS details (registration cost, expiry date, etc.) | :white\_check\_mark: |
| Resolve ENS address                                      | :white\_check\_mark: |
| Resolve Namestone subdomain (ENS offchain)               | :white\_check\_mark: |
| Resolve DNS Domain from ENS's DNS Registrar              | :white\_check\_mark: |
| Resolve [cb.id](http://cb.id) address                    | :white\_check\_mark: |
| ENS Profile images                                       | :white\_check\_mark: |
| All POAPs owned by resolved ENS address                  | :white\_check\_mark: |
| ENS V2                                                   | :white\_check\_mark: |

### Lens Specific

| Features                                                                          | Availability         |
| --------------------------------------------------------------------------------- | -------------------- |
| Lens Followers                                                                    | :white\_check\_mark: |
| Lens Following                                                                    | :white\_check\_mark: |
| Has XMTP                                                                          | :white\_check\_mark: |
| Resolve Lens to 0x                                                                | :white\_check\_mark: |
| Resolve Lens to ENS                                                               | :white\_check\_mark: |
| Resolve Lens to Farcaster                                                         | :white\_check\_mark: |
| Has more than X Followers                                                         | :white\_check\_mark: |
| Followers More than X                                                             | :white\_check\_mark: |
| Mutually following                                                                | :white\_check\_mark: |
| Followers in Common                                                               | :white\_check\_mark: |
| More than X Followers in Common                                                   | :white\_check\_mark: |
| All POAPs of Lens User                                                            | :white\_check\_mark: |
| All Lens users who have POAP                                                      | :white\_check\_mark: |
| All tokens and NFTs of Lens User                                                  | :white\_check\_mark: |
| All Lens users who have tokens and NFTs                                           | :white\_check\_mark: |
| Combo: Follows/following and has POAPs                                            | :white\_check\_mark: |
| Combos: Follows/following and has Tokens                                          | :white\_check\_mark: |
| Combos: Follows/following and has XMTP                                            | :white\_check\_mark: |
| Also follows/following on Farcaster                                               | :white\_check\_mark: |
| Date specific data (e.g. followed before/after date, had token before/after date) | Nov 2023             |
| Lens V2                                                                           | :white\_check\_mark: |
| Lens Profile metadata: profile images, bio, etc.                                  | :white\_check\_mark: |
| Lens Collects, combos thereof                                                     | Q1 2024              |
| Mints by Lens users                                                               | Oct/Nov 2023         |
| Lens users’ Contract Interactions                                                 | Dec 2023             |
| Open Actions indexing                                                             | Q1 2024              |

### Farcaster Specific

| Features                                                                          | Availability         |
| --------------------------------------------------------------------------------- | -------------------- |
| Farcaster Followers                                                               | :white\_check\_mark: |
| Farcaster Following                                                               | :white\_check\_mark: |
| Has XMTP                                                                          | :white\_check\_mark: |
| Resolve Farcaster to 0x                                                           | :white\_check\_mark: |
| Resolve Farcaster to ENS                                                          | :white\_check\_mark: |
| Resolve Farcaster to Lens                                                         | :white\_check\_mark: |
| Has more than X Followers                                                         | :white\_check\_mark: |
| Followers More than X                                                             | :white\_check\_mark: |
| Mutually following                                                                | :white\_check\_mark: |
| Followers in Common                                                               | :white\_check\_mark: |
| More than X Followers in Common                                                   | :white\_check\_mark: |
| All POAPs of Farcaster User                                                       | :white\_check\_mark: |
| All Farcaster users who have a POAP                                               | :white\_check\_mark: |
| All tokens and NFTs of Farcaster User                                             | :white\_check\_mark: |
| All Farcaster users who have tokens and NFTs                                      | :white\_check\_mark: |
| Combo: Follows/following and has POAPs                                            | :white\_check\_mark: |
| Combos: Follows/following and has Tokens                                          | :white\_check\_mark: |
| Combos: Follows/following and has XMTP                                            | :white\_check\_mark: |
| Also follows/following on Lens                                                    | :white\_check\_mark: |
| Date specific data (e.g. followed before/after date, had token before/after date) | Oct 2023             |
| Mints by Farcaster users                                                          | Oct/Nov 2023         |
| Farcaster users’ Contract Interactions                                            | Dec 2023             |

### ERC-6551 Specific

| Features                                          | Availability         |
| ------------------------------------------------- | -------------------- |
| All ERC6551 accounts deployed                     | :white\_check\_mark: |
| ERC6551 accounts owned by users                   | :white\_check\_mark: |
| ERC6551 accounts of an NFT collection             | :white\_check\_mark: |
| ENS & Lens of ERC6551 accounts (if any)           | :white\_check\_mark: |
| Token Balances of ERC6551 accounts                | :white\_check\_mark: |
| Traversing up ERC6551 tree                        | :white\_check\_mark: |
| Traversing down ERC6551 tree                      | :white\_check\_mark: |
| Cross-chain ERC6551 accounts (Ethereum & Polygon) | :white\_check\_mark: |
| Optimistic indexing/non-deployed ERC6551 accounts | Dec 2023             |

{% hint style="info" %}
\* all future release are subject to modification
{% endhint %}

Please contact us to let us know your needs.
