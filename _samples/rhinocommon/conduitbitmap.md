---
layout: code-sample
title: draw a non-square bitmap in a display conduit
author: 
categories: ['Draw'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['draw', 'non-square', 'bitmap', 'display', 'conduit']
order: 38
description:  
---



```cs
public class DrawBitmapConduit : Rhino.Display.DisplayConduit
{
  private readonly DisplayBitmap m_display_bitmap;

  public DrawBitmapConduit()
  {
    var flag = new System.Drawing.Bitmap(100, 100);
    for( int x = 0; x <  flag.Height; ++x )
        for( int y = 0; y < flag.Width; ++y )
            flag.SetPixel(x, y, Color.White);

    var g = Graphics.FromImage(flag);
    g.FillEllipse(Brushes.Blue, 25, 25, 50, 50);
    m_display_bitmap = new DisplayBitmap(flag);
  }

  protected override void DrawForeground(Rhino.Display.DrawEventArgs e)
  {
    e.Display.DrawBitmap(m_display_bitmap, 50, 50, Color.White);
  }
}

public class DrawBitmapCommand : Command
{
  public override string EnglishName { get { return "csDrawBitmap"; } }

  readonly DrawBitmapConduit m_conduit = new DrawBitmapConduit();

  protected override Result RunCommand(RhinoDoc doc, RunMode mode)
  {
    // toggle conduit on/off
    m_conduit.Enabled = !m_conduit.Enabled;
    
    RhinoApp.WriteLine("Custom conduit enabled = {0}", m_conduit.Enabled);
    doc.Views.Redraw();
    return Result.Success;
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Class DrawBitmapConduit
  Inherits Rhino.Display.DisplayConduit
  Private ReadOnly m_display_bitmap As DisplayBitmap

  Public Sub New()
    Dim flag = New System.Drawing.Bitmap(100, 100)
    For x As Integer = 0 To flag.Height - 1
      For y As Integer = 0 To flag.Width - 1
        flag.SetPixel(x, y, Color.White)
      Next
    Next

    Dim g = Graphics.FromImage(flag)
    g.FillEllipse(Brushes.Blue, 25, 25, 50, 50)
    m_display_bitmap = New DisplayBitmap(flag)
  End Sub

  Protected Overrides Sub DrawForeground(e As Rhino.Display.DrawEventArgs)
    e.Display.DrawBitmap(m_display_bitmap, 50, 50, Color.White)
  End Sub
End Class
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
from Rhino.Geometry import *
import System.Drawing
import Rhino.Display
import scriptcontext
import rhinoscriptsyntax as rs

class CustomConduit(Rhino.Display.DisplayConduit):
    def __init__(self):
      flag = System.Drawing.Bitmap(100,100)
      for x in range(0,100):
        for y in range(0,100):
          flag.SetPixel(x, y, System.Drawing.Color.Red)
      g = System.Drawing.Graphics.FromImage(flag)
      g.FillEllipse(System.Drawing.Brushes.Blue, 25, 25, 50, 50)
      self.display_bitmap = Rhino.Display.DisplayBitmap(flag)

    def DrawForeground(self, e):
      e.Display.DrawBitmap(self.display_bitmap, 50, 50, System.Drawing.Color.Red)

if __name__== "__main__":
    conduit = CustomConduit()
    conduit.Enabled = True
    scriptcontext.doc.Views.Redraw()
    rs.GetString("Pausing for user input")
    conduit.Enabled = False
    scriptcontext.doc.Views.Redraw()
```
{: #py .tab-pane .fade .in}

