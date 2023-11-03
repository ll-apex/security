# SecureOracle-Umgebung initialisieren

## Einführung

In dieser Übung prüfen und starten wir alle Komponenten, die für die erfolgreiche Ausführung dieses Workshops erforderlich sind.

_Geschätzte Laborzeit_: 30 Minuten

### Ziele

*   Initialisieren Sie die Workshopumgebung SecureOracle.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup

## Aufgabe 1: Prüfen, ob erforderliche Prozesse hochgefahren und gestartet sind

1.  Nachdem Sie nun Zugriff auf Ihre Remote-Desktopsession haben, gehen Sie wie unten angegeben vor, um Ihre Umgebung zu validieren, bevor Sie mit der Ausführung der nachfolgenden Übungen beginnen. Die folgenden Prozesse müssen ausgeführt werden:
    
    *   Datenbank-Listener
        *   HÖRER (1521)
    *   Datenbankserverinstanz
        *   IAMDB
    *   Hedwig-Mail-Server
    
    ![](./images/landing.png " ")
    
2.  Führen Sie in der _Terminal_\-Session den folgenden Befehl aus, um zu prüfen, ob die erwarteten Prozesse hochgefahren sind.
    
        <copy>
        systemctl status hedwig.service
        systemctl status oracle-database
        </copy>
        
    
    ![](./images/hedwig.png " ") ![](./images/sql.png " ")
    
    Wenn alle erwarteten Prozesse in der Ausgabe wie oben dargestellt angezeigt werden, ist Ihre Umgebung für die nächste Aufgabe bereit.
    
3.  Wenn fragwürdige Ausgaben, Fehler oder heruntergefahrene Komponenten angezeigt werden, starten Sie den Service entsprechend neu
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
    
    So starten Sie den _Hedwig Mail-Server_ neu:
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        

## Aufgabe 2: OIG-Komponenten starten

1.  Öffnen Sie eine neue Terminalsession _entfernt mit einem SSH-Client_, und fahren Sie wie unten gezeigt fort, um alle Komponenten als Benutzer "_oracle_" zu starten.
    
        <copy>sc start all</copy>
        
    
    **Hinweis:** Die Startzeit der OIG-Komponenten variiert zwischen 15 und 20 Minuten. ![OIG-Komponenten starten](./images/startall.png " ")
    
    Zu Ihrer Referenz stehen die folgenden Befehle zum Starten oder Stoppen der verschiedenen Komponenten und Anwendungen zur Verfügung.
    
        sc <start|stop|status> oim          // start, stop or status OIG, SOA Server and OUD
        sc <start|stop|status> oim_bip      // start, stop or status OIG, SOA Server, OUD and BIP Server
        sc <start|stop|status> oam          // start, stop or status OAM, OUD and both OHS Servers
        sc <start|stop|status> oam_ohs1     // start, stop or status OAM, OUD and OHS Server 1
        sc <start|stop|status> oam_ohs2     // start, stop or status OAM, OUD and OHS Server 2
        sc <start|stop|status> all          // start, stop or status all OIG and OAM components
        

## Aufgabe 3: Entwicklungstools ausführen

Die Entwicklungstools in SecureOracle unterstützen Anwendungsfälle wie die Bearbeitung von SOA-Composites für OIG-Workflowgenehmigungen, aber auch die Anpassung und Konfiguration der verschiedenen Komponenten nach Bedarf.

1.  Führen Sie in der auf dem Remote-Desktop geöffneten Terminalsession die folgenden Befehle aus, um **Oracle JDeveloper mit SOA-Erweiterungen** zu starten:
    
        <copy>
        cd ~
        ./startJDEVSOA.sh
        </copy>
        
    
    **Hinweis**: Beispielanwendungen mit SOA-Composites und OIG-Workflows finden Sie unter dem Ordner **`/home/oracle/jdevhome`**.
    
        NAME                                 DESCRIPTION
        RoleOwnerApproval.jws                Role single approval
        MultiRoleOwnerApproval.jws           Role multi-approval
        CustomDisconnectedProvisioning.jws   Disconnected app approval based on Sales Role
        
    
    Darüber hinaus finden Sie Out-of-the-box-SOA-Composites von OIM im folgenden Ordner:
    
        /home/oracle/products/oam/idm/server/workflows/composites
        
2.  Um **Oracle SQLDeveloper** zu starten, führen Sie die folgenden Befehle aus:
    
        <copy>
        ./startSQLDEV.sh
        </copy>
        
    
    **Hinweise**: Drei Verbindungen: `HR`, `HEDWIG` und `IAMDB` sind bereits für den Zugriff auf My HR-, Hedwig- und IAM-Datenbankschemas definiert. Alternativ können Sie SQL Developer auf Ihrem lokalen Hostcomputer installieren, um auf die verschiedenen Datenbankschemas zuzugreifen.
    
3.  Um **Apache Studio** zu starten und die OUD/LDAP-Instanz zu verwalten, führen Sie die folgenden Befehle aus:
    
        <copy>
        	./startAStudio.sh
        </copy>
        
    
    **Hinweis**: Eine Verbindung `IAM-OUD` ist bereits für den Zugriff auf OUD definiert.
    
4.  Um das **Oracle PL/SQL**\-Befehlszeilentool zu starten, das auf die IAM-Datenbank zeigt, führen Sie die folgenden Befehle aus:
    
        <copy>
        cd ~
        . ./setDBenv.sh
        ./startPLSQL.sh
        </copy>
        

## Aufgabe 4: Admin-Konsolen, Anwendungen und Benutzerzugangsdaten

Wenn Sie diese lieber von Ihrem lokalen Computer aus aufrufen möchten, wählen Sie eine der folgenden Optionen:

*   Verwenden Sie die in diesem Abschnitt angegebenen URLs wie dargestellt, nachdem Sie den Hosteintrag unten zu **`/etc/hosts`** auf Ihrem lokalen Mac/Linux-Host oder **`C:\Windows\System32\drivers\etc\hosts`** für Microsoft Windows-Hosts hinzugefügt haben
    
        <copy><public_ip> secureoracle.oracledemo.com  secureoracle</copy>
        
*   Ersetzen Sie in jeder URL unten **`secureoracle.oracledemo.com`** durch `Public_IP_Address` der Instanz.
    

1.  Verwenden Sie die folgenden URLs und Zugangsdaten, um auf alle verschiedenen Webkonsolen zuzugreifen:
    
    Oracle Identity Manager-Web-Admin-Konsole:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Self Service -
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Design Console Führen Sie zunächst die folgenden Befehle aus, um die Designkonsole zu starten:
    
        <copy>
        cd /home/oracle/products/oim/idm/designconsole
        ./xlclient.sh
        </copy>
        
    
    Wenn Sie dazu aufgefordert werden, geben Sie die folgende URL und die folgenden Zugangsdaten ein:
    
        URL         t3://secureoracle.oracledemo.com:14000
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle BI Publisher-Admin-Konsole -
    
        URL         http://secureoracle.oracledemo.com:9502/xmlpserver
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager EM-Konsole:
    
        URL         http://secureoracle.oracledemo.com:7001/em
        User        weblogic
        Password    Oracle123
        
    
    Oracle Access Manager-Administrationskonsole:
    
        URL         http://secureoracle.oracledemo.com:8001/oamconsole
        User        oamadmin
        Password    Oracle123
        
    
    Oracle Access Manager EM-Konsole:
    
        URL         http://secureoracle.oracledemo.com:8001/em
        User        weblogic
        Password    Oracle123
        
    
    E-Mail-Server-Administrationskonsole (Hedwig Mail Server):
    
        URL         http://secureoracle.oracledemo.com:7001/hedwig-web-0.6/
        User        admin
        Password    Oracle123
        
    
    **Hinweis:** Der **Hedwig-Mail-Server** kann neu gestartet werden, wenn bei der Verbindung mit dem Roundcube-E-Mail-Client Probleme auftreten. Beispiel: Führen Sie die folgenden Befehle aus, um den Hedwig Mail Server als Benutzer _opc_ neu zu starten:
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        
    
    E-Mail-Webclient (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    Meine HR-Anwendung:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        
    
    Meine IGA-Anwendung:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        User        mgraff
        Password    Oracle123
        
    
    APEX-Admin-Konsole
    
        URL         http://secureoracle.oracledemo.com:7001/ords/apex_admin
        User        admin
        Pass        #Oracle123
        
    
    APEX-HR-Workspace
    
        URL         http://secureoracle.oracledemo.com:7001/ords/
        Workspace   HRSPACE
        User        hradmin
        Pass        Oracle123
        
    
    **Hinweis**: Meine HR- und Meine IGA-Anwendungen sind verfügbar, nachdem die OIG-Domain gestartet wurde, während der Service (Oracle REST Data Services), der diese Anwendungen unterstützt, auf dem OIG-Admin-Server bereitgestellt wird.
    
2.  Die folgenden Zugangsdaten sind für den Zugriff auf andere Middleware-Komponenten verfügbar.
    
    Oracle IAM-Datenbank:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            sys as SYSDBA
        Password        Oracle123
        
    
    Meine HR-Anwendungsdatenbank:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            hr
        Password        Oracle123
        
    
    Oracle Unified Directory (OUD):
    
        Host/port       secureoracle.oracledemo.com:1389
        User            cn=Directory Manager
        Password        Oracle123
        

## Aufgabe 5: Branding SecureOracle (optional)

Mit den folgenden Anweisungen können Sie das Logo in der OIG-Selfserviceoberfläche anpassen. Für Illustrationen verwenden wir ein Beispiellogobild, das auf Ihrer Instanz bereitgestellt wird. Fühlen Sie sich frei, Ihr eigenes Bild zu verwenden, wenn gewünscht. Wenn Sie Ihr eigenes Logo verwenden möchten, beachten Sie die empfohlene Größe von 145 x 38 Pixeln.

1.  Kopieren Sie die Bilddatei in _`/home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images`_
    
        <copy>
        cp /home/oracle/demo/sample-data/mylogo.png /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images
        </copy>
        
2.  Melden Sie sich bei der OIG-Selfservicekonsole als Benutzer **xelsysadm** an. Klicken Sie auf den Link **Sandboxes** in der oberen rechten Ecke der Selfservice-Seite.
    
3.  Klicken Sie auf der Registerkarte **Sandboxes verwalten** auf **Sandbox erstellen**, geben Sie einen Namen ein, und klicken Sie auf **Speichern und schließen** und dann auf **OK**.
    
4.  Klicken Sie oben rechts auf der aktuellen Seite auf den Link **Anpassen**.
    
5.  Der Anpassungsbereich wird oben auf der Seite angezeigt. Klicken Sie auf **Struktur** und dann auf das Oracle-Logobild. Klicken Sie im Popup-Fenster **Gemeinsame Komponentenbearbeitung bestätigen** auf die Schaltfläche **Bearbeiten**.
    
6.  Klicken Sie als Nächstes auf das Symbol **Zahnrad und Bleistift** oben im rechten Fensterbereich.
    
7.  Klicken Sie im Fenster **Komponenteneigenschaften: commandImageLink** auf das Symbol **Pfeil nach unten** neben der Eigenschaft "Symbol", und wählen Sie **Expression Builder** aus.
    
8.  Geben Sie das Pfadbild wie folgt ein.
    
    Beispiel: Geben Sie den Pfad im folgenden Format ein:
    
        <copy>
        /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images/mylogo.png
        </copy>
        
9.  Klicken Sie auf **OK**, um das Expression Builder-Fenster zu schließen, und klicken Sie dann erneut auf **OK**, um die Änderungen zu bestätigen. Das benutzerdefinierte Logobild sollte anstelle des Oracle-Logobildes angezeigt werden.
    
10.  Klicken Sie in der oberen rechten Ecke auf die Schaltfläche **Schließen**, um den Anpassungsbereich zu schließen.
    
11.  Gehen Sie zurück zur Registerkarte **Sandboxes verwalten**, wählen Sie den erstellten Sandbox-Namen aus, und klicken Sie auf **Sandbox veröffentlichen**. Klicken Sie dann zur Bestätigung auf **Ja**.
    

## **Anhang**: Informationen zur Beispielorganisation

1.  SecureOracle enthält ein Beispiel für **Oracle-Benutzer** der obersten OIM-Organisation und zwei untergeordnete Abteilungen **Vertrieb** und **Finanzen**. Für jede Abteilung wurde ein Administratorkonto definiert, um die delegierte Administration zu demonstrieren. Darüber hinaus wurden Beispielbenutzer hinzugefügt, um die Genehmigung, Eskalation und Organisationstransfers durch Manager zu demonstrieren.
    
    ![Beispiel für eine OIM-Organisation](./images/img-orgtree.png " ")
    
    Abbildung 4. OIG-Beispielorganisation
    
2.  Im Folgenden finden Sie eine Kurzreferenz zu den Demobenutzern, die in SecureOracle enthalten sind. Weitere Informationen zur Verwendung dieser Benutzer und Organisationen in den Beispieldemonstrationen finden Sie in der Dokumentation zu Anwendungsfällen.
    
        USERNAME        ORGANIZATION     TITLE                        ADMIN ROLE      SCOPE OF CONTROL
        FINANCEADM      Finance          Administration Assistant     FinanceAdmin    Finance
        SALESADM        Sales            Administration Assistant     SaleseAdmin     Sales
        MGRAFF          Sales            Sales Manager
        HDANIELS        Sales            Sales Manager
        JSMITH          Finance          Finance Manager
        
3.  Darüber hinaus wurden E-Mail-Accounts für alle Demo erstellt, die users.You über den E-Mail-Client [Roundcube](http://secureoracle.oracledemo.com/roundcubemail-1.4.1/) mit den Zugangsdaten **USERNAME/Oracle123** auf ihre Posteingänge zugreifen kann.
    
        USERNAME     EMAIL
        AHUTTON      ahutton@oracledemo.com
        JMALLIN      jmallin@oracledemo.com
        DFAVIET      dfaviet@oracledemo.com
        SMAVRIS      smavris@oracledemo.com
        MCHAN        mchan@oracledemo.com
        HDANIELS     hdaniels@oracledemo.com
        MGRAFF       mgraff@oracledemo.com
        SALESADM     salesadm@oracledemo.com
        ECLARK       eclark@oracledemo.com
        JSMITH       jsmith@oracledemo.com
        PSONG        psong@oracledemo.com
        PCAR         pcar@oracledemo.com
        FINANCEADM   financeadm@oracledemo.com
        DCOBY        dcoby@oracledemo.com
        GMARTON      gmarton@oracledemo.com
        RLAURIA      rlauria@oracledemo.com
        RMAINOR      rmainor@oracledemo.com
        

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Weitere Informationen

Über diese Links erhalten Sie weitere Informationen zu Oracle Identity and Access Management:

*   [Oracle Identity Management-Website](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governance-Dokumentation](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Management-Dokumentation](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Danksagungen

*   **Autor** - Ricardo Gutierrez, Solution Engineering - Sicherheit und Management
*   **Mitwirkende** - Rene Fontcha, Sahaana Manavalan
*   **Zuletzt aktualisiert am/um** - Sahaana Manavalan, LiveLabs Developer, NA Technology, März 2022