# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo realizar los cambios necesarios en la aplicación EBS para activar la función Inicio de sesión único. Además, crearemos un usuario administrador en la aplicación EBS y en el dominio de identidad de OCI, que se utilizará para probar el flujo de SSO.

_Tiempo de laboratorio estimado:_ 20 minutos

### Objetivos

*   Creación del administrador del sistema de Oracle E-Business Suite en el dominio de identidad de OCI IAM
*   Actualización de la dirección de correo electrónico del administrador del sistema de Oracle E-Business Suite
*   Actualizar perfiles de Oracle E-Business Suite
*   Reiniciar aplicación de EBS

### Requisitos

En este laboratorio se asume que tiene:

*   Ha realizado correctamente las prácticas anteriores
*   JDK 8 o posterior instalado en el sistema local

## Tarea 1: Creación del administrador del sistema de Oracle E-Business Suite en el dominio de identidad de OCI IAM

Cree un usuario en el dominio de identidad de Oracle IAM que corresponda al administrador del sistema en Oracle E-Business Suite (EBS); de lo contrario, el administrador del sistema no podrá conectarse a la consola de EBS después de que EBS esté configurado para utilizar el dominio de identidad de OCI IAM para la autenticación.

1.  Conéctese a los dominios de identidad de OCI IAM para acceder a la **consola de OCI**. Una vez conectado, **navegue** a **Dominios** en **Identidad y seguridad**. Ahora seleccione el **dominio de identidad** aprovisionado anteriormente.
    
    ![Imagen 9](./images/image9.png "Imagen 9")
    
2.  Haga clic en **Usuarios**, en la ventana **Agregar usuario**, proporcione los siguientes valores y, a continuación, haga clic en **Crear**.
    
    1.  **Nombre**: EBS
    2.  **Apellidos**: Sysadmin
    3.  Desmarque Usar la dirección de correo electrónico como nombre de usuario.
    4.  **Nombre de usuario**: sysadmin
    5.  **Email**: proporcione la dirección de correo electrónico definida en la cuenta SYSADMIN de Oracle E-Business Suite.
    
    ![Imagen 10](./images/image10.png "Imagen 10")
    

## Tarea 2: Actualización de la dirección de correo electrónico del administrador del sistema de Oracle E-Business Suite

Actualizar la dirección de correo electrónico del usuario SYSADMIN en Oracle E-Business Suite para que coincida con la dirección de correo electrónico que ha proporcionado para el usuario correspondiente en Oracle Identity Cloud Service.

1.  Inicie sesión como administrador (por ejemplo, sysadmin) en la aplicación Oracle E-Business Suite.
    
2.  En la página inicial de Oracle E-Business Suite, despliegue el **navegador**, amplíe **Gestión de usuarios** y, a continuación, haga clic en **Usuarios**.
    
3.  En la página **Mantenimiento de usuarios**, busque por nombre de usuario **SYSADMIN** y haga clic en el **icono de actualización** del usuario **SYSADMIN**.
    
    ![Imagen 11](./images/image11.png "Imagen 11")
    
4.  Actualice el valor del campo **Correo electrónico** con la misma dirección de correo electrónico que proporcionó durante la creación del usuario administrador del sistema en el dominio de identidad de OCI IAM y, a continuación, haga clic en **Aplicar**.
    
    ![Imagen 12](./images/image12.png "Imagen 12")
    
5.  Cierre la aplicación Oracle E-Business Suite.
    

## Tarea 3: Actualización de Perfiles de Oracle E-Business Suite

_Siga estos pasos para configurar Oracle E-Business Suite a fin de redirigir a usuarios no autenticados con E-Business-Suite a E-Business Suite Asserter en lugar de utilizar la página de inicio de sesión local de Oracle E-Business Suite._

1.  En la página de inicio de Oracle E-Business Suite, desplácese hacia abajo por el navegador, amplíe **Administrador funcional** y haga clic en el separador **Servicios principales** y, a continuación, haga clic en **Separador Perfiles**.
    
2.  Introduzca **App%Agent%** en el campo Buscar, Valores de perfil, Código y, a continuación, haga clic en **Buscar**.
    
    ![Imagen 18](./images/image18.png "Imagen 18")
    
3.  En la página Definir valores de perfil: **Agente de autenticación de aplicación**, introduzca **URL de E-Business Suite Asserter: https://ebsasserter.example.com:7004/ebs** en el campo Valor de sitio y, a continuación, **guarde**.
    
4.  Vuelva al separador **Perfiles**, introduzca **%SSO%Type%** en la búsqueda, actualice la entrada de código **APPS\_SSO** de **SSWA a SSWAw/SSO** y **guarde** el perfil.
    
    ![Imagen 19](./images/image19.png "Imagen 19")
    

5 Vuelva al separador **Perfiles**, introduzca **%Oracle Applications Session%** en la búsqueda, actualice el valor de **HOST** a **DOMINIO** y **guarde** el perfil.

![Imagen 20](./images/image20.png "Imagen 20")

**Nota** Para ejecutar la aplicación EBS y actualizar estos vaules de perfil, debe tener instalado JAVA en el sistema local.

## Tarea 4: Reinicio de Oracle E-Business Suite

Una vez realizados los cambios de perfiles, acceda al servidor de EBS y ejecute los siguientes comandos.

    $ sudo hostname demoebs.example.com
    # sudo su - oracle
    $ /u01/install/APPS/scripts/stopapps.sh
    $ /u01/install/APPS/scripts/startapps.sh
    
    

**Nota** Utilice el nombre de host mencionado anteriormente siempre que sea necesario.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023