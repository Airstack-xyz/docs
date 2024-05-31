---
description: >-
  Learn how you can use Airstack to setup channel moderation quickly for FREE
  based on various onchain criteria on your Farcaster channel.
---

# ⚒️ Setup Channel Moderation

First, go to [airstack.xyz](https://airstack.xyz):&#x20;

{% embed url="https://airstack.xyz" %}
Airstack Explorer
{% endembed %}

and search your Farcaster user name in the user search bar:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.28.34.png" alt=""><figcaption><p>Search Farcaster Username</p></figcaption></figure>

Once you selected your Farcaster user name, you will be redirected to your Farcaster user page.

Here you should click on the **Channel Owner** tab:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 10.53.50 AM.png" alt=""><figcaption><p>Channel Owner Dashboard</p></figcaption></figure>

In the **Channel Owner** tab, it will show you all the Farcaster channels that you own.

To start moderating, simply click on the **Moderate** button:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 10.53.50 AM copy.png" alt=""><figcaption><p>Moderate Button Under Channel Owners</p></figcaption></figure>

select all the moderation rules/criteria to moderate your channel:

{% hint style="info" %}
The list of all available moderation rules/criteria is shown [here](overview.md#moderation-options).
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 10.58.18 AM.png" alt=""><figcaption><p>Select Moderation Rules/Criteria</p></figcaption></figure>

configure in the **Always Include Casts By** box whether to:

1. include any casts by owner (this is on by default)
2. include any casts by other whitelisted users (can select up to 5 users in the textbox):

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 10.58.36 AM (1).png" alt=""><figcaption><p>Configured Whose Casts Will Always Be Included</p></figcaption></figure>

Once everything is configured to your needs, then you can simply click on the **Save** button.

Once you have the moderation setup, you will then need to assign the [@notabot ](https://warpcast.com/notabot)to be the moderator of your channel.

To do so, go to your Warpcast channel URL and click on **Edit Channel** button:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.01.25 AM.png" alt=""><figcaption><p>Edit Farcaster Channel on Warpcast</p></figcaption></figure>

This will open a modal where you should click on **Configure moderation**:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.01.34 AM (1).png" alt=""><figcaption><p>Configure Farcaster Channel Moderation</p></figcaption></figure>

and then choose [@notabot](https://warpcast.com/notabot) to be the moderator of your Farcaster channel:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.01.54 AM.png" alt=""><figcaption><p>Select @notabot as moderator</p></figcaption></figure>

Now that you have @notabot set as the moderator of your Farcaster channel, simply go back to the **Channel Owner** dashboard to validate the @notabot assignment as channel moderator by clicking on the **Validate** button:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.02.16 AM.png" alt=""><figcaption><p>Validate @notabot as Channel Moderator</p></figcaption></figure>

When the validation is successful, the Airstack Farcaster Channel Mods will start auto-sweep on the latest 1500 casts and like the casts that fulfill the configured moderation criteria:

{% hint style="info" %}
Auto-sweep will only happen **ONCE** during setup. To manually re-sweep, click this guide [here](manual-sweep.md).
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.00.15 AM.png" alt=""><figcaption><p>Initial Auto-sweep on Farcaster Channel</p></figcaption></figure>

The casts liked by the bot will be the one appearing in the **Main** section of the Farcaster channel.

Once the moderation is completed, a green checklist will appear as shown below:

<figure><img src="../../.gitbook/assets/Screenshot 2024-05-30 at 11.02.28 AM.png" alt=""><figcaption><p>Successful Moderation on Farcaster Channel</p></figcaption></figure>

and you're all done!

🥳 Congratulations, you've just add moderation to your Farcaster channel using Airstack Channel Mods!

## Developer Support

If you have any questions or need help regarding setting up moderation on your Farcaster Channel using the Farcaster Channel Mods, please join our [Airstack's Warpcast Channel](https://warpcast.com/\~/channel/airstack).

## More Resources

* [Farcaster Channel Mod Overview](overview.md)
* [Edit Moderation](edit-moderation.md)
* [Manual Sweep](manual-sweep.md)
