---
layout: post
title: Cloudflared - Make your services public! 🌐
category: posts
lang: en
tags: [dev, host, local, lab]
image: /assets/img/posts/cloudflared.jpg
accent_image: 
  background: url('/assets/img/posts/cloudflare-logo.png') center/10%
  overlay: true
description: >
  Free service that allows you to share your services with anyone.
invert_sidebar: false
---

After you start having your self-hosted applications and exploring this universe further, at some point you will come across the problem of having all of this only available on your local network. You may want to share a new application you have added with a friend, or like the example of the password vault, in the Bitwarden post, which makes the application much simpler to use, and that is what I will address today.

All you need to make your application accessible from anywhere is a domain. I had some alternatives for free domain registration, but it is true that it is not available at the moment, so I recommend domains on porkbun or namecheap, which usually have promotions and several domains available.

The cloudflare tunnel works in a similar way to ngrok, for those who have already had contact with it, the great advantage of the service is that it creates a tunnel between your local network and the internet, even adding encryption protocol (HTTPS) between cloudflare and the connected client, without having to open any ports on the router, configure firewall, or deal with other complex security configurations.

To start, if you've read other posts, you should know where this starts, yes, docker! The Cloudflare website itself provides the command to create the tunnel with its token associated with it, follow this path within the website's dashboard:

<p>
<img src="/assets/img/posts/cloudflared-home.png">
<img src="/assets/img/posts/cloudflared-zero-trust.png">
</p>


At the last step you should get the token to create your docker container
```
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiYjE....
```

After creating your tunnel, you should be able to see that it is online and healthy, as in the following image:
<p>
<img src="/assets/img/posts/cloudflare-healthy.jpg">
</p>

The next step is to create a forwarding rule between the internet and your local network, to do this, follow this path:

3 dots > Configure > Public Hostname

<p>
<img src="/assets/img/posts/cloudflare-forward.jpg">
</p>

And to configure your first endpoint, you must fill in the following information:

<p>
<img src="/assets/img/posts/cloudflare-public-hostname.jpg">
</p>

After making the configurations, the service should be available by accessing the link that I configured in the domain session. One note is that I was unable to use the tunnel to export a UDP service, only HTTP and TCP. If you know of any alternative, please leave a comment and, until next time! 🎇