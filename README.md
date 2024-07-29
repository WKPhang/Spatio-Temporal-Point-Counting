# Spatio-Temporal-Point-Counting
R scripts for spatiotemporal points counting in GIS application

## Introduction
Here, I will introduce the 2 different vector shapefiles: origins vector and points vector. Origins vector contains coordinated points where they will be used to calculate points surrounding them based on given condition, whereas, points vector contains coordinated points to be counted.
Note: It is a best practice to ensure that there is a unique identifier (ID) for each origin points.
Note: In this example, I assumed that the Cartesian based CRS was used.

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
# Define the Euclidean distance threshold (1 km to 20 km)
# Please change accordingly if other than Cartesian CRS is used 
radii <-  seq(1000, 20000, by = 500)

# Function to count the number of points within a given radius from origins
count_points_within_distance <- function(radius) {
  within_distance <- st_is_within_distance(origins_vector, points_vector, dist = radius)
  sapply(within_distance, length)
}

counts_list <- lapply(radii, function(radius) {
  counts <- count_points_within_distance(radius)
  data.frame(ID = 1:nrow(origins_vector), radius = radius / 1000, count = counts)
})

# Combine the results into a long-format data frame
counts_long <- bind_rows(counts_list) %>%
  mutate(radius = as.factor(radius))

# Join the counts with origins_vector based on ID
origins_data <- left_join(origins_vector, counts_long, by = "ID")

# Print the first few rows to check the result
print(origins_data)
```
