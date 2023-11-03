# Oracle Database Vault en una instancia de Autonomous Database

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Database Vault (DV). Ofrece al usuario la oportunidad de aprender a configurar esas funciones en una instancia de Autonomous Database para evitar que usuarios con privilegios no autorizados accedan a datos confidenciales.

Los servicios de bases de datos gestionadas corren el riesgo de "observación de administrador", lo que permite a los usuarios con privilegios, y especialmente a las cuentas de usuarios con privilegios comprometidos, acceder a datos confidenciales. Oracle Autonomous Database con Database Vault proporciona potentes controles de seguridad, restringiendo el acceso a los datos de la aplicación por parte de usuarios de base de datos con privilegios, reduciendo el riesgo de amenazas internas y externas y abordando los requisitos de conformidad comunes.

Puede desplegar controles para bloquear el acceso a cuentas con privilegios a los datos de la aplicación y controlar operaciones confidenciales dentro de la base de datos. Las rutas de confianza se pueden utilizar para agregar controles de seguridad adicionales al acceso a datos autorizado y a los cambios en la base de datos. Las direcciones IP, los nombres de usuario, los nombres de programas de cliente y otros factores se pueden utilizar como parte de los controles de seguridad de Oracle Database Vault para aumentar la seguridad. **Oracle Database Vault protege los entornos de base de datos existentes de forma transparente, eliminando los costosos y laboriosos cambios en las aplicaciones.**

_Tiempo estimado:_ 35 minutos

_Versión probada en este laboratorio:_ Oracle Autonomous Database 19c

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Prevención del acceso no autorizado a datos en bases de datos autónomas con Database Vault (mayo de 2022)_"[](youtube:xKq0a_dwM1Y)

Vea el siguiente vídeo para una breve introducción al laboratorio. [Oracle Database Vault en una instancia de Autonomous Database](videohub:1_hpawkio9)

### Objetivos

El almacén de Oracle Database viene preinstalado con la base de datos autónoma. En este laboratorio:

*   Activación de Database Vault en una instancia de Autonomous Database
*   Protección de datos confidenciales mediante un dominio de Database Vault
*   Crear una política de auditoría para capturar violaciones de dominio
*   Probar controles de Database Vault con modo de simulación

Utilizará el esquema `SH1` que contiene varias tablas, como las tablas `CUSTOMERS` o `COUNTRIES`, que contienen información confidencial y deben protegerse de usuarios con privilegios, como el propietario del esquema (**user `SH1`**) y el DBA (**user `DBA_DEBRA`**). Sin embargo, los datos de estas tablas deben estar disponibles para el usuario de la aplicación (**user `APPUSER`**).

![](./images/adb-dbv_001.png " ")

**Nota:**

*   **En este taller, la sintaxis del comando Configure/Enable/Disable DV solo es para Autonomous Database Shared.**
*   Otros despliegues de Oracle Database, incluidos Autonomous Database Dedicated, Exadata Cloud Service, Database Systems y bases de datos locales, utilizan una sintaxis ligeramente diferente.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha completado anteriormente el paso "Prepare Your Environment"

### Tiempo de laboratorio (estimado)

| N.o de tarea | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Activar Database Vault | <5 minutos |
| 2 | Activar separación de funciones (SoD) | <5 minutos |
| 3 | Creación de un Dominio Simple | 10 minutos |
| 4 | Política de Auditoría para Capturar Violaciones de Dominio | 5 minutos |
| 5 | Modo de simulación | 10 minutos |
| 6 | Deshabilitar Database Vault | <5 minutos |

## Tarea 1: Activación de Database Vault

Comenzamos por crear dos cuentas de usuario DV:

*   **Propietario de Database Vault (`SEC_ADMIN_OWEN`)**
    *   Este usuario es obligatorio como propietario de objetos DV
    *   `SEC_ADMIN_OWEN` tiene los roles `DV_OWNER` y `DV_ADMIN` y puede configurar políticas de almacén de base de datos
*   **Gestor de Cuentas de Database Vault (`ACCTS_ADMIN_ACE`)**
    *   Este usuario es un rol opcional pero recomendado
    *   `ACCTS_ADMIN_ACE` tiene el rol `DV_ACCTMGR` y puede crear usuarios y cambiar contraseñas de usuario
*   Aunque el propietario de DV también puede convertirse en gestor de cuentas de DV, Oracle recomienda mantener la separación de funciones mediante el uso de dos cuentas diferentes

1.  Abra una hoja de trabajo de SQL en la base de datos autónoma como usuario _`ADMIN`_
    
    *   En Oracle Cloud Infrastructure (OCI), seleccione la base de datos "`ADB Security`" creada durante el paso "Preparación del entorno"
        
        ![](./images/adb-dbv_002.png " ")
        
    *   En la página de detalles de la base de datos "`ADB Security`", haga clic en el separador **Herramientas**
        
        ![](../prepare-setup/images/adb-set_010.png " ")
        
    *   La página Herramientas proporciona acceso a herramientas de desarrollador y administración de bases de datos para Autonomous Database: Database Actions, Oracle Application Express, Oracle ML User Administration y controladores de SODA. En el cuadro Database Actions, haga clic en \[**Open Database Actions**\].
        
        ![](../prepare-setup/images/adb-set_011.png " ")
        
    *   Se abre una página de conexión para Database Actions. Para este ejercicio práctico, simplemente utilice la cuenta de administrador por defecto de la instancia de base de datos (usuario _`admin`_) y haga clic en \[**Siguiente**\]
        
        ![](../prepare-setup/images/adb-set_012.png " ")
        
    *   Introduzca la contraseña de administrador que ha especificado al crear la base de datos (aquí _`WElcome_123#`_)
        
            <copy>WElcome_123#</copy>
            
    *   Haga clic en \[**Iniciar sesión**\]
        
        ![](../prepare-setup/images/adb-set_013.png " ")
        
    *   Se abre la página Database Actions. En el cuadro Desarrollo, haga clic en \[**SQL**\].
        
        ![](../prepare-setup/images/adb-set_014.png " ")
        
2.  Crear el propietario de Database Vault y los usuarios del gestor de cuentas
    
        <copy>
        -- Create DV owner
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        
        -- Create DV account manager
        CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;
        
        -- Enable SQL Worksheet for the users just created
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
    
    **Nota:** - Copie/pegue las siguientes consultas SQL en la hoja de trabajo de SQL. Pulse \[**F5**\] o haga clic en el icono "Run Scripts". Compruebe que no haya errores.
    
        ![](./images/adb-dbv_003.png " ")
        
3.  Configurar las cuentas de usuario de Database Vault
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
    ![](./images/adb-dbv_004.png " ")
    
4.  Verifique que Database Vault está configurado pero aún no está activado
    
        <copy>SELECT * FROM DBA_DV_STATUS;</copy>
        
    
    ![](./images/adb-dbv_005.png " ")
    
    **Nota:** `DV_CONFIGURE_STATUS` debe ser **TRUE**
    
5.  Ahora, active Database Vault
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
    
        ![](./images/adb-dbv_006.png " ")
        
6.  Debe "reiniciar" la base de datos para completar el proceso de activación de Database Vault
    
    *   Reinicie la base de datos desde la consola seleccionando "**Reiniciar**" en la lista desplegable "Más acciones", como se muestra
        
        ![](./images/adb-dbv_007.png " ")
        
    *   Una vez finalizado el reinicio, vuelva a la hoja de trabajo de SQL como usuario `ADMIN` y verifique que DV está activado
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_008.png " ")
        
    
    **Nota:** `DV_ENABLE_STATUS` debe ser **TRUE**
    
7.  Ahora, Database Vault está activado.
    

## Tarea 2: Activar separación de funciones (SoD)

En Autonomous DB, el usuario `ADMIN` tiene todos los privilegios, incluidos los necesarios para administrar las políticas de seguridad de Database Vault. En la vida real, puede que desee separar la administración de seguridad, la administración de usuarios y la administración de bases de datos en diferentes cuentas. A partir de ahora utilizaremos una cuenta de DBA en lugar del usuario `ADMIN` porque en producción es lo que debe hacer.

En el paso "Prepare su entorno", ha creado el usuario `DBA_DEBRA`. Este usuario tiene el rol `DBA` en la base de datos autónoma

1.  Para demostrar los efectos del almacén de base de datos SoD en una cuenta de DBA, abra la hoja de trabajo de SQL como usuario _`DBA_DEBRA`_ (como recordatorio, la contraseña es `WElcome_123#`)
    
        <copy>WElcome_123#</copy>
        
2.  Ver los roles de `DBA_DEBRA`
    
        <copy>SELECT * FROM session_roles ORDER BY 1;</copy>
        
    
        ![](./images/adb-dbv_009.png " ")
        
    
    **Nota:** Observe que DBA\_DEBRA tiene varios roles, incluido `PDB_DBA` (el rol de DBA en una base de datos autónoma)
    
3.  Cree un usuario de prueba `DEMO1`
    
        <copy>CREATE USER demo1;</copy>
        
    
        ![](./images/adb-dbv_010.png " ")
        
    
    **Nota:** Observe que `DBA_DEBRA` no puede crear un usuario, a pesar de tener el rol `DBA`, **porque Database Vault está activado**.
    
4.  Intentemos modificar el usuario `APPUSER`
    
        <copy>ALTER USER appuser IDENTIFIED BY WElcome_123456#;</copy>
        
    
        ![](./images/adb-dbv_011.png " ")
        
        **Note:** `DBA_DEBRA` can no longer change a user's passwords!
        
5.  De hecho, **una vez que DV está activado, inmediatamente comienza a forzar la separación de tareas**: el usuario `DBA_DEBRA` pierde su capacidad para crear/modificar/borrar cuentas de usuario de base de datos y ese privilegio tiene el rol de gestor de cuentas de DV.
    
6.  A medida que continúe con el ejercicio práctico, utilizará `SEC_ADMIN_OWEN` y `ACCTS_ADMIN_ACE` para todas las acciones de almacén de base de datos. Las tareas de administración de la base de datos (hechas por `DBA_DEBRA`) ahora están separadas de las tareas de administración de usuarios (`ACCTS_ADMIN_ACE`) y administración de seguridad (`SEC_ADMIN_OWEN`)
    

## Tarea 3: Creación de un Dominio Simple

A continuación, creamos un dominio para proteger la tabla `SH1.CUSTOMERS` del acceso de `DBA_DEBRA` y `SH1` (propietario de la tabla) y otorgar acceso solo a `APPUSER`.

Un dominio es una zona protegida dentro de la base de datos donde se pueden proteger los esquemas, los objetos y los roles de la base de datos. Por ejemplo, puede proteger un juego de esquemas, objetos y roles relacionados con la contabilidad, las ventas o los recursos humanos. Después de protegerlos en un dominio, puede utilizar el dominio para controlar el uso de los privilegios del sistema y de los objetos de cuentas o roles específicos. Esto le permite aplicar controles de acceso sensibles al contexto a cualquier persona que desee utilizar estos esquemas, objetos y roles.

1.  Para demostrar los efectos de este dominio, es importante ejecutar la misma consulta SQL de estos 3 usuarios antes y después de crear el dominio:
    
    *   Para continuar, **abra la hoja de trabajo de SQL en 3 páginas del explorador web** conectada con un usuario diferente (_`DBA_DEBRA`_, _`SH1`_ y _`APPUSER`_), como se muestra en la tarea 1 anteriormente
        
        **Nota:** Tenga en cuenta que solo se puede abrir una sesión de SQL Worksheet en una ventana de explorador estándar al mismo tiempo. Por lo tanto, **abra cada una de sus sesiones en una nueva ventana de explorador web (Mozilla, Chrome, Edge, Safari, etc.) o mediante el "modo de incógnito"**: como recordatorio, la contraseña de estos usuarios es la misma (aquí `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Copiar/pegar y ejecutar la siguiente consulta
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   como usuario "**`DBA_DEBRA`**"
            
            ![](./images/adb-dbv_012.png " ")
            
        *   como usuario "**`SH1`**"
            
            ![](./images/adb-dbv_013.png " ")
            
        *   como usuario "**`APPUSER`**"
            
            ![](./images/adb-dbv_014.png " ")
            
        
        **Nota:** - **Estos 3 usuarios pueden ver la tabla `SH1.CUSTOMERS`** - `SH1` porque `SH1` es el propietario - `DBA_DEBRA` porque tiene el rol de DBA - `APPUSER` porque tiene el privilegio del sistema "`READ ANY TABLE`".
        
2.  Ahora, vamos a crear un dominio para proteger las tablas `SH1` ejecutando esta consulta a continuación como usuario _`SEC_ADMIN_OWEN`_. Por lo tanto, **abra una 4a ventana del explorador web**
    
        <copy>
        -- Create the "PROTECT_SH1" DV realm
           BEGIN
              DVSYS.DBMS_MACADM.CREATE_REALM(
                  realm_name => 'PROTECT_SH1'
                  ,description => 'A mandatory realm to protect SH1 tables'
                  ,enabled => DBMS_MACUTL.G_YES
                  ,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
                  ,realm_type => 1); 
           END;
           /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;
        </copy>
        
    
        ![](./images/adb-dbv_015.png " ")
        
    
    **Nota:** - Ahora el dominio `PROTECT_SH1` se **crea como obligatorio y está activado**. La diferencia entre un **dominio obligatorio y un dominio normal** es que los dominios normales bloquean los privilegios del sistema (y permite los permisos de objeto directo), mientras que los dominios obligatorios bloquean los permisos de objeto directo (incluso por el propietario del objeto) además de los privilegios del sistema
    
3.  Agregue objetos al dominio para protegerlos (aquí, la tabla `CUSTOMERS`)
    
        <copy>
        -- Set SH1 objects as protected by the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   object_owner => 'SH1',
                   object_name  => 'CUSTOMERS',
                   object_type  => 'TABLE');
           END;
           /
        
        -- Show the objects protected by the DV realm PROTECT_SH1
        SELECT realm_name, owner, object_name, object_type
          FROM dvsys.dba_dv_realm_object
         WHERE realm_name IN (SELECT name FROM dvsys.dv$realm WHERE id# >= 5000);
        </copy>
        
    
    ![](./images/adb-dbv_016.png " ")
    
        **Note:** Now the table `CUSTOMERS` is protected and no one can access on it!
        
4.  Comprobar el efecto de este dominio
    
    *   Vuelva a ejecutar la siguiente consulta en SQL Worsheet de cada uno de los 3 usuarios (_`DBA_DEBRA`_, _`SH1`_ y _`APPUSER`_)
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_019.png " ")
        
        - **Objects in the realm cannot be accessed by any database users**, including the DBA (`DBA_DEBRA`) and the schema owner (`SH1`)!
        
5.  Ahora, vuelva a la hoja de trabajo de SQL como usuario _`SEC_ADMIN_OWEN`_ y asegúrese de que tiene un usuario de aplicación autorizado (`APPUSER`) en el dominio ejecutando esta consulta
    
        <copy>
        -- Grant access to APPUSER only for the DV realm "PROTECT_SH1"
           BEGIN
               DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
                   realm_name   => 'PROTECT_SH1',
                   grantee      => 'APPUSER');
           END;
           /
        </copy>
        
    
    ![](./images/adb-dbv_020.png " ")
    
6.  Vuelva a ejecutar la consulta SQL para mostrar que solo `APPUSER` ahora puede leer los datos
    
        <copy>
           SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
             FROM sh1.customers
            WHERE rownum < 10;
        </copy>
        
    
        - as user "**`DBA_DEBRA`**"
        
           ![](./images/adb-dbv_017.png " ")
        
        - as user "**`SH1`**"
        
           ![](./images/adb-dbv_018.png " ")
        
        - as user "**`APPUSER`**"
        
           ![](./images/adb-dbv_014.png " ")
        
        - **`APPUSER` must be the only user who has access to the table from now!**
        

## Tarea 4: Creación de una Política de Auditoría para Capturar Violaciones de Dominio

También puede que desee capturar una pista de auditoría de intentos de acceso no autorizados a los objetos de dominio. Dado que Autonomous Database incluye la auditoría unificada, crearemos una política para auditar las actividades del almacén de base de datos

1.  Abra una hoja de trabajo de SQL como usuario _`ACCTS_ADMIN_ACE`_ (como recordatorio, la contraseña es `WElcome_123#`).
    
        <copy>WElcome_123#</copy>
        
2.  Compruebe que no existe ningún log de pista de auditoría
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_021.png " ")
    
    **Nota:** La consulta no debe devolver ninguna fila.
    
3.  Crear una política de auditoría en el dominio de DV `PROTECT_SH1` creado anteriormente en el paso 2
    
        <copy>
        -- Create the Audit Policy
           CREATE AUDIT POLICY dv_realm_sh1
              ACTIONS SELECT, UPDATE, DELETE
              ACTIONS COMPONENT=DV Realm Violation ON "PROTECT_SH1";
        
        -- Enable the Audit Policy
           AUDIT POLICY dv_realm_sh1;
        </copy>
        
    
    ![](./images/adb-dbv_022.png " ")
    
4.  Al igual que en el paso 2, veamos los efectos de la auditoría
    
    *   Para continuar, **vuelva a ejecutar la misma consulta SQL en 3 hojas de trabajo de SQL diferentes abiertas en 3 ventanas del explorador web** conectadas con un usuario diferente (_`DBA_DEBRA`_, _`SH1`_ y _`APPUSER`_)
        
        **Nota:** Tenga en cuenta que solo se puede abrir una sesión de SQL Worksheet en una ventana de explorador estándar al mismo tiempo. Por lo tanto, **abra cada una de sus sesiones en una nueva ventana de explorador web (Mozilla, Chrome, Edge, Safari, etc.) o mediante el "modo de incógnito"**: como recordatorio, la contraseña de estos usuarios es la misma (aquí `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Copiar/pegar y ejecutar la siguiente consulta
        
            <copy>
               SELECT cust_id, cust_first_name, cust_last_name, cust_email, cust_main_phone_number
                 FROM sh1.customers
                WHERE rownum < 10;
            </copy>
            
        
        *   como usuario "**`DBA_DEBRA`**"
        
        ![](./images/adb-dbv_017.png " ")
        
        *   como usuario "**`SH1`**"
        
        ![](./images/adb-dbv_018.png " ")
        
        *   como usuario "**`APPUSER`**"
        
        ![](./images/adb-dbv_014.png " ")
        
        *   `DBA_DEBRA` **y** `SH1` **los usuarios no pueden acceder a la tabla** `SH1.CUSTOMERS` **y deben generar un registro de auditoría de su intento fallido de infringir la política.**
5.  Vuelva a la hoja de trabajo de SQL como usuario _`ACCTS_ADMIN_ACE`_ para revisar la pista de auditoría de violación de dominio
    
        <copy>
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        </copy>
        
    
    ![](./images/adb-dbv_023.png " ")
    
    **Nota:** Debe ver los intentos fallidos de `DBA_DEBRA` y `SH1`
    
6.  Cuando haya completado esta práctica de laboratorio, inicie sesión como usuario _`SEC_ADMIN_OWEN`_ para restablecer la configuración de auditoría
    
        <copy>
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        -- Purge the DB Vault audit trail logs
        DELETE FROM DVSYS.AUDIT_TRAIL$;
        
        -- Purge the audit trail logs
        BEGIN
            DBMS_AUDIT_MGMT.CLEAN_AUDIT_TRAIL(
               audit_trail_type         =>  DBMS_AUDIT_MGMT.AUDIT_TRAIL_UNIFIED,
               use_last_arch_timestamp  =>  FALSE);
        END;
        /
        
        -- Display the audit trail log
        SELECT os_username, dbusername, event_timestamp, action_name, sql_text 
          FROM unified_audit_trail
         WHERE DV_ACTION_NAME='Realm Violation Audit' order by 3;
        
        -- Disable the audit policy
        NOAUDIT POLICY dv_realm_sh1;
        
        -- Drop the audit policy
        DROP AUDIT POLICY dv_realm_sh1;
        
        </copy>
        
    
    ![](./images/adb-dbv_024.png " ")
    
7.  Ahora, ya no tiene política de auditoría ni dominio DV.
    

## Tarea 5: Modo de simulación

Utilizaremos el modo de simulación para encontrar los factores que se deben utilizar para nuestra conexión de "ruta de confianza" a la tabla `SH1.COUNTRIES`. Para ello, desactivaremos completamente el acceso a la tabla, pero, a continuación, pondremos la política de dominio en modo de simulación. Dado que el modo de simulación no bloqueará los comandos SQL reales, los comandos SQL funcionarán. Sin embargo, si la nueva regla de comando debería haber bloqueado el comando SQL, se creará una entrada en el modo de simulación. A continuación, puede revisar el log de simulación para buscar si ha capturado las violaciones correctas, los factores y las reglas asociadas.

1.  Abra una hoja de trabajo de SQL como usuario _`SEC_ADMIN_OWEN`_ (como recordatorio, la contraseña es `WElcome_123#`).
    
        <copy>WElcome_123#</copy>
        
2.  En primer lugar, consulte el log de simulación para mostrar que no tiene valores actuales
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_025.png " ")
    
3.  A continuación, cree una **regla de comando** que simule el bloqueo de todos los elementos `SELECT` de la tabla `SH1.COUNTRIES`
    
        <copy>
        BEGIN
            DBMS_MACADM.CREATE_COMMAND_RULE(
               command 	 => 'SELECT',
               rule_set_name   => 'Disabled',
               object_name       => 'COUNTRIES',
               object_owner       => 'SH1',
               enabled         => DBMS_MACUTL.G_SIMULATION);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_026.png " ")
    
4.  Como en el Paso 2, veamos ahora los efectos de la simulación
    
    *   Para continuar, **vuelva a ejecutar la misma consulta SQL en 3 hojas de trabajo SQL diferentes abiertas en 3 páginas del explorador web** conectadas con un usuario diferente (_`DBA_DEBRA`_, _`SH1`_ y _`APPUSER`_)
        
        **Nota:** Tenga en cuenta que solo se puede abrir una sesión de SQL Worksheet en una ventana de explorador estándar al mismo tiempo. Por lo tanto, **abra cada una de sus sesiones en una nueva ventana de explorador web (Mozilla, Chrome, Edge, Safari, etc.) o mediante el "modo de incógnito"**: como recordatorio, la contraseña de estos usuarios es la misma (aquí `WElcome_123#`)
        
               ````
               <copy>WElcome_123#</copy>
               ````
            
    *   Copie/pegue y ejecute varias veces la siguiente consulta SELECT en SH1. Tabla COUNTRIES
        
            <copy>
               SELECT * FROM sh1.countries WHERE rownum < 20;
            </copy>
            
        
        *   como usuario "**`DBA_DEBRA`**"
        
        ![](./images/adb-dbv_027.png " ")
        
        *   como usuario "**`SH1`**"
        
        ![](./images/adb-dbv_028.png " ")
        
        *   como usuario "**`APPUSER`**"
        
        ![](./images/adb-dbv_029.png " ")
        
        *   **Todos los usuarios pueden acceder a la** `SH1.CUSTOMERS` **tabla.**
5.  Ahora, vuelva a la hoja de trabajo de SQL como usuario _`SEC_ADMIN_OWEN`_ para ver las nuevas entradas que tenemos. Recuerde que hemos creado una regla de comando para simular el bloqueo de selección de usuario.
    
        <copy>
           SELECT violation_type, username, machine, object_owner, object_name, command, dv$_module
           FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_030.png " ")
    
    **Nota:**
    
    *   Aunque cada usuario puede ver los resultados, el log muestra todos los usuarios que han seleccionado y que la regla habría bloqueado
    *   También muestra desde dónde se conectaron y qué cliente utilizaron para conectarse
6.  Antes de pasar a la siguiente práctica de laboratorio, limpiaremos los logs de simulación y eliminaremos la regla de comando
    
        <copy>
        -- Purge simulation logs
        DELETE FROM DVSYS.SIMULATION_LOG$;
        
        -- Current simulation logs after purging
        SELECT count(*) FROM dba_dv_simulation_log;
        </copy>
        
    
    ![](./images/adb-dbv_031.png " ")
    
        <copy>
        -- Delete the Command Rule
        BEGIN
            DBMS_MACADM.DELETE_COMMAND_RULE(
               command        => 'SELECT', 
               object_owner   => 'SH1', 
               object_name    => 'COUNTRIES',
               scope          => DBMS_MACUTL.G_SCOPE_LOCAL);
        END;
        /
        </copy>
        
    
    ![](./images/adb-dbv_032.png " ")
    

## Tarea 6: Desactivación de Database Vault

1.  Regístrese como usuario _`SEC_ADMIN_OWEN`_ y borre el dominio de DV existente
    
        <copy>
        -- Drop the "PROTECT_SH1" DV realm
        BEGIN
            DVSYS.DBMS_MACADM.DELETE_REALM_CASCADE(realm_name => 'PROTECT_SH1');
        END;
        /
        
        -- Show the current DV realm
        SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 order by 1;
        
        </copy>
        
    
    ![](./images/adb-dbv_060.png " ")
    
2.  Ahora, desactive DB Vault en Autonomous Database
    
        <copy>EXEC DBMS_CLOUD_MACADM.DISABLE_DATABASE_VAULT;</copy>
        
    
    ![](./images/adb-dbv_061.png " ")
    
3.  Debe reiniciar la base de datos para completar el proceso de activación de Database Vault
    
    *   Reinicie la base de datos desde la consola seleccionando "**Reiniciar**" en la lista desplegable "Más acciones", como se muestra
        
        ![](./images/adb-dbv_007.png " ")
        
    *   Una vez completado el reinicio, inicie sesión en la hoja de trabajo de SQL como usuario _`DBA_DEBRA`_ y verifique que DV esté activado
        
            <copy>SELECT * FROM DBA_DV_STATUS;</copy>
            
        
        ![](./images/adb-dbv_062.png " ")
        
    
    **Nota:** `DV_ENABLE_STATUS` debe ser **FALSE**
    
4.  Ahora, borre el propietario de Database Vault y los usuarios del gestor de cuentas
    
        <copy>
        DROP USER sec_admin_owen;
        DROP USER accts_admin_ace;
        </copy>
        
    
        ![](./images/adb-dbv_063.png " ")
        
    
    **Nota:** Puesto que el Vaut de base de datos está desactivado, SoD también se desactiva automáticamente y ahora puede borrar usuarios con el usuario `DBA_DEBRA`
    
5.  Ahora, Database Vault está desactivado correctamente.
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Oracle Database Vault proporciona controles para evitar que los usuarios con privilegios no autorizados accedan a datos confidenciales. También evita cambios no autorizados en la base de datos.

Los controles de seguridad de Oracle Database Vault protegen los datos de las aplicaciones frente a accesos no autorizados y le ayudan a cumplir con los requisitos normativos y de privacidad.

![](./images/dv-concept.png " ")

Puede desplegar controles para bloquear el acceso de cuentas con privilegios a los datos de aplicación y controlar las operaciones delicadas dentro de la base de datos con Database Vault.

Mediante el análisis de privilegios y roles, puede aumentar la seguridad de las aplicaciones existentes mediante el uso de las mejores prácticas de privilegios mínimos.

Oracle Database Vault protege los entornos de base de datos existentes de forma transparente, eliminando cambios en la aplicación costosos que consumen mucho tiempo.

Oracle Database Vault permite crear un juego de componentes para gestionar la seguridad de la instancia de base de datos.

Estos componentes son:

*   **Dominios**

Un dominio es una zona de protección dentro de la base de datos donde se pueden proteger los esquemas, los objetos y los roles de la base de datos. Por ejemplo, puede proteger un juego de esquemas, objetos y roles relacionados con la contabilidad, las ventas o los recursos humanos. Después de protegerlos en un dominio, puede utilizar el dominio para controlar el uso de los privilegios de sistema y de objeto de cuentas o roles específicos. Esto le permite proporcionar controles de acceso contextuales para cualquier persona que desee utilizar estos esquemas, objetos y roles.

*   **Reglas de comando**

Una regla de comando es una política de seguridad especial que puede crear para controlar las condiciones en las que los usuarios pueden ejecutar casi cualquier sentencia SQL, incluidas las sentencias SELECT, ALTER SYSTEM, lenguaje de definición de base de datos (DDL) y lenguaje de manipulación de datos (DML). Las reglas de comando funcionan con juegos de reglas para determinar si se permite la sentencia.

*   **Factores**

Un factor es una variable o atributo con nombre, como una ubicación de usuario, una dirección IP de base de datos o un usuario de sesión, que Oracle Database Vault puede reconocer y utilizar para tomar decisiones de control de acceso. Los factores de las reglas se utilizan para controlar actividades como la autorización de cuentas de base de datos para conectarse a la base de datos o la ejecución de un comando de base de datos específico para restringir la visibilidad y la capacidad de gestión de los datos. Cada factor puede tener una o más identidades. Una identidad es el valor real de un factor. Un factor puede tener varias identidades según el método de recuperación de factores o su lógica de asignación de identidad.

*   **Regla**

La regla de un juego de reglas es una expresión PL/SQL que se evalúa como verdadera o falsa. Puede tener la misma regla en varios juegos de reglas.

*   **Juegos de Reglas**

Un juego de reglas es una recopilación de una o más reglas que puede asociar a una autorización de dominio, regla de comando, asignación de factor o rol de aplicación segura. El conjunto de reglas las evalúa como verdaderas o falsas según la evaluación de cada regla que contiene y el tipo de evaluación (Todas Verdaderas o Alguna Verdadera).

*   **Roles de aplicación segura**

Un rol de aplicación segura de Database Vault es un rol especial de Oracle Database que se puede activar según la evaluación de un juego de reglas de Oracle Database Vault.

Oracle Database Vault proporciona un juego de interfaces y paquetes PL/SQL que le permiten configurar estos componentes. En general, el primer paso es crear un dominio compuesto por los esquemas de base de datos u objetos de base de datos que desea proteger. Puede proteger aún más la base de datos mediante la creación de reglas, conjuntos de reglas, reglas de comandos, factores, identidades y roles de aplicación seguros. Además, puede ejecutar informes sobre las actividades que estos componentes supervisan y protegen.

### **Ventajas del uso de Database Vault**

*   Aborda las normativas de conformidad para minimizar el acceso a los datos
*   Protege los datos frente a cuentas de usuario con privilegios mal utilizadas o comprometidas
*   Diseñar y aplicar políticas de seguridad flexibles para la base de datos
*   Aborda los problemas de consolidación de bases de datos y entornos en la nube para reducir los costos y los datos de aplicaciones sensibles a la exposición de aquellos sin una verdadera necesidad de conocimiento
*   Proteger el acceso a los datos confidenciales mediante la creación de una ruta de confianza (consulte más mediante el [laboratorio de Full Database Vault](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=682&clear=180&session=4531599220675))

## Más información

Documentación técnica:

*   [Oracle Database Vault 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

Vídeo:

*   _Descripción de Oracle Database Vault (marzo de 2019)_[](youtube:oVidZw7yWIQ)
*   _Oracle Database Vault: casos de uso (Part1) (octubre de 2019)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault: casos de uso (Part2) (noviembre de 2019)_[](youtube:hh-cX-ubCkY)
*   _Introducción a Oracle Database Vault (mayo de 2021)_[](youtube:vSVr7avZ4Hg)
*   _Oracle Database Vault en ADB: recorrido rápido por los medios de comunicación_[](youtube:O_Hi2-vZ-zU)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Alan Williams, Rene Fontcha
*   **Última actualización por/fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Mayo de 2022