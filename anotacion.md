## Anotación de tipos celulares


### 55: Anotación de tipos celulares

En este análisis se utilizará la anotación de tipos celulares que se encuentra en el archivo Anndata (que hemos transformado y agregado variables a lo largo de todo el análisis). Se deben especificar los tipos celulares, estos pueden cambiarse en 'Function to manipulate the object/Comma-separated list of new categories: [especificar tipos celulares separados por coma]'

Utilizar la herramienta **“Mannipulate AnnData object”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado – wilc). 
* _Function to manipulate the object:_ Rename categories of annotation
    * _Key for observations or variables annotation:_ louvain
    * _Comma-separated list of new categories:_ CD4+ T, CD14+, B, CD8+ T, FCGR3A+, NK, Dendritic, Megakaryocytes
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo UMAP agrupado - wilc - anotado.
    * _Rename dataset: _Archivo UMAP agrupado - wilc - anotado.


### 58: Gráfico: Agregar anotación de tipo celular a grupos de células (png)

En este gráfico se espera observar algo similar al UMAP agrupado, pero con código de color para cada tipo celular, además del tipo celular escrito en cada grupo de células.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado - wilc - anotado). 
* _Format for saving figures:_ png
* _Method used for plotting: Embeddings:_ Scatter plot in UMAP basis, using 'pl.umap'
    * _Keys for annotations of observations/cells or variables/genes:_ louvain
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Show edges?:_ No
    * _Show arrows?:_ No
    * _Sort order?: _Yes
    * _Plot attributes_
        * _Projection of plot: _2d
        * _Location of legend: _on data
        * _Legend font weight: _normal
        * _Location of legend:_ on data
        * _Draw a frame around the scatter plot?:_ Yes
        * _Number of panels per row:_ 4
        * _Width of the space between multiple panels:_ 0.1
        * _Height of the space between multiple panels:_ 0.25
* _Configure Output: ‘out_svg’:_
    * _Label:_ 58: Anotación de tipo celular en grupos de células.
    * _Rename dataset: _58: Anotación de tipo celular en grupos de células.


### 59: Gráfico: Agregar anotación de tipo celular a grupos de células (svg)

Mismo gráfico que en el paso 58, pero diferente formato de salida

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado - wilc - anotado). 
* _Format for saving figures:_ svg
* _Method used for plotting: Embeddings:_ Scatter plot in UMAP basis, using 'pl.umap'
    * _Keys for annotations of observations/cells or variables/genes:_ louvain
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Show edges?:_ No
    * _Show arrows?:_ No
    * _Sort order?: _Yes
    * _Plot attributes_
        * _Projection of plot: _2d
        * _Location of legend: _on data
        * _Legend font weight: _normal
        * _Location of legend:_ on data
        * _Draw a frame around the scatter plot?:_ Yes
        * _Number of panels per row:_ 4
        * _Width of the space between multiple panels:_ 0.1
        * _Height of the space between multiple panels:_ 0.25
* _Configure Output: ‘out_png’:_
    * _Label:_ 59: Anotación de tipo celular en grupos de células.
    * _Rename dataset: _59: Anotación de tipo celular en grupos de células.


### 60: Gráfico: Expresión de principales genes para cada tipo celular

En este gráfico tenemos en el eje Y a la izquierda los diferentes tipos celulares anotados, en el eje Y a la derecha tenemos qué tan similares son estos grupos. En el eje X inferior se encuentran los genes que más influyen para la diferenciación entre grupos de células, y en el eje X superior se encuentra nuevamente la anotación de células, esta vez indicando cuales son los genes que más influyen en cada grupo de células anotado.

Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo UMAP agrupado - wilc - anotado). 
* _Format for saving figures:_ png
* _Method used for plotting:_ Generic: Makes a dot plot of the expression values, using 'pl.dotplot'
    * _Variables to plot (columns of the heatmaps):_ Subset of variables in 'adata.var_names'
        * _List of variables to plot:_ IL7R, CCR7, CD8A, CD14, LYZ, MS4A1, CD79A, GNLY, NKG7, KLRB1, FCER1A, CST3, PPBP, FCGR3A
    * _The key of the observation grouping to consider:_ louvain
    * _Number of categories:_ 8
    * _Use the log of the values?:_ No
    * _Use ‘raw’ attribute of input if present:_ Yes
    * _Compute and plot a dendrogram?:_ Yes
    * _Group of variables to highlight_
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _1: Group of variables to highlight_
            * _Start:_ 0
            * _End:_ 1
            * _Label:_ CD4+ T
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _2: Group of variables to highlight_
            * _Start:_ 2
            * _End:_ 2
            * _Label:_ CD8+
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _3: Group of variables to highlight_
            * _Start:_ 3
            * _End:_ 4
            * _Label:_ CD14+
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _4: Group of variables to highlight_
            * _Start:_ 5
            * _End:_ 6
            * _Label:_ B
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _5: Group of variables to highlight_
            * _Start_: 7
            * _End:_ 9
            * _Label:_ NK
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _6: Group of variables to highlight_
            * _Start:_ 10
            * _End:_ 11
            * _Label:_ Dendritic
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _7: Group of variables to highlight_
            * _Start:_ 12
            * _End: _12
            * _Label:_ Megakaryocytes
        * Seleccionar ícono [+ Insert Group of variables to highlight]
        * _8: Group of variables to highlight_
            * _Start:_ 13
            * _End:_ 13
            * _Label:_ FCGR3A+
    * _Custom figure size:_ No: the figure width is set based on the number of variable names and the height is set to 10
    * _Color palette:_ viridis (Perceptually Uniform Sequential)_ _
* _Configure Output: ‘out_png’:_
    * _Label:_ 60: Expresión de marcadores en grupos de células anotados.
    * _Rename dataset: _60: Expresión de marcadores en grupos de células anotados.
