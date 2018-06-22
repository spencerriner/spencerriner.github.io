---
title: "What is Keybase?"
date: 2017-12-11
tags: [keybase, encryption]
categories: [guide]
---

## What is Keybase?

I'm writing this because I've been asking myself this question for about a month since I heard about (and subsequently signed up for) Keybase. It seems like it's trying to do a lot of different things, but *identity verification* is the front runner.

## Key Exchange

I send encrypted emails at work all the time. It's kind of horrifying because of the layers of software that bog the entire process. Outlook goes unresponsive for around 10 seconds if I so much as select an encrypted email. Additionally, this process doesn't work at all on Linux which makes things even worse. But when you need to send sensitive information - IP addresses, passwords, vulnerabilities - you don't want anyone snooping on the content. Key exchange facilitates this process. I learned about this in college briefly, so I'll do a quick self-review.

I generate a keypair - I get a private key that is known only to me and a public key that I can publish anywhere. I want to send an email to Dave. Assuming Dave has a keypair of his own, I acquire Dave's *public* key (the how of this is a question I've always wondered - I think now I know) and sign my message using the key. Admittedly, I don't know the deep details of this process; maybe I'll brush up on my cryptology one of these days. But basically the key is used to generate a hash, which is unique to the contents of the message and the receiver's public key. I send Dave the whole package (it looks like gibberish) and he can then decrypt the message using his private key, which only he knows. That's basically it. If his private key can decrypt the message, he can be sure the message was meant for him.

## Problems

Even knowing this extra rudimentary outline of the process, I never got how it worked in practice. How do I get Dave's public key? Does he post it on his website or something? I can't think of a single person I know that has a public key I could go grab. And even once I get it, how do I know that it hasn't been compromised? That is, how do I know someone isn't *impersonating* Dave and posting their own public key? 

## Enter Keybase

Are the pieces coming together? Keybase is a centralized store of users' public keys. And users gain credibility by registering with various services, which in effect adds the potential to encrypt messages on platforms that don't natively support it. For instance - you find me on Twitter. You laugh at my inactive timeline and lack of followers. You want to send me an encrypted message, berating me for my social media ineptitude. You look me up on Keybase and see that my usernames match, and boom. It's as simple as that. That, to me, is pretty cool. The more services (your own personal website, your GitHub account, etc.) that are linked up with your key, the more credible you appear.

And it also solves a potential problem that I face at work pretty often. I can encrypt a message for a teammate or coworker, quickly encrypt it in Keybase, and send it out with no fears. No garbage laggy CPU-destroying software in the way. 

## What's Next?

I've spent a pretty considerable amount of time linking up accounts with Keybase just to fill in the boxes. It's obviously not intentionally gamified, but I just can't fight the urge to appear as credible as humanly possible via Keybase. It's pretty fun. Try it out! I have a sneaking suspicion that as time passes and people become disenchanted with the privacy invasion climate we live in, encryption will be an asset.