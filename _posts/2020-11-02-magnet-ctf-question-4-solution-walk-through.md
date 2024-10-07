---
layout: post
permalink: http://bakerstreetforensics.com/2020/11/02/magnet-ctf-question-4-solution-walk-through/
title: 'Magnet CTF: Question 4 Solution Walk-Through'
description: None
date: 2020-11-02 16:50:24 -0000
last_modified_at: 2020-11-02 16:50:24 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Android
---
**Challenge 4 (10/26-11/2) Animals That Never Forget**

**_Chester likes to be organized with his busy schedule. Global Unique Identifiers change often, just like his schedule but sometimes Chester enjoys phishing. What was the original GUID for his phishing expedition?_**

Week 4 was definitely a brain-teaser for me. On my first attempt I was focused on the final part of the question. **_What was the original GUID for his phishing expedition?_**

The equivalent of a GUID for an email would be the message ID. In the case data we have, we can see a portion of the original phishing email delivery, but the message headers were not visible. As the data sample was also used at the Magnet User Summit, I checked the Google TakeOut data for the same account and voila - the message ID (email GUID) for the phishing email was: **CACWSF2E=ghTTG2hx10GUh+WOf-GkORngZc+c7-BuY4R+cKUhGA@mail.gmail.com**

WRONG

So I go back to the question and this time don't ignore 75% of the verbiage. It's there for a reason, right?

So we know that our subject, Chester, is meticulous. My first inspection was of the calendar data - but nothing presented as intersting. Looking at the **Documents** artifacts, we can see that the subject uses Evernote - and among the notes on his device is one called **Phishy Phish Phish** _(at least is wasn't titled "Evidence")_.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/evernote.png?w=1024)

If we follow the source link to \user213...-Evernote.db we see that Evernote is storing this content in a SQLite database.

If we examine the **notes** table we can see that each notebook entry has its own **guid.** Our Phishy Phish Phish message has the guid of ****c80ab339-7bec-4b33-8537-4f5a5bd3dd25**. **

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/current-guid.png?w=1024)

Is that the answer? NO. 

_What was the**original GUID** for his phishing expedition?_

So this is a GUID, but is it the original?

Looking at a number of developer articles for Evernote I learned that a note can be moved or copied to another notebook. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/developer-notes.png?w=1024)https://dev.evernote.com/doc/reference/NoteStore.html#Fn_NoteStore_listNotebooks

When that operation takes place it keeps track of this activity in the **guid_updates** table.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/old-guid.png?w=1024)

If we reference this table we can find our message, identified by the current guid (identified above) and see that there's a column for **old_guid** which for the note in question is: **7605cc68-8ef3-4274-b6c2-4a9d26acabf1.**
