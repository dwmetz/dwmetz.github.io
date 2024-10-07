---
layout: post
permalink: http://bakerstreetforensics.com/2022/11/20/group-collections-from-o365-with-powershell/
title: Group collections from O365 with PowerShell
description: None
date: 2022-11-20 16:08:47 -0000
last_modified_at: 2022-11-21 13:36:55 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Axiom
- O365
---
If you’re working in or responding to an O365 environment, there’s plenty of opportunities where you need to search and collect from multiple O365 custodians at the same time. While the experience of the Security & Compliance Center has improved over the years, I still find it inefficient for creating larger collections - especially when each custodian has to be searched for and added one at a time.

![](https://bakerstreetforensics.com/wp-content/uploads/2022/11/powershell-cim_1.jpeg?w=1024)

I created a handful of PowerShell scripts that automate the creation of searches for a group of custodians (provided via .txt file). I’ve used these methods countless times for both eDiscovery and IR cases. 

There are different scripts to address the collection of:

  * O365 Mailbox - will capture email, calendar, tasks, contacts, MS Teams*. 
  * Microsoft Teams - either for a single custodian or for a group.
  * Microsoft OneDrive - collect the O365 OneDrive for Business for a group of custodians.
  * When Legal says “get it all” - All O365 mailbox contents, including Teams, and OneDrive.



Once the collection has been generated you will still need to log on to https://protection.office.com to retrieve the search results. 

**Prerequisites:**

  * ExchaneOnlineManagment PowerShell Module
  * Microsoft.Online.SharePoint.PowerShell Module



**O365 Mailbox Collections** : MSExchangeGroupSearch.ps1
    
    
    <# MS Exchange Security & Compliance Search 
    version 2.0
    https://github.com/dwmetz/Axiom-PowerShell
    Author: @dwmetz
    Function:
        Collect an O365 mailbox search for group of custodians.
        Note this script requires previous installation of the ExchangeOnlineManagement PowerShell module
        See https://docs.microsoft.com/en-us/powershell/exchange/connect-to-scc-powershell?view=exchange-ps for more information.
          
    This PowerShell script will prompt you for the following information:
        * Your user credentials                                          
        * The pathname for the text file that contains a list of user email addresses
        * The name of the Content Search that will be created
        * The search query string
    The script will then:
        * Create and start a Content Search using the above information
    
    Updates:
        17.November.2022 - updated ExchangeOnlineManagement connection, Security & Compliance Center (IPPSSession)
    
    #>
    # New Auth
    Import-module ExchangeOnlineManagement
    Connect-IPPSSession
    # Get other required information
    $inputfile = read-host "Enter the file name of the text file that contains the email addresses for the users you want to search"
    $searchName = Read-Host "Enter the name for the new search"
    $searchQuery = Read-Host "[Optional] Enter the search query you want to use"
    $emailAddresses = Get-Content $inputfile | Where-Object {$_ -ne ""}  | ForEach-Object{ $_.Trim() }
    Write-Host "Creating and starting the search"
    $search = New-ComplianceSearch -Name $searchName -ExchangeLocation $emailAddresses -ContentMatchQuery $searchQuery
    # Finally, start the search and then display the status
    if($search)
    {
        Start-ComplianceSearch $search.Name
        Get-ComplianceSearch $search.Name
    } 
    Write-Host "Search initiated"-ForegroundColor Blue
    Write-Host "Proceed to https://protection.office.com/ to download the results."-ForegroundColor Blue

**O365 Mailboxes and OneDrives** : MS-ExchangeODGroupSearch.ps1

_Note: you will get 2 authentication prompts as you are logging on to Security & Compliance Center as well as the Sharepoint Admin panel._
    
    
    <# MS Exchange & OneDrive Security & Compliance Search 
    version 2.0
    https://github.com/dwmetz/Axiom-PowerShell
    Author: @dwmetz
    
    Function: This script will generate a Security and Compliance Search to capture O365 Email and OneDrive for a list of custodians.
    
    This PowerShell script will prompt you for the following information:
        * Your user credentials                                          
        * The pathname for the text file that contains a list of user email addresses
        * The name of the Content Search that will be created
        * The search query string (optional. mastering the search query cmd is a dark art.)
     The script will then:
        * Find the OneDrive for Business site for each user in the text file
        * Create and start a Content Search using the above information
    #>
    Import-module ExchangeOnlineManagement
    Import-Module Microsoft.Online.SharePoint.PowerShell
    Connect-SPOService -Credential $creds -Url https://magdev-admin.sharepoint.com -ModernAuth $true -AuthenticationUrl https://login.microsoftonline.com/organizations
    Connect-IPPSSession
    # Get other required information
    $script:inputfile = read-host "Enter the file name of the text file that contains the email addresses for the users you want to search"
    $searchName = Read-Host "Enter the name for the new search"
    $tempDir = "C:\Temp"
    New-Item $tempDir\ODUrls.txt
    ForEach ($emailAddress in Get-Content $script:inputfile)
    {
        $OneDriveURL = Get-SPOSite -IncludePersonalSite $true -Limit all -Filter "Owner -like $emailAddress" | Select-Object -ExpandProperty Url 
        if ($null -ne $OneDriveURL){ 
            Add-content $tempDir\ODUrls.txt $OneDriveURL
            Write-Host "$emailAddress => $OneDriveURL"
        } else {
            Write-Warning "Could not locate OneDrive for $emailAddress"
        }
    }
    $emailAddresses = Get-Content $inputfile | Where-Object {$_ -ne ""}  | ForEach-Object{ $_.Trim() }
    $urls = Get-Content $tempDir\ODUrls.txt | Where-Object {$_ -ne ""}  | ForEach-Object{ $_.Trim() }
    Write-Host "Creating and starting the search"
    # Collect OneDrive & Email
    $search = New-ComplianceSearch -Name $searchName -ExchangeLocation $emailAddresses -SharePointLocation $urls -ContentMatchQuery $searchQuery
    # Finally, start the search and then display the status
    if($search)
    {
        Start-ComplianceSearch $search.Name
        Get-ComplianceSearch $search.Name
    } 
    Remove-Item $tempDir\ODUrls.txt
    

**MS One Drive:** MSOneDriveSearch.ps1

_Note: you will get 2 authentication prompts as you are logging on to Security & Compliance Center as well as the Sharepoint Admin panel._
    
    
    <# MS OneDrive Security & Compliance Search 
    version 2.0
    https://github.com/dwmetz/Axiom-PowerShell
    Author: @dwmetz
    Function:
    
    Function: This script will generate a Security and Compliance Search to capture OneDrive for a list of custodians.
    
    This PowerShell script will prompt you for the following information:
        * Your user credentials                                          
        * The pathname for the text file that contains a list of user email addresses
        * The name of the Content Search that will be created
        * The search query string (optional. mastering the search query cmd is a dark art.)
     The script will then:
        * Find the OneDrive for Business site for each user in the text file
        * Create and start a Content Search using the above information
    #>
    Import-module ExchangeOnlineManagement
    Import-Module Microsoft.Online.SharePoint.PowerShell
    Connect-SPOService -Credential $creds -Url https://magdev-admin.sharepoint.com -ModernAuth $true -AuthenticationUrl https://login.microsoftonline.com/organizations
    Connect-IPPSSession
    # Get other required information
    $script:inputfile = read-host "Enter the file name of the text file that contains the email addresses for the users you want to search"
    $searchName = Read-Host "Enter the name for the new search"
    $tempDir = "C:\Temp"
    New-Item $tempDir\ODUrls.txt
    ForEach ($emailAddress in Get-Content $script:inputfile)
    {
        $OneDriveURL = Get-SPOSite -IncludePersonalSite $true -Limit all -Filter "Owner -like $emailAddress" | Select-Object -ExpandProperty Url 
        if ($null -ne $OneDriveURL){ 
            Add-content $tempDir\ODUrls.txt $OneDriveURL
            Write-Host "$emailAddress => $OneDriveURL"
        } else {
            Write-Warning "Could not locate OneDrive for $emailAddress"
        }
    }
    $urls = Get-Content $tempDir\ODUrls.txt | Where-Object {$_ -ne ""}  | ForEach-Object{ $_.Trim() }
    # Collect OneDrive 
    $search = New-ComplianceSearch -Name $searchName -SharePointLocation $urls -ContentMatchQuery $searchQuery
    # Finally, start the search and then display the status
    if($search)
    {
        Start-ComplianceSearch $search.Name
        Get-ComplianceSearch $search.Name
    } 
    Remove-Item $tempDir\ODUrls.txt
    

* **Microsoft Teams**

There are 2 scripts here for Microsoft Teams. Note - by default a Mailbox .pst file that contains Teams data, will not show that Teams data when the .pst is viewed with Outlook. Magnet AXIOM easily parses the Teams content, whether integrated as part of a mailbox collection, or from collections where just MS Teams data is captured.

**MS Teams - single custodian** : MSTeamsSearch.ps1
    
    
    <# MS Teams Security & Compliance Search 
    version 2.0
    https://github.com/dwmetz/Axiom-PowerShell
    Author: @dwmetz
    Function:
        Collect an O365 mailbox search for MS Teams communications.
        Note this script requires previous installation of the ExchangeOnlineManagement PowerShell module
        See https://docs.microsoft.com/en-us/powershell/exchange/connect-to-scc-powershell?view=exchange-ps for more information.
        
    Updates:
        25.October.2022 - updated ExchangeOnlineManagement connection, Security & Compliance Center (IPPSSession)
        
    #>
        Import-module ExchangeOnlineManagement
        Connect-ExchangeOnline
        [string]$aname = Read-Host -Prompt 'Enter your account name'
        Connect-IPPSSession -UserPrincipalName $aname
        [string]$name = Read-Host -Prompt 'Enter a name for the search'
        [string]$email = Read-Host -Prompt 'Enter the users email address'
        new-compliancesearch -name $name -ExchangeLocation $email -ContentMatchQuery 'kind=microsoftteams','ItemClass=IPM.Note.Microsoft.Conversation','ItemClass=IPM.Note.Microsoft.Missed','ItemClass=IPM.Note.Microsoft.Conversation.Voice','ItemClass=IPM.Note.Microsoft.Missed.Voice','ItemClass=IPM.SkypeTeams.Message'
        Start-ComplianceSearch $name
        Get-ComplianceSearch $name
        Write-Host "Search initiated."-ForegroundColor Cyan
        Write-Host "Proceed to https://protection.office.com/ to download the results."-ForegroundColor Cyan

**MS Teams - group of custodians** : MSTeamsGroupSearch.ps1
    
    
    <# MS Teams (Group) Security & Compliance Search 
    version 1.0
    https://github.com/dwmetz/Axiom-PowerShell
    Author: @dwmetz
    Function:
        Collect MS Teams for group of custodians in O365.
        Note this script requires previous installation of the ExchangeOnlineManagement PowerShell module
        See https://docs.microsoft.com/en-us/powershell/exchange/connect-to-scc-powershell?view=exchange-ps for more information.
          
    This PowerShell script will prompt you for the following information:
        * Your user credentials                                          
        * The pathname for the text file that contains a list of user email addresses
    
    The script will then:
        * Create and start a Content Search using the above information
    
    Updates:
        17.November.2022 - ExchangeOnlineManagement connection, Security & Compliance Center (IPPSSession)
    
    #>
    # New Auth
    Import-module ExchangeOnlineManagement
    Connect-IPPSSession
    # Get other required information
    $inputfile = read-host "Enter the file name of the text file that contains the email addresses for the users you want to search"
    $searchName = Read-Host "Enter the name for the new search"
    $emailAddresses = Get-Content $inputfile | Where-Object {$_ -ne ""}  | ForEach-Object{ $_.Trim() }
    Write-Host "Creating and starting the search"
    $search = New-ComplianceSearch -Name $searchName -ExchangeLocation $emailAddresses -ContentMatchQuery 'kind=microsoftteams','ItemClass=IPM.Note.Microsoft.Conversation','ItemClass=IPM.Note.Microsoft.Missed','ItemClass=IPM.Note.Microsoft.Conversation.Voice','ItemClass=IPM.Note.Microsoft.Missed.Voice','ItemClass=IPM.SkypeTeams.Message'
    # Finally, start the search and then display the status
    if($search)
    {
        Start-ComplianceSearch $search.Name
        Get-ComplianceSearch $search.Name
    } 
    Write-Host "Search initiated."-ForegroundColor Cyan
    Write-Host "Proceed to https://protection.office.com/ to download the results."-ForegroundColor Cyan

All of the scripts above can be downloaded from my Axiom-PowerShell [GitHub repo](https://github.com/dwmetz/Axiom-PowerShell). You can grab all the scripts at once by going to the latest [releases](https://github.com/dwmetz/Axiom-PowerShell/releases/tag/v2.0) file. 
