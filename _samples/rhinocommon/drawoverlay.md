---
layout: code-sample
title: Display conduit to draw overlay text
author: 
categories: ['Draw'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['display', 'conduit', 'draw', 'overlay', 'text']
order: 67
description:  
---



```cs
class CustomConduit : Rhino.Display.DisplayConduit
{
  protected override void DrawForeground(Rhino.Display.DrawEventArgs e)
  {
    var bounds = e.Viewport.Bounds;
    var pt = new Rhino.Geometry.Point2d(bounds.Right - 100, bounds.Bottom - 30);
    e.Display.Draw2dText("Hello", System.Drawing.Color.Red, pt, false);
  }
}

[System.Runtime.InteropServices.Guid("58e7d4b7-407a-43b7-867b-3c517dd53d9d")]
public class ex_drawoverlay : Rhino.Commands.Command
{
  public override string EnglishName { get { return "csDrawOverlay"; } }

  readonly CustomConduit m_conduit = new CustomConduit();
  protected override Rhino.Commands.Result RunCommand(RhinoDoc doc, Rhino.Commands.RunMode mode)
  {
    // toggle conduit on/off
    m_conduit.Enabled = !m_conduit.Enabled;
    
    RhinoApp.WriteLine("Custom conduit enabled = {0}", m_conduit.Enabled);
    doc.Views.Redraw();
    return Rhino.Commands.Result.Success;
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Inherits Rhino.Commands.Command

Public Overrides ReadOnly Property EnglishName() As String
  Get
    Return "ex_drawoverlay"
  End Get
End Property

ReadOnly m_conduit As New CustomConduit()

Protected Overrides Function RunCommand(ByVal doc As RhinoDoc, ByVal mode As Rhino.Commands.RunMode) As Rhino.Commands.Result
  ' toggle conduit on/off
  m_conduit.Enabled = Not m_conduit.Enabled

  RhinoApp.WriteLine("Custom conduit enabled = {0}", m_conduit.Enabled)
  doc.Views.Redraw()
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import System.Drawing
import scriptcontext
import rhinoscriptsyntax as rs

# DisplayConduit subclass that overrides the DrawForeground function
# e is an instance of Rhino.Display.DrawEventArgs
class CustomConduit(Rhino.Display.DisplayConduit):
    def DrawForeground(self, e):
        color = System.Drawing.Color.Red
        bounds = e.Viewport.Bounds
        pt = Rhino.Geometry.Point2d(bounds.Right - 100, bounds.Bottom - 30)
        e.Display.Draw2dText("Hello", color, pt, False)


def showafterscript():
    # Create a custom conduit that can continue to draw after the
    # script has completed. The conduit is kept in the sticky
    # dictionary so we can get at it and turn it off in the future
    #
    # check to see if the conduit has been created and is in sticky
    conduit = None
    if scriptcontext.sticky.has_key("myconduit"):
        conduit = scriptcontext.sticky["myconduit"]
    else:
        # create a conduit and place it in sticky
        conduit = CustomConduit()
        scriptcontext.sticky["myconduit"] = conduit
    
    # Toggle enabled state for conduit. Every time this script is
    # run, it will turn the conduit on and off
    conduit.Enabled = not conduit.Enabled
    if conduit.Enabled: print "conduit enabled"
    else: print "conduit disabled"
    scriptcontext.doc.Views.Redraw()


def showinscript():
    # create a custom conduit that only displays during the execution
    # of this script. Once the script has completed, the conduit is turned
    # off and display goes back to normal
    conduit = CustomConduit()
    conduit.Enabled = True
    scriptcontext.doc.Views.Redraw()
    rs.GetString("Pausing for user input")
    conduit.Enabled = False
    scriptcontext.doc.Views.Redraw()

if __name__=="__main__":
    showinscript()
    #showafterscript()
```
{: #py .tab-pane .fade .in}


