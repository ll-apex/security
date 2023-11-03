# Beispielschemasetup

## Einführung

In dieser Übung wird gezeigt, wie Sie Ihre Datenbankschemas für die nachfolgenden Übungen einrichten.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung richten Sie ein Beispielschema ein:

*   Festlegen der Umgebungsvariablen
*   Beispielschemas der Datenbank abrufen und entpacken
*   Beispiel-Schemas installieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein LiveLabs Cloud-Account und ein zugewiesenes Compartment
*   Die IP-Adresse und der Instanzname für die Compute-Instanz DB19c
*   Erfolgreich bei Ihrem LiveLabs-Account angemeldet
*   Ein gültiges SSH-Schlüsselpaar

## Aufgabe 1: Beispieldaten installieren

In diesem Schritt installieren Sie eine Auswahl der Oracle Database-Beispielschemas. Weitere Informationen zu diesen Schemas finden Sie in der Schemavereinbarung am Ende dieser Übung.

Wenn Sie die Anweisungen unter den Beispielschemas eingeben, werden **SH**, **OE** und **HR** installiert. Diese Schemas werden in der Oracle-Dokumentation verwendet, um SQL-Sprachkonzepte und andere Datenbankfeatures anzuzeigen. Die Schemas selbst sind in Oracle Database-Beispielschemas [Oracle Database-Beispielschemas](https://www.oracle.com/pls/topic/lookup?ctx=dblatest&id=COMSC) dokumentiert.

1.  Führen Sie ein _whoami_ aus, um sicherzustellen, dass der Wert _oracle_ zurückkommt.)
    
    Hinweis: Wenn Sie in Windows mit putty ausgeführt werden, stellen Sie sicher, dass Ihr Sessiontimeout auf mehr als 0 festgelegt ist.
    
        <copy>whoami</copy>
        
2.  Wenn Sie nicht der Benutzer oracle sind, melden Sie sich erneut an:
    
        <copy>
        sudo su - oracle
        </copy>
        
    
    ![Ersatzbenutzer oracle](./images/sudo-oracle.png " ")
    
3.  Legen Sie die Umgebungsvariablen so fest, dass sie auf die Oracle-Binärdateien verweisen. Wenn Sie zur Eingabe der SID (Oracle Database System Identifier) aufgefordert werden, geben Sie **cdb1** ein.
    
        <copy>
        . oraenv
        </copy>
        
    
    ![Umgebungsvariablen](./images/oraenv.png " ")
    
4.  Rufen Sie die Beispielschemas der Datenbank ab, und dekomprimieren Sie sie. Legen Sie dann den Pfad in den Skripten fest.
    
        <copy>
        wget https://github.com/oracle/db-sample-schemas/archive/v19c.zip
        unzip v19c.zip
        cd db-sample-schemas-19c
        perl -p -i.bak -e 's#__SUB__CWD__#'$(pwd)'#g' *.sql */*.sql */*.dat
        sed -i 's#COMPRESS,#,#g' sales_history/csh_v3.sql
        </copy>
        
    
    ![Schema installieren](./images/install-schema-zip1.png " ") ![Schema installieren](./images/install-schema-zip2.png " ")
    
5.  Führen Sie diesen Befehl grep aus, um zu prüfen, ob der Pfad ordnungsgemäß festgelegt wurde. Nichts sollte zurückgegeben werden, wenn Sie diesen Befehl ausführen.
    
        <copy>
        grep 'CWD' */*.sql
        </copy>
        
6.  Melden Sie sich mit SQL\*Plus als Benutzer **oracle** an, und erstellen Sie den Tablespace TEST\_DATA, um die Installation von Beispielschemas vorzubereiten.
    
        <copy>
        sqlplus system/Oracle123@localhost:1521/pdb1
        create tablespace TEST_DATA datafile '/u01/oradata/cdb1/pdb1/test_data.dbf' size 300m autoextend on extent management local uniform size 512K;
        </copy>
        
    
    ![SQLPlus starten](./images/start-sqlplus-create-tbs-01.png " ")
    
7.  Installieren Sie die Beispielschemas, indem Sie das folgende Skript ausführen.
    
        <copy>
        @/home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/sh_main Oracle123 test_data temp Oracle123 /home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/ /home/oracle/ v3 localhost:1521/pdb1
        create index sh.sales_cust_channel_promo_idx on sh.sales(cust_id, channel_id, promo_id);
        </copy>
        
    
    ![Schemainstallation](./images/sales_history_creation.png " ")
    
        <copy>
        col table_name for a30
        set line 2000 pages 2000
        select table_name, num_rows from user_tables order by table_name;
        </copy>
        
    
    ![Schema installiert](./images/tables-created.png " ")
    
8.  Beenden Sie SQL Plus beim Benutzer "oracle".
    
        <copy>
        exit
        </copy>
        
    
    ![Zurück zu opc](images/return-to-opc.png)
    
9.  GZIP-Backup der Beispieldatendatei erstellen (Speicherkomprimierungssimulation)
    
    **Hinweis**: Ihre Zahlen können sich geringfügig von den im Screenshot gezeigten unterscheiden. Der Punkt ist jedoch, dass die Komprimierung aufgetreten ist. Solange das noch für Sie gezeigt wird, sind Sie gut, durch das Labor zu gehen.
    
        <copy>
        du -hs /u01/oradata/cdb1/pdb1/test_data.dbf
        cp /u01/oradata/cdb1/pdb1/test_data.dbf /u01/oradata/test_data.dbf
        </copy>
        
    
        <copy>
        gzip /u01/oradata/test_data.dbf
        du -hs /u01/oradata/test_data.dbf.gz
        </copy>
        
    
    ![Speicherkomprimierung](images/storage-compress-simulation.png)
    
    Wie Sie sehen können, haben wir gerade die Datendatei test\_data.dbf gesichert und von 317 MB auf 17 MB komprimiert.
    

**Herzlichen Glückwunsch!** Jetzt haben Sie die Umgebung zum Ausführen der Übungen.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Oracle Database-Beispielschemas - Vereinbarung

Copyright (c) 2019 Oracle

Hiermit wird jeder Person, die eine Kopie dieser Software und der zugehörigen Dokumentationsdateien (die "Software") erwirbt, kostenlos die Genehmigung erteilt, ohne Einschränkung mit der Software zu handeln, einschließlich und ohne Einschränkung der Rechte zur Verwendung, Kopie, Änderung, Zusammenführung, Veröffentlichung, Verteilung, Unterlizenzierung und/oder zum Verkauf von Kopien der Software und zur Genehmigung von Personen, denen die Software zur Verfügung gestellt wird, dies unter den folgenden Bedingungen:

Der obige Copyright-Hinweis und dieser Genehmigungshinweis müssen in allen Kopien oder wesentlichen Teilen der Software enthalten sein.

_DIE SOFTWARE WIRD "WIE SIE IST", OHNE GEWÄHRLEISTUNG JEGLICHER ART, EXPRESS ODER IMPLIZIERT, EINSCHLIESSLICH, ABER NICHT BESCHRÄNKT AUF DIE GARANTIEN DER MARKTGÄNGIGKEIT, FITNESS FÜR EINEN BESTIMMTEN ZWECK UND NICHTVERLETZUNG. IN KEINEM FALL SIND DIE AUTOREN ODER URHEBERRECHTSINHABER FÜR ANSPRÜCHE, SCHÄDEN ODER ANDERE HAFTUNG HAFTBAR, SEI ES IN EINER VERTRAGLICHEN HANDLUNG, FOLTER ODER ANDERWEITIG, AUS, AUSSERHALB ODER IN VERBINDUNG MIT DER SOFTWARE ODER DER VERWENDUNG ODER ANDEREN GESCHÄFTEN IN DER SOFTWARE._

## Danksagungen

*   **Autor**
    *   Royce Fu, Principal Database und O&M Solution Architect
    *   Noah Galloso, Solution Engineer, North America Specialist Hub
*   **Beitragende**
    *   Richard Evans, Produktmanagement für Datenbanksicherheit
    *   Gregg Christman, Database Advanced Compression Product Management
*   **Zuletzt aktualisiert von/Datum**
    *   Royce Fu, Juni 2023