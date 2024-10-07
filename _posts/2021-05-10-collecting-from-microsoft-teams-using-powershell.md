---
layout: post
permalink: http://bakerstreetforensics.com/2021/05/10/collecting-from-microsoft-teams-using-powershell/
title: Collecting from Microsoft Teams using PowerShell
description: None
date: 2021-05-10 22:58:49 -0000
last_modified_at: 2023-01-23 13:35:16 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Magnet
- Teams
---
There are two means by which to ingest Microsoft Teams information into Magnet Axiom for processing. The first approach uses Axiom Process. If you're collecting in this manner you will need to have the credentials of the user you are collecting from. Axiom will use those credentials to log into O365 and retrieve the user's data. Depending on the conditions of the investigation, you may have the option of resetting the password to gain access.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/screen-shot-2021-05-10-at-5.15.38-pm.png?w=1024)Magnet Axiom Cyber Teams Collection

If you don't have the user's credentials, but you do have administrative access to the Exchange environment, you can run a search from the Microsoft Security and Compliance center. Once the search completes you can export/download the results as a PST. To ingest the PST into Axiom just 'add evidence' , 'files & folders' and then browse to the PST file.

To speed up the process, I've written a small PowerShell script to build and run the Compliance Center search. The script depends on the **ExchangeOnlineManagment** module being installed. In this script we're connecting to Security & Compliance PowerShell using MFA and modern authentication.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/img_3786.jpg?w=1024)TeamsSearch.ps1

The script prompts for:

  * the identity (admin ID) of the investigator
  * a name to save the Compliance search
  * the email address of the user to collecting



Once this information is provided the script will build and run the Compliance Search in O365. From this point you can log into Compliance Center, navigate to the search and then export the contents as a PST.
    
    
    <# MS Teams Security & Compliance Search
    author: Doug Metz https://github.com/dwmetz
    Note this script requires previous installation of the ExchangeOnlineManagement PowerShell module
    See https://docs.microsoft.com/en-us/powershell/exchange/connect-to-scc-powershell?view=exchange-ps for more information.#>
    [string]$user = Read-Host -Prompt 'Exchange Credentials'
    Connect-IPPSSession -UserPrincipalName $user
    [string]$name = Read-Host -Prompt 'Enter a name for the search'
    [string]$email = Read-Host -Prompt 'Enter the users email address'
    new-compliancesearch -name $name -ExchangeLocation $email -ContentMatchQuery 'kind=microsoftteams','ItemClass=IPM.Note.Microsoft.Conversation','ItemClass=IPM.Note.Microsoft.Missed','ItemClass=IPM.Note.Microsoft.Conversation.Voice','ItemClass=IPM.Note.Microsoft.Missed.Voice','ItemClass=IPM.SkypeTeams.Message'
    Start-ComplianceSearch $name
    Get-ComplianceSearch $name
    New-ComplianceSearchAction -SearchName $name -Export
    Write-Host "Search initiated"-ForegroundColor Blue
    Write-Host "Proceed to https://protection.office.com/ to download the results."-ForegroundColor Blue

Either copy the code from here, or download from my [GitHub](https://github.com/dwmetz/Axiom-PowerShell/blob/main/MS-TeamsSearch.ps1) repository. 
