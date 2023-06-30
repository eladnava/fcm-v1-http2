# fcm-v1-http2

[![npm version](https://badge.fury.io/js/fcm-v1-http2.svg)](https://badge.fury.io/js/fcm-v1-http2)

Send multicast notifications using HTTP/2 multiplexing through the [FCM HTTP v1 API](https://firebase.google.com/docs/reference/fcm/rest/v1/projects.messages/send).

Supported features:

- [X] HTTP/2 session & stream concurrency
- [X] Token batching support
- [X] Uninstall detection
- [X] Retry mechanism

## Usage

First, install the package using npm:

```shell
npm install fcm-v1-http2 --save
```

Then, start using the package by importing and instantiating it:

```js
const fcmV1Http2 = require('fcm-v1-http2');

// Create a new client
const client = new fcmV1Http2({
  // Pass in your service account JSON private key file (https://console.firebase.google.com/u/0/project/_/settings/serviceaccounts/adminsdk)
  serviceAccount: require('./service-account.json'),
  // Max number of concurrent HTTP/2 sessions (connections)
  maxConcurrentConnections: 10,
  // Max number of concurrent streams (requests) per session
  maxConcurrentStreamsAllowed: 100
});

// Populate array with any number of FCM device tokens
const tokens = ['ccw_syAXSNOY9ml-Kqh9wo:APA91bHAEQccW1ZpbPvsGc0LFyjEthAt_GZO7HkBGiKounM................uIDEHijb4UR5f3dhyjhO5IbiWhJAA7RVp63KSFCg384PR7nfKADReWUONEJlCnHo15WwZagVTmFcgW'];

// Set FCM API v1 message params
// https://firebase.google.com/docs/reference/fcm/rest/v1/projects.messages#Message
const message = {
    data: {
        // Set custom payload
        message: 'Hello World'
    },
    android: {
        // Burst through Doze mode
        priority: 'high'
    }
};

// Send the notification
client.sendMulticast(message, tokens).then((unregisteredTokens) => {
    // Sending successful
    console.log('Message sent successfully');

    // Remove unregistered tokens from your database
    if (unregisteredTokens.length > 0) {
        console.log('Unregistered device token(s): ', unregisteredTokens.join(', '));
    }
}).catch((err) => {
    // Sending failed
    // Log error to console
    console.error('Sending failed:', err);
});
```

## Requirements

* Node.js v12 or newer

## License

Apache 2.0
