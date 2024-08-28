# OverpoweredJS Browser Fingerprinting API
This is a service I made that can be embedded on websites that can track Chromium browser instances between sites.

I recommend only using this on small sites in a way that won't cause issues if something breaks. The server may go down, and the JS only works on some browsers.

It presently doesn't work for Safari users, so be aware of that.

It's free, while it's up (may change hostname if traffic gets overwhelming). Feel free to stress test it, let me know if your `clusterUUID` is unique to your device. If it isn't, please create an Issue with the `hash` supplied in the response.

Also, I don't want to kill privacy. Please go easy on me.

# Demo
Visiting this URL will cause hashed information from your browser to be sent & stored on our servers automatically so the fingerprint can be processed.

Your information will only be used to improve our security product.

https://v0.telemetry.overpoweredjs.com/demo.html

# Use
Embed the API on any page served via HTTPS:
```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

Get the fingerprint object & send it to the server, then log the results:
```js
opjs().then((fp) => console.log(fp));
```

After calling the `opjs` function, you'll get something back like this:
```json
{
  "clusterUUID": "0W-C9Q-WS6-OHK",
  "hash": "981ea46cc95667439294e31fffd8d7c060a0e9f7f3f553a7c4943fa7541d9747",
  "botScore": 1
}
```

- The `clusterUUID` is the (hopefully) unique ID that is attached to the browser.
- The `hash` is simply the hash of the JSON object sent to the server. It can (and will) change, but the `clusterUUID` should not.
- The `botScore` is a score from 1 to 5 with 5 being the highest likelyhood of being a bot.

As of August 2024, the prototype service can track most Chromium-based browsers such as Google Chrome, Microsoft Edge and Opera.

Support for other browsers such as Firefox, Brave and Safari is in development. These browsers may cause collisions (having the same `clusterUUID` as other browser instances). This may or may not change in the future. For the time being, Safari will be rejected by the API. Apple devices will be the least unique due to the homogeneity of Apple's software and hardware, as well as Apple's continued efforts to make their software resistant to tracking.

This is intended to eventually be a commercial API for those priced out of similar SaaS fingerprinting solutions, and is intended to be as inexpensive as possible (perhaps even free for low amounts of traffic).

You may participate regardless of how much traffic you have or whether or not you're going to sign up for the service when it goes into production.

# Preliminary Terms of Service
Executing the API will cause hashed information from a user's browser to be stored on our servers, among other information like HTTP headers. The information collected will be used to improve our product.

Using the API on your site or app without user consent may be illegal in certain jurisdictions under certain circumstances. While data privacy regulations like GDPR generally permit security applications to function without user consent, other uses like marketing will require user consent in order to avoid being fined. It's recommended to gain explicit user consent regardless of the application as new regulations are always appearing and existing ones may change. Remember, consent is sexy.

Use with caution. I cannot give legal advice and I am not responsible for your actions. Don't do anything stupid, known abuse will be blacklisted without warning.

# Copyright
(c) 2024 Joe Rutkowski (Joe12387) - Joe@dreggle.com - github.com/Joe12387
