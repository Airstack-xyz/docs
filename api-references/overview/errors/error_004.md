---
description: Learn what ERROR_004 is and how to resolve the error.
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

# ERROR\_004

## Description

If you are getting this error, then it implies that there is an **insufficient filter fields** provided.

In other words, the input that you provided is either missing (if required) or have an invalid value.

## Resolution

**ERROR\_004** have variations, depending on the API you're calling and input you're providing. For resolutions, please check the table below for a case-by-case basis.

<table><thead><tr><th>Message</th><th width="167">API</th><th>Resolution</th></tr></thead><tbody><tr><td>Name OR ResolvedAddress is Mandatory</td><td><code>Domain</code></td><td>Provide either <code>name</code> or <code>resolvedAddress</code> as an input to the <code>Domain</code> API.</td></tr><tr><td>Mandatory Fields are missing</td><td>Any</td><td>Provide all the mandatory input fields for calling the API. For more details on which fields are required, check <a href="../../api-reference/">API reference</a>.</td></tr><tr><td>Input Cursor is not valid</td><td>Any</td><td>Make sure that you have the cursor input correct. For more details on cursor pagination, click <a href="../cursor-pagination.md">here</a>.</td></tr></tbody></table>

If you are not able to resolve **ERROR\_004**, please report it directly to [Airstack](https://airstack.xyz) through [Telegram](https://t.me/+1k3c2FR7z51mNDRh) or email us at [support@airstack.xyz](mailto:support@airstack.xyz).
