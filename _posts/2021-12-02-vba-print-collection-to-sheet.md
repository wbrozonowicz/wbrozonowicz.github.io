---
title: "VBA print Collection to sheet"
excerpt: "Fast and easy way to print Collection of objects to Excel WorkSheet"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - OOP
---

<!-- short intrduction -->
## Intro

Collection is very useful, dynamic list of objects. But there is no inbuilt method to copy it to Excel worksheet. The standard way is to use for each loop, but it is slow method (especially for very big collections). So I developed fast Sub that makes this operation easy and very fast. Let's take a look.


## Example

Important note: object that is stored in collection has to have special array "params" of type Variant, where all properties are stored. It is described in my post about Vba class generator.

Sub has 4 parameters:
1. Collection that We want to print
2. target WorkSheet
3. starting row
4. starting column

{% include code-header.html %}
```vb
Sub printCollectionToSheet(objCollection As collection, outSheet As Worksheet, startRow As Long, startCol As Long)

Dim counter As Long: counter = 1 'counter of collection elements
Dim paramsCount As Integer: paramsCount = UBound(objCollection(1).Params) 'counter of parameters of objects in collection (objects have to be of one class only)

Dim arrayResult() As Variant ' array with result data to print
Dim rng As Range

Dim startCellAddress As String: startCellAddress = outSheet.Cells(startRow, startCol).Address()
Dim endCellAddress As String: endCellAddress = outSheet.Cells(startRow + objCollection.Count - 1, startCol + paramsCount).Address()
Set rng = outSheet.Range(startCellAddress & ":" & endCellAddress)
arrayResult = rng.Value

Dim nParams() As Variant ' array with object params
Dim obj As Variant ' object from collection
Dim i As Long ' loop counter

    For Each obj In objCollection
        nParams = obj.Params
        For i = LBound(nParams) To UBound(nParams)
        arrayResult(counter, i + 1) = nParams(i)
        Next
        counter = counter + 1
    Next

outSheet.Range(startCellAddress & ":" & endCellAddress) = arrayResult ' print values to sheet
End Sub
```

Code is commented so there is no need for further explanation. Enjoy!