# Programar la copia de datos de auditoría en Object Storage mediante la API de REST de Oracle Data Safe

## Introducción

Al iniciar la pista de auditoría de la base de datos de destino en Oracle Data Safe, Oracle Data Safe comienza a copiar registros de auditoría de la pista de auditoría de la base de datos en el repositorio de Oracle Data Safe. En este laboratorio, utilizará la interfaz de programación de aplicaciones (API) de Oracle Data Safe y crontab en una instancia informática para programar la copia de los datos de auditoría de Oracle Data Safe para la base de datos de destino en el almacenamiento de objetos.

Si ya tiene un cubo en Oracle Cloud Infrastructure y ha iniciado la pista de auditoría para la base de datos de destino en Oracle Data Safe, puede omitir las tareas 1 y 2.

Tiempo de laboratorio estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear un cubo en el compartimento
*   Iniciar la pista de auditoría de la base de datos de destino en Oracle Data Safe
*   Ver la cantidad de datos de auditoría recopilados por Oracle Data Safe
*   Crear claves SSH en Cloud Shell
*   Crear una red virtual en la nube (VCN)
*   Creación de una instancia informática con la imagen de Oracle Linux Cloud Developer 8
*   Conéctese a su instancia informática desde Cloud Shell
*   Creación de una clave de API
*   Cargue su clave privada (archivo PEM) en su instancia informática
*   Cree un archivo de configuración
*   Compile el programa Java.
*   Obtener el OCID de compartimento para la base de datos de destino
*   Ejecutar el archivo de clase Java compilado
*   Verifique que los datos de auditoría estén en el cubo
*   Crear un script SH para cronjob
*   Programar el script SH mediante crontab
*   Eliminar la actividad programada en crontab

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

## Tarea 1: Creación de un cubo en el compartimento

Cree un cubo para almacenar los datos de auditoría. También puede utilizar el cubo para transferir un archivo PEM a una instancia informática en una tarea posterior.

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Storage** y, a continuación, **Buckets**.
    
2.  Seleccione el compartimento.
    
3.  Haga clic en **Crear cubo**.
    
    Aparece el cuadro de diálogo **Crear cubo**.
    
4.  Para el nombre del cubo, introduzca **DataSafeAuditData**.
    
5.  Deje la configuración por defecto tal cual y haga clic en **Crear**.
    

## Tarea 2: Iniciar la pista de auditoría para la base de datos de destino en Oracle Data Safe

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Oracle Database** y, a continuación, **Data Safe - Seguridad de base de datos**.
    
2.  En **Security Center** de la izquierda, haga clic en **Activity Auditing**.
    
3.  En **Recursos relacionados**, a la izquierda, haga clic en **Pistas de auditoría**.
    
4.  En la lista desplegable **Compartimento** de la izquierda, asegúrese de que el compartimento está seleccionado.
    
5.  A la derecha, haga clic en el nombre de la base de datos de destino de **UNIFIED\_AUDIT\_TRAIL**.
    
    Aparece la página **Detalles de pista de auditoría**.
    
6.  Haga clic en **Iniciar**.
    
    Se muestra el cuadro de diálogo **Start Audit Trail: UNIFIED\_AUDIT\_TRAIL**.
    
7.  Establezca la fecha de inicio al inicio del mes actual.
    
8.  Haga clic en **Iniciar**. Espere a que **Collection State** cambie de **STARTING** a **COLLECTING** y, a continuación, a **IDLE**. Se tarda aproximadamente un minuto.
    

## Tarea 3: Visualización de la cantidad de datos de auditoría recopilados por Oracle Data Safe

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Auditoría de actividad**.
    
2.  En **Centro de seguridad**, haga clic en **Perfiles de auditoría**.
    
3.  A la derecha, haga clic en el nombre de la base de datos de destino.
    
    Se muestra la página **Detalles de perfil de auditoría** de la base de datos de destino.
    
4.  Desplácese hacia abajo en la página hasta la sección **Calcular volumen de auditoría**.
    
5.  Haga clic en **Recopilado por Data Safe**.
    
    Se muestra el cuadro de diálogo **Calcular volumen recopilado**.
    
6.  Defina los campos **Mes de inicio** y **Mes de finalización** en el primer y último día del mes actual, respectivamente, y haga clic en **Recursos informáticos**. Anote el número de registros de auditoría recopilados por Oracle Data Safe.
    
7.  Si, por algún motivo, el número de registros de auditoría es igual a cero, ejecute el script SQL [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) en Database Actions para cargar datos de muestra en la base de datos. Este script genera una actividad de base de datos auditable para el usuario `ADMIN`. A continuación, repita los pasos 5 y 6 para ver la cantidad de datos de auditoría recopilados.
    

## Tarea 4: Creación de claves SSH en Cloud Shell

En Cloud Shell, cree un par de claves SSH que pueda utilizar para conectarse a la instancia informática. El nombre estándar de los talleres LiveLabs es `cloudshellkey`.

1.  En la barra de herramientas de Oracle Cloud Infrastructure, haga clic en **Herramientas de desarrollador** y, a continuación, seleccione **Cloud Shell**. Al abrir Cloud Shell por primera vez, se encuentra en el directorio raíz; por ejemplo, `/home/jody_glove`.
    
2.  (Opcional) Restablezca el entorno de Cloud Shell. El siguiente comando borra todos los datos del directorio `$HOME` de la máquina de Cloud Shell y restablece los archivos `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` y `$HOME/.emacs` a sus valores por defecto. Introduzca **y** en la petición de datos para confirmar.
    
        $ <copy>csreset --all</copy>
        
3.  Cree un directorio `.ssh` en el directorio raíz de la máquina de Cloud Shell, cámbielo y, a continuación, verifique el directorio en el que está trabajando.
    
        $ <copy>mkdir ~/.ssh</copy>
        $ <copy>cd ~/.ssh</copy>
        $ <copy>pwd</copy>
        
        /home/your-user-name/.ssh
        
4.  Mientras esté en el directorio `.ssh`, genere un par de claves SSH. El siguiente comando genera dos claves: una clave privada denominada `cloudshellkey` y una clave pública denominada `cloudshellkey.pub`. Utilice la convención de nomenclatura `cloudshellkey`, ya que es un estándar LiveLabs. Cuando se le solicite que introduzca una frase de contraseña, simplemente haga clic en **Intro** dos veces para no introducir una frase de contraseña.
    
        $ <copy>ssh-keygen -b 2048 -t rsa -f cloudshellkey</copy>
        
5.  Confirme que los archivos de clave privada y clave pública existen en el directorio `.ssh`.
    
        $ <copy>ls</copy>
        
        cloudshellkey cloudshellkey.pub
        
6.  Muestre el contenido de la clave pública. Posteriormente, copie esta información en el portapapeles y péguela en el cuadro de claves SSH al crear la instancia informática.
    
        $ <copy>cat cloudshellkey.pub</copy>
        
7.  Deje abierto Cloud Shell.
    

## Tarea 5: Creación de una red virtual en la nube (VCN)

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Networking** y, a continuación, **Virtual Cloud Network**.
    
2.  Seleccione el compartimento.
    
3.  Haga clic en **Crear VCN**.
    
4.  En **Nombre**, introduzca **labVCN**.
    
5.  Seleccione el compartimento.
    
6.  En **Bloques CIDRIPv4 CIDR IPv4**, introduzca **10.0.0.0/24**.
    
7.  Deje seleccionada la casilla de control **Usar los nombres de host de DNS en esta VCN**.
    
8.  En **Etiqueta de DNS**, introduzca **labVCN**.
    
9.  Haga clic en **Crear VCN**.
    
10.  Haga clic en **Crear subred**.
    
11.  Para el nombre, introduzca **lab-public-subnet1**.
    
12.  Seleccione el compartimento.
    
13.  Para el tipo de subred, deje **Regional** seleccionado.
    
14.  En **IPv4 CIDR Block**, introduzca **10.0.0.0/24**.
    
15.  Para acceder a la subred, deje **Subred pública** seleccionada.
    
16.  Deje seleccionada la casilla de control **Usar nombres de host de DNS en esta SUBRED**.
    
17.  En **Etiqueta DNS**, introduzca **subnet1**.
    
18.  Haga clic en **Crear subred**.
    
19.  En la parte izquierda, haga clic en **Gateways de Internet**.
    
20.  Haga clic en **Create Internet Gateway**. Se muestra el panel **Crear gateway de internet**.
    
21.  Para el nombre, introduzca **livelabs-igw**.
    
22.  Seleccione el compartimento.
    
23.  Haga clic en **Crear gateway de internet**.
    
24.  En la parte izquierda, haga clic en **Tablas de rutas**.
    
25.  En la lista de tablas de rutas, haga clic en **Tabla de rutas por defecto para labVCN**.
    
26.  Haga clic en **Agregar reglas de ruta**.
    
    Se muestra el panel **Agregar reglas de ruta**.
    
27.  Para el tipo de destino, seleccione **Gateway de Internet**.
    
28.  Para el bloque CIDR de destino, introduzca **0.0.0.0/0**.
    
29.  Para el gateway de Internet de destino, seleccione **livelabs-igw**.
    
30.  Haga clic en **Agregar reglas de ruta**.
    

## Tarea 6: Creación de una instancia informática con la imagen de Oracle Linux Cloud Developer 8

Oracle Linux Cloud Developer Image está soportado en todas las unidades de computación, excepto en las unidades de GPU. Se necesita un mínimo de 8 GB de memoria para esta imagen para todas las unidades estándar y flexibles. La única excepción es VM.Standard.E2.1. Unidad micro, que solo tiene 1 GB de memoria asignados. Debido al pequeño tamaño de memoria en VM.Standard.E2.1. Micro forma, algunos programas gráficos intensivos no están instalados en la imagen.

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Compute** y, a continuación, **Instances**.
    
2.  Seleccione el compartimento.
    
3.  Haga clic en **Crear instancia**.
    
4.  Introduzca un nombre fácil de recordar para su instancia informática.
    
5.  Deje el compartimento seleccionado.
    
6.  Deja la colocación tal cual.
    
7.  En la sección **Imagen y unidad**, haga clic en **Editar**.
    
8.  Haga clic en **Cambiar imagen**. Deje **Oracle Linux** seleccionado. Desplácese hacia abajo y seleccione **Oracle Linux Cloud Developer 8**. Seleccione la casilla de control **He revisado y acepto los siguientes documentos: Oracle LInux Cloud Developer Image Terms of Use**. Haga clic en **Seleccionar imagen**.
    
9.  Haga clic en **Cambiar unidad**. Deje seleccionada la **máquina virtual** y la serie de unidades **Ampere**. Deje **VM.Standard.A1. Imagen** flexible seleccionada. Desplácese hacia abajo e introduzca **8** GB de memoria. Haga clic en **Seleccionar unidad**.
    
10.  En la sección **Red**, haga clic en **Editar** y deje seleccionada la opción **Seleccionar red virtual en la nube existente**. En **Red virtual en la nube en su compartimento**, seleccione **labVCN**. En **Subred en su compartimento**, seleccione **lab-public-subnet1**. En **Dirección IPv4 pública**, deje seleccionada la opción **Asignar una dirección IPv4 pública**.
    
11.  En la sección **Agregar claves SSH**, seleccione **Pegar claves públicas**. Vuelva a Cloud Shell y copie toda la clave pública SSH en el portapapeles. Empieza por `ssh-rsa` y termina por algo similar a `jody_glove@1e3ebc618797`. En el cuadro Claves SSH, pegue la clave pública. Asegúrese de que no haya devoluciones difíciles. Si es necesario, puede ejecutar `cat .ssh/cloudshellkey.pub` para volver a mostrar la clave pública.
    
12.  En la sección **Volumen de inicio**, deje la configuración predeterminada tal cual.
    
13.  Haga clic en **Crear** y espere dos minutos hasta que se aprovisione la instancia informática. Se muestra la página **Solicitudes de trabajo**, donde puede ver información sobre la instancia informática.
    

## Tarea 7: Conexión a la instancia informática desde Cloud Shell

1.  Si ha salido de la página de instancia informática, puede volver a encontrarla haciendo lo siguiente: en el menú de navegación de Oracle Cloud Infrastructure, seleccione **Recursos informáticos** y, a continuación, **Instancias**. Seleccione el compartimento. Haga clic en el nombre de la instancia informática.
    
2.  En el separador **Información de instancia** de **Acceso a instancia**, copie la dirección IP pública en el portapapeles.
    
3.  En Cloud Shell, introduzca el siguiente comando `SSH` para conectarse a la instancia informática, sustituyendo `public-ip-address` por el que acaba de copiar en el portapapeles.
    
        $ <copy>ssh -i ~/.ssh/cloudshellkey opc@public-ip-address</copy>
        
    
    Recibirá un mensaje que indica que no se puede establecer la autenticidad de la instancia informática. ¿Desea continuar con la conexión?
    
4.  Introduzca **yes** para continuar.
    
    La dirección IP pública de la instancia informática se agrega a la lista de hosts conocidos en la máquina de Cloud Shell. La petición de datos de terminal se convierte en `[opc@<your-compute-name> ~]$`, donde `opc` es su cuenta de usuario en la instancia informática. Ahora está conectado a su nueva instancia informática.
    

## Tarea 8: Creación de una clave de API

La configuración del SDK consta de tres partes: crear una clave de API, crear un archivo de configuración y cargar un archivo PEM en la instancia informática. Para utilizar el SDK para Java, debe tener un par de claves utilizado para firmar solicitudes de API, con la clave pública cargada en Oracle. Solo el usuario que llama a la API debe estar en posesión de la clave privada.

1.  Comience por crear una clave de API. Para ello, en la esquina superior derecha de la consola de Oracle Cloud Infrastructure, haga clic en el icono **Perfil** y, a continuación, haga clic en su nombre de usuario.
    
2.  A la izquierda, haga clic en **Claves de API**.
    
3.  Haga clic en **Agregar clave de API**.
    
    Se muestra el cuadro de diálogo **Agregar clave de API**.
    
4.  Deje seleccionada la opción **Generar par de claves de API**, haga clic en **Descargar clave privada** y guarde la clave privada (archivo PEM) en un directorio local de la computadora.
    
5.  Haga clic en **Agregar**.
    
    Aparece el cuadro de diálogo **Vista previa del archivo de configuración**. Este cuadro de diálogo muestra una vista previa del archivo de configuración.
    
6.  Copie el contenido del archivo de configuración en un archivo de texto temporal porque lo necesita en una tarea posterior. Su contenido es similar al siguiente:
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=your-fingerprint
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
7.  Haga clic en **Cerrar**.
    
8.  En Cloud Shell, cambie al usuario `root`.
    
        $ <copy>sudo su -</copy>
        
9.  Cree un directorio `.oci`.
    
        # <copy>mkdir ~/.oci</copy>
        

## Tarea 9: Carga de la clave privada (archivo PEM) en la instancia informática

Cargue su clave privada (archivo PEM) en el almacenamiento de objetos y, a continuación, cópiela en su instancia informática.

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Almacenamiento** y, a continuación, **Cubos**. Seleccione el compartimento. Haga clic en el nombre del cubo.
    
    Se muestra la página **Detalles de cubo**.
    
2.  Haga clic en **Cargar** y, a continuación, en **Objetos**.
    
    Aparece el panel **Cargar objetos**.
    
3.  Arrastre el archivo de clave privada al área **Seleccionar archivos de la computadora** y haga clic en **Cargar**.
    
4.  Haga clic en **Cerrar**.
    
5.  Al final de la fila de la lista de archivos de clave privada, haga clic en los tres puntos y, a continuación, seleccione **Crear solicitud autenticada previamente**.
    
    Se muestra el panel **Crear solicitud preautenticada**.
    
6.  Haga clic en **Crear solicitud autenticada previamente**.
    
    Se muestra el cuadro de diálogo **Detalles de solicitud autenticada previamente**.
    
7.  Copie la **URL de solicitud autenticada previamente** en el portapapeles y péguela en un archivo de texto local temporal. _IMPORTANTE:_ no podrá volver a ver esta URL después de cerrar el cuadro de diálogo.
    
8.  Haga clic en **Cerrar**.
    
9.  En Cloud Shell, cambie al directorio `.oci`.
    
        # <copy>cd ~/.oci</copy>
        
10.  Utilice el comando `WGET` para copiar el archivo de clave privada del almacenamiento de objetos en el directorio `.oci`. Sustituya `pre-authenticated-request-url` por su propia URL.
    
        # <copy>wget pre-authenticated-request-url</copy>
        
11.  Muestre el contenido del directorio para asegurarse de que el archivo de clave privada esté presente.
    
        # <copy>ls</copy>
        

## Tarea 10: Creación de un archivo de configuración

En esta tarea, creará un archivo de configuración denominado `config` en el directorio `.oci` para el SDK y, a continuación, agregará el contenido de API obtenido de la clave de API (que ha creado en una tarea anterior). En el archivo de configuración, corrija la última línea agregando la ruta real al archivo de clave privada en la instancia informática. El archivo java que compile en una tarea posterior busca el archivo de configuración en `~/.oci/config` con un perfil denominado `DEFAULT`.

1.  Con el editor vi y mientras sigue en el directorio `.oci`, cree un archivo de configuración.
    
        # <copy>vi config</copy>
        
2.  Pegue el contenido del archivo de configuración en el archivo `config`. Nota: Antes pegó este contenido en un archivo de texto temporal. El contenido tiene un aspecto similar al siguiente código. Asegúrese de incluir `[DEFAULT]` en la parte superior.
    
         <copy>[DEFAULT]
         user=ocid1.user.oc1...
         fingerprint=your-fingerprint
         tenancy=ocid1.tenancy.oc1...
         region=eu-frankfurt-1
         key_file=<path to your private keyfile> # TODO</copy>
        
3.  Modifique la última línea para que sea la ruta al archivo PEM de la instancia informática. En el siguiente ejemplo, sustituya `your-private-key-file-name` por su propio nombre de archivo de clave privada.
    
        <copy>key_file=~/.oci/your-private-key-file-name</copy>
        
4.  Guarde y cierre el archivo (pulse **Escape**, introduzca **:wq** y pulse **Intro**).
    
5.  Muestre el contenido del directorio actual y asegúrese de que el archivo `config` está allí.
    
        # <copy>ls</copy>
        
        config  your-private-key-file-name
        

## Tarea 11: Compilación de un programa Java

La imagen de Oracle Linux Cloud Developer incluye el software SDK y Java ya instalado. El archivo jar de OCI se encuentra en `/usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-<version>.jar` y las bibliotecas de terceros están en `/usr/lib64/java-oci-sdk/third-party/lib`. Para compilar un archivo Java, utilice el comando `javac`.

En esta tarea, compilará un programa Java denominado `DataSafeRestAPIClientExample.java`, que viene con la instalación del SDK. El objetivo de este programa es copiar datos de auditoría del repositorio de Oracle Data Safe en un cubo de almacenamiento de objetos especificado. Si es necesario, también puede descargar el programa directamente desde Github ejecutando el siguiente comando: `wget https://raw.githubusercontent.com/oracle/oci-java-sdk/master/bmc-examples/src/main/java/DataSafeRestAPIClientExample.java`.

1.  Verifique la versión del archivo `oci-java-sdk-full-version.jar`. En este ejemplo, la versión es 2.27.0.
    
        # <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        oci-java-sdk-full-2.27.0.jar  oci-java-sdk-full-2.27.0-javadoc.jar  oci-java-sdk-full-2.27.0-sources.jar
        
2.  Cambie al directorio `/usr/lib64/java-oci-sdk`.
    
        # <copy>cd /usr/lib64/java-oci-sdk</copy>
        
3.  Compile el archivo `DataSafeRestAPIClientExample.java`. Asegúrese de utilizar la versión correcta en el nombre de archivo `oci-java-sdk-full-<version>.jar`. El siguiente ejemplo utiliza la versión 2.27.0.
    
    Es muy común que un programa Java dependa de una o más bibliotecas externas (archivos JAR). Utilice el indicador `-classpath` (o `-cp`) para indicar al compilador dónde buscar bibliotecas externas. Por defecto, el compilador busca en la classpath de inicialización de datos y en la variable de entorno `CLASSPATH`.
    
        # <copy>javac -cp lib/oci-java-sdk-full-2.27.0.jar:third-party/lib/* examples/DataSafeRestAPIClientExample.java</copy>
        
    
    Nota: No hay salida después de compilar el archivo. Simplemente vuelve a la petición de datos.
    
4.  Cambie al directorio `examples` y muestre los archivos. Confirme que ahora tiene un archivo de clase denominado `DataSafeRestAPIClientExample.class`.
    
        # <copy>cd examples</copy>
        # <copy>ls</copy>
        

## Tarea 12: Obtención del OCID de compartimento para la base de datos de destino

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
    

## Tarea 13: Ejecutar el archivo de clase Java compilado

Ejecute el archivo `DataSafeRestAPIClientExample.class` para probar que se ejecuta sin errores antes de programarlo. El programa requiere dos entradas, que se pueden definir por adelantado:

*   Nombre del cubo en el que almacenar los datos de auditoría copiados
*   OCID de compartimento de la base de datos de destino

1.  Ejecute los siguientes comandos para configurar dos variables: **BUCKET** y **COMPARTMENT**. Sustituya `compartment-ocid-for-target-database` por su propio OCID.
    
        # <copy>export BUCKET=DataSafeAuditData</copy>
        # <copy>export COMPARTMENT=compartment-ocid-for-target-database</copy>
        
2.  Asegúrese de que sigue trabajando en el directorio `/usr/lib64/java-oci-sdk/lib/examples`.
    
3.  Ejecute el siguiente comando para ejecutar el archivo de clase. El siguiente ejemplo utiliza `oci-java-sdk-full-2.27.0.jar`, pero asegúrese de utilizar la versión del sistema. Puede ignorar el error sobre el fallo al cargar la clase `org.slf4j.impl.StaticLoggerBinder`.
    
        # <copy>java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-15T19:03:33.225Z
        Querying for auditEvents with timeCollected Start = 2023-02-15T19:03:33.225Z, End = 2023-02-16T19:03:33.223Z
        
        
        Count43
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of auditeventjson2023-02-16T19:03:34.290935835Z _noofrecords_ 43 Start =2023-02-15T19:03:33.225Z, End=2023-02-16T19:03:33.223Z  OpcRequestId: fra-1:RMvrVLBJGMQYyKnOATHwJs6Ywthox3dK9BGYWIaZv3LD2lFq5oRaUuZKzWsJkwZf
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of cursor  OpcRequestId: fra-1:v5KE-S9VbuBMsqnof2qx5dkabTsHgbZv50wSAJJLk-TD-b3e4cqwRmIDG6Bdwa1y
        
4.  Revise la salida. La tercera última línea de salida indica el recuento de registros de auditoría copiados en Object Storage. Su valor puede ser diferente. Si el recuento es igual a cero, suprima los cursores del cubo y repita el paso 3.
    

## Tarea 14: Verifique que los datos de auditoría están en el cubo

1.  En el menú de navegación de Oracle Cloud Infrastructure, seleccione **Storage** y, a continuación, **Buckets**.
    
2.  Seleccione el compartimento.
    
3.  Haga clic en el nombre del cubo.
    
    Aparecerá la página **Detalles de cubo** del cubo.
    
4.  Haga clic en la sección **Objetos**.
    
5.  Observe que ahora tiene una línea de ítem denominada `auditeventjson` que contiene el texto `noofrecords_<some-number>`. Son los datos de auditoría copiados del repositorio de Oracle Data Safe. `<some-number>` es el número de registros de auditoría copiados.
    
    ![Objeto de registros de auditoría inicial](images/initial-audit-records-object.png "Objeto de registros de auditoría inicial")
    

## Tarea 15: Crear un script SH para cronjob

Ahora que ha verificado que el programa Java compilado funciona correctamente, está listo para programarlo mediante cronjob en su instancia informática. El daemon Cron es una utilidad de Linux incorporada que ejecuta procesos en el sistema a una hora programada. Cron lee crontab (tablas cron) para comandos y secuencias de comandos predefinidos. Debe ser el usuario `root` o un usuario con privilegios `sudo` para crear un trabajo cron.

En esta tarea, creará un script SH que contenga las variables y el comando Java para ejecutar el programa Java. En la siguiente tarea, programa el script SH.

1.  Cambie al directorio `/usr/local/bin`.
    
        # <copy>cd /usr/local/bin</copy>
        
2.  Con el editor vi, cree un archivo SH denominado `datasafejob.sh`.
    
        # <copy>vi datasafejob.sh</copy>
        
3.  Agregue el siguiente contenido al archivo SH. Observe que ejecutamos el archivo de clase con un comando Java ligeramente diferente al que utilizamos en la tarea 13. Para ejecutar el archivo de clase desde cualquier lugar, debemos incluir la ruta de acceso al directorio `examples` en la ruta de acceso de clase. De nuevo, estamos utilizando `oci-java-sdk-full-2.27.0.jar`. Asegúrese de utilizar la versión correcta en el sistema. Sustituya `compartment-ocid-for-target-database` por su propio OCID de compartimento.
    
        <copy>#!/bin/bash
        
        export BUCKET=DataSafeAuditData
        
        export COMPARTMENT=compartment-ocid-for-target-database
        
        java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
4.  Guarde y cierre el archivo (pulse **Escape**, introduzca **:wq** y pulse **Intro**).
    
5.  Permite agregar permisos al script.
    
        # <copy>chmod 777 datasafejob.sh</copy>
        

## Tarea 16: Programar el script SH mediante crontab

Comience programando el script SH para que se ejecute cada minuto para que pueda probar que la programación funciona. Después de confirmar, cambie el horario para que sea a las 2 am todos los días.

1.  Para editar el trabajo cron, introduzca el siguiente comando:
    
        # <copy>crontab -e</copy>
        
2.  Agregue lo siguiente a la primera línea y, a continuación, guarde (pulse **Escape**, introduzca **:wq** y pulse **Intro**):
    
        <copy>* * * * * /usr/local/bin/datasafejob.sh</copy>
        
3.  Genere alguna actividad para que Oracle Data Safe la audite. Para ello, acceda a Database Actions para la base de datos de destino. Descargue la secuencia de comandos [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) y ábrala en un editor de texto, como NotePad. Copie todo el script en el portapapeles y péguelo en la hoja de trabajo de Database Actions. En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script) y espere a que el script termine de ejecutarse.
    
4.  Vuelva al cubo y vea los datos de auditoría que se recopilan cada minuto. Los objetos de datos de auditoría pueden tardar hasta diez minutos en mostrarse.
    
    ![Objetos de registros de auditoría](images/audit-records-objects.png "Objetos de registros de auditoría")
    
5.  En Cloud Shell, acceda a crontab.
    
        # <copy>crontab -e</copy>
        
6.  Cambie el programa a las 2 de la mañana todos los días y, a continuación, guarde el archivo (pulse **Escape**, introduzca **:wq** y pulse **Intro**).
    
    En el siguiente ejemplo, `0 2 * * *` indica que el trabajo cron se ejecuta cada vez que el reloj del sistema muestra las 2 a. m.
    
        <copy>0 2 * * * /usr/local/bin/datasafejob.sh</copy>
        

## Tarea 17: Eliminar la actividad programada en crontab

1.  Acceda a crontab.
    
        # <copy>crontab -e</copy>
        
2.  Suprima el contenido.
    
3.  Guarde los cambios (pulse **Escape**, introduzca **:wq** y pulse **Intro**).
    
4.  Cierre Cloud Shell.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Visión general de Auditoría de actividad](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [Pistas de Auditoría](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [Imagen de Oracle Linux Cloud Developer](https://docs.oracle.com/en-us/iaas/oracle-linux/developer/index.htm)
*   [Introducción (con SDK para Java)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (en GitHub)](https://github.com/oracle/oci-java-sdk)
*   [SDK para Java (configuración del SDK)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [API de Data Safe (puntos finales y referencia)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [SDK de Oracle Cloud Infrastructure Java (paquetes y clases)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Contribuyentes**: Richard Evans, Anna Haikl, Russ Lowenthal, Archana Rao, Bettina Schaeumer
*   **Última actualización por/fecha**: Jody Glover, 11 de abril de 2023