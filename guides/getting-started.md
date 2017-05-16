---
layout: default
title: Getting started
menu:
  - title: The devtool
    anchor: '#devtool'
  - title: Quick integration
    anchor: '#integrate'
---

## Getting started

Notif.me is a SAAS service that helps you notify your users on multiple channels.

![img](/notifme-docs/assets/img/get_started_top.png)

<a id="devtool"></a>
### [The devtool](#devtool)

Notif.me is made with love by developers for developers. As developers, **local testing** is
essential for us. This is usually a challenge when working with notifications because they use
external services.

That's why we've bundled a set of tools to use with our service. It brings:

* **The catcher**: A previewer for each kind of notification. In dev mode, all the notifications
you send are forwarded to a catcher running on your localhost. You will see all your previews in
one place.

* **Easy webhook debugging**: The devtool acts as a proxy for the webhook you have
configured on your dashboard. So you will be able to set **local urls** for your webhooks!
No more headeaches with deploying a version just to test a webhook!

* **No more specific "dev" code**: ~~`if (dev) debugMail(mail) else sendWithXXX(mail)`~~ < with
notif.me you only set the right token for each environment and send all your notifications.
> **Note**: on *dev* environments notifications will never be sent to your users (except
registered test users) so... no stress!

Installation

 ```sh
yarn global add notifme-devtool
notifme-devtool tuto
 ```

 (Or with npm: `npm install -g notifme-devtool`)

> **Note**: we also provide a Docker image `docker run -p 1080:1080 --rm -it notifme/devtool tuto`

Then follow the onboarding instructions.

![Getting started gif](/notifme-docs/assets/img/getting-started.gif)

<a id="integrate"></a>
### [Quick integration](#integrate)

Once you have done the tutorial on the devtool, you can easily integrate notif.me into your backend.
We currently do not provide SDK but you just need to be able to communicate with our API with your
favorite HTTP client.

To send a notification, make a `POST` request on `https://[subdomain].notif.me/api/notification`
with your token on `Authentication` header. The API only accepts JSON payload with these parameters:

* `toUserId` (string): the unique identifier of the user on **your** service. Even though this field
is required, you don't have to create the user on our system before using it. It is used for
tracking conversations.
> **Note**: if you don't have an id for some users (a prospect for example), you can use something
like prospect-[random].

* `to` (object): endpoints for each channel for this notification. The key is the channel and the
value the endpoint.
```json
{
    "email": "julien@notif.me",
    "webpush": "uuid_generated_by_our_frontend_sdk"
}
```

* `template` (object): describes the content of your notification, if you have already created your
template on your Notif.me dashboard you can just set the name and everything's good. You can also overwrite all the fields previously defined on your dashboard.
> **Note**: Even though the name of the template is required, a template does not need to exist on
your account. You can set all the template during the api call and use the name you want.

* _`params`_ (object): optional, it's a plain object with your replacement variables available in
your template.


#### Example in Node.js
```javascript
import request from 'request'

request.post({
  url: 'https://[subdomain].notif.me/api/notification',
  auth: {
    bearer: 'MYTOKEN'
  },
  json: {
    toUserId: '84024',
    template: {
      name: 'welcome',
      // Example to overwrite the email subject and the webpush title.
      email: {
        subject: 'Hi {% raw %}{{firstname}}{% endraw %}'
      },
      webpush: {
        title: 'Hi {% raw %}{{firstname}}{% endraw %}'
      }
    }
  },
  params: {
    firstname: 'James'
  }
}, (err, res, body) => { ... })
```

<!--
To go further on these parameters visit the complete documentation of the
[send api](/notifme-docs/guides/api-send).
-->
