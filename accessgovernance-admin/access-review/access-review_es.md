# Realización de tareas de revisión de acceso

## Introducción

Los usuarios pueden realizar revisiones de acceso desde la consola de Oracle Access Governance (Mark Hernández, Harlan Bullard, Jerry Poland) y las revisará el administrador de Access Governance (Pamela Green).

*   Persona: Revisor de campaña y Administrador de campaña

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_0sz90jrj)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear solicitudes de revisión de acceso como usuarios de Access Governance
*   Crear campaña como administrador de campaña
*   Aprobar solicitudes de revisión de acceso como administrador de campaña de Access Governance

## Tarea 1: Conexión a Oracle Access Governance como usuario empleado para crear solicitudes de acceso

1.  Inicie sesión en Oracle Access Governance como usuario empleado: Mark Hernandez con el nombre de usuario y la contraseña que se mencionan a continuación.
    
    **Nombre de usuario:**
    
        <copy>mhernandez</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

3.  En la página de inicio de la consola de Oracle Access Governance, en el menú de navegación, seleccione **Mis cosas -> Solicitar un nuevo acceso**
    
    ![Revisión de acceso](images/navigate-access.png)
    
4.  En **¿Qué acceso está solicitando?**, seleccione **¿Qué acceso desea?**.
    
    ![Revisión de acceso](images/which-access.png)
    
5.  Solicitar un nuevo acceso -> Seleccione "Sí". Haga clic en **Siguiente**.
    
    ![Revisión de acceso](images/click-yes.png)
    
6.  En Qué desea acceder -> Seleccione **DB-Manage-Access** -> Haga clic en **Next** (Siguiente).
    

    ![Access Review](images/select-db-manage-access.png)
    

7.  Bajo Necesitamos más información para esta solicitud -> Proporcionar justificación.
    
    ![Revisión de acceso](images/provide-justification.png)
    
8.  Haga clic en **Enviar solicitud**.
    
9.  Inicie sesión en Oracle Access Governance como usuario empleado: Harlan Bullard con el nombre de usuario y la contraseña que se mencionan a continuación.
    

    **Username:**
    ```
    <copy>harlan.bullard</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

10.  En la página de inicio de la consola de Oracle Access Governance, en el menú de navegación, seleccione **Mis cosas -> Solicitar un nuevo acceso**
    
11.  En **¿Qué acceso está solicitando?**, seleccione **¿Qué acceso desea?**.
    
12.  Solicitar un nuevo acceso -> Seleccione "Sí". Haga clic en **Siguiente**.
    
13.  En Qué desea acceder -> Seleccione **DB-Manage-Access** -> Haga clic en **Next** (Siguiente).
    
14.  Bajo Necesitamos más información para esta solicitud -> Proporcionar justificación.
    
15.  Haga clic en **Enviar solicitud**.
    
16.  Conéctese a Oracle Access Governance como usuario empleado: Jerry Poland con el nombre de usuario y la contraseña que se mencionan a continuación.
    

    **Username:**
    ```
    <copy>jerry.poland</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

17.  En la página de inicio de la consola de Oracle Access Governance, en el menú de navegación, seleccione **Mis cosas -> Solicitar un nuevo acceso**
    
18.  En **¿Qué acceso está solicitando?**, seleccione **¿Qué acceso desea?**.
    
19.  Solicitar un nuevo acceso -> Seleccione "Sí". Haga clic en **Siguiente**.
    
20.  En Qué desea acceder -> Seleccione **DB-Manage-Access** -> Haga clic en **Next** (Siguiente).
    
21.  Bajo Necesitamos más información para esta solicitud -> Proporcionar justificación.
    
22.  Haga clic en **Enviar solicitud**.
    

## Tarea 2: Conexión a Oracle Access Governance como administrador de Access Governance para aprobar solicitudes de acceso

1.  Conéctese a Oracle Access Governance como usuario empleado: Pamela Green con el nombre de usuario y la contraseña que se mencionan a continuación.
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

2.  Vaya a MyStuff -> Approvals.You verá las solicitudes del usuario Harlan Bulllard, Mark Hernandez y Jerry Poland para **DB-Manage-Access**.
    
3.  En Acciones, haga clic en aprobar y aprobar la solicitud para los usuarios Harlan Bullard, Mark Hernández y Jerry Poland.
    

## Tarea 3: Ejecución de una carga de datos manual

1.  En la página inicial de la consola de Access Governance, navegue hasta Administración de servicios -> Sistema conectado.
    
    ![Crear Política](images/connected-system.png)
    
2.  En la página Sistemas conectados, seleccione el sistema conectado **OAG-DB**.
    
    ![Crear Política](images/select-system.png)
    
3.  Haga clic en Actions (Acciones) -> Load Data Now (Cargar datos ahora). Se realizará una carga de datos manual.
    
    ![Crear Política](images/load-date.png)
    
4.  Una vez finalizada la carga de datos, el estado se mostrará como Correcto.
    

## Tarea 4: Conexión a Oracle Access Governance como administrador de Access Governance para crear una campaña

1.  Conéctese a Oracle Access Governance como usuario empleado: Pamela Green con el nombre de usuario y la contraseña que se mencionan a continuación.
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        
2.  Vaya a Acceder a revisiones -> Campañas . Haga clic en **Crear una campaña**
    

![Revisión de acceso](images/navigate-campaigns.png)

![Revisión de acceso](images/create-campaign.png)

3.  En ¿Qué tipo de campaña de revisión de acceso te gustaría hacer? -> Seleccione **Sistemas de revisión gestionados por Access Governance**.

![Revisión de acceso](images/access-review-ag.png)

4.  Seleccione **¿Quién tiene acceso?**. Busque la **garantía de calidad** de la organización. Haga clic en **Aplicar mis selecciones**

![Revisión de acceso](images/who-has-access.png)

![Revisión de acceso](images/quality-assurance.png)

5.  Haga clic en **I'm good to go with the workflow**.

![Revisión de acceso](images/select-workflow.png)

6.  Seleccione **Seleccionaré mi propio flujo de trabajo**. Haga clic en **Siguiente**.
    
7.  Seleccione **¿Cuántos niveles de aprobación desea?** -> Flujo de trabajo de aprobación de un nivel.
    
8.  Seleccione **¿Quién es el primer revisor?** -> Propietario. Haga clic en Save y en Next
    

![Revisión de acceso](images/edit-workflow.png)

9.  En **¿Cómo desea programar la campaña?** -> Seleccione **Ejecutar ahora**. Proporcione **¿Cómo desea describir esta campaña?** y haga clic en **Siguiente**

![Revisión de acceso](images/create-workflow.png)

10.  Haga clic en Create. Ahora se ha creado la campaña.
    
11.  Para ver la campaña creada, vaya a Revisiones de acceso -> Campañas
    

![Revisión de acceso](images/campaign-name.png)

12.  Haga clic en la campaña para ver los detalles.

![Revisión de acceso](images/view-campaign.png)

13.  Haga clic en Detalles adicionales para ver más información sobre la campaña creada.

![Revisión de acceso](images/campaign-details.png)

14.  Vaya a Access Reviews -> My Access Reviews .

![Revisión de acceso](images/view-access-review-request.png)

15.  Verá solicitudes del usuario Harlan Bulllard, Mark Hernández y Jerry Poland para DB-Manage-Access. Haga clic en Ver para revisar las estadísticas de las solicitudes.

![Revisión de acceso](images/harlan-user.png)

![Revisión de acceso](images/jerry-user.png)

![Revisión de acceso](images/mark-user.png)

16.  En Acciones, haga clic en Aceptar y aceptar la solicitud para los usuarios Harlan Bullard, Mark Hernández y Jerry Poland.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, julio de 2023