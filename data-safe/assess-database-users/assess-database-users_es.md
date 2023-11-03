# Evaluar usuarios de base de datos

## Introducción

Evaluación de usuario le ayuda a evaluar la seguridad de los usuarios de la base de datos e identificar posibles usuarios de alto riesgo. Por defecto, Oracle Data Safe genera automáticamente evaluaciones de usuario para las bases de datos de destino y las almacena en el historial de evaluaciones. Puede analizar los datos de evaluación en todas las bases de datos de destino y para cada base de datos de destino. Puede supervisar el cambio de seguridad en las bases de datos de destino comparando la última evaluación con una base u otra evaluación.

En este laboratorio, explorará User Assessment.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Ver la página de visión general de Evaluación de usuario
*   Analizar usuarios en la última evaluación de usuario
*   Revise los registros de auditoría del usuario `ADMIN`
*   Crear un usuario en la base de datos destino
*   Refrescamiento de la evaluación de usuario más reciente y cambio de nombre
*   Ver el historial de evaluaciones de usuario para la base de datos destino
*   Comparar la evaluación de usuario más reciente con la evaluación de usuario inicial
*   Descargue la última evaluación del usuario como informe PDF
*   Visualización del historial de evaluaciones de usuarios para todas las bases de datos destino

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))
*   Se ha iniciado la recopilación de datos de auditoría para la base de datos de destino en Oracle Data Safe (consulte [Actividad de base de datos de auditoría](?lab=audit-database-activity)). La recopilación de datos de auditoría es necesaria si desea ver los registros de auditoría de los usuarios desde Evaluación de usuario.

### Supuestos

*   Los valores de datos pueden ser diferentes de los que se muestran en las capturas de pantalla.

[Evaluar usuarios de base de datos](videohub:1_fvykqng1)

## Tarea 1: Ver la página de visión general de Evaluación de usuario

1.  Vaya a **Evaluación de usuario**. Para ello, en la ruta de navegación situada en la parte superior de la página, haga clic en **Centro de seguridad**. En la parte izquierda, haga clic en **Evaluación de usuario**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
3.  En la parte superior de la página de visión general, revise los cuatro gráficos.
    
    *   El gráfico **Posible riesgo de usuario** muestra el número de usuarios que pueden tener un riesgo **crítico**, **alto**, **medio** y **bajo**.
    *   El gráfico **Roles de usuario** muestra el número de usuarios con los roles **DBA**, **Administrador de DVD** y **Administrador de auditoría**.
    *   El gráfico **Último cambio de contraseña** muestra el número de usuarios que han cambiado sus contraseñas en los últimos 30 días, en los últimos 30-90 días y hace más de 90 días.
    *   El gráfico **Last Login** muestra el número de usuarios que se han conectado a la base de datos de destino en las últimas 24 horas, en la última semana, en el mes actual, en el año actual y hace un año o más.
    
    ![Gráficos de página de visión general de Evaluación de usuario](images/ua-dashboard-charts.png "Gráficos de página de visión general de Evaluación de usuario")
    
4.  Revise el separador **Resumen de riesgo**.
    
    *   El separador **Resumen de riesgo** se centra en los riesgos potenciales en todas las bases de datos de destino seleccionadas. Muestra los posibles niveles de riesgo, el número de bases de datos de destino, el número total de usuarios en cada nivel de riesgo, el número total de usuarios con privilegios en cada nivel de riesgo y los recuentos de DBA, administradores de DV y administradores de auditoría.
    *   Los niveles de riesgo potenciales se clasifican como **Críticos**, **Altos**, **Media** y **Baja**.
    
    ![Separador Resumen de riesgo de evaluación de usuario](images/ua-risk-summary-tab.png "Separador Resumen de riesgo de evaluación de usuario")
    
5.  Haga clic en el separador **Resumen de destino**. En este separador se proporciona la siguiente información:
    
    *   Número de usuarios críticos y de alto riesgo, DBA, administradores de DV y administradores de auditoría
    *   Fecha y hora de la última evaluación del usuario
    *   Si la última evaluación del usuario se desvía de la base (si se define una)
    
    ![Separador Resumen de objetivo de evaluación de usuario](images/ua-target-summary-tab.png "Separador Resumen de objetivo de evaluación de usuario")
    

## Tarea 2: Analizar usuarios en la última evaluación de usuario

Oracle Data Safe genera automáticamente la última evaluación de usuario al registrar la base de datos de destino.

1.  En el separador **Resumen de destino**, haga clic en **Ver informe** para ver la última evaluación de usuario para la base de datos de destino.
    
2.  En la parte superior del informe del separador **Visión general**, revise los gráficos **Riesgo potencial de usuario**, **Roles de usuario**, **Último cambio de contraseña** y **Última conexión**.
    
    ![Últimos gráficos de evaluación de usuario](images/ua-latest-charts.png "Últimos gráficos de evaluación de usuario")
    
3.  Haga clic en el separador **Información de evaluación**. Puede ver la siguiente información:
    
    *   Nombre de la última evaluación de usuario
    *   OCID de la última evaluación de usuario
    *   Compartimento al que pertenece la última evaluación de usuario
    *   Nombre de la base de datos de destino
    *   La fecha y la hora de evaluación
    *   El programa para la última evaluación
    *   Si la última evaluación se define como una evaluación base
    *   Si la última evaluación cumple la evaluación de referencia (si se establece una)
    
    ![Separador Información de evaluación para la última evaluación de usuario](images/ua-assessment-information-tab-latest-assessment.png "Separador Información de evaluación para la última evaluación de usuario")
    
4.  Desplácese hacia abajo y revise la sección **Detalles del usuario**. Por defecto, esta tabla proporciona la siguiente información sobre cada usuario:
    
    *   Nombre de usuario
    *   Tipo de usuario (por ejemplo, PRIVILEGED, SCHEMA)
    *   Si el usuario es un DBA, un administrador de DV o un administrador de auditoría
    *   Nivel de riesgo potencial (por ejemplo, bajo, alto o crítico)
    *   Estado del usuario (por ejemplo, OPEN, LOCKED o EXPIRED\_AND\_LOCKED)
    *   Fecha y hora de la última conexión del usuario a la base de datos de destino
    *   Perfil de usuario
    *   Registros de auditoría para el usuario
    
    ![Detalles de última evaluación de evaluación de usuario](images/ua-latest-user-details.png "Detalles de última evaluación de evaluación de usuario")
    
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
    
    ![Detalles de usuario de EVIL RICH](images/ua-evil-rich-user-details.png "Detalles de usuario de EVIL RICH")
    
6.  Haga clic en **Cerrar**.
    
7.  Para filtrar el informe para mostrar solo usuarios de riesgo potencialmente crítico, haga lo siguiente: haga clic en el separador **Visión general**. En el gráfico **Riesgo de usuario potencial**, haga clic en la sección **CRÍTICO** del gráfico. Se crea automáticamente un filtro.
    
    ![Filtro de usuarios de riesgo crítico](images/ua-critical-risk-users-filter.png "Filtro de usuarios de riesgo crítico")
    
8.  Para eliminar el filtro, haga clic en la **X** junto al mismo.
    

## Tarea 3: Revisar los registros de auditoría del usuario `ADMIN`

1.  Identifique la fila de la tabla para el usuario `ADMIN`. En la columna **Registros de auditoría** del usuario `ADMIN`, haga clic en **Ver actividad**.
    
    ![Registros de auditoría de usuario ADMIN](images/ua-admin-user-audit-records.png "Registros de auditoría de usuario ADMIN")
    
    Se muestra el informe **Todas las actividades** para el usuario `ADMIN`.
    
2.  Examine el informe.
    
    *   El informe se filtra automáticamente para mostrar los registros de auditoría de la última semana, para el usuario `ADMIN` y para la base de datos de destino.
    *   En la parte superior del informe, puede ver los totales de **Destinos**, **Usuarios de base de datos**, **Hosts de cliente**, **DML**, **Cambios de privilegios**, **DDL**, **Cambios de usuario/derecho**, **Fallos de conexión**, **Login Successes** y **Total de eventos**.
    *   La columna **Evento** de la tabla muestra los tipos de actividades realizadas por el usuario `ADMIN`, por ejemplo, `GRANT`, `LOGON`, `CREATE USER`, etc.
    *   En la parte inferior de la página, puede hacer clic en los números de página para ver más registros de auditoría.
    
    ![Informe All Activity del usuario ADMIN](images/ua-all-activity-report-admin-user.png "Informe All Activity del usuario ADMIN")
    

## Tarea 4: Creación de un usuario en la base de datos destino

1.  Acceda a la hoja de trabajo de SQL en **Database Actions**.
    
2.  Si es necesario, borre la hoja de trabajo y el separador **Salida de script**.
    
3.  En la hoja de trabajo de SQL, introduzca los siguientes comandos:
    
        <copy>CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script) (círculo verde con una flecha blanca) para ejecutar la consulta.
    
    ![Botón Ejecutar script](images/run-script.png "Botón Ejecutar script")
    
5.  En el separador **Salida de script** de la parte inferior de la página, verifique que el usuario `JOE_SMITH` se ha creado y que el permiso se ha otorgado correctamente.
    

## Tarea 5: Refrescamiento de la última evaluación de usuario y cambio de nombre

1.  Vuelva al separador del explorador de Oracle Data Safe.
    
2.  En **Centro de seguridad**, a la izquierda, haga clic en **Evaluación de usuario**.
    
3.  Haga clic en el separador **Resumen de destino**.
    
4.  Haga clic en **Ver informe** para que la base de datos de destino abra la última evaluación de usuario.
    
5.  Para refrescar la última evaluación de usuario, haga clic en el botón **Refrescar ahora**.
    
    ![Botón Refrescar ahora de evaluación de usuario](images/ua-refresh-now-button.png "Botón Refrescar ahora de evaluación de usuario")
    
6.  En el panel **Refrescar ahora**, mantenga el nombre por defecto tal cual y haga clic en **Refrescar ahora**. Espere a que el estado de la última evaluación del usuario sea **SUCCEEDED**. Oracle Data Safe guarda automáticamente una copia estática de la evaluación en el historial de evaluaciones.
    
    ![Panel Refrescar ahora de evaluación de usuario](images/ua-refresh-now-panel.png "Panel Refrescar ahora de evaluación de usuario")
    
7.  Revise la última evaluación refrescada. Observe que se muestra el usuario que acaba de crear, `JOE_SMITH`.
    
8.  Haga clic en el separador **Información de evaluación** y, a continuación, haga clic en el icono **Lápiz** situado junto al nombre de la evaluación. Cambie el nombre a **Última evaluación de usuario** y, a continuación, haga clic en el icono **Guardar**. Espere a que el estado cambie de **UPDATING** a **SUCCEEDED**. El nombre se actualiza en la página.
    
    ![Última evaluación de usuario renombrada](images/ua-renamed-latest-assessment.png "Última evaluación de usuario renombrada")
    

## Tarea 6: Ver el historial de evaluaciones de usuario de la base de datos de destino

1.  En la parte superior de la página **Última evaluación de usuario**, haga clic en **Ver historial**.
    
2.  Asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
3.  Revise la lista de evaluaciones.
    
    ![Historial de evaluaciones para la última evaluación de usuario](images/user-assessment-history-page.png "Historial de evaluaciones para la última evaluación de usuario")
    
4.  Haga clic en **Cerrar** para volver a la última evaluación del usuario.
    
    Si ha salido de la última evaluación de usuario, puede volver a ella haciendo lo siguiente: haga clic en **Evaluación de usuario** en la ruta de navegación. Haga clic en el separador **Resumen de Destino**. Haga clic en **Ver informe** para la base de datos de destino.
    

## Tarea 7: Comparar la última evaluación del usuario con la evaluación inicial del usuario

Puede seleccionar una evaluación de usuario para comparar con la última evaluación de usuario. Con esta opción, no es necesario definir una línea base. Esta opción solo está disponible cuando está viendo la última evaluación de usuario.

1.  Mientras visualiza la última evaluación del usuario, a la izquierda, en **Recursos**, haga clic en **Comparar evaluaciones**.
    
2.  Desplácese hasta la sección **Comparación con otras evaluaciones**.
    
3.  Si no se muestra el compartimento, haga clic en **Cambiar compartimento** y seleccione el compartimento.
    
4.  En la lista desplegable **Seleccionar evaluación**, seleccione la evaluación inicial para la base de datos de destino (la segunda en la lista). En cuanto la seleccione, se iniciará la operación de comparación.
    
5.  Revise el informe de **comparación**.
    
    *   El informe indica que se ha agregado un nuevo usuario.
    *   La conclusión **Nuevo usuario** es un riesgo **CRÍTICO** potencial.
    
    ![Informe de comparación de evaluación de usuario](images/ua-comparison-report.png "Informe de comparación de evaluación de usuario")
    
6.  En la columna **Resultados de comparación** de la posible conclusión de riesgo crítico, haga clic en los enlaces **Abrir detalles** para ver más información.
    
    Se muestra el panel **Detalles de comparación**.
    
    ![Panel de detalles de comparación](images/ua-comparison-details-panel.png "Panel de detalles de comparación")
    
7.  Revise la información y, a continuación, haga clic en **Cerrar**.
    

## Tarea 8: Descargar la última evaluación del usuario como informe PDF

1.  En la parte superior de la última página de evaluación del usuario, en el menú **Más acciones**, haga clic en **Generar informe**.
    
    Se muestra el cuadro de diálogo **Generar informe**.
    
2.  Deje **PDF** seleccionado como formato de informe y haga clic en **Generar informe**.
    
3.  Espere a que aparezca un mensaje que indique que la **generación del informe PDF ha finalizado** y, a continuación, haga clic en el enlace **aquí**.
    
    ![Cuadro de diálogo Generar informe en Evaluación de usuario](images/ua-generate-report-dialog.png "Cuadro de diálogo Generar informe en Evaluación de usuario")
    
4.  Abrir el PDF y revisarlo.
    
    ![Última evaluación de usuario en formato PDF, página 1](images/ua-pdf-report-page1.png "Última evaluación de usuario en formato PDF, página 1")
    
    ![Última evaluación de usuario en formato PDF, página 2](images/ua-pdf-report-page2.png "Última evaluación de usuario en formato PDF, página 2")
    
5.  Cierre el PDF y vuelva al separador del explorador de Oracle Data Safe.
    

## Tarea 9: Visualización del historial de evaluaciones de usuarios para todas las bases de datos destino

En la página Historial de evaluaciones de usuarios, puede ver una lista de todas las evaluaciones de usuario guardadas para todas las bases de datos de destino.

1.  En la ruta de la parte superior de la página, haga clic en **Evaluación de usuario**.
    
2.  En **Recursos relacionados**, haga clic en **Historial de evaluaciones**.
    
3.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado.
    
4.  Observe que aquí se muestran las evaluaciones de usuario guardadas.
    
    *   Puede comparar el número de riesgos críticos, riesgos altos, DBA, administradores de DV y administradores de auditoría en todas las bases de datos de destino de los compartimentos seleccionados.
    *   También puede identificar rápidamente las evaluaciones de usuario definidas como líneas base.
    
    ![Historial de evaluaciones para todas las bases de datos destino](images/ua-assessment-history-all-targets.png "Historial de evaluaciones para todas las bases de datos destino")
    
5.  Para ordenar la lista por base de datos de destino, haga clic en la cabecera de columna **Base de datos de destino**.
    
6.  Haga clic en el nombre de una evaluación de usuario para la base de datos de destino. Observe que no puede refrescar los datos en una evaluación de usuario guardada.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Visión general de Evaluación de usuario](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-6BF46EE2-F7B5-4710-A09C-069EA95F8052)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023