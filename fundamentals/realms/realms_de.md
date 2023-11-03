# Blockieren gemeinsamer Vorgänge durch lokale Benutzer verhindern - Realms

## Einführung

In dieser Übung wird gezeigt, wie Sie verhindern, dass lokale Benutzer Oracle Database Vault-Steuerelemente für Objekte gemeinsamer Benutzer erstellen, die verhindern, dass allgemeine Benutzer auf lokale Daten in ihrem eigenen Schema in PDBs zugreifen. Ein lokaler Database Vault-Eigentümer einer PDB kann eine Realm mit allgemeinen Oracle-Schemas wie `DVSYS` oder `CTXSYS` erstellen und deren korrekte Funktionsweise verhindern. Im Rahmen dieser Übung wird das benutzerdefinierte Schema `C##TEST1` in der CDB-Root erstellt, um dieses Feature anzuzeigen.

Geschätzte Zeit: 40 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Einrichten der Umgebung

### Voraussetzungen

*   Oracle-Account
*   SSH-Schlüssel
*   DBCS-VM-Datenbank erstellen
*   21c einrichten

## Aufgabe 1: Database Vault auf CDB- und PDB-Ebene konfigurieren und aktivieren

1.  Konfigurieren und aktivieren Sie Database Vault auf CDB-Root-Ebene und auf PDB-Ebene. Das Skript erstellt die Tabelle `HR.G_EMP` im Root-Container sowie die Tabelle `HR.L_EMP` in `PDB21`.
    
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
        

## Aufgabe 2: Zugriff auf Tabellendaten ohne Realm für gemeinsame Objekte testen

1.  Stellen Sie als `C##SEC_ADMIN` eine Verbindung zur CDB-Root her, um den Status von `DV_ALLOW_COMMON_OPERATION` zu prüfen. Dies ist das Standardverhalten: Lokale Benutzer können Database Vault-Steuerelemente für gemeinsame Benutzerobjekte erstellen.
    
        $ <copy>sqlplus c##sec_admin</copy>
        
        Enter password: <i>WElcome123##</i>
        
    
        SQL> <copy>SELECT * FROM DVSYS.DBA_DV_COMMON_OPERATION_STATUS;</copy>
        
        NAME                      STATU
        ------------------------- -----
        DV_ALLOW_COMMON_OPERATION FALSE
        
        SQL>
        
2.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        

## Aufgabe 3: Zugriff auf Tabellendaten mit einer allgemeinen regulären oder obligatorischen Realm für gemeinsame Objekte testen

1.  Erstellen Sie eine allgemeine reguläre Realm in `C##TEST1`\-Tabellen in der CDB-Root.
    
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
        
2.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Erstellen Sie eine allgemeine obligatorische Realm in `C##TEST1`\-Tabellen in der CDB-Root.
    
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
        
8.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Aufgabe 4: Zugriff auf Tabellendaten für gemeinsame Objekte mit regulärer oder obligatorischer PDB-Realm testen

1.  Erstellen Sie eine reguläre PDB-Realm in `C##TEST1`\-Tabellen in `PDB21`.
    
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
        
2.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
6.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Erstellen Sie eine obligatorische PDB-Realm in `C##TEST1`\-Tabellen in `PDB21`.
    
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
        
8.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
11.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        SELECT * FROM c##test1.l_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
12.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm')</copy>
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Aufgabe 5: Lokale Benutzer daran hindern, Oracle Database Vault-Kontrollen für gemeinsame Objekte zu erstellen

1.  Schränken Sie die lokalen Benutzer ein, Oracle Database Vault-Kontrollen für gemeinsame Objekte zu erstellen.
    
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
        

## Aufgabe 6: Zugriff auf Tabellendaten mit einer allgemeinen regulären oder obligatorischen Realm für gemeinsame Objekte testen

1.  Erstellen Sie eine allgemeine reguläre Realm in `C##TEST1`\-Tabellen in der CDB-Root.
    
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
        
2.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        SQL>
        
4.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Erstellen Sie eine allgemeine obligatorische Realm in `C##TEST1`\-Tabellen in der CDB-Root.
    
        SQL> <copy>BEGIN
        DBMS_MACADM.CREATE_REALM(
        realm_name    => 'Root Test Realm',
        description   => 'Test Realm description',
        enabled       => DBMS_MACUTL.G_YES,
        audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL,
        realm_type    => 1);
        

ENDE; / 2 3 4 5 6 7 8 9

PL/SQL-Prozedur erfolgreich abgeschlossen.

SQL> BEGIN DBMS\_MACADM.ADD\_OBJECT\_TO\_REALM( realm\_name => 'Root Test Realm', object\_owner => 'C##TEST1', object\_name => '%', object\_type => '%'); END; / 2 3 4 5 6 7 8

    PL/SQL procedure successfully completed.
    
    SQL>
    ```
    

8.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
9.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        SELECT * FROM c##test1.g_emp
                               *
        ERROR at line 1:
        ORA-01031: insufficient privileges
        
        
        SQL>
        
10.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Root Test Realm')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        

## Aufgabe 7: Zugriff auf Tabellendaten für gemeinsame Objekte mit regulärer oder obligatorischer PDB-Realm testen

1.  Erstellen Sie eine reguläre PDB-Realm in `C##TEST1`\-Tabellen in `PDB21`.
    
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
        
2.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
3.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
4.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
5.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
6.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT c##sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL>
        
7.  Erstellen Sie eine obligatorische PDB-Realm in `C##TEST1`\-Tabellen in `PDB21`.
    
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
        
8.  Stellen Sie als allgemeiner Tabellenbesitzer `C##TEST1` eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test1</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
9.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zur CDB-Root her.
    
        SQL> <copy>CONNECT c##test2</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.g_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_GLOBAL       1000
        
        SQL>
        
10.  Melden Sie sich als `C##TEST1`, dem allgemeinen Eigentümer der Tabelle, bei `PDB21` an.
    
        SQL> <copy>CONNECT c##test1@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
11.  Stellen Sie als `C##TEST2`, einem weiteren allgemeinen Benutzer, eine Verbindung zu `PDB21` her.
    
        SQL> <copy>CONNECT c##test2@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM c##test1.l_emp;</copy>
        
        NAME           SALARY
        ---------- ----------
        EMP_LOCAL        2000
        
        SQL>
        
12.  Löschen Sie die Realm.
    
        SQL> <copy>CONNECT sec_admin@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>EXEC DBMS_MACADM.DELETE_REALM_CASCADE('Test Realm1')</copy>
        
        PL/SQL procedure successfully completed.
        
        SQL> <copy>EXIT</copy>
        $
        

## Aufgabe 8: Zusammenfassung

Fassen wir das Verhalten des Datenzugriffs auf Objekte gemeinsamer Benutzer in PDBs zusammen, wenn Sie den Wert `DV_ALLOW_COMMON_OPERATION` wechseln.

*   Wenn Sie eine reguläre oder obligatorische Realm in der CDB-Root und eine reguläre oder obligatorische PDB-Realm erstellen und `DV_ALLOW_COMMON_OPERATION` `TRUE` ist, können Sie auf Daten von allgemeinen Benutzerobjekten zugreifen.
    
*   Wenn lokale Realms erstellt wurden, als `DV_ALLOW_COMMON_OPERATION` auf `FALSE` gesetzt wurde, wären sie nach der neuen Kontrolle weiterhin vorhanden, die Durchsetzung würde jedoch ignoriert.
    

## Aufgabe 9: Database Vault sowohl in der PDB als auch in der CDB-Root deaktivieren

1.  Führen Sie das Skript `disable_DV.sh` aus, um Database Vault sowohl in der PDB als auch in der CDB-Root zu deaktivieren.
    
        $ <copy>/home/oracle/labs/M104781GC10/disable_DV.sh</copy>
        ...
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEY IDENTIFIED BY <i>WElcome123##</i> WITH BACKUP CONTAINER=CURRENT;
        keystore altered.
        
        SQL> exit
        $
        

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - David Start, Kay Malcolm, Database Product Management
*   **Zuletzt aktualisiert am/um** - David Start, Dezember 2020