# Realizar tareas de revisión de política

## Introducción

El revisor de campañas de Access Governance (Pamela Green) puede realizar tareas de revisión de políticas.

*   Tiempo estimado: 15 minutos
*   Persona: Revisor de campaña

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Examinar las tareas de revisión de políticas planteadas por la campaña
*   Evaluar las tareas de revisión de política que tiene asignadas como revisor de campaña

## Tarea 1: Realizar tareas de revisión de políticas

En esta tarea, revisará y certificará las tareas de revisión de OCI IAM generadas por la campaña creada en la tarea anterior.

1.  En el explorador, vaya a la consola de Oracle Access Governance mediante la URL mencionada en el _Laboratorio 3: Tarea 1_
    
2.  Introduzca el nombre de usuario y la contraseña del **revisor de campañas de Oracle Access Governance** (Pamela Green)
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

3.  En la página de inicio de la consola de Oracle Access Governance, en el menú de navegación, seleccione **Access Reviews -> My Access Reviews**.
    
4.  Para ver las tareas de revisión creadas por la campaña de revisión de políticas, haga clic en el separador **Tareas de revisión de políticas**. Verá todas las tareas de revisión de acceso a políticas que tiene asignadas como revisor. Oracle Access Governance utiliza un sistema de inteligencia basado en análisis interno para proporcionar recomendaciones de aceptación/revisión.
    

![Página inicial de Access Governance](images/my-access-reviews.png)

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
    
      - Let’s revoke the policy statement **Allow group Auditors to read audit-events in compartment Quality-Assurance** from the policy  **auditors-policy**. 
    
      ![Access Governance Homepage](images/auditor-policy.png)
    
      - Click on the cross button under Actions column for the policy statement **Allow group Auditors to read audit-events in compartment Quality-Assurance**
    
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
    

10.  Conéctese al dominio de identidad: consola de OCI de ag-domain como administrador de dominio de identidad. Navegue hasta Identidad y seguridad -> Identidad -> Políticas.

*   Verifique que toda la política: **security-admins-policy** se haya revocado correctamente de la lista de políticas.

Antes de realizar la revisión de políticas:

![Página inicial de Access Governance](images/before-security-policy.png)

Después de realizar la revisión de política:

![Página inicial de Access Governance](images/after-security-policy.png)

*   Verifique la sentencia de política: **Allow group Auditors to read audit-events in compartment Quality-Assurance** de la política: **auditors-policy** se ha revocado correctamente.

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

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023