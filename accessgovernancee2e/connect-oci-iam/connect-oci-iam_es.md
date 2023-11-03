# Integración de Oracle Access Governance con OCI IAM

## Introducción

Como administradores de arrendamiento y administradores de control de acceso de OCI, pueden aprender a integrar Oracle Access Governance con OCI IAM.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Política de configuración para permitir que Oracle Access Governance conecte OCI
*   Configuración de una nueva conexión a OCI IAM Cloud Service en la consola de Oracle Access Governance

## Tarea 1: Política de configuración para permitir que Oracle Access Governance conecte OCI

1.  Conéctese al dominio de identidad de la consola de OCI: ag-domain como administrador de dominio de identidad.
    
2.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Identity and Security en el menú Navigation. Seleccione Políticas en la lista de productos.
    
3.  En la página Políticas, en el compartimento raíz, haga clic en Crear política para crear una política: oci-iam-policy
    
        Name: oci-iam-policy
        Description: Allow Oracle Access Governance to connect OCI in tenancy
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
    
        <copy>allow resource accessgov-agent resource-scanner to read all-resources in tenancy
        allow resource accessgov-agent resource-manager to manage domains in tenancy
        allow resource accessgov-agent resource-manager to manage policies in tenancy
        </copy>
        
    
    Haga clic en Create.
    

## Tarea 2: Configuración de una nueva conexión de OCI IAM Cloud Service en la consola de Oracle Access Governance

1.  En un explorador, vaya a la página inicial del servicio Oracle Access Governance y conéctese como usuario con el rol de aplicación Administrador.

Introducir nombre de usuario y contraseña de administrador de campaña de Oracle Access Governance (Pamela Green)

    **Username:**
    ```
    <copy>pamela.green</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

2.  En la página inicial del servicio Oracle Access Governance, haga clic en el icono del menú de navegación y seleccione **Administración de servicios → Sistemas conectados**.
    
3.  Seleccione el botón **Agregar un sistema conectado** en la página Sistemas conectados.
    
    ![Seleccionar proveedor de servicios en la nube](images/cloud-service-provider.png)
    
4.  Seleccione el mosaico **¿Desea conectarse a un proveedor de servicios en la nube?** haciendo clic en el botón Agregar.
    
5.  En el paso **Seleccionar sistema**, seleccione el mosaico **Oracle Cloud Infrastructure** y, a continuación, haga clic en **Siguiente**.
    

![Seleccionar proveedor de servicios en la nube](images/select-oci.png)

6.  Introduzca el nombre y la descripción del sistema conectado y, a continuación, haga clic en **Siguiente**.

Nombre: OCI-IAM

Descripción: OCI-IAM

![OCI Introducir detalles](images/enter-oci-system-name.png)

7.  Introduzca el OCID de arrendamiento y el identificador de región.

Para obtener el OCID de arrendamiento, vaya al perfil de usuario en la esquina superior derecha y haga clic en Arrendamiento. Observe el OCID de arrendamiento para su uso posterior.

![OCI Introducir detalles](images/navigate-tenancy.png)

![OCI Introducir detalles](images/tenancy-ocid.png)

Para obtener el identificador de región, consulte el siguiente enlace.

https://docs.oracle.com/en/cloud/paas/access-governance/cagsi

![OCI Introducir detalles](images/oci-iam-details.png)

8.  Haga clic en **Agregar.** Haga clic en Gestionar para ver el estado. Si los detalles de conexión se validan correctamente, verá el estado **Correcto** para la operación **Validar**. La operación de carga de datos completa puede tardar unos minutos, según los datos disponibles en su arrendamiento de OCI. La carga de datos incremental se ejecuta cada cuatro horas para que este sistema conectado sincronice los datos.

![Estado de conexión de OCI](images/oci-connection-status.png)

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023