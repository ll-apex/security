# Mindestkennwortlänge in allen PDBs durchsetzen

## Einführung

In dieser Übung wird gezeigt, wie Sie die CDB-weite Mindestkennwortlänge für die Datenbankbenutzeraccounts durchsetzen, ohne den Zugriff auf die Datenbankbenutzerprofile einzuschränken.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Einrichten der Umgebung

### Voraussetzungen

*   Oracle-Account
*   SSH-Schlüssel
*   DBCS-VM-Datenbank erstellen
*   21c einrichten

## Aufgabe 1: Erforderliches Profil in der CDB-Root erstellen

1.  Stellen Sie eine Verbindung zur CDB-Root in `CDB21` her.
    
        $ <copy>sqlplus sys@CDB21 AS SYSDBA</copy>
        
        SQL*Plus: Release 21.0.0.0.0 - Production on Wed Aug 12 09:45:45 2020
        Version 21.1.0.0.0
        Enter password: <i>WElcome123##</i>
        Connected to:
        Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Development
        Version 21.1.0.0.0
        
        SQL>
        
2.  Erstellen Sie das erforderliche Root-Profil. Das obligatorische Root-Profil fungiert als stets aktives Benutzerprofil. Erforderliche Profillimits werden zusätzlich zu den vorhandenen Limits aus dem Profil durchgesetzt, dem der Benutzer zugewiesen ist. Dies führt zu einem Vereinigungseffekt in dem Sinne, dass das Skript zur Verifizierung der Passwortkomplexität des obligatorischen Profils vor dem Skript zur Passwortkomplexität aus dem Profil des Benutzerkontos ausgeführt wird, sofern vorhanden.
    
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
        

## Aufgabe 2: Initialisierungsparameter `MANDATORY_USER_PROFILE` festlegen

1.  Initialisierungsparameter festlegen
    
        SQL> <copy>ALTER SYSTEM SET mandatory_user_profile=C##PROF_MIN_PASS_LEN;</copy>
        System altered.
        
        SQL> <copy>SHOW PARAMETER mandatory_user_profile</copy>
        
        NAME                                 TYPE        VALUE
        ------------------------------------ ----------- ------------------------------
        mandatory_user_profile               string      C##PROF_MIN_PASS_LEN
        SQL>
        
    
    _Die Funktion zur Kennwortprüfung des obligatorischen Profils wird immer aus `CDB$ROOT` durchgesetzt. Das bedeutet, dass der Kennwortressourcengrenzwert immer aus `CDB$ROOT` abgerufen und ausgeführt und für die PDBs in der gesamten CDB durchgesetzt wird, je nach Initialisierungsparameter `MANDATORY_USER_PROFILE`._
    

## Aufgabe 3: Ersetzen Sie die Kennwortverifizierungsfunktion functio`n`, um die Mindestkennwortlänge durchzusetzen.

1.  Verifizierungsfunktion ersetzen
    
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
        

## Aufgabe 4: Test

1.  Erstellen Sie einen neuen Benutzer `JOHN` in `PDB21`.
    
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
        

## Aufgabe 5: Konfiguration zurücksetzen

1.  Löschen Sie das erforderliche Profil in der Root.
    
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
        
2.  Setzen Sie zuerst den Initialisierungsparameter `MANDATORY_USER_PROFILE` zurück.
    
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
        
3.  Starten Sie die Instanz neu.
    
        <copy>/home/oracle/labs/M104784GC10/wallet.sh</copy>
        
4.  Stellen Sie eine Verbindung zur Instanz her, und entfernen Sie das Profil
    
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
        

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - David Start, Kay Malcolm, Database Product Management
*   **Zuletzt aktualisiert am/um** - David Start, Dezember 2020