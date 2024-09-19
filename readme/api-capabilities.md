---
hidden: true
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
* **Offchain** data sources include Farcaster hubs, and token/NFT Metadata.
* Airstack’s data is uniquely organized to allow for **composability**, combining data from any of the Airstack data sources and APIs in a single query/response.
* Airstack provides **GraphQL APIs** for the data, with the unique ability to **query cross-chain, cross-project, and combine onchain and offchain data in a single query & response**
* **Farcaster. Airstack is the easiest way to build on Farcaster.** You can access every user, every cast, every channel, every like, every reply, every recast, every follow, and compose it with everything onchain.
* **Airstack provides read/write access to Farcaster Hubs** with REST and GRPC.
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

Please contact us to let us know your needs.
