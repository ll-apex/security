# Configurar y configurar la instancia de servicio Oracle Access Governance

## Introducción

En este laboratorio, configuraremos la instancia de servicio de OAG y realizaremos las configuraciones necesarias para ejecutar correctamente este taller.

Persona: administrador de dominio de identidad

_Tiempo estimado_: 30 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_21nk0xhx)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear instancia de servicio de AG
*   Acceder a la URL de la consola de AG
*   Asignación de roles de AG a usuarios en OCI IAM

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento válido de Oracle OCI, con privilegios de administrador de OCI.
*   Seleccione una región en la que Access Governance esté disponible.

## Tarea 1: Crear instancia de servicio de AG

Conéctese a la consola de OCI mediante el dominio de identidad: ag-domain como **Administrador de dominio de identidad**, si no está conectado actualmente al dominio de identidad.

1.  En la consola de OCI, haga clic en el icono Menú de navegación de la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Control de acceso_ en la lista de productos. Si no ve la opción de menú, compruebe la región seleccionada y asegúrese de que Access Governance está disponible en esa región.
    
    ![Crear Instancia de Servicio](images/oci-console.png)
    
2.  En la página Access Governance, seleccione _Instancias de servicio_.
    
        Name: ag-service-instance
        Description: Oracle Access Governance service instance
        Compartment: Ensure your ag-compartment is selected
        
    
    ![Crear Instancia de Servicio](images/create-service-instance.png)
    
3.  Seleccione el tipo de licencia: Access Governance para Oracle Workloads. Haga clic en _Crear instancia de servicio_
    
    ![Seleccionar tipo de licencia](images/license-type.png)
    
4.  Espere a que la instancia de servicio tenga el estado _Activo_. Anote esta URL, ya que la utilizaremos en las prácticas posteriores.
    
    ![La instancia de servicio está activa](images/ag-url.png)
    
5.  Haga clic en la instancia de servicio para acceder a la URL.
    
    ![Consola de Access Governance](images/ag-console.png)
    

## Tarea 2: Asignación de roles de aplicación de AG a usuarios

1.  Conéctese al dominio de identidad de la consola de OCI: ag-domain como administrador de dominio de identidad.
    
2.  En la consola de OCI, vaya a Identity -> Domains -> ag-domain -> Oracle Cloud Services -> AG-service-instance -> Application Role.
    
    *   Observe el rol de _administrador de AG_ y haga clic en la flecha hacia abajo en la esquina derecha.
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle.png)
    
    *   Haga clic en _Usuarios asignados -> Gestionar_. Seleccione _Verde Pamela_ en _Usuarios disponibles._ Haga clic en _Asignar_
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle-list.png)
    
    *   El usuario Pamela Green ahora está visible en _Usuarios asignados_.
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle-assign.png)
    
    *   Pamela Green ha sido asignada con el rol de aplicación _Administrador de AG_. Ahora puede cerrar la ventana.
3.  Ahora, observe el rol de _usuario de AG_ y el rol de _AG CloudAccessReviewer_ que se muestran. Asigne ambos roles a cada uno de los usuarios: _Mark Hernandez_, _Harlan Bullard_ y _Jerry Poland_.
    
    ![Roles de identidad y políticas de acceso de OIG](images/aguser.png)
    
    ![Roles de identidad y políticas de acceso de OIG](images/agreviewer.png)
    
    *   Haga clic en _Usuarios asignados -> Gestionar_. Seleccione _Mark Hernandez_, _Harlan Bullard_ y _Jerry Poland_ en _Usuarios disponibles._ Haga clic en _Asignar_. Los usuarios aparecen en _Usuarios asignados_:
    
    ![Roles de identidad y políticas de acceso de OIG](images/ag-userassign.png)
    
    ![Roles de identidad y políticas de acceso de OIG](images/ag-reviewerassign.png)
    
    *   Mark Hernández, Harlan Bullard y Jerry Poland ahora han sido asignados con los roles de aplicación _Usuario de AG_ y _AG CloudAccessReviewer_. Ahora puede cerrar la ventana.
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autores**: Anuj Tripathi, Anbu Anbarasu
*   **Última actualización por/fecha**: Anuj Tripathi, octubre de 2023