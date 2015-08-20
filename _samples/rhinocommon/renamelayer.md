---
layout: code-sample
title: Rename a Layer
author: 
categories: ['Layers'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['rename', 'layer']
order: 140
description:  
---



```cs
public class RenameLayerCommand : Command
{
  public override string EnglishName { get { return "csRenameLayer"; } }

  protected override Result RunCommand(RhinoDoc doc, RunMode mode)
  {
    string layer_name = "";
    var rc = RhinoGet.GetString("Name of layer to rename", true, ref layer_name);
    if (rc != Result.Success)
      return rc;
    if (String.IsNullOrWhiteSpace(layer_name))
      return Result.Nothing;
   
    // because of sublayers it's possible that more than one layer has the same name
    // so simply calling doc.Layers.Find(layerName) isn't good enough.  If "layerName" returns
    // more than one layer then present them to the user and let him decide.
    var matching_layers = (from layer in doc.Layers
                           where layer.Name == layer_name
                           select layer).ToList<Layer>();

    Layer layer_to_rename = null;
    if (matching_layers.Count == 0)
    {
      RhinoApp.WriteLine("Layer \"{0}\" does not exist.", layer_name);
      return Result.Nothing;
    }
    else if (matching_layers.Count == 1)
    {
      layer_to_rename = matching_layers[0];
    }
    else if (matching_layers.Count > 1)
    {
      for (int i = 0; i < matching_layers.Count; i++)
      {
        RhinoApp.WriteLine("({0}) {1}", i+1, matching_layers[i].FullPath.Replace("::", "->"));
      }
      int selected_layer = -1;
      rc = RhinoGet.GetInteger("which layer?", true, ref selected_layer);
      if (rc != Result.Success)
        return rc;
      if (selected_layer > 0 && selected_layer <= matching_layers.Count)
        layer_to_rename = matching_layers[selected_layer - 1];
      else return Result.Nothing;
    }

    if (layer_to_rename == null)
      return Result.Nothing;

    layer_name = "";
    rc = RhinoGet.GetString("New layer name", true, ref layer_name);
    if (rc != Result.Success)
      return rc;
    if (String.IsNullOrWhiteSpace(layer_name))
      return Result.Nothing;

    layer_to_rename.Name = layer_name;
    if (!layer_to_rename.CommitChanges())
      return Result.Failure;
    return Result.Success;
  }
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Class RenameLayerCommand
  Inherits Command
  Public Overrides ReadOnly Property EnglishName() As String
    Get
      Return "vbRenameLayer"
    End Get
  End Property

  Protected Overrides Function RunCommand(doc As RhinoDoc, mode As RunMode) As Result
    Dim layer_name As String = ""
    Dim rc = RhinoGet.GetString("Name of layer to rename", True, layer_name)
    If rc <> Result.Success Then
      Return rc
    End If
    If [String].IsNullOrWhiteSpace(layer_name) Then
      Return Result.[Nothing]
    End If

    ' because of sublayers it's possible that more than one layer has the same name
    ' so simply calling doc.Layers.Find(layerName) isn't good enough.  If "layerName" returns
    ' more than one layer then present them to the user and let him decide.
    Dim matching_layers = (From layer In doc.Layers Where layer.Name = layer_name Select layer).ToList()

    Dim layer_to_rename As Layer = Nothing
    If matching_layers.Count = 0 Then
      RhinoApp.WriteLine("Layer ""{0}"" does not exist.", layer_name)
      Return Result.[Nothing]
    ElseIf matching_layers.Count = 1 Then
      layer_to_rename = matching_layers(0)
    ElseIf matching_layers.Count > 1 Then
      For i As Integer = 0 To matching_layers.Count - 1
        RhinoApp.WriteLine("({0}) {1}", i + 1, matching_layers(i).FullPath.Replace("::", "->"))
      Next
      Dim selected_layer As Integer = -1
      rc = RhinoGet.GetInteger("which layer?", True, selected_layer)
      If rc <> Result.Success Then
        Return rc
      End If
      If selected_layer > 0 AndAlso selected_layer <= matching_layers.Count Then
        layer_to_rename = matching_layers(selected_layer - 1)
      Else
        Return Result.[Nothing]
      End If
    End If

    If layer_to_rename Is Nothing Then
      Return Result.[Nothing]
    End If

    layer_name = ""
    rc = RhinoGet.GetString("New layer name", True, layer_name)
    If rc <> Result.Success Then
      Return rc
    End If
    If [String].IsNullOrWhiteSpace(layer_name) Then
      Return Result.[Nothing]
    End If

    layer_to_rename.Name = layer_name
    If Not layer_to_rename.CommitChanges() Then
      Return Result.Failure
    End If
    Return Result.Success
  End Function
End Class
```
{: #vb .tab-pane .fade .in}


```python
import rhinoscriptsyntax as rs
from scriptcontext import doc

def rename():
    layerName = rs.GetString("Name of layer to rename")
    
    matchingLayers = [layer for layer in doc.Layers if layer.Name == layerName]
    
    layerToRename = None
    if len(matchingLayers) == 0:
        print "Layer \"{0}\" does not exist.".format(layerName)
        return
    if len(matchingLayers) == 1:
        layerToRename = matchingLayers[0]
    elif len(matchingLayers) > 1:
        i = 0;
        for layer in matchingLayers:
            print "({0}) {1}".format(
                i+1, matchingLayers[i].FullPath.replace("::", "->"))
            i += 1
            
        selectedLayer = rs.GetInteger(
            "which layer?", -1, 1, len(matchingLayers))
        if selectedLayer == None:
            return
        layerToRename = matchingLayers[selectedLayer - 1]
    
    layerName = rs.GetString("New layer name")
    layerToRename.Name = layerName
    layerToRename.CommitChanges()
    return

if __name__ == "__main__":
    rename()
        
```
{: #py .tab-pane .fade .in}

