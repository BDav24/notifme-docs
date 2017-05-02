---
layout: default
title: Getting started
---

## Getting started

Notif.me is a SAAS service that helps you notify your users on multi channels.

![img](/notifme-docs/assets/img/get_started_top.png)

### The devtool

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
