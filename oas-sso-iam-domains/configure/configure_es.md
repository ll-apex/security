# Despliegue la pila para configurar OAS, AppGate y el dominio de identidad

## Introducción

Con esta pila podremos configurar **OAS, gateway de aplicación y dominio de identidad**. Como parte de esta pila, se creará una aplicación empresarial en **Dominio de identidad**.

## Objetivos

1.  Configurar \*\*Aplicación de OAS \*\*
2.  Configuración del **gateway de aplicaciones**
3.  Cree la **aplicación empresarial** en **dominio de identidad**

## Requisitos

Una vez descargado **Stack2- Configure.zip**, descomprima el archivo zip y sustituya el contenido de los archivos **.pem** (AppGate\_PrivateKey.pem y OAS\_PrivateKey.pem) por el contenido correspondiente de la clave privada.

## Tarea 1: Despliegue de la pila de configuración mediante Resource Manager

1.  Una vez conectado a la consola de OCI, vaya a **Servicios para desarrolladores** y seleccione **Pilas** en **Gestor de recursos**. Ahora, haga clic en **Crear pila**.

**Nota** No seleccione el compartimento **Raíz** al crear la pila

    ![stacks](./images/stacks.png "stacks")
    
    ![create-stacks](./images/create-stacks.png "create-stacks")
    

2.  En el asistente Create Stack, seleccione la opción **Stack 2- Configure.zip** y, a continuación, examine para cargar la pila **Deploy** que ha descargado en la práctica de laboratorio anterior. Ahora, haga clic en **Siguiente**.
    
    ![browse_zip ](./images/browse_zip.jpg "browse_zip")
    
    ![uploaded_zip](./images/uploaded_zip.jpg "uploaded_zip")
    
    ![default_details](./images/default_details.jpg "default_details")
    

**Nota** La pila selecciona automáticamente el directorio de trabajo, proporciona a la pila un nombre y se selecciona el compartimento de trabajo. El nombre de pila y el compartimento se pueden cambiar si es necesario.

3.  Ahora, en la sección **Configurar variables**, rellene los valores mencionados a continuación y, a continuación, haga clic en **Siguiente**.
    
    1.  _Dirección IP pública del servidor AppGate_
    2.  _URL del dominio de identidad: URL del dominio desplegado. **Nota** Elimine **:443** del final de la URL del dominio._
    3.  _ID de cliente: introduzca el ID de cliente de la aplicación confidencial de IDCS_
    4.  _Secreto de cliente: introduzca el secreto de cliente de la aplicación confidencial de IDCS_
    5.  _URL de la página de llegada de OAS: se conectará desde la pila uno después de desplegar el servidor de OAS_
    6.  _URL de servidor de origen de OAS: se conectará desde la pila uno después de desplegar el servidor de OAS_
    7.  _Dirección IP de Weblogic de OAS: se conectará desde la pila uno después de desplegar el servidor de OAS_
    8.  _Nombre de usuario de administración de Weblogic de OAS: el mismo nombre de usuario que utilizó en Satck one_
    9.  _Introduzca la contraseña WebLogic: el mismo nombre de usuario que utilizó en Satck one_
    
    ![configure_variables](./images/configure_variables.png "configure_variables")
    
4.  Ahora, en **Revisar detalles**, compruebe las configuraciones y, a continuación, haga clic en **Crear**. Asegúrese de que la opción **Ejecutar aplicación** no esté seleccionada.
    
    ![revisión](./images/review.png "revisión")
    
    ![created_stack](./images/created_stack.png "created_stack")
    
5.  En la pila creada, haga clic en la opción **Plan**. Debe obtener una salida **Correcto**.
    
    ![initiate_plan](./images/initiate_plan.png "initiate_plan")
    
    ![plan_successful](./images/plan_successful.png "plan_successful")
    
6.  En la pila creada, haga clic en la opción **Aplicar**. Debe obtener una salida **Correcto**.
    
    ![initiate_apply](./images/initiate_apply.png "initiate_apply")
    
    ![apply_successful](./images/apply_successful.png "apply_successful")
    

**Nota** La pila puede tardar unos 35 minutos en ejecutarse. Espere hasta que el **trabajo** se realice correctamente.

## Conclusión

En este laboratorio, hemos podido desplegar y configurar correctamente la aplicación de OAS, el servidor del gateway de aplicación y el dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Chetan Soni, Sagar Takkar
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Chetan Soni de agosto de 2023