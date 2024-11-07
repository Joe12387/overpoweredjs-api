# OverpoweredJS API Documentation

## November Update
Our infrastructure is fully scaled, and we’re currently focused on developing the control panel and website for our product. We’ve chosen DigitalOcean’s App Platform, which enables instant scaling based on demand. At present, our service operates on a blend of non-autoscaled shared and dedicated resources, as we have no paying customers. However, if you’re interested in accessing this service on dedicated, redundant, and autoscaled hardware for enhanced performance, please reach out: Joe@dreggle.com

## Overview
OverpoweredJS is a browser fingerprinting API designed to identify and track browser instances. It helps website operators distinguish unique users and detect potential bots, enhancing website security. The service currently supports a wide range of browsers and is evolving into a commercial API priced by usage, though it remains free to use without signup or API key in its prototype stage.

[View Demo](https://overpoweredjs.com/demo.html)

## Browser Compatibility

- Supported Browsers: As of October 2024, OverpoweredJS supports all modern browsers, including Chromium-based browsers (Google Chrome, Microsoft Edge, Opera), as well as Firefox, Brave, and Safari.
- Browser Consistency: While most browsers can generate unique clusterUUID identifiers, some configurations or environments—particularly Safari, Firefox, and Brave—may produce collisions (instances where two or more devices share the same clusterUUID; view uniqueness for more information).

## Getting Started

To start using OverpoweredJS, embed the API script on any page served via HTTPS:

```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

Invoke the API by calling:

```js
opjs().then((fp) => console.log(fp));
```

This function retrieves the fingerprint object, which you can then send to your server or use directly in your client-side code.

## API Response Structure

When you call the OverpoweredJS API, it returns a fingerprint object similar to the following:

```json
{
  "clusterUUID": "AB-CDE-FGH-IJK",
  "botScore": 1,
  "uniquenessScore": 5,
  "hash": "484f9fd6ac89ab21042fd7540bbbe95e6453433ae0111b945d86b6d0ed98e616",
  "authToken": "117756340268441600"
}
```

### Parameters

- clusterUUID: A unique identifier for a browser instance, designed to remain consistent across sessions. It helps differentiate users and detect returning browsers.
- botScore: A score from 1 to 5 indicating the likelihood of the user being a bot.
- uniquenessScore: A score from 1 to 5 representing the fingerprint’s uniqueness.
- hash: A hash of the fingerprint data, useful for reference and troubleshooting.
- authToken: A token that can be sent to our servers to verify that the request is genuine and not forged by other means. See verification.

## Verification

Requests from the client containing only `clusterUUID` or `botScore` can be forged, so an `authToken` is provided for verification. The `authToken` is valid for five minutes after the `opjs` function resolves.

To verify the `authToken`, use a simple GET request:

`https://verification.overpoweredjs.com/verify/yourAuthTokenHere`

For example, if your token is `117756340268441600`, send a GET request to:

`https://verification.overpoweredjs.com/verify/117756340268441600`

If the token is valid and has not expired, the server will respond with HTTP status code 200 and return the original object, excluding the `authToken`. Otherwise, a 404 Not Found will be sent.

## Usage Guidance

The botScore and uniquenessScore parameters help determine the nature of the user or browser instance for security purposes. Here’s how to interpret and use these values effectively:

### botScore

The botScore ranges from 1 to 5, indicating the likelihood that the user is a bot.

#### Understanding botScore

- 5 (Probably a bot): High confidence; strongly suggests automation.
- 4 (Maybe a bot): Medium confidence; environment suggests automation.
- 3 (Inconclusive): Ambiguous result; does not inherently suggest automation. This could be a bot or might indicate that some APIs were unavailable. Refreshing the page may resolve this.
- 2 (Maybe a human): Likely human, but a few indicators raise slight uncertainty.
- 1 (Probably a human): High confidence that the user is a human.

##### Usage Recommendations for botScore

- Low-Risk Applications: Consider blocking users with a botScore of 5 (likely bots).
- Moderate-Risk Applications: Consider blocking users with a botScore of 4 or 5, as they suggest bot behavior with medium to high confidence.
- High-Risk Applications: Consider blocking users with a botScore of 3 or higher. Note that a score of 3 indicates ambiguity, which may be resolved by refreshing the page and calling the API again.

Blocking users with a botScore of 1 or 2 is strongly discouraged, as these scores generally indicate legitimate human users.

### uniquenessScore

Note: This metric is still in development and is not final.

The uniquenessScore ranges from 1 to 5, indicating how unique or trackable a browser fingerprint is. Higher scores reflect greater distinctiveness.

#### Understanding uniquenessScore

- 5 (Unique): Highly distinctive; probably represents a single device. Includes browsers like Google Chrome, Microsoft Edge, Opera, and other Chromium-based browsers that do not employ defenses against browser fingerprinting.
- 4 (Somewhat Unique): Likely distinct but may share minor similarities with other devices, possibly resulting in multiple devices being grouped under the same clusterUUID. This is the highest level a non-persistent fingerprint can attain. Includes Safari in regular browsing mode.
- 3 (Unknown): Unclear how unique the device’s characteristics are compared to others.
- 2 (Low Uniqueness): Likely similar to many other devices; common configurations.
- 1 (Not Unique): Shares substantial similarities with numerous other devices. Only used to identify Safari in private mode.

##### Usage Recommendations for uniquenessScore

Use the uniquenessScore to adjust your application’s security measures based on how critical it is to differentiate users. Higher scores suggest that the fingerprint is more unique and reliable for tracking purposes.

## Terms of Use and Legal Considerations

This API should be used responsibly for security purposes. Usage may be subject to privacy regulations such as GDPR or CCPA, so explicit user consent is recommended for broader applications. Misuse or abuse of the API may lead to blacklisting.

## Contact

For more details or to report issues, please reach out via email or GitHub:

 * Email: Joe@dreggle.com
 * GitHub: Joe12387

## License
© 2024 Joe Rutkowski (Joe12387)
