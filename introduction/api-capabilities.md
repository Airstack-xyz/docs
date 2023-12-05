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

# Table Of Contents

- [Technology](api-capabilities.md#technology)
- [Rate Limits](api-capabilities.md#rate-limits)
- [Features](api-capabilities.md#features)
  - [Blockchains, Tokens, and Dapps](api-capabilities.md#blockchains-tokens-and-dapps)
  - [Domains Specific](api-capabilities.md#domains-specific)
  - [Lens Specific](api-capabilities.md#lens-specific)
  - [Farcaster Specific](api-capabilities.md#farcaster-specific)
  - [ERC-6551 Specific](api-capabilities.md#erc-6551-specific)

## Technology

- [Airstack](https://app.airstack.xyz) **indexes onchain and offchain data** (see tables below for summary of what’s indexed as of 15 Nov 2023). Whenever possible we default to onchain as the source of truth.
- **Onchain** data is indexed through a blend of Substreams, RPC nodes, and Subgraphs. New transactions from the chains and projects we index appear in Airstack within seconds of onchain finalization. We use a blend of technologies depending on the use case.
  - We index onchain data optimistically without waiting for block confirmation
  - If our indexing layer detects a reorg, then we will redo those blocks
- **Offchain** data sources include Farcaster hubs, XMTP, and token/NFT Metadata.
- **Data is currently organized and stored in Airstack’s hosted infra**, using Kafka, MongoDB, Postgres, Clickhouse, Redis, S3.
  - This allows for blazing fast queries with responses in milliseconds
  - We currently invest considerably to maintain and optimize this infra, so our clients don’t need to worry about it
  - In the future we plan to decentralize this and make it permissionless to access.
- **We download and store/resize all the images and metadata in our CDN.**
  - We do this for all NFT, POAPs (ipfs, arweave, etc). And also for any NFTs with audio or media files.
  - We offer 5 image sizes from our CDN that are easy to integrate in any app without further treatment
  - We also index and store all the NFT metadata, traits
  - Also all Farcaster, Lens, ENS profiles, images, bios (edited)
- Airstack’s data is uniquely organized to allow for **composability**, combining data from any of the Airstack data sources and APIs in a single query/response.
- Airstack provides **GraphQL APIs** for the data, with the unique ability to **query cross-chain, cross-project, and combine onchain and offchain data in a single query & response**
- **Airstack AI** engine writes GraphQL queries for the user, allowing for complex natural language queries such as “show me all attendees of @ethcc6 and their Lens profiles and if they have XMTP” or “show POAPs in common for stani.lens and betashop9.lens”

## Rate Limits

We currently offer rate limit of 3000/min and burst of 300/second.

## Features

### Blockchains, Tokens, and Dapps\*

<table><thead><tr><th>Feature</th><th width="107">Ethereum</th><th width="100">Polygon</th><th width="85">Base</th><th width="89">Gnosis (POAP)</th><th width="105">Optimism</th><th>Offchain</th></tr></thead><tbody><tr><td>ERC-20</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td><td>Metadata</td></tr><tr><td>ERC-721</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>ERC-1155</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>ERC-6551</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Token Balances</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Token Holders</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Transfer History</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td></tr><tr><td>Mints</td><td>Nov 2023</td><td>Nov 2023</td><td>✅</td><td></td><td></td><td></td></tr><tr><td>POAP</td><td>✅</td><td></td><td></td><td>✅</td><td></td><td>Resized images Metadata, housed in our CDN</td></tr><tr><td>Domains</td><td>ENS Primary All</td><td></td><td></td><td></td><td></td><td>Metadata Images</td></tr><tr><td>Lens Profiles, bios, images</td><td></td><td>✅</td><td></td><td></td><td></td><td></td></tr><tr><td>Lens Social Graph</td><td></td><td>✅</td><td></td><td></td><td></td><td></td></tr><tr><td>Farcaster Profiles, bios, images</td><td></td><td></td><td></td><td></td><td>✅</td><td>✅ Hubs</td></tr><tr><td>Farcaster social graph</td><td></td><td></td><td></td><td></td><td></td><td>✅ Hubs</td></tr><tr><td>XMTP</td><td></td><td></td><td></td><td></td><td></td><td>✅</td></tr><tr><td>Tokens in common, holders in common</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td></tr><tr><td>Rec Engines</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td></tr><tr><td>Spam filters </td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td></tr><tr><td>Snapshot<br>time series data </td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td></td><td></td></tr><tr><td>Contracts / dapps </td><td>Dec 2023</td><td>Dec 2023</td><td>✅</td><td></td><td></td><td></td></tr><tr><td>Uniswap swaps</td><td>Dec 2023</td><td>Dec 2023</td><td>✅</td><td></td><td></td><td></td></tr><tr><td>AI Natural language queries</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td>Webhooks</td><td>Q1 2023</td><td>Q1 2023</td><td>✅</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td></tr><tr><td>API for Airstack AI</td><td>Q1 2023</td><td>Q1 2023</td><td>✅</td><td>Q1 2023</td><td>Q1 2023</td><td>Q1 2023</td></tr></tbody></table>

### Domains Specific

| Feature                                                  | Availability       |
| -------------------------------------------------------- | ------------------ |
| Fetch all ENS owned by a user (including subdomains)     | :white_check_mark: |
| Fetch Primary ENS of a user                              | :white_check_mark: |
| Fetch ENS details (registration cost, expiry date, etc.) | :white_check_mark: |
| Resolve ENS address                                      | :white_check_mark: |
| Resolve Namestone subdomain (ENS offchain)               | :white_check_mark: |
| Resolve DNS Domain from ENS's DNS Registrar              | :white_check_mark: |
| Resolve [cb.id](http://cb.id) address                    | :white_check_mark: |
| ENS Profile images                                       | :white_check_mark: |
| All POAPs owned by resolved ENS address                  | :white_check_mark: |
| ENS V2                                                   | :white_check_mark: |

### Lens Specific

| Features                                                                          | Availability       |
| --------------------------------------------------------------------------------- | ------------------ |
| Lens Followers                                                                    | :white_check_mark: |
| Lens Following                                                                    | :white_check_mark: |
| Has XMTP                                                                          | :white_check_mark: |
| Resolve Lens to 0x                                                                | :white_check_mark: |
| Resolve Lens to ENS                                                               | :white_check_mark: |
| Resolve Lens to Farcaster                                                         | :white_check_mark: |
| Has more than X Followers                                                         | :white_check_mark: |
| Followers More than X                                                             | :white_check_mark: |
| Mutually following                                                                | :white_check_mark: |
| Followers in Common                                                               | :white_check_mark: |
| More than X Followers in Common                                                   | :white_check_mark: |
| All POAPs of Lens User                                                            | :white_check_mark: |
| All Lens users who have POAP                                                      | :white_check_mark: |
| All tokens and NFTs of Lens User                                                  | :white_check_mark: |
| All Lens users who have tokens and NFTs                                           | :white_check_mark: |
| Combo: Follows/following and has POAPs                                            | :white_check_mark: |
| Combos: Follows/following and has Tokens                                          | :white_check_mark: |
| Combos: Follows/following and has XMTP                                            | :white_check_mark: |
| Also follows/following on Farcaster                                               | :white_check_mark: |
| Date specific data (e.g. followed before/after date, had token before/after date) | Nov 2023           |
| Lens V2                                                                           | :white_check_mark: |
| Lens Profile metadata: profile images, bio, etc.                                  | :white_check_mark: |
| Lens Collects, combos thereof                                                     | Q1 2024            |
| Mints by Lens users                                                               | Dec 2023           |
| Lens users’ Contract Interactions                                                 | Dec 2023           |
| Open Actions indexing                                                             | Q1 2024            |

### Farcaster Specific

| Features                                                                          | Availability       |
| --------------------------------------------------------------------------------- | ------------------ |
| Farcaster Followers                                                               | :white_check_mark: |
| Farcaster Following                                                               | :white_check_mark: |
| Has XMTP                                                                          | :white_check_mark: |
| Resolve Farcaster to 0x                                                           | :white_check_mark: |
| Resolve Farcaster to ENS                                                          | :white_check_mark: |
| Resolve Farcaster to Lens                                                         | :white_check_mark: |
| Has more than X Followers                                                         | :white_check_mark: |
| Followers More than X                                                             | :white_check_mark: |
| Mutually following                                                                | :white_check_mark: |
| Followers in Common                                                               | :white_check_mark: |
| More than X Followers in Common                                                   | :white_check_mark: |
| All POAPs of Farcaster User                                                       | :white_check_mark: |
| All Farcaster users who have a POAP                                               | :white_check_mark: |
| All tokens and NFTs of Farcaster User                                             | :white_check_mark: |
| All Farcaster users who have tokens and NFTs                                      | :white_check_mark: |
| Combo: Follows/following and has POAPs                                            | :white_check_mark: |
| Combos: Follows/following and has Tokens                                          | :white_check_mark: |
| Combos: Follows/following and has XMTP                                            | :white_check_mark: |
| Also follows/following on Lens                                                    | :white_check_mark: |
| Date specific data (e.g. followed before/after date, had token before/after date) | DeC 2023           |
| Mints by Farcaster users                                                          | Dec 2023           |
| Farcaster users’ Contract Interactions                                            | Dec 2023           |

### ERC-6551 Specific

| Features                                          | Availability       |
| ------------------------------------------------- | ------------------ |
| All ERC6551 accounts deployed                     | :white_check_mark: |
| ERC6551 accounts owned by users                   | :white_check_mark: |
| ERC6551 accounts of an NFT collection             | :white_check_mark: |
| ENS & Lens of ERC6551 accounts (if any)           | :white_check_mark: |
| Token Balances of ERC6551 accounts                | :white_check_mark: |
| Traversing up ERC6551 tree                        | :white_check_mark: |
| Traversing down ERC6551 tree                      | :white_check_mark: |
| Cross-chain ERC6551 accounts (Ethereum & Polygon) | :white_check_mark: |
| Optimistic indexing/non-deployed ERC6551 accounts | Dec 2023           |

{% hint style="info" %} \* all future release are subject to modification
{% endhint %}

Please contact us to let us know your needs.
