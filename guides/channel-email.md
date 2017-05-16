---
layout: default
title: Send an Email
menu:
  - title: Configure your provider
    anchor: '#configure'
  - title: Email content
    anchor: '#content'
  - title: Webhook events
    anchor: '#events'
---

## Send an Email

Email is an essential channel when we talk about notification.

> **Note**: We currently support only one transporter out-of-the-box `Mailgun` but you can
[contact us](https://www.notif.me/contact) if you want to use another one.

<a id="configure"></a>
### [Configure your provider](#configure)

For optimal deliverability Notif.me doesn't send directly your email, but forwards it to the
provider of your choice.

Go to the **Environment** page of [your dashboard](https://www.notif.me/login), select
**Production** and click on the `EDIT` button of the email transporter.

Enter your Mailgun `Api-Key` (you can find it in your Mailgun account).

![email_config](/notifme-docs/assets/img/email-config.png)

On the next page you will be able to select the domains you want to use with Notif.me. Typically you
will have a domain for your transactional emails and another one for your marketing campaigns.

![email_config](/notifme-docs/assets/img/email-config_2.png)

Finaly you would want (optional but recommended) to enable tracking on your emails.

<a id="configure-tracking"></a>
#### [Configure tracking](#configure-tracking)

1. Enable tracking on your Mailgun account. Go to Domains > Select your domain > Tracking Settings
and activate `Click Tracking` and `Open Tracking`.<br><br>
![email_config](/notifme-docs/assets/img/email-config_3.png)

1. Set all the webhooks on Mailgun to `https://[subdomain].notif.me/api/webhook/mailgun`.

![email_config](/notifme-docs/assets/img/email-config_4.png)

<a id="content"></a>
### [Email content](#content)

All the fields are customizable on your Dashboard > Templates, you can overwrite them during the
API request. You can use [{% raw %}{{ mustache }}{% endraw %}](https://mustache.github.io/mustache.5.html)
code to parameterize the template.

* `subject`: (string)
* `domain`: (null \| string) you can precise the domain (if you have multiple domain) you want to
use to send this email
* `from`: (null \| string) the sender of the email
* `replyTo`: (null \| email)
* `html`: (string) This is the body of the email.<br>
On your dashboard you will be able to write your email body in [MJML](https://mjml.io/). _"MJML is
a markup language designed to reduce the pain of coding a responsive email."_ You will be able to
write directly the body into HTML too if you want.<br>
If you are overwriting this field on the API, only HTML is supported.


<a id="events"></a>
### [Webhook events](#events)

If you have configured tracking ([see above](#configure-tracking)), you can register webhooks for
these events:

* `delivered`: the email has been accepted by the provider and is available for the user
* `opened`: the user has opened the email
* `clicked`: the user has clicked on a link in the body
* `unsubscribed`
* `complained`
* `bounced`
* `dropped`
* `answered`: (coming soon)
