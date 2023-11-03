# Gestionar campaña de revisión de acceso (propietario de campaña)

## Introducción

Los usuarios con el rol de **administrador de campaña** pueden supervisar y gestionar campañas de revisión de acceso mediante la consola de **Oracle Access Governance**. Los usuarios pueden ver el progreso de las **campañas en curso**, ver y descargar informes detallados de análisis de campañas, clonar **campañas anteriores**, finalizar **campañas en curso**, etc.

*   Tiempo estimado: 10 minutos
*   Persona: Administrador de campaña

Vea el siguiente vídeo para una breve introducción al laboratorio. [Gestión de campañas de revisión de acceso](videohub:1_b19sk9f6)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Muestra una lista de **campañas de certificación** de las que es propietario o que ha creado
*   Vea el progreso de las **campañas de certificación** realizadas por los revisores con **información analítica**

## Tarea 1: Conexión a Oracle Access Governance como administrador de campaña

1.  Si sigue conectándose como usuario desde la práctica anterior, asegúrese de desconectarse y volver a conectarse. Asegúrese de que tiene seleccionado el dominio de identidad **ag-domain**.
2.  Conéctese a **Oracle Access Governance** como **administrador de campaña - Pamela Green** con un nombre de usuario y una contraseña proporcionados por la instrucción LiveLabs. **Tenga en cuenta que el nombre de usuario en la captura de pantalla del paso LiveLabs puede ser diferente del nombre de usuario que ha recibido.** ![Conexión a Access Governance](images/admin-login.png)
3.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/admin-home.png)

## Tarea 2: Gestión y supervisión de campañas de certificación en curso

1.  Las campañas de revisión de acceso se organizan por **Próximas campañas**, **Campañas en curso** y **Campañas anteriores**. Haga clic en el botón **Seleccionar** del mosaico **Mostrar mis campañas en curso**. ![Campaña de supervisión de menús](images/admin-home.png)
2.  Verá una lista de las campañas **en curso** que posee o ha creado. Seleccione la campaña que ha creado en este laboratorio. ![Ver lista de campañas](images/admin-view-list-campaign.png)
3.  Puede conocer el progreso realizado por los revisores. Vea cuántos **revisores** están asignados a esta campaña. Para cada revisor, ¿cuántos elementos de revisión se asignan y cuál es el porcentaje de progreso realizado? ![Progreso de la campaña](images/admin-view-campaign-progress.png)
4.  Haga clic en el botón **Detalles adicionales** para ver más detalles de la campaña seleccionada. ![Detalles adicionales de campaña](images/admin-additional-details.png)
5.  Haga clic en el menú desplegable **Acciones** y seleccione **Ver informe** para ver un informe que muestra los detalles de progreso de la campaña seleccionada. Si el **informe de progreso de campaña** aún no se ha generado, seleccione una campaña anterior para ver el **informe de progreso**. ![Menú de progreso de la campaña](images/admin-view-campaign-progress-menu.png)
6.  Puede revisar análisis e informes listos para usar sobre el progreso de la campaña. También puede descargar el informe como un archivo PDF. ![Análisis de campaña](images/admin-view-campaign-analytics.png)
7.  Haga clic en **Cerrar** y vuelva a la pantalla de detalles de campaña, haga clic en el menú desplegable **Acciones**. Tiene la opción de **terminar** la campaña actual. También tiene la opción de **clonar** la campaña actual. ![Menú de detalle de campaña](images/admin-view-campaign-progress-menu.png)
8.  Haga clic en **Clonar** e introduzca un nuevo nombre para la campaña, seleccione **Ejecutar ahora** y, a continuación, haga clic en **Crear**. Ahora puede crear una nueva campaña clonando una campaña existente. ![Copiar campaña](images/admin-clone-campaign.png)
9.  Durante este laboratorio, como **administrador de campaña**, ha aprovechado las funciones de análisis proporcionadas por **Oracle Access Governance** para obtener información sobre el estado del progreso de la campaña. También aprenderá a crear rápidamente una nueva campaña clonando una campaña existente.

Ahora puede **proceder al siguiente laboratorio**

## Más información

*   [Gestión de campaña de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/kfdck/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Edward Lu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023