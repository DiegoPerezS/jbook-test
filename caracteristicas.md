## Selección de características

Luego de aplicar los filtros basados en calidad de células y expresión de genes, ahora debemos seleccionar grupos de genes cuya expresión esté asociada a ruido, o a información biológica importante. Este paso nos ayuda a reducir la cantidad de información a analizar, además de quitar aquellos genes cuya expresión sea parte del ruido de nuestro set de datos, ayudándonos a identificar con mayor claridad a los genes cuya expresión pueda atribuirse a señales biológicas de interés. 

La forma más simple para la seleccionar las características, es seleccionar los genes con mayor variación de expresión entre células (en algunos grupos la expresión es alta, mientras que en otros grupos la expresión es baja). De esta forma, se asume que una alta variación en algunos genes comparados con otros refleja diferencias biológicas y no estarían asociados a ruido o nivel base de variación biológica “poco interesante”.

Una de las formas más simples de cuantificar la variación por gen consiste en computar la varianza de los valores de expresión logarítmica normalizada para cada gen, entre todas las células de una población. Así, la selección de características sería consistente con la forma de agrupación, utilizando en ambos casos la expresión logarítmica normalizada de los genes.

El cálculo de la varianza por gen es simple, pero la selección de características requiere un modelamiento de la relación promedio-varianza, en donde se usará el procedimiento de Seurat.

Una vez que se hayan realizado los cálculos, se debe seleccionar el subconjunto de genes altamente variables que serán utilizados en análisis posteriores. Un número alto en el subconjunto de genes reduce el riesgo de perder cualquier gen de interés biológico, pero a costa del riesgo de incrementar el ruido de genes irrelevantes en el contexto biológico. Calcular el término medio entre no perder genes relevantes e incluir ruido a nuestro subconjunto de datos es difícil de determinar, pero hay estrategias que se utilizan comúnmente para ayudarnos en este punto. Se puede dirigir al [Capítulo 8 de “Orchestreating Single-Cell Analysis with Bioconductor”](https://bioconductor.org/books/release/OSCA/) para una buena representación de las diferentes estrategias. En este tutorial, definiremos el conjunto de genes altamente variables como aquellos que luego de la normalización, posean una dispersión normalizada mayor a 0.5.


### 29: Filtro: Genes altamente variables

Selección de genes con mayor variabilidad entre todas las células. Esto no sirve para extraer la población de genes que es distintiva para cada célula dentro de todo el análisis. Esto nos permitirá identificar qué genes serían los que nos sirvan para anotar la función de un grupo de células que posean patrones de expresión génicas similares, por ejemplo, diferenciar macrófagos de células B. 

Utilizar la herramienta **“Filter with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm).
* _ Method used for filtering : _Annotate (and filter) highly variable genes, using 'pp.highly_variable_genes'
    * _ Flavor for computing normalized dispersion : _seurat
    * _ Minimal mean cutoff : _0.0125
    * _ Maximal mean cutoff : _3
    * _ Minimal normalized dispersion cutoff : _0.5
* _Number of bins for binning the mean gene expression: _20
* _ Inplace subset to highly-variable genes? : _No
* _Configure Output: ‘Anndata’:_
    * _Label:_ Archivo – QC – norm – filtro 1.
    * _Rename dataset: _Archivo – QC – norm – filtro 1.


### 30: Gráfico: Dispersión de genes vs promedio de expresión de genes

Este paso sirve para evaluar la dispersión de la expresión de genes en el set de datos luego del cálculo anterior. Se generan dos gráficos, uno utiliza el valor de la dispersión de genes normalizados, y otro utiliza el valor de la dispersión de genes sin normalizar. En este gráfico, los genes altamente variables estarán coloreados de negro y los otros genes de gris.

 Utilizar la herramienta **“Plot with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – filtro 1).
* _Format for saving figures:_ png
* _Method used for plotting:_ Preprocessing: Plot dispersions versus means for genes, using 'pl.highly_variable_genes'
    * _Use the log of the values?_: No
    * _Plot highly variable genes or genes subset?_: Yes
* _Configure Output: ‘out_png’:_
    * _Label:_ 30 - Varianza de genes.
    * _Rename dataset: _30 - Varianza de genes.


### 31: Filtro: Eliminar genes con poca variabilidad entre células

Utilizar la herramienta **“Manipulate AnnData objetct”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – filtro 1). 
* _Function to manipulate the object:_ Filter observations or variables
    * _What to filter?:_ Variables (var)
    * _Type of filtering?:_ By key (column) values
        * _Key to filter:_ highly_variable
        * _Type of value to filter:_ Boolean
        * _Value to keep:_ Yes
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo – QC – norm – filtro 2.
    * _Rename dataset: _Archivo – QC – norm – filtro 2.


### 32: Filtro: Remover fuentes de variación indeseables

En este paso se aplican regresiones lineales o escalamiento para remover genes que representan una variabilidad indeseable, que no sería atribuible al tipo celular, si no a eventos que ocurren en la célula, como artefactos técnicos, ciclo celular, u otros factores similares.

Utilizar la herramienta **“Remove cofounders with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – filtro 2). 
* _Method used for plotting:_ Regress out unwanted sources of variation, using 'pp.regress_out'
    * _Keys for observation annotation on which to regress on: _total_counts, pct_counts_mito
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo – QC – norm – filtro 3.
    * _Rename dataset: _Archivo – QC – norm – filtro 3.


### 33: Escalado de datos

En este paso los valores de expresión se escalan a unidad de varianza, como ejemplo, la unidad de varianza entre células tiene un valor = 1, y un promedio de expresión con valor = 0. Esto otorga un peso equitativo a la expresión génica en los siguientes análisis y asegura que los genes con un valor de expresión muy alto no dominen el set de datos.

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC – norm – filtro 3). 
* _Method used for inspecting:_ Scale data to unit variance and zero mean, using 'pp.scale'
    * _Zero center?: _ Yes
    * _Maximum value:_ 10.0
* _Configure Output: ‘out_png’:_
    * _Label:_ Archivo – QC – norm – escalado.
    * _Rename dataset: _Archivo – QC – norm – escalado.
