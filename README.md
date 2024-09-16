# OverpoweredJS Browser Fingerprinting API

## Overview

OverpoweredJS is a browser fingerprinting API designed to track Chromium browser instances across different websites. This service can be embedded into websites to help identify and track unique browser instances for security purposes.

**Note:** This API is currently in a prototype stage. It is recommended for use on small-scale sites where potential issues will not cause significant disruptions. The server may experience downtime, and the JavaScript functionality is limited to certain browsers.

## Browser Compatibility

- **Supported Browsers:** As of August 2024, the prototype service supports most Chromium-based browsers, including Google Chrome, Microsoft Edge, and Opera.
- **Unsupported Browsers:** The API currently does not support Safari. Support for other browsers such as Firefox, Brave, and Safari is under development. These browsers may experience collisions, resulting in the same `clusterUUID` being assigned to different browser instances. For now, Safari requests are rejected by the API.

## Usage Terms

- The API is free to use in its current prototype state. Please be aware that the hostname may change if traffic becomes overwhelming.
- Users are encouraged to stress-test the API and report any issues. If your `clusterUUID` is not unique to your device, please create an issue and include the `hash` provided in the response.
- Please use this service responsibly. The aim is to enhance security without infringing on user privacy.

## Demo

Visiting the demo URL will send information from your browser to our servers for fingerprint processing. Your information will be used solely to improve our security product.

**[OverpoweredJS Browser Fingerprinting Demo](https://overpoweredjs.com/demo.html)**

## Getting Started

To use the OverpoweredJS API on any HTTPS-served page, embed the following script:
```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

Retrieve the fingerprint object and send it to the server, then log the results:
```js
opjs().then((fp) => console.log(fp));
```

After invoking the `opjs` function, you will receive a response similar to the following:
```json
{
  "clusterUUID": "0W-C9Q-WS6-OHK",
  "hash": "981ea46cc95667439294e31fffd8d7c060a0e9f7f3f553a7c4943fa7541d9747",
  "botScore": 1
}
```

**Response Parameters:**

- **`clusterUUID`**: A (hopefully) unique identifier attached to the browser instance. It is intended to remain consistent across sessions.
- **`hash`**: The hash of the JSON object sent to the server. This value may change over time, but the `clusterUUID` should remain the same.
- **`botScore`**: A score ranging from 1 to 5, where 5 indicates the highest likelihood of the user being a bot.

## Future Developments

This service is intended to evolve into a commercial API, offering an affordable alternative to existing SaaS fingerprinting solutions. The goal is to make it as cost-effective as possible, potentially even free for low-traffic use cases.

Participation is welcome regardless of your traffic levels or future plans to subscribe to the service upon its full release.

## Preliminary Terms of Service

By using the OverpoweredJS API, you acknowledge that information from users' browsers, along with other data such as HTTP headers, will be stored on our servers. This information will be used exclusively to improve our product.

**Legal Considerations:**

- **User Consent:** Utilizing the API on your site or application without obtaining user consent may be illegal in certain jurisdictions and under specific circumstances. While data privacy regulations like GDPR generally allow security applications to operate without explicit consent, other uses—such as marketing—may require it to avoid legal penalties.
- **Recommendation:** It is advisable to obtain explicit user consent regardless of the application, as data privacy regulations are continually evolving.
- **Disclaimer:** We cannot provide legal advice and are not responsible for your actions. Please use the API responsibly and in compliance with all applicable laws.

**Abuse Policy:**

- Any known misuse of the API will result in immediate blacklisting without prior notice.

## Contact Information

For any issues or inquiries, please feel free to reach out:

- **Email:** [Joe@dreggle.com](mailto:Joe@dreggle.com)
- **GitHub:** [Joe12387](https://github.com/Joe12387)

## License

© 2024 Joe Rutkowski (Joe12387)
