---
title:  "Snippet: password generator on Windows"
date:   2016-12-19 07:00:00 +0000
modified: 2016-12-19 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/19/snippet-windows-password-generator/
categories: snippet
tags: windows batch snippet security
---

Once given the task to create about a hundred of (unsimilar) *Active Directory* users on a Windows domain controller, I needed a *batch* script to create random passwords to my clipboard. The constrains were clear: alphanumeric `[a-zA-Z0-9]` with minimum and maximum character count. A double-click is all that's needed for execution.


<!--more-->

In my case, I wanted an alphanumeric password with 10 characters *on a click*. The following script executes the `RandomString` command with the parameters set.

Create `RandomPassword.bat`

{% gist 4012a94235ddafaf67e4cf3ec2bf9618 %}



Create `RandomString.bat`

{% gist d4159b18a4a0b52fbb54d4210c53f579 %}

