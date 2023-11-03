# Validar

## Introducción

Este laboratorio le mostrará cómo puede probar el flujo de SSO en la aplicación de OAS a través de **dominios de identidad de OCI IAM**

### Objetivos

*   Crear un **usuario de Analytics** en el nuevo **dominio de identidad** creado _Por ejemplo: usuario administrador de WebLogic_
*   Validación de la configuración del gateway de aplicación
*   Prueba de inicio de sesión único con Oracle Analytics Server

## Tarea 1: Crear un usuario administrador para probar el inicio de sesión único.

1.  Navegue al dominio de identidad recién creado y haga clic en la opción **Usuarios** del menú de la izquierda.
    
    ![directorio raíz del dominio](./images/domain-home.png "directorio raíz del dominio")
    
2.  Ahora, haga clic en **Crear usuario** y agregue los siguientes detalles para el usuario y **Crearlo**.
    
    1.  _Nombre_: OAS
    2.  _Apellido_: Administrador
    3.  Desactive **Usar la dirección de correo electrónico como nombre de usuario**
    4.  _Nombre de usuario_: administrador
    5.  _Correo electrónico_: correo electrónico válido para restablecer la contraseña de este usuario
    
    ![crear usuario](./images/create-user.png "crear usuario")
    

**Nota:** El **nombre de usuario** aquí debe ser el nombre de usuario administrador de WebLogic que ha creado durante el despliegue de la **pila 1**.

3.  En la nube recibirá un correo electrónico para **activar** el usuario administrador. Active el usuario haciendo clic en la opción **Restablecer Contraseña** del correo electrónico.

## Tarea 2: Validación de la configuración del gateway de aplicación

Una vez que la **pila 2- Configuración** se haya desplegado correctamente y se haya creado y activado el **usuario administrador**, acceda a la URL del gateway de aplicación que se menciona a continuación.

*   **https://publicIP\_App Puerta de enlace:4443/analytics**

El explorador debe remitirlo a la página de conexión de dominios de identidad.

## Tarea 3: Prueba de conexión única con Oracle Analytics Server a través del gateway de aplicación

1.  Probar SSO mediante el enlace del gateway de aplicación
    
    1.  Abra una ventana del explorador e introduzca la URL del gateway de aplicación: **https://publicIP\_App gateway:4443/analytics**
        
    2.  Aparece la página **Firma de dominios de identidad de OCI IAM**. Use el **Nombre de usuario y contraseña** del usuario creado previamente para conectarse.
        
    3.  Tras la autenticación correcta, se redirige al usuario a la página inicial de **Oracle Analyitcs Server** sin tener que introducir **Credenciales de OAS**.
        
        ![oas-página de inicio](./images/oas-home-page.png "oas-página de inicio")
        
    4.  Si aparece la página de inicio de **Oracle Analytics Server**, verifique el **nombre de usuario conectado** haciendo clic en la opción **Mi cuenta**.
        
        ![verificar-usuario de conexión](./images/verify-loggedin-user.png "verificar-usuario de conexión")
        
    5.  Desconéctese de Oracle OAS. El explorador se redirige a la página **Conexión a dominios de identidad de OCI IAM**.
        

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Chetan Soni, Sagar Takkar
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Chetan Soni de agosto de 2023