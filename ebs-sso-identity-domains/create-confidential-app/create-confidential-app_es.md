# Activación del dominio de identidad y creación de una aplicación confidencial para llamadas de API de REST

## Introducción

Durante el proceso de ejecución de la pila, se crea un dominio de identidad de Oracle del tipo **Oracle Apps Premium**. La cuenta de administrador que utilizó recibirá un correo electrónico generado automáticamente para restablecer la contraseña de ese usuario respectivo. En este módulo, mostraremos los pasos para restablecer la contraseña de los usuarios administradores y el procedimiento para conectarse al dominio premium de Oracle Apps recién creado.

Este módulo también le ayudará a crear una aplicación confidencial en el dominio de identidad recién creado, que se utilizará para realizar llamadas a la API de REST en el backend.

## Objetivos

1.  **Restablezca** la contraseña del usuario administrador y conéctese al dominio de identidad recién creado
2.  Crear una aplicación confidencial con el tipo de permiso permitido **Credenciales de cliente**

## Tarea 1: Restablecer la contraseña del usuario administrador e iniciar sesión en el dominio recién creado

1.  Durante el proceso de ejecución de pila, recibirá un correo electrónico generado automáticamente con el botón **Activar cuenta**. Haga clic en él.
    
    **Correo electrónico de ejemplo:** ![Captura 1](./images/image1.jpg "Captura 1")
    
2.  En la página que se carga, **restablezca la contraseña** de su cuenta de administrador en el dominio recién creado.
    
    ![Captura 2](./images/image2.jpg "Captura 2")
    
3.  Ahora, intente iniciar sesión en su nuevo dominio con la cuenta de administrador que acaba de completar el restablecimiento de contraseña.
    
    ![Captura 3](./images/image3.jpg "Captura 3")
    
4.  Después de una autenticación correcta, accederá a la página **Mi consola**, haga clic en la opción **Consola de OCI** del menú para ir a la pantalla **Consola de Infrastructure**.
    
    ![Captura 4](./images/image4.jpg "Captura 4")
    
5.  Debe aterrizar en la pantalla mencionada a continuación, que le proporciona los detalles sobre el dominio recién creado del tipo: **Oracle Apps Premium**
    
    ![Captura 5](./images/image55.jpg "Captura 5")
    

## Tarea 2: Creación de una aplicación confidencial

Registraremos una aplicación confidencial en el dominio de OCI-IAM recién creado con el tipo de permisos permitidos **Credenciales de cliente**

1.  En el **dominio de identidad** recién creado, copie la **URL de dominio de identidad** de la sección **Información de dominio**. Haga clic en **Aplicaciones** en el menú de navegación.
    
    ![Imagen 5](./images/image5.jpg "Imagen 5")
    
    ![Captura 6](./images/image6.jpg "Captura 6")
    

**Nota** Excluya **:443** del final de la **URL de dominio**

2.  Seleccione **Aplicación Confidencial** y, a continuación, haga clic en **Iniciar Flujo de Trabajo**

![Captura 7](./images/image7.jpg "Captura 7")

3.  Agregue el nombre a la aplicación y, a continuación, haga clic en **Siguiente**

![Captura 8](./images/image8.jpg "Captura 8")

4.  Seleccione **Credenciales de Cliente** como tipo de permiso y, a continuación, haga clic en **Siguiente**

![Captura 9](./images/image9.jpg "Captura 9")

5.  Seleccione **Administrador de dominio de identidad** en **Rol de aplicación** y **Agregar**.

![Captura 10](./images/image10.jpg "Captura 10")

6.  Finalmente, haga clic en **Terminar**.

![Captura 12](./images/image12.jpg "Captura 12")

6.  Haga clic en **Activar aplicación**

![Captura 13](./images/image13.jpg "Captura 13")

8.  Ahora, tome el **ID de cliente** y el **Secreto** para usarlo en configuraciones adicionales.

![Captura 14](./images/image14.jpg "Captura 14")

## Conclusión

En este laboratorio, hemos activado el usuario administrador en el dominio de identidad recién creado del tipo Oracle Apps Premium y, a continuación, hemos creado una aplicación confidencial para la API de REST, que se va a utilizar más en los scripts de Terraform.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023