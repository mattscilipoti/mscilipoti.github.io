---
title: "Email causing SocketError on Heroku?"
date: 2010-05-07
categories: [Heroku, Email]
---

Getting a `SocketError` while sending emails on Heroku, using SendGrid? It may be some Gmail environment variables.

While working on a project that I just inherited, we enabled SendGrid for our Heroku app. Sending an email raised this error:

```
SocketError (getaddrinfo: Name or service not known)
```

Google searches indicated ActiveMailer configuration issues, but Heroku was pretty clear:

> Rails apps using ActionMailer will just work, no setup is needed after the addon is installed.

While ensuring that `RACK_ENV` was set correctly (when all else fails), I found these two settings:

```
GMAIL_SMTP_PASSWORD => #blahblah#
GMAIL_SMTP_USER => admin@xyz.com
```

Removing them fixed my email. SendGrid is working fine.

Armed with this evidence, I found [this Heroku blog post](http://blog.heroku.com/archives/2009/11/9/tech_sending_email_with_gmail/). And so... someone had set up the server for Gmail, but something about it was failing now.

Now I know. And you do too.

**Note:** This is just one cause of this error.