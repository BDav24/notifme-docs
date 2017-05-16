---
layout: default
title: The devtool
---

### The devtool

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

If this is your first time using the devtool, following the [Getting started guide](/notifme-docs/guides/getting-started) is highly recommanded.

Usage:

 ```sh
yarn global add notifme-devtool
notifme-devtool start -e [subdomain].notif.me -t [MYDEVTOKEN]
 ```

 (Or with npm: `npm install -g notifme-devtool && notifme-devtool start -e [subdomain].notif.me -t [MYDEVTOKEN]`)

> **Note**: we also provide a Docker image `docker run -p 1080:1080 --rm -it notifme/devtool -e [subdomain].notif.me -t [MYDEVTOKEN]`
