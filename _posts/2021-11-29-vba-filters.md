---
title: "VBA filters"
excerpt: "Simple & easy example of filters in VBA"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags:
  - Excel
---

## Intro

Below simple and useful example how to setup and reset filters in Excel sheet using VBA. The goal is to have re-usable, configurable Sub for setup filters in sheet. We will cover two basic scenarios:

1. filter values from range (f.e. filter records with value > 1 and < 100)
2. filter with multiply criteria (list of values that We want to filter f.e. "New York", "Boston", "Chicago")

## Example

In below example We have Sub "setupFilters" with some parameters:

- options : it is variable with definition of our filters (type: Variant)
- sh : Excel sheet in which We want to setup filters (type: WorkSheet)

 Object "options" is nested array:

1. on the higher level is array of individual filter option (filter definition for one column)
2. on the middle level it is array with criteria for one column
3. on the lower level it is array of values to filter

Below example of this object:

```vb
Dim options As Variant
options = Array( _
Array(4, "filterRange", Array(">=1", "<100")), _
Array(5, "filterValues", Array("New York", "Boston", "Chicago")) _
)
```

Middle array takes three parameters:

- number of column to filter (f.e. 4 or 5)
- String variable for switching filter method
- array with filter values

There are two String variables to switch between filter methods:

- "filterRange" is for filter values from range
- "filterValues" is for filter with list of values

For "filterRange" method: array with filter values should be with 2 parameters:

```vb
Array(">=1", "<100")
```

For "filterValues" method: array with filter values could be array with many values:

```vb
Array("New York", "Boston", "Chicago")
```

Steps to implement in "setupFilters" Sub:

1. get last row and last column for filtering. In this example We will check data in the first row and in the first column, if data are specific and there are more records on different row/columns then on first, it should be adjust.
2. reset all filters in sheet (f.e. user could previously setup filter manually on some column, We want to reset this selection)
3. based on "options" object We will set Autofilter

{% include code-header.html %}

```vb
Sub setupFilters(options As Variant, sh As Worksheet)
Dim i As Integer
Dim lastRow As Long: lastRow = sh.Cells(sh.Rows.Count, 1).End(xlUp).Row
Dim lastCol As Long: lastCol = sh.Cells(1, sh.Columns.Count).End(xlToLeft).Column
  Call resetAllFiltersInSheet(sh)
  For i = LBound(options) To UBound(options)
    If (options(i)(1) = "filterValues") Then
        sh.Range(sh.Cells(1, 1), sh.Cells(lastRow, lastCol)).AutoFilter Field:=options(i)(0), Criteria1:=options(i)(2), Operator:=xlFilterValues
        ElseIf (options(i)(1) = "filterRange") Then
         sh.Range(sh.Cells(1, 1), sh.Cells(lastRow, lastCol)).AutoFilter Field:=options(i)(0), Criteria1:=options(i)(2)(0), Operator:=xlAnd, Criteria2:=options(i)(2)(1)
        End If
  Next
End Sub
```

Reset all filters in Excel sheet is a little bit tricky, because when there is no filter set in sheet, command ShowAllData will throw error. To avoid this We will wrap this code with "On Error" block in Sub "resetAllFiltersInSheet":

{% include code-header.html %}
```vb
Sub resetAllFiltersInSheet(sh As Worksheet)
On Error Resume Next
    sh.ShowAllData
  On Error GoTo 0
End Sub
```

The last step is to use previously crested Sub in project. We will call it from another Sub "setFilters":

{% include code-header.html %}
```vb
Sub setFilters()
Dim options As Variant
options = Array( _
Array(4, "filterRange", Array(">=1", "<9999")), _
Array(5, "filterValues", Array("AR", "ER", "CD")) _
)
Call setupFilters(options, shTest)
End Sub
```

We can now setup filters in many worksheets with different selections, only by specify dfferent "options" and Worksheet.
