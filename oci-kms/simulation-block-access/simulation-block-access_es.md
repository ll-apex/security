# Prueba de simulación de emergencia: acceso a datos de bloque

## Introducción

Escenario: Usted es el gerente del equipo de operaciones de seguridad de su compañía. Se acaba de producir una alerta de seguridad grave en su empresa. Después de hablar con el CEO de la compañía, el CISO ha tomado la decisión de cerrar el acceso a los datos almacenados en la nube. Por lo tanto, el CISO le pide que se asegure de que todos los datos cifrados almacenados en OCI, la nube de su empresa, sean inaccesibles.

Para bloquear el acceso a los datos, desactivará la clave de cifrado que ha creado al principio de este laboratorio práctico. Como esta clave se utiliza para cifrar datos en el cubo que ha creado, así como en Autonomous Database, desactivará a su vez cualquier posibilidad de acceder a los datos cifrados, así como a los propios objetos, incluso para los administradores de OCI.

Como administrador de datos, probará el acceso a los datos para darse cuenta de que ya no puede acceder a los datos ni a los objetos.

Tiempo estimado: 10 minutos

[Recorrido por el laboratorio](videohub:1_jhm25js1)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Simule una situación de emergencia en la que, como cliente, desee bloquear completamente el acceso a sus datos en OCI
*   Desactivar clave de cifrado desde la consola de gestión de claves CipherTrust externa
*   Pruebe el acceso a los datos cifrados y confirme que los usuarios ya no puedan acceder a los datos del cubo de almacenamiento y de Autonomous Database

## Tarea 1: Desactivación de claves en CipherTrust Manager

1.  Conéctese a la consola del gestor CipherTrust.
    
    Si ha perdido el enlace: para acceder a CipherTrust Manager as a Service, deberá crear la URL para acceder a su propio inquilino privado. Para ello, debe copiar y pegar esta URL: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM"** en la barra de direcciones del explorador y sustituir **XXX** por el número de alumno. Por ejemplo, si el número de alumno es 001, la URL completa para su propio inquilino CTM privado será: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001"**.
    
    ![Creación de URL en la barra de direcciones](images/ctm-address-bar.png "Creación de URL en la barra de direcciones")
    
    Una vez que acceda a la ventana de conexión, conéctese con el usuario **"Secops\_XXX"**, con la contraseña que se le ha proporcionado. Si no puede encontrar esta información, póngase en contacto con uno de los entrenadores para ayudarle.
    
    ![Conéctese al gestor de CipherTrust](images/ctm-login.png "Conéctese al gestor de CipherTrust")
    
2.  Introduzca las credenciales que se le han proporcionado. Ahora está conectado a la consola web de CipherTrust Manager. Haga clic en el icono Cloud Key Manager:
    
    ![CipherTrust Consola web del gestor](images/ctm-page.png "CipherTrust Consola web del gestor")
    
3.  En el panel izquierdo, haga clic en **Cloud Keys > Oracle**.
    
    ![Claves de Oracle](images/list-key.png "Claves de Oracle")
    
4.  Haga clic en los tres puntos situados a la derecha de la línea de clave y seleccione **Desactivar**:
    
    ![Desactivar claves](images/to-disable.png "Desactivar claves")
    
5.  Una nueva ventana le pedirá que confirme. Haga clic en **Desactivar**:
    
    ![Desactivar claves](images/disable.png "Desactivar claves")
    
6.  Haga clic en **Actualizar todo**:
    
    ![Refrescar Todo](images/disabling.png "Refrescar Todo")
    
    una nueva ventana le pedirá de nuevo que confirme. Haga clic en **Refrescar todo**:
    
    ![Confirmar Refrescamiento](images/refresh.png "Confirmar Refrescamiento")
    
7.  Espere hasta que las claves tengan el estado "Disabled" (Desactivado):
    
    ![Claves desactivadas](images/disabled.png "Claves desactivadas")
    

## Tarea 2: Confirmar que el acceso a los datos en el cubo es imposible como resultado

1.  Conéctese al inquilino en la nube de OCI como Data\_Manager\_XXX, donde "XXX" es el número de alumno (consulte la sección _"Introducción"_ para ver cómo conectarse a OCI) y navegue por el menú de hamburguesa principal hasta _"Almacenamiento > Object Storage > Cubos"_.
    
    ![Cubos](./images/buckets.png "Cubos")
    
2.  Como puede ver, el cubo que ha creado con una clave externa ya no es accesible e incluso un usuario como el usuario de Data Manager, que tiene derechos completos para gestionar este cubo, no puede acceder a ningún elemento de configuración ni ver ningún parámetro como Visibilidad y Nivel de almacenamiento por defecto.
    
    ![Cubos](./images/inaccessible-bucket.png "Cubos")
    
    Si hace clic en el cubo, no podrá acceder a:
    
    ![Sin acceso](./images/no-access-to-bucket.png "Sin acceso")
    
    y el cubo de recursos ocw23 sigue siendo accesible porque se ha configurado con claves gestionadas por Oracle por diseño. Es una práctica recomendada que los clientes pueden utilizar cuando no desean gestionar las claves y el ciclo de vida de las claves para los recursos que no contienen datos confidenciales. De esta forma, OCI permite a las empresas tener una solución de gestión de claves muy granular y potente para todos sus recursos de OCI.
    
3.  Ahora comprobaremos que cualquier solicitud autenticada previamente (PAR) que se haya creado ya no es funcional porque la clave estaba desactivada. Debe haber guardado la URL de una solicitud autenticada previamente para acceder al archivo Excel que ha cargado en el cubo en la tarea 2 del laboratorio 3. Copie esta URL que ha guardado y péguela en el explorador:
    
    ![Sin acceso a PAR](./images/no-access-par.png "Sin acceso a PAR")
    
    Como puede ver, la solicitud autenticada previamente ya no funciona y el mensaje de error explica claramente que esto se debe a que el cubo no puede acceder a la clave.
    

## Tarea 3: Comprobación del acceso a los datos en Autonomous Database

1.  Navegue por el menú de hamburguesa principal para: _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Aquí pueden ocurrir dos situaciones. Verá que Autonomous Database se ha parado y este es el resultado esperado. Esto se debe a que la clave que utiliza está en estado desactivado y la base de datos ya ha comprobado el estado de la clave de cifrado maestra. Si este es el caso, ¡felicidades! Ha terminado este laboratorio, puede omitir el final e ir al último laboratorio.
    
    Sin embargo, a medida que Autonomous Database Service realiza esta comprobación cada 15 minutos, el resultado de este paso puede variar en función del tiempo que haya tardado entre desactivar las claves e iniciar esta tarea y si ya se ha producido una comprobación o no. Por lo tanto, es posible que tenga la siguiente pantalla, que muestra que la base de datos aún se está ejecutando:
    
    ![Autonomous Database](./images/adb-running.png "Autonomous Database")
    
3.  En este caso, puede esperar un poco si tiene tiempo o lo más sencillo para el objetivo de la práctica de laboratorio es, como usuario de Data Manager, parar la base de datos e intentar iniciarla para confirmar que es imposible. Haga clic en el nombre de Autonomous Database:
    
    ![Autonomous Database](./images/adb-running.png "Autonomous Database")
    
    y haga clic en **Más acciones** y, a continuación, haga clic en **Detener**:
    
    ![Parada de Autonomous Database](./images/stop-adb.png "Parada de Autonomous Database")
    
    Una ventana le pedirá que confirme. Haga clic en **Parar**:
    
    ![Parada de Autonomous Database](./images/confirm-stop.png "Parada de Autonomous Database")
    
4.  Espere hasta que la base de datos se haya parado por completo:
    
    ![Instancia de Autonomous Database parada](./images/stopped-adb.png "Instancia de Autonomous Database parada")
    
5.  Intente volver a iniciar la base de datos haciendo clic en **Más acciones** e **Iniciar**:
    
    ![Inicio de Autonomous Database](./images/re-start.png "Inicio de Autonomous Database")
    
    Como puede ver, es totalmente imposible iniciar la base de datos o realizar acciones en su contenido o configuración debido al hecho de que el Gestor de Operaciones de Seguridad desactivó la clave de forma remota desde la consola Thales CipherTrust Manager:
    
    ![Inicio de Autonomous Database](./images/try-start-adb.png "Inicio de Autonomous Database")
    
    Al hacer clic en **Start**, siempre volverá a esa pantalla hasta que la clave se active en OCI Vault, lo que veremos en el siguiente laboratorio.
    

¡Felicidades! Ha bloqueado por completo el acceso a los datos de su empresa almacenados en OCI, por lo que los protege de cualquier amenaza potencial sobre la que se haya alertado a su director de seguridad de la información. Ahora puede ir al siguiente laboratorio y aprender cómo volver a activar el acceso cuando termine la alerta.

## Más información

*   [Carga de datos de archivos locales con Oracle Database Actions](https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/autonomous-database-shared/doc/load-data-sqldeveloper-web.html)
    
*   [Cargar datos en almacenamiento de objetos](https://docs.oracle.com/en-us/iaas/vision/vision-tutorials/vision/tutorials/Using_Pretrained_Models_in_the_Console/load_data.htm)
    
*   En este ejemplo, la desactivación de la clave hizo que la base de datos fuera imposible de iniciar. Esto se debe al diseño porque la clave estaba desactivada y el servicio Autonomous Database recibe una respuesta clara del servicio OCI Vault o KMS externo de que la clave está desactivada. Esta situación es muy diferente de lo que ocurriría si OCI Vault en sí mismo no estuviera disponible. Para proteger las bases de datos de producción de cualquier tipo de evento que impida que la base de datos acceda a Oracle Cloud Infrastructure Vault o al KMS externo, como una interrupción de red, Autonomous Database gestiona la interrupción de la siguiente manera:
    
        * There is a 2-hour grace period where the database remains up and running.
        
        * If Oracle Cloud Infrastructure Vault is not reachable at the end of the 2-hour grace period, the database Lifecycle State is set to Inaccessible. In this state existing connections are dropped and new connections are not allowed.
        
        * If Autonomous Data Guard is enabled, during or after the 2-hour grace period you can manually try to perform a failover operation. Autonomous Data Guard automatic failover is not triggered when you are using customer-managed encryption keys and the Oracle Cloud Infrastructure Vault is unreachable.
        
        * If Autonomous Database is stopped, then you cannot start the database when the Oracle Cloud Infrastructure Vault is unreachable. For this case, the work request shown when you click Work Requests on the Oracle Cloud Infrastructure console under Resources shows: 
        
        ```
        The Vault service is not accessible. 
        Your Autonomous Database could not be started. Please contact Oracle Support.
        ```
        
    
    Para comprobar todos los detalles al respecto, [consulte la documentación.](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/manage-keys-notes.html)
    

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Damien Rilliard, julio de 2023