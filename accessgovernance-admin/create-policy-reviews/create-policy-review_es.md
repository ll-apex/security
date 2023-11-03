# Creación y revisión de campañas de revisión de políticas

## Introducción

Los administradores de Access Governance (Pamela Green) pueden crear y revisar campañas de revisión de políticas.

*   Persona: Administrador de Access Governance

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_yivv30ww)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación de campañas de revisión de políticas para políticas de OCI IAM
*   Examinar las tareas de revisión de políticas planteadas por la campaña
*   Evaluar las tareas de revisión de política que tiene asignadas como revisor de campaña

## Tarea 1: Crear una campaña de revisión de política

1.  En el explorador, vaya a la consola de Oracle Access Governance.
    
2.  Introduzca el nombre de usuario y la contraseña del **administrador de Oracle Access Governance** (Pamela Green)
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

3.  Desde la página de inicio de Oracle Access Governance, vaya a **Acceder a revisiones** -> **Campañas**.

![Página inicial de Access Governance](images/navigate-campaign.png)

4.  Haga clic en **Crear una campaña**.

![Página inicial de Access Governance](images/create-a-campaign.png)

5.  En **¿Qué tipo de campaña de revisión de acceso te gustaría hacer?** , seleccione el acceso de revisión a **Oracle Cloud Infrastructure**
    
    ![Página inicial de Access Governance](images/select-oci-campaign.png)
    
6.  En el paso Criterios de selección, seleccione el mosaico **¿Qué arrendamientos?**. Verá una lista de arrendamientos en la nube disponibles.
    
    ![Seleccionar proveedor de nube](images/select-tenancies.png)
    
7.  Seleccione un arrendamiento en la nube adecuado. En este tutorial, seleccione su arrendamiento en la nube. Una marca verde está marcada contra su selección.
    
    ![Seleccionar proveedor de nube](images/which-tenancy.png)
    
8.  Haga clic en **Refinar más**. Puede acotar aún más la selección seleccionando un compartimento y un dominio específicos para ejecutar revisiones de políticas específicas del dominio.
    
    ![Seleccionar proveedor de nube](images/click-refine.png)
    
9.  Introduzca los detalles de **compartimento** que se mencionan a continuación y haga clic en **Aplicar**
    
    compartimento: ag-compartment
    
    ![Seleccionar proveedor de nube](images/ag-compartment.png)
    
10.  Vaya al paso siguiente para seleccionar las políticas que desea revisar. Seleccione el mosaico **¿Qué políticas?**. Verá una lista de políticas disponibles en el dominio seleccionado.
    
    ![Página inicial de Access Governance](images/select-which-policies.png)
    
11.  Seleccione las políticas que desea revisar. En este tutorial, seleccione las siguientes políticas y haga clic en **Aplicar mis selecciones.**
    
          - auditors-policy
          - network-admins-policy
          - security-admins-policy
        
    
    ![Página inicial de Access Governance](images/select-the-policies.png)
    
12.  Continúe con el paso **Asignar flujo de trabajo**. Para ello, haga clic en **Soy bueno, vaya a los flujos de trabajo.** Aquí puede definir el flujo de trabajo de aprobación para las tareas de revisión y hacer clic en **Siguiente**.
    
    ![Página inicial de Access Governance](images/choose-workflow.png)
    
    ![Página inicial de Access Governance](images/click-next-workflow.png)
    
13.  En el paso **Agregar detalles**, puede definir la frecuencia (una vez o periódica) con la que se va a ejecutar una campaña de revisión de acceso, proporcionar un nombre significativo a la campaña, agregar una descripción complementaria y asignar valores a atributos adicionales, como quién la posee y cuándo debe comenzar o finalizar la campaña.
    
14.  Para este tutorial, realice los siguientes cambios en el paso **Agregar detalles**:
    
          **How often do you want this to run?** : One time
        
          **What do you want to call this campaign?**: Policy-Review-OCI-IAM
        
          **How do you want to describe this campaign?**: Policy-Review-OCI-IAM
        
          **Who owns this campaign?**: Me
        
          **How would you like to schedule your campaign?** : Run now (will start 10 minutes from creation)
        
15.  Haga clic en **Siguiente**.
    
    ![Página inicial de Access Governance](images/campaign-information.png)
    
16.  El paso **Revisar y enviar** muestra la información que ha agregado en los pasos anteriores. Seleccione **Crear** para crear la campaña. La campaña está programada y se muestra en la página **Campañas**. Se ejecutará a los 10 minutos de la creación.
    
    ![OCI Introducir detalles](images/click-create-new-campaign.png)
    
    ![OCI Introducir detalles](images/campaign-scheduled.png)
    

## Tarea 2: Realizar tareas de revisión de políticas

Aquí revisará y certificará las tareas de revisión de OCI IAM generadas por la campaña creada en la tarea anterior.

1.  En el explorador, vaya a la consola de Oracle Access Governance.
    
2.  Introduzca el nombre de usuario y la contraseña del **revisor de campañas de Oracle Access Governance** (Pamela Green)
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

3.  En la página de inicio de la consola de Oracle Access Governance, en el menú de navegación, seleccione **Access Reviews -> My Access Reviews**.
    
4.  Para ver las tareas de revisión creadas por la campaña de revisión de políticas, haga clic en el separador **Tareas de revisión de control de acceso**. Verá todas las tareas de revisión de acceso a políticas que tiene asignadas como revisor. Oracle Access Governance utiliza un sistema de inteligencia basado en análisis interno para proporcionar recomendaciones de aceptación/revisión.
    

![Página inicial de Access Governance](images/access-control-review-tasks.png)

5.  Para este tutorial, vamos a comprobar las recomendaciones proporcionadas por Oracle Access Governance.

*   network-admins-policy está marcado para revisión
*   security-admins-policy está marcado para revisión
*   auditores-política marcada para aceptar

6.  Veamos las estadísticas generadas por Oracle Access Governance. Para **auditors-policy**, haga clic en los enlaces **Acciones** correspondientes en la columna **Estadísticas**.
    
7.  En la página **Estadísticas**, puede ver nuestra recomendación para la tarea de revisión de políticas. En el panel izquierdo, puede ver la información de la política. A la derecha, puede ver una lista completa de sentencias de política accionables y no accionables, ver los detalles de política para ver a quién y a qué otorga acceso la sentencia de política y tomar las decisiones adecuadas en cada sentencia.
    

    ![Access Governance Homepage](images/view-policy.png)
    

8.  Junto a la sentencia Policy, haga clic en el botón View para ver los recursos asociados a las políticas. Haga clic en Summmary y Detalles para ver la información. Haga clic en Close.
    
    ![Página inicial de Access Governance](images/summary.png)
    
    ![Página inicial de Access Governance](images/details.png)
    
9.  Para tomar una decisión de revisión, puede revocar todas las sentencias accionables o aceptar todas las sentencias accionables de esa política a la vez, o bien tomar decisiones de forma individual en cada sentencia de política. Para este tutorial, validemos 2 casos de uso:
    

    **Usecase 1:**  Revoke policy statement from a policy - **auditors-policy**
    
      - Let’s revoke the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment** from the policy  **auditors-policy**. 
    
      ![Access Governance Homepage](images/auditor-policy.png)
    
      - Click on the cross button under Actions column for the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment**
    
      ![Access Governance Homepage](images/click-revoke-auditor-policy.png)
    
      - Click on **Apply**
    
      ![Access Governance Homepage](images/click-apply-auditor-policy.png)
    
      -  Enter justification for why you revoke the access review items and accept the remaining items then click on **Submit** This will trigger the auto-remediation process in the Oracle Access Governance system.
    
      ![Access Governance Homepage](images/policy-review-list-new.png)
    
    
    **Usecase 2:**  Revoke an entire policy - **security-admins-policy** 
    
      - Let's revoke the entire policy **security-admins-policy** 
    
       ![Access Governance Homepage](images/security-policy.png)
    
      - Click on the Revoke All button. 
    
       ![Access Governance Homepage](images/revoke-all-security-policy.png)
    
      - Click **Apply.** The **Confirmation** dialog box is displayed.
    
        ![Access Governance Homepage](images/apply-security-policy.png)
    
     - Provide justification and then click **Submit.** The closed loop access remediation will take place automatically.
    

10.  (Opcional) Conéctese al dominio de identidad: consola de OCI de ag-domain como administrador de dominio de identidad. Navegue hasta Identidad y seguridad -> Identidad -> Políticas.

*   Verifique que toda la política: **security-admins-policy** se haya revocado correctamente de la lista de políticas.

Antes de realizar la revisión de políticas:

![Página inicial de Access Governance](images/before-security-policy.png)

Después de realizar la revisión de política:

![Página inicial de Access Governance](images/after-security-policy.png)

*   Verifique la sentencia de política: **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment** de la política: **auditors-policy** se ha revocado correctamente.

Antes de realizar la revisión de políticas:

![Página inicial de Access Governance](images/before-auditor-policy.png)

Después de realizar la revisión de política:

![Página inicial de Access Governance](images/after-auditor-policy.png)

Con esto se concluye el tutorial sobre la creación y realización de revisiones de políticas de OCI IAM.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023