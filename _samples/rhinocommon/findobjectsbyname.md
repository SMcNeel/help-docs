---
layout: code-sample
title: Find Objects by Name
author: 
categories: ['Other'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['find', 'objects', 'name']
order: 85
description:  
---



```cs
public static Rhino.Commands.Result FindObjectsByName(Rhino.RhinoDoc doc)
{
  const string name = "abc";
  Rhino.DocObjects.ObjectEnumeratorSettings settings = new Rhino.DocObjects.ObjectEnumeratorSettings();
  settings.NameFilter = name;
  System.Collections.Generic.List<Guid> ids = new System.Collections.Generic.List<Guid>();
  foreach (Rhino.DocObjects.RhinoObject rhObj in doc.Objects.GetObjectList(settings))
    ids.Add(rhObj.Id);

  if (ids.Count == 0)
  {
    Rhino.RhinoApp.WriteLine("No objects with the name " + name);
    return Rhino.Commands.Result.Failure;
  }

  Rhino.RhinoApp.WriteLine("Found {0} objects", ids.Count);
  foreach (Guid id in ids)
    Rhino.RhinoApp.WriteLine("  {0}", id);

  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Shared Function FindObjectsByName(ByVal doc As Rhino.RhinoDoc) As Rhino.Commands.Result
  Const name As String = "abc"
  Dim settings As New Rhino.DocObjects.ObjectEnumeratorSettings()
  settings.NameFilter = name
  Dim ids As New System.Collections.Generic.List(Of Guid)()
  For Each rhObj As Rhino.DocObjects.RhinoObject In doc.Objects.GetObjectList(settings)
    ids.Add(rhObj.Id)
  Next

  If ids.Count = 0 Then
    Rhino.RhinoApp.WriteLine("No objects with the name " & name)
    Return Rhino.Commands.Result.Failure
  Else
    Rhino.RhinoApp.WriteLine("Found {0} objects", ids.Count)
    For i As Integer = 0 To ids.Count - 1
      Rhino.RhinoApp.WriteLine("  {0}", ids(i))
    Next
  End If

  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext
import System.Guid

def FindObjectsByName():
    name = "abc"
    settings = Rhino.DocObjects.ObjectEnumeratorSettings()
    settings.NameFilter = name
    ids = [rhobj.Id for rhobj in scriptcontext.doc.Objects.GetObjectList(settings)]
    if not ids:
        print "No objects with the name", name
        return Rhino.Commands.Result.Failure
    else:
        print "Found", len(ids), "objects"
        for id in ids: print "  ", id
    return Rhino.Commands.Result.Success

if __name__ == "__main__":
    FindObjectsByName()
```
{: #py .tab-pane .fade .in}

