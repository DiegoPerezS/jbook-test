## Selección y filtro de células y genes basado en métricas de calidad

Para estimar los valores límite (threshold) que debemos usar en los análisis posteriores, primero se debe evaluar la distribución de las métricas de control de calidad para nuestro set de datos.

Las primeras dos métricas que debemos analizar son el tamaño de las células y el número de genes expresados, los pueden ser fácilmente estimadas desde la tabla de cuentas. La tercera métrica es la proporción de lecturas (reads) mapeadas a genes en el genoma mitocondrial, y para esto necesitamos la información sobre qué genes pertenecen a la mitocondria o no.

Para comprender mejor los siguientes pasos, es importante tener en cuenta estas definiciones:



    * **Tamaño de célula: **El número total de cuentas asociadas a una célula determinada. Cuando una célula sufre daños importantes o si la secuenciación de esa célula en particular generó pocas lecturas, se consideran células de baja calidad que podrían alterar los resultados finales. Ambos fenómenos se asocian con un bajo número de lecturas asociadas a la célula, por lo que es mejor eliminarlas del análisis.
    * **Número de genes expresados:** Número de genes con cuentas > 0. Un número bajo de genes expresados es otro indicador de daño celular o pérdida de muestra durante el procesamiento de esta. Si el número de genes expresados de una célula es bajo es un indicador de baja calidad para la información de dicha célula y es preferible eliminarla.
    * **Proporción de lecturas mapeadas a genes en el genoma mitocondrial:** Altas concentraciones de genes mitocondriales es un indicador común de células dañadas, en donde el RNA endógeno de la célula no fue captado o fue degradado durante el proceso. El RNA mitocondrial estaría con mayor representación en estas muestras dado a que la mitocondria tiene su propia membrana, la cual protegería al RNA con una barrera física extra a las que ya tendría como protección el RNA celular, pudiendo conservarse incluso cuando la célula haya resultado dañada durante los análisis.

Cabe destacar, que en este flujo de trabajo se han conservado los valores de corte (threshold) del tutorial que se usó como base, por lo que se debe analizar nuevamente para comprobar que estos valores sean adecuados para el set de datos analizado, y de lo contrario, modificarlos para obtener resultados de alta calidad.


### 11: Inspección de control de calidad

Se visualiza la información de la abundancia de células por gen expresado.

Resultado esperado:


```
	gene_ids	n_cells
AL627309.1	ENSG00000237683	9
AP006222.2	ENSG00000228463	3
RP11-206L10.2	ENSG00000228327	5
RP11-206L10.9	ENSG00000237491	3
```


Utilizar la herramienta “**Inspect AnnData object”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo filtrado).
* _What to inspect?:_ Key-indexed annotation of variables/features (var)
* _Configure Output: ‘var’:_
    * _Label: _Archivo – filt - anotado.
    * _Rename dataset: _Archivo – filt - anotado.

Con el archivo resultante se obtiene  la anotación de los genes, que consiste en el _gene_symbol_ y un identificador de la base de datos, en este caso, ENSEMBL. 

Para identificar los genes mitocondriales se utilizará el _gene_symbol_, que también corresponde a la abreviación del nombre del gen, e indica si un gen es mitocondrial utilizando el prefijo “MT-”. Cabe recalcar que esta estrategia se basa en que la anotación del genoma analizado sea de alta calidad.

Para llevar a cabo esto, necesitamos agregar una nueva columna al parámetro “var” del _Archivo – filt – anotado._ La nueva anotación en “var” será una tabla con el mismo número de líneas que el “var” original y tantas columnas como nuevas anotaciones. En este Flujo de trabajo, la tabla comenzó a crearse con la carga de archivos antes de iniciar la corrida, bajo el nombre _Generar header_. Este consiste en una tabla que sólo posee el header de la columna, _mito_. Luego en las filas de esta columna se anotarán los genes que sean mitocondriales o no. Esto lo lograremos llenando la información de cada gen basada en el prefijo del _gene_symbol_, en donde si un gen posee el prefijo “MT-”, el valor de la fila que corresponda con el gen será “True”, y si no es detectado este prefijo, el valor será “False”.


### 12: Anotación de genes mitocondriales: Extracción de genes presentes en el set de datos

Con este paso se obtendrá la anotación de los genes incluyendo _gene_symbol_, identificador del gen de la base de datos ENSEMBL (determinado por el archivo inicial **Genes**), y el número de células que expresan cada gen, este último dato no es relevante para este paso, pero es parte de la anotación extraída.

Utilizar la herramienta “**Select last lines from dataset (tail)”** con los siguientes ajustes:



* _Data input: _'infile' (txt) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt - anotado).
* _Operation:_ Keep everything from this line on
* _Number of lines:_ 2
* _Configure Output: ‘outfile’:_
    * _Label: _Anotación de genes (base).
    * _Rename dataset: _Anotación de genes (base).


### 13: Chequeo de formato de anotación de genes

El _gene_symbol _o nombre de los genes mitocondriales comienzan con un 'MT-', lo que nos ayudará a identificarlos. En este control de datos se revisa que esta nomenclatura esté presente en nuestro set de datos. 

Utilizar la herramienta “**Search in textfiles (grep)”** con los siguientes ajustes:



* _Data input: _'infile' (txt) (no manipulable en Flujo de Trabajo; corresponde al Anotación de genes (base).
* _that:_ Match
* _Type of regex:_ Perl
* _Regular Expression:_ ^mt
* _Match type:_ case insensitive
* _Output:_ Highlighted HTML (for easier viewing)
* _Configure Output: ‘output’:_
    * _Label:_ Chequeo: match genes mitocondriales.
    * _Rename dataset: _Chequeo: match genes mitocondriales.
* Al revisar el resultado, haciendo click en el ícono con forma de ojo en el módulo en la sección de **History **debería mostrar la siguiente información en el panel central:


```
MTOR	ENSG00000198793	35
MTHFR	ENSG00000177000	60
MTFR1L	ENSG00000117640	136
MTF1	ENSG00000188786	66
MTF2	ENSG00000143033	225
MTMR11	ENSG00000014914	135
MTX1	ENSG00000173171	136
MTR	ENSG00000116984	44
MTA3	ENSG00000057935	36
MTIF2	ENSG00000085760	68
```



### 14: Formatear texto para detectar genes mitocondriales

Uso de AWK para generar columna con valores binarios (1 o 0) dependiendo de si el nombre del gen tiene el prefijo 'MT-' o no, respectivamente.

Utilizar la herramienta “**Text reformatting with awk”** con los siguientes ajustes:



* _Data input: _'infile' (txt) (no manipulable en Flujo de Trabajo; corresponde al Anotación de genes (base)).
* _AWK Program:_ {print $1 ~ /^MT-/}
* _Configure Output: ‘outfile’:_
    * _Label:_ Chequeo: Anotación mitocondrial (sin procesar).
    * _Rename dataset: _Anotación mitocondrial (sin procesar).


### 15: Cambio de formato de binario a V/F

Utilizando reconocimiento de patrones se cambiarán los valores binarios de 1 o 0 asignados previamente a Verdadero o Falso respectivamente.

Utilizar la herramienta “**Replace Text in entire line”** con los siguientes ajustes:



* _Data input: _'infile' (txt) (no manipulable en Flujo de Trabajo; corresponde al Anotación mitocondrial (sin procesar)).
* _1: Replacement_
    * _Find pattern:_ 0
    * _Replace with:_ False
* _2: Replacement_
    * _Find pattern: _1
    * _Replace with:_ True
* _Configure Output: ‘outfile’:_
    * _Label:_ Chequeo: Anotación mitocondrial (sin header).
    * _Rename dataset: _Anotación mitocondrial (sin header).


### 16: Concatenar header y clasificación de genes mitocondriales

Se asignará el header subido al inicio del Flujo de Trabajo en **Generar header** a la anotación mitocondrial obtenida en el paso 15.

Utilizar la herramienta “**Concatenate datasets tail-to-head”** con los siguientes ajustes:



* _Data input: _'input1' (data) (no manipulable en Flujo de Trabajo; corresponde al Anotación mitocondrial (sin header) y  **Generar header**).
* _Configure Output: ‘out_file1’:_
    * _Label:_ Chequeo: Anotación mitocondrial.
    * _Rename dataset: _Anotación mitocondrial.


### 17: Añadir anotación mitocondrial a Archivo filtrado

Se agregará la anotación mitocondrial como columna de variante al Archivo filtrado.

Utilizar la herramienta “**Manipulate Anndata object”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo filtrado).
* _Function to manipulate the objetct:_ Add new annotation(s) for observations or variables
* _What to annotate?_: Variables (var)
* _Table with new annotations:_ Data input ‘new_annot’ (tabular) (no manipulable en Flujo de Trabajo; corresponde a la Anotación mitocondrial). 
* _Configure Output: ‘anndata’:_
    * _Label:_ Chequeo: Archivo – filt - mito.
    * _Rename dataset: _Archivo – filt -mito.


### 18: Inspeccionar integración de la anotación mitocondrial

Resultado esperado:


<table>
  <tr>
   <td><code>Column 1</code>
   </td>
   <td><code>Column 2</code>
   </td>
   <td><code>Column 3</code>
   </td>
   <td><code>Column 4</code>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td><code>gene_ids</code>
   </td>
   <td><code>n_cells</code>
   </td>
   <td><code>mito</code>
   </td>
  </tr>
  <tr>
   <td><code>AL627309.1</code>
   </td>
   <td><code>ENSG00000237683</code>
   </td>
   <td><code>9</code>
   </td>
   <td><code>False</code>
   </td>
  </tr>
  <tr>
   <td><code>AP006222.2</code>
   </td>
   <td><code>ENSG00000228463</code>
   </td>
   <td><code>3</code>
   </td>
   <td><code>False</code>
   </td>
  </tr>
  <tr>
   <td><code>RP11-206L10.2</code>
   </td>
   <td><code>ENSG00000228327</code>
   </td>
   <td><code>5</code>
   </td>
   <td><code>False</code>
   </td>
  </tr>
  <tr>
   <td><code>RP11-206L10.9</code>
   </td>
   <td><code>ENSG00000237491</code>
   </td>
   <td><code>3</code>
   </td>
   <td><code>False</code>
   </td>
  </tr>
  <tr>
   <td><code>LINC00115</code>
   </td>
   <td><code>ENSG00000225880</code>
   </td>
   <td><code>18</code>
   </td>
   <td><code>False</code>
   </td>
  </tr>
</table>


Utilizar la herramienta “**Inspect Anndata object”** con los siguientes ajustes:



* _Data input: _'input1' (data) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt - mito).
* _What to inspect?_: Key-indexed annotation of variables/features (var)
* _Configure Output: ‘var’:_
    * _Label:_ Chequeo: Integración de anotación mitocondrial.
    * _Rename dataset: _Integración de anotación mitocondrial.


### 19: Cómputo de métricas de control de calidad (QC)

*quality control = QC

Utilizar la herramienta “**Inspect and manipulate with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt - mito).
* _Method used for inspecting: _ Calculate quality control metrics, using 'pp.calculate_qc_metrics'.
    * _Name of kind of values in X:_ counts
    * _The kind of thing the variables are:_ genes
    * _Keys for boolean columns of '.var' which identify variables you could want to control for: _mito
* _Configure Output: ‘anndata_out’:_
    * _Label:_ Chequeo: Archivo – filt – mito - QC.
    * _Rename dataset: _Archivo – filt – mito - QC.


### 20: Gráfico: Métricas generales de QC

Este módulo generará tres gráficos que indican diferentes métricas de calidad:



* El panel 'n_genes_by_counts' indica el número de genes expresados por célula.
* El panel 'total_counts' indica el número de cuentas por célula (tamaño de célula).
* El panel 'pct_counts_mito' indica el porcentaje de genes mitocondriales expresados en cada célula

Utilizar la herramienta “**Plot with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt – mito - QC).
* _Format for saving figures:_ png
* _Method used for plotting:_ Generic: Violin plot, using 'pl.violin'
    * _Keys for accessing variables:_  	Subset of variables in 'adata.var_names' or fields of '.obs'
        * _Keys for accessing variables:_ total_counts, n_genes_by_counts, pct_counts_mito
* _Violin plot attributes_
    * _Display keys in multiple panels: _Yes **(sólo aplica para cuándo no se está trabajando en el Flujo de Trabajo).**
* _Configure Output: ‘out_png’:_
    * _Label:_ Chequeo: Métricas generales de QC.
    * _Rename dataset: _Métricas generales de QC.


### 21: Gráfico: Número de genes expresados y la proporción de genes mitocondriales

Indica la abundancia de genes expresados (eje x) y la proporción (%) de genes mitocondriales entre dichos genes (eje y). Este parámetro sirve para evaluar cuántos genes expresados se considerarán para los siguientes análisis, seleccionando un threshold de al menos N genes expresados. N se selecciona en función de la proporción de genes mitocondriales expresados, por ejemplo, las poblaciones de células que expresan 2000 genes y poseen en promedio un 5% de genes mitocondriales, podrían considerarse ruido por dos razones, poca cantidad de genes expresados, y un alto porcentaje de genes mitocondriales expresados.

Utilizar la herramienta “**Plot with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt – mito - QC).
* _Format for saving figures:_ png
* _Method used for plotting:_ Generic: Scatter plot along observations or variables axes, using 'pl.scatter'
    * _Plotting tool that computed coordinates: _Using coordinates
        * _x coordinate: _total_counts
        * _y coordinate: _pct_counts_mito
        * _Use the layers attribute?: No_
* _Configure Output: ‘out_png’:_
    * _Label:_ Chequeo: Genes expresados vs porcentaje de genes mitocondriales.
    * _Rename dataset: _Genes expresados vs porcentaje de genes mitocondriales.


### 22: Gráfico: Número de genes expresados por célula y total de genes expresados

Relación entre el número total de cuentas para una célula (eje x), y el número de genes con cuentas positivas (eje y).

Este gráfico indica si existe una correlación entre las lecturas de cada célula y la cantidad de genes expresados encontrados en dichas lecturas.

Utilizar la herramienta “**Plot with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt – mito - QC).
* _Format for saving figures:_ png
* _Method used for plotting:_ Generic: Scatter plot along observations or variables axes, using 'pl.scatter'
    * _Plotting tool that computed coordinates: _Using coordinates
        * _x coordinate: _total_counts
        * _y coordinate: _n_genes_by_counts
        * _Use the layers attribute?: No_
* _Configure Output: ‘out_png’:_
    * _Label:_ Chequeo: Genes expresados por célula vs total de genes expresados.
    * _Rename dataset: _Genes expresados por célula vs total de genes expresados.


### 23: Filtro: Eliminar células con pocos genes expresados

*A contar de este paso, el output no se ocupará la opción _Configure Output_ hasta el paso 25, ya que se aplicarán varios filtros sucesivos. Cabe destacar que en el Flujo de Trabajo, estos filtros son secuenciales, utilizando como input el output del filtro anterior, y en el caso de no usar el Flujo de Trabajo y trabajar en Galaxy únicamente con la interfaz de módulos, se debe seleccionar el input manualmente, teniendo cuidado de no perder el rastro del input y output correctos.

Basado en los gráficos 20-22, se proceden a escoger los valores de los threshold para filtrar células de baja calidad. Dado a los resultados en el gráfico 20, panel 'n_genes_by_counts', se establece que no se considerarán las células que posean menos de 200 genes expresados.

Utilizar la herramienta **“Filter with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – filt – mito - QC).
* _Method used for filtering:_ Filter cell outliers based on counts and numbers of genes expressed, using 'pp.filter_cells'
    * _Filter: _Minimum number of genes expressed
    * _Minimum number of genes expressed required for a cell to pass filtering: _200


### 24: Filtro: Eliminar células con demasiados genes expresados

Dado a los resultados en el gráfico 20, panel 'n_genes_by_counts', se establece que no se considerarán las células que posean menos de 2,500 genes expresados.

Utilizar la herramienta **“Filter with Scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al output del paso 23).
* _Method used for filtering:_ Filter cell outliers based on counts and numbers of genes expressed, using 'pp.filter_cells'
    * _Filter: _Maximum number of genes expressed
    * _Minimum number of genes expressed required for a cell to pass filtering: _2500


### 25: Filtro: Eliminar células con gran porcentaje de expresión de genes mitocondriales

Dado los resultados en el gráfico 20 ‘pct_counts_mito’, se establece que no se considerarán células con más de un 5% de genes mitocondriales expresados con respecto al total de genes expresados de la misma célula.

Utilizar la herramienta **“Manipulate AnnData object”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al output del paso 24).
* _Function to manipulate the object:_ Filter observations or variables
    * _What to filter:_ Observations (obs)
        * _Type of filtering?:_ By key (column) values
        * _Key to filter:_ pct_counts_mito
            * _Type of value to filter_: Number
            * _Filter:_ less than
            * _Value:_ 5.0
* _Configure Output: ‘Anndata’:_
    * _Label:_ Archivo – QC_filt.
    * _Rename dataset: _Archivo – QC_filt.


### 26: Normalización

Los datos deben ser normalizados para que cada célula de la muestra sea comparable entre sí, eliminando la variabilidad del set de datos producto de diferencias durante la síntesis de cDNA y amplificación por PCR.

El método de normalización empleado en este pipeline es llamado Scaling, el cual asume que todos los sesgos que pudiesen haberse generado en los experimentos se aplican de forma similar a todas las células. Con este método las cuentas de genes de cada célula son divididas por un factor de escalado (scaling) específico para cada célula, llamado factor de tamaño (size factor). El factor de tamaño de cada célula es computado directamente desde el tamaño de la célula (cantidad de reads asignados a dicha célula), y es transformado para que el tamaño promedio del factor entre todas las células sea igual a 1. 

Con este método, los valores de expresión obtenidos mantendrían la proporción de expresión de genes, generando un set de datos que permite comparar a cada célula entre sí.

Utilizar la herramienta **“Normalize with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC_filt).
* _Method used for normalization:_ Normalize counts per cell, using 'pp.normalize_total'
    * _Target sum:_ 10000.0
    * _Exclude (very) highly expressed genes for the computation of the normalization factor (size factor) for each cell:_ No
    * _Name of the field in ‘adata.obs’_ where the normalization factor is stored: norm
    * _List of layers to normalize: _all
    * _How to normalize layers?:_ After: for each layer in layers each cell has a total count equal to target_sum.
* _Configure Output: ‘Anndata’:_
    * _Label:_ Archivo – QC_filt – normalizado.
    * _Rename dataset: _Archivo – QC_filt – normalizado.


### 27: Transformación logaritmica

Esta transformación a los valores normalizados los hace análogos a un log(veces de cambio; 'log(FoldChange)').

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC_filt – normalizado).
* _Method used for inspecting:_ Logarithmize the data matrix, using 'pp.log1p'
* _Configure Output: ‘Anndata’:_
    * _Label:_ Archivo – QC_filt – norm - FC.
    * _Rename dataset: _Archivo – QC_filt – norm - FC.


### 28: Bloquear estado del archivo Anndata

En este paso se bloqueará el archivo que hemos trabajado hasta el paso previo, el cual está en formato Anndata, para usarlo en análisis posteriores.

Utilizar la herramienta **“Inspect and manipulate with scanpy”** con los siguientes ajustes:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Archivo – QC_filt – norm - FC).
* _Method used for inspecting:_ Logarithmize the data matrix, using 'pp.log1p'
* _Configure Output: ‘Anndata’:_
    * _Label:_ Archivo – QC – norm.
    * _Rename dataset: _Archivo – QC – norm.
