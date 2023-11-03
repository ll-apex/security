# Enmascaramiento de datos confidenciales

## Introducción

El enmascaramiento de datos proporciona una forma de enmascarar datos confidenciales para que los datos sean seguros con fines de no producción. Por ejemplo, las organizaciones a menudo necesitan crear copias de sus datos de producción para soportar actividades de desarrollo y prueba. La simple copia de los datos de producción expone datos confidenciales a nuevos usuarios. Para evitar un riesgo de seguridad, puede utilizar Enmascaramiento de datos para sustituir los datos confidenciales por datos realistas, pero ficticios.

Los roles otorgados a la cuenta de servicio de Oracle Data Safe en la base de datos de destino controlan qué funciones de Oracle Data Safe puede utilizar con la base de datos. Por defecto, Autonomous Database Serverless tiene todos los roles de Oracle Data Safe otorgados durante el registro de la base de datos de destino, excepto el rol de enmascaramiento de datos (`DS$DATA_MASKING_ROLE`).

Empiece otorgando el rol Data Masking en la base de datos de destino. A continuación, enmascare los datos confidenciales que ha detectado en el laboratorio [Detectar datos confidenciales](?lab=discover-sensitive-data-ocw) mediante la política de enmascaramiento por defecto generada por la función Enmascaramiento de datos. Vea el efecto anterior y posterior en los datos enmascarados mediante Oracle Database Actions.

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Otorgar el rol Enmascaramiento de datos a la base de datos de destino
*   Ver datos confidenciales en la base de datos de destino
*   Crear una política de enmascaramiento para la base de datos de destino
*   Modificar la política de enmascaramiento
*   Enmascaramiento de datos confidenciales en la base de datos de destino mediante el enmascaramiento de datos
*   Ver el informe Enmascaramiento de datos
*   Validar los datos enmascarados en la base de datos de destino

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller
*   Registró la base de datos de destino con Oracle Data Safe. Asegúrese de tener a mano la contraseña `ADMIN` para la base de datos.
*   Se ha creado un modelo de datos confidenciales

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.

Vea el siguiente vídeo para una breve introducción al laboratorio. [Enmascaramiento de datos confidenciales](videohub:1_czbwzq21)

## Tarea 1: Asignación del rol Enmascaramiento de datos a la base de datos de destino

1.  Vuelva a la hoja de trabajo de SQL en Database Actions.
    
2.  Si se le solicita que se conecte a la base de datos de destino, conéctese como usuario `ADMIN`.
    
3.  Haga clic en el botón **Clear** (Borrar) (icono de papelera) de la barra de herramientas para borrar la hoja de trabajo. Haga clic en el botón **Borrar salida** del separador **Salida de script** para borrar la salida.
    
4.  En la hoja de trabajo de SQL, introduzca el siguiente comando para otorgar el rol Enmascaramiento de datos a la cuenta de servicio de Oracle Data Safe en la base de datos de destino.
    
        <copy>EXECUTE DS_TARGET_UTIL.GRANT_ROLE('DS$DATA_MASKING_ROLE');</copy>
        
5.  En la barra de herramientas, haga clic en el botón **Ejecutar sentencia** (círculo verde con una flecha blanca) para ejecutar la consulta.
    
    ![Botón Run Statement en la barra de herramientas](images/run-statement-button.png "Botón Run Statement en la barra de herramientas")
    
6.  Verifique que la salida del script sea la siguiente:
    
    `PL/SQL procedure successfully completed`
    
    Ahora puede enmascarar datos confidenciales en la base de datos de destino.
    
7.  Borre la salida de la hoja de trabajo y el script.
    

## Tarea 2: Visualización de datos confidenciales en la base de datos de destino

En el laboratorio [Detectar datos confidenciales](?lab=discover-sensitive-data-ocw), ha aprendido que la tabla `HCM1.EMPLOYEES` tiene datos confidenciales. En esta tarea, puede ver los datos confidenciales reales de esa tabla mediante Database Actions.

1.  En el separador **Navegador** de Database Actions, seleccione el esquema **HCM1** de la primera lista desplegable.
    
2.  Arrastre la tabla `EMPLOYEES` a la hoja de trabajo.
    
    ![tabla EMPLOYEES](images/drag-employees-table-to-worksheet.png "tabla EMPLOYEES")
    
3.  Cuando se le solicite que seleccione un tipo de inserción, haga clic en **Seleccionar** y, a continuación, en **Aplicar**.
    
    ![Seleccionar el tipo de cuadro de diálogo de inserción](images/insertion-type-select.png "Seleccionar el tipo de cuadro de diálogo de inserción")
    
4.  Consulte la consulta SQL en la hoja de trabajo.
    
    ![Separador Worksheet mostrando la tabla EMPLOYEES](images/query-employees-table.png "Separador Worksheet mostrando la tabla EMPLOYEES")
    
5.  En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script).
    
    ![Botón Ejecutar script](images/run-script.png "Botón Ejecutar script")
    
6.  En el separador **Salida de script**, revise los resultados de la consulta.
    
    *   Los datos como `EMPLOYEE_ID`, `FIRST_NAME`, `LAST_NAME`, `EMAIL`, `PHONE_NUMBER` y `HIRE_DATE` se consideran datos confidenciales y se deben enmascarar si se comparten para un uso que no sea de producción.
7.  Vuelva al separador del explorador de Oracle Data Safe. Mantenga este separador del explorador abierto porque volverá a él más tarde.
    

## Tarea 3: Creación de una política de enmascaramiento para la base de datos de destino

El enmascaramiento de datos puede generar una política de enmascaramiento para la base de datos de destino según el modelo de datos confidenciales. Intenta seleccionar automáticamente un formato de enmascaramiento por defecto para cada columna confidencial. Puede editar estas selecciones por defecto y seleccionar otras diferentes según sea necesario. En ocasiones, es posible que se le solicite que corrija los problemas (si existen) en los formatos de enmascaramiento.

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Data Safe**.
    
2.  A la izquierda, en **Centro de seguridad**, haga clic en **Enmascaramiento de datos**.
    
3.  En **Recursos relacionados**, haga clic en **Políticas de enmascaramiento**.
    
4.  En **Ámbito de lista** a la izquierda, seleccione el compartimento. La página **Políticas de enmascaramiento** muestra que no hay políticas de enmascaramiento disponibles para la base de datos de destino.
    
    ![Página Políticas de Enmascaramiento](images/no-masking-policies-available.png "Página Políticas de Enmascaramiento")
    
5.  Haga clic en **Create Masking Policy**.
    
    Se muestra el panel **Crear política de enmascaramiento**.
    
6.  Configure la política de enmascaramiento de la siguiente manera:
    
    *   Nombre: **Mask SDM1**
    *   Compartment: **seleccione el compartimento**
    *   Descripción: **Política de enmascaramiento para SDM1**
    *   Seleccione cómo desea crear la política de enmascaramiento: deje seleccionada la opción **Uso de un modelo de datos confidenciales**.
    *   Modelo de datos confidenciales: seleccione **SDM1\[nombre\_de\_base\_datos\_de\_destino\]**. Si no tiene este modelo de datos confidenciales, consulte el laboratorio [Detectar datos confidenciales](?lab=discover-sensitive-data-ocw).
    
    ![Panel Crear política de enmascaramiento con SDM1](images/ocw/create-masking-policy-sdm1.png "Panel Crear política de enmascaramiento con SDM1")
    
7.  Haga clic en **Create Masking Policy**.
    
    _Importante No cierre el panel. Se cierra automáticamente una vez finalizadas todas las operaciones. Si cierra el panel antes de que finalicen las operaciones, la operación para agregar columnas a la política de enmascaramiento no se inicia._
    
    Se muestra la página **Detalles de política de enmascaramiento**.
    
8.  Revise la política de enmascaramiento.
    
    *   En el separador **Información de política de enmascaramiento**, puede ver el nombre de la política de enmascaramiento (y editarla), el identificador de Oracle Cloud (OCID) para la política de enmascaramiento, el compartimento en el que se almacena la política de enmascaramiento, un enlace a la solicitud de trabajo para la política de enmascaramiento, un enlace a las opciones de enmascaramiento, la base de datos de destino y el modelo de datos confidenciales a los que está asociada la política de enmascaramiento y la fecha/hora en la que se ha creado y actualizado por última vez la política de enmascaramiento.
    *   La tabla **Columnas de enmascaramiento** muestra todas las columnas de enmascaramiento y sus formatos de enmascaramiento. Si es necesario, puede seleccionar un formato de enmascaramiento diferente para cualquier columna de enmascaramiento. Puede hacer clic en el icono de lápiz junto a un formato de enmascaramiento para editarlo.
    
    ![Página Detalles de Política de Enmascaramiento para la Máscara SDM1 superior](images/ocw/masking-policy-details-top.png "Página Detalles de Política de Enmascaramiento para la Máscara SDM1 superior")
    
    ![Página Detalles de Política de Enmascaramiento para la Máscara SDM1 inferior](images/ocw/masking-policy-details-bottom.png "Página Detalles de Política de Enmascaramiento para la Máscara SDM1 inferior")
    

## Tarea 4: Modificación de la política de enmascaramiento

Defina `SALARY` para que sea un número fijo, como 50000.

1.  Localice la fila de la columna `SALARY` en la tabla `EMPLOYEES`.
    
2.  Haga clic en el botón del lápiz junto al formato de enmascaramiento.
    
    Se muestra la página **Editar formato de enmascaramiento**.
    
3.  En la lista desplegable **Entrada de formato de enmascaramiento**, seleccione **Número fijo**.
    
4.  En el cuadro **Número fijo**, introduzca **50000**.
    
    ![Página Editar Formato de Enmascaramiento](images/ocw/edit-masking-format-page.png "Página Editar Formato de Enmascaramiento")
    
5.  Haga clic en **Continuar**.
    
    Observe que la fila actualizada está resaltada.
    
    ![La fila actualizada está resaltada](images/ocw/updated-row-is-highlighted.png "La fila actualizada está resaltada")
    
6.  Para guardar la actualización, haga clic en **Guardar formatos de enmascaramiento** y espere a que finalice la operación de actualización.
    

## Tarea 5: Enmascaramiento de datos confidenciales en la base de datos de destino mediante el enmascaramiento de datos

Puede ejecutar un trabajo de enmascaramiento de datos en la base de datos de destino desde la página **Detalles de política de enmascaramiento** o **Enmascaramiento de datos**.

1.  En la página **Detalles de política de enmascaramiento**, haga clic en **Destino de máscara**.
    
    Aparece el panel **Enmascarar datos confidenciales**.
    
2.  En la lista desplegable **Base de datos destino**, seleccione la base de datos de destino y, a continuación, haga clic en **Enmascarar datos**.
    
    ![Enmascaramiento de panel de datos confidenciales](images/ocw/mask-sensitive-data-panel.png "Enmascaramiento de panel de datos confidenciales")
    
    Aparece la página **Request de trabajo**.
    
3.  Supervise el progreso de la solicitud de trabajo visualizando los mensajes de log en la tabla **Mensajes de log**.
    
    ![Mensajes de log para solicitud de trabajo de enmascaramiento de datos](images/ocw/masking-log-messages.png "Mensajes de log para solicitud de trabajo de enmascaramiento de datos")
    
4.  Espere a que el estado sea **SUCCEEDED**.
    

## Tarea 6: Visualización del informe Enmascaramiento de datos

1.  En la página **Solicitud de trabajo**, junto a **Informe de enmascaramiento** en el separador **Información de solicitud de trabajo**, haga clic en **Ver detalles**.
    
    Se muestra la página **Detalles de informe de enmascaramiento**.
    
2.  Revise el informe de enmascaramiento.
    
    *   El separador **Información de informe de enmascaramiento** muestra el nombre de la base de datos de destino, el nombre de la política de enmascaramiento (puede hacer clic en un enlace para verlo), el identificador de Oracle Cloud (OCID) para el informe de enmascaramiento, la fecha y hora en que se inició y finalizó el trabajo de enmascaramiento de datos y el número de tipos confidenciales enmascarados, esquemas, tablas, columnas y valores. Puede hacer clic en un enlace para ver las opciones de enmascaramiento. También hay un gráfico circular que muestra los porcentajes de valores enmascarados para cada tipo confidencial. Puede hacer clic en un segmento de tarta para profundizar en el gráfico.
    *   La tabla **Columnas enmascaradas** muestra cada columna confidencial enmascarada y su esquema, tabla, formato de enmascaramiento, tipo confidencial, columna principal y número total de valores enmascarados respectivos.
    
    ![Parte superior del informe de enmascaramiento](images/ocw/masking-report-top2.png "Parte superior del informe de enmascaramiento") ![Parte inferior de informe de enmascaramiento](images/ocw/masking-report-bottom.png "Parte inferior de informe de enmascaramiento")
    

## Tarea 7: Validar los datos enmascarados en la base de datos de destino

1.  Vuelva a la hoja de trabajo de SQL en Database Actions. Si la sesión ha caducado, vuelva a conectarse como usuario `ADMIN`. La sentencia `SELECT` de la tabla `EMPLOYEES` se debe mostrar en la hoja de trabajo.
    
2.  En la barra de herramientas, haga clic en el botón **Ejecutar sentencia** (círculo verde con una flecha blanca) para ejecutar la consulta.
    
    Al hacer clic en el botón **Ejecutar Sentencia** (en lugar del botón **Ejecutar Script**) se mostrarán los resultados en el separador **Resultados de Consulta** en lugar del separador **Salida de Script**. Esto le permitirá realizar una comparación antes y después de los datos enmascarados.
    
3.  Revise los datos enmascarados en el separador **Resultado de Consulta** en la parte inferior de la página.
    
    *   Puede cambiar el tamaño del panel para ver más datos y desplazarse hacia abajo y hacia la derecha.
    *   Busque la columna `SALARY` y verifique que los valores son todos 50000.
    
    ![Datos de EMPLEADO Enmascarados](images/ocw/masked-query-results.png "Datos de EMPLEADO Enmascarados")
    
4.  (Opcional) Haga clic en el separador **Salida de script** para ver los datos sin enmascarar originales.
    

## Más información

*   [Enmascaramiento de datos](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-masking.html)
*   [Registro de base de datos de destino](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 17 de agosto de 2023