---
layout: code-sample
title: Move CPlane
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['move', 'cplane']
order: 114
description:  
---



```cs
readonly Rhino.DocObjects.ConstructionPlane m_cplane;
public MoveCPlanePoint(Rhino.DocObjects.ConstructionPlane cplane)
{
  m_cplane = cplane;
}

protected override void OnMouseMove(Rhino.Input.Custom.GetPointMouseEventArgs e)
{
  Plane pl = m_cplane.Plane;
  pl.Origin = e.Point;
  m_cplane.Plane = pl;
}

protected override void OnDynamicDraw(Rhino.Input.Custom.GetPointDrawEventArgs e)
{
  e.Display.DrawConstructionPlane(m_cplane);
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Inherits Rhino.Input.Custom.GetPoint
Private ReadOnly m_cplane As Rhino.DocObjects.ConstructionPlane
Public Sub New(ByVal cplane As Rhino.DocObjects.ConstructionPlane)
  m_cplane = cplane
End Sub

Protected Overrides Sub OnMouseMove(ByVal e As Rhino.Input.Custom.GetPointMouseEventArgs)
  Dim pl As Plane = m_cplane.Plane
  pl.Origin = e.Point
  m_cplane.Plane = pl
End Sub

Protected Overrides Sub OnDynamicDraw(ByVal e As Rhino.Input.Custom.GetPointDrawEventArgs)
  e.Display.DrawConstructionPlane(m_cplane)
End Sub
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext

class MoveCPlanePoint(Rhino.Input.Custom.GetPoint):
    def __init__(self, cplane):
        self.m_cplane = cplane
    def OnMouseMove(self, e):
        pl = self.m_cplane.Plane
        pl.Origin = e.Point
        self.m_cplane.Plane = pl
    def OnDynamicDraw(self, e):
        e.Display.DrawConstructionPlane(self.m_cplane);

def MoveCPlane():
    view = scriptcontext.doc.Views.ActiveView
    if not view: return Rhino.Commands.Result.Failure
    
    cplane = view.ActiveViewport.GetConstructionPlane()
    origin = cplane.Plane.Origin
    gp = MoveCPlanePoint(cplane)
    gp.SetCommandPrompt("CPlane origin")
    gp.SetBasePoint(origin, True)
    gp.DrawLineFromPoint(origin, True)
    gp.Get()
    if gp.CommandResult()!=Rhino.Commands.Result.Success:
        return gp.CommandResult()

    point = gp.Point()
    v = origin - point
    if v.IsTiny(): return Rhino.Commands.Result.Nothing
    pl = cplane.Plane
    pl.Origin = point
    cplane.Plane = pl
    view.ActiveViewport.SetConstructionPlane(cplane)
    view.Redraw()
    return Rhino.Commands.Result.Success


if __name__=="__main__":
    MoveCPlane()
```
{: #py .tab-pane .fade .in}


