---
layout: default
title: Send a webpush
menu:
  - title: Prepare your site
    anchor: '#prepare'
  - title: The content
    anchor: '#content'
  - title: The events
    anchor: '#events'
---

## Send a webpush

<div>
  <img src="/notifme-docs/assets/img/webpush-top.png"
    style="float: right;margin: 0 0 40px 40px;max-width: calc((100% - 40px)/2);g" />
</div>


Web push notifications are messages that come from a site. You get them on your desktop or your Android device
even if your site or the browser is not opened.

Push notifications are an incredibly effective and cheap way to build deeper user engagement with your application
without asking them their email or to install an app.

<a id="prepare"></a>
### [Prepare your site](#prepare)

_**Important**: Your must serve the page you ask the permission and the service worker with `HTTPS`
only. If you don't have support for `HTTPS` please contact us to an alternative solution. During development
`HTTP` is allowed by the browser only on `localhost`_

There are 2 steps in order to send a webpush to a user with the help of our SDK.

#### Register a service worker that will handle a push event and show the notification.

Serve at the **top-level root** of your site directory a file called `sw.js`
(ie `https://yoursite.com/sw.js`) containing the following code

```javascript
self.NOTIFME_ENDPOINT='[subdomain].notif.me';
self.NOTIFME_ENV='Production';

importScripts('https://[subdomain].notif.me/static/sdk.js');
```

#### Asking them the permission for you site to show a Notification.

Add the following code to your site

```html
<script type="text/javascript" src="https://[subdomain].notif.me/static/sdk.js"></script>
<script type="text/javascript">
  const notifme = new Notifme('demo.dev.notif.me', 'Production');


  /*
   * When you are ready to ask the permission to your user,
   * use the register function
   */
  if (notifme.isWebpushSupported()) {
      notifme.register().then((token) => {
        // send this token to your backend
      });
  }
</script>
```

The register function displays a native browser prompt to the user asking him to allow your site to show
notification.

![prompt](/notifme-docs/assets/img/webpush-prompt.png)

The register function returns a `Promise` which be resolved with a token you have to save on your backend.
This token will be used as the endpoint on the `to` parameter.

```json
{
  "to": {
    "webpush": "[token]"
  }
}
```

> **Note**: To avoid frustration or misunderstanding of your users, Ask them to subscribe to push
at a time when the benefit is obvious. You can find some good advices
[here](https://web-push-book.gauntface.com/chapter-03/01-permission-ux/)

<a id="content"></a>
### [The content](#content)

![ui](/notifme-docs/assets/img/webpush-ui.png)

Notif.me does not reinvent the wheel and use the standard JSON to describe a webpush with one additional
fields `redirects`

Exemple of a JSON.

```json
{
  "title": "Welcome {% raw %}{{firstname}}{% endraw %}",
  "body": "We're very happy to welcome you on board",
  "icon": "https://fakeimg.pl/100x100/",
  "image": "https://fakeimg.pl/400x240/",
  "badge": "https://fakeimg.pl/80x80/",
  "actions": [
    {
      "action": "share",
      "title": "Share with your friends",
      "icon": "https://cdnjs.cloudflare.com/ajax/libs/ionicons/2.0.1/png/512/android-share.png"
    }
  ],
  "requireInteraction": true,
  "redirects": {
    "default": "https://www.notif.me",
    "share": "https://www.notif.me/?click=share"
  }
}
```

* `redirects` (object): A hashmap or urls to be opened when the user interacts with the notification.
The key is the name of the action clicked. A special key `default` is used when the user cliks on the
global notification window. In the above example if the user clicks on the action button "Share with your friends"
a new tab will be opened browsing `https://www.notif.me/?click=share` while if he clicks on the global notification
the tab will load `https://www.notif.me`.
If you don't provide urls in `redirects` nothing will happen but you will receive a callback on the configured
webhook.

Apart from the `requireInteraction` field, you can use {% raw %}{{Mustache}}{% endraw %} language on all fields.

> **Note**: some fields are currently only supported on Chrome or/and Android

<a id="events"></a>
### [The events](#events)

You can register a webhook for these events (from the dashboard or the Api):

* `delivered`: The browser (precisely the service worker) has received the payload of the webpush.
* `opened`: The webpush has been displayed to the user
* `clicked`: The user clicks on an action button or on the global notification. If an action has been clicked
the payload will contain an info object with the action.
```json
{
    "requestId": "XXX",
    "channel": "webpush",
    "event": {
      "created": "[the date of the event]",
      "name": "clicked",
      "info": {
        "action": "[action]"
      }
    }
}
```
* `closed`: The user has closed the notification
