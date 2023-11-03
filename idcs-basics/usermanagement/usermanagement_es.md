# Aprovisionamiento de una instancia

## Introducción

A menudo es un requisito para las organizaciones incorporar a un empleado o contratista o alguna identidad manualmente en un sistema de identidad central como IDCS. En este caso de uso, un administrador de usuario de IDCS agregará manualmente un nuevo usuario en el servicio de IDCS. Normalmente, esto se produciría mediante el aprovisionamiento automatizado, la importación masiva de archivos planos o la sincronización con su Active Directory local.

Las funciones de IDCS se crean al 100% mediante el servicio de API de REST de IDCS. Por lo tanto, cualquier tarea que realicemos de forma interactiva en la interfaz web también podría realizarse a través de una aplicación personalizada, utilizando estas mismas llamadas de API de REST. Es una clara ventaja de la arquitectura de API como elemento principal de Oracle con IDCS.

IDCS soporta la vinculación de usuarios (también grupos) desde Active Directory local, mediante la carga de archivos, la API de REST, la solución local de Oracle Identity Management o manualmente desde la consola de administración de IDCS. Para el taller utilizaremos la opción de carga de archivos y la gestión de usuarios de llamadas de API.

Tiempo de laboratorio estimado: 90 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear usuario en IU
*   importar usuarios con CSV
*   Crear usuarios mediante API de REST

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Aplicación nativa de Postman instalada localmente

## Tarea 1: Crear usuario en la interfaz de usuario

*   _Personas_:
    *   Administrador

1.  Vaya a la consola de administración de IDCS con las credenciales de su cuenta de administrador. Seleccione el menú _Usuarios_ de la izquierda y haga clic en _+Add_ o seleccione el icono _Agregar un usuario_ del panel de control.
    
    ![Imagen](images/L1001.png)
    
2.  Rellene todos los campos obligatorios y haga clic en Finalizar. Como práctica recomendada, utilice una convención de nomenclatura consistente en todas las prácticas. Por ejemplo:
    
    *   Nombre: Nombre
    *   Apellido: Apellido-UI
    *   Nombre de usuario/correo electrónico: firstname.lastname+ui@domain.tld
    
    ![Imagen](images/L1002.png)
    
3.  Verifique la creación del usuario. Para ello, vaya al separador _Usuarios_ de la consola de administración. Verifique que los nuevos usuarios estén visibles en la consola.
    
    ![Imagen](images/L1003.png)
    
        Once a new user is created in the IDCS service, the user receives an email invitation to finish first time login formalities such as setting a password to logging into IDCS.
        

## Tarea 2: Importación de usuarios con CSV

*   _Personas_:
    *   Administrador

Si es administrador de dominio de identidad o administrador de usuario, puede importar por lotes cuentas de usuario mediante un archivo de valores separados por comas (CSV).

1.  Obtenga el archivo CSV de carga. Para ello, seleccione el menú _Usuarios_ de la izquierda y haga clic en _Importar_. En la sección Ayuda de la derecha, haga clic en _Descargar archivo de ejemplo_.
    
    ![Imagen](images/L1004.png)
    
    **Alternativamente**, puede exportar una lista de usuarios existentes: filtre el usuario creado en la interfaz de usuario y, a continuación, haga clic en Exportar-> Exportar todo. Confirme la exportación y, a continuación, vaya al trabajo y descargue el archivo
    
    ![Imagen](images/L1005.png)
    
    Extraiga el archivo zip y abra _Users.csv_ o abra el archivo _UserExport... .csv_. Inspeccione el contenido del archivo desde su editor favorito. Puede utilizar los ejemplos predeterminados que encuentre en el archivo csv o puede completar algunos más si lo desea, pero manténgalo similar a los ejemplos. Por ejemplo, si utiliza el usuario existente exportado, modifique el sufijo (IU) a otro (por ejemplo, CSV).
    
2.  Vaya a la consola de administración de IDCS con las credenciales de su cuenta de administrador. Seleccione el menú _Usuarios_ de la izquierda o seleccione el icono _Usuarios_ del panel de control.
    
    ![Imagen](images/L1006.png)
    
3.  Haga clic en el botón _Importar_.
    
    ![Imagen](images/L1007.png)
    
4.  Aparece la pantalla Importar usuarios. Haga clic en _Examinar_. Seleccione el archivo CSV. Haga clic en _Importar_.
    
    ![Imagen](images/L1008.png)
    
        By uploading the sample file, sample users will be uploaded into IDCS. In order to upload different users, the csv sample file needs to be altered.
        
5.  Vaya a la opción de menú _Trabajos_ y verifique que el trabajo de importación ha terminado correctamente. Haga clic en _Ver detalles_ para comprobar si se han importado todos los usuarios. También se le presentará la lista de todos los usuarios de csv, junto con todos los detalles de atributos y el estado de creación.
    
    ![Imagen](images/L1009.png)
    
6.  Para verificar la creación de usuarios, vaya al separador _Usuarios_ de la consola de administración. Verifique que los nuevos usuarios estén visibles en la consola.
    
    ![Imagen](images/L1010.png)
    
7.  Haga clic en el usuario final de destino (por ejemplo, Firstname Lastname-CSV) y verifique la información detallada del atributo del usuario
    
8.  Por diversión, cambie el correo electrónico del usuario a uno que controle y seleccione _Actualizar usuario_. A continuación, haga clic en el botón _Reset Password_ (Restablecer contraseña).
    
    ![Imagen](images/L1011.png)
    
9.  Compruebe el mensaje de restablecimiento en su correo electrónico y cambie la contraseña.
    
    ![Imagen](images/L1012.png)
    
10.  A continuación, abra otro explorador (si utiliza Chrome, seleccione Ventana de incógnito) e inicie sesión en la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![Imagen](images/L1013.png)
    
    Verá que el usuario aún no tiene acceso a las aplicaciones. En el laboratorio 2, le mostraremos cómo un usuario puede solicitar aplicaciones.
    
    ![Imagen](images/L1014.png)
    

## Tarea 3: Creación de usuarios de API con API de REST

*   _Personas_:
    *   Administrador

Este caso de uso implica realizar llamadas de API a IDCS mediante un cliente REST; en este caso, Postman.

La recopilación Postman de llamadas de API de REST relevantes se proporciona a cada participante.

    This use case will demonstrate using a common utility, Postman, for connecting to Identity Cloud Service via the out-of-the-box REST API.
    

### Registrar una aplicación POSTMAN cliente en IDCS

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Seleccione el separador _Aplicaciones_ del panel de control de IDCS presentado después de conectarse
    
    ![Imagen](images/L1015.png)
    
3.  Haga clic en el botón _Agregar_ para crear una nueva aplicación para uso de Postman. Para que Postman pueda llamar a las API de REST de IDCS, primero necesita tener CLIENT\_ID y CLIENT\_SECRET que autoricen a Postman a hacerlo. Para ello, cree un tipo de aplicación confidencial en IDCS con tipos de permiso de autorización específicos, como verá en los siguientes pasos.
    
    ![Imagen](images/L1016.png)
    
4.  Seleccione _Aplicación confidencial_ en el menú emergente de tipos de aplicación
    
    ![Imagen](images/L1017.png)
    
5.  Defina el _nombre_ en "Postman-<\>" y, a continuación, haga clic en _Siguiente_.
    
    ![Imagen](images/L1018.png)
    
6.  Haga clic en _Configurar esta aplicación como cliente ahora_ para proporcionar los tipos de permiso de autorización y el rol de API.
    
    ![Imagen](images/L1019.png)
    
7.  Seleccione todas las casillas de control _Tipos de permiso permitidos_ y defina _URL de redirección_ en https://localhost.. Vaya a la sección _Otorgue al cliente acceso a las API de administrador de Identity Cloud Service_ (parte inferior de la página) y agregue el siguiente rol de API _Administrador de dominio de identidad_. De esta forma, básicamente otorga acceso a esta aplicación de IDCS al juego completo de API de IDCS. A continuación, haga clic en _Siguiente_.
    
    ![Imagen](images/L1020.png)
    
8.  No realice ningún cambio en _Recursos_ y haga clic en _Siguiente_
    
    ![Imagen](images/L1021.png)
    
9.  No realice cambios en _Política de nivel web_ y haga clic en _Siguiente_.
    
    ![Imagen](images/L1022.png)
    
10.  Para finalizar la aplicación de IDCS que otorga roles de API a Postman mediante el estándar Auth2.0, haga clic en _Finalizar_.
    
    ![Imagen](images/L1023.png)
    
11.  Una vez creada la aplicación, anote el _ID de cliente_ y el _Secreto de cliente_ y, a continuación, haga clic en _Cerrar_. La aplicación de escritorio Postman las utilizará para llamar a las API de IDCS mediante el protocolo estándar Auth2.0.
    
    ![Imagen](images/L1024.png)
    
12.  Haga clic en el botón _Activar_.
    
    ![Imagen](images/L1025.png)
    
13.  Confirme la activación de la aplicación
    
    ![Imagen](images/L1026.png)
    
14.  La aplicación está activa y lista para su uso.
    
    ![Imagen](images/L1027.png)
    
15.  Desconectarse de IDCS
    

### Configurar Postman

1.  Abrir Postman. Ignore todos los mensajes de inicio, si los hay.
    
    ![Imagen](images/L1028.png)
    
2.  En primer lugar, debemos importar las variables de entorno de Postman de IDCS, las variables globales y la recopilación de API de IDCS. Haga clic en el botón _Importar_ en la esquina superior izquierda.
    
    ![Imagen](images/L1029.png)
    
3.  Seleccione _Importar desde enlace_ y proporcione la URL de [Entorno de ejemplo](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/example_environment.json) en el cuadro para importar las variables de entorno y haga clic en _Importar_.
    
    ![Imagen](images/L1030.png)
    
4.  Para importar la recopilación Postman de la API de REST de Oracle Identity Cloud Service, en la página principal de Postman, vuelva a hacer clic en _Importar_.
    
5.  En el cuadro de diálogo Importar, seleccione _Importar desde enlace_, proporcione la URL de [Recopilación de Postman de IDCS](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/REST_API_for_Oracle_Identity_Cloud_Service.postman_collection.json) en el cuadro y, a continuación, haga clic en _Importar_.
    
        The left pane of Postman has a sub-tab called Collections.
        With Collections selected, you will see the various REST statements which have been pre-configured for today’s workshop.
        
6.  Para importar el archivo de variables globales, haga clic en _Importar_.
    
7.  En el cuadro de diálogo Importar, seleccione _Importar desde enlace_, proporcione la URL de [variables globales de IDCS](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/oracle_identity_cloud_service_postman_globals.json) en el cuadro y, a continuación, haga clic en _Importar_.
    
8.  Seleccione el entorno _Entorno de ejemplo de Oracle Identity Cloud Service con variables_ y, a continuación, haga clic en el botón _Configuración_ (icono de rueda dentada) para _Gestionar entornos_.
    
    ![Imagen](images/L1032.png)
    
9.  Haga clic en el entorno recién creado, que será como _Entorno de ejemplo de Oracle Identity Cloud Service con variables_; será el único disponible en sus casos en una instalación de Postman inicial.
    
    ![Imagen](images/L1033.png)
    
10.  Defina los siguientes valores iniciales y actuales de los parámetros para poder obtener un token de acceso de IDCS:
    
    | Parámetro | Valor |
    | --- | --- |
    | HOST | Ejemplo de URL de inquilino de IDCS: https://idcs-8b000000000000000000000000000000.identity.oraclecloud.com |
    | CLIENT\_ID | Aplicación cliente de Postman IDCS CLIENT\_ID |
    | CLIENT\_SECRET | Aplicación cliente de Postman IDCS CLIENT\_SECRET |
    
    ![Imagen](images/L1034.png)
    
11.  Haga clic en _Actualizar_ una vez completado.
    

### Solicitar un token de acceso

    The steps performed above are being done to obtain the Access Token, which is shown in the following screen shot.
    
    There are several types of OAUTH requests supported by IDCS, including client credentials.  With a client credentials request, the client ID and client secret are used by the calling application to identify itself and to request an Access Token.  The Access Token is available for use by the application until the time that it expires.
    
    This Access Token establishes trust between your application and IDCS.
    

1.  En el separador _Recopilaciones_, amplíe _OAuth_ y, a continuación, _Tokens_.
    
2.  Seleccione _Obtener access\_token (credenciales de cliente)_ y, a continuación, haga clic en _Enviar_. El token de acceso se devuelve en la respuesta de Oracle Identity Cloud Service.
    
    ![Imagen](images/L1035.png)
    
3.  Valide si puede ver el estado _200 OK_. ![Imagen](images/L1036.png)
    
4.  Resalte el contenido del token de acceso entre comillas y, a continuación, haga clic con el botón derecho en (\*). En el menú contextual, seleccione _Definir: Entorno de ejemplo de Oracle Identity Cloud Service con variables_. En el menú secundario, seleccione _access\_token_. El contenido resaltado se asigna como valor de token de acceso.
    
    ![Imagen](images/L1037.png)
    

### Creación de un usuario de IDCS a través de una API

1.  En el separador _Recopilaciones_, amplíe _Usuarios_ y, a continuación, _Crear_.
    
2.  Seleccione _Crear un usuario_. La información de la solicitud aparece con algunos valores predeterminados, como se puede ver en la siguiente pantalla de impresión. Puede cambiarlos según sus preferencias o simplemente puede usar los valores predeterminados completados. Por ejemplo:
    

*   givenName: nombre
*   familyName: Apellido-REST
*   nombre de usuario/correo electrónico->valor (x2): firstname.lastname+REST@domain.tld

3.  Haga clic en _Cuerpo_ y, a continuación, en _Enviar_.
    
    ![Imagen](images/L1038.png)
    
4.  En la respuesta, confirme que aparece el estado _201 Creado_ y que el cuerpo de la respuesta muestra detalles sobre el usuario que se ha creado correctamente en Oracle Identity Cloud Service.
    
5.  En la interfaz de usuario de IDCS puede ver que se está creando el usuario.
    
    ![Imagen](images/L1039.png)
    

También puede _modificar_ y _suprimir_ usuarios, y mucho más, mediante la API de REST. Para obtener más información, consulte la documentación a continuación.

Ahora puede continuar con la siguiente práctica de laboratorio

## Más información

Tutorial 1: [Oracle Identity Cloud Service: Primera llamada a la API de REST](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_1stcall_obe/rest_1stcall.html)

Tutorial 2: [Oracle Identity Cloud Service: Gestión de usuarios mediante llamadas de API de REST](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_users_obe/rest_users.html)

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020