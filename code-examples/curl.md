---
layout: default
title: Curl example
---

### Curl example
```shell
curl -X POST \
  https://[subdomain].notif.me/api/notification \
  -H 'authorization: your-api-token' \
  -H 'content-type: application/json' \
  -d '{
    "toUserId": "your-user-id",
    "to": {
        "email": "test@demo.com"
    },
    "template": {
      "name": "welcome",
      "channels": {
        "email": {
          "domain": "[subdomain].notif.me",
          "from": "team@[subdomain].com",
          "subject": "Welcome!",
          "html": "Hi {% raw %}{{name}}{% endraw %}, welcome to our service!"
        }
      }
    },
    "params": {"name": "James"}
  }'
```
