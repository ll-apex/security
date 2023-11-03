# Desplegar la pila ORM para configurar el módulo PAM de Linux y el dominio de identidad

## Introducción

Con esta pila podremos configurar **Linux Server and Identity Domain**. Como parte de esta pila, se creará una aplicación confidencial en **Dominio de identidad** y se crearán usuarios de ejemplo.

_Tiempo estimado:_ 20 minutos

### Objetivos

*   Instalación y configuración de **Linux-PAM mediante SSSD**
*   Cree la **aplicación confidencial** en **dominio de identidad** y cree los usuarios de _POSIX_ de ejemplo y un grupo POSIX.

### Requisitos

*   Una vez descargado el archivo **Stack2- Configure.zip**, utilice el archivo **SSH.key** actualizado, que se creó al desplegar **Stack1- Deploy.zip**.

## Tarea 1: Despliegue de la pila de configuración mediante Resource Manager

1.  Una vez conectado a la consola de OCI, vaya a **Servicios para desarrolladores** y seleccione **Pilas** en **Gestor de recursos**. Ahora, haga clic en **Crear pila**.
    
    ![pilas](./images/stacks.png "pilas")
    
    ![crear pilas](./images/create-stacks.png "crear pilas")
    
2.  En el asistente Create Stack, seleccione la opción **Stack 2- Configure.zip** y, a continuación, examine para cargar la pila **Deploy** que ha descargado en la práctica de laboratorio anterior. Ahora, haga clic en **Siguiente**.
    
    ![cargar](./images/upload.png "cargar")
    
3.  Ahora, en la sección **Configurar variables**, rellene los valores mencionados a continuación y, a continuación, haga clic en **Siguiente**.
    
    ![configurar-variables](./images/configure-variables.png "configurar-variables")
    
    1.  _Dirección IP pública de la instancia informática de Linux_ creada anteriormente
    2.  _URL de dominio de identidad_: URL de dominio del dominio desplegado. **Nota** Elimine **:443** del final de la URL del dominio.
    3.  _ID de cliente_: introduzca el ID de cliente de la aplicación confidencial de OCI IAM
    4.  _Secreto de cliente_: introduzca el secreto de cliente de la aplicación confidencial de OCI IAM
4.  Ahora, en **Revisar detalles**, compruebe las configuraciones y, a continuación, haga clic en **Crear**. Asegúrese de que la opción **Run Apply** (Ejecutar aplicación) esté seleccionada.
    
    ![revisión](./images/review.png "revisión")
    

**Nota** La pila puede tardar unos 15 minutos en completarse. Espere hasta que el **trabajo** se realice correctamente.

## Conclusión

En este laboratorio, hemos podido desplegar y configurar correctamente el módulo de autenticación conectable (PAM) de Linux en el servidor de Linux y configurar el dominio de identidad para crear una aplicación confidencial y usuarios _POSIX_ y un grupo _POSIX_.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023