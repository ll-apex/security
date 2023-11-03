# Reciba notificaciones sobre el cambio de seguridad en las bases de datos de destino configurando eventos de Oracle Data Safe

En este laboratorio, configurará el servicio Events para notificarle por correo electrónico cuando haya un cambio de seguridad en la base de datos de destino.

Tiempo de laboratorio estimado: 20 minutos

## Objetivos

En esta práctica de prácticas, aprenderá a:

*   Revisar la última evaluación de usuario
*   Definir la última evaluación de usuario como base
*   Crear una suscripción y un tema de notificación
*   Crear una regla en el servicio Events
*   Generar actividad en la base de datos destino
*   Refrescamiento de la última evaluación de usuario y análisis de los resultados
*   Generar un informe de comparación para la evaluación de usuario
*   Revise la notificación por correo electrónico

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Permisos de _administrador_ en su arrendamiento
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))
*   Una dirección de correo electrónico válida

### Supuestos

*   Los valores de datos pueden ser diferentes de los que se muestran en las capturas de pantalla.

## Tarea 1: Revisar la última evaluación del usuario

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Oracle Database** y, a continuación, **Data Safe - Seguridad de base de datos**.
    
2.  En **Centro de seguridad**, haga clic en **Evaluación de usuario**.
    
    Se muestra el panel de control Evaluación de usuario.
    
3.  Haga clic en el separador **Resumen de destino**.
    
4.  En la columna **Fecha de última evaluación**, haga clic en **Ver informe** para ver la última evaluación del usuario.
    
5.  Revise los gráficos en el separador **Visión general**.
    
    ![Separador Visión general de última evaluación de usuario](images/latest-ua-overview-tab.png "Separador Visión general de última evaluación de usuario")
    
6.  Desplácese hacia abajo y revise la información de la sección **Detalles del usuario**.
    
    ![Sección Detalles de usuario de última evaluación de usuario](images/latest-ua-user-details-section.png "Sección Detalles de usuario de última evaluación de usuario")
    

## Tarea 2: Definir la última evaluación de usuario como base

1.  Mientras sigue viendo la última evaluación del usuario, haga clic en **Definir como base**.
    
    Se muestra el cuadro de diálogo **¿Definir como base?** preguntando si está seguro.
    
2.  Haga clic en **Sí** y permanezca en la página hasta que aparezca el siguiente mensaje:
    
    `Baseline has been set.`
    
3.  Haga clic en **Ver historial**. Observe que en el compartimento tiene una evaluación base.
    
    ![Historial de evaluaciones, página](images/assessment-history.png "Historial de evaluaciones, página")
    

## Tarea 3: Creación de un tema de notificación y una suscripción

Para crear un tema de notificaciones, debe ser un administrador de arrendamiento.

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Servicios para desarrolladores** y, a continuación, en **Integración de aplicación**, seleccione **Notifications**.
    
    Se muestra la página **Notificaciones**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado.
    
3.  Haga clic en **Crear tema**.
    
    Se muestra el panel **Crear tema**.
    
4.  Introduzca un nombre de tema, por ejemplo, **security-drift**.
    
5.  Haga clic en **Crear**.
    
6.  Haga clic en el nombre del tema.
    
    Aparece la página **Detalles del Tema**.
    
7.  Haga clic en **Crear suscripción**.
    
    Se muestra el panel **Crear suscripción**.
    
8.  En **Protocolo**, deje **Correo electrónico** seleccionado.
    
9.  Introduzca su dirección de correo electrónico en **Correo electrónico**.
    
10.  Haga clic en **Crear**.
    
    El estado de la suscripción es **Pendiente**.
    
11.  Abra la aplicación de correo electrónico y localice el correo electrónico de Oracle. En el correo electrónico, haga clic en **Confirmar suscripción**.
    
    Se muestra una página **Suscripción confirmada** en el explorador.
    
12.  Refresque la página **Detalles del tema**. Observe que el estado de la suscripción ahora está definido en **Activo**.
    
    ![Suscripción activa](images/active-subscription.png "Suscripción activa")
    

## Tarea 4: Creación de una regla en el servicio Events

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Observability & Management** y, a continuación, **Events Service**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado.
    
3.  Haga clic en **Crear regla**.
    
4.  En **Nombre mostrado**, introduzca **Desplazamiento de seguridad**.
    
5.  En **Descripción**, introduzca **Enviar una notificación por correo electrónico cuando se compara una evaluación de usuario con una evaluación base y se encuentra una diferencia**.
    
6.  En la sección **Condiciones de regla**, deje **Tipo de evento** seleccionado como condición.
    
7.  En **Nombre de servicio**, seleccione **Data Safe**.
    
8.  En **Tipo de evento**, seleccione **Desplazamiento de evaluación de usuario desde base**.
    
9.  Haga clic en **Ver eventos de ejemplo (JSON)** y revise la lógica de regla. Esta es la información que recibirá en su correo electrónico. Haga clic en **Cancelar** para cerrar el panel.
    
10.  En **Tipo de acción**, seleccione **Notificaciones**.
    
11.  En **compartimento de notificaciones**, seleccione el compartimento.
    
12.  En **Tema**, seleccione el tema que acaba de crear (por ejemplo, **security-drift**).
    
    ![Página Creación de Reglas](images/create-rule-page.png "Página Creación de Reglas")
    
13.  Haga clic en **Crear regla**.
    
    Aparece la página **Security Drift** (Desplazamiento de seguridad).
    
    ![Cambio Seguridad, página](images/security-drift-page.png "Cambio Seguridad, página")
    

## Tarea 5: Generar actividad en la base de datos destino

En esta tarea, creará un usuario en la base de datos de destino con el rol `PDB_DBA`.

1.  Acceda a la hoja de trabajo de SQL en Database Actions. Si la sesión ha caducado, vuelva a conectarse como usuario `ADMIN`.
    
2.  Si es necesario, borre la hoja de trabajo y el separador **Salida de script**.
    
3.  En la hoja de trabajo, introduzca el siguiente comando:
    
        <copy>CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  En la barra de herramientas, haga clic en el botón **Ejecutar sentencia** (círculo verde con flecha blanca).
    
    ![Botón Ejecutar extracto](images/run-statement-button.png "Botón Ejecutar extracto")
    

## Tarea 6: Refrescar la última evaluación de usuario y analizar los resultados

1.  Vuelva al separador del explorador de Oracle Cloud Infrastructure.
    
2.  En el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Seguridad de base de datos**.
    
3.  En **Centro de seguridad**, haga clic en **Evaluación de usuario**.
    
4.  Haga clic en el separador **Resumen de destino**.
    
5.  Haga clic en **Ver informe** para ver la última evaluación del usuario.
    
6.  En la parte superior de la última evaluación del usuario, haga clic en **Refrescar ahora** para obtener los datos más recientes.
    
    Se muestra el panel **Actualizar ahora**.
    
7.  Deje el nombre de la evaluación por defecto tal cual y haga clic en **Refrescar ahora**. Espere a que el estado sea **SUCCEEDED**.
    
    *   Esta acción actualiza los datos de la última evaluación de usuario para la base de datos de destino y también guarda una copia de la evaluación en el historial de evaluaciones.
    *   La operación de refrescamiento tarda aproximadamente un minuto.
8.  Haga clic en **Ver historial**.
    
9.  Compare los valores de riesgo entre la evaluación base y la nueva evaluación que acaba de generar. ¿Hay diferencias?
    
    ![Historial de evaluaciones después de](images/assessment-history-after.png "Historial de evaluaciones después de")
    
10.  Haga clic en **Cerrar**.
    

## Tarea 7: Generación de un informe de comparación para la evaluación de usuario

Después de generar un informe de comparación, si hay un cambio de seguridad (que debería haber porque ha agregado un usuario con privilegios), el servicio Events debe disparar una notificación por correo electrónico.

1.  Con la última evaluación de usuario mostrada, en **Recursos** a la izquierda, haga clic en **Comparar con base**. Oracle Data Safe inicia automáticamente el procesamiento de la comparación.
    
2.  Una vez finalizada la operación de comparación, revise el informe **Comparación**. Haga clic en **Abrir detalles** para ver más información.
    
    ![Panel de detalles de comparación](images/comparison-details-panel.png "Panel de detalles de comparación")
    

## Tarea 8: Revisar la notificación por correo electrónico

1.  Abra la aplicación de correo electrónico.
    
2.  Localice y abra la notificación de correo electrónico de Oracle. El mensaje contiene texto similar al siguiente:
    
        <copy>{
        "eventType" : "com.oraclecloud.datasafe.userassessmentdriftfrombaseline",
        "cloudEventsVersion" : "0.1",
        "eventTypeVersion" : "2.0",
        "source" : "DataSafe",
        "eventTime" : "2023-02-16T19:54:52Z",
        "contentType" : "application/json",
        "data" : {
            "compartmentId" : "ocid1.compartment.oc1...",
            "compartmentName" : "compartment-name",
            "resourceName" : "userAssessment",
            "resourceId" : "not applicable",
            "availabilityDomain" : "ad3",
            "additionalDetails" : {
            "targetName" : "ATP2000",
            "comparedWith" : "ocid1.datasafeuserassessment.oc1..."
            }
        },
        "eventID" : "e46fad7b-ac11...",
        "extensions" : {
            "compartmentId" : "ocid1.compartment.oc1..."
        }
        }
        
        --
        You are receiving notifications as a subscriber to the topic: security-drift (Topic OCID: ocid1.onstopic.oc1.eu-frankfurt-1.aaaaa...). 
        To stop receiving notifications from this topic, unsubscribe: https://cell1.notification.eu-frankfurt-1.oci.oraclecloud.com/20181201/subscriptions/ocid1.onssubscription.oc1.eu-frankfurt-1.aaaaa.../unsubscription?token=YVpsOE4weTU4TTdKSGxoTkVwR3kyaU8...==&protocol=EMAIL
        
        Please do not reply directly to this email. If you have any questions or comments regarding this email, contact your account administrator.
        
        Oracle Corporation - Worldwide Headquarters
        2300 Oracle Way, Austin, Texas 78741 USA</copy>
        

## Más información

*   [Tipos de eventos para Oracle Data Safe](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-A8D65EBC-9A53-43EC-B335-0DA0E2F9CDC8)
*   [Eventos en Oracle Cloud Infrastructure](https://docs.oracle.com/en-us/iaas/Content/Events/home.htm)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Contribuyentes** - Bettina Schaeumer
*   **Última actualización por/fecha**: Jody Glover, 11 de abril de 2023