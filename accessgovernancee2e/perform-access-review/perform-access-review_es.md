# Realizar tarea de revisión de acceso

## Introducción

Las **revisiones de acceso** las pueden realizar desde la consola de **Oracle Access Governance** los usuarios con los siguientes roles, que están basados en atributos de datos derivados del sistema conectado:

*   **Usuario** (revisión de acceso asignada a mí)
*   **Mánager** (revisión de acceso asignada a usuarios de mi equipo)
*   **Responsable** (revisión de acceso asignada a usuarios a través de recursos de los que soy responsable)

Según la configuración del flujo de trabajo en el primer laboratorio **Crear campaña de revisión de acceso**, **Oracle Access Governance** distribuye las revisiones de acceso a los revisores correspondientes en toda la organización seleccionada. En este laboratorio, **usuario empleado** es el revisor de primer nivel y **gestor de usuarios** es el revisor de segundo nivel. Al aprovechar los **análisis prescriptivos** y los **insights** integrados en las revisiones de acceso, los empleados y los mánager de usuarios pueden tomar decisiones informadas sobre los derechos de acceso. Los usuarios también pueden aprobar artículos de bajo riesgo de forma masiva en función de las **recomendaciones de IA/AA** del sistema.

*   Tiempo estimado: 20 minutos
*   Persona: Empleado y mánager de usuarios

Vea el siguiente vídeo para una breve introducción al laboratorio. [Realizar revisión de acceso](videohub:1_jteb4r9y)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Aceptar o revocar la **tarea de revisión de acceso** que se me ha asignado de la **campaña de certificación** como **usuario empleado**
*   Aceptar o revocar la **tarea de revisión de acceso** asignada a mí desde la **campaña de certificación** como **mánager de usuarios**

## Tarea 1: Conexión a Oracle Access Governance como usuario empleado

1.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **ag-domain**.
    
2.  Inicie sesión en **Oracle Access Governance** como **usuario empleado: Mark Hernandez** con el nombre de usuario y la contraseña que se mencionan a continuación.
    
    **Nombre de usuario:**
    
        <copy>mhernandez</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        
    
    ![Conexión a Access Governance](images/user-ag-logon.png)
    
3.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/user-ag-homepage.png)
    

## Tarea 2: Realizar tarea de revisión de acceso (Revisión de usuario de empleado)

1.  Seleccione uno de los mosaicos Tareas de revisiones de acceso. Para este laboratorio, haga clic en el botón **Seleccionar** del mosaico **Me siento ambicioso, revisemos todo...** ![Tareas de revisión de acceso](images/user-ag-homepage.png)
    
2.  Verá una lista de **tareas de revisión de acceso** asignadas desde las **campañas de revisión de acceso** programadas desde el primer laboratorio. Puede realizar una búsqueda de las tareas de revisión de acceso creadas desde la primera práctica de laboratorio en función del valor **Origen de revisión**, también conocido como **Nombre de campaña**, en la columna central de la tabla, que anota en el **Laboratorio 1**. En caso de que la **campaña** del primer laboratorio no se haya iniciado aún, también puede seleccionar una **tarea de revisión** de una campaña preconfigurada. En ese caso, seleccione las tareas de revisión de acceso con **Origen de revisión** como **... Ejemplo de revisión de organización**, por ejemplo, **Ejemplo de revisión de acceso a organización**. Para revisar el acceso, siga los siguientes pasos:
    
    *   Compruebe la información de la tarea de revisión, como **Nombre de identidad**, **Nombre de asignación**, **Nombre de mánager**, **Tipo de asignación** y **Días de vencimiento** para los que se inicia la tarea.
    *   Filtre la lista de tareas de revisión seleccionando **Aceptar recomendación** o **Revisar recomendación**. Basado en el **análisis prescriptivo** basado en el algoritmo de aprendizaje automático, **Oracle Access Governance** recomienda realizar acciones para cada elemento de revisión en función de las puntuaciones de riesgo y los análisis calculados.
    *   Puede aceptar el elemento de revisión haciendo clic en **Aceptar** en la columna **Acciones**. Esta acción se sugiere solo para los elementos **Recomendar aceptación**.
    *   En caso de que desee ver las estadísticas analíticas, especialmente para los elementos marcados como **Revisión recomendada**, puede hacer clic en la columna **Ver** de **Estadísticas** para revisar una tarea. Las estadísticas de ![Revisión de selección de tareas de revisión de acceso](images/user-select-review-recommended.png) incluyen:
    *   La información basada en IA/AA con **puntuación de alineación** utiliza el **análisis de grupos de pares** de IA/AA realizado por **Oracle Access Governance** para recomendar este elemento para **revisar** o **aceptar**
    *   Descripción de la tarea de revisión
    *   Seguimiento de revisiones de accesos
    *   Cambios recientes en el perfil del usuario ![Acceso a revisión de tareas Estadísticas IA/AA](images/user-review-insight-analytics.png)
3.  Decidir (Aceptar o Revocar): seleccione todas las revisiones de acceso. Haga clic en el botón **Aceptar** en la columna **Acciones**. Introduzca una **justificación** para saber por qué acepta todos los elementos de revisión de acceso y, a continuación, haga clic en **Enviar**.
    

**Nota:** En este ejemplo de laboratorio, aceptamos todas las tareas de _Revisión de acceso_. Sin embargo, puede revisar todas las estadísticas y seleccionar **aceptar** o **revocar** este privilegio de acceso. **Aceptar** el elemento de tarea de revisión disparará la **tarea de revisión actual** asignada al revisor de segundo nivel, que es el **gestor de usuarios** en la siguiente tarea. Por el contrario, la **revocación** de acceso por parte de un **usuario empleado** no disparará la revisión de acceso de siguiente nivel por parte del **usuario gestor**.

![Recomendación de revisión de selección de usuario](images/user-select-review-recommended.png)

![Aceptación masiva de justificación](images/user-bulk-accept-justification.png)

## Tarea 3: Conexión a Oracle Access Governance como User Manager

1.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **ag-domain**.
    
2.  Inicie sesión en **Oracle Access Governance** como **usuario de mánager: Harlan Bullard** con el nombre de usuario y la contraseña que se mencionan a continuación.
    

**Nombre de usuario:** `<copy>harlan.bullard</copy>`

**Contraseña:** `<copy>Oracl@123456</copy>`

    ![Manager Access Governance Login](images/manager-ag-logon.png)
    

3.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Manager Access Governance](images/manager-ag-homepage.png)

## Tarea 4: Realizar tarea de revisión de acceso (revisión del gestor de usuarios)

1.  En esta práctica, el gestor de usuarios es el revisor de segundo nivel. Como mánager de usuarios, puede ver los elementos de revisión de acceso que los usuarios empleados aceptaron en la tarea anterior. Haga clic en el botón **Seleccionar** del mosaico **Me siento ambicioso, revisemos todo...**. Como alternativa, también puede hacer clic en el botón **Seleccionar** del mosaico **Estoy ocupado, revisemos...** para revisar solo los elementos de **alto riesgo**. ![Valoración del menú de apertura del mánager](images/manager-open-menu-manager-review.png)
    
2.  Verá una lista de las tareas de revisión de acceso que tiene asignadas desde campañas de revisión de acceso o desde los resultados de revisión de acceso de su empleado, que usted es el revisor de segundo nivel como mánager. Busque la tarea de revisión disparada por la **Tarea 2: Realizar tarea de revisión de acceso (revisión de usuario de empleado)** por el **nombre de identidad** y el **nombre de asignación**. También puede acotar la lista buscando las tareas de revisión de acceso en función del valor **Nombre de campaña** de **origen de revisión** en la columna central de la tabla. Para las tareas de revisión:
    
    *   Compruebe la información de la tarea de revisión, como **Nombre de identidad**, **Nombre de asignación**, **Nombre de mánager**, **Tipo de asignación** y **Días de vencimiento** para los que se inicia la tarea.
    *   Filtre la lista de tareas de revisión seleccionando **Aceptar recomendación** o **Revisar recomendación**. Según el **análisis prescriptivo** basado en el **algoritmo de IA/AA**, **Oracle Access Governance** recomienda acciones para cada elemento de revisión en función de las puntuaciones de riesgo y los análisis calculados.
    *   Puede aceptar o revocar el elemento de revisión haciendo clic en **Aceptar** o **Revocar** en la columna **Acciones**. La acción **Aceptar** se sugiere solo para los elementos **Aceptar recomendación**.
    *   En caso de que desee ver las estadísticas analíticas, especialmente para el elemento marcado como **Revisión recomendada**, puede hacer clic en la columna **Ver** de **Estadísticas** para revisar una tarea. Las estadísticas de ![Gestor de revisión de acceso](images/manager-access-review-manager.png) incluyen:
    *   La información basada en IA/AA con **puntuación de alineación** utiliza el **análisis de grupos de pares** de IA/AA realizado por **Oracle Access Governance** para recomendar este elemento para **revisar** o **aceptar**
    *   Descripción de la tarea de revisión
    *   Acceda al seguimiento de revisión; debe ver la **justificación** introducida por el autorrevisor del empleado en la tarea anterior.
    *   Cambios recientes en el perfil del usuario ![Estadísticas de IA y AA](images/manager-access-review-insights-manager.png)
3.  Decidir (Aceptar o Revocar): seleccione la _revisión de acceso_ para el usuario: _Marcar Hernández_ con _Tipo de asignación_ como _Cuenta_ y **Revocar** este privilegio de acceso. En este laboratorio, puede seleccionar una revisión de acceso con **Revisión recomendada**, ver los detalles y **Revocar**, lo que disparará el proceso de solución automática en el sistema **Oracle Access Governance**.
    
4.  Durante este laboratorio, ha navegado por la consola de **Oracle Access Governance** para seleccionar **tareas de revisión de acceso** asignadas a usted como **empleado** y **usuario gestor**, ver **análisis prescriptivo** y **recomendación** propuestas por **Oracle Access Governance** y tomar decisiones informadas **Aceptar** o **Revocar** para revisar tareas basadas en **análisis de grupo de pares** y **insights**.
    

## Tarea 5: Conexión a Oracle Identity Governance como Administrador del Sistema

1.  Conéctese a la consola de autoservicio de Identity. Abra un separador del explorador con la siguiente URL para acceder a la consola de identidad de OIG.
    
    **Dirección URL:**
    
          <copy>http://oimhost.us.oracle.com:14000/identity</copy>
        
    
    **Nombre de usuario:**
    
         <copy>xelsysadm</copy>
        
    
    **Contraseña:**
    
         <copy>Welcome1</copy>
        

![Página de Conexión de OIG](images/oig-logon.png) 2. Debe ver el panel de control principal de **Oracle Identity Governance**. Haga clic en **Manage (Gestionar) -> Users (Usuarios)**. ![Página inicial de OIG](images/oig-homepage.png)

3.  Haga clic en Users. Busque el usuario **Mark Hernandez**. Tenga en cuenta los derechos de aplicación de la **Figma** presentes para el usuario.

![Página inicial de Access Governance](images/initial-user-details.png)

4.  Haga clic en **Autoservicio -> Tareas de aprovisionamiento -> Ejecución manual**. Aparece la página Satisfacción Manual. Observe que se muestra la solicitud de satisfacción manual para el usuario. ![Lista de tareas de aprovisionamiento](images/provisioning-tasks.png)
    
5.  Haga clic en la solicitud. La página de detalles de la solicitud muestra una vista detallada de la solicitud en las secciones Detalles, Contenido y Elementos del carro. Permite la gestión completa de la tarea mostrada. Haga clic en Finalizado. ![Solicitudes de Satisfacción Manual](images/manual-fulfillment.png) Ahora la solución ha finalizado con aprobación y aprovisionamiento manual. ![Completar solicitud de ejecución manual](images/complete-manual-fulfillment.png)
    
6.  Volver a Autoservicio. Haga clic en Manage -> Users. Busque el usuario **Mark Hernandez** similar a **Tarea 5: Paso 2**.
    
7.  Observe que los derechos de aplicación de la **Figma** se han eliminado para el usuario **Mark Hernandez** ![OIG de detalles de usuario](images/user-details.png)
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de revisión de acceso de realización de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/aarrs/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Edward Lu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023