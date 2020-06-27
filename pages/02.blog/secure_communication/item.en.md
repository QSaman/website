---
title: Emails or Messaging Apps?
date: 22:39 06/26/2020 

hero_classes: text-light title-h1h2 overlay-dark-gradient hero-large parallax
show_sidebar: true
hero_image: header.jpg

taxonomy:
    category: en_blog
    en_tag: [security, email, messaging apps, WhatsApp, Telegram]
---

When you use messaging apps like [WhatsApp](https://www.whatsapp.com/) or [Telegram](https://telegram.org/) you are 100% sure that the communication is encrypted. Unfortunately that's not the case for emails.

===

When I travelled to my home country, Iran, I've noticed, unlike Canada, in many situation you are required to use Telegram which is strange for me at first. But I've changed my mind when I learned that emails are not always encrypted.

According to [Google](https://transparencyreport.google.com/safer-email/overview?hl=en), at the time of writing, 93% of outbound emails and 94% of inbound emails are encrypted (from/to gmail). One of the most popular way to encrypt emails is to use [STARTTLS](https://en.wikipedia.org/wiki/Opportunistic_TLS). Of course your email provider (e.g. gmail) must support it. There is an interesting project by [Electronic Frontier Foundation (EFF)](https://www.eff.org/) which tells you if your email provider supports STARTTLS. Unfortunately that project is not maintain anymore. For more information visit [STARTTLS Everywhere](https://starttls-everywhere.org/).

There are other methods for encrypting email message. For more information read this [article](https://en.wikipedia.org/wiki/Email_encryption). For inspecting email header to make sure whether or not email is encrypted, read this [page](https://www.paubox.com/blog/check-tls-secure-email/).

As you can see it's difficult to be sure that your email message is encrypted between servers. So if you want to send sensitive data, it's better to use a secure messaging app like WhatsApp or Telegram.
