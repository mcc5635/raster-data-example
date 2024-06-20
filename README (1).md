
# Pachama Interview Take-home Test Instructions

## 1. Data Download Instructions
- **Tool**: `gsutil`
- **Commands**:
  ```bash
  mkdir pachama_interview_data/
  gsutil -m cp -R gs://pachama-interview-data/* pachama_interview_data/
  ```
- **Expected Folder Structure**:
  ```
  pachama_interview_data/
    ├── rasters/
    │   ├── 299_383_10.tif
    │   ├── 299_384_10.tif
    └── coordinates.geojson
  ```

## 2. Data Description
### Raster Files
- **Format**: GeoTIFF
- **Naming**: Spherical Mercator XYZ tile name
- **Number of Files**: 2
- **Source**: Landsat imagery
- **Spatial Resolution**: 30 meters
- **Temporal Range**: 1984 to 2020
- **Bands**: 6 bands per year
- **Total Bands per Raster**: 222 (37 years * 6 bands)
- **Dimensions**: 984 x 1305 pixels
- **Resulting Numpy Array Shape**: `[222, 984, 1305]`
- **Coordinate Reference System (CRS)**: WGS84

### Coordinate Data
- **Format**: GeoJSON
- **Geometry**: Shapely MultiPoint
- **Data**: (longitude, latitude) pairs
- **File Location**: Root level of the bucket (`coordinates.geojson`)

## 3. Task Objective
- **Goal**: Efficiently index into the provided rasters and extract "chips."
- **Definition of Chip**: A square of pixels centered around a coordinate for a given year.

## 4. Ideas for Extensions
- **Vectorized Approach**: Implement a completely vectorized version with numpy.
- **Machine Learning Integration**: Use a library like PyTorch or TensorFlow to create a dataloader, where a labeled example is a tuple consisting of a chip and the shapely geometry for its center coordinate.
- **Optimization Techniques**: Use `mmap` and/or caching to extract chips from a data stream.

## 5. Questions to Address
1. **Runtime and Memory Complexities**:
   - **Factors**:
     - Number of raster files
     - Number of coordinates
   - **Description**: Informally describe the complexities as a function of these factors.

2. **Scalability Considerations**:
   - **Scenario**: Processing 100x the number of rasters and/or coordinates.
   - **Ideal Implementation Changes**: Describe how the implementation would need to adapt.

## 6. Implementation Steps
1. **Data Loading**:
   - Use `gsutil` commands to download data.
   - Load raster files and GeoJSON coordinates into the appropriate data structures.

2. **Chip Extraction**:
   - For each coordinate, locate its position within the raster files.
   - Extract a square of pixels (chip) centered around the coordinate for the specified year.
   - Ensure efficient indexing and data extraction to handle large data volumes.

3. **Optional Extensions**:
   - Implement vectorized operations using numpy for faster processing.
   - Develop a dataloader with PyTorch or TensorFlow for machine learning applications.
   - Optimize data access using memory mapping (`mmap`) and caching mechanisms.
