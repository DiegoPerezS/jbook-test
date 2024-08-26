## Creación de objeto AnnData


### 5: Transformación de set de datos a objeto AnnData

El objeto AnnData nos permite colapsar toda la información de los sets de datos de **Matriz, Genes, y Barcodes** en un solo objeto, el cual es requerido por Scanpy, y además permite una mejor manipulación de la información.

Utilizar la herramienta para importar **“Import Anndata and loom from different format”** y llenar las casillas de la siguiente forma:



* _Hd5 format to be created_: Anndata file.
    * _Format for the annotated data matrix:_ Matrix Market (mtx), from Cell ranger or not.
    * _Matrix:_ Seleccionar matriz de cuentas importada.
        * _Use 10x Genomics formatted mtx:_ Output from Cell Ranger v2 or earlier versions.
            * _Genes:_ Seleccionar información de los genes importada (no manipulable en Flujo de Trabajo).
            * _Barcodes:_ Seleccionar códigos de barra de cada célula (no manipulable en Flujo de Trabajo).
            * _Variables index_: gene_symbols.
            * _Make the variable index unique by appending ‘-1’, ‘-2’?:_ Yes.
            * _Keep only 'Gene Expression' data and ignore other feature types?: _Yes
* _Configure Output: ‘anndata’:_
    * _Label: _Objeto Anndata.
    * _Rename dataset: _Objeto Anndata.

Ya que el objeto Anndata es una extensión del formato HDF5, el cual es binario, el objeto no puede ser inspeccionado directamente. Por esto, es que necesita ser inspeccionado a través de una herramienta en Galaxy.




### 6: Inspección general del archivo

Resultado esperado:


```
AnnData object with n_obs × n_vars = 2700 × 32738
 var: 'gene_ids'
```


Utilizar la herramienta **“Inspect Anndata object”** y llenar las casillas de la siguiente forma:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Objeto Anndata).
* _What to inspect:_ General information about the object.
* _Configure Output: ‘general’:_
    * _Label: _Información general de Anndata.
    * _Rename dataset: _Información general de Anndata.


### 7: Inspección de matriz de datos

Resultado esperado:


```
MIR1302-10	FAM138A	OR4F5	RP11-34P13.7	RP11-34P13.8 ...
AAACATACAACCAC-1	0.0	0.0	0.0	0.0
AAACATTGAGCTAC-1	0.0	0.0	0.0	0.0
AAACATTGATCAGC-1	0.0	0.0	0.0	0.0
AAACCGTGCTTCCG-1	0.0	0.0	0.0	0.0
AAACCGTGTATGCG-1	0.0	0.0	0.0	0.0
```


Utilizar la herramienta **“Inspect Anndata object”** y llenar las casillas de la siguiente forma:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Objeto Anndata).
* _What to inspect:_ The full data matrix
* _Configure Output: ‘X’:_
    * _Label: _Matriz de datos.
    * _Rename dataset: _Matriz de datos.


### 8: Inspección de índice de cada célula

Asociado al código de barra.

Resultado esperado:


```
""
AAACATACAACCAC-1
AAACATTGAGCTAC-1
AAACATTGATCAGC-1
AAACCGTGCTTCCG-1
AAACCGTGTATGCG-1
```


Utilizar la herramienta **“Inspect Anndata object”** y llenar las casillas de la siguiente forma:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Objeto Anndata).
* _What to inspect:_ Key-indexed observations annotation (obs).
* _Configure Output: ‘obs’:_
    * _Label: _Indice de cada célula.
    * _Rename dataset: _Indice de cada célula.


### 9: Inspección de genes

Extrae identificadores de genes.

Resultado esperado:


```
gene_ids
MIR1302-10	ENSG00000243485
FAM138A	ENSG00000237613
OR4F5		ENSG00000186092
RP11-34P13.7	ENSG00000238009
RP11-34P13.8	ENSG00000239945
```


Utilizar la herramienta **“Inspect Anndata object”** y llenar las casillas de la siguiente forma:



* _Data input: _'input' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Objeto Anndata).
* _What to inspect:_ Key-indexed annotation of variables/features (var).
* _Configure Output: ‘var’:_
    * _Label: _Inspección de genes.
    * _Rename dataset: _Inspección de genes.

