---
layout: post
title:  Bulk updating Azure AD user details
date:   2020-01-09T12:00:00+13:00
author: Scott Pettit
categories: azuread
tags: powershell
---

When we on-board customers to [Microsoft 365](https://www.end2end.co.nz/services/microsoft-365/) one of the first things we do is look to use Azure AD as the source of truth for user contact details. This makes implementing Multi-factor Authentication, email signatures etc much easier. Today I wanted to share with you two simple scripts for exporting and importing these details. You can get these scripts from our GitHub repo as well: (https://github.com/end2endnz/scripts/)

## Exporting User Details

This will output a nice CSV file that someone can review and update in Excel.

{% highlight powershell %}
#Requires -Modules AzureAD

###
# Get tenant ID
###
$tenantId = Read-Host -Prompt 'Specify tenant ID'

###
# Connect to AzureAD PowerShell
###
Connect-AzureAD -TenantId $tenantId

###
# Collect user information
###
"Collecting user information..."
$Output = "UserDetails.csv"
$Results = @()
$AADUsers = Get-AzureADUser -All $True

foreach($User in $AADUsers) {
    $Properties = &#91;ordered] @{
        DisplayName = $User.DisplayName
        UserPrincipalName = $User.UserPrincipalName
        CompanyName = $User.CompanyName
        JobTitle = $User.JobTitle
        Department = $User.Department
        Landline = $User.TelephoneNumber
        Mobile = $User.Mobile
        StreetAddress = $User.StreetAddress
        City = $User.City
        Country = $User.Country
        AccountEnabled = $User.AccountEnabled
    }
    $Results += New-Object psobject -Property $Properties
}

###
# Save results to CSV
###
$Results | Export-Csv -NoTypeInformation -Path $Output

###
# Disconnect from AzureAD
###
Disconnect-AzureAD
{% endhighlight %}

## Importing User Details

This script bulk updates to whatever is in your CSV file, so before you do this check over the CSV file, especially if you got someone else to update the CSV file. Phone numbers should be in E.164 format (+<countrycode> <areacode> <phonenumber>).

{% highlight powershell %}
#Requires -Modules AzureAD

###
# Get tenant ID
###
$tenantId = Read-Host -Prompt 'Specify tenant ID'
$userCsv = Read-Host -Prompt 'Specify path to user CSV file'

###
# Connect to AzureAD PowerShell
###
Connect-AzureAD -TenantId $tenantId

###
# Import user information
###
"Importing user information from CSV..."
$users = Import-Csv -Path $userCsv

foreach($user in $users){
    if($user.CompanyName){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -CompanyName $user.CompanyName
    }
    if($user.Department){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -Department $user.Department
    }
    if($user.JobTitle){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -JobTitle $user.JobTitle
    }
    if($user.Landline){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -TelephoneNumber $user.Landline
    }
    if($user.Mobile){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -Mobile $user.Mobile
    }
    if($user.StreetAddress){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -StreetAddress $user.StreetAddress
    }
    if($user.City){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -City $user.City
    }
    if($user.Country){
        Set-AzureADUser -ObjectId $user.UserPrincipalName -Country $user.Country
    }
}

###
# Disconnect from AzureAD
###
Disconnect-AzureAD
{% endhighlight %}