# Creación de una aplicación confidencial en OCI IAM

## Introducción

Durante el proceso de ejecución de la pila, se crea un dominio de identidad de Oracle del tipo **Oracle Apps Premium**. La cuenta de administrador que utilizó recibirá un correo electrónico generado automáticamente para restablecer la contraseña de ese usuario respectivo. En este módulo, mostraremos los pasos para restablecer la contraseña de los usuarios administradores y el procedimiento para conectarse al dominio premium de Oracle Apps recién creado.

Este módulo también le ayudará a crear una aplicación confidencial en el dominio de identidad recién creado, que se utilizará para realizar llamadas a la API de REST en el backend.

_Tiempo estimado:_ 10 minutos

### Objetivos

*   **Restablezca** la contraseña del usuario administrador y conéctese al dominio de identidad recién creado.
*   Cree una **aplicación confidencial** en OCI IAM con el tipo de permiso permitido como _Credenciales de cliente_.

## Tarea 1: Restablecer la contraseña del usuario administrador e iniciar sesión en el dominio recién creado

1.  Durante el proceso de ejecución de pila, recibirá un correo electrónico generado automáticamente con el botón **Activar cuenta**. Haga clic en él.
    
    **Correo electrónico de ejemplo:** ![mensaje de correo electrónico de ejemplo](./images/sample-email.jpg "mensaje de correo electrónico de ejemplo")
    
2.  En la página que se carga, **restablezca la contraseña** de su cuenta de administrador en el dominio recién creado.
    
    ![restablecimiento de contraseña](./images/password-reset.jpg "restablecimiento de contraseña")
    
3.  Ahora, intente iniciar sesión en su nuevo dominio con la cuenta de administrador que acaba de completar el restablecimiento de contraseña.
    
    ![Conectar](./images/sign-in.jpg "Conectar")
    
4.  Después de una autenticación correcta, accederá a la página **Mi consola**, haga clic en la opción **Consola de OCI** del menú para ir a la pantalla **Consola de Infrastructure**.
    
    ![mi consola](./images/myconsole.jpg "mi consola")
    
5.  Debe aterrizar en la pantalla mencionada a continuación, que le proporciona los detalles sobre el dominio recién creado del tipo: **Oracle Apps Premium**
    
    ![visión general de dominio](./images/domain-overview.jpg "visión general de dominio")
    

## Tarea 2: Creación de una aplicación confidencial

Registraremos una aplicación confidencial en el dominio de OCI-IAM recién creado con el tipo de permisos permitidos **Credenciales de cliente**

1.  En el **dominio de identidad** recién creado, copie la **URL de dominio de identidad** de la sección **Información de dominio**. Haga clic en **Aplicaciones** en el menú de navegación.
    
    ![dominio-url](./images/domain-url.jpg "dominio-url")
    
    ![agregar solicitud](./images/add-application.jpg "agregar solicitud")
    

**Nota** Excluya **:443** del final de la **URL de dominio**

2.  Seleccione **Aplicación Confidencial** y, a continuación, haga clic en **Iniciar Flujo de Trabajo**
    
    ![creación-confidencial-aplicación](./images/create-confidential-application.jpg "creación-confidencial-aplicación")
    
3.  Agregue el nombre a la aplicación y, a continuación, haga clic en **Siguiente**
    
    ![nombre de aplicación](./images/app-name.jpg "nombre de aplicación")
    
4.  Seleccione **Credenciales de Cliente** como tipo de permiso y, a continuación, haga clic en **Siguiente**
    
    ![tipo de permiso](./images/grant-type.jpg "tipo de permiso")
    
5.  Seleccione **Administrador de dominio de identidad** en **Rol de aplicación** y **Agregar**.
    
    ![roles de aplicación](./images/app-roles.jpg "roles de aplicación")
    
6.  Finalmente, haga clic en **Terminar**.
    
    ![finalizar](./images/finish.jpg "finalizar")
    
7.  Haga clic en **Activar aplicación**
    
    ![activar](./images/activate.jpg "activar")
    
8.  Ahora, tome el **ID de cliente** y el **Secreto** para usarlo en configuraciones adicionales.
    
    ![secreto de ID de cliente](./images/client-id-secret.jpg "secreto de ID de cliente")
    

## Conclusión

En este laboratorio, hemos activado el usuario administrador en el dominio de identidad recién creado del tipo Oracle Apps Premium y, a continuación, hemos creado una aplicación confidencial para la API de REST, que se va a utilizar más en los scripts de Terraform.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023