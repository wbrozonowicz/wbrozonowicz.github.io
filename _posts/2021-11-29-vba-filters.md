---
title: "VBA filters"
excerpt: "Simple & easy example of filters in VBA"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - Excel
---

<!-- short intrduction -->
## Intro

Below simple and useful example how to setup and reset filters in Excel sheet using VBA.


## Example

In below example We have Sub "setupFiltersInSheet" with some parameters:
* sh : Excel sheet in which We want to setup filters
* lastRow : number of last row in sheet (see another post on my blog about the best method to get the last row in Excel sheet)
* criteriumName : String parameter that We want to setup in filter for column 26
* weekNum : another criterium, this time of type Integer and for column 28

Steps to implement in SUB:
1. turn off automatic calculation and screen updating (to speed up operation)
2. select sheet (based on sh parameter)
3. call another Sub "resetAllFiltersInSheet" that reset all filters (user could setup something in other column filter)
4. setup filters for columns (in below example: column 24, 26 and 28)
5. turn on back calculation and screen updating

Note: in column 24 we setup filter = current year number
{% include code-header.html %}
```vb
Sub setupFiltersInSheet(sh as Sheet, lastRow As Long, criteriumName As String, weekNum As Integer)
    Application.Calculation = xlManual
    Application.ScreenUpdating = False
        sh.Select
        Call resetAllFiltersInSheet(sh)
        ActiveSheet.Range("$A$1:$AF$" & lastRow).AutoFilter Field:=24, Criteria1:=year(Date)
        ActiveSheet.Range("$A$1:$AF$" & lastRow).AutoFilter Field:=26, Criteria1:=criteriumName
        ActiveSheet.Range("$A$1:$AF$" & lastRow).AutoFilter Field:=28, Criteria1:=weekNum
    Application.Calculation = xlAutomatic
    Application.ScreenUpdating = True
End Sub
```

Reset all filters in Excel sheet is a little bit tricky, because when there is no filter set in sheet, command ShowAllData will throw error. To avoid this We will wrap this code with "On Error" block in Sub "resetAllFiltersInSheet":
{% include code-header.html %}
```vb
Sub resetAllFiltersInSheet(sh as Sheet)
sh.Select
On Error Resume Next
    sh.ShowAllData
  On Error GoTo 0
End Sub
```

