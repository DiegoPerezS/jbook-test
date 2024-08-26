## Reducción de dimensiones

La reducción de dimensiones es necesaria para reducir el trabajo computacional, el ruido de la información, y para facilitar la representación gráfica de los datos. En los análisis de scRNA-seq se comparan las diferentes células en función de su expresión génica, por ejemplo, si usamos 2 genes para comparar las células, tendríamos un gráfico de dos dimensiones, en donde sólo habría dos puntos, uno por cada célula, y en cada eje estaría la expresión de cada gen. 

Para los sets de datos que se trabajan comúnmente en scRNA-seq, comúnmente se usan miles de genes para comparar las células entre sí, por lo que se trabajaría en un espacio de un alto número de dimensiones, pero el concepto se mantiene, cada célula será representada como un punto en el espacio multidimensional de acuerdo con su expresión génica. 

 Dado a que los diferentes patrones de expresión génica pueden estar correlacionados si las células se ven en un contexto biológico similar (como, por ejemplo, formar parte de un tejido determinado, o un grupo celular determinado), podemos comprimir la información de varios genes en una sola dimensión.


### 34: Análisis de componentes principales (PCA)

PCA: análisis que agrupa las muestras en torno a dimensiones que capturan componentes que generan mayor variabilidad. En este caso, los componentes principales serían N genes que pertenezcan a un grupo y cuyo perfil de expresión explique la variabilidad entre diferentes grupos de células. Cada uno de estos conjuntos de genes explica en mayor o menor medida la variabilidad de la expresión génica entre los diferentes grupos de células, por lo que este análisis nos permitiría seleccionar los distintos grupos de genes que expliquen en mayor medida las diferencias de expresión génica entre los diferentes grupos de células. 

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – escalado). 
* _Method used for plotting: _Computes PCA (principal component analysis) coordinates, loadings and variance decomposition, using 'tl.pca'
    * _Number of principal components to compute: _50
    * _Numpy data type string to which to convert the result:_ float32
        * _Type of PCA?: _Full PCA
            * _Compute standard PCA from covariance matrix?: _Yes
            * _“SVD solver to use”: _ARPACK wrapper in SciPy
            * _Initial states for the optimization: _0
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo – QC – norm – escalado.
    * _Rename dataset: _Archivo – QC – norm – esc - PCA.


### 35: Gráfico: PCA general

En este gráfico podemos ver la distribución de las células (representadas como puntos individuales) en función de los componentes principales en los que se agruparon los genes de acuerdo a sus patrones de expresión. En el eje X e Y veremos dos componentes principales diferentes (PCA 1, PCA 2, PCA 3, etc). Los grupos de células que posean patrones de expresión similares en genes agrupados en cada componente principal se agruparán en el gráfico. De la misma forma, al graficar PCA 1 vs PCA 2, y luego PCA 2 vs PCA 3, los grupos de células que se formen podrían cambiar, ya que cambiaron los grupos de genes, ya que estos no se repiten entre el PCA 1, PCA 2, y PCA 3.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – esc - PCA). 
* _Format for saving figures:_ png
* _Method used for plotting:_ PCA:_ _Plot PCA results, using 'pl.pca_overview'
    * _Sort order?:_ Yes
    * _Plot attributes:_
        * _Component:_
            * Presionar el ícono [+ Insert Component]
            * ***Solo fuera del espacio de Flujo de Trabajo: _Click on param-repeat Insert Component_**
            * _1: Component:_
                * _X-axis: _1
                * _Y-axis: _2
            * ***Solo fuera del espacio de Flujo de Trabajo: _Click on param-repeat Insert Component_**
            * Presionar el ícono [+ Insert Component]
            * _2: Component:_
                * _X-axis:_ 2
                * _Y-axis: _3
        * _Projection of plot:_ 2d
        * _Location of legend: _right margin
        * _Legent font weight: _normal
        * _Draw a frame around the scatter plot?: _Yes
        * _Number of panels per row: _2
        * _Width of the space between multiple panels: _0.1
        * _Height of the space between multiple panels: _0.25

            ***Algunas de estas configuraciones son exclusivas del espacio de Flujo de Trabajo.**

* _Configure Output: ‘out_png’:_
    * _Label:_ 35: PCA general.
    * _Rename dataset: _35: PCA general.


### 36: Gráfico: Ranking de genes en cada PCA

En este gráfico se seleccionan los genes expresados dentro de cada componente principal para identificar los genes que más contribuyen a la variación atribuida al componente al que pertenecen.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – esc - PCA). 
* _Format for saving figures:_ png
    * _Method used for plotting: _PCA: Rank genes according to contributions to PCs, using 'pl.pca_loadings'
        * _List of comma-sepparated components: _1,2,3
* _Configure Output: ‘out_png’:_
    * _Label:_ 36: Ranking de genes en cada PCA.
    * _Rename dataset: _36: Ranking de genes en cada PCA.


### 37: Gráfico: Influencia de genes sobre los componentes principales

En este gráfico se seleccionan los genes que mayor variación generan dentro de cada componente (paso 36). De esta forma se puede corroborar que dichos genes representarían la variabilidad de cada componente principal del que fueron extraídos. En este caso, se escogieron los genes de mayor relevancia en el PCA 1, PCA 2, y PCA 3, siendo CST3, NKG7, y PPBP, respectivamente.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – esc - PCA). 
* _Format for saving figures:_ png
* _Method used for plotting:_ PCA: Plot PCA results, using 'pl.pca_overview'
    * _Keys for annotations of obsercations/cells or variables/genes:_ CST3, NKG7, PPBP
    * _Use ‘raw’ attribute of input if present?:_ No
    * _Sort order?: _Yes
    * _Plot attributes:_
        * _Component:_
            * Presionar el ícono [+ Insert Component]
            * ***Solo fuera del espacio de Flujo de Trabajo: _Click on param-repeat Insert Component_**
            * _1: Component:_
                * _X-axis: _1
                * _Y-axis: _2
            * ***Solo fuera del espacio de Flujo de Trabajo: _Click on param-repeat Insert Component_**
            * Presionar el ícono [+ Insert Component]
            * _2: Component:_
                * _X-axis:_ 2
                * _Y-axis: _3
        * _Projection of plot:_ 2d
        * _Location of legend: _right margin
        * _Legent font weight: _normal
        * _Draw a frame around the scatter plot?: _Yes
        * _Number of panels per row: _2
        * _Width of the space between multiple panels: _0.1
        * _Height of the space between multiple panels: _025

            ***Algunas de estas configuraciones son exclusivas del espacio de Flujo de Trabajo.**

* _Configure Output: ‘out_png’:_
    * _Label:_ 37: PCAs por gen.
    * _Rename dataset: _37: PCAs por gen.


### 38: Gráfico: Variabilidad explicada por N componentes principales

En este gráfico se puede observar cuántos componentes principales explican la mayor variabilidad de expresión génica entre los diferentes grupos de células.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – esc - PCA). 
* _Format for saving figures:_ png
    * _Method used for plotting: _PCA: PCA: Scatter plot in PCA coordinates, using 'pl.pca_variance_ratio'
        * _Number of PCs to show: _30
        * _Use the log of the values?:_ Yes
* _Configure Output: ‘out_png’:_
    * _Label:_ 38: Variabilidad por cada PCA.
    * _Rename dataset: _38: Variabilidad por cada PCA.


### 39: Cálculo de vecinos cercanos (KNN)

En este paso se usa el método de K-nearest neighbors (KNN). Este método se encarga de agrupar las células con perfiles de expresión similares para así identificar diferentes poblaciones celulares dentro del análisis. Para este análisis se debe decidir cuántos vecinos son considerados para este cálculo (valor de k), por ejemplo, el perfil de expresión de una célula se comparará en general con todas las células, de este resultado, se tomarán las 3 células más similares a la célula evaluada y se irán agrupando para formar grupos más grandes. Cuando el análisis ya está en etapas más avanzadas y existen grupos formados, entonces la célula comparada será asignada al grupo con el que tenga más vecinos cercanos, por ejemplo, si dos vecinos son de la población A, y sólo un vecino pertenece a la población B, dicha célula será asignada al grupo A.

Utilizar la herramienta **“Inspect and Manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – esc - PCA). 
* _Method used for inspecting:_ Compute a neighborhood graph of observations, using 'pp.neighbors'
    * _The size of local neighborhood (in terms of number of neighboring data points) used for manifold approximation_: 10
    * _Number of PCs to use:_ 10
    * _Use a hard threshold to restrict the number of neighbors to n_neighbors?:_ Yes
    * _Numpy random seed:_ 0
    * _Method for computing connectivities:_ umap (McInnes et al, 2018)
    * _Distance metric:_ euclidean
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Archivo PCA - KNN.
    * _Rename dataset: _Archivo PCA - KNN.


### 40: Reducción de dimensiones: Generación de UMAP

En este paso se realiza una reducción dimensional, ya que el KNN genera relaciones multidimensionales y esto no es sencillo de visualizar. Para esto se escoge la técnica de UMAP, que se caracteriza por una mejor conservación de relaciones al disminuir las dimensiones de la agrupación por KNN.

Utilizar la herramienta **“Cluster, infer trajectories and embed with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA - KNN). 
* _Method used:_ Embed the neighborhood graph using UMAP, using 'tl.umap'
    * _Effective minimum distance between embedded points:_ 0.5
    * _Effective scale of embedded points: _1.0
    * _Number of dimensions of the embedding:_ 2
    * _Initial learning rate for the embedding optimization:_ 1.0
    * _Weighting applier to negative samples in low dimensional embedding optimization:_ 1.0
    * _The number of negative edge/1-simplex samples to use per positive edge/1-simplex sample in optimizing the low dimensional embedding: _5
    * _How to initialize the low dimensional embedding:_ Spectral embedding of the graph
    * _Random state:_ 0
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Archivo PCA – KNN - UMAP.
    * _Rename dataset: _Archivo PCA – KNN - UMAP.


### 41: Gráfico: UMAP

Representación gráfica de la agrupación de grupos de células generado anteriormente. En este caso se utilizan los genes relevantes designados en el paso 37, en la sección ['Tool Parameters'/'Keys for annotations of observations/cells or variables/genes'].

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA – KNN - UMAP). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Embeddings: Scatter plot in UMAP basis, using 'pl.umap'
    * _Keys for annotations of observations/cells or variables/genes:_ CST3, NKG7, PPBP
    * _Use ‘raw’ attribute of input if present?: _No
    * _Show edges?:_ No
    * _Show arrows?:_ No
    * _Sort order?:_ Yes
    * _Plot attributes:_
        * _Projection of plot:_ 2d
        * _Location of legend:_ right margin
        * _Legend font weight:_ normal
        * _Draw a frame around the scatter plot?:_ Yes
        * _Number of panels per row:_ 4
        * _Width of the space between multiple panels:_ 0.1
        * _Height of the space between multiple panels:_ 0.25
* _Number of panels per row:_ 2_ _
* _Configure Output: ‘out_png’:_
    * _Label:_ 41: gráfico UMAP.
    * _Rename dataset: _41: gráfico UMAP.


### 42: Agrupación de vecinos

En este paso se realizará la agrupación de las células vecinas, permitiendo ver las diferentes comunidades resultantes del análisis. Para esto se pueden utilizar tres métodos, SML, algoritmo Louvain, o el algoritmo Leiden. En este Flujo de Trabajo se sigue la recomendación de la fuente directa que lo generó, utilizando el algoritmo Louvain.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA – KNN - UMAP). 
* _Method used for plotting:_ Cluster cells into subgroups, using 'tl.louvain'
    * _Flavor for the clustering:_ vtraag (much more powerful)
        * _Resolution:_ 0.45
    * _Random state: _0
    * _Key under which to add the cluster labels:_ Louvain
    * _Interpret the adjacency matrix as directed graph?: _Yes
    * _Use weights from knn graph?: _No
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Archivo PCA – KNN – UMAP - agrupado.
    * _Rename dataset: _Archivo PCA – KNN – UMAP - agrupado.


### 43: Gráfico: diferentes UMAPS

En este paso se grafican los UMAPS de los tres genes de interés seleccionados en el paso 37, además del UMAP agrupado que se generó en el paso 42.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA – KNN – UMAP - agrupado). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Embeddings: Scatter plot in UMAP basis, using 'pl.umap'
    * _Keys for annotations of observations/cells or variables/genes_: louvain, CST3, NKG7, PPBP
    * _Use ‘raw’ attribute of input if present_: No
        * _Show edges?:_ No
    * _Show arrows?: _No
    * _Sort order?: _Yes
    * _Plot attributes:_
        * _Projection of plot: 2d_
        * _Location of legend:_ right margin
        * _Legend font weight:_ normal
        * _Draw a frame around the scatter plot?:_ Yes
        * _Number of panels per row:_ 2
        * _Width of the space between multiple panels:_ 0.1
        * _Height of the space between multiple panels:_ 0.25
        * _Number of panels per row:_ 2
* _Configure Output: ‘out_png’:_
    * _Label:_ 43: UMAP agrupado - UMAP genes.
    * _Rename dataset: _43: UMAP agrupado - UMAP genes.
