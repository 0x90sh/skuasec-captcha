# SkuaSec Captcha

SkuaSec Captcha is an advanced invisible CAPTCHA solution designed to protect your web applications from malicious bots and automated attacks without disrupting the user experience. Unlike traditional CAPTCHAs that require user interaction, SkuaSec Captcha works seamlessly in the background, identifying and mitigating threats while allowing legitimate users to interact with your site unimpeded.

[Use free SkuaSec Captcha](https://skuasec.com/)

## Key Features

- **Invisible Protection:** No user interaction required; works behind the scenes to block bots.
- **Seamless Integration:** Easy to integrate with minimal code changes on both the frontend and backend.
- **Customizable Settings:** Fine-tune the CAPTCHA behavior, including session TTL and validation limits.
- **Real-Time Validation:** Instant verification of CAPTCHA challenges via a simple API call.
- **Enhanced User Experience:** Keep your website secure without adding friction to the user journey.

SkuaSec Captcha is the ideal solution for websites that need robust bot protection while maintaining a smooth and uninterrupted user experience.

# SkuaSec Captcha Documentation

This documentation provides an overview of how to integrate and use SkuaSec Captcha in your web application.

## Table of Contents
- [Installation](#installation)
- [Setup](#setup)
- [Frontend Implementation](#frontend-implementation)
- [Backend Implementation](#backend-implementation)
- [Settings](#settings)

## Installation
To install SkuaSec Captcha, include the following script in the `<head>` section of your HTML:

```html
<script src="https://skuasec.com/captcha/js"></script>
```

## Setup
Add a `<div>` with the id `skuasec` and your public key anywhere in the `<body>` of your HTML, preferably within the form you want to protect:

```html
<div id="skuasec" public-key="your-public-key"></div>
```

The `challenge-id` element will be automatically created within this div upon successful captcha completion:

```html
<input type="hidden" name="challenge-id" id="challenge-id" value="challenge-id-value">
```

Optionally, you can add the attribute `auto-button="true"` to automatically disable and re-enable all submit buttons based on the captcha completion status:

```html
<div id="skuasec" public-key="your-public-key" auto-button="true"></div>
```

## Frontend Implementation
To handle captcha completion on the frontend, listen to the `skuaCompletion` event:

```javascript
document.addEventListener('skuaCompletion', (event) => {
    console.log('Event details:', event.detail);
    console.log('Event success:', event.detail.success);
    console.log('Event message:', event.detail.message);
    console.log('Event challenge_id:', event.detail.challenge_id);
});
```

The event contains `success`, `message`, and `challenge_id` details which can be used to customize your frontend implementation.

## Backend Implementation
Validate the captcha challenge in your backend before processing the rest of your code.

```http
POST /captcha/challenge/validate/[your-secret-key] HTTP/X.X
Host: skuasec.com
Content-Type: application/x-www-form-urlencoded

challenge_id=[challenge_id]
```

Below is a simplified PHP example:

```php
if (!isset($_POST['challenge-id'])) {
    echo("Challenge not completed.");
    return;
}
$challengeid = $_POST['challenge-id'];

$curl = curl_init();
curl_setopt_array($curl, array(
    CURLOPT_URL => 'https://skuasec.com/captcha/challenge/validate/your-secret-key',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => 'challenge_id=' . $challengeid,
    CURLOPT_HTTPHEADER => array(
        'Content-Type: application/x-www-form-urlencoded'
    ),
));

$response = curl_exec($curl);
curl_close($curl);
$response_data = json_decode($response, true);

if (isset($response_data['success']) && $response_data['success'] === true) {
    echo('Validation: <strong style="color: green;">Successful</strong>');
} else {
    echo('Validation: <strong style="color: red;">Failed</strong>');
}
```

## Settings
Configure the session TTL and validation amount settings to control the number of validations per challenge ID and the time to live for a challenge ID:

- **Session TTL**: Set the duration a challenge ID remains valid.
- **Validations Amount**: Limit the number of times a challenge ID can be validated.
