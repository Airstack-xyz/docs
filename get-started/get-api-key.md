---
description: Learn how to get your Airstack API key.
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

# 🗝 Get API Key

## Step 1: Create An Airstack Account

Go to [Airstack Dashboard](https://app.airstack.xyz) and click `Sign In` on the top-right corner of the page.

{% hint style="info" %}
If you do not have an account with Airstack, this will **create a new Airstack account** for you.

Otherwise, it will login to your existing Airstack account.
{% endhint %}

A login popup will appear for you:

<figure><img src="../.gitbook/assets/Screenshot 2023-12-04 at 13.52.36 (2).png" alt=""><figcaption><p>Login Popup</p></figcaption></figure>

You have several options to login:

* **Email & Social**
  * Email (passwordless)
  * Google
  * GitHub
* **Connect Wallet**
  * Metamask
  * Coinbase Wallet
  * WalletConnect

## Step 2: Complete Account Setup

### Step 2.1: Setup Billing

After account creation, you'll be directed to setup your billing to claim **2.5 million free Airstack credit** valued at $50.

To claim it, simply connect a wallet with ENS, Farcaster, or Lens:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Airstack Claim $50 Free Credits</p></figcaption></figure>

Otherwise, you can continue without any free trial and directly add your card to be charged for any of your API usage.

### Step 2.2: Setup Profile

Input your desired **Username**, **Project name**, and **Telegram** to complete your profile:

<figure><img src="../.gitbook/assets/Screenshot 2023-12-04 at 12.29.42.png" alt=""><figcaption><p>Airstack Complete Your Profile</p></figcaption></figure>

## Step 3: Get Production API Key

Once everything is set up, go to the settings page by hovering your mouse to the **Profile icon** on the top right and click [**Profile settings**](https://app.airstack.xyz/profile-settings/profile).

<figure><img src="../.gitbook/assets/Screenshot 2023-12-04 at 12.22.01.png" alt=""><figcaption><p>Profile Icon &#x26; Menu</p></figcaption></figure>

Then, simply click the 3rd entry on the sidebar, [**View API Keys**](https://app.airstack.xyz/profile-settings/api-keys), and copy your **Prod API key**.

## Step 4: Get The Test API Key (Optional)

Test API key will provide you with an API key that is free of usage charge. This is perfect if you need to use Airstack in your development.

{% hint style="info" %}
The test API key is **NOT** suitable for production purposes because it is by designed to have a very low rate limit of **20 requests/min** and bursts of **5 requests/sec**, which is only suitable for testing purposes.
{% endhint %}

To get your test API key, you'll need to **Add Card** on the banner in the [**View API Keys**](https://app.airstack.xyz/profile-settings/api-keys) **page:**

<figure><img src="../.gitbook/assets/Screenshot 2023-12-04 at 12.15.01.png" alt=""><figcaption></figcaption></figure>

You'll be redirected to Stripe to add your card. Your card will **not be charged** until have exhausted all your free Airstack credits.

After a card has been added to your [Airstack](https://airstack.xyz) account, you will see that a new **Test API key** has been added to your dashboard.

Simply copy and use it within your test environment.

## Developer Support

If you have any questions or need help regarding getting your Airstack API key, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

Learn to build more with Airstack using our tutorials:

* [Airstack AI](airstack-ai.md)
* [Quickstart](quickstart/)
  * [React](quickstart/react.md)
  * [Node.js](quickstart/node.md)
  * [Python](quickstart/python.md)
  * [Direct API Call](quickstart/direct-api-call.md)
* [Airstack Explorer (No Code)](airstack-explorer.md)
