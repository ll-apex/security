# Despliegue de la pila ORM para crear un servidor Linux y un dominio de identidad

## Introducción

Con esta pila podremos desplegar **Linux Server and Identity Domain** mediante Terraform. El dominio de identidad creado será del tipo **Oracle Apps Premium**. En el servidor Linux, configuraremos el **módulo de autenticación conectable (PAM) de Linux** mediante otra pila en el próximo laboratorio.

_Tiempo Estimado:_ 15 minutos

### Objetivos

*   Despliegue de un **servidor Linux**
*   Despliegue el **dominio de identidad** del tipo **Oracle Apps Premium**
*   Valide los recursos creados mediante el explorador web y el acceso SSH.

### Requisitos

*   Una vez descargado **Stack1- Deploy.zip**, descomprima el archivo zip y sustituya el contenido del archivo **SSH.key** y **SSH.key.pub** por el contenido correspondiente de la clave privada y la clave pública.

**Nota** El nombre del archivo no se debe cambiar.

## Tarea 1: Despliegue de la pila mediante Resource Manager

1.  Una vez conectado a la consola de OCI, vaya a **Developer Services** y, a continuación, seleccione **Stacks** en **Resource Manager**. Ahora, haga clic en **Crear pila**.
    
    ![Pilas](./images/stack.png "Pilas")
    
    ![Crear pilas](./images/create-stack.png "Crear pilas")
    
2.  En el asistente Create Stack, seleccione la opción **.zip** y, a continuación, examine para cargar la pila **Deploy** que ha descargado en la práctica de laboratorio anterior. Ahora, haga clic en **Siguiente**.
    
    ![cargar-zip](./images/upload-zip.png "cargar-zip")
    
    ![detalles de pila](./images/stack-details.png "detalles de pila")
    

**Nota** El nombre de pila y el compartimento se pueden cambiar si es necesario.

3.  Ahora, en la sección **Configurar variables**, seleccione el compartimento correspondiente en el que reside la VCN en la sección **Compartimento de instancia de Linux**, cargue la **clave pública SSH**. Seleccione el **dominio de disponibilidad**, la **VCN** y la **subred** respectivos donde se debe desplegar la instancia.
    
    ![detalles de instancia de linux](./images/linux-instance-details.png "detalles de instancia de linux")
    

**Nota** La clave pública SSH se debe generar como requisito.

4.  En la sección **Dominio de identidad de OCI**, seleccione el **compartimento de dominio de identidad** en el que se debe crear el dominio. A continuación, introduzca el nombre del dominio de identidad y proporcione detalles del administrador, como **Dirección de correo electrónico del administrador**, **Nombre** y **Apellido**.
    
    ![identidad-dominio-detalles](./images/identity-domain-details.png "identidad-dominio-detalles")
    
5.  Ahora, en **Revisar detalles**, compruebe las configuraciones y, a continuación, haga clic en **Crear**. Asegúrese de que la opción **Run Apply** (Ejecutar aplicación) esté seleccionada.
    
    ![revisión](./images/review.png "revisión")
    

**Nota** La pila puede tardar entre 1 y 2 minutos en ejecutarse. Espere hasta que se complete correctamente. Al finalizar, se enviará una notificación en la _Dirección de correo electrónico de administrador_ proporcionada anteriormente.

## Tarea 2: Validación de los recursos creados.

Una vez que la **pila** se haya desplegado correctamente, validaremos los recursos desplegados.

    1. Linux Server Instance
    2. Identity Domain 
    

1.  Compruebe el acceso SSH al servidor Linux mediante cualquier cliente SSH.

_Con la clave privada utilizada para la instancia, debe poder utilizar SSH en el sistema_

2.  Vaya a **Dominios** en **Identidad y seguridad** y al compartimento correspondiente en la consola de OCI para validar que se ha creado el dominio de tipo **Oracle Apps Premium**.

## Conclusión

En este laboratorio, hemos podido desplegar y validar correctamente un servidor Linux y un dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023