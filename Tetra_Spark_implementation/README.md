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

## Computing local topological features
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

## Computing global topological descriptors
### Directed graphs
The Compute_Graph_VE_Tetra_Spark.ipynb is used to compute the directed vertex-edge graph ($G_{VE}$).
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a GraphFrame containing the node DataFrame and arc DataFrame.

The Compute_Graph_FT_Tetra_Spark.ipynb is used to compute the directed triangle-tetrahedron graph ($G_{FT}$).
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a GraphFrame containing the node DataFrame and arc DataFrame.
 
### Morse manifolds
The Compute_A3_Tetra_Spark.ipynb is used to compute the ascending 3-manifolds.
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a DataFrame storing ascending 3-manifolds of each critical vertex as a connected component.

The Compute_D1_Tetra_Spark.ipynb is used to compute the descending 1-manifolds.
* Inputs:
  - the output of Compute_A3_Tetra_Spark.ipynb, which is the connected components corresponding to ascending 3-manifolds
* Outputs:
  - a DataFrame storing descending 1-manifolds connecting critical vertices and critical edges.

The Compute_D3_Tetra_Spark.ipynb is used to compute the descending 3-manifolds.
* Inputs:
  - the DataFrames $DF_V$ and $DF_T$
* Outputs:
  - a DataFrame storing descending 3-manifolds of each critical tetrahedron as a connected component.

The Compute_A1_Tetra_Spark.ipynb is used to compute the ascending 1-manifolds.
* Inputs:
  - the output of Compute_D3_Tetra_Spark.ipynb, which is the connected components corresponding to descending 3-manifolds
* Outputs:
  - a DataFrame storing ascending 1-manifolds connecting critical tetrahedra and critical triangles.
