---
layout: post
permalink: http://bakerstreetforensics.com/2020/10/26/magnet-ctf-question-3-solution-walk-through/
title: 'Magnet CTF: Question 3 Solution Walk-Through'
description: None
date: 2020-10-26 15:01:00 -0000
last_modified_at: 2020-10-26 15:06:54 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Android
---
_[Challenge 3…Which exit did the device user pass by that could have been taken for Cargo?]()_

![](https://bakerstreetforensics.com/wp-content/uploads/2020/10/8e83742dd6bada06fa01d48ae34d63a1.jpg?w=320)

In NJ it’s common to inquire where someone resides with the question “What exit?” I found it interesting that some of the test data examined as part of the CTF included artifacts that _originated_ in New Jersey. Yup. I hail from the land of Bruce Springsteen and Bon Jovi and if you even mention Jersey Shore in the context of a _reality_ show, please just… quietly go away.

Many types of forensic artifacts include metadata that ties that evidence to a particular location using GPS data. Maps and driving applications are among these, and commonly you can retrieve this data from photos and videos. (Note: a lot of services Facebook, Instagram, etc. – will have settings on whether or not they remove this data when media is shared.)

![](https://bakerstreetforensics.com/wp-content/uploads/2020/10/screen-shot-2020-10-22-at-4.15.11-pm-2.png?w=1024)

Selecting this view limits the presentation of artifacts to just those that include GPS data.

Reviewing the artifacts with GPS metadata there was nothing that immediately presented as a reference to _exit_  or _cargo_. 

However, just like Transformers, within these artifacts there is more than meets the eye. In the screenshot above you’ll see that several files start with MVIMG_ . These files are Google Motion Photos, essentially the functional equivalent of Live Photos on iOS.

A few weeks back I saw a Magnet webinar: Mobile Artifact Comparison – Understanding the Similarities Between iOS and Android Data

https://www.magnetforensics.com/resources/mobile-artifact-comparison-webinar-recording-oct-7 

Included in the comparison were both of these “live photo” types and [B1n2h3x](https://twitter.com/b1n2h3x) reference that previously she had carved out the MVIMG_ files and was able to isolate the key frame and the MP4 image (video) that comprised it.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/10/what.jpg?w=201)

_If you’re not supplementing your Magnet course training with their free webinars you’re really missing out._

  * Using Axiom, I exported all the MVIMG_ files to a folder.
  * Next I utilized GoMoPho - Google motion photos video extractor <https://github.com/cliveontoast/GoMoPho> and ran it against the directory which split the MVIMG_ files into .jpg and .mp4
  * From there I loaded the videos into VLC.  They were only a few seconds long and played very fast. This is where the playback speed settings in VLC come in handy. Drop it back to the slowest it will play.



When initially previewed, MVIMG_20200307_120326.jpg presented what appeared as a view from the highway in winter. There are no immediate discernable landmarks in the photo.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/10/image-2.png?w=77)

However, when the extracted video is played, we get a few seconds visibility from the car on the highway, including passing the following sign:

![](https://bakerstreetforensics.com/wp-content/uploads/2020/10/image-3.png?w=121)

Among the words on the sign we can read “Cargo” and the exit show at the bottom of the sign, **F16**.

PDF: [magnetweeklyctf-write-up-3-1.pdf](https://bakerstreetforensics.com/wp-content/uploads/2020/10/magnetweeklyctf-write-up-3-1.pdf)
