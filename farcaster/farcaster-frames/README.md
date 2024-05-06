---
description: Learn how to build Farcaster Frames with Airstack.
---

# 🖼️ Farcaster Frames

## How to build Farcaster Frames with Airstack

Building a Farcaster Frame? Read the official Farcaster Frames specs [here](https://docs.farcaster.xyz/learn/what-is-farcaster/frames) to get started.

The easiest way to get started building and deploying a Frame is with [FROG](https://www.paradigm.xyz/2024/02/frames) (Framework for Farcaster Frames) or [Frames.js](https://framesjs.org) or using Airstack's Frames SDKs.

* The benefit of using Frog is that it deploys a Frame server for you and also provides very simple routing, components, and a built-in debugger. [**Airstack Frog Recipes and Middleware** ](airstack-frog-recipes-and-middleware/)enable you to quickly build Frames with Frog and Airstack, configuring hubs, composing onchain data, and launching a Frame in just a few minutes.&#x20;
* _Frames.js_ a react based framework for making frames. Debugger included. [**Airstack frames.js middleware**](airstack-framesjs-middleware/) helps you easily build build Frames with frames.js middleware. Seamlessly integrate casts, likes, channels, profiles, follows, trending mints, tokens, swaps and everything onchain.
*   #### [Airstack Frames SDK](https://docs.airstack.xyz/airstack-docs-and-faqs/farcaster/farcaster-frames?\_gl=1\*1f8wz60\*\_ga\*MTA2Mzg2NjgzNC4xNzA5OTI2OTY0\*\_ga\_6PP294SC61\*MTcxMzgyODI1Ny4yNDcuMC4xNzEzODI4MjU3LjAuMC4w)&#x20;

    Empowers developers to seamlessly integrate everything Airstack offers into Frames using just a few lines of code



Once you have set up your Frame server with the right meta tags, you will want to know which users interacted with your Frame so that you can take the next appropriate action.

1. **Use** [**Airstack Validation API**](frames-validator.md) **to validate** the incoming user interaction and get details about the interacting user, cast author, and cast itself. For a tutorial on how to implement this, click [here](frames-validator.md).
2. To test Frames on your local machine, set up [ngrok](https://ngrok.com/download) and use ngrok as your Frame's POST URL.

Now 🪄 **build some magic into your Frames. Use** with Airstack [**Onchain Kit for Farcaster Frames**](https://docs.airstack.xyz/airstack-docs-and-faqs/guides/farcaster/airstack-onchain-kit-for-farcaster-frames) which contains dozens of simple guides for working with Frames, integrating Farcaster and onchain data. For easy integration, Airstack provides you with [**Airstack Frog Recipes**](airstack-frog-recipes-and-middleware/) **and** [**Airstack Frames SDK**](https://github.com/Airstack-xyz/airstack-frames-sdk) to integrate onchain data to your Farcaster Frames within just a few lines of code. Here are just some of the things you can do with Onchain Kit:

* Get user’s channels
* Get follows and followers of followers
* Get mint recommendations
* Get tokens and token holders on Base, Zora, Ethereum, Gold
* Get ENS
* Get friends of friends

## More Resources

* [Awesome Frames](https://github.com/davidfurlong/awesome-frames)
* [Coinbase Onchain Kit](https://onchainkit.xyz/)
