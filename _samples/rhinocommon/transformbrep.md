---
layout: code-sample
title: Transform a Brep
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['transform', 'brep']
order: 163
description:  
---



```cs
public static Rhino.Commands.Result TransformBrep(Rhino.RhinoDoc doc)
{
  Rhino.DocObjects.ObjRef rhobj;
  var rc = RhinoGet.GetOneObject("Select brep", true, Rhino.DocObjects.ObjectType.Brep, out rhobj);
  if(rc!= Rhino.Commands.Result.Success)
    return rc;

  // Simple translation transformation
  var xform = Rhino.Geometry.Transform.Translation(18,-18,25);
  doc.Objects.Transform(rhobj, xform, true);
  doc.Views.Redraw();
  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Shared Function TransformBrep(doc As Rhino.RhinoDoc) As Rhino.Commands.Result
  Dim rhobj As Rhino.DocObjects.ObjRef = Nothing
  Dim rc = RhinoGet.GetOneObject("Select brep", True, Rhino.DocObjects.ObjectType.Brep, rhobj)
  If rc <> Rhino.Commands.Result.Success Then
    Return rc
  End If

  ' Simple translation transformation
  Dim xform = Rhino.Geometry.Transform.Translation(18, -18, 25)
  doc.Objects.Transform(rhobj, xform, True)
  doc.Views.Redraw()
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext

def TransformBrep():
    rc, objref = Rhino.Input.RhinoGet.GetOneObject("Select brep", True, Rhino.DocObjects.ObjectType.Brep)
    if rc!=Rhino.Commands.Result.Success: return
    
    # Simple translation transformation
    xform = Rhino.Geometry.Transform.Translation(18,-18,25)
    scriptcontext.doc.Objects.Transform(objref, xform, True)
    scriptcontext.doc.Views.Redraw()

if __name__=="__main__":
    TransformBrep()
```
{: #py .tab-pane .fade .in}

