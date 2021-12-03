---
title: "VBA read data from another WorkBook"
excerpt: "How to read data into array from another Excel WorkBook in fast and easy way"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags:
  - Data manipulation
---

<!-- short intrduction -->

## Intro

One of the most common task is to read data from another Excel file (for example SAP raport) in VBA. The target is to have array with all data from report in order to further manipulation (f.e. copy or recalculation). Let's find out how to do it.

## Example

Notes:

- We will use function "getArrayData" that will return array with all data (type: Variant)
- Function "getArrayData" will have only two parameters:
  1. full path to another workbook
  2. name of the sheet from which We will copy data
- We can use function in Sub to get array and proceed with further recalculation
- Source file will be opened and closed in "Read Only" mode in background

Code of the function:

{% include code-header.html %}

```vb
Function getArrayData(filePath As String, shName As String) As Variant()
  Dim wb As Workbook: Set wb = Workbooks.Open(filePath, True, True)
  Dim lastRow As Long: lastRow = wb.Sheets(shName).Cells(wb.Sheets(shName).Rows.Count, 1).End(xlUp).Row
  Dim lastCol As Long: lastCol = wb.Sheets(shName).Cells(1, wb.Sheets(shName).Columns.Count).End(xlToLeft).Column
  Dim arrayData() As Variant: arrayData = wb.Sheets(shName).Range(Cells(1, 1), Cells(lastRow, lastCol)).Value
  wb.Close False: Set wb = Nothing
  getArrayData = arrayData
End Function
```

Example how to use the function in Sub:
{% include code-header.html %}

```vb
Sub exampleSubThatManipulatesData()
Dim resultData() As Variant: resultData = getArrayData("C:\Report.xlsx", "Sheet1")
' -> proceed with resultData to manipulate data, for example print data to active sheet:
Range(Cells(1, 1), Cells(UBound(resultData, 1), UBound(resultData, 2))) = resultData
End Sub
```

That's all - simple and easy!
