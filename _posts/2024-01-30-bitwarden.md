---
layout: post
title: Bitwarden (Password manager)! 🔒
category: posts
lang: en
tags: [dev, host, local, lab]
image: /assets/img/posts/bitwarden-logo.jpg
accent_image: 
  background: url('/assets/img/posts/vaultwarden.svg') center/25%
  overlay: true
description: >
  You can entirely host your own password vault!
invert_sidebar: false
---

A few weeks ago I wrote about NAS and mini server alternatives, always thinking about energy efficiency and the functionality it seeks to achieve. Continuing on the subject, I would like to talk today about self-hosting and the entire community I discovered.

In the article from a few weeks ago, I gave some examples of uses for a mini server, and it turns out that it is much more useful than I initially imagined. My idea now is to talk a little more about how my infrastructure is, which self-hosted apps I have been using and delve deeper into the ones I find most interesting.

To start this series, I would like to talk more about one of the apps I use most on a daily basis, Bitwarden.

Bitwarden is a password vault. Working in IT, I've always known the risks of using the same password for all services, and that simply enabling 2-factor authentication doesn't guarantee your security. That's when I started researching more about password vaults. At first, I didn't want to pay a monthly fee, especially since I would have to trust a third-party company to store all my passwords, and some of them have already been involved in data leaks. The first alternative I found was KeepasXC, which is open source, has several options for extremely secure encryption algorithms, and even has a mobile app. The only problem I found is that it stores everything in a file on your device, so you have to make a backup yourself or use an app like syncfy to synchronize the file between different devices. But I still wasn't happy, it didn't seem like the best alternative, and then I found Bitwarden, the company that maintains it also offers hosting services, in case you prefer not to have to do the work of hosting it yourself, and the source code of the app is open source, it also has all the advantages of KeePassXC but with a web interface and a Docker image that can easily be used for self-hosting.

It also has a mobile app, fingerprint authentication, importing passwords from other apps like Google Chrome and there are still some advantages between the self-hosted version and the free version using the company's servers. In self-hosting, TOTP authentication is available, the same one used by Google and Microsoft Authenticator and which generates temporary codes for login.

Here is the docker command to start a new server instance in case you want to test it:

```
docker pull vaultwarden/server:latest
docker run --detach --name vaultwarden \
  --env DOMAIN="https://vw.domain.tld" \
  --volume /vw-data/:/data/ \
  --restart unless-stopped \
  --publish 80:80 \
  vaultwarden/server:latest
```

However, I recommend using docker compose, when you have more apps on your server and need to manage all of this, it will make your life much easier:

```
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vw.domain.tld"
    volumes:
      - ./vw-data/:/data/
    ports:
      - 80:80
```

And that's it for this post, my initial configuration left the server available only on the local network, but to have my password vault available from anywhere I configured a Cloudflare tunnel which I will discuss how to configure in the following post.

Sponsor the Bitwarden:
https://github.com/bitwarden

And the Vaultwarden:
https://github.com/dani-garcia/vaultwarden