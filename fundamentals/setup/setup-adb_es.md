# Configurar

## Introducción

En la práctica anterior, creó una instancia de ADB. En este laboratorio, se conectará a la instancia de ADB desde Oracle Cloud Shell.

Tiempo estimado: 20 minutos

### Objetivos

*   Crear un cubo, un token de autenticación y Oracle Wallet
*   Cargar instancia de ADB
*   Asignación de Roles y Privilegios a Usuarios
*   Crear una credencial de base de datos para los usuarios

### Requisitos

*   Laboratorio: aprovisionamiento de ADB

## Tarea 1: Creación de un cubo

1.  Conéctese a Oracle Cloud si aún no se ha conectado.
    
2.  Haga clic en el menú de hamburguesa, vaya a **Almacenamiento** y haga clic en **Cubos**.
    
    ![](./images/object_storage.png " ")
    
3.  Seleccione el compartimento en el que se aprovisiona el ATP y haga clic en **Crear cubo**.
    
    ![](./images/step1-3.png " ")
    
4.  Asigne un nombre al cubo **adb1** y haga clic en **Crear**.
    
    ![](./images/step1-4.png " ")
    
5.  Una vez creado el cubo, haga clic en el cubo y anote `bucket name` y `namespace`.
    
    ![](./images/step1-5.png " ")
    

## Tarea 2: Creación de Oracle Wallet en Cloud Shell

Hay varias formas de crear Oracle Wallet para ADB. Utilizaremos Oracle Cloud Shell, ya que este no es el centro de este taller. Para obtener más información sobre las carteras de Oracle y utilizar la interfaz para crear una, consulte el laboratorio de este taller: [Análisis de los datos con ADB - Laboratorio 6](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?p180_id=553)

1.  Conéctese a Oracle Cloud si aún no está conectado.
    
2.  Haga clic en el icono de Cloud Shell para iniciar Cloud Shell ![](./images/cloud-shell.png " ")
    
3.  Mientras se inicia Cloud Shell, haga clic en el menú de hamburguesa -> **Autonomous Transaction Processing** ![](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")
    
4.  Haga clic en el **Nombre mostrado** para ir a la página principal de ADB.
    
    ![](./images/step2-4.png " ")
    
5.  Busque y copie el **OCID** (ID de Oracle Cloud) que necesitará en unos minutos.
    
    ![](./images/locate-ocid.png " ")
    
6.  Utilice autonomous\_database\_ocid para crear Oracle Wallet. Definirá la contraseña de cartera con el mismo valor que la contraseña de administrador de ADB para facilitar su uso: _WElcome123##_ Nota: Esta no es una práctica recomendada y se utiliza solo para este ejercicio práctico.
    
7.  Copie el siguiente comando y péguelo en Cloud Shell. No pulse Intro todavía.
    
        <copy>
        cd ~
        oci db autonomous-database generate-wallet --password WElcome123## --file 21c-wallet.zip --autonomous-database-id  </copy> ocid1.autonomousdatabase.oc1.iad.xxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/wallet.png " ")
    
8.  Pulse Copiar para copiar el OCID del paso 5 y rellenar el OCID de la base de datos autónoma que aparece en la sección de salida de su terraform. Asegúrese de que haya un espacio entre la frase --autonomous-database-id y el ocid. Haga clic en **enter**. Ten paciencia, tarda unos 20 segundos.
    
9.  El archivo de cartera se descargará en el sistema de archivos de Cloud Shell en /home/yourtenancyname
    
10.  Introduzca el comando list en su shell en la nube a continuación para verificar que se haya creado _21c-wallet.zip_
    
        ls
        
    
    ![](./images/21cwallet.png " ")
    

## Tarea 3: Creación de token de autenticación

1.  Haga clic en el icono de persona en la esquina superior derecha.
    
2.  Seleccione **Configuración de usuario**
    
    ![](./images/select-user.png " ")
    
3.  Copie el **nombre de usuario**.
    
    ![](./images/copy-username.png " ")
    
4.  En el separador **Información de usuario**, haga clic en el botón **Copiar** para copiar el **OCID** de usuario.
    
    ![](./images/copy-user-ocid.png " ")
    
5.  Cree el token de autenticación con la descripción `adb1` mediante el siguiente comando sustituyendo el _OCID de usuario_ real por el ID de usuario siguiente. _Nota: Si ya tiene un token de autenticación, puede que aparezca un error si intenta crear más de 2 por usuario_
    
        <copy>
         oci iam auth-token create --description adb1 --user-id </copy> ocid1.user.oc1..axxxxxxxxxxxxxxxxxxxxxx
        
    
    ![](./images/token.png " ")
    
6.  Identifique la línea en la salida que comienza con **"token"**.
    
7.  Copie el valor del **token** en algún lugar seguro, lo necesitará en los siguientes pasos.
    

## Tarea 4: Carga de una Instancia de ADB con Esquemas de Aplicación

1.  Vuelva a Cloud Shell e inicie Cloud Shell si aún no se está ejecutando
    
2.  Ejecute el comando wget para descargar el script load\_21c.sh del almacenamiento de objetos.
    
        <copy>
        cd $HOME
        pwd
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-21c.sh
        chmod +x load-21c.sh
        export PATH=$PATH:/usr/lib/oracle/19.10/client64/bin
        </copy>
        
3.  Ejecute el script de carga transfiriendo los dos argumentos desde el bloc de notas, la contraseña de administrador y el nombre de la instancia de ATP. Este script importará todos los datos de la instancia de ATP de la aplicación y configurará SQL Developer Web para cada esquema. Este script se ejecuta como usuario opc. El nombre de ATP debe ser el nombre de la instancia de ADB. En el siguiente ejemplo, utilizamos _adb1_. Esta secuencia de comandos de carga tarda aproximadamente 3 minutos en ejecutarse. _Nota: Si utiliza un nombre de ADB diferente, sustituya adb1 por el nombre de la instancia de adb_
    
        <copy> 
        ./load-21c.sh WElcome123## adb1 2>&1 > load-21c.out</copy>
        
    
    ![](./images/load21c-1.png " ")
    

## Tarea 5: Asignación de Roles y Privilegios a Usuarios

1.  Vuelva a la página inicial de Autonomous Database.
    
    ![](./images/step4-0.png " ")
    
    ![](./images/step4-1.png " ")
    
2.  Haga clic en el separador **Herramientas**.
    
    ![](./images/step4-tools.png " ")
    
3.  Haga clic en **Database Actions**.
    
    ![](./images/step4-database.png " ")
    
4.  Seleccione **admin** para su nombre de usuario.
    
    ![](./images/step4-admin.png " ")
    
5.  Contraseña: **WElcome123##**.
    
    ![](./images/step4-password.png " ")
    
6.  En Administración, seleccione **Usuarios de la base de datos**.
    
    ![](./images/step4-databaseuser.png " ")
    
7.  Para el usuario **RR. HH.**, haga clic en **3 puntos** para ampliar el menú y seleccione **Editar**.
    
    ![](./images/step4-edit.png " ")
    
8.  Active los controles deslizantes **Activación de REST** y **Autorización necesaria**.
    
    ![](./images/step4-enable-rest.png " ")
    
9.  Haga clic en el separador **Roles otorgados** en la parte superior.
    
    ![](./images/step4-roles.png " ")
    
10.  Desplácese hacia abajo en **DWROLE** y asegúrese de que las casillas de control **1st** y **3rd** estén activadas.
    
    ![](./images/step4-dwrole.png " ")
    
11.  Desplácese hasta la parte inferior y haga clic en **Apply Changes** (Aplicar cambios).
    
    ![](./images/step4-apply.png " ")
    
12.  Haga clic en la **X** en la barra de búsqueda para volver a ver todos los usuarios.
    
    ![](./images/step4-cancel-search.png " ")
    
13.  Repita los pasos del 7 al 12 para el usuario **OE**.
    
    ![](./images/step5-13a.png " ")
    
    ![](./images/step5-13b.png " ")
    
    ![](./images/step5-13c.png " ")
    
    ![](./images/step5-13d.png " ")
    
    ![](./images/step5-13e.png " ")
    
14.  Repita los pasos 7 a 12 para el usuario **REPORT**.
    
    ![](./images/step5-14a.png " ")
    
    ![](./images/step5-14b.png " ")
    
    ![](./images/step5-14c.png " ")
    
    ![](./images/step5-14d.png " ")
    
    ![](./images/step5-14e.png " ")
    

## Tarea 6: Conexión a SQL Developer Web

1.  Realice una prueba para asegurarse de que los datos se han cargado mediante el registro en SQL Developer Web.
    
2.  En la parte superior izquierda, seleccione el **botón de hamburguesa** y expanda el separador **Desarrollo**. Seleccione **SQL**.
    
    ![](./images/step4-sql.png " ")
    
3.  Haga clic en la **X** para descartar la ventana emergente.
    
    ![](./images/step4-sql-x.png " ")
    
4.  Ejecute el fragmento de código que aparece a continuación y verifique que hay 665 elementos.
    
        <copy>
        select count(*) from oe.order_items;
        </copy>
        
    
    ![](./images/step4-run.png " ")
    

## Tarea 7: Creación de una credencial de base de datos para los usuarios

Para acceder a los datos del almacén de objetos, debe permitir que el usuario de la base de datos se autentique con el almacén de objetos mediante la cuenta del almacén de objetos de OCI y el token de autenticación. Para ello, cree un objeto CREDENTIAL privado para el usuario que almacene esta información cifrada en Autonomous Transaction Processing. Esta información solo se puede utilizar para el esquema de usuario.

1.  Copie y pegue este fragmento de código en la hoja de trabajo de SQL Developer. Especifique las credenciales para el servicio Oracle Cloud Infrastructure Object Storage sustituyendo `<username>` y `<token>` por el siguiente nombre de usuario y contraseña:
    
    *   Nombre de credencial: descripción del token de autenticación. En este ejemplo, el token de autenticación se crea con la descripción `adb1` del paso 1.
    *   Nombre de usuario: el nombre de usuario será el **nombre de usuario de OCI** que anotó en el paso 3
    *   Contraseña: la contraseña será el **Token** de autenticación del almacén de objetos de OCI que ha generado en el paso 3.
    
        	<copy>
        	BEGIN
        		DBMS_CLOUD.CREATE_CREDENTIAL(
        		credential_name => 'adb1',
        		username => '<username>',
        		password => '<token>'
        		);
        	END;
        	/
        	</copy>
        
    
    ![](./images/step7-1.png " ")
    
    Ahora está listo para cargar datos del almacén de objetos.
    
2.  Haga clic en la flecha hacia abajo junto a la palabra **ADMIN** y **Sign Out**.
    
    ![](./images/step4-signout.png " ")
    

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autoras**: Kay Malcolm, directora sénior de gestión de productos de base de datos
*   **Contribuyentes**: Anoosha Pilli, Didi Han, Database Product Management
*   **Última actualización por/fecha**: Didi Han, abril de 2021