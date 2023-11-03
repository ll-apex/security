# Despliegue de la pila para configurar EBS, EBS Asserter y el dominio de identidad

## Introducción

Con esta pila podremos configurar **EBS, EBS Asserter Server and Identity Domain**. Como parte de esta pila, se creará una aplicación confidencial en **Dominio de identidad** y se crearán usuarios de ejemplo.

## Objetivos

1.  Configuración de la **aplicación de EBS - 12.2.11**
2.  Configurar el **servidor EBS Asserter**
3.  Cree la **aplicación confidencial** en **Dominio de identidad**

## Requisitos

Una vez descargado **Stack2- Configure.zip**, descomprima el archivo zip y sustituya el contenido de los archivos **.pem** (ebs.pem y ebsasserter.pem) por el contenido correspondiente de la clave privada.

## Tarea 1: Despliegue de la pila de configuración mediante Resource Manager

1.  Una vez conectado a la consola de OCI, vaya a **Servicios para desarrolladores** y seleccione **Pilas** en **Gestor de recursos**. Ahora, haga clic en **Crear pila**.

**Nota** No seleccione el compartimento **Raíz** al crear la pila

![Captura 1](./images/image10.png "Captura 1")

![Captura 2](./images/image11.png "Captura 2")

2.  En el asistente Create Stack, seleccione la opción **Stack 2- Configure.zip** y, a continuación, examine para cargar la pila **Deploy** que ha descargado en la práctica de laboratorio anterior. Ahora, haga clic en **Siguiente**.
    
    ![Imagen 1](./images/image1.jpg "Imagen 1")
    
    ![Imagen 2](./images/image2.jpg "Imagen 2")
    
    ![Imagen 3](./images/image3.jpg "Imagen 3")
    

**Nota** La pila selecciona automáticamente el directorio de trabajo, proporciona a la pila un nombre y se selecciona el compartimento de trabajo. El nombre de pila y el compartimento se pueden cambiar si es necesario.

3.  Ahora, en la sección **Configurar variables**, rellene los valores mencionados a continuación y, a continuación, haga clic en **Siguiente**.
    
    1.  _Dirección IP pública del servidor de afirmación_
    2.  _Dirección IP pública del servidor de EBS_
    3.  _Introduzca la contraseña WebLogic_. **Nota** Esta es la misma contraseña que ha colocado en el secreto del almacén que se utiliza en Stack1 - Deploy.zip
    4.  _URL de dominio de identidad_: URL de dominio del dominio desplegado. **Nota** Elimine **:443** del final de la URL del dominio.
    5.  _ID de cliente_: introduzca el ID de cliente de la aplicación confidencial de IDCS
    6.  _Secreto de cliente_: introduzca el secreto de cliente de la aplicación confidencial de IDCS
    
    ![Captura 3](./images/image12.png "Captura 3")
    
4.  Ahora, en **Revisar detalles**, compruebe las configuraciones y, a continuación, haga clic en **Crear**. Asegúrese de que la opción **Ejecutar aplicación** no esté seleccionada.
    
    ![Captura 4](./images/image13.png "Captura 4")
    
5.  En la pila creada, haga clic en la opción **Plan**. Debe obtener una salida **Correcto**.
    
    ![Imagen 7](./images/image7.jpg "Imagen 7")
    
    ![Imagen 8](./images/image8.jpg "Imagen 8")
    
6.  En la pila creada, haga clic en la opción **Aplicar**. Debe obtener una salida **Correcto**.
    
    ![Imagen 9](./images/image9.jpg "Imagen 9")
    

**Nota** La pila puede tardar unos 15 minutos en ejecutarse. Espere hasta que el **trabajo** se realice correctamente.

## Conclusión

En este laboratorio, hemos podido desplegar y configurar correctamente la aplicación EBS, el servidor EBS Asserter y el dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023