# OverpoweredJS API Documentation

## Introduction

OverpoweredJS is a browser fingerprinting API designed to identify and track browser instances. It helps website operators distinguish unique users and detect potential bots, enhancing website security. The service currently supports a wide range of browsers and is evolving into a commercial API priced by usage. Currently, it remains free to use without signup or API key in its prototype stage.

[View Demo](https://overpoweredjs.com/demo.html)

## Getting Started

### Requirements

- **HTTPS**: OverpoweredJS requires that your web page is served over HTTPS.
- **Supported Browsers**: As of October 2024, OverpoweredJS supports all modern browsers, including Chromium-based browsers (Google Chrome, Microsoft Edge, Opera), as well as Firefox, Brave, and Safari.

### Installation

Include the OverpoweredJS script in your HTML page:

```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

### Basic Usage

Invoke the API by calling:

```javascript
opjs().then((fp) => {
  console.log(fp);
});
```

This function retrieves the fingerprint object, which you can then send to your server or use directly in your client-side code.

**Note**: Running `opjs()` on page load can degrade bot detection results. For effective bot detection, it is advised to load the script on page load and execute `opjs()` only after the user has completed an action, such as clicking a button.

## Quick Start Example

Here is a minimal example of integrating OverpoweredJS into your web page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>OverpoweredJS Integration Example</title>
  <script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
</head>
<body>
  <button id="detectBot">Check Bot Status</button>

  <script>
    document.getElementById('detectBot').addEventListener('click', function() {
      opjs().then((fp) => {
        console.log('Fingerprint:', fp);
        // Send fingerprint data to your server
        fetch('/your-endpoint', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(fp),
        })
        .then(response => response.json())
        .then(data => {
          console.log('Server Response:', data);
        })
        .catch((error) => {
          console.error('Error:', error);
        });
      });
    });
  </script>
</body>
</html>
```

## API Reference

### `opjs()` Function

The `opjs()` function collects the browser fingerprint and returns a Promise that resolves to the fingerprint object.

#### Syntax

```javascript
opjs().then((fingerprint) => {
  // Handle the fingerprint object
});
```

#### Parameters

The `opjs()` function does not take any parameters.

#### Returns

A Promise that resolves to a fingerprint object.

### Fingerprint Object Structure

When you call the OverpoweredJS API, it returns a fingerprint object with the following structure:

```json
{
  "clusterUUID": "AB-CDE-FGH-IJK",
  "botScore": 1,
  "uniquenessScore": 5,
  "hash": "484f9fd6ac89ab21042fd7540bbbe95e6453433ae0111b945d86b6d0ed98e616",
  "authToken": "117756340268441600"
}
```

#### Properties

- **`clusterUUID`** (`string`): A unique identifier for a browser instance, designed to remain consistent across sessions. It helps differentiate users and detect returning browsers.

- **`botScore`** (`number`): A score from 1 to 5 indicating the likelihood of the user being a bot.

- **`uniquenessScore`** (`number`): A score from 1 to 5 representing the fingerprint's uniqueness.

- **`hash`** (`string`): A hash of the fingerprint data, useful for reference and troubleshooting.

- **`authToken`** (`string`): A token that can be sent to our servers to verify that the request is genuine and not forged by other means. See [Authentication and Verification](#authentication-and-verification).

## Authentication and Verification

While the `opjs()` function runs client-side, you may want to verify the authenticity of the data on your server to ensure it hasn't been tampered with.

### Verification Endpoint

The `authToken` provided in the fingerprint object can be used to verify the data with OverpoweredJS servers.

#### Request

To verify the `authToken`, make a GET request to the following endpoint:

```
GET https://verification.overpoweredjs.com/verify/{authToken}
```

Replace `{authToken}` with the actual token.

**Example:**

```
GET https://verification.overpoweredjs.com/verify/117756340268441600
```

#### Response

If the token is valid and has not expired (tokens are valid for five minutes after the `opjs()` function resolves), the server responds with HTTP status code `200 OK` and returns the original fingerprint object, excluding the `authToken`.

**Example Response:**

```json
{
  "clusterUUID": "AB-CDE-FGH-IJK",
  "botScore": 1,
  "uniquenessScore": 5,
  "hash": "484f9fd6ac89ab21042fd7540bbbe95e6453433ae0111b945d86b6d0ed98e616"
}
```

If the token is invalid or has expired, the server responds with HTTP status code `404 Not Found`.

### Server-Side Verification Example

Here's how you might verify the `authToken` on your server using Node.js and the `fetch` API:

```javascript
const authToken = '117756340268441600';

fetch(`https://verification.overpoweredjs.com/verify/${authToken}`)
  .then(response => {
    if (response.status === 200) {
      return response.json();
    } else {
      throw new Error('Invalid or expired authToken');
    }
  })
  .then(fingerprint => {
    console.log('Verified Fingerprint:', fingerprint);
    // Proceed with processing the fingerprint data
  })
  .catch(error => {
    console.error('Verification Error:', error);
  });
```

## Error Handling

### Client-Side Errors

Errors encountered when calling `opjs()` can be handled using the `catch` method of the Promise.

```javascript
opjs()
  .then((fp) => {
    // Handle fingerprint
  })
  .catch((error) => {
    console.error('opjs Error:', error);
  });
```

### Verification Errors

When verifying the `authToken`, you may encounter HTTP errors:

- **`404 Not Found`**: The `authToken` is invalid or has expired.
- **`500 Internal Server Error`**: An unexpected server error occurred.

## Rate Limiting

Currently, there is no explicit rate limiting enforced during the prototype stage. However, excessive or abusive use of the API may result in temporary or permanent blocking.

**Note**: As the service evolves into a commercial API, usage limits and rate limiting policies may be introduced.

## Usage Guidance and Best Practices

### Interpreting `clusterUUID`

- **Purpose**: Identifies the browser or device, designed to remain consistent across sessions.
- **Uniqueness**: Each `clusterUUID` is accompanied by a `uniquenessScore` to indicate how unique it is.

### Interpreting `uniquenessScore`

- **Range**: 1 to 5
- **Description**:
  - **5 (Unique)**: Highly distinctive; likely represents a single device.
  - **4 (Somewhat Unique)**: Likely distinct but may share minor similarities with other devices.
  - **3 (Unknown)**: Ambiguous uniqueness; unclear distinctiveness.
  - **2 (Low Uniqueness)**: Likely similar to many other devices.
  - **1 (Not Unique)**: Shares substantial similarities with numerous other devices.

**Usage Recommendations**:

- Use the `uniquenessScore` to adjust your application's security measures based on how critical it is to differentiate users.
- Higher scores suggest that the fingerprint is more unique and reliable for tracking purposes.

### Interpreting `botScore`

- **Range**: 1 to 5
- **Description**:
  - **5 (Probably a bot)**: High confidence; strongly suggests automation.
  - **4 (Maybe a bot)**: Medium confidence; environment suggests automation.
  - **3 (Inconclusive)**: Ambiguous result; does not inherently suggest automation.
  - **2 (Maybe a human)**: Likely human, but slight uncertainty.
  - **1 (Probably a human)**: High confidence that the user is a human.

**Usage Recommendations**:

- **Low-Risk Applications**: Consider blocking users with a `botScore` of **5**.
- **Moderate-Risk Applications**: Consider blocking users with a `botScore` of **4** or **5**.
- **High-Risk Applications**: Consider blocking users with a `botScore` of **3** or higher. Note that a score of 3 is ambiguous and may be resolved by refreshing the page and calling the API again.

**Important**: Blocking users with a `botScore` of **1** or **2** is strongly discouraged, as these scores generally indicate legitimate human users.

### Best Practices

- **Delayed Execution**: Execute `opjs()` after user interaction (e.g., button click) for better bot detection results.
- **Data Privacy**: Ensure compliance with applicable privacy laws when collecting and storing fingerprint data.
- **Server-Side Verification**: Use the `authToken` to verify data with OverpoweredJS servers to prevent forgery.

## Browser Compatibility

- **Supported Browsers**: All modern browsers, including Chromium-based browsers (Google Chrome, Microsoft Edge, Opera), Firefox, Brave, and Safari.
- **Browser Consistency**: While most browsers generate unique `clusterUUID` identifiers, some configurations—particularly in Safari, Firefox, and Brave—may produce collisions where multiple devices share the same `clusterUUID`.

## Security Considerations

- **Data Security**: Treat fingerprint data as sensitive information. Securely transmit and store it to prevent unauthorized access.
- **User Consent**: Depending on your region and application, you may need to obtain explicit user consent before collecting fingerprint data.
- **Compliance**: Ensure that your use of OverpoweredJS complies with all applicable laws and regulations, such as GDPR and CCPA.

## Terms of Use and Legal Considerations

This API should be used responsibly for security purposes. Usage may be subject to privacy regulations such as GDPR or CCPA, so explicit user consent is recommended for broader applications. Misuse or abuse of the API may lead to blacklisting.

- **Prohibited Uses**: Do not use OverpoweredJS for unlawful activities or in violation of any privacy laws.
- **Liability**: The user is solely responsible for compliance with all applicable laws and regulations.
- **Monitoring**: OverpoweredJS reserves the right to monitor usage and take appropriate action against misuse.

## FAQ

### Can I track people across different websites?

**No**. OverpoweredJS is designed to assign identifiers based on the origin (domain) to provide privacy to site visitors and to ensure scalability. Cross-site tracking is not supported in the standard service.

### I need cross-site tracking. Can you provide it?

We can provide cross-site tracking services using dedicated infrastructure. This service is significantly more resource-intensive and expensive.

**Contact**: If you require cross-site tracking, email Joe@dreggle.com with a detailed explanation of your needs and how you will comply with all applicable privacy laws. You will need to sign an agreement accepting sole responsibility for your use of the service. Unlawful activity will result in service termination and potential reporting to regulatory bodies.

### What is Dedicated Infrastructure?

Our Dedicated Infrastructure allows us to cater to high-traffic sites that require scaling, redundancy, and performance. It involves a custom setup tailored to your needs and may cost thousands of dollars per month, depending on traffic.

**Contact**: For more information, email Joe@dreggle.com.

### Can I license the code and run it myself?

If you are a large company that requires consulting regarding user tracking, bot detection, or similar fields of study, please contact me for more information.

## Support and Contact Information

For more details, support, or to report issues, please reach out via email or GitHub:

- **Email**: Joe@dreggle.com
- **GitHub**: [Joe12387](https://github.com/Joe12387)

## Changelog

### November Update (2024)

- Our infrastructure is fully scaled.
- Developing the control panel and website.
- Transitioned to DigitalOcean’s App Platform for instant scaling.
- Service operates on a blend of non-autoscaled shared and dedicated resources.

## License

© 2024 Joe Rutkowski (Joe12387)
