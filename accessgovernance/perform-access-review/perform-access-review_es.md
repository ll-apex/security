# Realizar tarea de revisión de acceso

## Introducción

Las **revisiones de acceso** las pueden realizar desde la consola de **Oracle Access Governance** los usuarios con los siguientes roles, que están basados en atributos de datos derivados del sistema conectado:

*   **Usuario** (revisión de acceso asignada a mí)
*   **Mánager** (revisión de acceso asignada a usuarios de mi equipo)
*   **Responsable** (revisión de acceso asignada a usuarios a través de recursos de los que soy responsable)

Según la configuración del flujo de trabajo en el primer laboratorio **Crear campaña de revisión de acceso**, **Oracle Access Governance** distribuye las revisiones de acceso a los revisores correspondientes en toda la organización seleccionada. En este laboratorio, **usuario empleado** es el revisor de primer nivel y **gestor de usuarios** es el revisor de segundo nivel. Al aprovechar los **análisis prescriptivos** y los **insights** integrados en las revisiones de acceso, los empleados y los mánager de usuarios pueden tomar decisiones informadas sobre los derechos de acceso. Los usuarios también pueden aprobar artículos de bajo riesgo de forma masiva en función de las **recomendaciones de IA/AA** del sistema.

*   Tiempo estimado: 20 minutos
*   Persona: Empleado y mánager de usuarios

Vea el siguiente vídeo para una breve introducción al laboratorio. [Realizar revisión de acceso](videohub:1_kui9p56v)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Aceptar o revocar la **tarea de revisión de acceso** que se me ha asignado de la **campaña de certificación** como **usuario empleado**
*   Aceptar o revocar la **tarea de revisión de acceso** asignada a mí desde la **campaña de certificación** como **mánager de usuarios**

## Tarea 1: Conexión a Oracle Access Governance como usuario empleado

1.  Abra el explorador Chrome y vaya a la URL de **Oracle Access Governance** según la asignación del **grupo**.
    *   [Oracle Access Governance LiveLabs Grupo 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **accessgov\_iam**.
3.  Conéctese a **Oracle Access Governance** como **usuario empleado** con un nombre de usuario y una contraseña proporcionados por la instrucción LiveLabs. **Tenga en cuenta que el nombre de usuario en la captura de pantalla del paso LiveLabs puede ser diferente del nombre de usuario que ha recibido.** ![Conexión a Access Governance](images/ag-logon.png)
4.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/ag-homepage.png)

## Tarea 2: Realizar tarea de revisión de acceso (Revisión de usuario de empleado)

1.  Seleccione uno de los mosaicos Tareas de revisiones de acceso. Para este laboratorio, haga clic en el botón **Seleccionar** del mosaico **Me siento ambicioso, revisemos todo...** ![Tareas de revisión de acceso](images/open-menu-review.png)
2.  Verá una lista de **tareas de revisión de acceso** asignadas desde las **campañas de revisión de acceso** programadas desde el primer laboratorio. Puede realizar una búsqueda de las tareas de revisión de acceso creadas desde la primera práctica de laboratorio en función del valor **Origen de revisión**, también conocido como **Nombre de campaña**, en la columna central de la tabla, que anota en el **Laboratorio 1**. En caso de que la **campaña** del primer laboratorio no se haya iniciado aún, también puede seleccionar una **tarea de revisión** de una campaña preconfigurada. En ese caso, seleccione las tareas de revisión de acceso con **Origen de revisión** como **... Ejemplo de revisión de acceso a organización**, por ejemplo, **Ejemplo de revisión de acceso a organización de soporte**. Para revisar el acceso, siga los siguientes pasos:
    *   Compruebe la información de la tarea de revisión, como **Nombre de identidad**, **Nombre de asignación**, **Nombre de mánager**, **Tipo de asignación** y **Días de vencimiento** para los que se inicia la tarea.
    *   Filtre la lista de tareas de revisión seleccionando **Aceptar recomendación** o **Revisar recomendación**. Basado en el **análisis prescriptivo** basado en el algoritmo de aprendizaje automático, **Oracle Access Governance** recomienda realizar acciones para cada elemento de revisión en función de las puntuaciones de riesgo y los análisis calculados.
    *   Puede aceptar el elemento de revisión haciendo clic en **Aceptar** en la columna **Acciones**. Esta acción se sugiere solo para los elementos **Recomendar aceptación**.
    *   En caso de que desee ver las estadísticas analíticas, especialmente para los elementos marcados como **Revisión recomendada**, puede hacer clic en la columna **Ver** de **Estadísticas** para revisar una tarea. Las estadísticas de ![Tareas de revisión de acceso](images/select-review-recommended.png) incluyen:
    *   La información basada en IA/AA con **puntuación de alineación** utiliza el **análisis de grupos de pares** de IA/AA realizado por **Oracle Access Governance** para recomendar este elemento para **revisar** o **aceptar**
    *   Descripción de la tarea de revisión
    *   Seguimiento de revisiones de accesos
    *   Cambios recientes en el perfil del usuario ![Tareas de revisión de acceso](images/review-insight-analytics.png)
3.  Decide (Accept or Revoke): Review all insights and select to **accept** or **revoke** this access privilege. In this lab, you may pick one access review with **Recommend Review**, view the detail, and **Accept** it. Enter **justification** for why you accept this access review item, which will be logged in **Access review trail**. **Accept** the review task item will trigger the **current review task** assigned to the second-level reviewer, which is the **user manager** in the next task. On the contrary, **revoke** access by an **employee user** will not trigger next-level access review by the **manager user**.  
    ![Tareas de revisión de acceso](images/revoke-accept-with-insights.png)
4.  Acción masiva basada en la recomendación: también puede seleccionar varias tareas de revisión y decidir aceptar o revocar esos privilegios. Por ejemplo, al seleccionar el filtro **Aceptar recomendación**, se devolverá una lista de elementos de revisión de acceso recomendados por **Oracle Access Governance** para **Aceptar** basados en **análisis prescriptivos**. ![Revisión de acceso](images/bulk-review.png)
5.  Selección masiva: seleccione todos los elementos **Aceptar recomendación** y, a continuación, haga clic en el botón **Aceptar**. Del mismo modo, seleccione todos los elementos de **Recomendar revisión** y, a continuación, haga clic en el botón **Revocar**. ![Tareas de revisión de acceso](images/bulk-review-selection.png)
6.  Acción masiva con justificación: proporcione la justificación para **Aceptar** o **Revocar** y, a continuación, haga clic en **Enviar**. ![Tareas de revisión de acceso](images/bulk-accept-justification.png)

## Tarea 3: Conexión a Oracle Access Governance como User Manager

1.  Abra el explorador Chrome y vaya a la URL de **Oracle Access Governance** según la asignación del **grupo**.
    *   [Oracle Access Governance LiveLabs Grupo 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **accessgov\_iam**.
3.  Conéctese a **Oracle Access Governance** como **usuario gestor** con un nombre de usuario y una contraseña proporcionados por la instrucción LiveLabs. **Tenga en cuenta que el nombre de usuario en la captura de pantalla del paso LiveLabs puede ser diferente del nombre de usuario que ha recibido.** ![Conexión a Access Governance](images/ag-logon.png)
4.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/ag-homepage.png)

## Tarea 4: Realizar tarea de revisión de acceso (revisión del gestor de usuarios)

1.  En esta práctica, el gestor de usuarios es el revisor de segundo nivel. Como mánager de usuarios, puede ver los elementos de revisión de acceso que los usuarios empleados aceptaron en la tarea anterior. Haga clic en el botón **Seleccionar** del mosaico **Me siento ambicioso, revisemos todo...**. Como alternativa, también puede hacer clic en el botón **Seleccionar** del mosaico **Estoy ocupado, revisemos...** para revisar solo los elementos de **alto riesgo**. ![Texto alternativo de imagen](images/open-menu-manager-review.png)
2.  Verá una lista de las tareas de revisión de acceso que tiene asignadas desde campañas de revisión de acceso o desde los resultados de revisión de acceso de su empleado, que usted es el revisor de segundo nivel como mánager. Busque la tarea de revisión disparada por la **Tarea 2: Realizar tarea de revisión de acceso (revisión de usuario de empleado)** por el **nombre de identidad** y el **nombre de asignación**. También puede acotar la lista buscando las tareas de revisión de acceso en función del valor **Nombre de campaña** de **origen de revisión** en la columna central de la tabla. Para las tareas de revisión:
    *   Compruebe la información de la tarea de revisión, como **Nombre de identidad**, **Nombre de asignación**, **Nombre de mánager**, **Tipo de asignación** y **Días de vencimiento** para los que se inicia la tarea.
    *   Filtre la lista de tareas de revisión seleccionando **Aceptar recomendación** o **Revisar recomendación**. Según el **análisis prescriptivo** basado en el **algoritmo de IA/AA**, **Oracle Access Governance** recomienda acciones para cada elemento de revisión en función de las puntuaciones de riesgo y los análisis calculados.
    *   Puede aceptar o revocar el elemento de revisión haciendo clic en **Aceptar** o **Revocar** en la columna **Acciones**. La acción **Aceptar** se sugiere solo para los elementos **Aceptar recomendación**.
    *   En caso de que desee ver las estadísticas analíticas, especialmente para el elemento marcado como **Revisión recomendada**, puede hacer clic en la columna **Ver** de **Estadísticas** para revisar una tarea. Las estadísticas de ![Tareas de revisión de acceso](images/access-review-manager.png) incluyen:
    *   La información basada en IA/AA con **puntuación de alineación** utiliza el **análisis de grupos de pares** de IA/AA realizado por **Oracle Access Governance** para recomendar este elemento para **revisar** o **aceptar**
    *   Descripción de la tarea de revisión
    *   Acceda al seguimiento de revisión; debe ver la **justificación** introducida por el autorrevisor del empleado en la tarea anterior.
    *   Cambios recientes en el perfil del usuario ![Tareas de revisión de acceso](images/access-review-insights-manager.png)
3.  Decidir (Aceptar o Revocar): revise todas las estadísticas y seleccione **Aceptar** o **Revocar** este privilegio de acceso. En este laboratorio, puede seleccionar una revisión de acceso con **Revisión recomendada**, ver los detalles y **Revocar**, lo que disparará el proceso de solución automática en el sistema **Oracle Access Governance**.
4.  Durante este laboratorio, ha navegado por la consola de **Oracle Access Governance** para seleccionar **tareas de revisión de acceso** asignadas a usted como **empleado** y **usuario gestor**, ver **análisis prescriptivo** y **recomendación** propuestas por **Oracle Access Governance** y tomar decisiones informadas **Aceptar** o **Revocar** para revisar tareas basadas en **análisis de grupo de pares** y **insights**.
5.  Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de revisión de acceso de realización de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/aarrs/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autor**: Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Última actualización por/fecha**: Edward Lu, Gestión de productos de Oracle IAM, octubre de 2022