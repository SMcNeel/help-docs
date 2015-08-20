---
layout: code-sample
title: Visual Analysis Modes
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['visual', 'analysis', 'modes']
order: 28
description:  
---



```cs
public override string EnglishName { get { return "cs_analysismode_on"; } }

protected override Rhino.Commands.Result RunCommand(RhinoDoc doc, Rhino.Commands.RunMode mode)
{
  // make sure our custom visual analysis mode is registered
  var zmode = Rhino.Display.VisualAnalysisMode.Register(typeof(ZAnalysisMode));

  const ObjectType filter = Rhino.DocObjects.ObjectType.Surface | Rhino.DocObjects.ObjectType.PolysrfFilter | Rhino.DocObjects.ObjectType.Mesh;
  Rhino.DocObjects.ObjRef[] objs;
  var rc = Rhino.Input.RhinoGet.GetMultipleObjects("Select objects for Z analysis", false, filter, out objs);
  if (rc != Rhino.Commands.Result.Success)
    return rc;

  int count = 0;
  for (int i = 0; i < objs.Length; i++)
  {
    var obj = objs[i].Object();

    // see if this object is alreay in Z analysis mode
    if (obj.InVisualAnalysisMode(zmode))
      continue;

    if (obj.EnableVisualAnalysisMode(zmode, true))
      count++;
  }
  doc.Views.Redraw();
  RhinoApp.WriteLine("{0} objects were put into Z-Analysis mode.", count);
  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Inherits Rhino.Commands.Command
Public Overrides ReadOnly Property EnglishName() As String
  Get
    Return "cs_analysismode_on"
  End Get
End Property

Protected Overrides Function RunCommand(doc As RhinoDoc, mode As Rhino.Commands.RunMode) As Rhino.Commands.Result
  ' make sure our custom visual analysis mode is registered
  Dim zmode = Rhino.Display.VisualAnalysisMode.Register(GetType(ZAnalysisMode))

  Const filter As ObjectType = Rhino.DocObjects.ObjectType.Surface Or Rhino.DocObjects.ObjectType.PolysrfFilter Or Rhino.DocObjects.ObjectType.Mesh
  Dim objs As Rhino.DocObjects.ObjRef() = Nothing
  Dim rc = Rhino.Input.RhinoGet.GetMultipleObjects("Select objects for Z analysis", False, filter, objs)
  If rc <> Rhino.Commands.Result.Success Then
    Return rc
  End If

  Dim count As Integer = 0
  For i As Integer = 0 To objs.Length - 1
    Dim obj = objs(i).[Object]()

    ' see if this object is alreay in Z analysis mode
    If obj.InVisualAnalysisMode(zmode) Then
      Continue For
    End If

    If obj.EnableVisualAnalysisMode(zmode, True) Then
      count += 1
    End If
  Next
  doc.Views.Redraw()
  RhinoApp.WriteLine("{0} objects were put into Z-Analysis mode.", count)
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
no python code sample available
```
{: #py .tab-pane .fade .in}


