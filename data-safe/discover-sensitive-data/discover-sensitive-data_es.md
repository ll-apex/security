# Detectar datos confidenciales

## Introducción

Detección de datos le ayuda a buscar datos confidenciales en las bases de datos de destino. Se indica a Detección de datos qué tipo de datos confidenciales buscar e inspecciona los datos reales en la base de datos de destino y su diccionario de datos y, a continuación, le devuelve una lista de columnas confidenciales. De forma predeterminada, Detección de datos puede buscar una amplia variedad de datos confidenciales relacionados con información de identificación, biográfica, informática, financiera, sanitaria, laboral y académica.

Para empezar, examine los datos confidenciales en una de las tablas de la base de datos de destino mediante Database Actions. A continuación, utilice Oracle Data Safe para detectar datos confidenciales en la base de datos de destino y generar un modelo de datos confidenciales. Cree un PDF del modelo de datos confidenciales.

Tiempo de laboratorio estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Detección de datos confidenciales en la base de datos de destino mediante la detección de datos
*   Analizar el modelo de datos confidenciales
*   Creación de un PDF del informe Sensitive Data Model

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

### Supuestos

*   Los valores de datos pueden ser diferentes de los que se muestran en las capturas de pantalla.

## Tarea 1: Detección de datos confidenciales en la base de datos de destino mediante Data Discovery

1.  Asegúrese de que está en el separador del explorador de Oracle Data Safe. Si es necesario, vuelve a iniciar sesión.
    
2.  En la ruta de navegación de la parte superior de la página, haga clic en **Data Safe**.
    
3.  A la izquierda, en **Security Center**, y haga clic en **Data Discovery**.
    
4.  En la lista desplegable **compartimento**, seleccione el compartimento.
    
    Se muestra una página de visión general de Detección de datos con estadísticas para las cinco bases de datos de destino principales del compartimento. Lo más probable es que la página esté vacía porque es la primera vez que utiliza Detección de datos en este taller.
    
5.  Haga clic en **Discover Sensitive Data**.
    
    Se muestra el asistente **Crear modelo de datos confidenciales**.
    
6.  En la página **Proporcionar información básica**, haga lo siguiente y, a continuación, haga clic en **Siguiente**.
    
    *   En el cuadro **Nombre**, introduzca **SDM1**.
    *   Deje el juego de compartimentos en el compartimento.
    *   En el cuadro **Descripción**, introduzca **Modelo de datos confidenciales 1**.
    *   Seleccione la base de datos de destino
    
    ![Información Básica, página](images/provide-basic-information-page.png "Información Básica, página")
    
7.  En la página **Seleccionar esquemas**, deje seleccionada la opción **Seleccionar solo esquemas específicos**. Desplácese hacia abajo y seleccione el esquema **HCM1** y, a continuación, haga clic en **Siguiente**. Puede que tenga que hacer clic en el botón de flecha derecha en la parte inferior de la página para desplazarse a la página 2.
    
    ![Página Seleccionar Esquemas](images/select-schemas-page.png "Página Seleccionar Esquemas")
    
8.  En la página **Seleccionar tipos confidenciales**, amplíe todas las categorías confidenciales moviendo el control deslizante **Ampliar todo** a la derecha. Desplácese por la página y revise los tipos confidenciales. Observe que puede seleccionar tipos confidenciales individuales, categorías confidenciales y todos los tipos confidenciales a la vez. En la parte superior de la página, active la casilla de control **Todo** y, a continuación, haga clic en **Siguiente**.
    
    ![Selección de Tipos Sensibles, página](images/select-sensitive-types-page.png "Selección de Tipos Sensibles, página")
    
9.  En la página **Seleccionar opciones de detección**, seleccione **Recopilar, mostrar y almacenar datos de ejemplo** y, a continuación, haga clic en **Crear modelo de datos confidenciales** en la parte inferior de la página para iniciar el proceso de detección de datos.
    
    ![Selección de Opciones de Detección, página](images/select-discovery-options-page.png "Página Seleccionar Opciones de Detección")
    
10.  Espere a que se cree el modelo de datos confidenciales. Se muestra la página **Detalles del modelo de datos confidenciales**.
    

## Tarea 2: Análisis del modelo de datos confidenciales

1.  Revise la información de la página **Detalles de modelo de datos confidenciales**.
    
    *   El separador **Información de modelo de datos confidenciales** muestra información sobre el modelo de datos confidenciales, incluido su nombre y el identificador de Oracle Cloud (OCID), el compartimento en el que lo ha guardado, la fecha y hora en que se creó y actualizó por última vez, el base de datos de destino asociada a ella, el esquema seleccionado para la detección (HCM1), los tipos confidenciales seleccionados para la detección (haga clic en el enlace **Ver detalles**) y los totales de esquemas confidenciales detectados, tablas confidenciales, columnas confidenciales, tipos confidenciales y valores confidenciales.
    *   Puede ver los tipos confidenciales seleccionados para la detección (haga clic en **Ver detalles**).
    *   Puede ver la información de la solicitud de trabajo (haga clic en **Ver detalles**).
    *   El gráfico circular muestra porcentajes de categorías y tipos confidenciales.
    *   La tabla **Columnas confidenciales** muestra las columnas confidenciales detectadas. De forma predeterminada, la tabla se muestra con el formato **Vista plana**. Para cada columna confidencial, puede ver el nombre de esquema, el nombre de tabla, el nombre de columna, el tipo confidencial, la columna principal, el tipo de dato, el recuento de filas estimado, los datos de muestra (si ha elegido recuperar datos de muestra y si existen) y los registros de auditoría. Revise los datos de muestra para tener una idea de cómo se ve.
    
    ![Parte superior de la página Detalles de modelo de datos confidenciales](images/sensitive-data-model-details-page-1.png "Parte superior de la página Detalles de modelo de datos confidenciales") ![Parte inferior de la página Detalles de modelo de datos confidenciales](images/sensitive-data-model-details-page-2.png "Parte inferior de la página Detalles de modelo de datos confidenciales")
    
2.  Coloque el mouse sobre la categoría **Información de Identificación** del gráfico para ver su valor. El valor de porcentaje puede ser diferente al valor que se muestra en la captura de pantalla.
    
    ![Categoría de información de identificación en gráfico de modelo de datos confidenciales](images/sdm-chart-identification-information.png "Categoría de información de identificación en gráfico de modelo de datos confidenciales")
    
3.  Con el mouse sobre **Información de Identificación**, haga clic en el segmento del gráfico circular para aumentar el detalle. Observe que la categoría **Información de identificación** ahora se divide en dos categorías más pequeñas (**Identificadores personales** e **Identificadores públicos**).
    
    ![Identificadores personales y públicos en el gráfico del modelo de datos confidenciales](images/sdm-chart-personal-public-identifiers.png "Identificadores personales y públicos en el gráfico del modelo de datos confidenciales")
    
4.  Para aumentar detalle, haga clic en el enlace **Todo** de la ruta de navegación del gráfico.
    
5.  En **Columnas confidenciales**, en la lista desplegable, seleccione **Vista de tipo confidencial** para ordenar las columnas confidenciales por tipo confidencial. De forma predeterminada, todos los elementos se expanden en la vista. Puede contraer los elementos moviendo el control deslizante **Ampliar todo** a la izquierda.
    
    ![Vista de tipo confidencial del modelo de datos confidenciales](images/sensitive-type-view-sdm1.png "Vista de tipo confidencial del modelo de datos confidenciales")
    
6.  En la lista desplegable, seleccione **Vista de esquema** para ordenar las columnas confidenciales por nombre de tabla.
    
    *   Si se detecta una columna confidencial porque tiene una relación con otra columna confidencial como se define en el diccionario de datos de la base de datos, la otra columna confidencial se muestra en la **columna principal**. Por ejemplo, `EMPLOYEE_ID` en la tabla `EMP_EXTENDED` tiene una relación con `EMPLOYEE_ID` en la tabla `EMPLOYEES`.
    
    ![Vista de esquema del modelo de datos confidenciales](images/schema-view-sdm1.png "Vista de esquema del modelo de datos confidenciales")
    

## Tarea 3: Creación de un PDF del informe Modelo de datos confidenciales

1.  En la parte superior de la página **Detalles de modelos de datos confidenciales**, haga clic en **Generar informe**.
    
    Se muestra el cuadro de diálogo **Generar informe**.
    
2.  Deje **PDF** seleccionado, haga clic en **Generar informe** y espere a que el informe se genere al 100 %. Haga clic **aquí** para descargar el informe.
    
    ![Generar informe en PDF de SDM1](images/generate-pdf-report-sdm1.png "Generar informe en PDF de SDM1")
    
3.  Abrir el informe PDF y revisarlo.
    
    *   La tabla **Resumen** muestra los totales de las columnas y valores explorados y los recuentos de los tipos confidenciales, las tablas confidenciales, las columnas confidenciales y los valores confidenciales.
    *   La tabla **Columnas confidenciales** muestra las columnas confidenciales del modelo de datos confidenciales. Para cada columna confidencial, la tabla muestra su tipo confidencial, nombre de esquema, nombre de tabla, nombre de columna, recuento de valores confidenciales, si los datos de la columna coinciden (S o N), si el nombre de la columna coincide (S o N) y si el comentario de la columna coincide (S o N).
    
    ![Informe en PDF de SDM1](images/pdf-report-sdm1.png "Informe en PDF de SDM1")
    
4.  Cierre el informe PDF y vuelva a Oracle Data Safe.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Identificación de datos](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-discovery.html)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023