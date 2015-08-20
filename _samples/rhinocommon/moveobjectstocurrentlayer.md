---
layout: code-sample
title: Move Selected Objects to Current Layer
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['move', 'selected', 'objects', 'current', 'layer']
order: 116
description:  
---



```cs
public class MoveSelectedObjectsToCurrentLayerCommand : Command
{
  public override string EnglishName
  {
    get { return "csMoveSelectedObjectsToCurrentLayer"; }
  }

  protected override Result RunCommand(RhinoDoc doc, RunMode mode)
  {
    // all non-light objects that are selected
    var object_enumerator_settings = new ObjectEnumeratorSettings();
    object_enumerator_settings.IncludeLights = false;
    object_enumerator_settings.IncludeGrips = true;
    object_enumerator_settings.NormalObjects = true;
    object_enumerator_settings.LockedObjects = true;
    object_enumerator_settings.HiddenObjects = true;
    object_enumerator_settings.ReferenceObjects = true;
    object_enumerator_settings.SelectedObjectsFilter = true;
    var selected_objects = doc.Objects.GetObjectList(object_enumerator_settings);

    var current_layer_index = doc.Layers.CurrentLayerIndex;
    foreach (var selected_object in selected_objects)
    {
      selected_object.Attributes.LayerIndex = current_layer_index;
      selected_object.CommitChanges();
    }
    doc.Views.Redraw();
    return Result.Success;
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Class MoveSelectedObjectsToCurrentLayerCommand
  Inherits Command
  Public Overrides ReadOnly Property EnglishName() As String
    Get
      Return "vbMoveSelectedObjectsToCurrentLayer"
    End Get
  End Property

  Protected Overrides Function RunCommand(doc As RhinoDoc, mode As RunMode) As Result
    ' all non-light objects that are selected
    Dim object_enumerator_settings = New ObjectEnumeratorSettings()
    object_enumerator_settings.IncludeLights = False
    object_enumerator_settings.IncludeGrips = True
    object_enumerator_settings.NormalObjects = True
    object_enumerator_settings.LockedObjects = True
    object_enumerator_settings.HiddenObjects = True
    object_enumerator_settings.ReferenceObjects = True
    object_enumerator_settings.SelectedObjectsFilter = True
    Dim selected_objects = doc.Objects.GetObjectList(object_enumerator_settings)

    Dim current_layer_index = doc.Layers.CurrentLayerIndex
    For Each selected_object As RhinoObject In selected_objects
      selected_object.Attributes.LayerIndex = current_layer_index
      selected_object.CommitChanges()
    Next
    doc.Views.Redraw()
    Return Result.Success
  End Function
End Class
```
{: #vb .tab-pane .fade .in}


```python
from Rhino import *
from Rhino.Commands import *
from Rhino.DocObjects import *
from scriptcontext import doc

def RunCommand():
  # all non-light objects that are selected
  object_enumerator_settings = ObjectEnumeratorSettings()
  object_enumerator_settings.IncludeLights = False
  object_enumerator_settings.IncludeGrips = True
  object_enumerator_settings.NormalObjects = True
  object_enumerator_settings.LockedObjects = True
  object_enumerator_settings.HiddenObjects = True
  object_enumerator_settings.ReferenceObjects = True
  object_enumerator_settings.SelectedObjectsFilter = True
  selected_objects = doc.Objects.GetObjectList(object_enumerator_settings)

  current_layer_index = doc.Layers.CurrentLayerIndex
  for selected_object in selected_objects:
    selected_object.Attributes.LayerIndex = current_layer_index
    selected_object.CommitChanges()

  doc.Views.Redraw()
  return Result.Success

if __name__ == "__main__":
  RunCommand()
```
{: #py .tab-pane .fade .in}

