---
title: "VBA Select Case statement"
excerpt: "Simple & easy example of Select Case statement in VBA"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - basics
---

<!-- short intrduction -->
## Intro

"Select Case" ("Switch" equivalent in JAVA) statement is used to do some action when condition is true. It is similar to if statement but it is better for cases, when You have to check many conditions (many "ifs" are required).


## Example

Below simple and easy example for Select Case in VBA:
```vb
Dim dayNumber 
dayNumber = 4    
Select Case dayNumber    
Case 1     
    Debug.Print "Monday" 
Case 2    
    Debug.Print "Tuesday" 
Case 3   
    Debug.Print "Wednesday" 
Case 4     
    Debug.Print "Thursday" 
Case 5    
    Debug.Print "Friday" 
Case 6    
    Debug.Print "Saturday" 
Case 7    
    Debug.Print "Sunday" 
Case Else    
    Debug.Print "Invalid day number" 
End Select
```
