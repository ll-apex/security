# Creación de campaña de revisión de acceso: revisión propia y de mánager de usuarios

## Introducción

Como usuario con un rol de **administrador de campaña**, puede crear campañas de revisión de acceso desde la consola de **Oracle Access Governance**. Puede definir criterios de selección para revisiones de acceso basados en **usuarios** (quién tiene acceso), **aplicaciones** (a qué accede), **permisos** (qué permisos) y **roles** (qué roles).

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de campaña

Vea el siguiente vídeo para una breve introducción al laboratorio. [Crear campaña de revisión de acceso](videohub:1_9s3mt0qx)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear **campaña de revisión de acceso** para su propia revisión y la de los mánager de usuarios
*   Definir el **flujo de trabajo de los revisores**
*   Ejecutar ahora o programar una **campaña de revisión de acceso**

## Tarea 1: Conexión a Oracle Access Governance como administrador de campañas de revisión de acceso

1.  Asegúrese de que tiene seleccionado el dominio de identidad **ag-domain**.
2.  Conéctese a la consola de **Oracle Access Governance**. Consulte el _laboratorio 4: Tarea 1.4_ para obtener la URL de instancia de servicio de **Oracle Access Governance**.
3.  Conéctese como **administrador de campaña - Pamela Green** con el nombre de usuario y la contraseña que se mencionan a continuación.

**Nombre de usuario:** `<copy>pamela.green</copy>`

**Contraseña:** `<copy>Oracl@123456</copy>`

![Conexión a Access Governance](images/admin-login.png)

4.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/admin-home.png)

## Tarea 2: Creación de campaña de revisión de acceso como administrador de campaña

1.  Haga clic en el icono de menú de hamburguesa en la esquina superior izquierda. En el menú desplegable, seleccione **Campañas**. ![Seleccionar criterios](images/admin-campaign.png)
2.  Haga clic en **Crear una campaña**. ![Seleccionar criterios](images/admin-create-campaign.png)
3.  Puede seleccionar cualquiera de las 4 dimensiones **¿Quién tiene acceso?** (Usuarios), **A qué están accediendo** (Aplicaciones), **Qué permisos** (Permiso) y **Qué roles** (Roles). En esta práctica de laboratorio, puede seleccionar primero el mosaico **¿Quién tiene acceso?** (Usuarios). ![Seleccionar usuarios](images/admin-select-dimensions.png)
4.  Puede seleccionar usuarios por **organización**, **código de puesto** o **ubicación**. Para esta práctica de laboratorio, debe seleccionar la **organización - Garantía de calidad y operaciones** que los usuarios están revisando según el laboratorio assignment.After que, haga clic en **Aplicar mis selecciones**, que le devolverá al asistente **Crear una nueva campaña de revisión de acceso**. ![Seleccionar organizaciones](images/select-org.png) ![Seleccionar los siguientes criterios](images/admin-select-next.png)
5.  En los gráficos circulares de la esquina superior derecha, puede revisar el ámbito de los **usuarios**, las **aplicaciones**, los **permisos** y los **roles** seleccionados en la selección **Lo que he seleccionado**, revise las selecciones realizadas por usted. En este laboratorio, puede hacer clic en el botón **I'm good, go to workflows**. ![Seleccionar el flujo de trabajo](images/admin-select-next.png)
6.  Revise el flujo de trabajo y los revisores seleccionados automáticamente. Puede cambiarlos si es necesario. Por ejemplo, si hace clic en **Seleccionaré mi propio flujo de trabajo**, se abrirá el menú **Configurar flujo de trabajo**. En esta práctica de laboratorio, puede que no necesite cambiar el flujo de trabajo y los revisores. Acepte el valor por defecto y haga clic en **Siguiente**. Con esta configuración, las revisiones de acceso se dirigen primero al **usuario empleado**, después de la auto revisión del empleado, se dirigirá al segundo **mánager de usuarios** revisor para su aprobación. ![Workflow por Defecto](images/admin-configure-workflow.png)
7.  Acepte el valor por defecto **Una vez** para el campo **¿Con qué frecuencia desea que se ejecute?**. Puede proporcionar un nombre de campaña y una descripción de su elección. Por ejemplo, introduzca el nombre de la campaña como **Revisión de acceso a organización**, introduzca la descripción que prefiera, seleccione **Ejecutar ahora** y haga clic en el botón **Siguiente** para programar la campaña. Observe el **nombre de campaña** como referencia en la siguiente práctica de laboratorio para buscar la campaña de revisión de acceso recién creada. Para los revisores, el **nombre de la campaña** se denomina **origen de revisión** en el panel de control de tareas de revisión. ![Flujo de trabajo por defecto - Ejecución única](images/admin-default-workflow.png)
8.  Puede revisar los criterios de campaña, el flujo de trabajo, los revisores y el programa de ejecución seleccionados. Para este laboratorio, haga clic en el botón **Crear** para crear y programar una campaña. La campaña comenzará aproximadamente **10 minutos** desde la creación.  
    ![Ver gráficos - Crear](images/admin-summary.png)
9.  Se ha programado una **Revisión de acceso a organización** de campaña recién creada en la sección **Mis próximas campañas**. **La campaña recién creada** tarda aproximadamente 10 minutos en pasar al estado **en curso**. ![Ver gráficas - campaña creada](images/admin-view-created-campaign.png)
10.  Durante este laboratorio, ha navegado por la consola de **Oracle Access Governance** y creado una **campaña de revisión de acceso de usuario** como **administrador de campaña**.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Edward Lu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023