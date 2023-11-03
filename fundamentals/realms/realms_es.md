# Impedir que los usuarios locales bloqueen operaciones comunes: dominios

## Introducción

En este laboratorio se muestra cómo evitar que los usuarios locales creen controles de Oracle Database Vault en objetos de usuarios comunes, lo que evitaría que los usuarios comunes accedan a los datos locales en su propio esquema en PDB. Un propietario de Database Vault local de PDB puede crear un dominio en torno a esquemas comunes de Oracle como `DVSYS` o `CTXSYS` y evitar que funcione correctamente. Para esta práctica, el esquema personalizado `C##TEST1` se crea en la raíz de CDB para mostrar esta función.

Tiempo estimado: 40 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar el entorno

### Requisitos

*   Una cuenta de Oracle
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Configuración 21c

## Tarea 1: Configuración y activación de Database Vault en los niveles de CDB y PDB

1.  Configure y active Database Vault a nivel raíz de CDB y a nivel de PDB. El script crea la tabla `HR.G_EMP` en el contenedor raíz y también la tabla `HR.L_EMP` en `PDB21`.
    
        $ <copy>cd /home/oracle/labs/M104781GC10</copy>
        
        $ <copy>/home/oracle/labs/M104781GC10/setup_DV.sh</copy>
        
        $ ./setup_DV_CDB.sh
        ...
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE OPEN IDENTIFIED BY <i>WElcome123##</i> container=all;
        keystore altered.
        
        ...
        
        SQL> create user c##sec_admin identified by <i>WElcome123##</i> container=ALL;
        User created.
        
        SQL> grant create session, set container, restricted session, DV_OWNER to c##sec_admin container=ALL;
        Grant succeeded.
        
        SQL> drop user c##accts_admin cascade;
        drop user c##accts_admin cascade
                  *
        ERROR at line 1:
        ORA-01918: user 'C##ACCTS_ADMIN' does not exist
        
        SQL> create user c##accts_admin identified by <i>WElcome123##</i> container=ALL;
        User created.
        
        SQL> grant create session, set container, DV_ACCTMGR to c##accts_admin container=ALL;
        Grant succeeded.
        
        SQL> grant select on sys.dba_dv_status to c##accts_admin container=ALL;
        Grant succeeded.
        
        SQL> EXIT
        
        ...
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:26:21 +00:00
        
        SQL> DROP TABLE g_emp;
        Table dropped.
        
        SQL> CREATE TABLE g_emp(name CHAR(10), salary NUMBER) ;
        Table created.
        
        SQL> INSERT INTO g_emp values('EMP_GLOBAL',1000);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:27:54 +00:00
        Connected to:
        
        SQL> DROP TABLE l_emp;
        Table dropped.
        
        SQL> CREATE TABLE l_emp(name CHAR(10), salary NUMBER);
        Table created.
        
        SQL> INSERT INTO l_emp values('EMP_LOCAL',2000);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Last Successful login time: Tue Feb 18 2020 08:27:54 +00:00
        Connected to:
        
        SQL> DROP TABLE l_tab;
        Table dropped.
        
        SQL> CREATE TABLE l_tab(code NUMBER);
        Table created.
        
        SQL> INSERT INTO l_tab values(1);
        1 row created.
        
        SQL> INSERT INTO l_tab values(2);
        1 row created.
        
        SQL> COMMIT;
        Commit complete.
        
        SQL> EXIT
        
        $
        

## Tarea 2: Prueba de la accesibilidad de los datos de la tabla sin dominio en objetos comunes

1.  Conéctese a la raíz de CDB como `C##SEC_ADMIN` para verificar el estado de `DV_ALLOW_COMMON_OPERATION`. Este es el comportamiento por defecto: permite a los usuarios locales crear controles de Database Vault en objetos de usuarios comunes.
    
        $ <copy>sqlplus c##sec_admin</copy>
        
        Enter password: <i>WElcome123##</i>
        
    
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION FALSE
        
        SQL>
        
2.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        

## Tarea 3: Prueba de la accesibilidad de los datos de la tabla con un dominio común normal u obligatorio en objetos comunes

1.  Cree un dominio normal común en las tablas `C##TEST1` de la raíz de CDB.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Root Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Root Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Cree un dominio obligatorio común en las tablas `C##TEST1` de la raíz de CDB.
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Root Test Realm',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
        realm_name   => 'Root Test Realm',
        object_owner => 'C##TEST1',
        object_name  => '%',
        object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
8.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Tarea 4: Prueba de la accesibilidad de los datos de la tabla en objetos comunes con un dominio normal u obligatorio de PDB

1.  Cree un dominio normal de PDB en las tablas `C##TEST1` de `PDB21`.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
6.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Cree un dominio obligatorio de PDB en las tablas `C##TEST1` de `PDB21`.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
8.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
11.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
12.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Tarea 5: Impedir que los usuarios locales creen controles de Oracle Database Vault en objetos comunes

1.  Restrinja a los usuarios locales la creación de controles de Oracle Database Vault en objetos comunes.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION FALSE
        
        SQL> <copy>EXEC DBMS_MACADM.ALLOW_COMMON_OPERATION</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION TRUE
        
        SQL>
        

## Tarea 6: Prueba de la accesibilidad de los datos de la tabla con un dominio común normal u obligatorio en objetos comunes

1.  Cree un dominio normal común en las tablas `C##TEST1` de la raíz de CDB.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Root Test Realm',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Root Test Realm',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
2.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Cree un dominio obligatorio común en las tablas `C##TEST1` de la raíz de CDB.
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Root Test Realm',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 1);
        

FIN; / 2 3 4 5 6 7 8 9

Procedimiento PL/SQL terminado correctamente.

SQL> BEGIN DBMS\_MACADM.ADD\_OBJECT\_TO\_REALM( realm\_name => 'Root Test Realm', object\_owner => 'C##TEST1', object\_name => '%', object\_type => '%'); END; / 2 3 4 5 6 7 8

    PL/SQL procedure successfully completed.
    
    SQL>
    ```
    

8.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Tarea 7: Prueba de la accesibilidad de los datos de la tabla en objetos comunes con un dominio normal u obligatorio de PDB

1.  Cree un dominio normal de PDB en las tablas `C##TEST1` de `PDB21`.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Test Realm1',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 0);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
        realm_name   => 'Test Realm1',
        object_owner => 'C##TEST1',
        object_name  => '%',
        object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        BEGIN
        
        *
        
        ERROR at line 1:
        ORA-47286: cannot add %, C##TEST1.%  to a realm
        ORA-06512: at "DVSYS.DBMS_MACADM", line 1059
        ORA-06512: at line 2
        
        SQL> <copy>!oerr ora 47286</copy>
        47286, 00000, "cannot add %s, %s.%s  to a realm"
        // *Cause: When ALLOW COMMON OPERATION was set to TRUE, a smaller scope user was not allowed to add a larger scope user's object or a larger scope role to a realm.
        // *Action: When ALLOW COMMON OPERATION is TRUE, do not add a larger scope user's object or a larger scope role to a realm.
        SQL>
        
2.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Borre el dominio.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Cree un dominio obligatorio de PDB en las tablas `C##TEST1` de `PDB21`.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
          realm_name    => 'Test Realm1',
          description   => 'Test Realm description',
          enabled       => DBMS_MACUTL.G_YES,
          audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
          realm_type    => 1);
        END;
        /</copy>  2    3    4    5    6    7    8    9
        
        PL/SQL procedure successfully completed.
        
    
        SQL> <copy>BEGIN
        DBMS_MACADM.ADD_OBJECT_TO_REALM(
          realm_name   => 'Test Realm1',
          object_owner => 'C##TEST1',
          object_name  => '%',
          object_type  => '%');
        END;
        /</copy>  2    3    4    5    6    7    8
        
        BEGIN
        
        *
        
        ERROR at line 1:
        
        ORA-47286: cannot add %, C##TEST1.%  to a realm
        
        ORA-06512: at "DVSYS.DBMS_MACADM", line 1059
        
        ORA-06512: at line 2
        
        SQL>
        
8.  Conéctese a la raíz de CDB como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  Conéctese a la raíz de CDB como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  Conéctese a `PDB21` como `C##TEST1`, el propietario común de la tabla.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Conéctese a `PDB21` como `C##TEST2`, otro usuario común.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Borre el dominio.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>EXIT</copy>
        $
        

## Tarea 8: Resumen

Resumamos el comportamiento del acceso a datos en objetos de usuarios comunes en PDB al cambiar el valor `DV_ALLOW_COMMON_OPERATION`.

*   Si crea un dominio normal u obligatorio en la raíz de CDB y un dominio de PDB normal u obligatorio, y si `DV_ALLOW_COMMON_OPERATION` es `TRUE`, se podrá acceder a los datos de objetos de usuarios comunes.
    
*   Si se hubieran creado dominios locales cuando `DV_ALLOW_COMMON_OPERATION` se definiera en `FALSE`, seguirían existiendo después del nuevo control, pero la aplicación se ignoraría.
    

## Tarea 9: Desactivación de Database Vault tanto en la PDB como en la raíz de CDB

1.  Ejecute el script `disable_DV.sh` para desactivar Database Vault tanto en la PDB como en la raíz de la CDB.
    
        $ <copy>/home/oracle/labs/M104781GC10/disable_DV.sh</copy>
        ...
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEY IDENTIFIED BY <i>WElcome123##</i> WITH BACKUP CONTAINER=CURRENT;
        keystore altered.
        
        SQL> exit
        $
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: David Start, Kay Malcolm, Database Product Management
*   **Última actualización por/fecha**: David Start, diciembre de 2020