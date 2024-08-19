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

# 🚦 Tech Infra & Capabilities

## Topics

* [Technology](api-capabilities.md#technology)
* [Rate Limits](api-capabilities.md#rate-limits)
* [Features](api-capabilities.md#features)

### Technology

* [Airstack](https://app.airstack.xyz) **indexes onchain and offchain data**. Whenever possible we default to onchain as the source of truth.
* **Onchain** data is indexed through a blend of technologies depending on use case. New transactions from the chains and projects we index appear in Airstack within seconds of onchain finalization.
  * We index onchain data optimistically without waiting for block confirmation
  * If our indexing layer detects a reorg, then we will redo those blocks
* **Offchain** data sources include Farcaster hubs, and token/NFT Metadata.
* Airstack’s data is uniquely organized to allow for **composability**, combining data from any of the Airstack data sources and APIs in a single query/response.
* Airstack provides **GraphQL APIs** for the data, with the unique ability to **query cross-chain, cross-project, and combine onchain and offchain data in a single query & response**
* **Farcaster. Airstack is the easiest way to build on Farcaster.** You can access every user, every cast, every channel, every like, every reply, every recast, every follow, and compose it with everything onchain.
* **Airstack provides read/write access to Farcaster Hubs** with REST and GRPC.
* **Airstack AI** engine writes GraphQL queries for the user, allowing for complex natural language queries such as “show me all attendees of @ethcc6 and their Lens profiles and if they have XMTP” or “show POAPs in common for stani.lens and betashop9.lens”
* **Abstractions.** Airstack abstractions enable devs to easily integrate use cases not just data. The GUIDES in the left menu are designed to help you easily find abstractions that match your use cases.
* **Data is currently organized and stored in Airstack’s hosted infra**.
  * This allows for blazing fast queries with responses in milliseconds
  * We currently invest considerably to maintain and optimize this infra, so our clients don’t need to worry about it
  * In the future we plan to decentralize this and make it permissionless to access.
* **We download and store/resize all images and metadata in our CDN.**
  * We do this for all NFT, POAPs (ipfs, arweave, etc). And also for any NFTs with audio or media files.
  * We offer 5 image sizes from our CDN that are easy to integrate in any app without further treatment
  * We also index and store all the NFT metadata, traits
  * Also all Farcaster, Lens, ENS profiles, images, bios

### Rate Limits

We currently offer rate limit of 3000/min and burst of 300/second.

### Features

#### These charts summarize the major Features available in Airstack. 

<figure><img src="../.gitbook/assets/Screenshot 2024-04-11 at 12.40.42 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Welcome to The Composability Network-min 2.png" alt=""><figcaption><p>Airstack Web3 Integration</p></figcaption></figure>

Please contact us to let us know your needs.
