# 21C-Umgebung einrichten

## Einführung

In dieser Übung führen Sie die Skripte aus, um die Umgebung für den Oracle Database 21c-Workshop einzurichten.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Verbindungen definieren und testen
*   Für die Ausführung von 21c-Übungen erforderliche Skripte herunterladen
*   Skripte mit dem gewählten Passwort aktualisieren

### Voraussetzungen

*   Oracle-Account
*   Praktische Erfahrung mit vi oder nano
*   SSH-Schlüssel
*   DBCS-VM-Datenbank erstellen
*   Öffentliche DBCS-Adresse
*   Eindeutiger Datenbankname

## Aufgabe 1: Serversetup

1.  Wenn Sie noch nicht angemeldet sind, melden Sie sich bei Oracle Cloud an, und starten Sie die Oracle Cloud Shell neu. Fahren Sie andernfalls mit Schritt 3 fort.
    
2.  Navigieren Sie in Cloud Shell oder im Terminalfenster zu dem Ordner, in dem Sie die SSH-Schlüssel erstellt haben, und geben Sie diesen Befehl mit Ihrer IP-Adresse ein:
    
        $ <copy>ssh -i <<sshkeyname>> opc@</copy>< your_IP_address >
        Enter passphrase for key './myOracleCloudKey':
        Last login: Tue Feb  4 15:21:57 2020 from < your_IP_address >
        [opc@tmdb1 ~]$
        
3.  Sie müssen die Verzeichnisse für die Containerdatenbank und die integrierbaren Datenbanken erstellen.
    
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
        
4.  Nach der Anmeldung können Sie zum BS-Benutzer "oracle" wechseln.
    
        [opc@tmdb1 ~]$ <copy>sudo su - oracle</copy>
        
5.  Der erste Schritt besteht darin, alle für diese Übung erforderlichen Skripte abzurufen.
    
        <copy>
        cd /home/oracle
        wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/Cloud_21c_Labs.zip
        </copy>
        
6.  Cloud\_21c\_Labs.zip entpacken
    
        <copy>
        unzip Cloud_21c_Labs.zip
        </copy>
        
7.  Machen Sie das Skript für alle lesbar, beschreibbar und ausführbar.
    
        <copy>
        chmod 777 /home/oracle/labs/update_pass.sh
        </copy>
        
8.  Führen Sie das Shellskript /home/oracle/labs/update\_pass.sh aus. Das Shellskript fordert Sie zur Eingabe von password\_defined\_during\_DBSystem\_creation auf und legt es in allen Shellskripten und SQL-Skripten fest, die in den Übungen verwendet werden.
    
        [oracle@da3 ~]$ <copy>/home/oracle/labs/update_pass.sh</copy>
        Enter the password you set during the DBSystem creation: WElcome123##
        

## Aufgabe 2: Datenbank erstellen

1.  Erstellen Sie nun die Datenbank `CDB21`. Auf diesem Server ist eine Datenbank vorhanden, aber `CDB21` wurde mit allem erstellt, was für diesen Workshop benötigt wird.
    
        <copy>
        cd /home/oracle/labs/M104784GC10
        /home/oracle/labs/M104784GC10/create_CDB21.sh
        </copy>
        
2.  Legen Sie Ihre Umgebung fest. Geben Sie bei der Eingabeaufforderung `CDB21` ein.
    
        [oracle@db1 ~]$ <copy>. oraenv</copy>
        ORACLE_SID = [CDB21] ? CDB21
        The Oracle base remains unchanged with value /u01/app/oracle
        [oracle@db1 ~]$
        
3.  Prüfen Sie, ob Oracle Database 21c `CDB21` und `PDB21` mit den folgenden Befehlen erstellt wurden.
    
        <copy>
        ps -ef|grep smon
        sqlplus / as sysdba
        </copy>
        
    
        <copy>
        show pdbs
        exit;
        </copy>
        
4.  Stellen Sie sicher, dass der TNS-Alias für `CDB21`, `PDB21` und `PDB21_2` in der Datei tnsnames.ora erstellt wurde. Wenn sie nicht da sind, müssen Sie sie hinzufügen. Die Datei befindet sich in `/u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora`.
    
        	  <copy>
        	  cat /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
5.  Dann sollte der Eintrag für `CDB21` beim Erstellen der Datenbank erstellt worden sein. Kopieren Sie für `PDB21` also einfach den Eintrag für `CDB21`, und ändern Sie den Wert `CDB21` an beiden Stellen in `PDB21`. Wiederholen Sie dies für `PDB21_2`.
    
        	  <copy>
        	  vi /u01/app/oracle/homes/OraDB21Home1/network/admin/tnsnames.ora
        	  </copy>
        
6.  Es wird mehr in Ihrer tnsnames.ora geben, aber die für die drei Einträge, die uns wichtig sind, sollten wie die Einträge unten aussehen, aber stattdessen mit Ihrem Hostnamen.
    
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
        
7.  Testen Sie die Verbindung zu CDB21. Melden Sie sich mit SQL\*Plus mit dem Kennwort **WElcome123##** bei CDB21 an.
    
        	  <copy>
        	  sqlplus sys@cdb21 AS SYSDBA
        	  </copy>
        
8.  Prüfen Sie, ob der Containername **CDB$ROOT** lautet.
    
        <copy>
        	  SHOW CON_NAME;
        	  </copy>
        
9.  Testen Sie die Verbindung zu PDB21 mit demselben Kennwort wie im vorherigen Schritt.
    
        <copy>
        CONNECT sys@PDB21 AS SYSDBA
        </copy>
        
10.  Zeigen Sie den Containernamen an. Daraufhin sollte **PDB21** angezeigt werden.
    

    ```
    <copy>
    SHOW CON_NAME;
    </copy>
    ```
    

11.  Beenden Sie SQL\*Plus.
    
        <copy>
        exit
        </copy>
        

## Danksagungen

*   **Autor** - Donna Keesling, Datenbank-UA-Team
*   **Mitwirkende** - David Start, Kay Malcolm, Database Product Management
*   **Zuletzt aktualisiert am/um** - Arabella Yao, Produktmanagerin, Datenbankproduktmanagement, Dezember 2021