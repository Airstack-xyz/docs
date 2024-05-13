---
description: >-
  Learn how to use Airstack to recommend trending mints to your users based on
  various conditions.
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

# 📈 Trending Mints

Airstack provides two methods for generating trending mints:

## 1. Airstack Abstractions (no backend db required)

Airstack Abstractions require no backend database. You simply input a couple of criteria and Airstack returns the calculated results. For example: Trending Mints > Farcaster Users > On Base > Last 24 Hours

Trending Mints Abstractions are currently available for:

* Mints on Base & Degen L3 Blockchains
* All Base Users or Filtered to Farcaster Accounts
* Various time ranges

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f535">🔵</span> <strong>All Base Users (Abstraction)</strong></td><td>Learn how to recommend mints that are trending among all Base users.</td><td></td><td><a href="global.md">global.md</a></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3a9">🎩</span> <strong>All Degen L3 Users (Abstraction)</strong></td><td>Learn how to recommend mints that are trending among all Degen L3 users.</td><td></td><td></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f49c">💜</span> <strong>Farcaster Users (Abstraction)</strong></td><td>Learn how to recommend mints that are trending among Farcaster users.</td><td></td><td><a href="farcaster-users.md">farcaster-users.md</a></td></tr></tbody></table>

## 2. Data Transpositions (backend db required)

Data Transpositions require you to synthesize data in your backend to recommend Mints based on various criteria such as tokens in common, onchain graph, social follows, and more

Data Transposition method is currently available for:

* Base, Zora, Ethereum, Gold
* Various criteria that you can program with Airstack APIs

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f578">🕸️</span> <strong>Onchain Graph (Data Transpositions)</strong></td><td>Learn how to recommend mints that are trending amongst the user's onchain connections (Farcaster, Lens, NFTs in common, POAPs in common, token transfers, and more).</td><td></td><td><a href="onchain-graph.md">onchain-graph.md</a></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f45b">👛</span> <strong>Common Minters (Data Transpositions)</strong></td><td>Learn how to recommend mints based on related audiences: users who have X are also Minting Y.</td><td></td><td><a href="common-minters.md">common-minters.md</a></td></tr></tbody></table>
