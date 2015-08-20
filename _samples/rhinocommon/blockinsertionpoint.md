---
layout: code-sample
title: Obtain insertion point of block
author: 
categories: ['Blocks'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['obtain', 'insertion', 'point', 'block']
order: 31
description:  
---



```cs
public static Rhino.Commands.Result BlockInsertionPoint(Rhino.RhinoDoc doc)
{
  Rhino.DocObjects.ObjRef objref;
  Result rc = Rhino.Input.RhinoGet.GetOneObject("Select instance", true, Rhino.DocObjects.ObjectType.InstanceReference, out objref);
  if (rc != Rhino.Commands.Result.Success)
    return rc;
  Rhino.DocObjects.InstanceObject instance = objref.Object() as Rhino.DocObjects.InstanceObject;
  if (instance != null)
  {
    Rhino.Geometry.Point3d pt = instance.InsertionPoint;
    doc.Objects.AddPoint(pt);
    doc.Views.Redraw();
  }
  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Shared Function BlockInsertionPoint(ByVal doc As Rhino.RhinoDoc) As Rhino.Commands.Result
  Dim rc As Rhino.Commands.Result
  Dim objref As Rhino.DocObjects.ObjRef = Nothing
  rc = Rhino.Input.RhinoGet.GetOneObject("Select instance", True, Rhino.DocObjects.ObjectType.InstanceReference, objref)
  If rc <> Rhino.Commands.Result.Success Then
    Return rc
  End If
  Dim instance As Rhino.DocObjects.InstanceObject = TryCast(objref.[Object](), Rhino.DocObjects.InstanceObject)
  If instance IsNot Nothing Then
    Dim pt As Rhino.Geometry.Point3d = instance.InsertionPoint
    doc.Objects.AddPoint(pt)
    doc.Views.Redraw()
  End If
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext

def BlockInsertionPoint():
    rc, objref = Rhino.Input.RhinoGet.GetOneObject("Select instance", True, Rhino.DocObjects.ObjectType.InstanceReference)
    if rc!=Rhino.Commands.Result.Success: return rc;
    instance = objref.Object()
    if instance:
        pt = instance.InsertionPoint
        scriptcontext.doc.Objects.AddPoint(pt)
        scriptcontext.doc.Views.Redraw()
        return Rhino.Commands.Result.Success
    return Rhino.Commands.Result.Failure

if __name__=="__main__":
    BlockInsertionPoint()
```
{: #py .tab-pane .fade .in}


