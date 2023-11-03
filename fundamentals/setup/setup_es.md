# Configurar entorno 21C

## Introducción

En este laboratorio, ejecutará los scripts para configurar el entorno para el taller de Oracle Database 21c.

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Definir y probar las conexiones
*   Descargar scripts necesarios para ejecutar laboratorios 21c
*   Actualizar scripts con la contraseña seleccionada

### Requisitos

*   Una cuenta de Oracle
*   Conocimientos prácticos de vi o nano
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Dirección Pública de DBCS
*   Nombre Único de la Base de Datos

## Tarea 1: Configuración del servidor

1.  Si aún no está conectado, conéctese a Oracle Cloud y reinicie Oracle Cloud Shell. De lo contrario, vaya al paso 3.
    
2.  En Cloud Shell o en la ventana de terminal, navegue hasta la carpeta en la que creó las claves SSH e introduzca este comando mediante su dirección IP:
    
        $ <copy>ssh -i <<sshkeyname>> opc@</copy>< your_IP_address >
        Enter passphrase for key './myOracleCloudKey':
        Last login: Tue Feb  4 15:21:57 2020 from < your_IP_address >
        [opc@tmdb1 ~]$
        
3.  Tendrá que crear los directorios para la base de datos de contenedores y las bases de datos de conexión.
    
        <copy>
        sudo mkdir /u02/app/oracle/oradata/CDB21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/CDB21
        sudo chmod 755 /u02/app/oracle/oradata/CDB21
        sudo mkdir /u02/app/oracle/oradata/pdb21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/pdb21
        sudo chmod 755 /u02/app/oracle/oradata/pdb21
        sudo mkdir /u02/app/oracle/oradata/PDB21
        sudo chown oracle:oinstall /u02/app/oracle/oradata/PDB21
        sudo chmod 755 /u02/app/oracle/oradata/PDB21
        sudo mkdir /u02/app/oracle/oradata/toys_root
        sudo chown oracle:oinstall /u02/app/oracle/oradata/toys_root
        sudo chmod 755 /u02/app/oracle/oradata/toys_root
        sudo mkdir /u03/app/oracle/fast_recovery_area
        sudo chown oracle:oinstall /u03/app/oracle/fast_recovery_area
        sudo chmod 755 /u03/app/oracle/fast_recovery_area
        </copy>
        
4.  Una vez conectado, puede cambiar al usuario "oracle".
    
        [opc@tmdb1 ~]$ <copy>sudo su - oracle</copy>
        
5.  El primer paso es obtener todos los scripts necesarios para realizar esta práctica.
    
        <copy>
        cd /home/oracle
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/Cloud_21c_Labs.zip
        </copy>
        
6.  Descomprima Cloud\_21c\_Labs.zip
    
        <copy>
        unzip Cloud_21c_Labs.zip
        </copy>
        
7.  Haga que todos puedan leer, escribir y ejecutar el script.
    
        <copy>
        chmod 777 /home/oracle/labs/update_pass.sh
        </copy>
        
8.  Ejecute el script de shell /home/oracle/labs/update\_pass.sh. El script de shell le solicita que introduzca password\_defined\_during\_DBSystem\_creation y lo define en todos los scripts de shell y scripts SQL que se utilizarán en las prácticas.
    
        [oracle@da3 ~]$ <copy>/home/oracle/labs/update_pass.sh</copy>
        Enter the password you set during the DBSystem creation: WElcome123##
        

## Tarea 2: Creación de base de datos

1.  Ahora, para crear la base de datos `CDB21`. Hay una base de datos existente en este servidor, pero `CDB21` se ha creado con todo lo necesario para este taller.
    
        <copy>
        cd /home/oracle/labs/M104784GC10
        /home/oracle/labs/M104784GC10/create_CDB21.sh
        </copy>
        
2.  Defina el entorno. En la petición de datos, escriba `CDB21`
    
        [oracle@db1 ~]$ <copy>. oraenv</copy>
        ORACLE_SID = [CDB21] ? CDB21
        The Oracle base remains unchanged with value /u01/app/oracle
        [oracle@db1 ~]$
        
3.  Verifique que Oracle Database 21c `CDB21` y `PDB21` se crean mediante los siguientes comandos.
    
        <copy>
        ps -ef|grep smon
        sqlplus / as sysdba
        </copy>
        
    
        <copy>
        show pdbs
        exit;
        </copy>
        
4.  Asegúrese de que el alias de TNS se ha creado para `CDB21`, `PDB21` y `PDB21_2` en el archivo tnsnames.ora. Si no están allí, tendrás que agregarlos. El archivo se encuentra en `/u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora`.
    
        	  <copy>
        	  cat /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
5.  A continuación, se debe haber creado la entrada para `CDB21` al crear la base de datos. Por lo tanto, para `PDB21`, copie la entrada para `CDB21` y cambie el valor `CDB21` a `PDB21` en ambos lugares. Repita esto para `PDB21_2`.
    
        	  <copy>
        	  vi /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
6.  Habrá más en su tnsnames.ora, pero las tres entradas que nos importan deberían parecerse a las entradas siguientes, pero con su nombre de host en su lugar.
    
        CDB21 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = CDB21)
         )
        )
        
        PDB21 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = PDB21)
         )
        )
        
        PDB21_2 =
        (DESCRIPTION =
         (ADDRESS = (PROTOCOL = TCP)(HOST = db1.subnet11241424.vcn11241424.oraclevcn.com)(PORT = 1521))
         (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = PDB21_2)
         )
        )
        
7.  Pruebe la conexión a CDB21. Conéctese a CDB21 con SQL\*Plus mediante la contraseña **WElcome123##**.
    
        	  <copy>
        	  sqlplus sys@cdb21 AS SYSDBA
        	  </copy>
        
8.  Verifique que el nombre del contenedor sea **CDB$ROOT**.
    
        <copy>
        	  SHOW CON_NAME;
        	  </copy>
        
9.  Pruebe la conexión a PDB21 con la misma contraseña utilizada en el paso anterior.
    
        <copy>
        CONNECT sys@PDB21 AS SYSDBA
        </copy>
        
10.  Muestre el nombre del contenedor. Ahora debe mostrar **PDB21**.
    

    ```
    <copy>
    SHOW CON_NAME;
    </copy>
    ```
    

11.  Salga de SQL\*Plus.
    
        <copy>
        exit
        </copy>
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: David Start, Kay Malcolm, Database Product Management
*   **Fecha y fecha de última actualización**: Arabella Yao, mánager de productos, Database Product Management, diciembre de 2021