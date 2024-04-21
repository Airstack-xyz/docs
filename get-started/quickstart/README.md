---
description: >-
  Try Airstack API and integrate it into your dapp within minutes into your tech
  stack. You can either call the Airstack API with the Airstack SDKs or direct
  API call.
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

# 🚀 Quickstart & SDKs

{% hint style="info" %}
* **API Endpoint:** [https://api.airstack.xyz/graphql](https://api.airstack.xyz/graphql)
* **Record limit** per API call
  * Maximum amount 200
  * Default to 50
  * We support cursor-based pagination to retrieve the next set of results.
* It is best practice that you keep the size of the array for input filters, e.g. `_in` or `_nin`, to a **maximum length of 200** per API call.
{% endhint %}

To use the Airstack APIs, you have two options to make API calls:

## Airstack SDKs

This is most suitable if you are using languages/frameworks that the SDKs support. Currently, Airstack offer 3 different SDKs:

| SDKs                                                                       | Languages                                                                                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Airstack Web SDK](../../web-sdk-reference/overview.md)                    | JS/TS frontend applications, including Vanilla.js, React.js,  React Native, Next.js, etc. | <p>This SDK is perfect for integrating Airstack APIs to any frontend JS/TS-based applications.<br><br>For React-based applications, there are available hooks for you to call the Airstack API and manage states easily.</p>                                                                                                                                                                                                                                                                                                                                    |
| [Airstack Node SDK](../../nodejs-sdk-reference/overview.md)                | JS/TS Node.js backend applications.                                                       | <p>This SDK is perfect for integrating Airstack APIs to any JS/TS Node.js-based backend/server-side applications, including <a href="https://docs.farcaster.xyz/reference/frames/spec#data-structures">Farcaster Frames</a>.<br><br>This SDK might also work on serverless functions, e.g. Vercel Serverless Function. </p><p></p><p>Depending on the cloud runtime, some environment might have limitations that made it incompatible with the SDKs. In that case, you will need to call the API through <a href="direct-api-call.md">Direct API call</a>.</p> |
| [Airstack Python SDK](https://github.com/Airstack-xyz/airstack-python-sdk) | Python-based applications.                                                                | This SDK is perfect for integrating Airstack APIs to any Python-based applications/project, either for application development or data analysis.                                                                                                                                                                                                                                                                                                                                                                                                                |

We recommend that you use Airstack SDKs to integrate Airstack APIs easily to your application. Below are some tutorials to quickly get you started integrating Airstack depending on the tech stack you're using:

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f310">🌐</span> <strong>JavaScript (Browser)</strong></td><td>Learn how to integrate Airstack APIs to your frontend TypeScript/JavaScript application using the Airstack Web SDK.</td><td></td><td><a href="javascript-browser.md">javascript-browser.md</a></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="269b">⚛️</span> <strong>React</strong></td><td>Learn how to integrate Airstack APIs to your React-based application, including both React Web and React Native, using the Airstack Web SDK.</td><td></td><td><a href="react.md">react.md</a></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="23ed">⏭️</span> <strong>Next.js (Browser)</strong></td><td>Learn how to integrate Airstack APIs to your Next.js client-side (browser) application, using the Airstack Web SDK.</td><td></td><td></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f5fc">🗼</span> <strong>Node.js</strong></td><td>Learn how to integrate Airstack APIs to your Node.js application using the Airstack Node SDK.</td><td></td><td><a href="node.md">node.md</a></td></tr><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f40d">🐍</span> <strong>Python</strong></td><td>Learn how to integrate Airstack APIs to your Python-based application using the Airstack Python SDK.</td><td></td><td><a href="python.md">python.md</a></td></tr></tbody></table>

## Others

Otherwise, you can always call the Airstack API directly with existing GraphQL library. For step-by-step guide on how to do that, check out the tutorial below:

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><span data-gb-custom-inline data-tag="emoji" data-code="1f3af">🎯</span> <strong>Direct API Call</strong></td><td>Learn how to use Airstack by making direct API call without Airstack SDK. In this tutorial, you will learn how to integrate in Node.js. However, this should also work in any other tech stacks.</td><td></td><td><a href="direct-api-call.md">direct-api-call.md</a></td></tr></tbody></table>
