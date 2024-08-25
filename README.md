# OverpoweredJS Browser Fingerprinting API
This is a service I made that can be embedded on websites that can track Chromium browser instances between sites.

I recommend only using this on small sites in a way that won't cause issues if something breaks. The server may go down, and the JS only works on some browsers.

It presently doesn't work for Firefox or Safari users, so be aware of that.

It's free, while it's up. Feel free to stress test it, let me know if your `clusterUUID` is unique to your device. If it isn't, please create an Issue with the `hash` supplied in the response.

Also, I don't want to kill privacy. Please go easy on me.

# Use
Embed the API on any page served via HTTPS:
```html
<script src="https://v0.telemetry.overpoweredjs.com/opjs.min.js"></script>
```

Get the fingerprint object & send it to the server, then log the results:
```js
opjs().then((fp) => fp.getFingerprint().then((res) => console.log(res)))
```

### (c) 2024 Joe Rutkowski <Joe+opjs@dreggle.com>
