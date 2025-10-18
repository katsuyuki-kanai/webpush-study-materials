# Implementation Examples for Web Push Notifications

## Client-Side Subscription

To subscribe to push notifications on the client side, you can use the following code snippet:

```javascript
// Check if service workers are supported
if ('serviceWorker' in navigator && 'PushManager' in window) {
    navigator.serviceWorker.register('service-worker.js')
    .then(function(registration) {
        console.log('Service Worker registered!');

        // Subscribe to push notifications
        return registration.pushManager.subscribe({
            userVisibleOnly: true,
            applicationServerKey: '<YOUR_PUBLIC_VAPID_KEY>'
        });
    })
    .then(function(subscription) {
        console.log('User is subscribed:', subscription);
        // Send subscription to the server
    })
    .catch(function(err) {
        console.error('Failed to subscribe the user: ', err);
    });
}
```

## Server-Side Push Sending

To send push notifications from the server, use the following Node.js example:

```javascript
const webPush = require('web-push');

const vapidKeys = {
    publicKey: '<YOUR_PUBLIC_VAPID_KEY>',
    privateKey: '<YOUR_PRIVATE_VAPID_KEY>'
};

webPush.setVapidDetails(
    'mailto:example@yourdomain.org',
    vapidKeys.publicKey,
    vapidKeys.privateKey
);

const subscription = { /* Your subscription object here */ };

const payload = JSON.stringify({ title: 'Test Notification', body: 'This is a test push notification' });

webPush.sendNotification(subscription, payload)
    .then(response => {
        console.log('Push notification sent:', response);
    })
    .catch(error => {
        console.error('Error sending push notification:', error);
    });
```

## Configuration Examples

### Example VAPID Key Generation

You can generate VAPID keys using the following command:

```bash
npx web-push generate-vapid-keys
```

This will output a public and private key that you can use to configure your push notifications.

### Example Service Worker

Here’s a simple service worker example to handle incoming push notifications:

```javascript
self.addEventListener('push', function(event) {
    const data = event.data ? event.data.json() : {};

    const options = {
        body: data.body,
        icon: 'images/icon.png',
        badge: 'images/badge.png'
    };

    event.waitUntil(
        self.registration.showNotification(data.title, options)
    );
});
```

---
Feel free to modify the code snippets according to your project requirements!