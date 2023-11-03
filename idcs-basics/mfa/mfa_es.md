# Activación de MFA condicional en IDCS

## Introducción

La autenticación multifactor (MFA) es un método de autenticación que requiere el uso de más de un factor para verificar la identidad de un usuario.

Con la MFA activada en Oracle Identity Cloud Service, cuando un usuario se conecta a una aplicación, se le solicita su nombre de usuario y contraseña, que es el primer factor, algo que sabe. El usuario debe proporcionar un segundo tipo de verificación. Esto se denomina verificación en 2 pasos. Los dos factores trabajan juntos para agregar una capa adicional de seguridad mediante el uso de información adicional o un segundo dispositivo para verificar la identidad del usuario y completar el proceso de inicio de sesión.

Los usuarios están cada vez más conectados y acceden a sus cuentas y aplicaciones desde cualquier lugar. Como administrador, cuando agrega la MFA además del nombre de usuario y la contraseña tradicionales, esto le ayuda a proteger el acceso a los datos y las aplicaciones. Esto también reduce la probabilidad de robo y fraude de identidad en línea, lo que protege sus aplicaciones empresariales incluso si una contraseña de cuenta se ve comprometida.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   seleccionar los factores de MFA que desea activar
*   Definir una política de conexión para Salesforce
*   inicio de sesión como usuario final con MFA
*   bloqueo de acceso mediante política
*   probar la política actualizada

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Una cuenta de Google
*   Una cuenta de desarrollador de Salesforce
*   Se completó el laboratorio 2

## Tarea 1: Activar factores de MFA

*   _Personas_:
    *   Administrador

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Vaya a _Seguridad_ y seleccione _MFA_.
    
3.  Seleccione los _factores de MFA_ que desea activar y, a continuación, haga clic en _Guardar_.
    
    ![Imagen](images/L5001.png)
    

## Tarea 2: Definición de una política de conexión para Salesforce

*   _Personas_:
    *   Administrador

Una política de conexión permite a los administradores de dominio de identidad, administradores de seguridad y administradores de aplicaciones definir los criterios que utiliza Oracle Identity Cloud Service para determinar si se permite a un usuario conectarse a Oracle Identity Cloud Service o evitar que un usuario acceda a Oracle Identity Cloud Service.

Oracle Identity Cloud Service proporciona una política de conexión por defecto que contiene una regla de conexión por defecto. Oracle Identity Cloud Service evalúa los criterios de la regla para cualquier usuario que intente conectarse a Oracle Identity Cloud Service. Por defecto, esta regla permite a todos los usuarios conectarse a Oracle Identity Cloud Service. Esto significa que la autenticación que utilice el usuario, ya sea la autenticación local, proporcionando un nombre de usuario y una contraseña, o la autenticación mediante un proveedor de identidad externo, será suficiente.

Sin embargo, puede basarse en esta política agregándole otras reglas de conexión. Al agregar estas reglas, puede evitar que algunos usuarios se conecten a Oracle Identity Cloud Service. O bien, puede permitirles conectarse, pero solicitarles un factor adicional para acceder a los recursos protegidos por Oracle Identity Cloud Service, como la consola Mi perfil o la consola de Identity Cloud Service.

1.  Vaya a _Seguridad_ y, a continuación, seleccione _Perímetros de red_.
    
    ![Imagen](images/L5002.png)
    
        A network perimeter contains a list of IP addresses.
        
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        
        You can also configure Oracle Identity Cloud Service so that users can log in, using only IP addresses contained in the network perimeter. This is known as whitelisting, where users who attempt to sign in to Oracle Identity Cloud Service with these IP addresses will be accepted. Whitelisting is the reverse of blacklisting, the practice of identifying IP addresses that are suspicious, and as a result, will be denied access to Oracle Identity Cloud Service.
        
2.  Haga clic en _Agregar_ para agregar un nuevo perímetro de red. Introduzca un rango de IP como se muestra a continuación. Por ahora, coloque un rango ficticio o incluso una dirección ficticia (por ejemplo, 10.0.0.0), de modo que no se deniegue la autenticación y haga clic en _Guardar_.
    
    ![Imagen](images/L5003.png)
    
3.  Vaya a _Seguridad_ y seleccione _Políticas de conexión_. Para definir una política de conexión para la aplicación de Salesforce, haga clic en _Agregar_.
    
    ![Imagen](images/L5004.png)
    
        Note that a Default Sign-On Policy exists and is enabled by default. The default policy applies to all application that have not been explicitly mapped to a sign on policy.
        
4.  Introduzca un _nombre de política_ y una _descripción_ en la sección Detalles y haga clic en el botón _\>_ para pasar a la siguiente sección.
    
    ![Imagen](images/L5005.png)
    
5.  En la sección _Reglas de conexión_, haga clic en _Agregar reglas_ para agregar una nueva regla.
    
    ![Imagen](images/L5006.png)
    
6.  Cree una regla para solicitar MFA cuando un usuario pertenezca al grupo Empleados. Asegúrese de que se han definido los siguientes parámetros:
    
    | Parámetro | Valor |
    | --- | --- |
    | Nombre de regla | MFA para empleados |
    | Y es miembro de estos grupos | Empleado |
    | Solicitar factor adicional | Activado |
    | Inscripción | Opcional |
    | Frecuencia de factor adicional al utilizar un dispositivo de confianza | Una Vez por Sesión |
    
7.  Haga clic en _Guardar_ para guardar la regla
    
    ![Imagen](images/L5007.png)
    
8.  Haga clic _Agregar_ para agregar una segunda regla.
    
    ![Imagen](images/L5008.png)
    
9.  Asegúrese de que los siguientes parámetros están definidos:
    
    | Parámetro | Valor |
    | --- | --- |
    | Nombre de regla | IP en lista negra |
    | Y la dirección IP de cliente del usuario es | En una o más de estas redes IP incluidas en la lista negra |
    | El acceso es | Denegado |
    
10.  Haga clic en _Guardar_ para guardar la regla
    
    ![Imagen](images/L5009.png)
    
        Note the Sign-On Rules are ordered. The actions (allow/deny/prompt for MFA) are determined by the first rule where the conditions match. If none of the rule conditions match the default action is deny.
        
11.  Para mover la regla _IP en la lista negra_ a la parte superior, haga clic en el icono de reordenación y arrastre la regla a la parte superior. Haga clic en _Finalizar_.
    
    ![Imagen](images/L5010.png)
    
        The above rules are evaluated as below
        
        a. if user access is from a blacklisted IP the deny access.
        b. if user is a member of Salesforce Employee group, then prompt for MFA once per session.
        c. for all other users deny access.
        
12.  Haga clic en el separador _Aplicaciones_ y en el botón _Asignar_.
    
    ![Imagen](images/L5011.png)
    
13.  Seleccione la aplicación _Salesforce_ y haga clic en _Aceptar_.
    
    ![Imagen](images/L5012.png)
    
14.  Salesforce se ha agregado correctamente a la política de conexión.
    
    ![Imagen](images/L5013.png)
    
15.  Vuelva a _Políticas de conexión_ y haga clic en _Activar_ para activar la política de _Salesforce_.
    
    ![Imagen](images/L5014.png)
    

## Tarea 3: Conexión como usuario final con MFA

*   _Personas_:
    *   Usuario final

1.  Conéctese a la consola de IDCS con una cuenta de usuario asignada a la aplicación Salesforce.
    
2.  Haga clic en _Salesforce Chatter_.
    
    ![Imagen](images/L5015.png)
    
3.  Se le pedirá que active la verificación en 2 pasos para su cuenta. Haga clic en _Activar_ para activar la verificación en 2 pasos para la cuenta. Tenga en cuenta que la inscripción se ha definido como opcional en la política de conexión y que el usuario final tiene la opción de omitir este paso.
    
    ![Imagen](images/L5016.png)
    

### Opciones de MFA en Oracle Identity Cloud Service: correo electrónico

    Email:
    Users receive an email message that contains a temporary passcode that must be used during log in.
    

R. Oracle define automáticamente su cuenta de correo electrónico principal como una opción de MFA. Sin embargo, se recomienda hacer clic en la opción _Definir como valor por defecto_.

![Imagen](images/L5017.png)

B. Introduzca el código que se ha enviado a su dirección de correo electrónico y haga clic en _Verificar_.

![Imagen](images/L5018.png)

C. Ahora, su dirección de correo electrónico se ha inscrito correctamente. Se recomienda configurar un método adicional. Para continuar con la configuración de MFA, seleccione otro método, es decir, _Aplicación móvil_. También puede hacer clic en _Listo_ y finalizar la configuración de MFA.

![Imagen](images/L5019.png)

    Note: Under *My Profile* and under the tab *Security*, further MFA options can be configured at a later point of time.
    

### Opciones de MFA en Oracle Identity Cloud Service: aplicación Mobile Authenticator

    Mobile Authenticator Application:
    Users generate an OTP on the OMA App on their device that must be used during log in.
    

R. Descargue la aplicación _Oracle Mobile Authenticator_ en su teléfono móvil.

B. Si su móvil tiene conectividad a Internet, escanee el código QR.

![Imagen](images/L5020.png)

C. Si su teléfono móvil no tiene conectividad a Internet o está utilizando un autenticador de 3a parte (por ejemplo, Google Authenticator) i. Marque la casilla _Modo fuera de línea o Usar otra aplicación de autenticador_ y escanee el nuevo código QR fuera de línea que se genera. Después de escanear el código QR, la aplicación móvil generará OTP cada 30 segundos. ii. Introduzca la OTP en el campo de código de acceso y haga clic en _Verificar_.

![Imagen](images/L5021.png)

D. Puede inscribir un método de verificación en 2 pasos adicional haciendo clic en otro método. En este tutorial, continuamos con _Número de móvil_.

### Opciones de MFA en Oracle Identity Cloud Service: mensaje de texto (SMS)

    Text Message (SMS):
    Users receive a temporary passcode as a text message (SMS) on their device that must be used during log in.
    

R. Haga clic en _Número de móvil_.

B. Introduzca su número de teléfono móvil y haga clic en _Enviar_.

![Imagen](images/L5022.png)

C. Se enviará un _SMS_ (mensaje de texto) que contenga una OTP a su número de móvil.

D. Introduzca la OTP en el campo de código de acceso y haga clic en _Verificar_.

### Opciones de MFA en Oracle Identity Cloud Service: preguntas de seguridad

    Security Questions:
    Users are prompted during the sign-in process to correctly answer a defined number of security questions to verify their identity.
    

A. Haga clic en _Preguntas de seguridad_.

B. Seleccione la pregunta que desea utilizar, introduzca las respuestas que recordará y, opcionalmente, introduzca aciertos para cada respuesta y haga clic en _Guardar_.

![Imagen](images/L5023.png)

C. Haga clic en _Listo_ para completar el proceso. Ahora será redirigido a Salesforce.

![Imagen](images/L5024.png)

### Opciones de MFA en Oracle Identity Cloud Service: método de verificación de copias de seguridad

    Backup verification method:
    Oracle makes it possible to use a backup verification method in case the current MFA option cannot be entered, i.e. when the configured MFA device is not reachable.
    

A. Acceda a IDCS y, en Mis aplicaciones, seleccione Salesforce. Se mostrará una opción de MFA preconfigurada. En este caso, es la _aplicación de autenticador móvil_.

B. Haga clic en _Usar método de verificación de copia de seguridad_. Se muestra la lista de métodos de verificación en 2 pasos de copia de seguridad inscritos.

![Imagen](images/L5025.png)

C. En este ejemplo, seleccione _Security Questions_ (Preguntas de seguridad).

D. Responda a la pregunta de seguridad y haga clic en _Verificar_.

![Imagen](images/L5026.png)

## Tarea 4: Bloquear el acceso mediante una política

*   _Personas_:
    
    *   Administrador
    
        Block blacklisted IPs
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        

1.  Vaya a _Seguridad_ y seleccione _Perímetros de red_. Haga clic en el menú a la derecha del perímetro de red de IPs en la lista negra que creó anteriormente y haga clic en Editar. Se mostrará la dirección IP ficticia configurada anteriormente.
    
    ![Imagen](images/L5027.png)
    
2.  Cambie la IP ficticia al rango de IP:
    

*   Abra un explorador web y acceda a un sitio web que muestre su _dirección IP pública_, por ejemplo: https://www.whatismyip.com/
    
*   Copie su dirección IP pública.
    
*   Vuelva a IDCS, sustituya la dirección IP ficticia por la dirección IP pública y haga clic en _Save_.
    
    ![Imagen](images/L5028.png)
    

## Tarea 5: Prueba de la política actualizada

*   _Personas_:
    *   Usuario final

1.  Conéctese a la consola de IDCS con una cuenta de usuario asignada a la aplicación Salesforce.
    
2.  Haga clic en el mosaico _Chatter de Salesforce_.
    
    ![Imagen](images/L5029.png)
    
3.  Usted denegará el acceso, dado que su solicitud se origina en una IP en la lista negra.
    
    ![Imagen](images/L5030.png)
    

Ahora puede continuar con la siguiente práctica de laboratorio

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020