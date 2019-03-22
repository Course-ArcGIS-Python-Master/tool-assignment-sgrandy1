# Tool Assignment (due Friday 22nd March 1.30pm)

In this assignment, you are instructed to produce a small tool that takes advantage of arcpy and Python. You will need to provide example data, and the code should run on all PC's. The tool needs to manipulate a dataset across three different processes, for example, extracting, modifying and exporting data. The exact workflow is entirely up to yourself. You are expected to take 3-4 hours on this coding assignment, and you should deposit your code and example files within a Github repository for feedback and grading. 

The criteria are: 
* Cleanliness of code (10 points)
* Functionality (10 points)
* Appropriate use of documentation (10 points) 
* Depth of processing operation (10 points)
* In addition, you must provide example data and minimise the amount of editing a user must make in order for the program to run (10 points).

Help is here when you need it, just email Andy.

# Python Tool Project
# This tool can be used to spatially display Eastern Spadefoot Toad (Scaphiopus holbrookii) sitings in Florida. The
# information is derived from sitings in the eastern United States from inNaturalist Observations on GBIF.

# Convert CSV to Shapefile

import arcpy
# Set your workspace to the directory where you are storing your files
arcpy.env.workspace = r"C:\ArcGISWork\PythonCourse\PythonToolProject"

in_Table = r"EasternSpadefoot.csv"
x_coords = "decimalLongitude"
y_coords = "decimalLatitude"
z_coords = ""
out_Layer = "EasternSpadefoot"
saved_Layer = r"EasternSpadefoot_Output.shp"

# Set the spatial reference
spRef = arcpy.SpatialReference(4326)  # 4326 == WGS 1984

lyr = arcpy.MakeXYEventLayer_management(in_Table, x_coords, y_coords, out_Layer, spRef, z_coords)

# Print the total rows
print(arcpy.GetCount_management(out_Layer))

# Save to a layer file
arcpy.CopyFeatures_management(lyr, saved_Layer)

if arcpy.Exists(saved_Layer):
    print "Created file successfully!"

# Project Tool
# Local variables:
FloridaBoundary_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\FloridaBoundary.shp"
FloridaBoundary_Project_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\FloridaBoundary_Project.shp"

# Process: Project
arcpy.Project_management(FloridaBoundary_shp, FloridaBoundary_Project_shp, "GEOGCS['GCS_WGS_1984',DATUM['D_WGS_1984',SPHEROID['WGS_1984',6378137.0,298.257223563]],PRIMEM['Greenwich',0.0],UNIT['Degree',0.0174532925199433]]", "", "PROJCS['WGS_1984_Web_Mercator_Auxiliary_Sphere',GEOGCS['GCS_WGS_1984',DATUM['D_WGS_1984',SPHEROID['WGS_1984',6378137.0,298.257223563]],PRIMEM['Greenwich',0.0],UNIT['Degree',0.0174532925199433]],PROJECTION['Mercator_Auxiliary_Sphere'],PARAMETER['False_Easting',0.0],PARAMETER['False_Northing',0.0],PARAMETER['Central_Meridian',0.0],PARAMETER['Standard_Parallel_1',0.0],PARAMETER['Auxiliary_Sphere_Type',0.0],UNIT['Meter',1.0]]", "NO_PRESERVE_SHAPE", "", "NO_VERTICAL")

print "Florida boundary projected!"

# Clip Tool
# Local variables:
EasternSpadefoot_Output_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\EasternSpadefoot_Output.shp"
FloridaBoundary_Project_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\FloridaBoundary_Project.shp"
EasternSpadefoot_Clip_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\EasternSpadefoot_Clip.shp"

# Process: Clip
arcpy.Clip_analysis(EasternSpadefoot_Output_shp, FloridaBoundary_Project_shp, EasternSpadefoot_Clip_shp, "")

print "Clip Created!"

# Buffer Tool
# Local variables:
EasternSpadefoot_Clip_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\EasternSpadefoot_Clip.shp"
EasternSpadefoot_Buffer_shp = "C:\\ArcGISWork\\PythonCourse\\PythonToolProject\\EasternSpadefoot_Buffer.shp"

# Process: Buffer
arcpy.Buffer_analysis(EasternSpadefoot_Clip_shp, EasternSpadefoot_Buffer_shp, "100 Meters", "FULL", "ROUND", "NONE", "", "PLANAR")

print "Buffer created!"

print "Tool processes completed!"
