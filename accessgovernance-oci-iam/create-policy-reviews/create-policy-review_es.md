# Creación de campañas de revisión de políticas

## Introducción

Los administradores de Access Governance (Pamela Green) pueden crear una campaña de revisión de políticas.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de Access Governance

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación de campañas de revisión de políticas para políticas de OCI IAM

## Tarea 1: Crear una campaña de revisión de política

1.  En el explorador, vaya a la consola de Oracle Access Governance mediante la URL mencionada en el _Laboratorio 3: Tarea 1_
    
2.  Introduzca el nombre de usuario y la contraseña del **administrador de Oracle Access Governance** (Pamela Green)
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

3.  Desplácese hacia abajo y seleccione el mosaico **"Vamos a crear un trabajo y definir una nueva campaña"**. También puede seleccionar **Menú de navegación -> Revisiones de acceso -> Campañas.** En la página **Campañas**, haga clic en el botón **Crear campaña**.

![Página inicial de Access Governance](images/ag-homepage-campaign.png)

*   En el paso Criterios de selección, seleccione el mosaico **¿Qué proveedores de nube?**. Verá una lista de arrendamientos en la nube disponibles.

![Seleccionar proveedor de nube](images/select-cloud-providers.png)

*   Seleccione un arrendamiento en la nube adecuado. En este tutorial, seleccione su arrendamiento en la nube. Una marca verde está marcada contra su selección.

![Seleccionar proveedor de nube](images/green-tick-cloud-provider.png)

*   Haga clic en **Refinar más**. Puede acotar aún más la selección seleccionando un compartimento y un dominio específicos para ejecutar revisiones de políticas específicas del dominio.

![Seleccionar proveedor de nube](images/click-refine.png)

*   Introduzca los detalles de **dominio** y **compartimento** que se mencionan a continuación y haga clic en **Aplicar**
    
    *   dominio: ag-domain
    *   compartimento: ag-compartment

![Seleccionar proveedor de nube](images/click-apply-refine.png)

*   Vaya al paso siguiente para seleccionar las políticas que desea revisar. Seleccione el mosaico **¿Qué políticas?**. Verá una lista de políticas disponibles en el dominio seleccionado.

![Página inicial de Access Governance](images/select-which-policies.png)

*   Seleccione las políticas que desea revisar. En este tutorial, seleccione las siguientes políticas y haga clic en **Aplicar mis selecciones.**
    
    *   política de auditores
    *   política-admins-red
    *   política security-admins-
    
    ![Página inicial de Access Governance](images/select-the-policies.png)
    
*   Continúe con el paso **Asignar flujo de trabajo**. Para ello, haga clic en **Soy bueno, vaya a los flujos de trabajo.** Aquí puede definir el flujo de trabajo de aprobación para las tareas de revisión y hacer clic en **Siguiente**.
    

![Página inicial de Access Governance](images/choose-workflow.png)

![Página inicial de Access Governance](images/click-next-workflow.png)

*   En el paso **Agregar detalles**, puede definir la frecuencia (una vez o periódica) con la que se va a ejecutar una campaña de revisión de acceso, proporcionar un nombre significativo a la campaña, agregar una descripción complementaria y asignar valores a atributos adicionales, como quién la posee y cuándo debe comenzar o finalizar la campaña.
    
*   Para este tutorial, realice los siguientes cambios en el paso **Agregar detalles**:
    
    **¿Con qué frecuencia desea que se ejecute?**: Una vez
    
    **¿Qué nombre desea asignar a esta campaña?**: Policy-Review-OCI-IAM
    
    **¿Cómo desea describir esta campaña?**: Policy-Review-OCI-IAM
    
    **¿A quién pertenece esta campaña?**: Yo
    
    **¿Cómo desea programar la campaña?**: Ejecutar ahora (comenzará 10 minutos desde la creación)
    
*   Haga clic en **Siguiente**.
    

![Página inicial de Access Governance](images/campaign-information.png)

*   El paso **Revisar y enviar** muestra la información que ha agregado en los pasos anteriores. Seleccione **Crear** para crear la campaña. La campaña está programada y se muestra en la página **Campañas**. Se ejecutará a los 10 minutos de la creación.

![OCI Introducir detalles](images/click-create-new-campaign.png)

![OCI Introducir detalles](images/campaign-scheduled.png)

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023