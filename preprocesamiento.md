## Preprocesamiento

Antes de hacer los análisis de agrupación de las células los datos deben ser procesados para eliminar la información de baja calidad.

Para este procesamiento de datos se considera:



1. Selección y filtro de células y genes basado en métricas de calidad

    Células de baja calidad podrían alterar los análisis posteriores, por esto deben ser removidas, ya que son consideradas ruido y su baja calidad podría deberse a daños celulares producidos durante el procesamiento de esta.

2. Normalización de datos y escalamiento

    Esto eliminará sesgos célula específicos (como la eficiencia de captura), permitiéndonos realizar comparaciones específicas entre células en los análisis subsiguientes. Algunas transformaciones, comúnmente logarítmicas, también son aplicadas para ajustar la relación promedio-varianza.

3. Selección de características

    Con esto se selecciona un subconjunto de características interesantes para los análisis posteriores. Esto se logra modelando la varianza entre las diferentes células para cada gen, conservando los genes altamente variables entre células. Este paso sirve para reducir el consumo computacional y el ruido que puedan generar genes de poco interés.



### 10: Filtro: Eliminar genes expresados en menos de N células

En este paso se filtrarán los genes expresados en una cantidad mínima de células, dado que esto puede considerarse ruido y debiesen ser removidos.

Utilizar la herramienta “**Filter with scanpy”** con los siguientes ajustes:



* _Data input: _'adata' (h5ad) (no manipulable en Flujo de Trabajo; corresponde al Objeto Anndata).
    * _Method used for filtering:_ Filter genes based on number of cells or counts, using 'pp.filter_genes'
        * _Filter:_ Minimum number of cells expressed
        * _Minimum number of counts required for a cell to pass filtering:_ 3
* _Configure Output: ‘anndata_out’:_
    * _Label: _Archivo filtrado.
    * _Rename dataset: _Archivo filtrado.
* Al revisar el Archivo filtrado, haciendo click en el ícono con forma de ojo en el módulo en la sección de **History **debería mostrar la siguiente información:
    * Cuadro superior (verde, mismo color del fondo):


```
[n_obs × n_vars]
2700 × 13714
[var]
-    gene_ids
-    n_cells

```



    * Cuadro inferior (blanco):


```
[n_obs x n_vars]
    2700 x 13714
[obs]: 1 layer
    _index
[var]: 3 layers
    _index, gene_ids, n_cells
[obsm]: 0 layers
[varm]: 0 layers    
[uns]: 0 layers
```
