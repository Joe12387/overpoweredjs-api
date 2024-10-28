# OverpoweredJS API
## Overview

OverpoweredJS is a browser fingerprinting API designed to identify and track browser instances. This API aids in distinguishing unique users and detecting potential bots, enhancing security for website operators. The service currently supports a wide range of browsers and is evolving into a commercial API priced at $1 per 1,000 requests, though it remains free to use without signup or API key in its prototype stage.

## Browser Compatibility

 * Supported Browsers: As of October 2024, OverpoweredJS supports all modern browsers, including Chromium-based options (Google Chrome, Microsoft Edge, Opera) as well as Firefox, Brave, and Safari.
 * Browser Consistency: While most browsers can generate unique clusterUUID identifiers, some configurations or environments (e.g., Safari, Brave) may produce duplicates, which may impact uniqueness scoring.

## Use

Embed the API on any page served via HTTPS:
```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

Get the fingerprint object & send it to the server, then log the results:
```js
opjs().then((fp) => console.log(fp));
```

## API Response Structure

When you make a request to the OverpoweredJS API, you’ll receive a JSON response similar to this:
```json
{
  "clusterUUID": "AB-CDE-FGH-IJK",
  "botScore": 1,
  "uniquenessScore": 5,
  "hash": "484f9fd6ac89ab21042fd7540bbbe95e6453433ae0111b945d86b6d0ed98e616"
}
```

## Parameters

* `clusterUUID`: A unique identifier for a browser instance, allowing tracking. This value is designed to remain consistent across sessions, helping differentiate users and detect returning browsers.
* `botScore`: A score from 1 to 5 indicating the likelihood of the user being a bot:
  * 1: Likely a human.
  * 2: Possibly a human but uncertain.
  * 3: Inconclusive; refreshing may yield a clearer result.
  * 4: Possible bot (low confidence).
  * 5: Likely a bot (high confidence).
* `uniquenessScore`: A scale from 1 to 5 representing the fingerprint’s uniqueness:
  * 5: Highly unique; very unlikely to match other devices.
  * 4: Probably unique, with minor similarities to other devices.
  * 3: Unknown or unclear.
  * 2: Possibly not unique, sharing similarities with multiple devices.
  * 1: Not unique; likely to match other devices.
* `hash`: A hash of the fingerprint data for reference and troubleshooting.

## Getting Started

Embed the OverpoweredJS API script on any HTTPS page to start using it:
```html
<script src=“https://v0.telemetry.overpoweredjs.com/opjs.min.js”></script>
```

Invoke the API by calling:
```js
opjs().then((fp) => console.log(fp));
```

## Use Guidance

The `botScore` and `uniquenessScore` parameters help determine the nature of the user or browser instance for security purposes. Here's how to interpret and use these values effectively:

### botScore

`botScore` ranges from 1 to 5, indicating the likelihood of the user being a bot.

#### Understanding botScore

`botScore` can be used to set security thresholds based on the risk profile of the application:

- 5 (Probably a bot): High confidence that the user is a bot. Indicators strongly suggest automated behavior.
- 4 (Maybe a bot): Medium confidence of bot-like behavior; patterns suggest automation, though some indicators could be from a human.
- 3 (Inconclusive): Ambiguous result; some indicators are bot-like, but the overall profile does not strongly suggest automation. This could be a bot, or an indication that some APIs were unavailable. Refreshing the page may resolve this.
- 2 (Maybe a human): Likely human, but a few indicators raise slight uncertainty.
- 1 (Probably a human): High confidence that the user is a human.

#### Usage Guidance for botScore

- Low-Risk Applications: Block only scores of 5 (likely a bot).
- Moderate-Risk Applications: Block scores of 4 and 5, as they suggest bot behavior with medium to high confidence.
- High-Risk Applications: Consider blocking scores of 3 or higher. Note that a score of 3 indicates a not inherently suspicious discrepancy, which may be transient and correctable by refreshing the page and calling the API again.

Blocking users with a botScore of 1 or 2 is strongly discouraged, as these are likely human users, and such scores generally indicate legitimate activity.

### uniquenessScore

The uniquenessScore (1 to 5) indicates how unique or trackable a browser fingerprint is, with higher scores reflecting greater distinctiveness. Use this score to set your app’s security level based on how critical it is to differentiate users:

 - 5 (Unique): Highly distinctive, probably represents a single device. Includes Google Chrome, Microsoft Edge, Opera, and other Chromium browsers that do not employ defenses from browser fingerprinting.
 - 4 (Somewhat Unique): Likely distinct but may share minor similarities with other devices, resulting in multiple devices being grouped with the same clusterUUID. This is also the highest level a non-persistent fingerprint can attain. Includes Safari in regular browsing.
 - 3 (Unknown): It's unclear how unique the device's characteristics are compared to other devices.
 - 2 (Low Uniqueness): Likely similar to many other devices, common configurations. Includes Brave and Firefox.
 - 1 (Not Unique): Shares substantial similarities, matching numerous other devices. Only used to identify Safari in private mode.

#### Usage Guidance for uniquenessScore

 - High-Security Applications: Accept only users with a uniquenessScore of 5 to ensure maximum tracability and minimal fingerprint overlap. This will heavily impact users using privacy-focused browsers, including Safari.
 - Medium-Security Applications: Accept scores of 4 and 5 to allow mostly unique instances while maintaining some tolerance for UUID collisions. This will allow most browsers, including Safari as long as it's not in a private window or a tab.
 - Low-Security Applications: Accept scores of 3, 4, and 5, allowing for broader access while still filtering out the least unique instances.

You, of course, do not have to block anyone based on uniqueness, but this should give you an idea of how to use this if trackability is important to you.

## Terms of Use and Legal Considerations

This API should be used responsibly for security purposes. Use may be subject to privacy regulations, so explicit user consent is recommended for broader applications. Misuse or abuse may lead to blacklisting.

For more details or to report issues, please reach out via email or on GitHub.

## Contact:

 * Email: Joe@dreggle.com
 * GitHub: Joe12387

## License
© 2024 Joe Rutkowski (Joe12387)
