# Default Tablespace-Verschlüsselungsalgorithmus festlegen

## Einführung

In dieser Übung wird gezeigt, wie bei den Kennwörtern in den Kennwortdateien in Oracle Database 21c die Groß-/Kleinschreibung beachtet wird. In früheren Oracle Database-Releases behalten Kennwortdateien standardmäßig ihre ursprünglichen Verifizierungen ohne Beachtung der Groß-/Kleinschreibung bei. Der Parameter zum Aktivieren oder Deaktivieren der Groß-/Kleinschreibung für Kennwortdateien `IGNORECASE` wurde entfernt. Bei allen Passwörtern in neuen Passwortdateien muss die Groß-/Kleinschreibung beachtet werden.

Voraussichtliche Zeit: 5 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Einrichten der Umgebung

### Voraussetzungen

*   Oracle-Account
*   SSH-Schlüssel
*   DBCS-VM-Datenbank erstellen
*   21c einrichten

## Aufgabe 1: Standard-Tablespace-Verschlüsselungsalgorithmus festlegen

1.  Melden Sie sich bei der CDB-Root an, und zeigen Sie den Standard-Tablespace-Verschlüsselungsalgorithmus an.
    
        	$ <copy>sqlplus / AS SYSDBA</copy>
        	Connected to:
        
        	Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        	Version 21.2.0.0.0
        
    
        	SQL> <copy>SHOW PARAMETER TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM</copy>
        
        	NAME                                       TYPE   VALUE
        	------------------------------------------ ------ -----------------------
        	tablespace_encryption_default_algorithm    string AES128
        
        	SQL>
        
2.  Ändern Sie den Tablespace-Verschlüsselungsalgorithmus.
    
        	SQL> <copy>ALTER SYSTEM SET TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM=AES192;</copy>
        	System altered.
        
        	SQL> <copy>EXIT</copy>
        
        	$
        
3.  Stellen Sie eine Verbindung zur PDB her, und erstellen Sie einen neuen Tablespace in `PDBTEST`.
    
        	$ <copy>sqlplus sys@PDB21 AS SYSDBA</copy>
        	Enter password: <b><i>WElcome123##</i></b>
        
        	Connected.
        

    SQL> <copy>CREATE TABLESPACE tbstest DATAFILE '/u02/app/oracle/oradata/pdb21/test01.dbf' SIZE 2M;</copy>
    Tablespace created.
    
    SQL>
    ```
    

## Aufgabe 2: Verifizieren des verwendeten Tablespace-Verschlüsselungsalgorithmus

1.  Prüfen Sie den verwendeten Tablespace-Verschlüsselungsalgorithmus.
    
        	SQL> <copy>SELECT name, encryptionalg
        				FROM v$tablespace t, v$encrypted_tablespaces v
        				WHERE t.ts#=v.ts#;</copy>
        
        	NAME                           ENCRYPT
        	------------------------------ -------
        	USERS                          AES128
        	TBSTEST                        AES192
        
        	SQL> <copy>EXIT</copy>
        
        	$
        

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - David Start, Kay Malcolm, Database Product Management
*   **Zuletzt aktualisiert am/um** - David Start, Dezember 2020