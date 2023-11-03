# Creación de campaña de revisión de acceso - Revisión propia y de usuario

## Introducción

Como usuario con un rol de **administrador de campaña**, puede crear campañas de revisión de acceso desde la consola de **Oracle Access Governance**. Puede definir criterios de selección para revisiones de acceso basados en **usuarios** (quién tiene acceso), **aplicaciones** (a qué accede), **permisos** (qué permisos) y **roles** (qué roles).

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de campaña

Vea el siguiente vídeo para una breve introducción al laboratorio. [Crear campaña de revisión de acceso](videohub:1_p2m93d2k)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear **campaña de revisión de acceso** para su propia revisión y la de los mánager de usuarios
*   Definir el **flujo de trabajo de los revisores**
*   Ejecutar ahora o programar una **campaña de revisión de acceso**

## Tarea 1: Conexión a Oracle Access Governance como administrador de campañas de revisión de acceso

1.  Abra el explorador Chrome y vaya a la URL de **Oracle Access Governance** según la asignación del **grupo**.
    *   [Oracle Access Governance LiveLabs Grupo 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Asegúrese de que tiene seleccionado el dominio de identidad **accessgov\_iam**.
3.  Conéctese a **Oracle Access Governance** como **administrador de campaña** con un nombre de usuario y una contraseña proporcionados por la instrucción LiveLabs. **Tenga en cuenta que el nombre de usuario en la captura de pantalla del paso LiveLabs puede ser diferente del nombre de usuario que ha recibido.** ![Conexión a Access Governance](images/ag-logon.png)
4.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/ag-homepage.png)

## Tarea 2: Creación de campaña de revisión de acceso como administrador de campaña

1.  Desplácese a la parte inferior y seleccione **Vamos a crear un trabajo y definir una nueva campaña** o en el menú desplegable y seleccione **Crear una nueva campaña**. ![Seleccionar criterios](images/create-campaign.png)
2.  Puede seleccionar cualquiera de las 4 dimensiones **¿Quién tiene acceso?** (Usuarios), **A qué están accediendo** (Aplicaciones), **Qué permisos** (Permiso) y **Qué roles** (Roles). En esta práctica de laboratorio, puede seleccionar primero el mosaico **¿Quién tiene acceso?** (Usuarios). ![Seleccionar usuarios](images/select-dimensions.png)
3.  Puede seleccionar usuarios por **organización**, **código de puesto** o **ubicación**. Para este laboratorio, debe seleccionar la **organización** que los usuarios están revisando en función de la asignación del laboratorio. Por ejemplo, seleccione la organización **Soporte**. Otras **organizaciones** en asignaciones de laboratorio incluyen **Human Resources**, **Software Engineering**, **Product Management** y **Finance**. Después, haga clic en **Aplicar mis selecciones**, lo que le devolverá al asistente **Crear una nueva campaña de revisión de acceso**. ![Seleccionar organizaciones](images/select-users.png)
4.  Puede seleccionar cualquiera de las 3 dimensiones restantes **A qué están accediendo** (aplicaciones), **Qué permisos** (permiso) o **Qué roles** (roles). En este laboratorio, puede seleccionar el mosaico **A qué están accediendo** (Aplicaciones). ![Seleccionar los siguientes criterios](images/select-next.png)
5.  Puede seleccionar **Aplicaciones** por nombre. En este laboratorio, puede seleccionar las aplicaciones **Confluence central**, **Insignia corporativa** y **Ordenador portátil corporativo** y, a continuación, hacer clic en **Aplicar mis selecciones**, lo que le devolverá al asistente **Crear una nueva campaña de revisión de acceso**. ![Seleccionar Aplicaciones](images/select-applications.png)
6.  En los gráficos circulares de la esquina superior derecha, puede revisar el ámbito de los **usuarios**, las **aplicaciones**, los **permisos** y los **roles** seleccionados en la selección **Lo que he seleccionado**, revise las selecciones realizadas por usted. En este laboratorio, puede hacer clic en el botón **I'm good, go to workflows**. ![Ver gráficos](images/view-charts.png)
7.  Revise el flujo de trabajo y los revisores seleccionados automáticamente. Puede cambiarlos si es necesario. Por ejemplo, si hace clic en **Seleccionaré mi propio flujo de trabajo**, se abrirá el menú **Configurar flujo de trabajo**. En esta práctica de laboratorio, puede que no necesite cambiar el flujo de trabajo y los revisores. Acepte el valor por defecto y haga clic en **Siguiente**. Con esta configuración, las revisiones de acceso van primero al **usuario empleado**, después de la autoevaluación del empleado, irá al segundo **mánager de usuarios** revisor para su aprobación. ![Workflow por Defecto](images/configure-workflow.png) ![Workflow por Defecto](images/default-workflow.png)
8.  Acepte el valor por defecto **Una vez** para el campo **¿Con qué frecuencia desea que se ejecute?**. Puede proporcionar un nombre de campaña y una descripción de su elección. Por ejemplo, introduzca el nombre de la campaña como **Revisión de acceso corporativo de organización de soporte**, introduzca la descripción que prefiera, seleccione **Ejecutar ahora** y haga clic en el botón **Siguiente** para programar la campaña. Observe el **nombre de campaña** como referencia en la siguiente práctica de laboratorio para buscar la campaña de revisión de acceso recién creada. Para los revisores, el **nombre de la campaña** se denomina **origen de revisión** en el panel de control de tareas de revisión. ![Nombre de campaña](images/name-campaign.png)
9.  Puede revisar los criterios de campaña, el flujo de trabajo, los revisores y el programa de ejecución seleccionados. Para este laboratorio, haga clic en el botón **Crear** para crear y programar una campaña. La campaña comenzará aproximadamente **10 minutos** desde la creación.  
    ![Ver gráficos](images/summary.png)
10.  Se ha programado una **Revisión de acceso corporativo de soporte** de campaña recién creada en la sección **Mis próximas campañas**. **La campaña recién creada** tarda aproximadamente 10 minutos en pasar al estado **en curso**. ![Ver gráficos](images/view-created-campaign.png)
11.  Durante este laboratorio, ha navegado por la consola de **Oracle Access Governance** y creado una **campaña de revisión de acceso de usuario** como **administrador de campaña**.
12.  Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autor**: Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Última actualización por/fecha**: Edward Lu, Gestión de productos de Oracle IAM, octubre de 2022