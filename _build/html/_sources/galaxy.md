## Introducción al Flujo de Trabajo en Galaxy

Para comenzar a utilizar el Flujo de Trabajo de este tutorial:



* Abrir la pestaña **Flujo de Trabajo** en la interfaz gráfica de Galaxy
* Seleccionar **Importar** 
* En la sección **Archived Workflow File** se debe subir el Flujo de Trabajo **Agrupación utilizando Scanpy.ga**.
* Galaxy volverá a la pestaña **Flujo de Trabajo**.
* Seleccionar el ícono de la columna **Run** en el Flujo de Trabajo **Agrupación utilizando Scanpy (imported from uploaded file)**. Al posar el cursor sobre el ícono aparecerá el mensaje “Ejecutar Flujo de Trabajo”.
* El Flujo de Trabajo solicitará los archivos previamente especificados, el archivo de **Matriz,** **Genes, y Barcodes**. Además, se debe crear un archivo en **Generar header** que será utilizado durante el análisis.
    * Presionar el ícono **Importar**. Al posar el cursor sobre el ícono aparecerá el mensaje “Upload Dataset”.
    * Seleccionar **Paste/Fetch data**.
    * Escribir **“mito” **en el cuadro de texto y seleccionar **tabular** en **Type (set all)**.
    * Pulsar **Start**.
* Crear un nuevo Historial para la ejecución del Flujo de Trabajo:
    *  En la barra de **History** seleccionar ícono **“+”**. Al posar el cursor sobre el ícono aparecerá el mensaje “Crear historial nuevo”.
    * Cambiar el nombre a uno escogido por el usuario para identificar la corrida del Flujo de Trabajo.
* Presionar **Run Workflow**.

Al completar estos pasos el Flujo de Trabajo ya habrá empezado a correr, si los archivos fueron subidos correctamente, el proceso debería terminar sin problemas.


## Detalles de los módulos del Flujo de Trabajo 

Cada módulo cuenta con título y anotación, los cuales corresponden al título y descripción de este tutorial. La anotación consiste en información que contextualiza la función del módulo usado para ayudar al usuario a comprender cada detalle de este Flujo de Trabajo

