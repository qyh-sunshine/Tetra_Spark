## Encoding a mesh as two DataFrames

The input file in the .ts format is converted to two DataFrames using the Convert_TS_DataFrames.ipynb program.
* Inputs:
  - a .ts file, e.g., brain.ts
* Outputs:
  - two .csv files corresponding to the DataFrames $DF_V$ and $DF_T$


## Deriving connectivity relations
The TopoRela_Boundary_Tetra_Spark.ipynb, TopoRela_Coboundary_Tetra_Spark.ipynb, and TopoRela_Adjacent_Tetra_Spark.ipynb programs extract boundary, coboundary, and adjacent relations for a mesh, respectively.
* Inputs:
  - the DataFrame $DF_T$
* Outputs:
  - a .parquet file in Spark, which is used to store the derived DataFrame corresponding to the desired connectivity relation.

## Computing topological features
### Discrete vertex distortion
The Topo_distortion_Tetra_Spark.ipynb is used to compute the discrete distortion for each vertex in a mesh.
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a DataFrame storing discrete distortion for each vertex in a mesh.
 
### Critical points
The Topo_CritPts_Tetra_Spark.ipynb is used to extract critical points in a mesh.
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a DataFrame storing the critical size of each vertex in a mesh.
 
### Forman gradient
The Topo_Forman_Tetra_Spark.ipynb is used to compute the Forman gradient in a mesh.
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a DataFrame storing critical simplices and simplex pairs in a mesh.
