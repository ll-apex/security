# Detectar datos confidenciales

## Introducción

Detección de datos le ayuda a buscar datos confidenciales en las bases de datos de destino. Se indica a Detección de datos qué tipo de datos confidenciales buscar e inspecciona los datos reales en la base de datos de destino y su diccionario de datos y, a continuación, le devuelve una lista de columnas confidenciales. De forma predeterminada, Detección de datos puede buscar una amplia variedad de datos confidenciales relacionados con información de identificación, biográfica, informática, financiera, sanitaria, laboral y académica.

Utilice Oracle Data Safe para detectar datos confidenciales en la base de datos de destino y, a continuación, ajuste el modelo de datos confidenciales.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Detección de datos confidenciales en la base de datos de destino mediante la detección de datos
*   Ajustar el modelo de datos confidenciales

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller
*   Registró la base de datos de destino con Oracle Data Safe

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.

Vea el siguiente vídeo para una breve introducción al laboratorio. [Detección de datos confidenciales](videohub:1_d0fz5ep5)

## Tarea 1: Detección de datos confidenciales en la base de datos de destino mediante Data Discovery

1.  Asegúrese de que está en el separador del explorador de Oracle Data Safe. Si es necesario, vuelve a iniciar sesión.
    
2.  En la ruta de navegación de la parte superior de la página, haga clic en **Data Safe**.
    
3.  A la izquierda, en **Security Center**, y haga clic en **Data Discovery**.
    
4.  En la lista desplegable **compartimento**, seleccione el compartimento.
    
    Se muestra una página de visión general de Detección de datos con estadísticas para las cinco bases de datos de destino principales del compartimento. Lo más probable es que la página esté vacía porque es la primera vez que utiliza Detección de datos en este taller.
    
5.  Haga clic en **Discover Sensitive Data**.
    
    Se muestra la página **Crear modelo de datos confidenciales**.
    
6.  En la página **Proporcionar información básica**, haga lo siguiente y, a continuación, haga clic en **Siguiente**.
    
    *   En el cuadro **Nombre**, introduzca **SDM1**.
    *   Deje el juego de compartimentos en el compartimento.
    *   En el cuadro **Descripción**, introduzca **Modelo de datos confidenciales 1**.
    *   Seleccione la base de datos de destino
    
    ![Información Básica, página](images/ocw/provide-basic-information-page.png "Información Básica, página")
    
7.  En la página **Seleccionar esquemas**, deje seleccionada la opción **Seleccionar solo esquemas específicos**. Desplácese hacia abajo y seleccione el esquema **HCM1** y, a continuación, haga clic en **Siguiente**. Puede que tenga que hacer clic en el botón de flecha derecha en la parte inferior de la página para desplazarse a la página 2.
    
    ![Página Seleccionar Esquemas](images/ocw/select-schemas-page.png "Página Seleccionar Esquemas")
    
8.  En la página **Seleccionar tipos confidenciales**, amplíe todas las categorías confidenciales moviendo el control deslizante **Ampliar todo** a la derecha. Desplácese por la página y revise los tipos confidenciales. Observe que puede seleccionar tipos confidenciales individuales, categorías confidenciales y todos los tipos confidenciales a la vez. En la parte superior de la página, active la casilla de control **Todo** y, a continuación, haga clic en **Siguiente**.
    
    ![Selección de Tipos Sensibles, página](images/ocw/select-sensitive-types-page.png "Selección de Tipos Sensibles, página")
    
9.  En la página **Seleccionar opciones de detección**, seleccione **Recopilar, mostrar y almacenar datos de ejemplo** y, a continuación, haga clic en **Crear modelo de datos confidenciales** en la parte inferior de la página para iniciar el proceso de detección de datos.
    
    ![Selección de Opciones de Detección, página](images/select-discovery-options-page.png "Página Seleccionar Opciones de Detección")
    
10.  Espere a que se cree el modelo de datos confidenciales.
    
    Se muestra la página **Detalles del modelo de datos confidenciales**.
    
11.  Revise la información de la página **Detalles de modelo de datos confidenciales**.
    
    *   El separador **Información de modelo de datos confidenciales** muestra información sobre el modelo de datos confidenciales, incluido su nombre y el identificador de Oracle Cloud (OCID), el compartimento en el que lo ha guardado, la fecha y la hora en que se ha creado y actualizado por última vez, la base de datos de destino asociada a ella, el esquema seleccionado para la detección (HCM1) y los totales para los esquemas confidenciales detectados, las tablas confidenciales, las columnas confidenciales, los tipos confidenciales y los valores confidenciales.
    *   Puede ver los tipos confidenciales seleccionados para la detección (haga clic en **Ver detalles**).
    *   Puede ver la información de la solicitud de trabajo (haga clic en **Ver detalles**).
    *   El gráfico circular compara el número de valores confidenciales por categoría confidencial y tipo confidencial.
    *   La tabla **Columnas confidenciales** muestra las columnas confidenciales detectadas. De forma predeterminada, la tabla se muestra con el formato **Vista plana**. Para cada columna confidencial, puede ver el nombre de esquema, el nombre de tabla, el nombre de columna, el tipo confidencial, la columna principal, el tipo de dato, el recuento de filas estimado y los datos de muestra (si ha elegido recuperar datos de muestra y si existen). Revise los datos de muestra para tener una idea de cómo se ve.
    
    ![Parte superior de la página Detalles de modelo de datos confidenciales](images/ocw/sensitive-data-model-details-page-1.png "Parte superior de la página Detalles de modelo de datos confidenciales") ![Parte inferior de la página Detalles de modelo de datos confidenciales](images/ocw/sensitive-data-model-details-page-2.png "Parte inferior de la página Detalles de modelo de datos confidenciales")
    

## Tarea 2: Ajuste del modelo de datos confidenciales

Elimine la columna `DATE_OF_HIRE` del modelo de datos confidenciales.

1.  En la sección **Columnas confidenciales**, haga clic en **Eliminar columnas**.
    
    Se muestra el panel **Eliminar Columnas**.
    
2.  En el cuadro **NOMBRE DE COLUMNO**, introduzca **DATE** y, a continuación, seleccione **DATE\_OF\_HIRE**.
    
3.  Haga clic en **Buscar**.
    
4.  Seleccione la casilla de control de la columna **DATE\_OF\_HIRE** en la tabla **JOB\_HISTORY** y, a continuación, haga clic en **REMOVE COLUMNS**.
    
    ![Panel Eliminar columnas](images/ocw/remove-columns-panel.png "Panel Eliminar columnas")
    

## Más información

*   [Identificación de datos](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-discovery.html)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 17 de agosto de 2023