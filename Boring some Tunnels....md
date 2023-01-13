---
title: # Sample Post
menu_order: 1
post_status: publish
post_excerpt:
taxonomy:
    category:
        - Linux
    post_tag:
        - ssh
        - tunnel
comment_status: open
---

# How to bore some Tunnels...

I am way too lazy to remember all sorts of commands. At the same time, when in my company VPN, because of how the VPN client works, I can regularly not access things on my home network. Be it some Windows VM that I've running inside a virtual machine on my server, or be it just to check out what's going on with my raid controllers.

Oh well, I know you can just shell out an ssh command to tunnel. But I don't want to remember the options. So, Python ftw:

![](tunnel.png)

The code is [here](https://github.com/mnott/tunnel).

Note that I'm using my gateway as jumphost as that's the one thing I can always connect to, never mind what the VPN client attempts to cripple down my client.


