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

Notif.me is a SAAS service that helps you notify your users on multi channels.

![img](/notifme-docs/assets/img/get_started_top.png)

<a id="devtool"></a>
### [The devtool](#devtool)

Notif.me is made with love by developers for developers. As developers, **local testing** is essential
for us. This is usually a challenge when working with notifications because there are external to your service.

That's why we have built a single devtool to use with our service. With the devtool you will have:

* **The catcher**: In dev mode, all the notifications sent by your service to our endpoint will end on a
catcher running on your localhost. You will have all the previews of what your service is currently
sending in one place.

* **Easy webhook debugging**: The devtool acts as a proxy for the webhook you have
configured on your dashboard. So you will be able to set **local urls** for your webhooks !
No more headeaches with deploying a version just to test a webhook !

* **No specific "dev" code**: ~~`if (dev) debugMail(mail) else sendWithXXX(mail)`~~ < with notif.me
you can forget the ton of problems you would have with specific code. Set the good token
on each environment and send all your notifications (**including during development**) to us.
> **Note**: on *dev* environments notifications will never be sent to final users (except the ones you
have set for testing purpose) so .. no stress !

Installation

 ```sh
yarn global add notifme-devtool
notifme-devtool tuto
 ```

 (Or with npm: `npm install -g notifme-devtool`)

> **Note**: we also provide a Docker image `docker run -p 1080:1080 --rm -it notifme/devtool tuto`

Enjoy the onboarding instructions

![Getting started gif](/notifme-docs/assets/img/getting-started.gif)

<a id="integrate"></a>
### [Quick integration](#integrate)

Once you have done the tutorial on the devtool, you can easily integrate notif.me to your backend. We
currently do not provide SDK but you just need to be able to communicate with our API with your favorite
HTTP client.

To send a notification, make a `POST` request on `https://[subdomain].notif.me/api/notification` with your token
on `Authentication` header. The API only accepts JSON payload with these parameters:

* `toUserId` (string): the unique identifier of the user on **your** service. Although this field is mandatory,
you don't have to create the user on our system before use it. It is used to attribute the notification to someone.
> **Note**: if you don't have an id for some users (classicaly for prospect), you can set this field
to somthing like prospect-[random]

* `to` (object): endpoints for each channel for this notification. The key is the channel and the value
the endpoint.
```json
{
    "email": "julien@notif.me",
    "webpush": "uuid_generated_by_our_frontend_sdk"
}
```

* `template` (object): describe the content of your notification, if you have already created your template
on your Notifme dashboard you can just set the name and that's good enough. You can also overwrite all the fields
previously defined on your dashboard
> **Note**: Although the name of the template is mandatory, a template does not need to exist on your account
you can set all the template during the api call and set the name you want

* _`params`_ (object): Optional, it's a plain object with your replacement variables available in your template.

```javascript
// Exemple of a notification send in Node.js
import request from 'request';

request.post({
  url: 'https://subdomain.notif.me/api/notification',
  auth: {
    bearer: 'MYTOKEN'
  },
  json: {
    toUserId: '84024',
    template: {
      name: 'welcome',
      // I want to overwrite the email subject and the webpush title.
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
}, (err,res, body) => { .. });
```

To go further on these parameters visit the complete documentation of the
[send api](http://notifme-docs/guides/api-send).
