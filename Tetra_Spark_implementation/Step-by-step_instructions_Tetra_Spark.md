# Step-by-step instructions for reproducing the results
This repository implements the workflow described in "Distributed Topological Data Analysis of Tetrahedral Meshes with Apache Spark." The pipeline is organized into five major stages:
+ Encoding a tetrahedral mesh by two Spark DataFrames
+ Computing local topological features (e.g., the Forman gradient) from the base encoding
+ Building auxiliary directed graphs from the computed Forman gradient
+ Extracting 3-manifolds using graph-level connected components
+ Extracting 1-manifolds via BFS traversals within each component

## 1. Encoding a tetrahedral mesh by two Spark DataFrames
In this step, the .ts mesh is parsed to extract all vertices and tetrahedra (Convert_TS_DataFrames.ipyn). Vertices are sorted according to their elevation values and encoded into a vertex DataFrame $DF_V$. Extreme vertex indices within each tetrahedron are re-mapped to their elevation-based ordering. The tetrahedra, using these updated vertex indices, are then stored in the tetrahedron DataFrame $DF_T$. Both DataFrames are written to HDFS as CSV or Parquet, making them ready for distributed processing.

## 2. Computing local topological features (e.g., the Forman gradient) from the base encoding
This stage derives local topological structures from the tetrahedron encoding. The notebook Topo_Forman_Tetra_Spark.ipynb extracts the VT, VF, VE, EF, and FT relations (as specified in the code) and uses them to compute the Forman gradient. Additional topological features—such as distortions or critical point information—are computed in separate notebooks (Topo_distortion_Tetra_Spark.ipynb and Topo_CritPts_Tetra_Spark.ipynb) following similar Spark-based processing patterns.

## 3. Building auxiliary directed graphs from the computed Forman gradient
Based on the computed Forman gradient, auxiliary directed graphs ($G_{VE}$ and $G_{FT}$) are created to represent relationships among cells of the mesh. Each graph consists of two DataFrames: one for nodes and one for arcs. These graphs serve as the foundation for later manifold extraction.

## 4. Extracting 3-manifolds using graph-level connected components
Taking the computation of ascending 3-manifolds of critical vertices as an example, this step is completed using the Compute_A3_Tetra_Spark.ipynb. To obtain ascending 3-manifolds, connected components are computed directly on the auxiliary graphs using Spark’s built-in $connectedComponents()$ functionality. The inputs are the constructed graph $G_{VE}$, and the outputs are ascending 3-manifolds, represented by a DataFrame called $result\\_{con}$. Similarly, the computation of descending 3-manifolds of critical tetrahedra are completed by Compute_D3_Tetra_Spark.ipynb.

## Extracting 1-manifolds via BFS traversals within each component
This final stage performs a breadth-first search (BFS) within each connected component to trace 1-manifolds. Taking the computation of descending 1-manifolds connecting critical vertices and critical edges as an example, this step is performed using the Compute_D1_Tetra_Spark.ipynb program. The inputs are the output of Compute_A3_Tetra_Spark.ipynb, which is the connected components corresponding to ascending 3-manifolds. The outputs are descending 1-manifolds represented by a DataFrame.