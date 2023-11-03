# Despliegue de la pila para instalar OAS, la instancia del gateway de aplicación y el dominio de identidad

## Introducción

Con esta pila podremos desplegar/instalar la **versión 6.4 de OAS, el servidor del gateway de aplicación y el dominio de identidad**. El dominio de identidad creado será del tipo **Oracle Apps Premium**.

## Objetivos

1.  Despliegue y configuración de la **aplicación OAS versión 6.4**
2.  Desplegar **App Gateway Server**
3.  Despliegue el **dominio de identidad** del tipo **Oracle Apps Premium**
4.  Valide los recursos creados mediante el explorador web y el acceso SSH.

## Requisitos

Una vez descargado **Stack1- Deploy.zip**, descomprima el archivo zip y sustituya el contenido del archivo **SSH.key** por el contenido correspondiente de la clave privada.

**Nota** El nombre del archivo no se debe cambiar.

## Tarea 1: Despliegue de la pila mediante Resource Manager

1.  Una vez conectado a la consola de OCI, vaya a **Developer Services** y, a continuación, seleccione **Stacks** en **Resource Manager**. Ahora, haga clic en **Crear pila**.

**Nota** No seleccione el compartimento **Raíz** al crear la pila

![Captura 1](./images/image21.png "Captura 1")

![Captura 2](./images/image22.png "Captura 2")

2.  En el asistente Create Stack, seleccione la opción **.zip** y, a continuación, examine para cargar la pila **Deploy** que ha descargado en la práctica de laboratorio anterior. Ahora, haga clic en **Siguiente**.
    
    ![Imagen 1](./images/image1.jpg "Imagen 1")
    
    ![Imagen 2](./images/image2.jpg "Imagen 2")
    
    ![Imagen 3](./images/image3.jpg "Imagen 3")
    

**Nota** La pila selecciona automáticamente el directorio de trabajo, proporciona a la pila un nombre y se selecciona el compartimento de trabajo. El nombre de pila y el compartimento se pueden cambiar si es necesario.

3.  En la sección **Instancia informática de Oracle Anlytics Server**, proporcione el **Nombre mostrado** para la instancia de OAS y seleccione el **Compartimento** en el que se va a colocar la instancia. A continuación, seleccione el **dominio de disponibilidad** para la instancia. Seleccione la **unidad** y el **tamaño del volumen de inicio** para la instancia y, a continuación, cargue la **clave pública SSH**.
    
    ![Imagen 4](./images/image4.jpg "Imagen 4")
    

**Nota** La clave pública SSH se debe generar como requisito.

4.  En la sección **Configuración de red**, seleccione el **compartimento de VCN** que tiene la **red existente** y, a continuación, seleccione la **subred**. Seleccione la casilla de control **Asignar una dirección IP pública a la instancia informática** si ha seleccionado la subred pública.
    
    ![Imagen 5](./images/image5.jpg "Imagen 5")
    
5.  En la sección **Configuración de Dominio de Oracle Analytics Server**, seleccione la casilla de control para crear el dominio de OAS. Proporcione el **nombre de usuario de administrador de Analytics** y la **contraseña de administrador de Analytics**, este nombre de usuario y contraseña se utilizarán para conectarse a WebLogic desplegado en la instancia de OAS. A continuación, proporcione la **cadena de conexión de base de datos** en el formato especificado y la base de datos debe ser un sistema de base de datos de máquina virtual de Oracle Cloud. Especifique el **nombre de usuario de administrador de base de datos** del sistema de base de datos Oracle VM que ha creado. Además, especifique la **Contraseña de administrador de base de datos** para conectarse a la base de datos. A continuación, proporcione el **prefijo de esquema de base de datos** y la **contraseña de esquema de base de datos** para completar la configuración del dominio.
    
    ![Imagen 6](./images/image6.jpg "Imagen 6")
    
6.  En la sección **Instancia informática de gateway de aplicación**, seleccione el **compartimento de instancia** para colocar la instancia de gateway de aplicación. Proporcione el **nombre de la instancia** para el gateway de aplicación. A continuación, cargue la **clave pública SSH**. Ahora seleccione el **dominio de disponibilidad** para mantener la instancia. A continuación, seleccione **Compartimento de red** para seleccionar la **red existente** para el gateway de aplicación. Además, seleccione la **subred pública existente para la instancia**
    
    ![Imagen 7](./images/image7.jpg "Imagen 7")
    
7.  En la sección **Dominio de identidad de OCI**, seleccione el **compartimento** en el que desea crear el **dominio de identidad**. Proporcione un **nombre de dominio**, una **dirección de correo electrónico de administrador** y detalles de administrador básicos válidos. Ahora haga clic en **Siguiente**.
    
    ![Imagen 8](./images/image8.jpg "Imagen 8")
    
8.  Ahora, en **Revisar detalles**, compruebe las configuraciones y, a continuación, haga clic en **Crear**. Asegúrese de que la opción **Ejecutar aplicación** no esté seleccionada.
    
    ![Imagen 9](./images/image9.jpg "Imagen 9")
    
    ![Imagen 10](./images/image10.jpg "Imagen 10")
    
    ![Imagen 11](./images/image11.jpg "Imagen 11")
    
    ![Imagen 12](./images/image12.jpg "Imagen 12")
    
9.  En la pila creada, haga clic en la opción **Plan**. Debe obtener una salida **Correcto**.
    
    ![Imagen 13](./images/image13.jpg "Imagen 13")
    
    ![Imagen 14](./images/image14.jpg "Imagen 14")
    
10.  En la pila creada, haga clic en la opción **Aplicar**. Debe obtener una salida **Correcto**.
    
    ![Imagen 15](./images/image15.jpg "Imagen 15")
    

**Nota** El despliegue de pila tardará 2 minutos en completarse, pero la creación del dominio de OAS tardará entre 30 y 40 minutos aproximadamente.

## Tarea 2: Validación de los recursos creados.

Compruebe SSH en la instancia de OAS y en la instancia de gateway de aplicación.

_Con la clave privada de esta instancia, debe poder utilizar SSH en estos sistemas_

Una vez que la **pila** se haya desplegado correctamente, puede conectarse mediante SSH a la instancia de OAS y comprobar a continuación.

1.  Navegue hasta el directorio **/u01/app/oas-scripts** y busque el archivo **oas\_install.finish**. Este archivo indica que la instalación del dominio de OAS ha finalizado.
    
2.  Navegue hasta el directorio **/var/log** y compruebe los archivos log **oas\_cloudinit.log** y **oas\_create\_domain.log** para verificar que el dominio se haya creado correctamente.
    
3.  Comprobar la instancia de OAS
    

Intente acceder a su instancia de OAS a través de esta URL: _http://OAS\_Instance\_Public\_IP:9500/console_

**Nombre de usuario**: _Nombre de usuario de administrador de análisis_

**Contraseña**: _Contraseña de administrador de Analytics_

3.  Vaya a **Dominios** en **Identidad y seguridad** en la consola de OCI para validar que se ha creado el dominio de tipo **Oracle Apps Premium**.

## Conclusión

En este laboratorio, hemos podido desplegar y validar correctamente la aplicación de OAS, el servidor del gateway de aplicación y el dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Sagar Takkar, Chetan Soni
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Sagar Takkar de agosto de 2023