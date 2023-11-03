# Integración de Oracle Access Governance con OCI IAM

## Introducción

Como **administradores de Access Governance**, pueden aprender a integrar Oracle Access Governance con OCI IAM.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de Access Governance

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configuración de una nueva conexión a OCI IAM Cloud Service en la consola de Oracle Access Governance

## Tarea 1: Configuración de una nueva conexión de OCI IAM Cloud Service en la consola de Oracle Access Governance

1.  En un explorador, vaya a la página inicial del servicio Oracle Access Governance mediante la URL indicada en el _Laboratorio 3: Tarea 1_ y conéctese como usuario con el rol de aplicación **Administrador de Access Governance**.

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
    
    ![Seleccionar proveedor de servicios en la nube](images/add-system.png)
    
4.  Seleccione el mosaico **¿Desea conectarse a un proveedor de servicios en la nube?** haciendo clic en el botón Agregar.
    

![Seleccionar proveedor de servicios en la nube](images/select-cloud-provider.png)

5.  En el paso **Seleccionar sistema**, seleccione el mosaico **Oracle Cloud Infrastructure** y, a continuación, haga clic en **Siguiente**.

![Seleccionar proveedor de servicios en la nube](images/select-oci.png)

6.  Introduzca el nombre y la descripción del sistema conectado y, a continuación, haga clic en **Siguiente**.

Nombre: OCI-IAM

Descripción: OCI-IAM

![OCI Introducir detalles](images/enter-oci-system-name.png)

![OCI Introducir detalles](images/enter-data.png)

7.  Para obtener la huella del usuario de OCI (agcs-user). Abra una **nueva ventana de explorador privado** e inicie sesión en el **dominio por defecto** de la consola de OCI como **administrador de dominio**.
    
8.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Dominios_ en la lista de productos.
    
    ![Navegación a dominios](images/navigate-domains.png)
    
9.  En la página Dominios, haga clic en _Dominio por defecto_. Asegúrese de que el **compartimento raíz** está seleccionado.
    
    ![Navegación a dominios](images/default-domain.png)
    
10.  Seleccione _Usuarios_. Haga clic en _agcs-user_.
    
    ![Crear usuario](images/select-users.png)
    

Desplácese hacia abajo, haga clic en **API keys**.

     ![OCI Enter details](images/api.png)
    

Haga clic en **Agregar clave de API**. Haga clic en **Generar par de claves de API**.

    ![OCI Enter details](images/add-api-key.png)
    

Haga clic en **Descargar clave privada** y **Descargar clave pública**.

![OCI Introducir detalles](images/click-add.png)

Haga clic en **Agregar**.

Anote la **clave privada descargada** en un editor de texto. Esto es necesario para el siguiente paso.

En **Vista previa de archivo de configuración**, anote los siguientes detalles necesarios para el siguiente paso.

*   OCID de usuario
*   Huella
*   OCID de arrendamiento
*   Región

![OCI Introducir detalles](images/config-file.png)

11.  Vuelva al explorador con Oracle Access Governance y continúe introduciendo los siguientes detalles mencionados a continuación:

**¿Cuál es el OCID del usuario de OCI?**: introduzca el identificador de Oracle Cloud (OCID) para el usuario de OCI (agcs-user) anotado en el paso anterior.

**¿Cuál es la huella del usuario de OCI?**: introduzca la huella de la clave pública de la clave de firma de API anotada en el paso anterior.

**¿Qué es la clave SSH privada del usuario de OCI?**: introduzca la clave SSH privada descargada (archivo .pem) del paso anterior para la clave de firma de API.

**¿Qué es el OCID del arrendamiento de OCI?**: introduzca el OCID del arrendamiento de destino anotado en el paso anterior.

**¿Qué es la región principal del arrendamiento de OCI?**: introduzca la región principal del arrendamiento de OCI de destino mediante el identificador de región anotado en el paso anterior.

![OCI Introducir detalles](images/details-entered.png)

12.  Haga clic en **Agregar.** Haga clic en Gestionar para ver el estado. Si los detalles de conexión se validan correctamente, verá el estado **Correcto** para la operación **Validar**. La operación de carga de datos completa puede tardar unos minutos, según los datos disponibles en su arrendamiento de OCI. La carga de datos incremental se ejecuta cada cuatro horas para que este sistema conectado sincronice los datos.

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