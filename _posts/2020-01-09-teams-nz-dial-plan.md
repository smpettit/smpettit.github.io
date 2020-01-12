---
layout: post
title:  Setting up the NZ Local Dial Plan in Teams
date:   2020-01-09T16:00:00+13:00
author: Scott Pettit
categories: teams
tags: powershell
---

If you're looking to really maximise Microsoft Teams and turn on the [phone system](https://www.vorco.net/voice/microsoft-teams/) features then for New Zealand users you're probably also going to get push back from users if they now need to dial using E.164 format (e.g. +64 9 1234567) instead of just dialling the local number like they're used to.

## Step 1 - Create a dial plan for each of the local calling areas

First we create a new dial plan for each area code, they're empty to begin with and won't do anything useful.

{% highlight powershell %}
New-CsTenantDialPlan -Identity "NZ-09" -Description "Dial plan for NZ 09 area code users" -SimpleName "NZ-09"
New-CsTenantDialPlan -Identity "NZ-07" -Description "Dial plan for NZ 07 area code users" -SimpleName "NZ-07"
New-CsTenantDialPlan -Identity "NZ-06" -Description "Dial plan for NZ 06 area code users" -SimpleName "NZ-06"
New-CsTenantDialPlan -Identity "NZ-04" -Description "Dial plan for NZ 04 area code users" -SimpleName "NZ-04"
New-CsTenantDialPlan -Identity "NZ-03" -Description "Dial plan for NZ 03 area code users" -SimpleName "NZ-03"
{% endhighlight %}

## Step 2 - Add substitution (normalization) rules to each dial plan

Now we add rules for each area code to recognise local numbers and prefix them with the relevant area code. We also need to add handling for special numbers (toll-free, emergency, service codes).

{% highlight powershell %}
$nr1 = New-CsVoiceNormalizationRule -Parent "NZ-09" -InMemory -Pattern '^(&#91;2-9]\d{6})$' -Translation '+649$1' -Name "NZ-Local-09"
$nr2 = New-CsVoiceNormalizationRule -Parent "NZ-09" -InMemory -Pattern '^(?:(?:0(18|172|8&#91;1-9]\d*))|(12&#91;356]|&#91;19]11|105))$' -Translation '+64$1$2' -Name "NZ-Special"
Set-CsTenantDialPlan -Identity "NZ-09" -NormalizationRules @{add=$nr1}
Set-CsTenantDialPlan -Identity "NZ-09" -NormalizationRules @{add=$nr2}
$nr1 = New-CsVoiceNormalizationRule -Parent "NZ-07" -InMemory -Pattern '^(&#91;2-9]\d{6})$' -Translation '+647$1' -Name "NZ-Local-07"
$nr2 = New-CsVoiceNormalizationRule -Parent "NZ-07" -InMemory -Pattern '^(?:(?:0(18|172|8&#91;1-9]\d*))|(12&#91;356]|&#91;19]11|105))$' -Translation '+64$1$2' -Name "NZ-Special"
Set-CsTenantDialPlan -Identity "NZ-07" -NormalizationRules @{add=$nr1}
Set-CsTenantDialPlan -Identity "NZ-07" -NormalizationRules @{add=$nr2}
$nr1 = New-CsVoiceNormalizationRule -Parent "NZ-06" -InMemory -Pattern '^(&#91;2-9]\d{6})$' -Translation '+646$1' -Name "NZ-Local-06"
$nr2 = New-CsVoiceNormalizationRule -Parent "NZ-06" -InMemory -Pattern '^(?:(?:0(18|172|8&#91;1-9]\d*))|(12&#91;356]|&#91;19]11|105))$' -Translation '+64$1$2' -Name "NZ-Special"
Set-CsTenantDialPlan -Identity "NZ-06" -NormalizationRules @{add=$nr1}
Set-CsTenantDialPlan -Identity "NZ-06" -NormalizationRules @{add=$nr2}
$nr1 = New-CsVoiceNormalizationRule -Parent "NZ-04" -InMemory -Pattern '^(&#91;2-9]\d{6})$' -Translation '+644$1' -Name "NZ-Local-04"
$nr2 = New-CsVoiceNormalizationRule -Parent "NZ-04" -InMemory -Pattern '^(?:(?:0(18|172|8&#91;1-9]\d*))|(12&#91;356]|&#91;19]11|105))$' -Translation '+64$1$2' -Name "NZ-Special"
Set-CsTenantDialPlan -Identity "NZ-04" -NormalizationRules @{add=$nr1}
Set-CsTenantDialPlan -Identity "NZ-04" -NormalizationRules @{add=$nr2}
$nr1 = New-CsVoiceNormalizationRule -Parent "NZ-03" -InMemory -Pattern '^(&#91;2-9]\d{6})$' -Translation '+643$1' -Name "NZ-Local-03"
$nr2 = New-CsVoiceNormalizationRule -Parent "NZ-03" -InMemory -Pattern '^(?:(?:0(18|172|8&#91;1-9]\d*))|(12&#91;356]|&#91;19]11|105))$' -Translation '+64$1$2' -Name "NZ-Special"
Set-CsTenantDialPlan -Identity "NZ-03" -NormalizationRules @{add=$nr1}
Set-CsTenantDialPlan -Identity "NZ-03" -NormalizationRules @{add=$nr2}
{% endhighlight %}

## Step 3 - Apply the Dial Plan to your users

Depending on where your users are located in NZ you need to assign the right dial plan to them.

{% highlight powershell %}
Grant-CsTenantDialPlan -Identity aucklanduser@company.co.nz -PolicyName "NZ-09"
Grant-CsTenantDialPlan -Identity christchurchuser@company.co.nz -PolicyName "NZ-03"
{% endhighlight %}

Wait a bit for these to provision in Teams (can take a while, like 8 hours) and your users will now be able to dial local numbers and service numbers without having to format them as +64xxxxxxxx