# Prueba de simulación de emergencia: volver a activar el acceso a los datos

## Introducción

Escenario: la alerta de seguridad ha terminado. Después de todas las comprobaciones necesarias, el CISO ha tomado la decisión de volver a activar el acceso a los datos almacenados en la nube. Por lo tanto, el CISO le pide, como director del equipo de operaciones de seguridad de la compañía, que continúe y restaure las operaciones normales y el acceso a los datos en OCI, la nube de su compañía.

Para volver a activar el acceso a los datos, volverá a activar la clave de cifrado que ha creado al principio de este laboratorio práctico. Como esta clave se utiliza para cifrar datos en el cubo que ha creado, así como en Autonomous Database, se volverá a activar completamente el acceso a los datos.

Como administrador de datos, probará que el acceso adecuado a los datos es realmente posible y que las operaciones normales se pueden reiniciar, ahora que la alerta de seguridad ha terminado. Esto marcará el final de este laboratorio práctico.

Tiempo estimado: 10 minutos

[Recorrido por el laboratorio](videohub:1_cg98w5b0)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Vuelva a activar la clave de cifrado utilizada en OCI desde la consola de gestión de claves CipherTrust externa a medida que finaliza la alerta de seguridad
*   Pruebe el acceso a los datos cifrados y confirme que los usuarios pueden volver a acceder a los datos en el cubo de almacenamiento y en Autonomous Database

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Todas las prácticas anteriores se han completado correctamente

## Tarea 1: volver a activar la clave en el gestor de CipherTrust

1.  Vuelva a la consola del gestor CipherTrust. Si lo cerró o perdió el enlace: para acceder a CipherTrust Manager as a Service, deberá crear la URL para acceder a su propio inquilino privado. Para ello, debe copiar y pegar esta URL: "https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM" en la barra de direcciones del explorador y agregar el número de alumno al final de la URL. Por ejemplo, si su número de alumno es 001, la URL completa para su propio inquilino CTM privado será: "https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001". Una vez que acceda a su ventana de inicio de sesión, inicie sesión con su usuario "Secops\_XXX", con la contraseña que se le ha proporcionado. Si no puede encontrar esta información, póngase en contacto con uno de los entrenadores para ayudarle.
    
    ![Conéctese al gestor de CipherTrust](images/ctm-login.png "Conéctese al gestor de CipherTrust")
    
    Introduzca las credenciales que se le han proporcionado. Ahora está conectado a la consola web de CipherTrust Manager. Haga clic en el icono Cloud Key Manager:
    
    ![CipherTrust Consola web del gestor](images/ctm-page.png "CipherTrust Consola web del gestor")
    
2.  En el panel izquierdo, haga clic en **Cloud Keys > Oracle**.
    
    ![Claves de Oracle](images/menu-keys.png "Claves de Oracle")
    
3.  Haga clic en los tres puntos situados a la derecha de la línea de clave y seleccione **Activar**:
    
    ![Activar claves](images/to-enable.png "Activar claves")
    
4.  Una nueva ventana le pedirá que confirme. Haga clic en **Activar**:
    
    ![Activar claves](images/enable-key.png "Activar claves")
    
5.  Haga clic en **Actualizar todo**:
    
    ![Refrescar Todo](images/refresh-all.png "Refrescar Todo")
    
    una nueva ventana le pedirá de nuevo que confirme. Haga clic en **Refrescar todo** de nuevo:
    
    ![Refrescar Todo](images/refresh.png "Refrescar Todo")
    
6.  Espere hasta que las claves tengan el estado "Enabled" (Activado):
    
    ![Claves activadas](images/enabled-key.png "Claves activadas")
    

## Tarea 2: Como resultado, es posible confirmar el acceso a los datos en el cubo

1.  Conéctese al inquilino en la nube de OCI como Data\_Manager\_XXX, donde "XXX" es el número de alumno (consulte el laboratorio TODO para ver cómo conectarse a OCI) y navegue por el menú de hamburguesa principal hasta _"Almacenamiento > Almacenamiento de objetos > Cubos"_.
    
    ![Cubos](./images/buckets.png "Cubos")
    
2.  Como puede ver, se puede volver a acceder al cubo que ha creado con una clave externa:
    
    ![Cubos](./images/bucket-visible.png "Cubos")
    
    Si hace clic en el cubo, podrá acceder y ver los objetos dentro del cubo que se muestran a continuación:
    
    ![Acceso](./images/upload-object.png "Acceso")
    
3.  Ahora comprobaremos que la solicitud autenticada previamente (PAR) que ha creado vuelva a funcionar cuando la clave esté activada. Copie la URL que ha guardado en la tarea 2 del laboratorio 3 y péguela de nuevo en el explorador. Confirme que puede descargar el documento. Por lo tanto, ahora hemos confirmado que al volver a activar la clave desde la instancia externa de CipherTrust Manager se devuelve un comportamiento totalmente funcional al cubo de almacenamiento de OCI.
    

## Tarea 3: Comprobación del acceso a los datos en Autonomous Database

1.  Navegue por el menú de hamburguesa principal para: _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Aquí puedes tener dos situaciones. La base de datos ya se ha reiniciado si Autonomous Database Service ya ha comprobado que la clave está activada de nuevo. O lo más probable es que la base de datos siga parada:
    
    ![Instancia de Autonomous Database parada](./images/stopped-adb.png "Instancia de Autonomous Database parada")
    
3.  Por lo tanto, iniciará la base de datos porque la clave se vuelve a activar haciendo clic en **Más acciones** e **Iniciar**:
    
    ![Inicio de Autonomous Database](./images/re-start.png "Inicio de Autonomous Database")
    
    Una vez que haga clic en **Start** (Iniciar), verá que la base de datos se está iniciando:
    
    ![Inicio de una instancia de Autonomous Database](./images/starting-adb.png "Inicio de una instancia de Autonomous Database")
    
    Espere hasta que se inicie la base de datos:
    
    ![Autonomous Database disponible](./images/adb-available.png "Autonomous Database disponible")
    
    Como puede ver, ahora es posible iniciar la base de datos a medida que el servicio Autonomous Database vuelve a activar y acceder a la clave.
    
4.  Para garantizar que los datos se puedan descifrar, intentemos acceder a los datos de la base de datos. Para ello, haga clic en la pantalla de inicio **Acciones de base de datos**:
    
    ![Acciones de base de datos](./images/db-actions.png "Acciones de base de datos")
    
5.  Una vez allí, haga clic en el cuadrado **SQL** en Development:
    
    ![Desarrollo de SQL](./images/sql.png "Desarrollo de SQL")
    
6.  La interfaz de usuario de desarrollo de Web SQL está abierta y ahora puede ver los datos en la tabla, hacer clic con el botón derecho en la tabla y hacer clic en **Abrir**:
    
    ![Abrir tabla](./images/see-data.png "Abrir tabla")
    
7.  En la nueva ventana, haga clic en el separador **Datos**:
    
    ![Ver datos](./images/data.png "Ver datos")
    
    Como puede ver, ahora tiene de nuevo una visibilidad completa de los datos de la base de datos, ya que la clave se ha vuelto a activar.
    

¡Felicidades! Ha terminado este laboratorio práctico. Por favor llame a uno de los entrenadores para mostrar su finalización y hacer cualquier pregunta. ¡Esperamos que lo hayas disfrutado y aprendido algo! El equipo está aquí para responder cualquier pregunta que pueda tener. Y si te gustó, ¡habla de ello a tu alrededor! ¡Nos vemos pronto!

## Más información

*   [Uso de sus propias claves en Vault para el cifrado del servidor](https://docs.oracle.com/en-us/iaas/Content/Object/Tasks/encryption.htm#UsingYourKMSKeys)
*   [Gestión de claves de cifrado en Autonomous Database](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-encrypt-set-rotate-keys.html#GUID-0795135D-B057-4DBC-92C9-368AF4C82D0A)

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Sonia, agosto de 2023