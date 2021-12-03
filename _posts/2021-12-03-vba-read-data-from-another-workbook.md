---
title: "VBA read data from another WorkBook"
excerpt: "How to read data into array from another Excel WorkBoo in fast and easy way"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - Data manipulation
---

<!-- short intrduction -->
## Intro

One of the most common task is to read data from another Excel file (for example Sap raport) in VBA. The target is to have array with all data from report in order to further manipulation (recalculation). Let's find out how to do it.


## Example

Steps to implement:

1. Open another Excel workbook
2. Find out what is size of data (how many columns and rows)
3. Read data to array
4. Close file

{% include code-header.html %}
First code with opening file - We will use Sub that will open workbook:
```vb
Sub openWorkbook(filePath as String) 
    Dim workBook As Workbook
    Set workBook = Workbooks.Open(filePath)
    End Sub
```

Now We can use this object to read data:
{% include code-header.html %}
```vb
Function getArrayData(filePath as String, sh as WorkSheet)
  Dim lastRow As Long: lastRow = sh.Cells(sh.Rows.Count, 1).End(xlUp).Row
  Dim lastCol As Long: lastCol = sh.Cells(1, sh.Columns.Count).End(xlToLeft).Column
  Dim arrayData() As Variant
    arrayData = Workbook(filePath).sh.Range("A2:L" & lastrow).Value
    getArrayData = arrayData
End Function
```
{% include code-header.html %}
The last step is Sub for close file
```vb
Sub closeWorkbook(filePath as String) 
    Dim workBook As Workbook
    Set workBook = Workbooks.Close(filePath)
    End Sub
```


