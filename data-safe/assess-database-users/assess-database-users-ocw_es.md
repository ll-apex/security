# Evaluar usuarios de base de datos

## Introducción

Evaluación de usuario le ayuda a evaluar la seguridad de los usuarios de la base de datos e identificar posibles usuarios de alto riesgo. Por defecto, Oracle Data Safe genera automáticamente evaluaciones de usuario para las bases de datos de destino y las almacena en el historial de evaluaciones. Puede analizar los datos de evaluación en todas las bases de datos de destino y para cada base de datos de destino. Puede supervisar el cambio de seguridad en las bases de datos de destino comparando la última evaluación con una base u otra evaluación.

En este laboratorio, explorará User Assessment.

Tiempo estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Ver la página de visión general de Evaluación de usuario
*   Analizar usuarios en la última evaluación de usuario
*   Cambiar usuarios y derechos en la base de datos destino
*   Refrescamiento de la última evaluación de usuario
*   Ver el historial de evaluaciones de usuario para la base de datos destino
*   Comparar la evaluación de usuario más reciente con la evaluación de usuario inicial

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller
*   Registró la base de datos de destino con Oracle Data Safe

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.

Vea el siguiente vídeo para una breve introducción al laboratorio. [Evaluación de usuarios de base de datos](videohub:1_fvykqng1)

## Tarea 1: Ver la página de visión general de Evaluación de usuario

1.  Vaya a **Evaluación de usuario**. Para ello, en la ruta de navegación situada en la parte superior de la página, haga clic en **Centro de seguridad**. En la parte izquierda, haga clic en **Evaluación de usuario**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
3.  En la parte superior de la página de visión general, revise los cuatro gráficos.
    
    *   El gráfico **Posible riesgo de usuario** muestra el número y el porcentaje de usuarios que pueden tener un **riesgo crítico**, un **riesgo alto**, un **riesgo medio** y un **riesgo bajo**.
    *   El gráfico **Roles de usuario** muestra el número de usuarios con los roles **DBA**, **Administrador de DVD** y **Administrador de auditoría**.
    *   El gráfico **Último cambio de contraseña** muestra el número de usuarios que han cambiado sus contraseñas en los últimos 30 días, en los últimos 30-90 días y hace más de 90 días.
    *   El gráfico **Last Login** muestra el número de usuarios que se han conectado a la base de datos de destino en las últimas 24 horas, en la última semana, en el mes actual, en el año actual y hace un año o más.
    
    ![Primeros tres gráficos de la página de visión general de Evaluación de usuario](images/ocw/ua-dashboard-charts1.png "Primeros tres gráficos de la página de visión general de Evaluación de usuario")
    
    ![Último gráfico de página de visión general de evaluación de usuario](images/ocw/ua-dashboard-charts2.png "Último gráfico de página de visión general de evaluación de usuario")
    
4.  Revise el separador **Resumen de riesgo**.
    
    *   El separador **Resumen de riesgo** se centra en los riesgos potenciales en todas las bases de datos de destino seleccionadas. Muestra los posibles niveles de riesgo, el número de bases de datos de destino, el número total de usuarios en cada nivel de riesgo, el número total de usuarios con privilegios en cada nivel de riesgo y los recuentos de DBA, administradores de DV y administradores de auditoría.
    *   Los niveles de riesgo potenciales se clasifican como **Críticos**, **Altos**, **Media** y **Baja**.
    
    ![Separador Resumen de riesgo de evaluación de usuario](images/ocw/ua-risk-summary-tab.png "Separador Resumen de riesgo de evaluación de usuario")
    
5.  Haga clic en el separador **Resumen de destino**. En este separador se proporciona la siguiente información:
    
    *   Número de usuarios críticos y de alto riesgo, DBA, administradores de DV y administradores de auditoría
    *   Fecha y hora de la última evaluación del usuario
    *   Si la última evaluación del usuario se desvía de la base (si se define una)
    
    ![Separador Resumen de objetivo de evaluación de usuario](images/ocw/ua-target-summary-tab.png "Separador Resumen de objetivo de evaluación de usuario")
    

## Tarea 2: Analizar usuarios en la última evaluación de usuario

La última evaluación de usuario es la que generó automáticamente Oracle Data Safe al registrar la base de datos de destino.

1.  En el separador **Resumen de destino**, haga clic en **Ver informe** para ver la última evaluación de usuario para la base de datos de destino.
    
2.  En la parte superior del informe del separador **Visión general**, revise los gráficos **Riesgo potencial de usuario**, **Roles de usuario**, **Último cambio de contraseña** y **Última conexión**.
    
    ![Últimos gráficos de evaluación de usuario](images/ocw/ua-latest-charts1.png "Últimos gráficos de evaluación de usuario")
    
    ![Últimos gráficos de evaluación de usuario](images/ocw/ua-latest-charts2.png "Últimos gráficos de evaluación de usuario")
    
3.  Haga clic en el separador **Información de evaluación** y revise los detalles.
    
    ![Separador Información de evaluación](images/ocw/ua-assessment-information-tab.png "Separador Información de evaluación")
    
4.  Desplácese hacia abajo y revise la sección **Detalles del usuario**. En esta tabla, se proporciona la siguiente información sobre cada usuario:
    
    *   Nombre de usuario
    *   Tipo de usuario (por ejemplo, PRIVILEGED, SCHEMA)
    *   Si el usuario es un DBA, un administrador de DV o un administrador de auditoría
    *   Nivel de riesgo potencial (por ejemplo, bajo, alto o crítico)
    *   Estado del usuario (por ejemplo, OPEN, LOCKED o EXPIRED\_AND\_LOCKED)
    *   Fecha y hora de la última conexión del usuario a la base de datos de destino
    *   Perfil de usuario
    *   Registros de auditoría para el usuario
    
    ![Detalles de última evaluación de evaluación de usuario](images/ocw/ua-latest-assessment-details.png "Detalles de última evaluación de evaluación de usuario")
    
5.  En la columna **Nombre de usuario**, haga clic en un usuario que sea un riesgo potencial **CRÍTICO**, por ejemplo, **EVIL\_RICH**.
    
    El panel **User Details** (Detalles del usuario) muestra la siguiente información sobre el usuario:
    
    *   Nombre de la base de datos destino
    *   Nombre de usuario
    *   Perfil de usuario
    *   Tipo de usuario (por ejemplo, PRIVILEGED)
    *   Estado (por ejemplo, OPEN)
    *   Riesgo potencial (por ejemplo, CRÍTICO): pase el cursor sobre la **i** para ver lo que constituye un usuario de riesgo crítico.
    *   Fecha y hora de la última conexión
    *   Fecha y hora en que se creó el usuario
    *   Fecha y hora en que se cambió la contraseña
    *   Roles con privilegios (los roles de administrador otorgados al usuario)
    *   Roles: amplíe **Todos los Roles** para ver todos los roles otorgados al usuario.
    *   Privilegios: amplíe **Todos los Privilegios** para ver todos los privilegios otorgados al usuario.
    
    ![Detalles de usuario ADMIN](images/ocw/ua-admin-user-details.png "Detalles de usuario ADMIN")
    
6.  Haga clic en **Cerrar**.
    
7.  Para filtrar el informe para mostrar solo usuarios de riesgo potencialmente crítico, haga lo siguiente: haga clic en el separador **Visión general**. En el gráfico **Riesgo de usuario potencial**, haga clic en la sección **CRÍTICO** del gráfico. Se crea automáticamente un filtro.
    
    ![Filtro de usuarios de riesgo crítico](images/ocw/ua-critical-risk-users-filter.png "Filtro de usuarios de riesgo crítico")
    
8.  Para eliminar el filtro, haga clic en la **X** junto al mismo.
    

## Tarea 3: Cambiar usuarios y derechos en la base de datos de destino

1.  Acceda a la hoja de trabajo de SQL en **Database Actions**.
    
2.  Si es necesario, borre la hoja de trabajo y el separador **Salida de script**.
    
3.  En la hoja de trabajo de SQL, introduzca los siguientes comandos:
    
        <copy>DROP USER evil_rich;
        CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script) (círculo verde con una flecha blanca) para ejecutar la consulta.
    
    ![Botón Ejecutar script](images/run-script.png "Botón Ejecutar script")
    
5.  En el separador **Salida de script** de la parte inferior de la página, verifique que el usuario `EVIL_RICH` se ha borrado, que se ha creado el usuario `JOE_SMITH` y que el permiso se ha otorgado correctamente.
    

## Tarea 4: Refrescamiento de la última evaluación de usuario

1.  Vuelva al separador del explorador de Oracle Data Safe. Lo dejó por última vez en la página **Detalles de evaluación de usuario** para la última evaluación de usuario.
    
2.  Haga clic en el botón **Actualizar ahora**.
    
    Se muestra el panel **Actualizar ahora**.
    
3.  Mantenga el nombre por defecto tal cual y haga clic en **Refrescar ahora**. Espere a que el estado de la última evaluación del usuario sea **SUCCEEDED**. Oracle Data Safe guarda automáticamente una copia estática de la evaluación en el historial de evaluaciones.
    
    ![Panel Refrescar ahora de evaluación de usuario](images/ocw/ua-refresh-now-panel.png "Panel Refrescar ahora de evaluación de usuario")
    
4.  Revise la última evaluación refrescada.
    

## Tarea 5: Comparar la última evaluación del usuario con la evaluación inicial del usuario

Puede seleccionar una evaluación de usuario para comparar con la última evaluación de usuario. Con esta opción, no es necesario definir una línea base. Esta opción solo está disponible cuando está viendo la última evaluación de usuario. Tenga en cuenta que podría haber definido una línea base y comparado la última evaluación con ella.

1.  Mientras visualiza la última evaluación del usuario, a la izquierda, en **Recursos**, haga clic en **Comparar evaluaciones**.
    
2.  Desplácese hasta la sección **Comparación con otras evaluaciones**.
    
3.  Si no se muestra el compartimento, haga clic en **Cambiar compartimento** y seleccione el compartimento.
    
4.  En la lista desplegable **Seleccionar evaluación**, seleccione la evaluación inicial para la base de datos de destino (la segunda en la lista). En cuanto la seleccione, se iniciará la operación de comparación.
    
5.  Revise los resultados.
    
    *   Se ha agregado un nuevo usuario y se ha suprimido un usuario.
    *   La conclusión Nuevo usuario se identifica como un riesgo **CRITICIAL** potencial.
    
    ![Informe de comparación de evaluación de usuario](images/ocw/ua-comparison-report2.png "Informe de comparación de evaluación de usuario")
    
6.  En la columna **Resultados de comparación**, haga clic en uno de los enlaces **Abrir detalles** para ver más información.
    
    Se muestra el panel **Detalles de comparación**.
    
    ![Panel de detalles de comparación](images/ocw/ua-comparison-details-panel.png "Panel de detalles de comparación")
    
7.  Revise la información y, a continuación, haga clic en **Cerrar**. En este punto, puede definir una evaluación base.
    

## Más información

*   [Visión general de Evaluación de usuario](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-6BF46EE2-F7B5-4710-A09C-069EA95F8052)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 17 de agosto de 2023