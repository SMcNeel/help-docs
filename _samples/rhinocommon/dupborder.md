---
layout: code-sample
title: Duplicate borders of a surface
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['duplicate', 'borders', 'surface']
order: 69
description:  
---



```cs
public static Rhino.Commands.Result DupBorder(Rhino.RhinoDoc doc)
{
  const ObjectType filter = Rhino.DocObjects.ObjectType.Surface | Rhino.DocObjects.ObjectType.PolysrfFilter;
  Rhino.DocObjects.ObjRef objref;
  Rhino.Commands.Result rc = Rhino.Input.RhinoGet.GetOneObject("Select surface or polysurface", false, filter, out objref);
  if (rc != Rhino.Commands.Result.Success || objref == null)
    return rc;

  Rhino.DocObjects.RhinoObject rhobj = objref.Object();
  Rhino.Geometry.Brep brep = objref.Brep();
  if (rhobj == null || brep == null)
    return Rhino.Commands.Result.Failure;

  rhobj.Select(false);
  Rhino.Geometry.Curve[] curves = brep.DuplicateEdgeCurves(true);
  double tol = doc.ModelAbsoluteTolerance * 2.1;
  curves = Rhino.Geometry.Curve.JoinCurves(curves, tol);
  for (int i = 0; i < curves.Length; i++)
  {
    Guid id = doc.Objects.AddCurve(curves[i]);
    doc.Objects.Select(id);
  }
  doc.Views.Redraw();
  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Shared Function DupBorder(ByVal doc As Rhino.RhinoDoc) As Rhino.Commands.Result
  Const filter As ObjectType = Rhino.DocObjects.ObjectType.Surface Or Rhino.DocObjects.ObjectType.PolysrfFilter
  Dim objref As Rhino.DocObjects.ObjRef = Nothing
  Dim rc As Rhino.Commands.Result = Rhino.Input.RhinoGet.GetOneObject("Select surface or polysurface", False, filter, objref)
  If rc <> Rhino.Commands.Result.Success OrElse objref Is Nothing Then
    Return rc
  End If

  Dim rhobj As Rhino.DocObjects.RhinoObject = objref.[Object]()
  Dim brep As Rhino.Geometry.Brep = objref.Brep()
  If rhobj Is Nothing OrElse brep Is Nothing Then
    Return Rhino.Commands.Result.Failure
  End If

  rhobj.[Select](False)
  Dim curves As Rhino.Geometry.Curve() = brep.DuplicateEdgeCurves(True)
  Dim tol As Double = doc.ModelAbsoluteTolerance * 2.1
  curves = Rhino.Geometry.Curve.JoinCurves(curves, tol)
  For i As Integer = 0 To curves.Length - 1
    Dim id As Guid = doc.Objects.AddCurve(curves(i))
    doc.Objects.[Select](id)
  Next
  doc.Views.Redraw()
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext

def DupBorder():
    filter = Rhino.DocObjects.ObjectType.Surface | Rhino.DocObjects.ObjectType.PolysrfFilter
    rc, objref = Rhino.Input.RhinoGet.GetOneObject("Select surface or polysurface", False, filter)
    if rc != Rhino.Commands.Result.Success: return rc

    rhobj = objref.Object()
    brep = objref.Brep()
    if not rhobj or not brep: return Rhino.Commands.Result.Failure
    rhobj.Select(False)
    curves = brep.DuplicateEdgeCurves(True)
    tol = scriptcontext.doc.ModelAbsoluteTolerance * 2.1
    curves = Rhino.Geometry.Curve.JoinCurves(curves, tol)
    for curve in curves:
        id = scriptcontext.doc.Objects.AddCurve(curve)
        scriptcontext.doc.Objects.Select(id)
    scriptcontext.doc.Views.Redraw()
    return Rhino.Commands.Result.Success

if __name__=="__main__":
    DupBorder()
```
{: #py .tab-pane .fade .in}


