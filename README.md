# OverpoweredJS Browser Fingerprinting API
This is a service I made that can be embedded on websites that can track Chromium browser instances between sites.

I recommend only using this on small sites in a way that won't cause issues if something breaks. The server may go down, and the JS only works on some browsers.

It presently doesn't work for Firefox or Safari users, so be aware of that.

It's free, while it's up. Feel free to stress test it, let me know if your `clusterUUID` is unique to your device. If it isn't, please create an Issue with the `hash` supplied in the response.

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

# Preliminary Terms of Service
Executing the API will cause hashed information from a user's browser to be stored on our servers, among other information like HTTP headers. The information collected will be used to improve our product.

Using the API on your site or app without user consent may be illegal in certain jurisdictions.

Use with caution. I cannot give legal advice and I am not responsible for your actions. Don't do anything stupid, known abuse will be blacklisted without warning.

### (c) 2024 Joe Rutkowski <Joe+opjs@dreggle.com>
