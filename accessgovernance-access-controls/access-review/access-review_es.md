# Realizar revisiones de acceso para la base de datos

## Introducción

Los usuarios pueden realizar revisiones de acceso desde la consola de Oracle Access Governance (Mark Hernández, Harlan Bullard, Jerry Poland) y las revisará el administrador de Access Governance (Pamela Green).

*   Persona: Revisor de campaña y Administrador de campaña

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_0sz90jrj)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear campaña como administrador de campaña
*   Aprobar solicitudes de revisión de acceso como administrador de campaña de Access Governance

## Tarea 1: Creación de una campaña

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

## Tarea 2: Revisar el acceso

1.  Vaya a Access Reviews -> My Access Reviews .
    
    ![Revisión de acceso](images/view-access-review-request.png)
    
2.  Verá las tareas de revisión de acceso para el usuario Harlan Bulllard, Mark Hernández y Jerry Poland para DB-Manage-Access. Haga clic en Ver para revisar las estadísticas de las tareas de revisión.
    

![Revisión de acceso](images/harlan-user.png)

![Revisión de acceso](images/jerry-user.png)

![Revisión de acceso](images/mark-user.png)

3.  En Acciones, haga clic en Aceptar a la solicitud para los usuarios Harlan Bullard, Mark Hernández y Jerry Poland.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autores**: Anuj Tripathi
*   **Contribuyentes**: Anbu Anbarasu
*   **Última actualización por/fecha**: Anuj Tripathi, octubre de 2023