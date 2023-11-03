# Activar Database Vault y verificar la aplicación HR

## Introducción

En este laboratorio, activaremos Database Vault en nuestra instancia de ATP y reiniciaremos la instancia de ATP para reflejar ese cambio. Luego, echaremos otro vistazo a la aplicación Glassfish y verificaremos que todo sigue funcionando de manera acusada.

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Active Database Vault en la instancia de ATP **(se necesita reiniciar)**.
*   Compruebe que la aplicación HR sigue funcionando.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Finalización de las siguientes prácticas anteriores: configuración de la instancia de Autonomous Database, conexión a la aplicación de recursos humanos Glassfish heredada, carga y verificación de los datos de la aplicación Glassfish

## Tarea 1: Activación de Database Vault en la instancia de ATP

1.  Minimice la consola de Cloud Shell. Mediante el menú de hamburguesa de la parte superior derecha, vaya a **Oracle Database>Autonomous Database**. En el compartimento correcto, seleccione la instancia ATP que ha creado para la aplicación Glassfish.
    
2.  Seleccione **Acciones de base de datos**.
    
    ![Abrir acciones de base de datos](images/database-actions.png)
    
3.  Conéctese a Database Actions con las credenciales **ADMIN** que creó al aprovisionar la instancia de ATP.
    
    ![Acciones de base de datos de conexión](images/db-actions-login.png)
    
4.  En la sección **Development**, seleccione **SQL**.
    
    ![Seleccionar SQL](images/select-sql.png)
    
5.  En el esquema **ADMIN**, copie y pegue los siguientes comandos para crear el **propietario de Database Vault**. Seleccione el botón **Ejecutar Script** para ejecutar las sentencias. Compruebe la **salida de script** en la parte inferior de la página para asegurarse de que las sentencias se han ejecutado correctamente.
    
        <copy>
        CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO sec_admin_owen;
        GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
        GRANT AUDIT_ADMIN to sec_admin_owen;
        </copy>
        
    
    ![Crear propietario de DVD](images/create-dv-owner.png)
    
    ![Comprobar ejecución de dv](images/check-dv-execution.png)
    
6.  Borre la hoja de trabajo. En el esquema **ADMIN**, copie y pegue los siguientes comandos para crear el **gestor de cuentas de Database Vault**. Seleccione el botón **Ejecutar Script** para ejecutar las sentencias. Compruebe la **salida de script** en la parte inferior de la página para asegurarse de que las sentencias se han ejecutado correctamente.
    
        <copy>CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
        GRANT CREATE SESSION TO accts_admin_ace;
        GRANT AUDIT_ADMIN to accts_admin_ace;</copy>
        
7.  Borre la hoja de trabajo. En el esquema **ADMIN**, copie y pegue los siguientes comandos para crear el usuario de DBA, `dba_debra`. Seleccione el botón **Ejecutar Script** para ejecutar las sentencias. Compruebe la **salida de script** en la parte inferior de la página para asegurarse de que las sentencias se han ejecutado correctamente.
    
        <copy>
        CREATE USER dba_debra IDENTIFIED by WElcome_123#;
        GRANT PDB_DBA to dba_debra;
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('dba_debra'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('dba_debra'), p_auto_rest_auth => TRUE);
        END;
        /
        </copy>
        
8.  Active los privilegios de **Hoja de Trabajo de SQL** para los usuarios que se acaban de crear. Borre la hoja de trabajo después de ejecutar cada uno de los siguientes comandos y compruebe que las sentencias se ejecutaron correctamente.
    
        <copy>
        BEGIN
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
           ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('employeesearch_prod'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('employeesearch_prod'), p_auto_rest_auth => TRUE)
        END;
        /
        </copy>
        
9.  Borre la hoja de trabajo. Ahora ejecutará los procedimientos de configuración y activación de Oracle Database Vault. Este paso agrega roles relacionados con DV a la base de datos, otorga a sec\_admin\_owen el rol DV\_OWNER y otorga a accts\_admin\_ace el rol DV\_ACCTMGR. Para obtener más información sobre los cambios que la activación y configuración de Database Vault realiza en la instancia de ADB, consulte la sección **Más información** en la parte inferior de este laboratorio.
    
        <copy>EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');</copy>
        
    
        <copy>EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;</copy>
        
10.  Compruebe que el estado de configuración de Database Vault es **verdadero pero no activado** consultando la tabla `DBA_DV_STATUS`. Asegúrese de que la hoja de trabajo está limpia y, a continuación, copie y pegue la siguiente consulta en la hoja de trabajo. Ejecute el comando y asegúrese de haber recibido una salida adecuada.
    

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

11.  Vuelva a la página **Detalles de Autonomous Database**. En **Más acciones**, seleccione **Reiniciar** (esto puede tardar unos minutos).

![Reiniciar atp](images/restart-atp.png)

12.  Vuelva a la **hoja de trabajo de SQL de Database Actions** y refresque la página. Asegúrese de que la hoja de trabajo está limpia y copie/pegue la consulta siguiente. Compruebe la salida de la secuencia de comandos para asegurarse de que `DV_CONFIGURE_STATUS` y `DV_ENABLE_STATUS` sean ahora **TRUE**.

    <copy>SELECT * FROM DBA_DV_STATUS;</copy>
    

## Tarea 2: Verificar que la aplicación HR sigue funcionando

1.  Con el explorador web, vuelva a la aplicación Glassfish y refresque **las páginas de producción y desarrollo** para verificar que funciona sin problemas.
    
    ![Verificar app](images/front-page-prod.png)
    
    ![Verificar aplicación de desarrollo](images/front-page-dev.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [Página de llegada de Oracle Database Vault](https://www.oracle.com/security/database-security/database-vault/)
*   [Uso de Oracle Database Vault con Autonomous Database](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-database-vault.html)

## Reconocimientos

*   **Autor**: Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Richard Evans, director sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022