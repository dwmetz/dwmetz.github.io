---
layout: post
permalink: http://bakerstreetforensics.com/2023/03/09/nsrl-query-from-the-command-line/
title: NSRL Query from the Command Line
description: None
date: 2023-03-09 18:04:36 -0000
last_modified_at: 2023-03-10 03:13:32 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Curl
- NSRL
- Python
---
In digital forensics, we're frequently trying to separate the signal from the noise. When examining operating systems - including mobile, it can be helpful to know what files came with the operating system. By filtering those out we can concentrate on what's _new_ on the device as we start looking for activity.

> The National Software Reference Library (NSRL) is designed to collect software from various sources and incorporate file profiles computed from this software into a Reference Data Set (RDS) of information. The RDS can be used by law enforcement, government, and industry organizations to review files on a computer by matching file profiles in the RDS. This will help alleviate much of the effort involved in determining which files are important as evidence on computers or file systems that have been seized as part of criminal investigations.
> 
> https://www.nist.gov/itl/ssd/software-quality-group/national-software-reference-library-nsrl

Recently I came across a [site](https://circl.lu/services/hashlookup/#circl-hashlookup-hashlookup-circl-lu) that, among other capabilities, has the option of doing an NSRL lookup using `curl` from the command line.

Me being Mr. PowerShell I wanted to see what the syntax would be to do the same lookup with PowerShell. So where did I turn? No, not to Jeffrey Snover. I went to ChatGPT. I'd heard quite about how services like these, while not trustworthy for anything of historical accuracy, are pretty good at translating code. 

The original syntax:
    
    
    curl -X 'GET' \
      'https://hashlookup.circl.lu/lookup/sha1/09510d698f3202cac1bb101e1f3472d3fa399128' \
      -H 'accept: application/json'
    

![](https://bakerstreetforensics.com/wp-content/uploads/2023/03/screenshot-2023-03-09-at-12.26.49-pm.png?w=808)

Sure enough it returned functional code to do the same operation in PowerShell. What I really appreciated though is the detailed information beneath that explains the parallel functions between the two, and what the different values represent. I could see myself using 'explain this code to me' in the future.

**PowerShell NSLR Query Syntax:**
    
    
    Invoke-RestMethod -Uri 'https://hashlookup.circl.lu/lookup/sha1/3f64c98f22da277a07cab248c44c56eedb796a81' -Headers @{accept='application/json'} -Method GET
    

* * *

I also asked it to convert the `curl` command to Python which it handled equally well, and once again the same level of explanation of what's going on beneath the code.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/03/screenshot-2023-03-09-at-12.35.24-pm.png?w=843)

**Python NSRL Query Syntax:**
    
    
    import requests
    response = requests.get('https://hashlookup.circl.lu/lookup/sha1/09510d698f3202cac1bb101e1f3472d3fa399128', headers={'accept': 'application/json'})
    print(response.json())

* * *

Curl script & output

![](https://bakerstreetforensics.com/wp-content/uploads/2023/03/screenshot-2023-03-09-at-12.41.01-pm.png?w=1024)

Python script & output

![](https://bakerstreetforensics.com/wp-content/uploads/2023/03/screenshot-2023-03-09-at-12.42.23-pm.png?w=1024)

PowerShell script & output

![](https://bakerstreetforensics.com/wp-content/uploads/2023/03/screenshot-2023-03-09-at-12.44.00-pm.png?w=1024)

* * *

Of the three, I prefer the output of the PowerShell command as the output is the most readable. In the screenshot above, four queries were run. For the first two there wasn't a matching hash detected, so we can't confirm whether those were included with the operating system. For the second two queries, which happen to be for executable names that are frequently misused by bad actors, we see that the hashes queried do match the published NSRL.
