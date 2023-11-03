# Auditoría de actividad de base de datos

## Introducción

En Oracle Data Safe, puede aprovisionar políticas de auditoría en las bases de datos de destino y recopilar datos de auditoría en el repositorio de Oracle Data Safe. Hay políticas de auditoría básicas, de administrador, de usuario, predefinidas y personalizadas de Oracle, así como políticas de auditoría diseñadas para ayudar a su organización a cumplir los estándares de conformidad. Al registrar una base de datos de destino, Oracle Data Safe crea automáticamente un perfil de auditoría, una política de auditoría y pistas de auditoría relevantes para la base de datos de destino.

Para empezar, revise la configuración global en Oracle Data Safe. A continuación, revise el perfil de auditoría, las pistas de auditoría y la política de auditoría que se crean automáticamente para la base de datos de destino. Inicie la recopilación de datos de auditoría en la base de datos de destino y aprovisione algunas políticas de auditoría. Analice los eventos de auditoría, vea informes, cree un informe de auditoría personalizado y descargue el informe de auditoría personalizado como PDF.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Revisión de la configuración global para Oracle Data Safe
*   Revisar el perfil de auditoría de la base de datos destino
*   Revisar la política de auditoría de la base de datos de destino
*   Revise las pistas de auditoría de la base de datos destino
*   Ver la cantidad de registros de auditoría disponibles en la base de datos de destino para las pistas de auditoría detectadas
*   Iniciar recopilación de datos de auditoría
*   Revisar el panel de control Auditoría de actividad
*   Aprovisionamiento de políticas de auditoría en la base de datos de destino
*   Analizar los eventos de auditoría de la base de datos destino
*   Ver el informe Todas las actividades
*   Creación de un informe de auditoría personalizado
*   Generar y descargar un informe de auditoría personalizado como PDF
*   Ver el historial de informes de auditoría
*   Programe su informe de auditoría personalizado

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud
*   Conexión a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

### Supuestos

*   Los valores de datos pueden ser diferentes de los que se muestran en las capturas de pantalla.

## Tarea 1: Revisar la configuración global de Oracle Data Safe

1.  Acceda a la página **Visión general** de Oracle Data Safe haciendo clic en **Data Safe** en la ruta de navegación de la parte superior de la página.
    
2.  En **Data Safe**, haga clic en **Configuración**.
    
3.  Revise la configuración global.
    
    *   Cada servicio regional de Oracle Data Safe en un arrendamiento tiene una configuración global para el uso de pagos, el período de retención en línea y el período de retención de archivos.
    *   La configuración global se aplica a todas las bases de datos de destino a menos que sus perfiles de auditoría las sustituya.
    *   Por defecto, el uso de pagos está activado para todas las bases de datos de destino, el período de retención en línea se define en el valor máximo de 12 meses y el período de retención del archivo se define en el valor mínimo de 0 meses. Tenga en cuenta que no puede activar el uso de pago para una cuenta de prueba gratuita.
    
    ![Configuración global](images/global-settings.png "Configuración global")
    

## Tarea 2: Revisar el perfil de auditoría de la base de datos destino

1.  En la ruta de navegación, haga clic en **Data Safe**.
    
2.  En **Security Center** de la izquierda, haga clic en **Activity Auditing**.
    
3.  En **Recursos relacionados**, haga clic en **Perfiles de auditoría**.
    
4.  En la lista desplegable **Compartimento** en **Ámbito de lista**, asegúrese de que el compartimento está seleccionado.
    
5.  A la derecha, revise la información del perfil de auditoría sobre la base de datos de destino y, a continuación, haga clic en el nombre de la base de datos de destino para ver más detalles.
    
    ![Perfil de Auditoría, página](images/audit-profiles-page.png "Perfil de Auditoría, página")
    
6.  Revise los detalles en el perfil de auditoría.
    
    *   Hay valores por defecto para el uso de pago, el período de retención en línea y el período de retención fuera de línea.
    *   Todos los valores de perfil de auditoría inicial de la base de datos de destino se heredan de los valores globales de Oracle Data Safe, pero puede modificarlos aquí según sea necesario.
    
    ![Detalles de Perfil de Auditoría, página](images/audit-profile-details-page.png "Detalles de Perfil de Auditoría, página")
    

## Tarea 3: Revisar las pistas de auditoría de la base de datos de destino

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En la parte izquierda, en **Related Resources**, haga clic en **Audit Trails**.
    
3.  En **Ámbito de lista**, a la izquierda, asegúrese de que está seleccionado el compartimento.
    
4.  En **Filtros** a la izquierda, seleccione la base de datos de destino.
    
5.  A la derecha, revise las pistas de auditoría de la base de datos de destino. Oracle Data Safe detecta una pista de auditoría para una instancia de Autonomous Database denominada `UNIFIED_AUDIT_TRAIL`.
    
    ![Pistas de Auditoría, página](images/audit-trails-page.png "Pistas de Auditoría, página")
    
6.  Haga clic en el nombre de la base de datos de destino para una de las pistas de auditoría y revise la información de la página **Detalles de pista de auditoría**. Aquí es donde puede gestionar la recopilación de datos de auditoría para la pista de auditoría. Observe que la pista de auditoría está actualmente inactiva.
    
    ![Detalles de Pista de Auditoría, página](images/audit-trail-details-page.png "Detalles de Pista de Auditoría, página")
    

## Tarea 4: Revisar la política de auditoría de la base de datos de destino

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En **Recursos relacionados**, haga clic en **Políticas de auditoría**.
    
3.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
4.  En la lista desplegable **Target Databases** de la izquierda, seleccione la base de datos destino.
    
5.  A la derecha, revise la información proporcionada para la política de auditoría de la base de datos de destino. Tenga en cuenta que solo la categoría **Políticas adicionales** tiene políticas activadas, lo que se indica con un círculo verde con una marca de control. Se trata de políticas predefinidas de Oracle activadas por defecto en una base de datos de Autonomous Transaction Processing.
    
    ![Página Políticas de Auditoría](images/audit-policies-page.png "Página Políticas de Auditoría")
    
6.  Haga clic en el nombre de la base de datos de destino para ver más detalles en la página **Detalles de Política de Auditoría**. Desplácese hacia abajo y revise la lista de políticas de auditoría disponibles para la base de datos de destino.
    
    *   Un círculo gris significa que la política de auditoría aún no se ha aprovisionado en la base de datos de destino. Un círculo verde significa que la política de auditoría está aprovisionada.
    *   Puede optar por aprovisionar y activar cualquier número de políticas de auditoría en la base de datos de destino y definir filtros en usuarios y roles.
    
    ![Página Detalles de Políticas de Auditoría](images/audit-policies-details-page.png "Página Detalles de Políticas de Auditoría")
    

## Tarea 5: Ver la cantidad de registros de auditoría disponibles en la base de datos de destino para las pistas de auditoría detectadas

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En la parte izquierda, en **Recursos relacionados**, haga clic en **Perfiles de auditoría**.
    
3.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
4.  En la lista desplegable **Target Databases** de la izquierda, seleccione la base de datos destino.
    
5.  A la derecha, haga clic en el nombre de la base de datos de destino.
    
6.  Desplácese hacia abajo hasta la sección **Calcular volumen de auditoría** y haga clic en **Disponible en la base de datos de destino**.
    
    Se muestra el cuadro de diálogo **Calcular volumen disponible**.
    
7.  Para la fecha de inicio, haga clic en el widget de calendario y seleccione la fecha actual a las 00:00 UTC. Seleccione la fecha actual porque la base de datos de destino es nueva.
    
8.  En la lista desplegable **Ubicaciones de pista**, seleccione `UNIFIED_AUDIT_TRAIL`.
    
9.  Haga clic en **Recursos informáticos** y espere a que Oracle Data Safe calcule el volumen de auditoría disponible.
    
    ![Cuadro de diálogo Compute Available Volume (Calcular volumen disponible)](images/compute-available-volume-dialog-box.png "Cuadro de diálogo Compute Available Volume (Calcular volumen disponible)")
    
10.  En la columna **Disponible en base de datos de destino**, visualice el número de registros de auditoría para `UNIFIED_AUDIT_TRAIL`.
    
    *   En nuestro caso, el número de registros en `UNIFIED_AUDIT_TRAIL` es pequeño porque la base de datos de destino se acaba de aprovisionar. Sin embargo, para una base de datos de destino más antigua, es probable que haya un gran número de registros de auditoría.
    *   Oracle Data Safe divide los números por mes. Estos valores le ayudan a decidir una fecha de inicio para la pista de auditoría de Oracle Data Safe.
    *   No se preocupe si el número de registros de auditoría del sistema es diferente al que se muestra a continuación.
    
    ![Disponible en la columna Base de Datos de Destino](images/available-in-target-database.png "Disponible en la columna Base de Datos de Destino")
    

## Tarea 6: Iniciar recopilación de datos de auditoría

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En la parte izquierda, en **Related Resources**, haga clic en **Audit Trails**.
    
3.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
4.  En la lista desplegable **Target Databases** de la izquierda, seleccione la base de datos destino.
    
5.  A la derecha, haga clic en el nombre de la base de datos de destino para `UNIFIED_AUDIT_TRAIL`.
    
    Aparece la página **Detalles de pista de auditoría**.
    
6.  Haga clic en **Iniciar**.
    
    Se muestra el cuadro de diálogo **Start Audit Trail: UNIFIED\_AUDIT\_TRAIL**.
    
7.  Configure una fecha de inicio basada en los datos de la región **Compute Audit Volume** del perfil de auditoría que visualizó en la tarea 5 (paso 10). Por ejemplo, si tiene un mes en la lista (febrero de 2023), puede establecer la fecha de inicio a principios de febrero.
    
    ![Cuadro de diálogo Iniciar pista de auditoría](images/start-audit-trail-dialog-box.png "Cuadro de diálogo Iniciar pista de auditoría")
    
8.  Haga clic en **Iniciar**. Espere a que el **estado de recopilación** cambie de **STARTING** a **COLLECTING** y, a continuación, a **IDLE**. Se tarda aproximadamente un minuto.
    
    ![Estado de recopilación inactivo](images/collection-state-idle.png "Estado de recopilación inactivo")
    

## Tarea 7: Revisar el panel de control Auditoría de actividad

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
    Por defecto, en el panel de control de Auditoría de actividad se muestra un resumen de los eventos de auditoría de la última una semana para todas las bases de datos de destino en forma de gráficos y tablas. A la izquierda, en **Ámbito de lista** y **Filtros**, puede filtrar por compartimento, período de tiempo y base de datos de destino.
    
2.  En la lista desplegable **Compartimentos** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
3.  En la lista desplegable **Bases de Datos Destino** de la izquierda, seleccione la base de datos de destino. El panel de control se actualiza automáticamente para incluir estadísticas de eventos de auditoría solo para la base de datos de destino.
    
4.  Revise los gráficos.
    
    *   El gráfico **Actividad de Conexión Fallida** muestra el número de conexiones fallidas en la base de datos de destino durante la última semana. Puede que tenga o no conexiones fallidas, según cómo haya interactuado en Database Actions hasta ahora.
    *   En el gráfico **Actividad de administración** se muestra el número de cambios de esquema de base de datos, conexiones, cambios de configuración de auditoría y cambios de derechos en la base de datos de destino durante la última semana.
    *   El gráfico **Todas las actividades** muestra el recuento total de eventos de auditoría en la base de datos de destino para el período de tiempo especificado.
    
    ![Gráficos iniciales del panel de control de Auditoría de actividad](images/activity-auditing-dashboard-charts-initial.png "Gráficos iniciales del panel de control de Auditoría de actividad")
    
5.  En el separador **Resumen de eventos**, revise las estadísticas de las categorías de eventos de auditoría.
    
    Las estadísticas incluyen el número de bases de datos de destino que tienen un evento de auditoría en cada categoría de evento y el número total de eventos por categoría. Puesto que sólo está viendo estadísticas para la base de datos de destino, la columna **Bases de Datos de Destino** muestra las que hay.
    
    ![Separador Resumen de eventos del panel de control Auditoría de actividad](images/activity-auditing-events-summary-tab.png "Separador Resumen de eventos del panel de control Auditoría de actividad")
    
6.  Haga clic en el separador **Resumen de Destino** y revise los distintos recuentos de eventos de auditoría por base de datos de destino.
    
    Los eventos de auditoría incluyen el número de fallos de conexión, los cambios de esquema, los cambios de derechos, los cambios de configuración de auditoría, toda la actividad (todos los eventos de auditoría), las violaciones de Database Vault y los cambios de política de Database Vault.
    
    ![Separador Resumen de destino del panel de control Auditoría de actividad](images/activity-auditing-dashboard-target-summary-tab.png "Separador Resumen de destino del panel de control Auditoría de actividad")
    

## Tarea 8: Aprovisionar políticas de auditoría

1.  En **Recursos relacionados**, haga clic en **Políticas de auditoría**.
    
2.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
3.  En la lista desplegable **Target Databases** de la izquierda, seleccione la base de datos destino.
    
4.  A la derecha, haga clic en el nombre de la base de datos de destino.
    
5.  Tenga en cuenta que las siguientes políticas de auditoría personalizadas están aprovisionadas en la base de datos de destino, pero aún no están activadas:
    
    *   `APP_USER_NOT_APP_SERVER`
    *   `EMPSEARCH_SELECT_USAGE_BY_PETE`
    *   `ADB_SAAS_ADMIN_AUDIT`
    *   `EMP_RECORD_CHANGES`
6.  Haga clic en **Update and Provision**.
    
    Se muestra el panel **Aprovisionar políticas de auditoría**.
    
7.  Seleccione **Excluir actividad de usuarios de Data Safe**.
    
8.  En **Auditoría básica**, seleccione **Cambios de esquema de base de datos** y **Actividad de base de datos crítica**.
    
9.  En **Auditoría de actividad de administrador**, seleccione **Actividad de usuario de administrador**.
    
10.  En **Políticas personalizadas**, seleccione **APP\_USER\_NOT\_APP\_SERVER**.
    
11.  Haga clic en **Actualizar y Aprovisionar** para aprovisionar las políticas seleccionadas en la base de datos de destino.
    
    ![Panel Provisionar Políticas de Auditoría](images/provision-audit-policies-panel.png "Panel Provisionar Políticas de Auditoría")
    
12.  Espere a que finalice el aprovisionamiento y, a continuación, vea la información de política actualizada en la página. Observe que las políticas que ha activado ahora tienen círculos verdes.
    

![Políticas activadas](images/enabled-policies.png "Políticas activadas")

## Tarea 9: Análisis de los eventos de auditoría de la base de datos de destino

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En la lista desplegable **Target Databases** de la izquierda, seleccione la base de datos destino.
    
    El panel de control se actualiza automáticamente para incluir estadísticas de eventos de auditoría para la base de datos de destino. ¿Se nota alguna diferencia en los números?
    
    ![Gráficos del panel de control Auditoría de actividad después de aprovisionar políticas](images/activity-auditing-dashboard-charts-afterprovision.png "Gráficos del panel de control Auditoría de actividad después de aprovisionar políticas") ![Tabla del panel de control Auditoría de actividad después de aprovisionar políticas](images/activity-auditing-dashboard-table-afterprovision.png "Tabla del panel de control Auditoría de actividad después de aprovisionar políticas")
    
3.  Observa que hay cambios de esquema. Para investigar, en el separador **Resumen de eventos**, haga clic en **Cambios de esquema por administrador** para ver más detalles.
    
4.  En la página **Cambios de esquema por administrador**, revise lo siguiente:
    
    *   Filtros definidos en la parte superior de la página. Hay dos filtros definidos en **Tiempo de operación**, que definen el período de tiempo de la última semana. Hay un filtro definido en **ID de destino**, que define la base de datos de destino en la base de datos.
    *   Número total de destinos, usuarios de base de datos, hosts de cliente, sentencias `CREATE`, sentencias `ALTER` y sentencias `DROP`
    *   Número total de eventos
    *   Los eventos de auditoría individuales
    
    ![Página Cambios de Esquema por Administrador](images/schema-changes-by-admin-page.png "Página Cambios de Esquema por Administrador")
    
5.  Haga clic en la flecha hacia abajo al final de cualquier fila de la tabla de eventos para ver más detalles sobre el evento. Al hacer clic en la flecha hacia abajo, cambia a una flecha hacia arriba.
    
    ![Expansor de tabla de eventos de auditoría](images/audit-event-table-expander.png "Expansor de tabla de eventos de auditoría")
    
6.  ¿Qué se ha emitido el SQL?
    
    Respuesta: Desplácese hacia abajo hasta la línea de ítem **Texto SQL**. Aquí puede elegir mostrar el SQL o copiarlo. El SQL emitido fue el siguiente:
    
        <copy>drop function HCM1.return_condition</copy>
        

## Tarea 10: Ver el informe Todas las actividades

Por defecto, el informe All Activity muestra los eventos de auditoría de la última semana para todas las bases de datos de destino en los compartimentos seleccionados.

1.  En **Recursos relacionados**, haga clic en **Informes de auditoría**. Oracle Data Safe tiene los siguientes informes de auditoría predefinidos:
    
    *   Toda la actividad
    *   Actividad de administración
    *   Cambios de derechos/usuario
    *   Cambios de política de auditoría
    *   Actividad de Conexión
    *   Acceso a datos
    *   Modificación de datos
    *   Cambios del esquema de base de datos
    *   Actividad de Data Safe
    *   Actividad de Database Vault
    *   Actividad de usuario común
    *   Errores de base de datos
    *   Actividad de extracción de datos
    *   Actividad de datos confidenciales
    
    ![Página Informes de Auditoría](images/audit-reports-page.png "Página Informes de Auditoría")
    
2.  Asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
3.  Haga clic en el informe **Todas las actividades** para verlo.
    
4.  Ver los filtros definidos en el informe.
    
    *   Por defecto, el informe se filtra para mostrar los eventos de auditoría de la última semana para todas las bases de datos de destino de los compartimentos seleccionados.
    *   Puede crear filtros adicionales según sea necesario.
5.  Consulte los totales del informe.
    
    *   Puede hacer clic en **Destinos**, **Usuarios de Base de Datos** y **Hosts de Cliente** para ver la lista de destinos, usuarios de base de datos y hosts de cliente respectivamente.
    *   Si hace clic en **DML**, **Cambios de Privilegios**, **DDL**, **Cambios de Usuario/Derecho**, **Fallos de Conexión**, **Login Successes** o **Total de Eventos**, la tabla de eventos de auditoría se filtra en consecuencia.
6.  Desplácese hacia abajo y vea los eventos de auditoría individuales.
    
7.  Para ver más detalles de un evento de auditoría concreto, haga clic en la flecha abajo para ampliar la fila y mostrar los detalles del evento concreto. Para obtener más información, puede copiar sus valores en el portapapeles.
    
    ![Informe de toda la actividad](images/all-activity-report.png "Informe de toda la actividad")
    

## Tarea 11: Creación de un informe de auditoría personalizado

1.  En la parte superior del informe **Todas las actividades**, agregue los dos filtros siguientes. Para agregar un filtro, haga clic en **\+ Otro filtro**. Cuando haya terminado de definir los parámetros de filtro, haga clic en **Aplicar**.
    
    *   **Target = nombre de la base de datos de destino**
    *   **Propietario de objeto = HCM1**
2.  Haga clic en **Gestionar columnas**. En el panel **Gestionar columnas**, seleccione las columnas **Destino**, **Usuario de base de datos**, **Evento**, **Objeto**, **Tiempo de operación** y **Políticas de auditoría unificadas**. Haga clic en **Apply Changes** (Aplicar cambios).
    
    La tabla muestra las columnas seleccionadas. Observe también que los totales también se ajustan.
    
    ![Informe de toda la actividad](images/custom-audit-report3.png "Informe de toda la actividad")
    
3.  Haga clic en **Crear informe personalizado**.
    
    Se muestra el cuadro de diálogo **Crear informe personalizado**.
    
4.  Introduzca el nombre mostrado **Todos los informes de actividad en el esquema: HCM1 en el nombre de base de datos de destino**. Introduzca una descripción opcional. Seleccione el compartimento, si es necesario. Haga clic en **Crear informe personalizado** y espere a que se genere el informe.
    
    ![Cuadro de diálogo Crear informe personalizado](images/create-custom-report-dialog-box.png "Cuadro de diálogo Crear informe personalizado")
    
5.  En el cuadro de diálogo **Crear informe personalizado**, haga clic en el enlace **Haga clic aquí** para navegar hasta el informe personalizado.
    
    *   Si necesita modificar el informe personalizado, puede hacer clic en **Guardar informe** para guardar los cambios.
    *   Para ver el informe personalizado en el futuro, en **Recursos relacionados** para **Auditoría de actividad**, haga clic en **Informes de auditoría**. Haga clic en el separador **Informes personalizados** y, a continuación, en el nombre del informe de auditoría personalizado.

## Tarea 12: Generar y descargar un informe de auditoría personalizado como PDF

1.  En la página de informe de auditoría personalizado, haga clic en **Generar informe**.
    
    Se muestra el cuadro de diálogo **Generar informe**.
    
2.  Deje **PDF** seleccionado.
    
3.  Introduzca el nombre mostrado **Informe de Todas las Actividades en el Esquema: HCM1 en el nombre de la base de datos de destino**.
    
4.  (Opcional) Introduzca una descripción.
    
5.  Asegúrese de que el compartimento está seleccionado.
    
6.  Deje la hora de inicio del informe tal cual.
    
7.  Haga clic en **Generar informe** y espere hasta que se genere el informe PDF. Se muestra un mensaje que indica que la generación del informe ha finalizado.
    
    ![Generar PDF de informe de auditoría personalizado](images/generate-pdf-custom-audit-report.png "Generar PDF de informe de auditoría personalizado")
    
8.  Haga clic en el enlace **aquí** para descargar el informe.
    
9.  Si se le solicita que abra o guarde el informe, elija guardar.
    
10.  Cierre el cuadro de diálogo **Generate Report** (Generar informe).
    
11.  Abrir el informe PDF y verlo.
    

![Informe PDF de todas las actividades](images/all-activity-report-pdf.png "Informe PDF de todas las actividades")

12.  Para cerrar el informe PDF, cierre el separador del explorador.
    
13.  Para cerrar el cuadro de diálogo **Generar informe**, haga clic en **Cerrar**.
    

## Tarea 13: Visualización del historial de informes de auditoría

1.  En **Recursos relacionados**, haga clic en **Historial de informes de auditoría**.
    
2.  Vea los detalles del informe personalizado. En esta página, puede hacer clic en el nombre de un informe para ver sus detalles y descargarlo como un documento PDF o XLS (en función de cómo lo haya generado originalmente). Oracle Data Safe mantiene el historial de informes de auditoría durante un máximo de tres meses.
    
    ![Historial para informe personalizado](images/history-custom-report.png "Historial para informe personalizado")
    
3.  En la columna **Nombre del informe**, haga clic en el nombre del informe personalizado para ver sus detalles.
    
    ![Detalles del informe personalizado](images/custom-report-details.png "Detalles del informe personalizado")
    

## Tarea 14: Programar el informe de auditoría personalizado

Programe su informe de auditoría personalizado para generar un PDF todos los domingos a las 11 p. m. UTC.

1.  En la ruta de navegación, seleccione **Auditoría de actividad**.
    
2.  En **Recursos relacionados**, haga clic en **Informes de auditoría**.
    
3.  A la derecha, haga clic en el separador **Informes personalizados**.
    
4.  En la columna **Nombre del informe** de la tabla, haga clic en el nombre del informe personalizado.
    
    Se muestra el informe personalizado.
    
5.  Haga clic en **Gestionar programación de informe**.
    
    Se muestra el panel **Gestionar programa de informes**.
    
6.  Introduzca un nombre de programa, por ejemplo, **Todas las actividades HCM1 en el programa de nombre de base de datos**.
    
7.  Asegúrese de que el compartimento está seleccionado.
    
8.  Deje **PDF** seleccionado como formato de informe.
    
9.  En **Frecuencia de programación**, seleccione **Semanal**.
    
10.  En **Cada**, seleccione **Domingo**.
    
11.  En **Hora (en UTC)**, seleccione **11 PM**.
    
12.  Para **Intervalo de tiempo de eventos**, deje **Últimos días** y **7** tal cual, para que solo se muestren datos de una semana en el informe.
    

![Panel Gestionar programación de informe](images/manage-report-schedule-panel.png "Panel Gestionar programación de informe")

13.  Haga clic en **Save Schedule**.
    
    El panel se cierra y volverá al informe personalizado.
    
14.  Para ver el programa, en **Recursos relacionados**, haga clic en **Informes de auditoría**. A la derecha, haga clic en el separador **Informes personalizados**. Observe que ahora hay un programa de informes para el informe personalizado. Puede acceder a los informes generados por el programa en la página **Historial de informes de auditoría**.
    
    ![Informe personalizado con programa](images/custom-report-w-schedule.png "Informe personalizado con programa")
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Visión general de Auditoría de actividad](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-A73D8630-E59F-44C3-B467-F8E13041A680)
*   [Visualización y gestión de informes de auditoría](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-364B6431-9861-4B42-B24D-103D5F43B44A)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023