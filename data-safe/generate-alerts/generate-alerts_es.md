# Generar alertas

## Introducción

Una alerta es un mensaje que le avisa cuando un evento de auditoría concreto se produce en una base de datos de destino. En Oracle Data Safe, puede aprovisionar políticas de alerta en las bases de datos de destino, ver y gestionar alertas, ver informes de alerta predefinidos y crear informes de alerta personalizados.

Para empezar, revise las políticas de alerta predefinidas en Oracle Data Safe y, a continuación, aprovisione dos de ellas. Mediante Database Actions, realice actividad en la base de datos de destino para generar alertas en Oracle Data Safe. Revise las alertas generadas y cree un informe de alertas personalizado. Descargue el informe en formato PDF.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Revisión de las políticas de alerta de Oracle Data Safe
*   Aprovisionamiento de políticas de alerta en la base de datos de destino
*   Realizar actividad en la base de datos de destino para generar alertas en Oracle Data Safe
*   Revisar alertas en Oracle Data Safe
*   Ver detalles de una alerta y cerrarla
*   Creación de un informe de alertas personalizado
*   Generar y descargar un informe de alertas personalizado como PDF
*   Ver el historial de informes de alerta

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud y se conectó a Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe. Asegúrese de tener la contraseña `ADMIN` para la base de datos de destino disponible (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database)).
*   Se ha iniciado la recopilación de datos de auditoría para la base de datos de destino en Oracle Data Safe (consulte [Actividad de base de datos de auditoría](?lab=audit-database-activity))

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.

## Tarea 1: Revisión de las políticas de alerta de Oracle Data Safe

1.  En **Centro de seguridad**, haga clic en **Alertas**.
    
    Se muestra la página **Alertas**. El panel de control de alertas no tiene datos porque aún no ha activado ninguna política de alertas.
    
    ![Panel de control de alertas sin datos](images/alerts-dashboard-no-data.png "Panel de control de alertas sin datos")
    
2.  En **Recursos relacionados**, haga clic en **Políticas de alertas**.
    
3.  Revise la lista de políticas de alerta disponibles que proporciona Oracle Data Safe. Son los siguientes:
    
    *   Conexiones fallidas por parte del usuario administrador
    *   Cambios de perfil
    *   Cambios de parámetros de base de datos
    *   Cambios de política de auditoría
    *   Creación o modificación de usuarios
    *   Cambios de derechos de usuario
    *   Cambios del esquema de base de datos
    
    ![Políticas de alerta de Oracle Data Safe](images/oracle-data-safe-alert-policies.png "Políticas de alerta de Oracle Data Safe")
    
4.  Haga clic en la política de alerta **User Creation/Modification** y revise los detalles.
    
    Se muestra la página **Detalles de política de alerta** para la política de alerta **Creación/Modificación de usuario**.
    
    ![Detalles de política de alerta de modificación de creación de usuario](images/user-creation-modification-alert-policy-details.png "Detalles de política de alerta de modificación de creación de usuario")
    
5.  Junto a **Política aplicada en bases de datos de destino**, haga clic en **Ver lista** para ver las bases de datos de destino asociadas a la política de alerta.
    
    Se muestra la página **Asociaciones entre Políticas y Destinos** con el filtro **Nombre de Política** definido en **Creación/Modificación de Usuarios**. Como todavía no ha asociado la política de alerta a ninguna base de datos de destino, en la tabla se muestra **No hay asociaciones entre políticas y destinos disponibles**.
    
    ![No hay ninguna asociación de política y destino disponible](images/no-target-policy-associations.png "No hay ninguna asociación de política y destino disponible")
    

## Tarea 2: Aprovisionamiento de políticas de alerta en la base de datos de destino

1.  En la lista desplegable **Nombre de política** en **Filtros**, seleccione **Todo**.
    
2.  En la página **Asociaciones entre políticas y destinos**, haga clic en **Aplicar política**.
    
    Se muestra el panel **Aplicar y activar política de alerta a bases de datos de destino**.
    
3.  Seleccione **Solo Destinos Seleccionados**.
    
4.  Si es necesario, haga clic en **Cambiar compartimento** y seleccione el compartimento.
    
5.  En la lista desplegable, seleccione la base de datos destino.
    
6.  Seleccione **Políticas Seleccionadas Sólo**.
    
7.  En la lista desplegable, una a la vez, seleccione las políticas de alerta **Creación/Modificación de Usuario** y **Inicios de Sesión Fallidos por Usuario Administrador**.
    
8.  Haga clic en **Apply Policy** (Aplicar política) y espere hasta que un mensaje indique que puede cerrar el panel.
    
    ![Panel Aplicar y activar políticas de alerta a bases de datos de destino](images/apply-and-enable-alert-policy-panel.png "Panel Aplicar y activar políticas de alerta a bases de datos de destino")
    
9.  Haga clic en **Cerrar**.
    
    Las dos asociaciones de política de destino para la base de datos de destino se muestran en la página y están activadas y activas. Si no se muestra una asociación de política de destino, borre el filtro para el nombre de la política si aún está definido en **Creación/Modificación de usuarios**.
    
    ![Dos asociaciones de política y destino para la base de datos de destino](images/two-target-policy-associations-for-target.png "Dos asociaciones de política y destino para la base de datos de destino")
    

## Tarea 3: Realizar actividad en la base de datos de destino para generar alertas en Oracle Data Safe

En esta tarea, puede realizar actividades en la base de datos de destino en Database Actions para generar algunos datos de auditoría. En primer lugar, intente iniciar sesión como usuario `ADMIN` con contraseñas incorrectas. A continuación, inicie sesión y cree una cuenta de usuario.

1.  Vuelva a la hoja de trabajo de SQL en Database Actions.
    
2.  Si la sesión ha caducado, está bien. Haga clic en **Aceptar** y, a continuación, en **Dejar**. De lo contrario, en la lista desplegable de la esquina superior derecha, seleccione **Cerrar sesión** y, a continuación, en el cuadro de diálogo, haga clic en **Dejar**.
    
    Se muestra la página **Conectar**. El campo de nombre de usuario se rellena previamente con el usuario `ADMIN`.
    
3.  Haga esto dos veces: introduzca una contraseña incorrecta y, a continuación, haga clic en **Conectar**.
    
    Se muestra un mensaje **Credenciales no válidas**.
    
    ![Mensaje de contraseña de base de datos no válida](images/invalid-database-password.png "Mensaje de contraseña de base de datos no válida")
    
4.  Introduzca la contraseña correcta y haga clic en **Conectar**.
    
5.  Si es necesario, en **Desarrollo**, haga clic en **SQL**.
    
6.  Borre la hoja de trabajo y, a continuación, pegue el siguiente script SQL. Sustituya `your-password` por la contraseña que desee. La contraseña debe tener entre 12 y 30 caracteres de longitud y debe incluir al menos una letra mayúscula, una letra minúscula y un carácter numérico. No puede contener su nombre de usuario ni el carácter de comillas dobles (").
    
        <copy>drop user MALFOY cascade;
        create user MALFOY identified by your-password;
        grant PDB_DBA to MALFOY;</copy>
        
7.  En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script) y espere a que el script termine de ejecutarse.
    
8.  En la salida del script, verifique que el usuario `MALFOY` se ha borrado correctamente y, a continuación, se ha vuelto a crear.
    
9.  Vuelva al separador del explorador de Oracle Data Safe y espere un par de minutos a que Oracle Data Safe genere las alertas.
    

## Tarea 4: Revisar alertas en Oracle Data Safe

1.  En **Security Center** de la izquierda, haga clic en **Alerts**.
    
2.  En **Filtros** a la izquierda, seleccione la base de datos de destino.
    
3.  Observe que el panel de control Alerts ahora tiene datos.
    
    *   El gráfico **Resumen de alertas** muestra que hay cuatro alertas. Dos son de riesgo crítico y dos de riesgo medio.
    *   El gráfico **Alertas abiertas** muestra que hay cuatro alertas en el día actual.
    *   El separador **Resumen de Alertas** muestra el número de alertas críticas, altas y medias junto con los recuentos de bases de datos de destino. También muestra el número total de alertas y bases de datos de destino.
    *   El separador **Resumen de Destinos** muestra el número de alertas abiertas, críticas, altas y medias.
    
    ![Panel de control de alertas con datos](images/alerts-dashboard-with-data.png "Panel de control de alertas con datos") ![Separador Resumen de Destinos](images/targets-summary.png "Separador Resumen de Destinos")
    
4.  En **Recursos relacionados**, haga clic en **Informes**.
    
5.  En la columna **Nombre del informe** de la derecha, haga clic en el informe **Todas las alertas** para verlo.
    
    ![Informe Todas las alertas](images/alerts-reports.png "Informe Todas las alertas")
    
6.  Revise el informe.
    
    *   El informe se filtra automáticamente para mostrar todas las alertas de todas las bases de datos de destino del compartimento seleccionado durante la última semana. Para crear filtros personalizados manualmente, puede utilizar el **Creador de consultas de SCIM**.
    *   Puede ver varios totales, incluido el número total de bases de datos de destino, el número total de alertas abiertas y cerradas y el número total de alertas críticas, altas, medias y bajas. Puede hacer clic en el total de **Destinos** para ver la lista de bases de datos de destino. Puede hacer clic en los otros totales para alternar un filtro en la lista de alertas.
    *   En la parte inferior del informe, puede ver la lista de alertas. Por defecto, la tabla muestra el nombre de la alerta, el estado de la alerta, la gravedad de la alerta, las bases de datos de destino en las que se ha producido el evento auditado y cuándo se ha creado la alerta.
    *   Tiene opciones para crear un informe PDF o XLS, crear un informe personalizado, programar un informe personalizado, abrir y cerrar alertas y especificar qué columnas de tabla desea que se muestren en la página.
    
    ![Informe Todas las alertas](images/all-alerts-report.png "Informe Todas las alertas")
    
7.  En la parte superior del informe, haga clic en **\+ Otro filtro**. Cree el filtro **Bases de datos de destino = nombre de base de datos de destino** y haga clic en **Aplicar**.
    
    En la tabla solo se muestran las alertas que pertenecen a la base de datos de destino.
    
8.  Haga clic en **\+ Otro filtro**. Cree el filtro **Nombre de alerta = Creación/Modificación de usuario** y haga clic en **Aplicar**.
    
    En la tabla solo se muestran las alertas relacionadas con la creación/modificación de usuarios.
    
9.  Revise las alertas generadas para **User Creation/Modification**.
    
    ![Alertas de creación/modificación de usuarios](images/alerts-user-creation-modification.png "Alertas de creación/modificación de usuarios")
    

## Tarea 5: Ver detalles de una alerta y cerrarla

1.  Haga clic en uno de los nombres de alerta para ver más detalles al respecto.
    
2.  Revise la siguiente información sobre la alerta:
    
    *   Nombre de la alerta (instancia de la alerta)
    *   Base de datos destino a la que se aplica la alerta
    *   gravedad de alerta
    *   Estado de alerta: si la alerta está abierta o cerrada
    *   Tipo de alerta: actualmente todos los tipos de alerta son AUDITING
    *   Política que genera la alerta
    *   Operación de usuario que ha generado la alerta
    *   Tiempo de operación y estado
    *   Cuando se ha creado y actualizado la alerta
    *   Identificador de Oracle Cloud (OCID) para la alerta
    *   compartimento en el que reside la descripción
    *   Detalles de la operación
    
    ![Página Detalles de Alerta](images/alert-details-page.png "Página Detalles de Alerta")
    
3.  Para cerrar la alerta, haga clic en **Cerrar**.
    
    El estado de alerta se establece inmediatamente en **CLOSED** (Cerrado).
    

## Tarea 6: Creación de un informe de alertas personalizado

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Todas las Alertas** para volver al informe Todas las Alertas.
    
2.  Además del filtro predeterminado que ya está establecido, agregue dos filtros más:
    
    *   **Bases de datos de destino = your-target-database-name**
    *   **Nombre de alerta = Conexiones fallidas por parte del usuario administrador**
3.  Haga clic en **Crear informe personalizado**.
    
    Se muestra el cuadro de diálogo **Crear informe personalizado**.
    
4.  En **Nombre mostrado**, introduzca **Inicios de sesión fallidos por usuario administrador para el nombre de base de datos de destino**. (Opcional) Introduzca una descripción. Seleccione el compartimento. Haga clic en **Crear informe personalizado** y espere a que se genere el informe.
    
    ![Cuadro de diálogo Crear informe personalizado](images/create-custom-report-dialog-box.png "Cuadro de diálogo Crear informe personalizado")
    
5.  Haga clic en el enlace **Haga clic aquí** para ver el informe.
    

## Tarea 7: Generar y descargar un informe de alertas personalizado como PDF

1.  En la página de informe personalizado, haga clic en **Generar informe**.
    
    Se muestra el cuadro de diálogo **Generar informe**.
    
2.  Deje **PDF** seleccionado.
    
3.  Introduzca el nombre mostrado **Conexiones de administrador con fallos para el nombre de la base de datos de destino**.
    
4.  (Opcional) En **Descripción**, introduzca **Conexiones fallidas por usuario administrador para la base de datos de destino su-nombre de base de datos de destino**.
    
5.  Deje el compartimento seleccionado, deje el límite de filas definido en 10000 y deje la hora de inicio del informe tal cual.
    
6.  Haga clic en **Generar informe** y espere hasta que se genere el informe PDF.
    
    Aparece un mensaje que indica que la generación del informe se ha completado.
    
    ![Cuadro de diálogo Generar informe](images/generate-report-dialog-box.png "Cuadro de diálogo Generar informe")
    
7.  Haga clic en el enlace **aquí** para descargar el informe.
    
8.  Si es necesario, elija guardar el informe en su computadora local.
    
9.  Abra el informe PDF y visualícelo. Cuando haya terminado, cierre el separador del explorador.
    
    ![Informe PDF de inicios de sesión de administrador fallidos](images/failed-admin-logins-report-pdf.png "Informe PDF de inicios de sesión de administrador fallidos")
    
10.  En el cuadro de diálogo **Generar informe**, haga clic en **Cerrar**.
    

## Tarea 8: Ver el historial del informe de alertas

1.  En **Recursos relacionados**, haga clic en **Historial de informes de alertas**.
    
2.  Observe que su informe personalizado aparece en la lista. Puede ver su estado, su descripción, cuándo se generó, si fue generado por usted (`GENERATED`) o por el programador, el formato de archivo disponible para descargar y un icono de descarga. Oracle Data Safe mantiene el informe disponible hasta tres meses.
    
    ![Historial de informes de alerta](images/alert-report-history.png "Historial de informes de alerta")
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Descripción general de alertas](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-37F8AC38-44D4-42D1-AE93-9775DCF21511)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023