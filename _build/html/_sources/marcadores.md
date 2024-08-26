## Identificación de marcadores


### 44: Identificación de marcadores (t-test)

Se usará el análisis de t-test como método para encontrar los genes diferencialmente expresados que sean relevantes entre los diferentes grupos de células obtenidos previamente.

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA – KNN – UMAP - agrupado). 
* _Method used for inspecting:_ Rank genes for characterizing groups, using 'tl.rank_genes_groups'
    * _The key of the observations grouping to consider:_ louvain
    * _Use ‘raw’ attribute of input if present_: Yes
    * _Comparison:_ Compare each group to the union of the rest of the group
    * _The number of genes that appear in the returned tables:_ 100
        * _Method:_ t-test
        * _P-value correction method:_ Benjamini-Hochberg
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Archivo UMAP agrupado – t-test.
    * _Rename dataset: _Archivo UMAP – agr – t-test.


### 45: Identificación de marcadores (Wilcoxon)

Este paso es una alternativa al paso 44, ocupando otro método en lugar del t-test. El método de Wilcoxon es recomendable para usar en publicaciones, y se basa en evaluar la separación entre la distribución de expresión de genes de los diferentes grupos.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo PCA – KNN – UMAP - agrupado). 
* _Method used for inspecting:_ Rank genes for characterizing groups, using 'tl.rank_genes_groups'
    * _The key of the observations grouping to consider:_ louvain
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Comparison:_ Compare each group to the union of the rest of the group
    * _The number of genes that appear in the returned tables:_ 100
        * _Method:_ Wilcoxon-Rank-Sum
        * _P-value correction method:_ Benjamini-Hochberg
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Archivo UMAP agrupado – wilc.
    * _Rename dataset: _Archivo UMAP agrupado – wilc.


### 46: Gráfico: Genes con mayor influencia en cada grupo de células (t-test)

Este gráfico coloca a los genes en el eje X identificados con un número, este número representaría únicamente un gen, no es un valor que tomar en cuenta; la abreviación de cada gen se grafica encuentra perpendicular a su ubicación en el eje X, a la altura de su valor en el eje Y. En el eje Y se grafica el score de los genes, el cual representa la influencia de este gen en cada grupo de genes formado en los análisis anteriores.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – t-test).
* _Format for saving figures:_ png
* _Method used for plotting:_ Marker genes: Plot ranking of genes using dotplot plot, using 'pl.rank_genes_groups'
    * _Number of genes to show:_ 20
    * _Font size for gene names:_ 8
    * _Number of panels per row: _3
    * _Should the y-axis of each panels be shared?: _No (colocar esta opción en ‘Yes’ hará que el eje Y tenga el mismo rango de valores entre todos los gráficos)
* _Configure Output: ‘out_png’:_
    * _Label:_ 46: Principales genes en cada grupo (t-test).
    * _Rename dataset: _46: Principales genes en cada grupo (t-test).


### 47: Genes con mayor influencia (t-test)

En este paso se identifican los genes con mayor influencia en cada grupo de genes previamente obtenido mediante el uso de los datos de UMAP. Esta información se entrega como tabla, complementaria al gráfico del paso 46.

En este módulo se pueden seleccionar la información que tendrá el output, en donde podemos escoger:



* Los nombres de genes en cada grupo (uns_rank_genes_groups_names)
* El score de genes en cada grupo (uns_rank_genes_groups_scores)
* Las veces de cambio de genes en cada grupo (uns_rank_genes_groups_logfoldchanges)
* El p-value de genes en cada grupo (uns_rank_genes_groups_pvals)
* El p-value ajustado de genes en cada grupo (uns_rank_genes_groups_pvals_adj).

Utilizar la herramienta **“Inspect AnnData object”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – t-test). 
* _What to inspect?:_ Unstructured annotation (uns)
    * _What to inspect in uns?:_ Rank gene groups (rank_genes_groups)
* _Configure Output: ‘uns_rank_genes_groups_names’:_
    * _Label:_ 47: Ranking de genes por nombre (t-test).
    * _Rename dataset: _47: Ranking de genes por nombre (t-test).
* _Configure Output: ‘uns_rank_genes_groups_scores’:_
    * _Label:_ 47: Ranking de genes por score (t-test).
    * _Rename dataset: _47: Ranking de genes por score (t-test).
* _Configure Output: uns_rank_genes_groups_ logfoldchanges‘’:_
    * _Label:_ 47: Ranking de genes por expresión (t-test).
    * _Rename dataset: _47: Ranking de genes por expresión (t-test).
* _Configure Output: ‘uns_rank_genes_groups_pvals’:_
    * _Label:_ 47: Ranking de genes por p-value (t-test).
    * _Rename dataset: _47: Ranking de genes por p-value (t-test).
* _Configure Output: ‘uns_rank_genes_groups_pavals_adj’:_
    * _Label:_ 47: Ranking de genes por p-value ajustado (t-test).
    * _Rename dataset: _47: Ranking de genes por p-value ajustado (t-test).


### 48: Gráfico: Genes con mayor influencia en cada grupo de células (Wilcoxon)

Este gráfico coloca a los genes en el eje X identificados con un número, este número representaría únicamente un gen, no es un valor que tomar en cuenta; la abreviación de cada gen se grafica encuentra perpendicular a su ubicación en el eje X, a la altura de su valor en el eje Y. En el eje Y se grafica el score de los genes, el cual representa la influencia de este gen en cada grupo de genes formado en los análisis anteriores.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc).
* _Format for saving figures:_ png
* _Method used for plotting:_ Marker genes: Plot ranking of genes using dotplot plot, using 'pl.rank_genes_groups'
    * _Number of genes to show:_ 20
    * _Font size for gene names:_ 8
    * _Number of panels per row: _3
    * _Should the y-axis of each panels be shared?: _No (colocar esta opción en ‘Yes’ hará que el eje Y tenga el mismo rango de valores entre todos los gráficos)
* _Configure Output: ‘out_png’:_
    * _Label:_ 48: Principales genes en cada grupo (Wilcoxon).
    * _Rename dataset: _48: Principales genes en cada grupo (Wilcoxon).


### 49: Gráfico: Expresión de N marcadores en cada grupo (violín)

Se analiza la expresión de los marcadores más relevantes encontrados, en este caso 8 (Se ajusta la cantidad de filas que representan la expresión de N marcadores en: ‘Method used for plotting/Number of categories: 8’). Para este gráfico se seleccionaron los 8 marcadores utilizando la información entregada en el paso 47, la tabla 47: Ranking de genes por nombre (t-test), seleccionando el primer gen de cada grupo, estos deben estar escirtos en ‘Method used for plotting/Variables to plot/ List of variables to plot: [nombre de los genes separados con coma]’.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
* _Format for saving figures:_ png
* _Method used for plotting: Generic:_ Stacked violin plot, using 'pl.stacked_violin'
    * _Variables to plot (columns of the heatmaps):_ Subset of variables in 'adata.var_names'
        * _List of variables to plot:_ LDHB, LYZ, CD74, CCL5, LST1, NKG7, HLA-DPA1, PF4
    * _The key of the observation grouping to consider:_ louvain
    * _Number of categories:_ 8
    * _Use the log of the values?:_ No
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Compute and plot dendogram?:_ No
    * _Custom figure size:_ Yes
        * _Figure width:_ 10
        * _Figure height:_ 10
    * _Swap axes?:_ Yes
    * _Violin plot attributes:_
        * _Add a stripplot on top of the violin plot:_ Yes (se puede cambiar a ‘No’, esto agrega un gráfico de dispersión, en donde cada punto refleja un valor de expresión de cada célula)
            * _Add jutter to the stripplot:_ Yes
            * _Size of the jitter points:_ 1.0
        * _Method used to scale the width of each violin:_ width: each violin will have the same width
    * _Colors to use in each of the stacked violin plots: _muted
    * _Standardize a dimension between 0 and 1: _No standardization
* _Configure Output: ‘out_png’:_
    * _Label:_ 49: Expresión de N marcadores en cada grupo (violín).
    * _Rename dataset: _49: Expresión de N marcadores en cada grupo (violín).


### 50: Gráfico: Expresión de N marcadores en cada grupo (mapa de vecinos)

Este gráfico es una alternativa complementaria al gráfico generado en el paso 49. Nos permite observar los diferentes grupos de células, incluyendo un gráfico general (louvain) en donde se muestra la expresión de los 8 marcadores con los que trabajamos en el paso 49, además de un gráfico para cada marcador en donde el coloreado de cada punto, representaría la expresión de ese gen en cada célula. Este gráfico es similar al obtenido en el paso 43.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde Archivo UMAP agrupado – wilc). 
* _Format for saving figures:_ png
* _Method used for plotting: _Embeddings: Scatter plot in UMAP basis, using 'pl.umap'
    * _Keys for annotations of observations/cells or variables/genes: _louvain, _LDHB, LYZ, CD74, CCL5, LST1, NKG7, HLA-DPA1, PF4_
    * _Use ‘raw’ attribute of input if present: _Yes
    * _Show edges?: _No
    * _Show arrows?: _No
    * _Sort order?:_ Yes
    * _Plot attributes_
        * _Projection of plot:_ 2d
        * _Location of legend: _right margin
        * _Legend font weight: normal_
        * _Draw a frame around the scatter plot?: _Yes
        * _Number of panels per row:_ 2
        * _Width of the space between multiple panels:_ 0.1
        * _Heigh of the space between multiple panels:_ 0.25
* _Configure Output: ‘out_png’:_
    * _Label:_ 50: Expresión de N marcadores en cada grupo (UMAP).
    * _Rename dataset: _50: Expresión de N marcadores en cada grupo (UMAP).


### 51: Gráfico: Expresión de los top N marcadores en células de cada grupo (heatmap)

N = 20, se puede cambiar en 'Method used for plotting/Number of genes to show: 20'. Muestra el top N para cada grupo de células.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Marker genes: Plot ranking of genes as heatmap plot, using 'pl.rank_genes_groups_heatmap'
    * _Number of genes to show:_ 20
    * _Use the log of the values?: _No
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Compute and plot a dendrogram?:_ Yes
    * _Custom figure size:_ No: the figure width is set based on the number of variable names and the height is set to 10
* _Configure Output: ‘out_png’:_
    * _Label:_ 51: Heatmap top N genes
    * _Rename dataset: _51: Heatmap top N genes.


### 52: Genes con mayor influencia (Wilcoxon)

En este paso se identifican los genes con mayor influencia en cada grupo de genes previamente obtenido mediante el uso de los datos de UMAP. Esta información se entrega como tabla, complementaria al gráfico del paso 48.

En este módulo se pueden seleccionar la información que tendrá el output, en donde podemos escoger:



* Los nombres de genes en cada grupo (uns_rank_genes_groups_names)
* El score de genes en cada grupo (uns_rank_genes_groups_scores)
* Las veces de cambio de genes en cada grupo (uns_rank_genes_groups_logfoldchanges)
* El p-value de genes en cada grupo (uns_rank_genes_groups_pvals)
* El p-value ajustado de genes en cada grupo (uns_rank_genes_groups_pvals_adj).

Utilizar la herramienta **“Inspect AnnData object”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
* _What to inspect?:_ Unstructured annotation (uns)
    * _What to inspect in uns?:_ Rank gene groups (rank_genes_groups)
* _Configure Output: ‘uns_rank_genes_groups_names’:_
    * _Label:_ 52: Ranking de genes por nombre (Wilcoxon).
    * _Rename dataset: _52: Ranking de genes por nombre (Wilcoxon).
* _Configure Output: ‘uns_rank_genes_groups_scores’:_
    * _Label:_ 52: Ranking de genes por score (Wilcoxon).
    * _Rename dataset: _52: Ranking de genes por score (Wilcoxon).
* _Configure Output: uns_rank_genes_groups_ logfoldchanges‘’:_
    * _Label:_ 52: Ranking de genes por expresión (Wilcoxon).
    * _Rename dataset: _52: Ranking de genes por expresión (Wilcoxon).
* _Configure Output: ‘uns_rank_genes_groups_pvals’:_
    * _Label:_ 52: Ranking de genes por p-value (Wilcoxon).
    * _Rename dataset: _52: Ranking de genes por p-value (Wilcoxon).
* _Configure Output: ‘uns_rank_genes_groups_pavals_adj’:_
    * _Label:_ 52: Ranking de genes por p-value ajustado (Wilcoxon).
    * _Rename dataset: _52: Ranking de genes por p-value ajustado (Wilcoxon).


### 53: Comparación de la expresión de marcadores

Se compara la expresión de los tres marcadores anteriormente seleccionados en el paso 37, aquellos que presentaban mayor influencia en los componentes principales para cada grupo de células.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Generic: Violin plot, using 'pl.violin'
    * _Keys for accessing variables:_ Subset of variables in 'adata.var_names' or fields in '.obs'
        * _Keys for accessing variables:_ CST3, NKG7, PPBP
    * _The key of the observation grouping to consider:_ Louvain
    * _Use the log of the values?: _No
    * _Use the ‘raw’ attribute of input if present:_ Yes
* _Configure Output: ‘out_png’:_
    * _Label:_ 53: Comparación de expresión de marcadores entre grupos.
    * _Rename dataset: _53: Comparación de expresión de marcadores entre grupos.


### 54: Identificación de marcadores entre dos grupos

Comparación entre un número determinado de grupos, se pueden seleccionar en 'Subset of groups which comparison shall be restricted:0' (grupo 0), y 'Comparioson/Group identifier with respecto to which compare: 1' (grupo 1).

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
*  _Method used for inspecting: _Rank genes for characterizing groups, using 'tl.rank_genes_groups'
    * _The key of the observations grouping to consider: _louvain
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Subset of groups to which comparison shall be restricted: _0
    * _Comparison: _Compare with respect to a specific group
        * _Group identifier with respect to which compare: _1
    * _The number of genes that appear in the returned tables: _100
    * _Method: _Wilcoxon-Rank-Sum
        * _P-value correction method: _Benjamini-Hochberg
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo UMAP agrupado – wilc – grupo 0 vs 1.
    * _Rename dataset: _Archivo UMAP agrupado – wilc – grupo 0 vs 1.


### 56: Gráfico: Ranking de genes que distinguen al grupo 0 del grupo 1

En este gráfico se da un score a qué tan diferente es la expresión entre el grupo 0 con respecto al grupo 1. En el eje X nuevamente, el valor numérico es sólo un indicativo de la posición del gen en el gráfico y no debe interpretarse.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc – grupo 0 vs 1). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Marker genes: Plot ranking of genes using dotplot plot, using 'pl.rank_genes_groups'
    * _Number of genes to show:_ 20
    * _Should the y-axis of each panels be shared?:_ No
    * _Font size for gene names:_ 8
    * _Number of panels per row: _3
* _Configure Output: ‘out_png’:_
    * _Label:_ 56: Gráfico: Ranking de genes que distinguen al grupo 0 del grupo 1.
    * _Rename dataset: _56: Gráfico: Ranking de genes que distinguen al grupo 0 del grupo 1.


### 57: Gráfico: Diferencia de expresión de genes marcadores que distinguen al grupo 0 del grupo 1

Representación en gráfico de violin que complementa al gráfico paso 56. La cantidad de genes graficadas puede cambiarse en 'Method used for plotting/ Number of genes to show: 10'

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc – grupo 0 vs 1).
* _Format for saving figures:_ png
* _Method used for plotting:_ Marker genes: Plot ranking of genes as violin plot, using 'pl.rank_genes_groups_violin'
    * _Which genes to plot?:_ A number of genes
        * _Number of genes to show:_ 10
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Split the violins?:_ Yes
* _Configure Output: ‘out_png’:_
    * _Label:_ 57: Diferencia de expresión de genes marcadores que distinguen al grupo 0 del grupo 1.
    * _Rename dataset: _57: Diferencia de expresión de genes marcadores que distinguen al grupo 0 del grupo 1.
