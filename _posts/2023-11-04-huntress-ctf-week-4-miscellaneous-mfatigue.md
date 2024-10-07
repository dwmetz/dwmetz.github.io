---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/04/huntress-ctf-week-4-miscellaneous-mfatigue/
title: 'Huntress CTF: Week 4 - Miscellaneous: MFAtigue'
description: None
date: 2023-11-04 15:00:00 -0000
last_modified_at: 2023-11-04 12:29:41 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- Miscellaneous
tags:
- active directory
- hashes
- HuntressCTF
- impacket
- MFA
- ntds
- passwords
- rockyou
---
## MFAtigue

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-9.14.07e280afam.png?w=1024)

For any of these challenges where there's a download and an online component, I'll usually start with the files.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.18.10e280afam.png?w=207)

OK. So how can we get a password if we have access to the ntds.dit and the SYSTEM registry hive?

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.19.20e280afam.png?w=1024)

The [iredteam.com](https://www.ired.team/offensive-security/credential-access-and-credential-dumping/ntds.dit-enumeration) article looks like a good place to start.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.30.53e280afam.png?w=1024)

There's a reference to dumping hashes using [impacket](https://github.com/fortra/impacket).

I don't have the SECURITY hive, but I do have the ntds.dit and the SYSTEM hive.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.12.37e280afam.png?w=1024)

From here we'll copy out all the hashes for user accounts. The accounts ending with $ are computer accounts so we won't bother with those.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.13.12e280afam.png?w=885)

With the hashes isolated in a text file, we can run hashcat on the hashes using the **rockyou** wordlist.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.07.12e280afam.png?w=1024)

...output continues...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.07.59e280afam.png?w=1024)

We've got a match on the hash ending ..cadab42a.

Referencing that against our account information, we see that found hash is the password for JILLIAN_DOTSON.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-9.56.03e280afam.png?w=985)

Now for the url in the challenge. It brings us to a Microsoft sign-in page. We'll use the account huntressctf\JILLIAN_DOTSON

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.15.21e280afam.png?w=1024)

And the cracked password of katlyn99...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.15.43e280afam.png?w=1024)

Oh but wait. The account has MFA?!!

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.15.56e280afam.png?w=1024)

Hit the **Send Push Notification**

Then **again** ,

And **again**...

After a mildly obnoxious number of repeated attempts....

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-10.16.19e280afam.png?w=1024)

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
