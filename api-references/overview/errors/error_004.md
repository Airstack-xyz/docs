---
description: Learn what ERROR_004 is and how to resolve the error.
---

# ERROR\_004

## Description

If you are getting this error, then it implies that there is an **insufficient filter fields** provided.

In other words, the input that you provided is either missing (if required) or have an invalid value.

## Resolution

**ERROR\_004** have variations, depending on the API you're calling and input you're providing. For resolutions, please check the table below for a case-by-case basis.

<table><thead><tr><th>Message</th><th width="167">API</th><th>Resolution</th></tr></thead><tbody><tr><td>Address OR Name OR Symbol OR Type is Mandatory</td><td><code>Token</code></td><td>Provide <code>address</code> to <code>Token</code> API.</td></tr><tr><td>Address OR Metadata Name is Mandatory</td><td><code>TokenNft</code></td><td>Provide either <code>address</code> or <code>tokenId</code> as an input to <code>TokenNft</code> API.</td></tr><tr><td>From Address OR To Address OR Token Address OR Transaction Hash is Mandatory</td><td><code>TokenTransfer</code></td><td>Provide at least one of either <code>address</code>, <code>tokenAddress</code>, or <code>transactionHash</code> as an input to the <code>TokenTransfer</code> API.</td></tr><tr><td>Name OR ResolvedAddress is Mandatory</td><td><code>Domain</code></td><td>Provide either <code>name</code> or <code>resolvedAddress</code> as an input to the <code>Domain</code> API.</td></tr><tr><td>Mandatory Fields are missing</td><td>Any</td><td>Provide all the mandatory input fields for calling the API. For more details on which fields are required, check <a href="../../api-reference/">API reference</a>.</td></tr><tr><td>Input Cursor is not valid</td><td>Any</td><td>Make sure that you have the cursor input correct. For more details on cursor pagination, click <a href="../cursor-pagination.md">here</a>.</td></tr></tbody></table>

If you are not able to resolve **ERROR\_004**, please report it directly to [Airstack](https://airstack.xyz) through [Telegram](https://t.me/+1k3c2FR7z51mNDRh) or email us at [support@airstack.xyz](mailto:support@airstack.xyz).
