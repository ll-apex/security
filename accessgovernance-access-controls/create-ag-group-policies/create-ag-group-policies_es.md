# Creación de usuarios y políticas de IAM para Access Governance

## Introducción

Crear política de IAM y usuarios para Access Governance.

*   Persona: Administrador de dominio predeterminado

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_x0vzlnhh)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar **política** para acceso de administrador de dominio
*   Usuarios de OCI IAM manualmente

## Tarea 1: Creación de una política de IAM

1.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Políticas_ en la lista de productos.
    
2.  En la página Políticas, haga clic en _Crear política_ para crear la política: ag-access-policy en el compartimento raíz.
    
        Name: ag-access-policy
        Description: IAM policy for granting Domain_Administrators access to manage access governance instances
        Compartment: Ensure your  root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statements :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage agcs-instance in compartment ag-compartment
        Allow group ag-domain/Domain_Administrators to read objectstorage-namespace in tenancy</copy>
        
    
    Haga clic en _Crear_.
    

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
    
        First Name: Jerry
        Last Name: Poland
        Username: jerry.poland
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    Haga clic en _Crear_.
    
5.  Desconéctese de la consola en la nube.
    
6.  Para cada usuario creado, se enviará un correo de activación al ID de correo electrónico proporcionado en la _Tarea 3: Paso 4_. Restablezca la contraseña de los 3 usuarios mediante el _correo de activación_ recibido para cada uno de ellos. Restablezca la contraseña a la siguiente contraseña:
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autores**: Anuj Tripathi, Anbu Anbarasu
*   **Última actualización por/fecha**: Anuj Tripathi, octubre de 2023