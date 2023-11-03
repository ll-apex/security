# Copiar datos de auditoría en el almacenamiento de objetos mediante la API de REST de Oracle Data Safe

## Introducción

Al iniciar la pista de auditoría de la base de datos de destino en Oracle Data Safe, Oracle Data Safe comienza a copiar registros de auditoría de la pista de auditoría de la base de datos en el repositorio de Oracle Data Safe. En este laboratorio, utilizará la interfaz de programación de aplicaciones (API) de Oracle Data Safe para copiar los datos de auditoría de Oracle Data Safe para la base de datos de destino en el almacenamiento de objetos.

Tiempo de laboratorio estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear un cubo para almacenar los datos de auditoría
*   Iniciar la pista de auditoría de la base de datos de destino en Oracle Data Safe
*   Ver la cantidad de datos de auditoría recopilados por Oracle Data Safe
*   Acceso a Cloud Shell en Oracle Cloud Infrastructure y revisión del SDK para la instalación de Java
*   Configurar el SDK
*   Compilar un archivo Java
*   Obtener el OCID de compartimento para la base de datos de destino
*   Ejecutar el archivo Java compilado
*   Verifique que los datos de auditoría se copian en el cubo

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

### Supuestos

Cloud Shell está ejecutando las siguientes versiones de aplicación:

*   Java versión 11.0.17
*   Java(TM) SE Runtime Environment 18.9 (creación 11.0.17+10-LTS-269)
*   Linux 7.9
*   Oracle Cloud Infrastructure Java SDK 3.2.2

## Tarea 1: Crear un cubo para almacenar los datos de auditoría

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Storage** y, a continuación, **Buckets**.
    
2.  Asegúrese de que el compartimento está seleccionado.
    
3.  Haga clic en **Crear cubo**.
    
    Se muestra el panel **Crear cubo**.
    
4.  Para el nombre del cubo, introduzca **DataSafeAuditData**.
    
5.  Deje la configuración por defecto tal cual y haga clic en **Crear**.
    

## Tarea 2: Iniciar la pista de auditoría para la base de datos de destino en Oracle Data Safe

1.  En el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Seguridad de base de datos**.
    
2.  En **Security Center** de la izquierda, haga clic en **Activity Auditing**.
    
3.  En **Recursos relacionados**, a la izquierda, haga clic en **Pistas de auditoría**.
    
4.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
5.  A la derecha, haga clic en el nombre de la base de datos de destino de **UNIFIED\_AUDIT\_TRAIL**.
    
    Aparece la página **Detalles de pista de auditoría**.
    
6.  Haga clic en **Iniciar**.
    
    Se muestra el cuadro de diálogo **Start Audit Trail: UNIFIED\_AUDIT\_TRAIL**.
    
7.  Establezca la fecha de inicio al inicio del mes actual.
    
    *   Si es el primer día del mes, puede seleccionar el día anterior para asegurarse de recopilar todos los datos.
    *   No seleccione la opción **Depuración automática**.
8.  Haga clic en **Iniciar**. Espere a que **Collection State** cambie de **STARTING** a **COLLECTING** y, a continuación, a **IDLE**. Se tarda aproximadamente un minuto.
    

## Tarea 3: Visualización de la cantidad de datos de auditoría recopilados por Oracle Data Safe

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En **Centro de seguridad**, haga clic en **Perfiles de auditoría**.
    
3.  A la derecha, haga clic en el nombre de la base de datos de destino.
    
    Se muestra la página **Detalles de perfil de auditoría** de la base de datos de destino.
    
4.  Desplácese hacia abajo en la página hasta la sección **Calcular volumen de auditoría**.
    
5.  Haga clic en **Recopilado por Data Safe**.
    
    Se muestra el cuadro de diálogo **Calcular volumen recopilado**.
    
6.  Defina los campos **Mes de inicio** y **Mes de finalización** en el primer y último día del mes actual, respectivamente, y haga clic en **Recursos informáticos**.
    
7.  En la columna **Recopilado en Data Safe (en línea)**, tome nota del número de registros de auditoría recopilados por Oracle Data Safe.
    

## Tarea 4: Acceso a Cloud Shell en Oracle Cloud Infrastructure y revisión del SDK para la instalación de Java

El SDK para Java de Oracle Cloud Infrastructure (oci-java-sdk) proporciona un SDK para Java que puede utilizar para gestionar los recursos de Oracle Cloud Infrastructure.

1.  Para abrir Cloud Shell, en la esquina superior derecha de la consola de Oracle Cloud Infrastructure, haga clic en el icono de **Herramientas de desarrollador** y, a continuación, seleccione **Cloud Shell**.
    
    Al abrir Cloud Shell por primera vez, el directorio actual es el directorio raíz; por ejemplo, `/home/jody_glove`.
    
2.  (Opcional) Restablezca el entorno de Cloud Shell. El siguiente comando borra todos los datos del directorio `$HOME` de la máquina de Cloud Shell y restablece los archivos `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` y `$HOME/.emacs` a sus valores por defecto. Introduzca **y** en la petición de datos para confirmar.
    
        $ <copy>csreset --all</copy>
        
3.  Revise el directorio `/usr/lib64/java-oci-sdk`. Esta es la ubicación del SDK de Java de OCI.
    
        $ <copy>ls /usr/lib64/java-oci-sdk</copy>
        
        addons  apidocs  buildTools  CHANGELOG.md  CONTRIBUTING.md  examples  lib  LICENSE.txt  NOTICE.txt  README.md  shaded  third-party  THIRD_PARTY_LICENSES.txt
        
4.  Muestre el contenido del directorio `/usr/lib64/java-oci-sdk/lib`. Observe la versión del archivo `oci-java-sdk-full-version.jar`. En el siguiente ejemplo, la versión es 3.3.0.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        jersey  jersey3  oci-java-sdk-full-3.3.0.jar  oci-java-sdk-full-3.3.0-javadoc.jar  oci-java-sdk-full-3.3.0-sources.jar
        
5.  Muestre las bibliotecas de terceros en el directorio `/usr/lib64/java-oci-sdk/third-party/lib`.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/third-party/lib</copy>
        
6.  Muestre los ejemplos en el directorio `/usr/lib64/java-oci-sdk/examples`. Observe que hay un archivo `DataSafeRestAPIClientExample.java`. Este programa Java contiene comandos de la API de REST de Oracle Data Safe que copian datos de auditoría de un compartimento especificado en un cubo especificado del almacenamiento de objetos.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/examples</copy>
        
        ...
        DataSafeRestAPIClientExample.java
        ...
        
7.  Revise el archivo `DataSafeRestAPIClientExample.java`.
    
        $ <copy>cat /usr/lib64/java-oci-sdk/examples/DataSafeRestAPIClientExample.java</copy>
        

## Tarea 5: Configuración del SDK

Los SDK de Oracle Cloud Infrastructure requieren información de configuración básica, como las credenciales de usuario y el OCID de arrendamiento. En esta tarea, puede proporcionar esta información mediante la creación de un archivo de configuración.

1.  Cree un directorio denominado `.oci`, otorgue permisos `read/write/execute` y, a continuación, cámbielo a él.
    
        $ <copy>mkdir ~/.oci</copy>
        $ <copy>chmod 777 ~/.oci</copy>
        $ <copy>cd ~/.oci</copy>
        
2.  En la esquina superior derecha de la consola de Oracle Cloud Infrastructure, haga clic en el icono **Perfil** y, a continuación, seleccione el nombre de usuario.
    
3.  A la izquierda, haga clic en **Claves de API**.
    
4.  Haga clic en **Agregar clave de API**.
    
    Se muestra el cuadro de diálogo **Agregar clave de API**.
    
5.  Deje seleccionada la opción **Generar par de claves de API** y haga clic en **Descargar clave privada**. Se descarga una clave privada (archivo PEM) en el explorador. Guarde el archivo de clave privada en el directorio local que desee de la computadora.
    
6.  Haga clic en **Agregar**.
    
    Aparece el cuadro de diálogo **Vista previa del archivo de configuración** que muestra una vista previa del archivo de configuración.
    
7.  Copie el contenido de la vista previa del archivo de configuración en un archivo de texto local temporal. Asegúrese de incluir `[DEFAULT]`. Debe tener un aspecto similar a este:
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
8.  Haga clic en **Cerrar**.
    
    La nueva clave de API se muestra en **Claves de API**.
    
9.  En Cloud Shell, en la esquina superior derecha, haga clic en el icono del **menú de Cloud Shell** y seleccione **Cargar**.
    
    Se muestra el cuadro de diálogo **Carga de Archivos en el Directorio Raíz**.
    
10.  Arrastre el archivo de clave privada al cuadro de diálogo y haga clic en **Cargar**.
    
    El archivo de clave privada se carga en el directorio raíz.
    
11.  Para cerrar el cuadro de diálogo **Transferencias de archivos**, haga clic en **Ocultar**.
    
12.  Mueva el archivo de clave privada al directorio `~/.oci`. En el siguiente ejemplo, sustituya `your-private-key-file.pem` por su propio nombre de archivo de clave privada.
    
        $ <copy>mv ~/your-private-key-file.pem ~/.oci/your-private-key-file.pem</copy>
        
13.  Cree un archivo de configuración en el directorio `~/.oci` mediante el editor vi.
    
        $ <copy>vi config</copy>
        
14.  Pegue el contenido del archivo de configuración en el archivo `config`. El contenido debe tener un aspecto similar al siguiente código. No olvides incluir `[DEFAULT]`.
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
15.  Modifique la última línea para que sea la ruta al archivo de clave privada. En el siguiente ejemplo, sustituya `your-private-key-file.pem` por su propio nombre de archivo de clave privada. Elimine el texto **\# TODO**.
    
        <copy>key_file=~/.oci/your-private-key-file.pem</copy>
        
16.  Guarde y cierre el archivo (pulse **Escape**, introduzca **:wq** y, a continuación, pulse **Intro**).
    

## Tarea 6: Compilación de un archivo Java

Utilice el comando `javac` para compilar el archivo `DataSafeRestAPIClientExample.java`. Las dos variables siguientes ya están definidas en Cloud Shell. Puede utilizarlos al compilar y ejecutar el programa Java.

*   `$OCI_JAVA_SDK_LOCATION` = `/usr/lib64/java-oci-sdk`
*   `$OCI_JAVA_SDK_FULL_JAR_LOCATION` = `/usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-version.jar`

1.  Copie `DataSafeRestAPIClientExample.java` en el directorio actual (`~/.oci`).
    
        $ <copy>cp $OCI_JAVA_SDK_LOCATION/examples/DataSafeRestAPIClientExample.java .</copy>
        
2.  Compile `DataSafeRestAPIClientExample.java`. No hay salida después de compilar el programa.
    
        $ <copy>javac -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/* DataSafeRestAPIClientExample.java</copy>
        

## Tarea 7: Obtención del OCID de compartimento para la base de datos de destino

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Oracle Database** y, a continuación, **Data Safe - Seguridad de base de datos**.
    
2.  En la parte izquierda, haga clic en **Target Databases**.
    
3.  A la izquierda, seleccione el compartimento.
    
4.  A la derecha, haga clic en el nombre de la base de datos de destino.
    
    Se muestra la página **Detalles de base de datos de destino**.
    
5.  En el separador **Detalles de base de datos de destino**, anote el compartimento.
    
6.  En el menú de navegación, seleccione **Identity & Security** y, a continuación, a la derecha en **Identity**, seleccione **Compartments**.
    
7.  Haga clic en el nombre del compartimento.
    
    Se muestra la página **Detalles del compartimento**.
    
8.  En el separador **Información de compartimento**, haga clic en el enlace **Copiar** situado junto a **OCID** y pegue el OCID en un archivo de texto local temporal. Necesita el OCID para la siguiente tarea.
    

## Tarea 8: Ejecutar el archivo Java compilado

1.  Vuelva a Cloud Shell y ejecute los siguientes comandos para definir dos variables: **BUCKET** y **COMPARTMENT**. En el siguiente ejemplo, sustituya `your-compartment-ocid` por el OCID de compartimento para la base de datos de destino.
    
        $ <copy>export BUCKET=DataSafeAuditData</copy>
        $ <copy>export COMPARTMENT=your-compartment-ocid</copy>
        
2.  Busque la versión del archivo `oci-java-sdk-common-httpclient-jersey-version.jar`. En el siguiente ejemplo, la versión es 3.3.0.
    
        $ <copy> ls $OCI_JAVA_SDK_LOCATION/lib/jersey</copy>
        
        oci-java-sdk-common-httpclient-jersey-3.3.0.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-javadoc.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-sources.jar
        
3.  Ejecute `DataSafeRestAPIClientExample.class` con el siguiente comando. Sustituya `version` en `oci-java-sdk-common-httpclient-jersey-version.jar` por la versión obtenida en el paso anterior. Puede ignorar el error sobre el fallo al cargar la clase `org.slf4j.impl.StaticLoggerBinder`.
    
        $ <copy>java -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/*:$OCI_JAVA_SDK_LOCATION/third-party/jersey/lib/*:$OCI_JAVA_SDK_LOCATION/lib/jersey/oci-java-sdk-common-httpclient-jersey-version.jar:$HOME/.oci DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-14T22:01:38.325Z
        Querying for auditEvents with timeCollected Start = 2023-02-14T22:01:38.325Z, End = 2023-02-15T22:01:38.324Z
        
        
        Count35
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z  OpcRequestId: fra-1:q_rNFX-2hAnzGEoiurT376...
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of cursor  OpcRequestId: fra-1:YpGeKJmQ7HtfJLXCVGLYKIEGCEPGbsdF...
        
4.  Revise la salida. La tercera última línea de salida indica el recuento de registros de auditoría copiados en Object Storage. El valor puede ser diferente del que se muestra en este ejemplo.
    

## Tarea 9: Verifique que los datos de auditoría se copian en el cubo

1.  En el menú de navegación, seleccione **Storage** y, a continuación, **Buckets**.
    
2.  Asegúrese de que el compartimento está seleccionado.
    
3.  Haga clic en el nombre del cubo.
    
    Aparecerá la página **Detalles de cubo** del cubo.
    
4.  Haga clic en la sección **Objetos**.
    
5.  Observe que ahora tiene una línea de ítem denominada `auditeventjson` que contiene el texto `noofrecords_<some-number>`. Son los datos de auditoría copiados del repositorio de Oracle Data Safe. `<some-number>` es el número de registros de auditoría copiados.
    
        <copy>auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z</copy>
        
6.  Suprimir el objeto y el cursor: uno a la vez, haga clic en los tres puntos al final de la fila y seleccione **Suprimir**. En el cuadro de diálogo **Confirm Delete Object**, haga clic en **Delete**.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Visión general de Auditoría de actividad](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [Pistas de Auditoría](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [Introducción (con SDK para Java)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (en GitHub)](https://github.com/oracle/oci-java-sdk)
*   [SDK para Java (configuración del SDK)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [API de Data Safe (puntos finales y referencia)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [SDK de Oracle Cloud Infrastructure Java (paquetes y clases)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, base de datos
*   **Consultores**: Richard Evans, Bettina Schaeumer, Archana Rao, Anna Haikl
*   **Última actualización por/fecha**: Jody Glover, 11 de abril de 2023