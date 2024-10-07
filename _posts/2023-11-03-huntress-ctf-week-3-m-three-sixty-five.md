---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/03/huntress-ctf-week-3-m-three-sixty-five/
title: 'Huntress CTF: Week 3 - M Three Sixty Five'
description: None
date: 2023-11-03 13:00:00 -0000
last_modified_at: 2023-10-22 11:24:20 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- M365
tags:
- AADInternals
- HuntressCTF
- O365
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.26.39e280afpm.png?w=1024)

This is a multipart challenge. All the flags can be found within the live Microsoft 365 instance that we'll ssh into.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.28.26e280afpm.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.31.32e280afpm.png?w=1024)

The clue is _street address._ I'm not too fluent in the capabilities of AADInternals, so the first thing I do is head over to the [documentation](https://aadinternals.com/aadinternals/).

If I do a search on 'street' I see that it's part of an Output example for **Get-AADintTenantDetails**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.57.31e280afpm.png?w=922)

Ok, let's give that command a go.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-1.36.17e280afpm.png?w=1024)

And there's the flag under the street value.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.37.52e280afpm.png?w=1024)

For the next one, It not so subtly says that Conditional Access Policies will be part of this, so again we reference the docs. **Get-AADIntConditionalAccessPolicies** seems like a good candidate.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-1.37.48e280afpm.png?w=1024)

Two for two.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.41.53e280afpm.png?w=1024)

Microsoft Teams will be our focus on the third one. There's dozens of Teams commands available within AADInternals. If we focus on _message_ , that will get us to **Get-AADIntTeamsMessages**.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-1.39.45e280afpm.png?w=1024)

Having the documentation for the syntax really helped on this one.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.46.07e280afpm.png?w=1024)

And for the last one, no there isn't a Get-AADIntPresident command. That would be too easy. How about a command that will show us all the users?

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-1.55.00e280afpm.png?w=546)

Scrolling up through the output, we find that the President (PattiF), has a _flag_ in the telephone number field.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-1.54.30e280afpm.png?w=1024)

4 out of 4.

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
