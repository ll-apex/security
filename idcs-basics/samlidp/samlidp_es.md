# Configurar Federación: Proveedor de Identidad Externo (Okta)

## Introducción

Un _proveedor de identidad_, conocido como _proveedor de afirmación de identidad_, proporciona identificadores para los usuarios que desean interactuar con Oracle Identity Cloud Service mediante un sitio web externo a Oracle Identity Cloud Service. Un _proveedor de servicios_ es un sitio web como Oracle Identity Cloud Service que aloja aplicaciones. Puede activar un proveedor de identidad y definir uno o más proveedores de servicios. A continuación, los usuarios pueden acceder a las aplicaciones alojadas por los proveedores de servicios directamente desde el proveedor de identidad.

Para este ejercicio, un sitio web puede permitir a los usuarios conectarse a Oracle Identity Cloud Service con credenciales de Okta. Okta actúa como proveedor de identidad y Oracle Identity Cloud Service funciona como proveedor de servicios. Okta verifica que el usuario es un usuario autorizado y devuelve información a Oracle Identity Cloud Service.

Tiempo de laboratorio estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar Federación en Okta
*   Configurar Federación en IDCS
*   conéctese a IdP

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Una cuenta de Google
*   Una cuenta de integrador de Okta

## Tarea 1: Configuración de Okta

*   _Personas_:
    *   Administrador

1.  1.  Vaya a Okta y regístrese para obtener una [cuenta de desarrollador](https://www.okta.com/integrate/signup/). Esta parte se ha tratado en la sección de requisitos previos.
2.  Una vez que tenga la credencial de cuenta de prueba, conéctese mediante la URL que ha recibido después de solicitar la prueba (la personalizada, por ejemplo: https://oracleworkshop.oktareview.com) y vaya a _Aplicaciones_. En el menú superior izquierdo, seleccione _Interfaz de usuario clásica_. ![Imagen](images/L4001.png)
    
3.  En el separador superior, seleccione _Applications_ y, a continuación, _Applications_ ![Imagen](images/L4002.png)
    
4.  Seleccione el botón _Agregar aplicación_. ![Imagen](images/L4003.png)
    
5.  Seleccione el botón _Create New App_. ![Imagen](images/L4004.png)
    
6.  Seleccione _SAML2.0_ y haga clic en _Crear_. ![Imagen](images/L4005.png)
    
7.  Proporcione un nombre de aplicación, por ejemplo: **IDCS\_SP** y haga clic en _Siguiente_. ![Imagen](images/L4006.png)
    
8.  Para la sección _Configurar SAML_, debe introducir valores de los metadatos de IDCS
    
        https://<your tenant>.identity.oraclecloud.com/fed/v1/metadata
        
9.  Introduzca los siguientes valores de los metadatos en la configuración de SAML de Okta:
    
    *   En los metadatos de IDCS, busque _AssertionConsumerService_ en la parte inferior. Copie la URL resaltada en _Ubicación_ como se muestra a continuación. Introduzca la URL como _URL de inicio de sesión único_ en Okta ![Imagen](images/L4007.png)
    *   En los metadatos de IDCS, busque _entityID_ en la parte superior. Copie la URL resaltada como se muestra a continuación. Introduzca la URL como _URI de público (ID de entidad de SP)_ en Okta ![Imagen](images/L4008.png)
    *   Cuando haya terminado, la configuración general de Okta debería parecerse al ejemplo siguiente. Deje el resto de los campos con sus valores por defecto ![Imagen](images/L4009.png)
10.  Desplácese hacia abajo hasta el final de la página y haga clic en _Siguiente_. ![Imagen](images/L4010.png)
    
11.  Proporcione los comentarios obligatorios y seleccione _Finalizar_. ![Imagen](images/L4011.png)
    
12.  Haga clic en _Metadatos de proveedor de identidad_ en _Configuración_ para exportar el archivo de metadatos de IDP. ![Imagen](images/L4012.png)
    
13.  Los metadatos se mostrarán en otra ventana del explorador. Guárdelo en un archivo xml haciendo clic con el botón derecho y elija "Guardar como", ya que lo necesitará en unos minutos dentro de IDCS. ![Imagen](images/L4013.png)
    
14.  Vuelva a _Okta_ y seleccione el separador _Asignaciones_ de la aplicación recién creada para asignar usuarios a la aplicación. ![Imagen](images/L4014.png)
    
15.  Haga clic en _Asignar_ y seleccione _Asignar a personas_ ![Imagen](images/L4015.png)
    
16.  Seleccione un usuario en la lista y presione _Asignar_. Introduzca el nombre de usuario del usuario de IDCS correspondiente y seleccione _Guardar y volver_. Cuando el usuario esté asignado, haga clic en _Listo_.
    
    ![Imagen](images/L4016.png)
    
    Nota: Este no es el nombre de usuario que proporciona al conectarse a Okta. Eso no cambia. Lo que está introduciendo aquí es el nombre de usuario del usuario correspondiente en IDCS. Esto permite un enlace entre cuentas con diferentes nombres de usuario en IDCS y Okta. Las cuentas pueden tener nombres de usuario diferentes, por ejemplo, porque Okta los necesita en formato de correo electrónico, mientras que IDCS no.
    
17.  IDCS ahora está configurado como proveedor de servicios en Okta. Abra IDCS para configurar Okta como proveedor de identidad.
    

## Tarea 2: Configuración de IDCS

*   _Personas_:
    *   Administrador

1.  Conéctese a la consola de administración de IDCS, vaya a _Seguridad_ y, a continuación, haga clic en _Proveedores de identidad_. Haga clic en _Agregar IDP de SAML_. Introduzca el nombre del proveedor (por ejemplo, "Okta-<\>") y la descripción y haga clic en Next. ![Imagen](images/L4017.png) 
    
2.  Cargue el archivo xml de metadatos de Okta exportado anteriormente y haga clic en _Siguiente_. ![Imagen](images/L4018.png)
    
3.  Defina los siguientes parámetros:
    
    *   Atributo de usuario de proveedor de identidad = **ID de nombre**
    *   Atributo de usuario de Oracle Identity Cloud Service = **Nombre de usuario**
    *   Formato NameID solicitado = **No especificado** ![Imagen](images/L4019.png)
4.  En la pantalla de exportación no hay nada que hacer, ya que ya habíamos recuperado los metadatos de IDCS de la URL y no es necesario volver a descargarlos. Haga clic en _Siguiente_. ![Imagen](images/L4020.png)
    
5.  Seleccione _Probar Conexión_ para comprobar la conexión entre IDCS y Okta. ![Imagen](images/L4021.png)
    
6.  Introduzca sus credenciales de Okta. ![Imagen](images/L4022.png)
    
7.  Si todo funciona, verá el mensaje que se muestra a continuación. De lo contrario, compruebe las configuraciones de la aplicación IDCS en Okta y/o el IDP de SAML de Okta en IDCS. ![Imagen](images/L4023.png)
    
8.  En el flujo de configuración del IDP de SAML, seleccione _Siguiente_. ![Imagen](images/L4024.png)
    
9.  Seleccione _Activar_ y, a continuación, _Finalizar_. Ahora ha agregado Okta como proveedor de identidad externo y ha establecido correctamente una conexión con IDCS. ![Imagen](images/L4025.png)
    
10.  Después de finalizar correctamente la configuración de la aplicación Okta, volverá a la pantalla _Proveedores de identidad_. Seleccione el menú _Mostrar en Página de Conexión_. ![Imagen](images/L4026.png)
    
11.  Vaya a _Seguridad_, seleccione _Políticas de IDP_ y haga clic en _Política de Proporción de Identidad por Defecto_. ![Imagen](images/L4027.png)
    
12.  Seleccione la opción de menú _\+ Asignar_ y agregue el proveedor de Okta. ![Imagen](images/L4028.png)
    
13.  Seleccione _Okta_ y haga clic en _Aceptar_. ![Imagen](images/L4029.png)
    
14.  _Okta_ se mostrará en la lista ![Imagen](images/L4030.png)
    
15.  _Desconéctese_ de IDCS. ![Imagen](images/L4031.png)
    

## Tarea 3: Verificación del inicio de sesión de IDP externo

*   _Personas_:
    *   Administrador

1.  Acceda a la consola de administración de IDCS y Okta se mostrará como una opción de conexión. Seleccione _Okta_. ![Imagen](images/L4032.png)
    
2.  Escriba las credenciales de conexión de Okta. ![Imagen](images/L4033.png)
    
3.  Una vez autenticada correctamente, la solicitud se enviará a la pantalla Mis aplicaciones de IDCS. ![Imagen](images/L4034.png)
    
    Nota: para que la federación funcione, debe conectarse con una cuenta definida en IDCS y Okta.
    

Ahora puede continuar con la siguiente práctica de laboratorio

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020