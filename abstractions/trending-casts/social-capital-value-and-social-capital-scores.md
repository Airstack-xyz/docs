---
description: >-
  Learn what Social Capital Value & Social Capital Scores are -- and how they're
  used to fetch high-quality trending casts in the TrendingCasts API.
---

# 😎 Social Capital Value & Social Capital Scores

**Social Capital Value (SCV)** is a metric developed by Airstack to identify high-quality Trending Casts on Farcaster. \
\
The SCV of a cast is determined through the following process:

* Each Farcaster user is assigned a **Social Capital Score (SCS)**, which quantifies their positive impact on the platform.&#x20;
  * This score is derived from a variety of onchain criteria. Users with higher SCSs have more positive downstream influence, as their actions and engagements tends to result in higher quality overall engagement.&#x20;
  * Currently, the SCS is powered by a proprietary algorithm from Airstack. This algorithm is kept confidential to prevent manipulation.
  * Over time, we aim to refine the SCS based on its performance and eventually release it as an open-source protocol.
* The SCS of a user impacts the SCV of a cast when the user engages with it. The type of  engagement also plays a role. The accumulated SCS of all interactions contributes to the cast's overall SCV. Casts are then ranked by their SCV, promoting higher-quality casts.

This methodology ensures that the most impactful casts are highlighted on Farcaster, fostering a constructive community environment.

You can fetch the SCS of any user and SCV of any cast [via the Airstack APIs](https://app.airstack.xyz).