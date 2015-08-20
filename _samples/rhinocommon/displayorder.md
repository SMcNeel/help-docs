---
layout: code-sample
title: Display Order
author: 
categories: ['Draw'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['display', 'order']
order: 59
description:  
---



```cs
public class DisplayOrderCommand : Command
{
  public override string EnglishName { get { return "csDisplayOrder"; } }

  private List<RhinoObject> m_line_objects = new List<RhinoObject>(); 

  protected override Result RunCommand(RhinoDoc doc, RunMode mode)
  {
    // make lines thick so draw order can be easily seen
    var dm = DisplayModeDescription.GetDisplayModes().Single(x => x.EnglishName == "Wireframe");
    var original_thikcness = dm.DisplayAttributes.CurveThickness;
    dm.DisplayAttributes.CurveThickness = 10;
    DisplayModeDescription.UpdateDisplayMode(dm);

    AddLine(Point3d.Origin, new Point3d(10,10,0), Color.Red, doc);
    AddLine(new Point3d(10,0,0), new Point3d(0,10,0), Color.Blue, doc);
    AddLine(new Point3d(8,0,0), new Point3d(8,10,0), Color.Green, doc);
    AddLine(new Point3d(0,3,0), new Point3d(10,3,0), Color.Yellow, doc);
    doc.Views.Redraw();
    Pause("draw order: 1st line drawn in front, last line drawn in the back.  Any key to continue ...");

    //all objects have a DisplayOrder of 0 by default so changing it to 1 moves it to the front.  Here we move the 2nd line (blue) to the front
    m_line_objects[1].Attributes.DisplayOrder = 1;
    m_line_objects[1].CommitChanges();
    doc.Views.Redraw();
    Pause("Second (blue) line now in front.  Any key to continue ...");

    for (int i = 0; i < m_line_objects.Count; i++)
    {
      m_line_objects[i].Attributes.DisplayOrder = i;
      m_line_objects[i].CommitChanges();
    }
    doc.Views.Redraw();
    Pause("Reverse order of original lines, i.e., Yellow 1st and Red last.  Any key to continue ...");

    // restore original line thickness
    dm.DisplayAttributes.CurveThickness = original_thikcness;
    DisplayModeDescription.UpdateDisplayMode(dm);

    doc.Views.Redraw();
    return Result.Success;
  }

  private void Pause(string msg)
  {
    ObjRef obj_ref;
    var rc = RhinoGet.GetOneObject(msg, true, ObjectType.AnyObject, out obj_ref);
  }

  private void AddLine(Point3d from, Point3d to, Color color, RhinoDoc doc)
  {
    var guid = doc.Objects.AddLine(from, to);
    var obj = doc.Objects.Find(guid);
    m_line_objects.Add(obj);
    obj.Attributes.ObjectColor = color;
    obj.Attributes.ColorSource = ObjectColorSource.ColorFromObject;
    obj.CommitChanges();
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Class DisplayOrderCommand
  Inherits Command
  Public Overrides ReadOnly Property EnglishName() As String
    Get
      Return "vbDisplayOrder"
    End Get
  End Property

  Private m_line_objects As New List(Of RhinoObject)()

  Protected Overrides Function RunCommand(doc As RhinoDoc, mode As RunMode) As Result
    ' make lines thick so draw order can be easily seen
    Dim dm = DisplayModeDescription.GetDisplayModes().[Single](Function(x) x.EnglishName = "Wireframe")
    Dim original_thikcness = dm.DisplayAttributes.CurveThickness
    dm.DisplayAttributes.CurveThickness = 10
    DisplayModeDescription.UpdateDisplayMode(dm)

    AddLine(Point3d.Origin, New Point3d(10, 10, 0), Color.Red, doc)
    AddLine(New Point3d(10, 0, 0), New Point3d(0, 10, 0), Color.Blue, doc)
    AddLine(New Point3d(8, 0, 0), New Point3d(8, 10, 0), Color.Green, doc)
    AddLine(New Point3d(0, 3, 0), New Point3d(10, 3, 0), Color.Yellow, doc)
    doc.Views.Redraw()
    Pause("draw order: 1st line drawn in front, last line drawn in the back.  Any key to continue ...")

    'all objects have a DisplayOrder of 0 by default so changing it to 1 moves it to the front.  Here we move the 2nd line (blue) to the front
    m_line_objects(1).Attributes.DisplayOrder = 1
    m_line_objects(1).CommitChanges()
    doc.Views.Redraw()
    Pause("Second (blue) line now in front.  Any key to continue ...")

    For i As Integer = 0 To m_line_objects.Count - 1
      m_line_objects(i).Attributes.DisplayOrder = i
      m_line_objects(i).CommitChanges()
    Next
    doc.Views.Redraw()
    Pause("Reverse order of original lines, i.e., Yellow 1st and Red last.  Any key to continue ...")

    ' restore original line thickness
    dm.DisplayAttributes.CurveThickness = original_thikcness
    DisplayModeDescription.UpdateDisplayMode(dm)

    doc.Views.Redraw()
    Return Result.Success
  End Function

  Private Sub Pause(msg As String)
    Dim obj_ref As ObjRef = Nothing
    Dim rc = RhinoGet.GetOneObject(msg, True, ObjectType.AnyObject, obj_ref)
  End Sub

  Private Sub AddLine(from As Point3d, [to] As Point3d, color As Color, doc As RhinoDoc)
    Dim guid = doc.Objects.AddLine(from, [to])
    Dim obj = doc.Objects.Find(guid)
    m_line_objects.Add(obj)
    obj.Attributes.ObjectColor = color
    obj.Attributes.ColorSource = ObjectColorSource.ColorFromObject
    obj.CommitChanges()
  End Sub
End Class
```
{: #vb .tab-pane .fade .in}


```python
from System.Collections.Generic import *
from System.Drawing import *
from Rhino import *
from Rhino.Commands import *
from Rhino.Display import *
from Rhino.Geometry import *
from Rhino.Input import *
from Rhino.DocObjects import *
from scriptcontext import doc

m_line_objects = []

def RunCommand():
  # make lines thick so draw order can be easily seen
  wfdm = [dm for dm in DisplayModeDescription.GetDisplayModes() if dm.EnglishName == "Wireframe"][0]
  original_thikcness = wfdm.DisplayAttributes.CurveThickness
  wfdm.DisplayAttributes.CurveThickness = 10
  DisplayModeDescription.UpdateDisplayMode(wfdm)

  AddLine(Point3d.Origin, Point3d(10,10,0), Color.Red, doc)
  AddLine(Point3d(10,0,0), Point3d(0,10,0), Color.Blue, doc)
  AddLine(Point3d(8,0,0), Point3d(8,10,0), Color.Green, doc)
  AddLine(Point3d(0,3,0), Point3d(10,3,0), Color.Yellow, doc)
  doc.Views.Redraw()
  Pause("draw order: 1st line drawn in front, last line drawn in the back.  Any key to continue ...")

  #all objects have a DisplayOrder of 0 by default so changing it to 1 moves it to the front.  Here we move the 2nd line (blue) to the front
  m_line_objects[1].Attributes.DisplayOrder = 1
  m_line_objects[1].CommitChanges()
  doc.Views.Redraw()
  Pause("Second (blue) line now in front.  Any key to continue ...")

  for i in range(0, m_line_objects.Count - 1):
    m_line_objects[i].Attributes.DisplayOrder = i
    m_line_objects[i].CommitChanges()

  doc.Views.Redraw()
  Pause("Reverse order of original lines, i.e., Yellow 1st and Red last.  Any key to continue ...")

  # restore original line thickness
  wfdm.DisplayAttributes.CurveThickness = original_thikcness
  DisplayModeDescription.UpdateDisplayMode(wfdm)

  doc.Views.Redraw()
  return Result.Success

def Pause(msg):
  rc, obj_ref = RhinoGet.GetOneObject(msg, True, ObjectType.AnyObject)

def AddLine(from_pt, to_pt, color, doc):
  guid = doc.Objects.AddLine(from_pt, to_pt)
  obj = doc.Objects.Find(guid)
  m_line_objects.Add(obj)
  obj.Attributes.ObjectColor = color
  obj.Attributes.ColorSource = ObjectColorSource.ColorFromObject
  obj.CommitChanges()

if __name__ == "__main__":
  RunCommand()
```
{: #py .tab-pane .fade .in}

