# meshlab_extract_texture
# Steps to create Textured Mesh from Point Cloud using Meshlab
## Get your PointCloud into MeshLab
* Import the pointcloud file in ".ply" file format in [Meshlab](http://www.meshlab.net/). Before importing make sure you do some pre-processing / cleaning on point cloud so as to ease the process of meshing.

## Point Cloud Simplification and Normals Computation
* Next we need to reduce the number of point samples for smooth meshing.
  * So go to **Filters -> Point Set -> Point Cloud Simplification**. Enter **Number of samples** circa 5% of original number of points. Make sure **Best Sample Heuristic** is checked.
* After point cloud simplification, make sure to select **Simplified** point cloud in the **Show Layer Dialog** on the right hand side. If not visible, it can be opened by navigating to **View -> Show Layer Dialog**. Now we need to compute normals for point set.
  * So go to **Filters -> Point Set -> Compute normals for point sets** . Enter **Neighbour num** between 10 - 100. Initially try with 10 and try to get a mesh and later see if this can be improved by increasing the neighbour number. For **Smooth Iteration** initially try with 0 value and may be later it can be tried with values between 5 - 10. I mostly use value 8.
* Make sure if your normals are properly computed by going to  **Render -> Show Normal**.

## Meshing /  Poisson Surface Reconstruction
* Next we are going to use Poisson Surface reconstruction to do meshing.
  * So go to **Filters ->Remeshing, Simplification and Reconstruction -> Screened Poisson Surface Reconstruction**. Initially try with default parameters then later one can play around with reconstruction depth, number of samples and interpolation weight values.
  * This will create another mesh layer called **Poisson** in the **Show layer Dialog** which has surfaces now. Make sure to select that to peform further operations.
  * One can observe that it has also created some extra surfaces. To remove them go to **Filters -> Selection -> Select Faces with edges longer than ...**. By default the value is automatically computed, just click on apply. Then click on delete face button (triangle face and three vertex with a cross over it). This will remove extra surfaces.
  * After this operation, still some noise faces can be seen. To remove them go to **Filters -> Cleaning and Repairing -> Remove isolated pieces (wrt Face Num.)**. Use the default value and make sure **Remove unreferenced vertices** is checked. This will remove some noise faces.
  * Even after the above operation some noise faces are seen. To remove them go to **Filters -> Selection -> Select non Manifold Vertices**. Click apply. Then click on delete face button (triangle and threwe vertex with a cross over it). This will remove remaining extra faces.

## Texturizing the Mesh using pointcloud color attributes.
* Now that we have a mesh, next step is to get the texture for the mesh from the pointcloud.
* Make sure to select **Poisson** in the **Show layer Dialog** to peform further operations.
* Go to **Filters -> Texture -> Per Vertex Texture Function**. Click on apply.
* Go to **Filters -> Texture -> Convert PerVertex UV into PerWedge UV**. Click on apply.
* Go to **Filters -> Texture -> Parametrization: trivial Per-triangle**.
  * Quads -per -line : 0
  * Texture Dimension (px) : 4096 or (1024, 2048).
  * Inter-triangle border (px) : 0
  * Method : Basic (with Space-optimizing somethimes Meshlab crashes.)
  * Click on Apply.
* Now go to **File -> Save Project As..**. Save the project in ".mlp" file format.
* Now go to **File -> Export Mesh As..**. Save the mesh in ".obj" file format. After clicking on save, it will open saving options. Make sure under **Wedge**, **TexCoord** is checked. Then click on OK.
* Now go to **Filters -> Texture -> Transfer: Vertex Color to Texture**.
  * Texture file : (If your mesh was saved as mesh_1.obj, the texture file name should be mesh_1.png)
  * Texture width (px) : 4096 (make sure this is same as **Texture Dimension** in the above steps)
  * Texture height (px) : 4096 (make sure this is same as **Texture Dimension** in the above steps)
  * Check **Assign texture** and **Fill texture**.
  * Click on Apply.

* Now you have a textured mesh. There is also a texture image file saved in the same directory where original mesh was saved.
* If originally you saved your mesh as mesh_1.obj, after the above operation it will create mesh_1.obj.mtl and mesh_1.png. These are the three files needed to view textured mesh on any ".obj" file viewing software like Blender or autodesk etc.

Initially I couldn't find any proper step-by-step process to create textured mesh on meshlab. I hope these steps help people to come up with their own textured mesh. :D

If something is not clear, don't hesistate to comment on this.


Cheers!


