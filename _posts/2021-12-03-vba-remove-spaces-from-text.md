---
title: "VBA remove spaces from text"
excerpt: "Simple & easy example of removing spaces in VBA"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - basics
---

<!-- short intrduction -->
## Intro

Sometimes it is needed to remove spaces from text variable. It is easy task, all We need is simple function that will return result text (without spaces).

## Example

Function to remove spaces from text in VBA:
{% include code-header.html %}
```vb
Function getStringWithoutSpaces(textIn As String) As String
Dim temporaryText As String
    Do
        temporaryText = textIn
        textIn = Replace(textIn, " ", "")
    Loop Until temporaryText = textIn
getStringWithoutSpaces = textIn
End Function
```

Function use "Do ... Loop Until" to remove spaces: it is iterating with replacing space (" ") until the result is equal to input (no more spaces to replace). 