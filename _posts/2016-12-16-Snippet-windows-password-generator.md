---
title:  "Snippet: Windows password generator"
date:   2016-12-16 07:00:00 +0000
modified: 2016-12-16 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/16/snippet-windows-password-generator/
categories: snippet
tags: windows batch snippet security
---



<!--more-->


 - `RandomString.bat`

```
@echo off & setlocal EnableDelayedExpansion

REM Taken and changed from http://stackoverflow.com/a/5825866
REM
REM
REM Change these values to whatever you want, or change the code to take them
REM as command-line arguments.  You must set CHARS_LEN to the string length
REM of the string in the CHARS variable.
REM 
REM This script generates a string of these characters at least
REM MIN_CHARS_IN_LINE chars long and at most MAX_CHARS_IN_LINE chars long.

SET CHARS=abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789
SET /A CHARS_LEN=26 + 26 + 10
SET /A MIN_CHARS_IN_LINE=%1
SET /A MAX_CHARS_IN_LINE=%2

REM Pick a random line length and output a random character until we reach that
REM length.

call:rand %MIN_CHARS_IN_LINE% %MAX_CHARS_IN_LINE%
SET /A LINE_LENGTH=%RAND_NUM%

SET LINE=
for /L %%a in (1 1 %LINE_LENGTH%) do (
    call:rand 1 %CHARS_LEN%
    SET /A CHAR_INDEX=!RAND_NUM! - 1
    CALL SET EXTRACTED_CHAR=%%CHARS:~!CHAR_INDEX!,1%%
    SET LINE=!LINE!!EXTRACTED_CHAR!
)
echo|set /p=!LINE!

goto:EOF

REM The script ends at the above goto:EOF.  The following are functions.

REM rand()
REM Input: %1 is min, %2 is max.
REM Output: RAND_NUM is set to a random number from min through max.
:rand
SET /A RAND_NUM=%RANDOM% * (%2 - %1 + 1) / 32768 + %1
goto:EOF
```


 - `RandomPassword.bat`

```
@echo off
RandomString 8 8 | clip
```
