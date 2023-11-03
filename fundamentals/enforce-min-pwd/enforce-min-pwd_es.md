# Aplicación de una longitud mínima de contraseña en todas las PDB

## Introducción

En este laboratorio se muestra cómo aplicar a toda la CDB la longitud mínima de contraseña para las cuentas de usuario de base de datos sin restringir el acceso a los perfiles de usuario de base de datos.

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar el entorno

### Requisitos

*   Una cuenta de Oracle
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Configuración 21c

## Tarea 1: Crear un perfil obligatorio en la raíz de CDB

1.  Conéctese a la raíz de CDB en `CDB21`.
    
        $ <copy>sqlplus sys@CDB21 AS SYSDBA</copy>
        
        SQL*Plus: Release 21.0.0.0.0 - Production on Wed Aug 12 09:45:45 2020
        Version 21.1.0.0.0
        Enter password: <i>WElcome123##</i>
        Connected to:
        Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Development
        Version 21.1.0.0.0
        
        SQL>
        
2.  Cree el perfil raíz obligatorio. El perfil raíz obligatorio actúa como un perfil de usuario siempre activo. Los límites de perfil obligatorios se aplican además de los límites existentes del perfil al que está asignado el usuario. Esto crea un efecto de unión en el sentido de que el script de verificación de complejidad de contraseña del perfil obligatorio se ejecutará antes que el script de complejidad de contraseña del perfil de la cuenta de usuario, si lo hay.
    
        SQL> <copy>COL resource_name FORMAT A30</copy>
        
        SQL> <copy>COL limit FORMAT A30</copy>
        
        SQL> <copy>CREATE MANDATORY PROFILE c##prof_min_pass_len
                              LIMIT PASSWORD_VERIFY_FUNCTION ora12c_stig_verify_function
                              CONTAINER=ALL;</copy>
        
        Profile created.
        
        SQL> <copy>SELECT resource_name, limit, mandatory FROM cdb_profiles
                    WHERE profile='C##PROF_MIN_PASS_LEN' AND resource_type='PASSWORD';</copy>
        
        RESOURCE_NAME                  LIMIT                          MAN
        ------------------------------ ------------------------------ ---
        PASSWORD_VERIFY_FUNCTION       FROM ROOT                      YES
        FAILED_LOGIN_ATTEMPTS                                         YES
        PASSWORD_LIFE_TIME                                            YES
        PASSWORD_REUSE_TIME                                           YES
        PASSWORD_REUSE_MAX                                            YES
        PASSWORD_LOCK_TIME                                            YES
        PASSWORD_GRACE_TIME                                           YES
        INACTIVE_ACCOUNT_TIME                                         YES
        PASSWORD_ROLLOVER_TIME                                        YES
        PASSWORD_VERIFY_FUNCTION       ORA12C_STIG_VERIFY_FUNCTION    YES
        FAILED_LOGIN_ATTEMPTS                                         YES
        PASSWORD_LIFE_TIME                                            YES
        PASSWORD_REUSE_TIME                                           YES
        PASSWORD_REUSE_MAX                                            YES
        PASSWORD_LOCK_TIME                                            YES
        PASSWORD_GRACE_TIME                                           YES
        INACTIVE_ACCOUNT_TIME                                         YES
        PASSWORD_ROLLOVER_TIME                                        YES
        
        18 rows selected.
        
        SQL>
        

## Tarea 2: Definición del parámetro de inicialización `MANDATORY_USER_PROFILE`

1.  Definir el parámetro de inicialización
    
        SQL> <copy>ALTER SYSTEM SET mandatory_user_profile=C##PROF_MIN_PASS_LEN;</copy>
        System altered.
        
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string      C##PROF_MIN_PASS_LEN
        SQL>
        
    
    _Se prevé que la función de verificación de contraseña del perfil obligatorio se aplique siempre desde `CDB$ROOT`, lo que significa que el límite de recursos de contraseña siempre se recupera y ejecuta desde `CDB$ROOT` y se aplica en las PDB de toda la CDB según el parámetro de inicialización `MANDATORY_USER_PROFILE`._
    

## Tarea 3: Sustituya la función de verificación de contraseña`n` para aplicar la longitud mínima de contraseña.

1.  Sustituir la función de verificación
    
        SQL> <copy>CREATE OR REPLACE FUNCTION ora12c_stig_verify_function
                  ( username VARCHAR2, password VARCHAR2, old_password VARCHAR2)
                  RETURN BOOLEAN
                  IS
                  BEGIN   
                   -- mandatory verify function will always be evaluated regardless of the  
                   -- password verify function that is associated to a particular profile/user
                   -- requires the minimum password length to be 10 characters
                   IF NOT ora_complexity_check(password, chars => 10) THEN return(false);   
                   END IF;
                   RETURN(true);
                   END;
                   /</copy>
        
        Function created.
        
        SQL>
        

## Tarea 4: Prueba

1.  Cree un nuevo usuario `JOHN` en `PDB21`.
    
        SQL> <copy>CONNECT system@PDB21</copy>
        
        Enter password: <i>Welcome123##</i>
        Connected.
        
    
        SQL> <copy>CREATE USER john IDENTIFIED BY pass;</copy>
        CREATE USER john IDENTIFIED BY pass
        *
        ERROR at line 1:
        ORA-28219: password verification failed for mandatory profile
        ORA-20000: password length less than 10 characters
        
    
        SQL> <copy>CREATE USER john IDENTIFIED BY password123;</copy>
        User created.
        
    
        SQL> <copy>DROP USER john CASCADE;</copy>
        User dropped.
        
        SQL>
        

## Tarea 5: Restablecer la configuración

1.  Borre el perfil obligatorio en la raíz.
    
        SQL> <copy>CONNECT sys@cdb21 AS SYSDBA</copy>
        Connected.
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        DROP PROFILE c##prof_min_pass_len
        *
        ERROR at line 1:
        ORA-02381: cannot drop C##PROF_MIN_PASS_LEN profile
        
    
        SQL> <copy>!oerr ora 2381</copy>
        02381, 00000, "cannot drop %s profile"
        //  *Cause:  An attempt was made to drop PUBLIC_DEFAULT or a mandatory profile,
        //           which is not allowed due to following restrictions:
        //             * PUBLIC_DEFAULT profile can be dropped only when the database
        //               is in migration mode.
        //             * A mandatory profile can be dropped only if it is not set as a
        //               mandatory profile in root container (CDB$ROOT) of a multitenant
        //               container database (CDB) or in a Pluggable Database (PDB).
        //  *Action: If you are trying to drop the PUBLIC_DEFAULT profile, try dropping
        //           it during migration mode. If you are trying to drop a mandatory
        //           profile, check the MANDATORY_USER_PROFILE system parameter setting
        //           in the root container (CDB$ROOT) or in a Pluggable Database (PDB)
        //           and retry the operation after resetting the MANDATORY_USER_PROFILE
        //           system parameter by executing ALTER SYSTEM RESET DDL statement.
        
        SQL>
        
2.  Restablezca primero el parámetro de inicialización `MANDATORY_USER_PROFILE`.
    
        SQL> <copy>ALTER SYSTEM RESET mandatory_user_profile;</copy>
        System altered.
        
    
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string      C##PROF_MIN_PASS_LEN
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        DROP PROFILE c##prof_min_pass_len
        *
        ERROR at line 1:
        ORA-02381: cannot drop C##PROF_MIN_PASS_LEN profile
        
        SQL><copy>exit;</copy>
        
3.  Reinicie la instancia.
    
        <copy>/home/oracle/labs/M104784GC10/wallet.sh</copy>
        
4.  Conéctese a la instancia y elimine el perfil
    
        SQL> <copy>sqlplus sys@cdb21 AS SYSDBA</copy>
        Connected.
        
    
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string
        
    
        SQL> <copy>DROP PROFILE c##prof_min_pass_len;</copy>
        Profile dropped.
        
        SQL> <copy>EXIT</copy>
        
        $
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: David Start, Kay Malcolm, Database Product Management
*   **Última actualización por/fecha**: David Start, diciembre de 2020