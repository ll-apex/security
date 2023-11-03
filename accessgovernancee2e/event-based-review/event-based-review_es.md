# Gestión de la revisión de acceso basada en eventos

## Introducción

Los usuarios con el rol de **administrador de campaña** pueden crear y gestionar campañas de revisión de acceso basadas en eventos mediante la consola de **Oracle Access Governance**. Los usuarios pueden ver el progreso de las **campañas en curso**, ver y descargar informes detallados de análisis de campañas, clonar **campañas anteriores**, finalizar **campañas en curso**, etc.

*   Tiempo estimado: 10 minutos
*   Persona: Administrador de campaña

Vea el siguiente vídeo para una breve introducción al laboratorio. [Gestión de campañas de revisión basadas en eventos](videohub:1_1azcpenj)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Muestra una lista de **campañas de certificación** de las que es propietario o que ha creado
*   Vea el progreso de las **campañas de certificación** realizadas por los revisores con **información analítica**

## Tarea 1: Conexión a Oracle Access Governance como administrador de campaña

1.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **ag-domain**.
2.  Inicie sesión en **Oracle Access Governance** como **administrador de campaña - Pamela Green** con el nombre de usuario y la contraseña que se mencionan a continuación.

**Nombre de usuario:** `<copy>pamela.green</copy>`

**Contraseña:** `<copy>Oracl@123456</copy>` ![Conexión a Access Governance](images/admin-login.png)

3.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/event-based-setup.png)

## Tarea 2: Activar campañas de revisión de acceso basadas en eventos

1.  Seleccione Administración basada en eventos → Configuración basada en eventos en el menú de navegación. ![Configuración basada en eventos](images/event-based-setup.png)
    
2.  Cada tipo de evento se muestra como un mosaico con el estado Activado o Desactivado y el menú desplegable Acciones, que proporciona la opción Editar o Ver detalles. Seleccione Editar para el tipo de evento **Identidad activada**. ![Editar identidad activada](images/edit-identity-enabled.png)
    
3.  En la pantalla Configure the event type: utilice el botón de radio para activar el event-type. Si desea aprobar automáticamente esta tarea para este tipo de evento, seleccione Sí.
    
4.  El servicio Oracle Access Governance proporciona un flujo de trabajo óptimo sugerido para el tipo de evento. Puede seleccionar Guardar para aceptar el flujo de trabajo sugerido.
    
5.  En la pantalla **Configurar el tipo de evento**. Seleccione **Guardar** para mantener los cambios en la configuración de tipo de evento.
    

![Activar finalización](images/enable-complete.png)

## Tarea 3: Desactivación del usuario en Oracle Identity Governance

1.  Conéctese a la consola de autoservicio de Identity. Abra un separador del explorador con la siguiente URL para acceder a la consola de identidad de OIG.

**URL:** `<copy>http://oimhost.us.oracle.com:14000/identity</copy>` **Nombre de usuario:** `<copy>xelsysadm</copy>`

**Contraseña:** `<copy>Welcome1</copy>`

![Inicio de sesión de OIG](images/oig-login-page.png)

2.  Debe ver el panel de control principal de **Oracle Identity Governance** ![Autoservicio de OIG](images/self-service.png)
    
3.  Haga clic en Administrar en la esquina superior derecha. A continuación, haga clic en Users.Select **Nombre mostrado** e introduzca el nombre de usuario **Roger Young**. Se muestra el perfil de usuario de **Roger Young**.
    

![Gestionar autoservicio](images/manage-self-service.png) 4. Haga clic en el botón **Desactivar** para desactivar la identidad del usuario **Roger Young**

![Desactivar usuario](images/disable-user.png)

5.  Proporcione la justificación y haga clic en _Submit_.

![Enviar desactivación de usuario](images/disable-submit.png)

6.  Refresque la página; ahora verá que el usuario **Roger Young** tiene el estado desactivado.

![Usuario desactivado](images/user-disabled.png)

## Tarea 4: Carga de datos en la consola de OAG

1.  En la consola de Oracle Access Governance, acceda al menú de navegación seleccionando el icono del menú de navegación. Seleccione **Administración de servicios → Sistemas conectados**.
    
    ![Carga de datos](images/data-load.png)
    
2.  En la pantalla **Sistemas conectados**, seleccione el botón **Gestionar** del sistema conectado de Oracle Access Governance que desea gestionar.
    
3.  Seleccione la opción **Cargar datos ahora** del menú desplegable **Acciones** en la esquina superior derecha. Esto iniciará una carga de datos de la que puede realizar un seguimiento del estado en el **log de actividad.** Pantalla Refrescar y esperar a que el estado sea **Correcto**
    

## Tarea 5: Activación del usuario en Oracle Identity Governance

1.  Conéctese a la consola de autoservicio de Identity. Abra un separador del explorador con la siguiente URL para acceder a la consola de identidad de OIG.
    
    **Dirección URL:**
    
        <copy>http://oimhost.us.oracle.com:14000/identity</copy>
        
    
    **Nombre de usuario:**
    
        <copy>xelsysadm</copy>
        
    
    **Contraseña:**
    
        <copy>Welcome1</copy>
        

![Página de Conexión de OIG](images/oig-login-page.png) 2. Debe ver el panel de control principal de **Oracle Identity Governance**. Haga clic en Administrar en la esquina superior derecha. A continuación, haga clic en Users.Select **Nombre mostrado** e introduzca el nombre de usuario **Roger Young**. El perfil de usuario de **Roger Young** es displayed.The; el usuario tiene el estado **Disabled** (Desactivado).

![Activar usuario](images/enable-user.png) 3. Haga clic en el botón **Activar** para activar la identidad de usuario **Roger Young**. Proporcione la justificación y haga clic en _Enviar_ 4. Refresque la página; ahora verá que el usuario **Roger Young** tiene el estado **Active**.

## Tarea 6: Volver a realizar la carga de datos en la consola de OAG

1.  En la consola de Oracle Access Governance, acceda al menú de navegación seleccionando el icono del menú de navegación. Seleccione **Administración de servicios → Sistemas conectados**.
    
    ![Carga de datos](images/data-load.png)
    
2.  En la pantalla **Sistemas conectados**, seleccione el botón **Gestionar** del sistema conectado de Oracle Access Governance que desea gestionar.
    
3.  Seleccione la opción **Cargar datos ahora** del menú desplegable **Acciones** en la esquina superior derecha. Esto iniciará una carga de datos de la que puede realizar un seguimiento del estado en el **log de actividad.** Pantalla Refrescar y esperar a que el estado sea **Correcto**
    

## Tarea 7: Conexión a Oracle Access Governance como administrador de campaña

1.  En la consola de Oracle Access Governance, acceda al menú de navegación seleccionando el icono del menú de navegación. Seleccione **Mis revisiones de acceso**.
    
    ![Revisión de My Access](images/my-access-review.png)
    
2.  Puede ver el evento de certificación que la identidad de Roger acaba de activar.
    
    ![Aceptar revisión de acceso](images/accpet-review.png)
    
3.  **¡Felicidades!** Ahora finalizará el **Laboratorio práctico sobre Oracle Access Governance**. En este taller, ha aprendido a:
    
    *   Creación de campañas de revisión de acceso como **administrador de campañas**
    *   Revise los privilegios de usuario para usted y sus subordinados directos como **mánager de usuarios**.
    *   Realizar tareas de revisión de acceso como **usuario empleado** y **mánager de usuarios**
    *   Supervisar y gestionar campañas de revisión de acceso como **administrador de campañas**
    *   Gestionar campañas de revisión de acceso basadas en eventos como **administrador de campañas**

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [Gestión de campaña de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/kfdck/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023