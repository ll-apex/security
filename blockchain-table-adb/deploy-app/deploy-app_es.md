# Laboratorio avanzado: instalación de Node.js en la instancia informática y despliegue de la aplicación

## Introducción

En la práctica anterior, generamos las claves SSH necesarias para la instancia informática utilizada para que la aplicación node.js proporcione la firma criptográfica de los datos de usuario fuera de la base de datos.

En este laboratorio, instalaremos Node.js en la instancia informática. A continuación, para prepararse para la firma de datos, generará un par de claves privada/pública y un certificado PKI en la instancia informática, que incluirá su clave pública. Posteriormente, almacenará el certificado en la instancia de base de datos de Oracle Autonomous Transaction Processing y guardará el GUID de certificado que devuelve. Por último, despliegue la aplicación y firme una fila en la base de datos desde Cloud Shell.

Tiempo estimado: 25 minutos

Vea el siguiente video para ver un breve recorrido por el laboratorio.

[](youtube:2QHI3kH8H0o)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Aprovisione una instancia informática de Oracle Linux y SSH en la instancia
*   Generar Certificado y Almacenar Certificado en la Base de Datos
*   Instale Node.js en la instancia informática
*   Abrir firewall para puertos
*   Descargar y desplegar la aplicación
*   Firmar una fila y verificar todas las filas de la tabla de blockchain

### Requisitos

En este taller se asume que tiene:

*   LiveLabs Cuenta en la nube
*   Las prácticas anteriores se han completado correctamente

## Tarea 1: Aprovisionar una instancia informática

1.  En la consola de Oracle Cloud, haga clic en el menú de navegación, busque **Compute** y seleccione **Instances** en Compute.
    
    ![](./images/lab5-task1-1.png " ")
    
2.  Asegúrese de que está en la misma región y compartimento que la instancia de Oracle Autonomous Database y haga clic en **Crear instancia**.
    
    ![](./images/lab5-task1-2.png " ")
    
3.  Asigne un nombre a la instancia. En esta práctica de laboratorio, utilizamos el nombre **DEMOVM**.
    
    ![](./images/lab5-task1-3.png " ")
    
4.  En Colocación, Imagen y forma, elija lo siguiente:
    
    *   **Dominio de disponibilidad**: en esta práctica de laboratorio, deje la instancia por defecto en Colocación siempre gratis elegible o haga clic en **Editar** y seleccione un dominio de disponibilidad (AD).
    *   **Imagen y unidad**: para esta práctica de laboratorio, deje el recurso elegible por defecto - Siempre gratis o puede hacer clic en **Editar** para cambiar la imagen y la unidad.
    
    ![](./images/lab5-task1-4.png " ")
    
5.  En Agregar claves SSH, seleccione **Pegar claves públicas** y pegue la clave pública indicada en la práctica de laboratorio anterior y haga clic en **Crear**.
    
    ![](./images/lab5-task1-5.png " ")
    
6.  La instancia comenzará a aprovisionarse. En unos minutos, el estado pasará de Provisioning a Running. En este punto, su instancia informática está lista para utilizarse. Consulte los detalles de la instancia y copie la **dirección IP pública** para utilizarla más tarde.
    
    ![](./images/lab5-task1-61.png " ") ![](./images/lab5-task1-62.png " ")
    

## Tarea 2: Conexión a la instancia informática

Existen varias formas de conectarse a su instancia en la nube. Elija la forma de conectarse a su instancia en la nube que coincida con la clave SSH que ha generado. \*(es decir, si ha creado sus claves SSH en Cloud Shell, seleccione Cloud Shell)\*

*   Oracle Cloud Shell
*   Emulador MAC o Windows CYGWIN
*   Windows con Putty

### Oracle Cloud Shell

1\. Si se ha desconectado de Cloud Shell o para reiniciar Oracle Cloud Shell, haga clic en el icono de Cloud Shell situado a la derecha de la región de la consola en la nube. \*Nota: Asegúrese de que está en la región que se le ha asignado\*

    ![](./images/lab5-task2-1.png " ")
    

2.  En el siguiente comando, sustituya "sshkeyname" por su nombre de clave ssh real del laboratorio 1 y sustituya "Your Compute Instance Public IP Address" por el que ha copiado en la tarea 1 de este laboratorio. Introduzca el comando editado para conectarse a la instancia.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    _Nota: Los corchetes angulares <> no deben aparecer en el comando._
    
    ![](./images/lab5-task2-4.png " ")
    
3.  Cuando se le solicite, responda **yes** para continuar la conexión.
    
    ![](./images/lab5-task2-5.png " ")
    
4.  Continúe con la siguiente tarea en el menú de la izquierda.
    

### Emulador MAC o Windows CYGWIN

1.  Vaya a **Recursos informáticos**, haga clic en **Instancia** y seleccione la instancia que ha creado (asegúrese de seleccionar el compartimento correcto).
    
2.  En la página inicial de la instancia, busque la dirección IP pública de su instancia.
    
3.  Abra un terminal (MAC) o emulador cygwin como usuario opc. Introduzca Yes cuando se le solicite.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/lab5-cloudshellssh.png " ")
    
    ![](./images/lab5-cloudshelllogin.png " ")
    
    _Nota: Los corchetes angulares <> no deben aparecer en el comando._
    
4.  Después de iniciar sesión correctamente, continúe con la siguiente tarea.
    

### Windows con Putty

1.  Abre masilla y crea una nueva conexión.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/lab5-ssh-first-time.png " ")
    
    _Nota: Los corchetes angulares <> no deben aparecer en el comando._
    
2.  Introduzca un nombre para la sesión y haga clic en **Guardar**.
    
    ![](./images/lab5-putty-setup.png " ")
    
3.  Haga clic en **Conexión** y, a continuación, en **Datos** en el panel de navegación izquierdo y defina el nombre de usuario de conexión automática en root.
    
4.  Haga clic en **Conexión**, **SSH** y, a continuación, en **Autenticación** en el panel de navegación de la izquierda y configure la clave privada SSH que se utilizará haciendo clic en Examinar en el archivo de clave privada para la autenticación.
    
5.  Vaya a la ubicación donde guardó el archivo de clave privada SSH, seleccione el archivo y haga clic en Open. NOTA: No puede conectarse mientras está en una VPN o en la oficina de Oracle en una empresa clara (elija clear-internet).
    
    ![](./images/lab5-putty-auth.png " ")
    
6.  La ruta de acceso del archivo de clave privada SSH se muestra ahora en el archivo de clave privada para el campo de autenticación.
    
7.  Haga clic en Session en el panel de navegación de la izquierda y, a continuación, en Save in the Load, guarde o suprima una sesión almacenada STEP.
    
8.  Haga clic en Open para iniciar la sesión con la instancia. ¡Felicidades! Ahora tiene una instancia de Linux totalmente funcional que se ejecuta en Oracle Cloud Compute.
    

## Tarea 3: Generar certificado

Vamos a generar el par de claves x509. Estamos generando el par de claves y un certificado X509 que se utilizará para la firma de datos más adelante.

1.  Ahora que nos hemos conectado a la instancia informática en Oracle Cloud Shell mediante SSH, descargemos el archivo nodejs.zip.
    
        <copy>
        	cd ~
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/nodejs.zip
        </copy>
        
    
    ![](./images/lab5-task7-2.png " ")
    
2.  descomprimir el archivo nodejs.
    
        <copy>
        unzip nodejs.zip
        </copy>
        
    
    ![](./images/lab5-task7-3.png " ")
    
3.  Navegue hasta la carpeta nodejs.
    
        <copy>
        cd nodejs
        </copy>
        
    
    ![](./images/lab5-task7-4.png " ")
    
4.  Ejecute el comando para generar el par de claves x509: _user01.key_, _user01.pem_ en la carpeta nodejs.
    
    Cuando se le solicite, proporcione los detalles de cada parámetro y pulse Intro: Nombre de país, Estado, Nombre de localidad, Nombre de organización, Nombre común, Dirección de correo electrónico
    
        	<copy>
        	openssl req -x509 -sha256 -nodes -newkey rsa:4096 -keyout user01.key -days 730 -out user01.pem
        	</copy>
        
    
    ![](./images/lab5-task7-5.png " ")
    
5.  Muestre los archivos y observe que se ha creado el par de claves _user01.key_ y _user01.pem_.
    
        <copy>ls</copy>
        
    
    ![](./images/lab5-task7-6.png " ")
    
6.  `cat` la clave _user01.pem_.
    
        	<copy>cat user01.pem</copy>
        
    
    ![](./images/lab5-task7-7.png " ")
    

## Tarea 4: Almacenar Certificado en la Base de Datos

1.  Vaya al separador con SQL Developer Web en el explorador, copie y pegue el siguiente procedimiento en SQL Worksheet. No ejecute el procedimiento todavía.
    
        	<copy>
        	set serveroutput on
        	DECLARE
        		amount NUMBER := 32767;
        		cert_guid RAW(16);
        		cert clob := '-----BEGIN CERTIFICATE----MIIFcjCCA1oCCQC+Rsk9wAYlzDANBgkqhkiG9w0BAQsFADB7MQswCQYDVQQGEwJV
        		………
        		-----END CERTIFICATE-----';
        	BEGIN
        
        		DBMS_USER_CERTS.ADD_CERTIFICATE(
          		utl_raw.cast_to_raw(cert), cert_guid);
        		DBMS_OUTPUT.PUT_LINE('Certificate GUID = ' || cert_guid);
        	END;
        	/
        	</copy>
        
    
    Desde Oracle Cloud Shell en el separador anterior, copie la clave pem y sustituya "-----BEGIN CERTIFICATE----MIIFcjCCA1oCCQC+Rsk9wAYlzDAN.........-----END CERTIFICATE-----" por la clave pem copiada en la hoja de trabajo de SQL Developer Web. Asegúrese de que la clave pem esté dentro de los apóstrofes y ejecute el procedimiento.
    
    El procedimiento debe verse así
    
        	set serveroutput on
        	DECLARE
        		amount NUMBER := 32767;
        		cert_guid RAW(16);
        		cert clob := '-----BEGIN CERTIFICATE-----
        	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
        	-----END CERTIFICATE-----';
        	BEGIN
        		DBMS_USER_CERTS.ADD_CERTIFICATE(
        			utl_raw.cast_to_raw(cert), cert_guid);
        		DBMS_OUTPUT.PUT_LINE('Certificate GUID = ' || cert_guid);
        	END;
        	/
        
    
    ![](./images/lab5-task8-1.png " ")
    
2.  Copie el valor de GUID de certificado. No se volverá a mostrar.
    
    > **Nota:** No vuelva a ejecutar el procedimiento para generar otro GUID de certificado.
    
    Esta salida tiene el siguiente aspecto:
    
        	Certificate GUID = C8D2C1F00236AD7CE0533D11000AE2FC
        
3.  Ejecute este comando para mostrar el certificado.
    
        	<copy>
        	SELECT * FROM USER_CERTIFICATES;
        	</copy>
        
    
    ![](./images/lab5-task8-3.png " ")
    

## Tarea 5: Instalación de Node.js en la instancia informática

Ahora, veamos cómo instalar Node.js en la instancia informática para que la aplicación Node.js interactúe con los puntos finales de descanso de la base de datos autónoma de Oracle.

1.  Vuelva al separador con la consola de Oracle Cloud. Si se desconecta de Cloud Shell, haga clic en el icono de Cloud Shell en la parte superior derecha de la página para iniciar Oracle Cloud Shell y SSH en la instancia con este comando. De lo contrario, puede continuar con el paso.
    
        <copy>
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        </copy>
        
    
    ![](./images/lab5-task7-1.png " ")
    
2.  Nodejs se publica como un módulo AppStream para el repositorio Oracle Linux 8; ol8\_appstream está activado por defecto en Oracle Linux 8 mediante sudo. Esto tomará alrededor de un minuto y dirá "¡Completo!" cuando termine. Ejecute el siguiente comando para activar Nodejs.
    
        <copy>
        sudo dnf config-manager --set-enabled ol8_appstream
        </copy>
        
    
    ![](./images/task1-2.png " ")
    
3.  Ejecute este comando para instalar Node.js para configurar el entorno de tiempo de ejecución. Escriba `y` cuando se le solicite. Después de la instalación, imprimirá "Complete!".
    
        <copy>
        sudo dnf module install nodejs
        </copy>
        
    
    ![](./images/task1-3.png " ")
    

## Tarea 6: Abrir firewall para puertos

Para conectarse a la instancia de Oracle Autonomous Database desde la máquina virtual, necesitamos abrir puertos de firewall. El firewall interno de la instancia informática de linux de Oracle no tiene ningún puerto activado por defecto. Necesitamos habilitar un puerto.

1.  Ejecute este comando sudo para agregar permanentemente el puerto 8080 en la zona pública.
    
        <copy>
        sudo firewall-cmd --permanent --zone=public --add-port=8080-8080/tcp
        </copy>
        
    
    ![](./images/task2-1.png " ")
    
2.  Vuelva a cargar el firewall para asegurarse de que se ha agregado el puerto.
    
        <copy>
        sudo firewall-cmd --reload
        </copy>
        
    
    ![](./images/task2-2.png " ")
    
3.  Muestre todos los puertos para ver si el puerto 8080 está disponible. Si muestra 8080/tcp significa que el firewall de máquina virtual que viene por defecto con Oracle linux ha activado el puerto 8080 en el protocolo TCP.
    
        <copy>
        sudo firewall-cmd --permanent --zone=public --list-ports
        </copy>
        
    
    ![](./images/task2-3.png " ")
    

## Tarea 7: Despliegue de la aplicación

En la máquina virtual de Oracle Linux, puesto que tenemos Node.js instalado y los puertos están activados, descargue y despliegue la aplicación.

1.  Navegue hasta la carpeta nodejs.
    
        <copy>
        cd nodejs
        </copy>
        
    
    ![](./images/task3-1.png " ")
    
2.  Ahora, modifiquemos el archivo index.js con la URL de la instancia de base de datos de Oracle Autonomous Transaction Processing y el GUID de certificado. Copie y pegue el siguiente comando en el bloc de notas, sustituya `<paste your atp url>` en el siguiente comando por la URL de la base de datos de Oracle Autonomous Transaction Processing y pulse `Enter`.
    
        sed -i 's,atp-url,<paste your atp url>,g' index.js
        
    
    El comando debe verse así:
    
        sed -i 's,atp-url,https://fw8mxn5ftposwuj-demoatp.adb.us-ashburn-1.oraclecloudapps.com,g' index.js
        
3.  Copie y pegue el comando modificado en Cloud Shell y pulse `Enter`. Este comando busca la cadena `atp-url` en el archivo index.js y la sustituye por la URL de la base de datos de Oracle Autonomous Transaction Processing.
    
    ![](./images/task3-3.png " ")
    
4.  Copie y pegue el siguiente comando en el bloc de notas. Sustituya `<paste your certificate guid>` en el siguiente comando por el GUID de certificado anotado anteriormente.
    
        sed -i 's,cert-guid,<paste your certificate guid>,g' index.js
        
    
    El comando debe verse así:
    
        sed -i 's,cert-guid,C8D2C1F00236AD7CE0533D11000AE2FC,g' index.js
        
5.  Copie y pegue el comando modificado en Cloud Shell y pulse `Enter`. Este comando busca la cadena `cert-guid` en el archivo index.js y la sustituye por el GUID de certificado.
    
    ![](./images/task3-5.png " ")
    
6.  Duplique el separador actual en el explorador.
    
    ![](./images/task3-6.png " ")
    
7.  Volvamos a conectarnos a la instancia informática. Haga clic en el icono de Cloud Shell en la parte superior derecha de la página para iniciar Oracle Cloud Shell y SSH en la instancia mediante el comando.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/task1-1.png " ")
    
8.  Navegue a la carpeta nodejs y ejecute el comando para desplegar la aplicación. Una vez ejecutado el comando `node bin/www`, la aplicación Node.js se ejecutará y recibirá en el puerto 8080.
    
        <copy>
        cd nodejs
        node bin/www
        </copy>
        
    
    Si el cursor está inactivo, significa que la aplicación nodejs se está ejecutando.
    
    ![](./images/task3-7.png " ")
    

## Tarea 8: Firmar la fila

Vamos a utilizar el comando curl para enviar una solicitud REST a la aplicación node.js que acabamos de iniciar. Esto hará que la aplicación:

*   Recuperar el contenido de la fila mediante los procedimientos DBMS\_BLOCKCHAIN\_TABLE.GET\_BYTES\_FOR\_ROW\_SIGNATURE
*   Llamar al comando openssl para generar una firma con su clave privada
*   Agregue la firma a la fila mediante el procedimiento DBMS\_BLOCKCHAIN\_TABLE.SIGN\_ROW, momento en el que la base de datos utilizará el GUID del certificado para recuperar el certificado, extraer la clave pública y verificar que la firma generada con la clave privada en la aplicación node.js es válida según los datos presentes en la fila y la clave pública en el certificado X.509

Si la firma y los datos se verifican con la clave pública, el campo de firma se almacena en la tabla y la aplicación devuelve el estado 200 y un mensaje de éxito. Si esto falla, devuelve el estado de error 400.

1.  Vuelva a la ventana anterior de Cloud Shell que no tiene la aplicación Node.js en ejecución.
    
    Si se ha desconectado de Cloud Shell, haga clic en el icono de Cloud Shell en la parte superior derecha de la página para iniciar Oracle Cloud Shell, utilizar SSH en la instancia y navegar a la carpeta nodejs. De lo contrario, puede continuar con el paso.
    
        <copy>
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        cd nodejs
        </copy>
        
    
    ![](./images/lab5-task7-1.png " ")
    
2.  Sustituya el número 1 para los valores instanceId, chainId y seqId y actualícelo con los valores instanceId, chainId y seqId indicados en el siguiente comando y pulse Intro.
    
        curl --location --request POST 'http://localhost:8080/transactions/row' --header 'Content-Type: application/json' --data '{"instanceId":1,"chainId":1,"seqId":1}'
        
    
    Después de sustituir los valores instanceId, chainId y seqId en el comando, se debe parecer a lo siguiente:
    
        curl --location --request POST 'http://localhost:8080/transactions/row' --header 'Content-Type: application/json' --data '{"instanceId":1,"chainId":5,"seqId":1}'
        
3.  Observe que el mensaje JSON con estado 200 y el mensaje que se muestra es `Signature has been added to the row successfully`, lo que significa que la fila se ha firmado correctamente.
    
    ![](./images/task4-3.png " ")
    
4.  Para verificarlo, vuelva al separador con la aplicación Blockchain APEX con la lista de transacciones y refresque el separador. Observe que la fila con los valores ID de instancia - , ID de cadena - e ID de secuencia - `IS Signed` debe mostrar una marca verde que indique que la fila se ha firmado correctamente.
    
    ![](./images/task4-4.png " ")
    
5.  Vuelva a SQL Developer Web y consulte todas las columnas de la tabla `bank_ledger` para ver los valores reales en las columnas de firma y GUID.
    
    Desplácese a la salida de la derecha y observe los valores de las columnas orabctab\_signature$, \_signature\_alg$ y \_signature\_cert$.
    
        	<copy>
        	select bank, deposit_date, deposit_amount, ORABCTAB_INST_ID$,
        	ORABCTAB_CHAIN_ID$, ORABCTAB_SEQ_NUM$,
        	ORABCTAB_CREATION_TIME$, ORABCTAB_USER_NUMBER$,
        	ORABCTAB_HASH$, ORABCTAB_SIGNATURE$, ORABCTAB_SIGNATURE_ALG$,
        	ORABCTAB_SIGNATURE_CERT$ from bank_ledger;
        	</copy>
        
    
    ![](./images/task4-5.png " ")
    

## Tarea 9: Verificación de filas con firma

1.  En la hoja de trabajo de SQL, copie ane pegue el siguiente procedimiento para verificar las filas de la tabla de blockchain mediante DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS.
    
    > _Nota: Se espera que cada tabla de blockchain tenga diferentes valores de ID de instancia. No se preocupe si no ve los mismos valores en la salida que en la captura de pantalla. Si el procedimiento PL/SQL se completa correctamente, la tabla de blockchain se verifica correctamente._
    
        	<copy>
        	DECLARE
        		verify_rows NUMBER;
        		instance_id NUMBER;
        	BEGIN
        		FOR instance_id IN 1 .. 4 LOOP
        			DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS('DEMOUSER','BANK_LEDGER',
        	NULL, NULL, instance_id, NULL, verify_rows);
        		DBMS_OUTPUT.PUT_LINE('Number of rows verified in instance Id '||
        	instance_id || ' = '|| verify_rows);
        		END LOOP;
        	END;
        	/
        	</copy>
        
    
    ![](./images/lab5-task6-1.png " ")
    

## Reconocimientos

*   **Autor**: Mark Rakhmilevich, Anoosha Pilli
*   **Contribuyentes**: Anoosha Pilli, Salim Hlayel, Brianna Ambler, directora de productos, Oracle Database
*   **Última actualización por/fecha**: Marion Smith, abril de 2022