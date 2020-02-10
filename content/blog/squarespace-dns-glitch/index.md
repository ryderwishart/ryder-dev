---
title: Glitch with Squarespace DNS Record
date: "2020-02-10"
description: "I ran into a bizarre glitch when adding a TXT record to a client's Squarespace-controlled domain."
---

Are you having problems with the Squarespace DNS zone editor?

It might not be your fault.

**TLDR: you might just have to chat with customer service.**

## Glitch adding SPF verification for Zoho Mail to Squarespace

I was trying to add a TXT record, but the frontend (what I was doing) was not talking to the backend (Squarespace's own copy of the DNS records). The result was this glitch I recorded.

I'll add a video recording *here*.

Every time I tried to add my SPF email verification for Zoho Mail to this Squarespace-controlled domain, there was an unwanted tab character ('\009') that I could not eliminate.

You could see this character when I used Google's online dig tool. 

When I deleted the character and saved my changes, it looked like everything was fixed. However, when I backed out of the advanced DNS editor and then loaded it back up, the character was back!

## Solving the glitch

In this case, there was nothing I could do immediately to fix the problem. I had to chat with customer service, and "Chris" was very helpful. When it became clear that customer service was as confused as I was, I recorded the video above and sent it in an email.

Normally I wouldn't register a domain through Squarespace, but my client already had. I have nothing against Squarespace, I just don't normally use it. My general dislike of most frontend systems was once again confirmed through this experience, but I can't say anything about making a website with them.

If you run into the same problem as me, I hope this blog post helps you solve it.

Just link to this page when you're chatting with customer service.

If it helps you out, I'd appreciate a mention on Twitter, or a comment on this page (whenever I get around to adding a comments section to my blog posts!).
