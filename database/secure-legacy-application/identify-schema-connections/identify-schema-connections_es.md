# Identificar las conexiones al esquema EMPLOYEESEARCH\_PROD

## Introducción

En este laboratorio, exploraremos la creación de un dominio en nuestra instancia de ATP. Mantendremos nuestro dominio en **modo de simulación**. A su vez, esto nos permitirá capturar un registro de errores durante la fase de desarrollo de un dominio o regla de comando. Mediante el modo de simulación, identificaremos nuestras conexiones al esquema `EMPLOYEESEARCH_PROD`.

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Cree un dominio de Database Vault.
*   Utilice el **modo de simulación** para identificar las conexiones al esquema `EMPLOYEESEARCH_PROD` (conexiones de aplicación frente a conexiones que no son de aplicación).

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Finalización de las siguientes prácticas anteriores: configuración de la instancia de Autonomous Database, conexión a la aplicación Glassfish HR heredada, carga y verificación de los datos de la aplicación Glassfish, activación de Database Vault y verificación de la aplicación HR

## Tarea 1: Creación de un dominio de Database Vault

1.  Vuelva a la **hoja de trabajo de SQL de Database Actions**. En la barra de menús de `ADMIN` en la parte superior derecha, seleccione **Cerrar sesión**. Vuelva a conectarse a Database Actions en el usuario `sec_admin_owen` y seleccione **SQL** en **Desarrollo**. Asegúrese de que la hoja de trabajo está limpia y de que el esquema se actualiza de **ADMIN** a `SEC_ADMIN_OWEN`.
    
    ![Cambiar esquema de acciones de base de datos](images/change-schema-dbactions.png)
    
2.  Copie y pegue el siguiente script en la hoja de trabajo de SQL para crear un dominio de Database Vault `PROTECT_MYHRAPP` con el fin de proteger `EMPLOYEESEARCH_PROD`. Seleccione el botón **Run Script** (Ejecutar script) para ejecutar el script. Compruebe la salida del script en la parte inferior para asegurarse de que el script se ha ejecutado correctamente.
    
        	<copy>BEGIN
        			DVSYS.DBMS_MACADM.CREATE_REALM(
        				realm_name => 'PROTECT_MYHRAPP'
        				,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        				,enabled => DBMS_MACUTL.G_SIMULATION
        				,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        				,realm_type => 1); 
        		END;
        		/</copy>
        
    
    ![Crear Dominio de DVD](images/create-realm.png)
    
3.  Borre la hoja de trabajo de SQL. Copie y pegue la siguiente consulta y compruebe la salida para mostrar que se ha creado el dominio de Database Vault actual.
    
        	<copy>SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;</copy>
        
    
    ![Mostrar dominio de DVD](images/show-dv-realm.png)
    
4.  Borre la hoja de trabajo de SQL. Copie y pegue el siguiente script para agregar el esquema `EMPLOYEESEARCH_PROD` y todos sus objetos al dominio de Database Vault `PROTECT_MYHRAPP`. Esto se aplica a los objetos existentes y a los nuevos creados en el esquema `EMPLOYEESEARCH_PROD`. Compruebe la salida para ver que el script se ha ejecutado correctamente.
    
        <copy>BEGIN
           DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
               realm_name   => 'PROTECT_MYHRAPP',
               object_owner => 'EMPLOYEESEARCH_PROD',
               object_name  => '%',
               object_type  => '%');
        END;
        /</copy>
        
    
    En la siguiente tarea, vamos a capturar el acceso a los objetos del esquema `EMPLOYEESEARCH_PROD`, incluido el acceso del propio esquema. Esto nos ayudará a entender qué objetos están disponibles y de dónde se recuperan.
    
5.  Vuelva a la aplicación **HR Production** y desconéctese y, a continuación, vuelva a iniciar sesión como **hradmin**. Compruebe que los datos siguen apareciendo y están disponibles. Haga clic en las fichas del menú de perfil de empleado para asegurarse de que todos los datos están presentes.
    
    ![Buscar empleados](images/search-emp.png)
    
    ![Seleccionar empleados](images/select-emp.png)
    
    ![Navegar por los empleados](images/navigate-emp.png)
    

## Tarea 2: Utilizar el modo de simulación para identificar las conexiones al esquema EMPLOYEESEARCH\_PROD

1.  Como `SEC_ADMIN_OWEN`, copie y pegue la siguiente consulta para ver `dba_dv_simulation_log`. Asegúrese de que `EMPLOYEESEARCH_PROD` es el **propietario del objeto**. Observe cómo el esquema `EMPLOYEESEARCH_PROD` está etiquetado como **Violación de Dominio**. Configuraremos Database Vault para que `EMPLOYEESEARCH_PROD` sea el único esquema que pueda acceder a los datos.
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Mostrar logs de simulación](images/sec-view-log.png)
    
2.  En la barra de menús de `SEC_ADMIN_OWEN` en la parte superior derecha, seleccione **Cerrar sesión**. Vuelva a conectarse a Database Actions en el usuario `dba_debra` y seleccione **SQL** en **Desarrollo**. Asegúrese de que la hoja de trabajo está limpia y de que el esquema se actualiza a `DBA_DEBRA`.
    
    ![Conexión de debra de Dba](images/dba-deb-login.png)
    
3.  Copie y pegue los siguientes comandos para consultar la tabla `DEMO_HR_EMPLOYEES` en el esquema `EMPLOYEESEARCH_PROD`.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Consulta con dba](images/dba-query.png)
    
    Observe cómo el DBA puede consultar los objetos encontrados en el esquema EMPLOYEESEARCH\_PROD, los mismos datos que se muestran en la aplicación HR. Este es un ejemplo de lo que prohibiremos cuando cambiemos el dominio de Database Vault de la simulación al modo de aplicación. Esto también desactivará el acceso para usuarios con privilegios, como `ADMIN`, y usuarios con el rol `PDB_DBA`
    
4.  Desconéctese de `DBA_DEBRA` y vuelva a conectarse a las acciones de la base de datos como `SEC_ADMIN_OWEN`. En **Desarrollar**, seleccione **SQL**. Copie, pegue y ejecute el siguiente comando para consultar la tabla `dba_dv_simulation_log`.
    
        	<copy>select username, violation_type, realm_name, object_owner, object_name, client_ip, dv$_module, dv$_client_identifier from dba_dv_simulation_log;</copy>
        
    
    ![Consultar logs sim](images/query-sim-log.png)
    
    Esta consulta muestra todo el uso reciente y las conexiones a objetos del dominio de almacén de base de datos creado. Observe cómo hay conexiones de usuarios como `DBA_DEBRA, SEC_ADMIN_OWEN, and ADMIN`. Estos usuarios además de `EMPLOYEESEARCH_PROD` estarán prohibidos una vez que proporcionemos autorización de dominio a `EMPLOYEESEARCH_PROD`. Observe también la columna `DV$_MODULE`. Muestra los métodos de conexión a los objetos `EMPLOYEESEARCH_PROD`. La única conexión en la que confiaremos son las conexiones procedentes de `JDBC Thin Client`, que es la conexión de la aplicación HR. Todas las demás conexiones se bloquearán mediante el dominio creado.
    
5.  Borre la tabla `dba_dv_simulation_log` con los siguientes comandos.
    
        	<copy>delete from dvsys.simulation_log$;</copy>
        
    
        	<copy>commit;</copy>
        
6.  Asegúrese de que la hoja de trabajo de SQL está clara. Copie, pegue y ejecute el siguiente bloque PL\*SQL para agregar el dominio `EMPLOYEESEARCH_PROD` como participante autorizado en el dominio `PROTECT_MYHRAPP`. Esto permite al esquema `EMPLOYEESEARCH_PROD` acceder a sus propios objetos sin mostrarse como una violación de dominio en los logs de simulación y garantiza que puede acceder a los objetos cuando cambiamos del modo de simulación al modo de aplicación.
    
        	<copy>
        	begin
        		DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			, grantee => 'EMPLOYEESEARCH_PROD'
        			, rule_set_name => null
        			, auth_options => DBMS_MACUTL.G_REALM_AUTH_OWNER); 
        	end;
        	/</copy>
        
    
    ![Agregar autorización de dominio](images/add-auth.png)
    
7.  Vuelva a la **aplicación de producción de RR. HH.** y desconéctese y vuelva a conectarse como **hradmin**. Eche un vistazo a los datos de los empleados para asegurarse de que siguen presentes y de que la aplicación sigue funcionando correctamente.
    
    ![Buscar empleados](images/search-emp.png)
    
    ![Seleccionar empleados](images/select-emp.png)
    
8.  Abra una copia de seguridad de **Database Actions** en Oracle Cloud. Como `sec_admin_owen`, copie, pegue y ejecute el siguiente comando para consultar la tabla `dba_dv_simulation_log`.
    
        	<copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Buscar empleados](images/log-query.png)
    
    Ahora que hemos agregado la autorización de dominio a `EMPLOYEESEARCH_PROD`. Las conexiones entrantes del **cliente JDBC** o la aplicación HR ya no califican como una violación de dominio porque ahora son un usuario y una conexión de confianza.
    
9.  Reubique la consola de Cloud Shell. Utilice los siguientes comandos para ejecutar la secuencia de comandos `dv_query_employee_search.sh` como **ADMIN**.
    
    _Nota: Si se ha desconectado de la instancia de Glassfish debido a la inactividad, utilice el siguiente comando para volver a conectarse. Puede encontrar la dirección IP pública en el panel de control de detalles de la instancia en Oracle Cloud:_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    Estos son los comandos para ejecutar `dv_query_employee_search.sh`:
    
        	<copy>cd cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![Consultar mediante admin](images/bash-query.png)
    
10.  Minimice Cloud Shell y vuelva al separador **Database Actions** en Oracle Cloud. Como `sec_admin_owen`, copie, pegue y ejecute el siguiente comando para volver a consultar la tabla `dba_dv_simulation_log`.
    
        <copy>select * from dba_dv_simulation_log;</copy>
        
    
    ![Consultar administración posterior](images/post-admin-query.png)
    

Puesto que `ADMIN` ha accedido a los objetos de base de datos `EMPLOYEESEARCH_PROD`, se mostrará como una violación de dominio en el log de simulación de Database Vault. A continuación, moveremos el dominio de **simulación a modo de aplicación** y veremos que los usuarios con privilegios, como `ADMIN` y `DBA_DEBRA`, ya no tendrán privilegios suficientes para consultar objetos `EMPLOYEESEARCH_PROD`

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [Configuración de Dominios de Database Vault](https://docs.oracle.com/database/121/DVADM/cfrealms.htm#DVADM003)
*   [Modo de Simulación de Oracle Databse Vault](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dvadmusing-training-mode-to-log-realm-and-command-rule-activities.html)

## Reconocimientos

*   **Autor**: Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Richard Evans, director sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022