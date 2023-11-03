# Explore las funciones de la aplicación Glassfish HR con Database Vault activado

## Introducción

En este laboratorio, ahora cambiaremos nuestro dominio de Database Vault del modo **simulación** al modo **aplicación**. Esto comprometerá nuestro dominio a la producción. A continuación, pondremos a prueba nuestro dominio y demostraremos la incapacidad de consultar los datos de la aplicación bajo un usuario concreto.

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Voltear el dominio de Database Vault del modo **simulación** al modo **aplicación**
*   Demostrar la incapacidad de consultar los datos de `EMPLOYEESEARCH_PROD` con `DBA_DEBRA` y `ADMIN`.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Finalización de las siguientes prácticas anteriores: configuración de la instancia de Autonomous Database, conexión a la aplicación Glassfish HR heredada, carga y verificación de los datos de la aplicación Glassfish, activación de Database Vault y verificación de la aplicación HR, identificación de las conexiones al esquema EMPLOYEESEARCH\_PROD

## Tarea 1: Cambio del dominio de Database Vault de la simulación al modo de aplicación

1.  En **Database Actions**, como `sec_admin_owen`, ejecute la siguiente consulta en la tabla `dvsys.dba_dv_realm_auth`.
    
        	<copy>select grantee from dvsys.dba_dv_realm_auth where realm_name = 'PROTECT_MYHRAPP';</copy>
        
    
    ![Consultar dbadv](images/final-verification.png)
    
    Esto muestra una verificación final de que el dominio protege `EMPLOYEESEARCH_PROD` y que `EMPLOYEESEARCH_PROD` está autorizado como **propietario del dominio** sin ningún juego de reglas (nulo).
    
2.  Como `sec_admin_owen`, copie y pegue el siguiente comando para cambiar el dominio de Database Vault del modo **simulación** al modo **aplicación**. Compruebe la salida para ver que el procedimiento se ha completado correctamente.
    
        	<copy>BEGIN
        		DVSYS.DBMS_MACADM.UPDATE_REALM(
        			realm_name => 'PROTECT_MYHRAPP'
        			,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
        			,enabled => DBMS_MACUTL.G_YES
        			,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
        			,realm_type => 1); 
        	END;
        	/</copy>
        
    
    ![Modo de aplicación](images/enforcement-mode.png)
    

_Nota_: La actualización puede tardar unos minutos en aplicarse.

## Tarea 2: Demostrar la incapacidad de consultar los datos de `EMPLOYEESEARCH_PROD` con DBA\_DEBRA y ADMIN.

1.  Desconéctese de las acciones de la base de datos y vuelva a conectarse como `DBA_DEBRA`. Copie y pegue los siguientes comandos para consultar objetos en `EMPLOYEESEARCH_PROD`.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Debra privilegios insuficientes](images/debra-insufficient.png)
    
    Observe que ahora `DBA_DEBRA` ya no tiene autorización para consultar objetos de base de datos protegidos por el dominio y propiedad de `EMPLOYEESEARCH_PROD`.
    
2.  Abra Cloud Shell. Utilice los siguientes comandos para ejecutar el script `dv_query_employee_search.sh` como `ADMIN`.
    
    _Nota: Si se ha desconectado de la instancia de Glassfish debido a la inactividad, utilice el siguiente comando para volver a conectarse. Puede encontrar la dirección IP pública en el panel de control de detalles de la instancia en Oracle Cloud:_
    
        	<copy>chmod 600 myhrappkey</copy>
        
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    _Estos son los comandos para ejecutar dv\_query\_employee\_search.sh:_
    
        	<copy>cd secure-legacy-app-db-vault</copy>
        
    
        	<copy>./dv_query_employee_search.sh</copy>
        
    
    ![Los privilegios de administrador no son suficientes](images/admin-insufficient.png)
    
    Observe cómo `ADMIN` ya no tiene autorización para consultar objetos de base de datos protegidos por el dominio y propiedad de `EMPLOYEESEARCH_PROD`.
    
3.  Minimice Cloud Shell. Localice **Database Actions** y desconéctese de `DBA_DEBRA`. Vuelva a conectarse como `ADMIN` y, a continuación, copie y pegue los siguientes comandos para consultar objetos en `EMPLOYEESEARCH_PROD`.
    
        	<copy>alter session set current_schema = EMPLOYEESEARCH_PROD;</copy>
        
    
        	<copy>select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME</copy>
        
    
    ![Acciones de base de datos de administración](images/admin-db-actions.png)
    
    `ADMIN` ahora también tiene la incapacidad de consultar objetos protegidos por `EMPLOYEESEARCH_PROD` en **Database Actions**.
    

¡Felicidades! Ahora ha trasladado y protegido correctamente una aplicación de RR. HH. heredada en Oracle Cloud.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Richard Evans, director sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022