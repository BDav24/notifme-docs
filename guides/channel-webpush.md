---
layout: default
title: Send a webpush
menu:
  - title: Prepare your site
    anchor: '#configure'
  - title: Webpush content
    anchor: '#content'
  - title: Webhook events
    anchor: '#events'
---

## Send a webpush

<div>
  <img src="/notifme-docs/assets/img/webpush-top.png"
    style="float: right; margin: 0 0 40px 40px; max-width: calc((100% - 40px) / 2);" />
</div>

Webpush notifications are messages that come from a site. You get them through your browser on your
desktop or your Android device even if your site or the browser is closed.

Push notifications are an incredibly effective and cheap way to build deeper user engagement with
your application without asking them their email or to install an app.

<a id="configure"></a>
### [Prepare your site](#configure)

>_**Important**: your must serve the page you ask the permission from and the service worker with
`HTTPS` only. If you don't have support for `HTTPS` please [contact us](https://www.notif.me/contact).
During development `HTTP` is allowed by the browser on `localhost`._

There are 2 steps in order to send a webpush to a user with the help of our SDK.

#### 1. Register a service worker that will handle a push event and show the notification

Serve at the **top-level root** of your site directory a file called `sw.js`
(i.e. `https://yoursite.com/sw.js`) containing the following code:

```javascript
self.NOTIFME_ENDPOINT = '[subdomain].notif.me' // replace subdomain
self.NOTIFME_ENV = 'Production' // 'Development' in local

importScripts('https://[subdomain].notif.me/static/js/sdk.js')
```

#### 2. Asking users the permission to receive your notifications

Add the following code to your site:

```html
<script type="text/javascript" src="https://[subdomain].notif.me/static/js/sdk.js"></script>
<script type="text/javascript">
  const notifme = new Notifme('demo.dev.notif.me', 'Production')
  /*
   * When you are ready to ask the permission to your user, use the register
   * function. It might after a click on a button or anything you want.
   */
  if (notifme.isWebpushSupported()) {
    notifme.register('sw.js').then((token) => {
      // Send and save this token in your backend.
    });
  }
</script>
```

The register function displays a native browser prompt to the user asking him to allow your
notifications.

![prompt](/notifme-docs/assets/img/webpush-prompt.png)

The register function returns a `Promise` which be resolved with a token you have to save in your
backend. This token will be used as the user endpoint in the parameter `to`.

```json
{
  "to": {
    "webpush": "[token]"
  }
}
```

> **Note**: to avoid frustration or misunderstanding of your users, ask them to subscribe to your
webpushs at a time when their benefit is obvious. You can some best practices
[here](https://web-push-book.gauntface.com/chapter-03/01-permission-ux/).

<a id="content"></a>
### [Webpush content](#content)

![ui](/notifme-docs/assets/img/webpush-ui.png)

Notif.me API does not reinvent the wheel and uses the standard JSON to describe a webpush with only
one additional field: `redirects`.

Exemple of a JSON query:

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

* `redirects` (object): a hashmap of URLs to be opened when the user interacts with the notification.
The key is the name of the action clicked. A special key `default` is used when the user clicks on
the global notification window. In the above example if the user clicks on the action button "Share
with your friends" a new tab will be opened with the URL `https://www.notif.me/?click=share` while
if he clicks on the global notification the tab will load `https://www.notif.me`.
If you don't provide URLs in `redirects` nothing will happen but you will receive a callback on the
configured webhook.

Apart from the `requireInteraction` field, you can use
[{% raw %}{{ mustache }}{% endraw %}](https://mustache.github.io/mustache.5.html) templates on all
fields.

> **Note**: some fields are currently only supported on Chrome or/and Android.

<a id="events"></a>
### [Webhook events](#events)

You can register a webhook for these events (from the dashboard or the API):

* `delivered`: the browser (precisely the service worker) has received the payload of the webpush.
* `opened`: the webpush has been displayed to the user
* `clicked`: the user has clicked on an action button or on the global notification. If an action
has been clicked the payload will contain an info object with the action.
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
* `closed`: the user has closed the notification
