---
description: >-
  Learn how to integrate Captcha functionality into your Farcaster Frames using
  the Airstack Frog Recipes.
---

# 🤖 Captcha Verification

## Step 1: Create Captcha Frames

First, create two new Frames for integrating the captcha:

* `/generate-captcha`: This Frame will be the 1st entry in the captcha flow to initially generate the captcha challenge that is unique to the user.
* `/verify-captcha`: This Frame will be the next entry after generating the captcha. In this Frame, the user's answer for the captcha challenge will be validated.

Additionally, add two new fields `captchaId` and `valueHash` in the initial state of the Frame that you will be used for the captcha verification.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { Frog } from "@airstack/frog";

export const app = new Frog({
  // Initialize the Frames state
  initialState: {
    captchaId: "",
    valueHash: "",
  },
});

app.frame("/generate-captcha", async (c) => {});

app.frame("/verify-captcha", async (c) => {});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { Frog } = require("@airstack/frog");

export const app = new Frog({
  // Initialize the Frames state
  initialState: {
    captchaId: "",
    valueHash: "",
  },
});

app.frame("/generate-captcha", async (c) => {});

app.frame("/verify-captcha", async (c) => {});
```
{% endtab %}
{% endtabs %}

## Step 2: Generate Captcha Challenge

Then, generate a captcha challenge using the [`generatedCaptchaChallenge`](https://www.npmjs.com/package/@airstack/frames#generatecaptchachallenge) function in the `/generate-captcha` Frame.

The function will help you randomly generate two integers for your users, which they will need to provide the correct sum result for verification.

In addition to the two integers, a template Frame image will be given along with the hashed sum results in the `state` field. The `state` field should be stored in the Frame state.

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { generateCaptchaChallenge } from "@airstack/frog";

app.frame("/generate-captcha", async (c) => {
  const { deriveState } = c ?? {};
  const { data, state, image } = await generateCaptchaChallenge();
  // The 2 numbers generated can be used to generate custom Frame image
  const { numA, numB } = data ?? {};
  deriveState((previousState) => {
    // Store `state` data into Frames state
    previousState.captchaId = state.captchaId;
    previousState.valueHash = state.valueHash;
  });
  return c.res({
    image, // Use template Frame image
    intents: [
      <TextInput placeholder="Type Here!" />,
      <Button action="/verify-captcha">Verify</Button>,
    ],
  });
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const { generateCaptchaChallenge } = require("@airstack/frog");

app.frame("/generate-captcha", async (c) => {
  const { deriveState } = c ?? {};
  const { data, state, image } = await generateCaptchaChallenge();
  // The 2 numbers generated can be used to generate custom Frame image
  const { numA, numB } = data ?? {};
  deriveState((previousState) => {
    // Store `state` data into Frames state
    previousState.captchaId = state.captchaId;
    previousState.valueHash = state.valueHash;
  });
  return c.res({
    image, // Use template Frame image
    intents: [
      <TextInput placeholder="Type Here!" />,
      <Button action="/verify-captcha">Verify</Button>,
    ],
  });
});
```
{% endtab %}
{% endtabs %}

In the Frame, it will also render a text input for user to input the captcha answer and a `Verify` button to go to the next Frame `/verify-captcha` for captcha validation.

## Step 3: Verify Captcha Challenge

Once the user clicked on the `Verify` button, they will be redirected to the `/verify-captcha` Frame where [`validateCaptchaChallenge`](https://www.npmjs.com/package/@airstack/frames#validatecaptchachallenge) function will be called to validate the user's captcha answer with the Frame state (generated from [`generateCaptchaChallenge`](https://www.npmjs.com/package/@airstack/frames#generatecaptchachallenge)).

If the validation is successful, `isValidated` field will return `true`. Otherwise, `false`.

Similarly, you can use the `image` field for the Farcaster Frame image, which will provide you a template verification image from [Airstack](https://airstack.xyz).

{% tabs %}
{% tab title="TypeScript" %}
```typescript
import { validateCaptchaChallenge } from "@airstack/frog";

app.frame("/verify-captcha", async (c) => {
  const { inputText, deriveState } = c ?? {};
  const state = deriveState() ?? {};
  const { isValidated, image } = await validateCaptchaChallenge({
    inputText,
    state,
  });
  deriveState((previousState) => {
    // Clear Frames state
    previousState.captchaId = "";
    previousState.valueHash = "";
  });
  return c.res({
    image,
    intents: [
      <Button action={isValidated ? "/test" : "/generate-captcha"}>
        {isValidated ? "Continue" : "Try Again?"}
      </Button>,
    ],
  });
});
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Some code
```
{% endtab %}
{% endtabs %}

🥳 Congratulations, you've just integrated Captcha into your Farcaster Frames using Airstack Frog Recipes!

## Developer Support

If you have any questions or need help integrating Captcha into your Farcaster Frames using the Airstack Frog Recipe, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Airstack Frames SDK Refrence](https://www.npmjs.com/package/@airstack/frames)
* [Farcaster Data](onchain-data.md)
* [Farcaster Hubs](farcaster-hubs.md)
* [Allow List](allow-list.md)
* [Integrate to Existing Frog Project](../integrations/frog.md)
