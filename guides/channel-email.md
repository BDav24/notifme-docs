---
layout: default
title: Send an Email
menu:
  - title: Enable the channel
    anchor: '#prepare'
  - title: The content
    anchor: '#content'
  - title: The events
    anchor: '#events'
---



## Send an Email

Email is an unavoidable channel when we talk about notification.

> **Note**: We currently support out-of-the-box only one transporter `Mailgun` but contact us if you
want to use another one.


<a id="prepare"></a>
### [Enable the channel](#prepare)

Notif.me will not send directly your email, instead we are going to use an Email transporter.

Browse the Production environment page in your dashboard http://[subdomain].notif.me/dashboard/environments?env=Production and click
on the `EDIT` button of your Mail transporter.

Enter your Mailgun `Api-Key` (you can find it on your mailgun account).

![email_config](/notifme-docs/assets/img/email-config.png)

On the next page you will be able to select the domains you want to use with Notif.me. Typically you
will have a domain for your transactional emails and another one for your marketing campaigns.

![email_config](/notifme-docs/assets/img/email-config_2.png)

Finaly you would want (optional but recommended) to enable tracking on your emails.

1. Enable tracking on your Mailgun account. Go to Domains > select your domain > Tracking Settings
and turn on `Click Tracking` and `Open Tracking`.

![email_config](/notifme-docs/assets/img/email-config_3.png)

2. Configure the webhooks on Mailgun to `https://[subdomain].notif.me/api/webhook/mailgun`. Go to
Webhooks and set each of them to this url

![email_config](/notifme-docs/assets/img/email-config_4.png)

<a id="content"></a>
### [The content](#content)

All the fields are customizable on your Dashboard > Templates, you can overwrite them during the
API request. You can use {% raw %}{{Mustache}}{% endraw %} code to parameterize the Template.

* `subject`: (string)
* `domain`: (null \| string) you can precise the domain (if you have multiple domain) you want to use
to send this email
* `from`: (null \| string) the sender of the email
* `replyTo`: (null \| email)
* `html`: (string) This is the body of the mail. On your dashboard you will be able to write your
email body into [mjml](https://mjml.io/). _"MJML is a markup language designed to reduce the pain of coding a responsive email."_
You will be able to write directly the body into HTML too if you want. If you are overwriting this
field on the API, only HTML is supported.


<a id="events"></a>
### [The events](#events)

If you have configured the tracking part (see above), you can register webhooks for these events:

* `delivered`: The mail has been accepter by the final provider and is available for the user
* `opened`: The user opens the email
* `clicked`: The user clicks on a link in the body
* `unsubscribed`
* `complained`
* `bounced`
* `dropped`
* `answered`: (work in progress)
