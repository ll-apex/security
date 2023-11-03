# Configurar el entorno

## Introducción

Oracle Identity Cloud Service (IDCS) proporciona integración con cualquier servicio que se pueda integrar mediante el protocolo _SAML_ (Security Access Markup Language). Las administraciones podrán gestionar usuarios en varias aplicaciones a través de un único panel de control y los usuarios finales podrán acceder a las aplicaciones con un solo clic.

IDCS proporciona soporte para perfiles de conexión y desconexión de POST del explorador SAML 2.0 estándar.

Tenga en cuenta que, aunque IDCS soporta SAML y OpenID Connect/OAuth, hay muchas veces en las que un sistema no activado para SAML necesita SSO. IDCS App Gate proporciona soporte para sistemas no SAML que utilizan SSO basado en cabecera, basado en cookies o de relleno de formulario. App Gate es un dispositivo de software basado en Nginx desplegado en una configuración de proxy que se puede desplegar en AWS, OCI, OCI-C o localmente en VMWare u Oracle Virtualbox. Se entrega a través de Identity Cloud Service y está disponible para los clientes de IDCS Standard.

Este laboratorio está destinado a demostrar conexión única (SSO) federada con una aplicación SaaS de 3a parte. El objetivo es ilustrar el valor de negocio de tener IDCS como proveedor de identidad central para su creciente cartera de productos y servicios de Oracle y no Oracle.

Tiempo de laboratorio estimado: 90 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar aplicación de Salesforce
*   asignar aplicaciones a grupo
*   solicitar acceso a grupo
*   verificar configuración de SSO
*   Configurar aprovisionamiento y sincronización

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Una cuenta de Google
*   Una cuenta de desarrollador de Salesforce

## Tarea 1: Configuración de la aplicación Salesforce

*   _Personas_:
    *   Administrador

En este ejercicio práctico, configuraremos la integración con _Salesforce_ mediante SAML. IDCS actuará como _IdP_ (proveedor de identidad) y la organización de Salesforce como _SP_ (proveedor de servicios también conocido como usuario de confianza)

1.  Descargue metadatos de IDCS en un archivo XML local. Los metadatos están disponibles en la siguiente ubicación:
    
        https://<your tenant>/fed/v1/metadata
        
    
    Según el navegador que esté utilizando, la forma más sencilla de hacerlo es hacer clic con el botón derecho en cualquier parte de la página y elegir la opción "Guardar como", proporcionar un nombre y guardarlo como archivo XML. Trate de no utilizar la opción copiar / pegar como el archivo xml puede ser alterado.
    
    ![Imagen](images/L2001.png)
    

**Los pasos 2 a 8 siguientes se trataron en la sección de requisitos del taller**

2.  Regístrese para obtener una [cuenta de desarrollador de Salesforce](https://developer.salesforce.com).
    
    ![Imagen](images/L2002.png)
    
    Una vez registrado, compruebe su correo electrónico y verifique el registro de la cuenta y luego inicie sesión en Salesforce. Tenga en cuenta que puede que tenga que seleccionar "Cambiar a Salesforce Classic".
    
3.  Acceda a la aplicación Salesforce mediante la URL proporcionada después del registro, haga clic en el menú de perfil según la siguiente pantalla de impresión y seleccione la opción: _Cambiar a Salesforce Classic_.
    
    ![Imagen](images/L2003.png)
    
4.  Vaya a _Configuración_ en el menú del separador superior, cerca del perfil.
    
    ![Imagen](images/L2004.png)
    
5.  Registre un dominio personalizado. Vaya a _Gestión de dominios_ y seleccione _Mi dominio_ y registre un dominio de su elección. Esto es para tener una URL de aplicación personalizada dedicada a nuestro caso de uso.
    
6.  _Compruebe la disponibilidad_ del nuevo dominio.
    
7.  _Registre_ el nuevo dominio.
    
    ![Imagen](images/L2005.png)
    
8.  Salesforce actualiza sus registros de dominio con el nuevo. Cuando haya terminado, recibirá un correo electrónico de confirmación. Puede tardar unos minutos antes de que el dominio esté disponible.
    
9.  En Administración de dominios, haga clic en Dominios y haga clic en su dominio recién creado.
    
    ![Imagen](images/L2006.png)
    
10.  Cuando se registre su nuevo dominio, anote la URL de su dominio
    
    ![Imagen](images/L2007.png)
    
11.  Inicie sesión con el nuevo dominio mediante la URL anterior y, a continuación, vuelva a _Gestión de dominios_, haga clic en _Dominios_ y, a continuación, haga clic en el dominio creado recientemente.
    
12.  Haga clic en _Desplegar en usuarios_. Cuando aparezca la ventana emergente, haga clic en _OK_ (Aceptar).
    
    ![Imagen](images/L2008.png)
    
13.  El dominio está configurado y desplegado para los usuarios. Asegúrese de que puede ver en Mi dominio que el paso 4 está completo y muestra _Dominio desplegado en usuarios_.
    
14.  En la barra de menús lateral, vaya a _Controles de seguridad_ y seleccione _Configuración de conexión única_.
    
15.  Haga clic en _Editar_ y active la opción _Conexión única federada mediante SAML_. Haga clic en _Guardar_.
    
    ![Imagen](images/L2010.png)
    
        We need to activate the SAML option on Salesforce in order to be able to integrate with IDCS via these open standards.  Also, the SAML metadata needs to be exchanged between IDCS and Salesforce
        
16.  Haga clic en el botón _Nuevo desde Archivo de Metadatos_ para importar metadatos de IDCS. Seleccione el archivo de metadatos descargado del paso 1 mediante el botón _Seleccionar archivo_. Haga clic en _Crear_.
    
    ![Imagen](images/L2011.png)
    
        IDCS metadata file contains information about IDCS endpoints for SSO, single logout, Entity ID, Issuer details, IDCS signing certificate etc which are needed by Salesforce in order to establish a federated trust with IDCS.
        
17.  Mantenga toda la información por defecto y haga clic en _Guardar_.
    
    ![Imagen](images/L2012.png)
    
    A continuación se muestran los detalles de este ejemplo. Tenga en cuenta que las URL y la información del dominio serán diferentes.
    
    ![Imagen](images/L2013.png)
    
18.  Tome nota de lo siguiente:
    

*   Valor de nombre de dominio, por ejemplo: prodsys-dev-ed
    
*   (Opcional) Valor de ID de organización, por ejemplo: 00D4J000000DND9
    
    ![Imagen](images/L2014.png)
    

Nota: Consulte el paso siguiente en caso de que el valor de ID de organización no se muestre en el campo URL de conexión.

19.  En caso de que el ID de organización no se muestre en la URL de conexión, haga clic en _Perfil de compañía_ y, a continuación, en _Información de compañía_ para recuperar el _ID de organización_.
    
    ![Imagen](images/L2015.png)
    

Salesforce se ha configurado correctamente. El siguiente paso es configurar IDCS.

## Tarea 2: Crear aplicación IDCS de Salesforce

*   _Personas_:
    *   Administrador

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![Imagen](images/L2016.png)
    
2.  Seleccione el separador Applications en el panel de control de IDCS que se muestra después de conectarse.
    
    ![Imagen](images/L2017.png)
    
3.  Haga clic en el botón _Agregar_ para crear una nueva aplicación.
    
    ![Imagen](images/L2018.png)
    
4.  Seleccione _App Catalog_.
    
    ![Imagen](images/L2019.png)
    
        Note that IDCS provides an application template for Salesforce.
        The App Catalog is a collection of partially configured application templates that Oracle creates and maintains for you. You can use the templates to define the application, configure Single Sign-On, and enable and configure provisioning and synchronization. The App Catalog templates allow you to onboard applications quickly and securely.
        
        App templates simplify adding a new application by prepopulating all common attributes and values so that you just have to enter your tenant specific details.
        
5.  En el _Catálogo de aplicaciones_, busque _Salesforce_. Haga clic en _Agregar_.
    
    ![Imagen](images/L2020.png)
    
6.  En la primera página de la pantalla de configuración, proporcione el valor _Nombre de dominio_ del que tomó nota desde Salesforce. También puede agregar el _ID de organización_. Haga clic en _Siguiente_.
    
7.  Haga clic de nuevo en _Siguiente_ cuando esté en la pantalla _Configuración de SSO_.
    
    ![Imagen](images/L2021.png)
    
8.  Haga clic en _Terminar_.
    
    ![Imagen](images/L2022.png)
    
9.  Activar la aplicación
    

    ![Image](images/L2023.png)
    

## Tarea 3: Asignar aplicaciones a grupo

*   _Personas_:
    *   Administrador

En IDCS tiene la opción de asignar acceso a los usuarios directamente, mediante asignación directa a una aplicación específica o indirectamente mediante grupos dedicados. Para el caso de uso, utilizaremos grupos para proporcionar a los usuarios acceso a la aplicación Salesforce.

1.  Haga clic en el cajón de navegación y seleccione _Grupos_.
    
2.  Agregue un grupo con la etiqueta _Empleado_. Marque la casilla _El usuario puede solicitar acceso_.
    
    ![Imagen](images/L2024.png)
    
3.  Haga clic en _Terminar_.
    
4.  Vaya al separador _Acceso_. Haga clic en _Asignar_.
    
5.  Seleccione el botón _Asignar_ en línea con la aplicación _Salesforce_ y haga clic en _Aceptar_.
    
    ![Imagen](images/L2025.png)
    
        Note: you need to add a corresponding account in Salesforce for Federation to work correctly.  How accounts may be created in production are out of scope for this workshop but can be accomplished in a number of methods.  
        
6.  Inicie sesión en su [cuenta de desarrollador de Salesforce](https://developer.salesforce.com/) para crear una cuenta correspondiente para una que tengamos en IDCS.
    
    ![Imagen](images/L2026.png)
    
7.  En caso de que no sea Salesforce Classic, haga clic en el menú de perfil según la siguiente pantalla de impresión y seleccione la opción: _Cambiar a Salesforce Classic_.
    
    ![Imagen](images/L2027.png)
    
8.  Vaya a _Configuración_ en el menú del separador superior, cerca del perfil.
    
    ![Imagen](images/L2028.png)
    
9.  En el panel izquierdo, haga clic en _Gestionar usuarios_, a continuación, en _Usuarios_ y, por último, en _Nuevo usuario_.
    
    ![Imagen](images/L2029.png)
    
10.  Cree un usuario como se muestra a continuación. Asegúrese de utilizar la dirección de correo electrónico como conexión y utilice una que coincida con una cuenta de IDCS. Defina los siguientes parámetros:
    
    | Parámetro | Valor |
    | --- | --- |
    | Rol | p. ej., equipo de marketing |
    | Licencia de usuario | Plataforma Salesforce |
    | Perfil | Usuario de plataforma estándar |
    
    ![Imagen](images/L2030.png)
    

Ahora que la cuenta está disponible en Salesforce e IDCS tiene la cuenta autenticada que está lista para probar.

## Tarea 4: Solicitar acceso a grupo

*   _Personas_:
    *   Usuario final

Recuerde que hemos creado el grupo _Empleado_ en IDCS que tiene acceso a la aplicación Salesforce, pero que aún no tiene ningún usuario asignado. Puesto que hemos elegido la opción _El usuario puede solicitar acceso_ al crear el grupo, ahora deberíamos poder solicitar acceso a él e implícitamente a la aplicación Salesforce.

1.  Cierre el explorador para borrar la caché y, a continuación, conéctese a la consola de IDCS.
    
        https://<yourtenant>/ui/v1/myconsole
        
    
        Please use the same user credentials that you have entered in the previous section in order to create a user account in Salesforce. If you have followed the conventions recommended by this guide this should be in the form of:     <username>+ui@<email_provider.com>
        
2.  En la página _Mis aplicaciones_, haga clic en el botón de solicitud de acceso _\+ Agregar_.
    
    ![Imagen](images/L2031.png)
    
3.  En el separador _Grupos_, seleccione el grupo _Empleado_ y seleccione el signo _+_.
    
    ![Imagen](images/L2032.png)
    
4.  Proporcione la _justificación_ en la página emergente resultante. Haga clic en _Aceptar_. Se trata de una solicitud aprobada automáticamente y el acceso se debe otorgar inmediatamente sin necesidad de intervención del administrador.
    
    ![Imagen](images/L2033.png)
    
5.  Vaya a la sección _Mi acceso_ desde el menú ubicado en la parte superior derecha.
    
    ![Imagen](images/L2034.png)
    
6.  Asegúrese de que las aplicaciones de Salesforce estén visibles ahora en la página _Mis aplicaciones_.
    
    ![Imagen](images/L2035.png)
    

## Tarea 5: Verificación de la Configuración de SSO

*   _Personas_:
    *   Usuario final

1.  En la consola de IDCS, haga clic en la aplicación _Chatter de Salesforce_ en la página _Mis aplicaciones_.
    
    Nota: En caso de que las aplicaciones de Salesforce no estén visibles mientras el usuario está asignado a un grupo que da acceso a Salesforce, primero se debe configurar una sincronización con el usuario en IDCS y Salesforce. En el paso 6 se explica cómo sincronizar usuarios entre IDCS y Salesforce.
    
    ![Imagen](images/L2036.png)
    
2.  Asegúrese de que el usuario está conectado automáticamente a Salesforce Chatter (SSO).
    
    ![Imagen](images/L2037.png)
    

Ahora debería ver la misma información de perfil de usuario que ha iniciado en IDCS, en Salesforce sin tener que conectarse a Salesforce.

    When you submit your IDCS user login credentials, in the background, IDCS prepares a SAML assertion and redirects the user’s browser to the Salesforce SaaS application.
    
    Salesforce consumes the SAML assertion and maps the user in its local identity store, based upon the federation agreements which had previously been configured.
    

## Tarea 6: Configuración del aprovisionamiento y la sincronización

*   _Personas_:
    *   Administrador

### Obtención de nombre de host, ID de organización y nombre de dominio de Salesforce

Se necesita un nombre de host, un ID de organización y un nombre de dominio para poder configurar la aplicación Salesforce en Oracle Identity Cloud Service. Estos valores se obtienen de Salesforce.

1.  En el menú de navegación de la izquierda de la página de inicio, busque y haga clic en _Configuración de conexión única_. La página Configuración de conexión única aparece como el único enlace válido.
    
    ![Imagen](images/L2038.png)
    
2.  En la sección _Configuración de conexión única_, haga clic en el nombre proporcionado para el proveedor de identidad en la página _Configuración de conexión única_.
    
3.  Tome nota del nombre de host del valor proporcionado en el campo Entity ID y haga clic en el nombre de instancia correspondiente.
    
    ![Imagen](images/L2039.png)
    
        Note: Use this host name value while enabling user provisioning for the Salesforce app in Oracle Identity Cloud Service in the "Enabling Provisioning" section, and while verifying the SSO initiated from Salesforce in the "Verifying Service Provider Initiated SSO from Salesforce" section.
        
4.  En la sección _Puntos finales_, anote el nombre de dominio y el ID de organización de la URL de conexión de Salesforce:
    
        https://<Domain_Name>.my.salesforce.com?so=<Organization_ID>
        or
        https://<Domain_Name>.my.salesforce.com
        
    
    El nombre de dominio aparece al principio y el ID de organización al final de la URL.
    
    ![Imagen](images/L2040.png)
    

### Obtención de la clave de consumidor y el secreto de consumidor de Salesforce

1.  Cambie a _Lightning Experience_ en Salesforce.
    
    ![Imagen](images/L2041.png)
    
2.  Haga clic en la rueda dentada en la esquina superior derecha de la página y seleccione _Configurar_.
    
    ![Imagen](images/L2042.png)
    
3.  En el menú de navegación de la izquierda de la página de inicio, busque y haga clic en App Manager. Aparece la página _Administrador de aplicaciones_ de Lightning Experience.
    
    ![Imagen](images/L2043.png)
    
4.  En la esquina derecha de la página, haga clic en _Nueva aplicación conectada_. Aparece la página Nueva aplicación conectada.
    
    ![Imagen](images/L2044.png)
    
5.  En la sección Información básica, introduzca el _nombre de la aplicación conectada_ de la aplicación que desea conectar.
    
6.  Introduzca el _correo electrónico de contacto_ del administrador.
    
7.  En la sección API (Activar configuración de OAuth), seleccione la casilla de control _Activar configuración de OAuth_.
    
8.  Introduzca cualquier URL de dominio público que reciba el token de acceso del servidor de autorización en el campo _URL de devolución de llamada_. Por ejemplo, https://login.salesforce.com.
    
9.  En el campo _Ámbitos OAuth seleccionados_, seleccione _Acceso completo (completo)_ en la lista _Ámbitos OAuth disponibles_ y, a continuación, haga clic en _Agregar_ para otorgar acceso completo a la modificación de OAuth.
    
    ![Imagen](images/L2045.png)
    
10.  Haga clic en _Guardar_.
    
11.  Espere de 2 a 10 minutos para que los cambios surtan efecto en el servidor antes de utilizar la aplicación conectada y, a continuación, haga clic en _Continuar_.
    
12.  En la mayoría de los casos, la página de la aplicación recién creada aparece automáticamente. En caso de que no aparezca, siga los siguientes pasos. En el menú de navegación de la izquierda de la página de inicio, busque y haga clic en _Gestor de aplicaciones_. Busque la aplicación recién creada, haga clic en la flecha pequeña situada a la derecha y seleccione _Ver_.
    
    ![Imagen](images/L2046.png)
    
13.  En la sección API (Activar configuración de OAuth), haga clic en _Haga clic para mostrar_ junto al campo _Secreto de consumidor_.
    
    ![Imagen](images/L2047.png)
    
14.  Anote los valores de _Clave de consumidor_ y _Secreto de consumidor_.
    

### Derivación de la contraseña de administrador de Salesforce

**Se debe agregar un token de seguridad a la contraseña del administrador** antes de activar el aprovisionamiento y la sincronización para la aplicación Salesforce. Obtiene el valor de token de Salesforce.

El valor final se verá así:

    <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdjFKYSj028WJhU1GG)>
    

1.  En la esquina superior derecha de la página de inicio de Salesforce, haga clic en el icono de usuario y, a continuación, haga clic en Configuración en la lista desplegable.
    
    ![Imagen](images/L2048.png)  
     
    
2.  En el menú de navegación de la izquierda, busque y haga clic en _Reset My Security Token_.
    
    ![Imagen](images/L2049.png)
    
3.  En la página _Restablecer mi token de seguridad_, haga clic en _Restablecer mi token de seguridad_. Se envía un token de seguridad a la dirección de correo electrónico del administrador.
    
4.  Tome nota del token de seguridad y agregue el token de seguridad a la contraseña del administrador.
    
        <yourSalesforceAdminPassword + securityToken(ex: LKdMzECdWJhU1GG)>
        
    
        NOTE: the <>, + and example must not be included
        

### Recuperación del ID de perfil de Salesforce

Los usuarios creados en Oracle Identity Cloud Service deben estar asignados a un perfil de Salesforce. El valor de ID de perfil de este perfil de Salesforce se debe capturar de Salesforce antes de activar el aprovisionamiento y la sincronización de usuarios para la aplicación Salesforce.

1.  Haga clic en _Cambiar a Salesforce Classic_
    
    ![Imagen](images/L2050.png)
    
2.  Haga clic en _Configurar_.
    
    ![Imagen](images/L2051.png)
    
3.  Vaya a _Administrar_ y seleccione _Gestionar usuarios y perfiles_.
    
    ![Imagen](images/L2052.png)
    
4.  En la segunda página, haga clic en _Usuario de plataforma estándar_.
    
    ![Imagen](images/L2053.png)
    

  5. Observe la URL del perfil. El _ID de perfil_ es el ID al final de la URL.

    ![Image](images/L2054.png)
    

6.  Tome nota del valor, ya que lo usaremos más adelante.

### Activación del aprovisionamiento y la sincronización para Salesforce

El aprovisionamiento le proporcionará las siguientes operaciones disponibles:

*   _Crear cuenta_: crea automáticamente una cuenta de Salesforce cuando se otorga acceso a Salesforce al usuario correspondiente en Oracle Identity Cloud Service.
*   _Desactivar cuenta_: desactiva o activa automáticamente una cuenta de Salesforce cuando se desactiva o activa el acceso a Salesforce para el usuario correspondiente en Oracle Identity Cloud Service.
*   _Suprimir cuenta_: elimina automáticamente una cuenta de Salesforce cuando se revoca el acceso a Salesforce del usuario correspondiente en Oracle Identity Cloud Service.

Además, puede asignar atributos entre los campos de cuenta de usuario definidos en Salesforce y los campos correspondientes definidos en Oracle Identity Cloud Service.

1.  Vaya a la consola de administración de IDCS y seleccione las aplicaciones de Salesforce que ha creado anteriormente.
    
2.  En la página _Provisioning_, seleccione _Enable Provisioning_.
    
3.  Aparecerá una ventana. Haga clic en _Otorgar consentimiento_
    
4.  Rellene los siguientes parámetros con los valores que ha derivado en los pasos anteriores de Salesforce:
    
    ![Imagen](images/L2055.png)
    
    | Parámetro | Valor |
    | --- | --- |
    | Nombre de host | Introduzca el nombre de host de Salesforce que ha obtenido realizando los pasos de la sección Obtención de nombre de host, ID de organización y nombre de dominio de Salesforce". |
    | Usuario Administrador | Introduzca el nombre de usuario de cuenta de administrador. |
    | Contraseña de administrador | Introduzca la contraseña actualizada que ha obtenido realizando los pasos de la sección "Deriving the Administrator Password from Salesforce". |
    | ID de cliente | Introduzca la clave de consumidor de Salesforce que ha obtenido realizando los pasos de la sección "Obtención de la clave de consumidor y el secreto de consumidor de Salesforce". |
    | Secreto de cliente | Introduzca el secreto de consumidor de Salesforce que ha obtenido realizando los pasos de la sección "Obtención de la clave de consumidor y el secreto de consumidor de Salesforce". |
    
5.  Haga clic en _Probar conectividad_ para verificar la conexión con Salesforce. Oracle Identity Cloud Service muestra un mensaje de confirmación.
    
6.  Haga clic en _Asignación de atributos_.
    
    ![Imagen](images/L2056.png)
    
7.  Defina el _ID de perfil_ en el valor recuperado anteriormente.
    
    ![Imagen](images/L2057.png)
    

  8. Desplácese hacia abajo en la página _Aprovisionamiento_ y seleccione _Activar sincronización_.

    ![Image](images/L2058.png)
    

Esta opción sincronizará los detalles de cuenta existentes de Salesforce y los enlazará a los usuarios correspondientes de Oracle Identity Cloud Service.  

### Prueba de sincronización en IDCS

1.  En la página de aplicaciones, seleccione _Importar_.
    
2.  Haga clic en _Importar_ y espere un momento
    
    ![Imagen](images/L2059.png)
    
3.  Haga clic en _Actualizar_.
    
    ![Imagen](images/L2060.png)
    
4.  Se mostrarán los usuarios importados que se importaron desde IDCS.
    
    ![Imagen](images/L2061.png)
    

  5. El paso final es confirmar los usuarios que desea enlazar entre IDCS y Salesforce. En Estado de sincronización, haga clic en _Confirmar_. Un enlace correcto entre un usuario de IDCS y Salesforce conducirá al estado de sincronización _Confirmado_.

    ![Image](images/L2062.png)
    

    Every user that has status Confirmed in the Synchronization Status has access to Salesforce in My Apps. Also, the application Salesforce will be visible to all users in My Apps that have status Confirmed.
    

Ahora puede continuar con la siguiente práctica de laboratorio

 

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020