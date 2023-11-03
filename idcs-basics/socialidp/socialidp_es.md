# Carga de datos en una instancia

## Introducción

Un proveedor de identidad, conocido como proveedor de afirmación de identidad, proporciona identificadores para los usuarios que desean interactuar con Oracle Identity Cloud Service mediante un sitio web externo a Oracle Identity Cloud Service. Un proveedor de servicios es un sitio web como Oracle Identity Cloud Service que aloja aplicaciones. Puede activar un proveedor de identidad y definir uno o más proveedores de servicios. A continuación, los usuarios pueden acceder a las aplicaciones alojadas por los proveedores de servicios directamente desde el proveedor de identidad.

Oracle Identity Cloud Service (IDCS) soporta proveedores de identidad social para que los usuarios puedan conectarse a IDCS con sus credenciales sociales. La función de conexión social permite a los usuarios registrarse automáticamente en IDCS si aún no tienen una cuenta.

Los siguientes proveedores sociales vienen listos para usar con IDCS:

*   Facebook
*   Google
*   LinkedIn
*   Microsoft
*   Twitter

IDCS también soporta cualquier proveedor de identidad social genérico compatible con OpenID Connect.

Los siguientes enlaces son a un vídeo que muestra la configuración con Google e IDCS. Las pantallas tienen fecha, pero los parámetros y conceptos son útiles.

[](youtube:-OwFAGJw3vo)

Otro video - con más detalle, pero con las pantallas de estilo más antiguas:

[](youtube:JU8ArDvzWq0)

Tiempo de laboratorio estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar Federación en Google
*   Configurar Federación en IDCS
*   Verificación de conexión social

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Una cuenta de Google

## Tarea 1: Configurar Google

*   _Personas_:
    *   Administrador

En este [enlace](https://developers.google.com/identity/sign-in/web/sign-in) encontrará un tutorial rápido para integrar el inicio de sesión de Google en su aplicación web.

Esta guía también proporciona un tutorial detallado sobre cómo integrar Google como IdP para IDCS.

Para utilizar Google como IdP para IDCS, necesitamos obtener las credenciales de Auth 2.0 de Google. Esto se puede hacer teniendo una cuenta de desarrollador en google y crear una aplicación para este propósito.

1.  Para empezar, haga clic en la [página de protocolo OpenIDConnect](https://developers.google.com/identity/protocols/OpenIDConnect) de la página _Google Identity Platform_.
    
2.  Desplácese hacia abajo hasta el subtítulo _Obtener credenciales OAuth 2.0_ y haga clic en la página _Credenciales_ con hiperenlace. Se abrirá una nueva ventana que le pedirá que inicie sesión con su cuenta de Google. _Inicia sesión_ con la cuenta de Google que creaste durante la sección de requisitos previos de este taller. ![Imagen](images/L3001.png)
    
3.  Haga clic en _Seleccionar un proyecto_ y seleccione _NUEVO PROYECTO_. ![Imagen](images/L3002.png)
    
4.  Seleccione un nombre para el proyecto y haga clic en _CREAR_. ![Imagen](images/L3003.png)
    
5.  En _Credenciales_, haga clic en la _pantalla de consentimiento OAuth_.
    
6.  Seleccione un _nombre de aplicación_. ![Imagen](images/L3004.png)
    
7.  Para _dominios autorizados_, seleccione _oraclecloud.com_ y haga clic en _Guardar_. ![Imagen](images/L3005.png)
    
8.  Haga clic en _Crear credenciales_ y seleccione _OAuth ID de cliente_. ![Imagen](images/L3006.png)
    
9.  Seleccione _Aplicación web_ y un _nombre_.
    
10.  Defina la _URL redireccionada autorizada_ en el siguiente valor:
    
        https://<your tenant>/oauth2/v1/social/callback
        
11.  Haga clic en _Crear_ para crear las claves. ![Imagen](images/L3007.png)
    
12.  Recibirá el _ID de cliente_ y el _secreto de cliente_. Guárdalos para más tarde. ![Imagen](images/L3008.png)
    

## Tarea 2: Configuración de IDCS

*   _Personas_:
    *   Administrador

Ahora vuelva a _IDCS_ e introduzca los valores de parámetros del proveedor para configurar Google como Social IdP.

1.  Vaya a la consola de administración de IDCS. En el menú de la barra lateral izquierda, vaya al separador _Seguridad_ y, a continuación, a _Proveedores de identidad_.
    
2.  Haga clic en _Agregar IDP social_
    
3.  Seleccione el tipo como _Google_. Proporcione un nombre del proveedor (por ejemplo, "Google-<\>" y haga clic en _Siguiente_. ![Imagen](images/L3009.png) 
    
4.  Rellene el _ID de cliente_ y el _Secreto de cliente_ que hemos obtenido anteriormente. ![Imagen](images/L3010.png)
    
5.  Haga clic en _Finalizar_. A continuación, haga clic en el botón de menú del lado derecho y seleccione _Activar_ para activar Google como IdP. ![Imagen](images/L3011.png)
    
    Ahora, Google se define como un IdP social para IDCS, pero aún no está visible en la página de inicio de sesión. Los siguientes pasos describen cómo asignarla a una política de IDP para que Google sea visible en la página de inicio de sesión de IDCS.
    
6.  Haga clic de nuevo en el botón de menú y seleccione la opción _Mostrar en Página de Conexión_ para mostrar Google como IdP en la página de conexión de IDCS. ![Imagen](images/L3012.png)
    
    Una vez configurado para _Mostrar en la página de conexión_, verá un "ojo".
    
    ![Imagen](images/L3013.png)
    
7.  En el menú de la barra lateral, haga clic en _Seguridad_ y, a continuación, en _Políticas de IDP_. Haga clic en _Política de identidad por defecto_. ![Imagen](images/L3014.png)
    
8.  Seleccione el separador _Proveedores de identidad_ y haga clic en _Asignar_ para asignar Google como IdP. ![Imagen](images/L3015.png)
    
9.  Seleccione _Google_ y haga clic en _Aceptar_. ![Imagen](images/L3016.png)
    
10.  Google aparece ahora en la lista de proveedores de identidad. Google se ha configurado y asignado como IdP para IDCS. El siguiente paso será vincular tu cuenta de Google con tu cuenta de IDCS. Para ello, haga clic en _Mi perfil_. ![Imagen](images/L3017.png)
    
11.  En _Mi perfil_, seleccione el separador _Cuentas sociales_. Haga clic en _Enlazar una cuenta social_ para enlazar su cuenta de Gmail a su cuenta de IDCS. ![Imagen](images/L3018.png)
    
12.  Haga clic en el _menú de hamburguesa_ y haga clic en _Enlace_. ![Imagen](images/L3019.png)
    
13.  Su cuenta de Gmail ahora está enlazada a su cuenta de IDCS. Haga clic en _Cerrar sesión_ para desconectarse de IDCS. ![Imagen](images/L3020.png)
    

## Tarea 3: Verificar conexión social

*   _Personas_:
    *   Usuario final

Ahora vuelva a _IDCS_ e introduzca los valores de parámetros del proveedor para configurar Google como Social IdP.

1.  Vaya a la consola de administración de IDCS. Haga clic en _Google_ como método de inicio de sesión. ![Imagen](images/L3021.png)
    
2.  Aparece la página de inicio de sesión de Google. Rellene la dirección y la contraseña de Gmail y haga clic en _Siguiente_. ![Imagen](images/L3022.png)
    
3.  Google redirige a la página _Mis aplicaciones_ de IDCS. La integración de Google como IdP para IDCS se ha probado correctamente. ![Imagen](images/L3023.png)
    

Ahora puede continuar con la siguiente práctica de laboratorio

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020