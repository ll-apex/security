# Creación de grupos y políticas para Access Governance

## Introducción

Cree políticas para Access Governance.

*   Persona: Administrador de dominio predeterminado

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_x0vzlnhh)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear **agcs-user** para Access Governance
*   Configurar **grupos** para Access Governance
*   Configurar **políticas** para Access Governance
*   Configurar la **política** para permitir que Oracle Access Governance conecte OCI
*   Configurar **política** para acceso de administrador de dominio

## Tarea 1: Crear usuario de AGCS

1.  Conexión al **dominio por defecto** de la consola de OCI como **administrador de dominio por defecto**
    
2.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Dominios_ en la lista de productos.
    
    ![Navegación a dominios](images/navigate-domains.png)
    
3.  En la página Dominios, haga clic en _Dominio predeterminado_. Asegúrese de que el **compartimento raíz** está seleccionado.
    
    ![Navegación a dominios](images/default-domain.png)
    
4.  Seleccione _Usuarios_. Haga clic en _Crear usuario_.
    
    ![Crear usuario](images/select-users.png)
    
    ![Crear usuario](images/create-user.png)
    
    Introduzca los siguientes detalles para crear _agcs-user_
    
        First name: agcs
        Last name: user
        Use the email address as the username: Uncheck the checkbox 
        Username: agcs-user
        Email: agcsuser@example.com
        
    
    ![Crear usuario](images/createuser-tab.png)
    
    Haga clic en _Crear_.
    
    Se ha creado _agcs-user_.
    

## Tarea 2: Crear Grupo AG

1.  Conexión al **dominio por defecto** de la consola de OCI como **administrador de dominio por defecto**
    
2.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Dominios_ en la lista de productos.
    
    ![Navegación a dominios](images/navigate-select-domain.png)
    
3.  En la página Dominios, haga clic en _Dominio predeterminado_. Asegúrese de que el **compartimento raíz** está seleccionado. Seleccione _Grupos_. Haga clic en _Crear grupo_
    
    ![Seleccionar el dominio de identidad](images/default-domain.png)
    
    ![Seleccionar Grupos](images/select-group.png)
    
    Introduzca los siguientes detalles para crear el grupo _agcs-group_ y asignar el usuario **agcs-user** al grupo
    
        Name: agcs-group
        Description: Access governance group to manage users 
        Users: Select the agcs-user 
        
    
    Haga clic en _Crear_.
    
    ![Crear grupo AG](images/creategroup-tab.png)
    
    El _grupo_ se creó correctamente.
    

## Tarea 3: Crear políticas de AG

1.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Políticas_ en la lista de productos.
    
2.  En la página Políticas, haga clic en _Crear política_ para crear la política: ag-access-policy en el compartimento raíz.
    
        Name: ag-access-policy
        Description: IAM policy for granting Domain_Administrators access to manage access governance instances
        Compartment: Ensure your  root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage agcs-instance in compartment ag-compartment
        Allow group ag-domain/Domain_Administrators to read objectstorage-namespace in tenancy</copy>
        
    
    Haga clic en _Crear_.
    
    En la página Policies, en el compartimento raíz, haga clic en Create Policy para crear una política: agcs-policy
    
        Name: agcs-policy
        Description: Oracle Access Governance policy 
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
    
        <copy>ALLOW GROUP agcs-group to read all-resources IN TENANCY
        ALLOW GROUP agcs-group to manage policies IN TENANCY
        ALLOW GROUP agcs-group to manage domains IN TENANCY
        </copy>
        
    
    Haga clic en Create.
    
    En la página Políticas, en el compartimento raíz, haga clic en Crear política para crear la política: domain-admin-policy
    
        Name: domain-admin-policy
        Description: IAM policy (domain-admin-policy) in the COMPARTMENT to give access to the Identity Domain admin for the compartment created
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage all-resources in compartment ag-compartment</copy>
        
    
    Haga clic en _Crear_.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, junio de 2023