# Despliegue de la pila para instalar y EBS, EBS Asserter y el dominio de identidad

## Introducción

Con esta pila podremos desplegar/instalar **EBS - 12.2.11, servidor EBS Asserter y dominio de identidad**. El dominio de identidad creado será del tipo **Oracle Apps Premium**. En la aplicación EBS, configuraremos EBS con **Web Entry Point** y algunos usuarios de demostración.

## Objetivos

1.  Despliegue y configuración de la **aplicación EBS**
2.  Despliegue del **servidor EBS Asserter**
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

3.  Ahora, en la sección **Configurar variables**, cargue la **clave pública SSH**. Asegúrese de que se ha marcado **Políticas de OCI**
    
    ![Imagen 4](./images/image4.jpg "Imagen 4")
    

**Nota** La clave pública SSH se debe generar como requisito.

4.  En la sección **Red virtual en la nube**, seleccione el **compartimento de red** que tiene su **VCN** existente y, a continuación, seleccione el **compartimento de subred** que tiene su **subred pública** existente. Seleccione **Subred regional** en la opción **Intervalo de subred** y seleccione la subred regional existente en **Subred existente para Weblogic Server**
    
    ![Imagen 5](./images/image5.jpg "Imagen 5")
    
5.  En **Configuración de Dominio de Weblogic**, proporcione las **Credenciales de Weblogic** y seleccione **jdk8** en la opción **Java development Kit**.
    
    ![Imagen 6](./images/image6.jpg "Imagen 6")
    

**Nota** El nombre de usuario de Weblogic utiliza la contraseña almacenada en el _secreto del almacén_

6.  En la sección **Instancia informática de EBS**, seleccione el **compartimento** en el que desea desplegar la **aplicación de EBS**. Cargue **SSH Public** para la aplicación de EBS y seleccione la **subred pública existente** para la instancia de EBS.
    
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
    

**Nota** La pila puede tardar entre 30 y 40 minutos en ejecutarse. Espere hasta que se cree correctamente.

## Tarea 2: Validación de los recursos creados.

Una vez que la **pila** se haya desplegado correctamente, actualizaremos el archivo **Hosts** en nuestro sistema local y, a continuación, validaremos los recursos desplegados.

    1. Public Ip Address of Asserter  ebsasserter.example.com
    2. Public IP Address of EBS Instance  demoebs.example.com
    

**Nota** Utilice el valor **ebsasserter.example.com** para EBS Asserter y **demoebs.example.com** para la aplicación EBS en todo el laboratorio.

1.  Compruebe el acceso SSH a la instancia de EBS y al servidor de Asserter.

_Con la clave privada de esta instancia, debe poder utilizar SSH en estos sistemas_

2.  Comprobación de la instancia de EBS

Intente acceder a su instancia de EBS a través de esta URL: _http://demoebs.example.com:8000/OA\_HTML/AppsLogin_

**Nombre de usuario**: _SYSADMIN_

**Contraseña**: introduzca la _contraseña SYSADMIN_. Utilice _sysadmin@1234_ como contraseña para iniciar sesión.

3.  Vaya a **Dominios** en **Identidad y seguridad** en la consola de OCI para validar que se ha creado el dominio de tipo **Oracle Apps Premium**.

## Conclusión

En este laboratorio, hemos podido desplegar y validar correctamente la aplicación EBS, el servidor EBS Asserter y el dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023