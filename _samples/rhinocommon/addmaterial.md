---
layout: code-sample
title: Add a basic Material
author: 
categories: ['Adding Objects'] 
platforms: ['Cross-Platform']
apis: ['RhinoCommon']
languages: ['C#', 'Python', 'VB.NET']
keywords: ['basic', 'material']
order: 14
description:  
---



```cs
public static Rhino.Commands.Result AddMaterial(Rhino.RhinoDoc doc)
{
  // materials are stored in the document's material table
  int index = doc.Materials.Add();
  Rhino.DocObjects.Material mat = doc.Materials[index];
  mat.DiffuseColor = System.Drawing.Color.Chocolate;
  mat.SpecularColor = System.Drawing.Color.CadetBlue;
  mat.CommitChanges();

  // set up object attributes to say they use a specific material
  Rhino.Geometry.Sphere sp = new Rhino.Geometry.Sphere(Rhino.Geometry.Plane.WorldXY, 5);
  Rhino.DocObjects.ObjectAttributes attr = new Rhino.DocObjects.ObjectAttributes();
  attr.MaterialIndex = index;
  attr.MaterialSource = Rhino.DocObjects.ObjectMaterialSource.MaterialFromObject;
  doc.Objects.AddSphere(sp, attr);

  // add a sphere without the material attributes set
  sp.Center = new Rhino.Geometry.Point3d(10, 10, 0);
  doc.Objects.AddSphere(sp);

  doc.Views.Redraw();
  return Rhino.Commands.Result.Success;
}
```
{: #cs .tab-pane .fade .in .active}


```vbnet
Public Shared Function AddMaterial(ByVal doc As Rhino.RhinoDoc) As Rhino.Commands.Result
  ' materials are stored in the document's material table
  Dim index As Integer = doc.Materials.Add()
  Dim mat As Rhino.DocObjects.Material = doc.Materials(index)
  mat.DiffuseColor = System.Drawing.Color.Chocolate
  mat.SpecularColor = System.Drawing.Color.CadetBlue
  mat.CommitChanges()

  ' set up object attributes to say they use a specific material
  Dim sp As New Rhino.Geometry.Sphere(Rhino.Geometry.Plane.WorldXY, 5)
  Dim attr As New Rhino.DocObjects.ObjectAttributes()
  attr.MaterialIndex = index
  attr.MaterialSource = Rhino.DocObjects.ObjectMaterialSource.MaterialFromObject
  doc.Objects.AddSphere(sp, attr)

  ' add a sphere without the material attributes set
  sp.Center = New Rhino.Geometry.Point3d(10, 10, 0)
  doc.Objects.AddSphere(sp)

  doc.Views.Redraw()
  Return Rhino.Commands.Result.Success
End Function
```
{: #vb .tab-pane .fade .in}


```python
import Rhino
import scriptcontext
import System.Drawing

def AddMaterial():
    # materials are stored in the document's material table
    index = scriptcontext.doc.Materials.Add()
    mat = scriptcontext.doc.Materials[index]
    mat.DiffuseColor = System.Drawing.Color.Chocolate
    mat.SpecularColor = System.Drawing.Color.CadetBlue
    mat.CommitChanges()

    # set up object attributes to say they use a specific material
    sp = Rhino.Geometry.Sphere(Rhino.Geometry.Plane.WorldXY, 5)
    attr = Rhino.DocObjects.ObjectAttributes()
    attr.MaterialIndex = index
    attr.MaterialSource = Rhino.DocObjects.ObjectMaterialSource.MaterialFromObject
    scriptcontext.doc.Objects.AddSphere(sp, attr)

    # add a sphere without the material attributes set
    sp.Center = Rhino.Geometry.Point3d(10, 10, 0)
    scriptcontext.doc.Objects.AddSphere(sp)

    scriptcontext.doc.Views.Redraw();

if __name__=="__main__":
    AddMaterial()
```
{: #py .tab-pane .fade .in}


