# Spatio-Temporal-Point-Counting
R scripts for spatiotemporal points counting in GIS application

## Introduction
Here, I will introduce the 2 different vector shapefiles: origins vector and points vector. Origins vector contains coordinated points where they will be used to calculate points surrounding them based on given condition, whereas, points vector contains coordinated points to be counted.
Note: It is a best practice to ensure that there is a unique identifier (ID) for each origin points

## Preliminary scripts
```
library(sf)
library(dplyr)
library(tidyr)

# Read the shapefiles
origins_vector <- st_read("path_to_origin_vector.shp")
points_vector <- st_read("path_to_points_vector.shp")

# Ensure both layers have the same CRS (Coordinate Reference System)
if (st_crs(origins_vector) != st_crs(points_vector)) {
  points_vector <- st_transform(points_vector, st_crs(origins_vector))
}

# Ensure geometries are of type POINT
if (!all(st_geometry_type(origins_vector) == "POINT")) {
  stop("origins_vector should only contain point geometries.")
}
if (!all(st_geometry_type(points_vector) == "POINT")) {
  stop("points_vector should only contain point geometries.")
}
```

## Points counting at various radius from the origin sites
```




```
