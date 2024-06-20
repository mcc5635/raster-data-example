
# Example Instructions

## 1. Data Download Instructions
- **Tool**: `gsutil`
- **Download from public Google Cloud Storage bucket with following commands using `gsutil` tool**:
  ```bash
  mkdir pachama_interview_data/
  gsutil -m cp -R gs://pachama-interview-data/* pachama_interview_data/
  ```
- **Once Downloaded, Expected Folder Structure**:
  ```
  `Schema:`
  pachama_interview_data/
    ├── rasters/
    │   ├── 299_383_10.tif  (part 1)
    │   ├── 299_384_10.tif  (part 2)
    └── coordinates.geojson (part 2)
  ```
  ```
- **Data Requirements**:
  `Raster Files:`
  - Rasters are stored in GeoTIFF format. 
  - Each file is named according to its spherical mercator XYZ tile name.
  - There are 2 raster files in the GCS bucket under the rasters directory.
  - The rasters are derived from Landsat imagery.
  - The spatial resolution of the raster data is 30 meters. We stacked yearly images from 1984 to 2020, where each year's image
  contains 6 bands of data (a raster band is analogous to an image channel).
  - The result is (37 years * 6 bands per year) = 222 total bands per raster file.
  - Each raster has dimensions of 984x1305 pixels.
  - The resulting numpy array for the file has the shape [222, 984, 1305] .
  - The coordinate reference system (CRS) for the rasters is WGS84.
  `Coordinates:`
  - The coordinate data is stored as a shapely MultiPoint geometry in a GeoJSON file.
  - Each coordinate represents a (longitude, latitude) pair .
  - You may assume that each coordinate listed exists in one of the raster files.
  - The GeoJSON file is located at the root level of the bucket with the name coordinates.geojson .
  ```
  `Task:`
  The goal of the assignment is to efficiently index into the provided rasters and extract
  "chips". A chip is defined as a square of pixels centered around a coordinate for a
  given year.
  ```
  `Ideas for extensions:`
  Create a completely vectorized version of this approach with numpy.
  Use a machine learning library such as PyTorch or TensorFlow to create a
  dataloader for this dataset, where a labeled example is a tuple consisting of a chip
  and the shapely geometry for its center coordinate.
  Use mmap and/or caching to extract chips from a data stream
  ```
  `Questions:`
  Informally describe the runtime and memory complexities of your solution as a
  function of:
  - The number of raster (.tif) files
  - The number of coordinates
  - How would an ideal implementation change if you had to process 100x the number of rasters and/or coordinates?
