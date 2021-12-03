---
title: "VBA Automatic class generator"
excerpt: "Automatic class generator for VBA projects"
last_modified_at: 2017-03-09T10:27:01-05:00
categories:
  - VBA
tags: 
  - Excel
---

<!-- short intrduction -->
## Intro

Creating new class in VBA is not very cool: IDE is not comfortable and there are no automatic code generators (getters, setters etc.) like for example in Intellij (Java). I created my own implementation of automatic generator of new class, with getters and setters and toString function. Below example.


## Example

How it's working:
* in one sheet there are definitions of all properties:

![sheet with class definition](/images/class_generator/1.JPG)

* Sub "GenerateNewClass()" uses values from cells in this sheet to create vba code, that can be used in new class module

* code is printed to another sheet, from which is easy to copy result

Let's take a look at the code (this time a little bit longer ;-) :

{% include code-header.html %}
```vb
Sub GenerateNewClass()

Dim i As Integer
Dim propertyName As String
Dim propertyType As String
Dim worksheetProps As Worksheet: Set worksheetProps = shNewObject
Dim worksheetClass As Worksheet: Set worksheetClass = shCls
Dim arrayData() As Variant
Dim lineNum As Integer: lineNum = 1
Dim outCollection As New collection

shCls.Cells.ClearContents

outCollection.add "Option Explicit"
outCollection.add ""

Dim rng As Range
Set rng = worksheetProps.Range("A2").CurrentRegion.Offset(1, 0): Set rng = rng.Resize(rng.rows.Count - 1)
arrayData = rng

    For i = LBound(arrayData) To UBound(arrayData)
    ' variable definition
    propertyName = arrayData(i, 1)
    propertyName = "p" & UCase(Left(propertyName, 1)) & Mid(propertyName, 2)
    propertyType = arrayData(i, 2)
    outCollection.add "Private " & propertyName & " As " & propertyType
    Next

outCollection.add "Private pParams() As Variant"

    For i = LBound(arrayData)  To UBound(arrayData)
    propertyName = arrayData(i, 1)
    propertyName = "p" & UCase(Left(propertyName, 1)) & Mid(propertyName, 2)
    propertyType = arrayData(i, 2)
   
    'getter definition
    outCollection.add ""
    outCollection.add "Property Get " & Mid(propertyName, 2) & "() As " & propertyType
    outCollection.add Mid(propertyName, 2) & " = " & propertyName
    outCollection.add "End Property"
   
    'setter definition
    outCollection.add ""
    outCollection.add "Property Let  " & Mid(propertyName, 2) & "(param" & Mid(propertyName, 2) & " As " & propertyType & ")"
    outCollection.add propertyName & " = param" & Mid(propertyName, 2)
    outCollection.add "End Property"
    Next

'params - setter
Dim paramsString As String
outCollection.add ""
paramsString = "pParams = Array("
outCollection.add "Public Sub setParams()"

    For i = LBound(arrayData) To UBound(arrayData)
    propertyName = arrayData(i, 1)
    propertyName = UCase(Left(propertyName, 1)) & Mid(propertyName, 2)
    paramsString = paramsString & "Me." & propertyName
        If i < UBound(arrayData) Then
        paramsString = paramsString & Chr(44)
        Else
        paramsString = paramsString & ")"
        End If
    Next

outCollection.add paramsString
outCollection.add "End Sub"

'params - getter
outCollection.add ""
outCollection.add "Property Get Params() As Variant()"
outCollection.add "Params = pParams"
outCollection.add "End Property"

' toString definition
outCollection.add ""
Dim outString As String: outString = "toString="
Dim lastSign As String
outCollection.add "Public Function toString()"

    For i = LBound(arrayData) To UBound(arrayData)
        If i = UBound(arrayData) Then
        lastSign = ""
        Else
        lastSign = Chr(38) & Chr(32) & Chr(34) & "|" & Chr(34) & Chr(32) & Chr(38) & Chr(32)
        End If
    propertyName = arrayData(i, 1)
    propertyName = "p" & UCase(Left(propertyName, 1)) & Mid(propertyName, 2)
    propertyType = arrayData(i, 2)
    outString = outString & Chr(34) & Mid(propertyName, 2) & Chr(34) & Chr(32) & Chr(38) & Chr(32) & Chr(34) & "=" & Chr(34) & Chr(32) & Chr(38) & Chr(32) & "me." & Mid(propertyName, 2) & Chr(32) & lastSign
    Next

outCollection.add outString
outCollection.add "End Function"

' print generated class to sheet
Dim obj As Variant
Dim outArray() As Variant
outArray = shCls.Range(Cells(1, 1), Cells(outCollection.Count, 1))
    For Each obj In outCollection
        outArray(lineNum, 1) = outCollection(lineNum)
        lineNum = lineNum + 1
    Next
shCls.Range(Cells(1, 1), Cells(outCollection.Count, 1)) = outArray

End Sub
```


Now let's discuss some parts of the code:
1. generator use two sheets: 
    * input is in shNewObject (all properties and types)
    * output is in shCls (generated code is in the first column of this WorkSheet)
2. exery line of the generated code is just a String added to Collection object
3. new class has special property: "params". In this array We will have all other properties. The reason of this is to have possibility to iterate through all object properties (could be very handy for further use - f.e. writing out Collection to sheet, subject of another post on my blog)
4. after changing properties in code (assign values to properties), it is necessary to use setParams function to update this array
4. to speed up final writing out of the code to sheet, results from Collection object is copied to array (to avoid writing line by line to cells, which is slow)
5. array (with final result) is in single step just printed to worksheet range

Below results that can be copy and paste to new class module:

![sheet with result](/images/class_generator/2.JPG)

That's all, good luck with generating new classes in VBA!