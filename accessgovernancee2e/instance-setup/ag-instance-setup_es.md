# Configurar y configurar la instancia del servicio Oracle Access Governance y crear usuarios en OCI IAM

## Introducción

En este laboratorio, configuraremos la instancia de servicio de OAG y realizaremos las configuraciones necesarias para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 30 minutos

_Persona_: administrador del dominio de identidad

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear instancia de servicio de AG
*   Acceder a la URL de la consola de AG
*   Creación de usuarios en OCI IAM

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
    

## Tarea 2: Creación de usuarios en OCI IAM

1.  Haga clic en el icono del menú de navegación situado en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Identity and Security en el menú Navigation. Seleccione Dominios de la lista de productos.
    
    ![Navegación a dominios](images/navigate-select-domain.png)
    
2.  En la página Dominios, haga clic en Dominio de identidad: _de nuevo dominio_ que ha creado.
    
    ![Navegación a dominios de identidad](images/open-domains.png)
    
    Seleccione _Usuarios_. Haga clic en _Crear usuario_.
    
    ![Navegar a usuarios](images/navigate-to-users.png)
    
3.  Desmarque "Usar la dirección de correo electrónico como nombre de usuario"
    
4.  Introduzca los siguientes detalles para crear 3 usuarios: Pamela Green (administrador de campaña), Harlan Bullard (mánager), Mark Hernández (usuario empleado) en IAM. Asegúrese de utilizar diferentes IDs de correo electrónico para diferentes usuarios.
    
        First Name: Pamela
        Last Name: Green
        Username: pamela.green
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-pamela.png)
    
    Haga clic en _Crear_.
    
        First Name: Harlan
        Last Name: Bullard
        Username: harlan.bullard
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-harlan.png)
    
    Haga clic en _Crear_.
    
        First Name: Mark
        Last Name: Hernandez
        Username: mhernandez
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-mark.png)
    
    Haga clic en _Crear_.
    
5.  Desconéctese de la consola en la nube.
    
6.  Para cada usuario creado, se enviará un correo de activación al ID de correo electrónico proporcionado en la _Tarea 3: Paso 4_. Restablezca la contraseña de los 3 usuarios mediante el _correo de activación_ recibido para cada uno de ellos. Restablezca la contraseña a la siguiente contraseña:
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        
7.  Conéctese al dominio de identidad de la consola de OCI: ag-domain como administrador de dominio de identidad.
    
    *   En la consola de OCI, vaya a Identity -> Domains -> ag-domain -> Oracle Cloud Services -> AG-service-instance -> Application Role.
        
    *   Observe el rol de _administrador de AG_ y el rol de _administrador de campaña de AG_ que se muestran. Haga clic en la flecha hacia abajo en la esquina derecha de cada uno de ellos.
        
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle.png)
    
    *   Haga clic en _Usuarios asignados -> Gestionar_. Seleccione _Verde Pamela_ en _Usuarios disponibles._ Haga clic en _Asignar_
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle-list.png)
    
    *   El usuario Pamela Green ahora está visible en _Usuarios asignados_.
    
    ![Roles de identidad y políticas de acceso de OIG](images/user-approle-assign.png)
    
    *   Pamela Green ha sido asignada con el rol de aplicación _Administrador de AG_ y _Administrador de campaña de AG_. Ahora puede cerrar la ventana.
        
    *   Ahora, observe el rol de _usuario de AG_ y el rol de _AG CloudAccessReviewer_ que se muestran. Haga clic en la flecha hacia abajo en la esquina derecha de cada uno de ellos.
        
        ![Roles de identidad y políticas de acceso de OIG](images/aguser.png)
        
        ![Roles de identidad y políticas de acceso de OIG](images/agreviewer.png)
        
    *   Haga clic en _Usuarios asignados -> Gestionar_. Seleccione _Mark Hernandez_ y _Harlan Bullard_ en _Usuarios disponibles._ Haga clic en _Asignar_
        
    
    ![Roles de identidad y políticas de acceso de OIG](images/ag-userassign.png)
    
    ![Roles de identidad y políticas de acceso de OIG](images/ag-reviewerassign.png)
    
    *   Mark Hernández y Harlan Bullard ahora tienen asignado el rol de aplicación _AG User_ y _AG CloudAccessReviewer_. Ahora puede cerrar la ventana.
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Edward Lu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023