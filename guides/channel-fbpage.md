---
layout: default
title: Send a Facebook message
menu:
  - title: Configure your page
    anchor: '#configure'
  - title: Message content
    anchor: '#content'
  - title: Webhook events
    anchor: '#events'
---

<div>
  <img src="/notifme-docs/assets/img/fbpage-top.png"
    style="float: right; margin: 0 0 40px 40px; max-width: calc((100% - 40px) / 3);" />
</div>

## Send a Facebook message

If you are on Facebook and have a Facebook page, you can build a bot for your users to interact
with it.

> _"Now people won't need to download an app to interact with you. Just build your bot and instantly
reach people on whichever device and platform they use."_

You can use also your bot to re-engage users repecting [Facebook policy](https://developers.facebook.com/docs/messenger-platform/policy-overview).

<a id="configure"></a>
### [Configure your Facebook page](#configure)

If you are not comfortable with the Messenger platform please start with the official
[documentation](https://messenger.fb.com/get-started).

Notif.me will use your app to communicate with the Facebook platform. If it's not already done, generate
with your app a `PAGE ACCESS TOKEN`. **Important**: your app needs to be approved by Facebook on `pages_messaging`.
Then configure the Facebook channel on your dashboard.

<!-- If you want Notif.me to be able to receive the events (optional) you need to set the webhook as below and subscribe
it to your Facebook page.

![config](/notifme-docs/assets/img/fbpage-config.png)

> **Note**: You can only set one webhook per application, this is a Facebook restriction but you can
still receive all the events on your backend through a Notif.me webhook. -->

<a id="content"></a>
### [Message content](#content)

For the template payload you can follow the official documentation [from Facebook](https://developers.facebook.com/docs/messenger-platform/send-api-reference#request) ommiting
the recipient field. Instead you have to pass the recipient id (this id is a page scoped user id) in our `to`
field

```json
{
  "to": {
    "fbpage": "[page scoped user_id given by facebook]"
  }
}
```

Each time a user interacts with your page you will receive this user id in the payload of the
callback sent to your webhook.

You can use [{% raw %}{{ mustache }}{% endraw %}](https://mustache.github.io/mustache.5.html)
templates in the payload of the facebook template.

_Example of template:_
```json
{
  "message": {
    "attachment": {
      "type": "template",
      "payload": {
        "template_type": "generic",
        "elements": [
          {
            "title": "Welcome {% raw %}{{firstname}}{% endraw %} ",
            "default_action": {
              "type": "web_url",
              "url": "https://www.notif.me"
            },
            "image_url": "https://fakeimg.pl/800x800/",
            "subtitle": "We're very happy to welcome you on board",
            "buttons": [
              {
                "type": "web_url",
                "url": "https://www.notif.me/?click=share",
                "title": "Share"
              }
            ]
          }
        ]
      }
    }
  }
}
```

<a id="events"></a>
### [Webhook events](#events)

This is a beta feature, please [contact us](https://www.notif.me/contact) if you want more
information.

<!-- Events are available only if you configure the Facebook webhooks to point to
`https://[sudomain].notif.me/api/webhook/facebook`. The `requestId` field refers to the last request
sent to the user through Notif.me. If the user interacts for the first time with your page, this field
is null. -->


<!-- You can register a webhook for these events (from the dashboard or the Api):

* `delivered`: Equivalent of the message_reads event
* `opened`: Equivalent of the message_reads event (duplicate of delivered)
* `clicked`: Equivalent of message_postbacks event. The user clicks on a [postback button](https://developers.facebook.com/docs/messenger-platform/send-api-reference/postback-button).
You will find the original [payload](https://developers.facebook.com/docs/messenger-platform/webhook-reference/postback) in `event.info`
```json
{
    "requestId": "XXX",
    "channel": "webpush",
    "event": {
      "created": "[the date of the event]",
      "name": "clicked",
      "info": {
        "payload": "[USER_DEFINED_PAYLOAD]"
      }
    }
}
```
* `answered`: Equivalent of messages event. You will find the original [payload](https://developers.facebook.com/docs/messenger-platform/webhook-reference/message) in `event.info` -->
