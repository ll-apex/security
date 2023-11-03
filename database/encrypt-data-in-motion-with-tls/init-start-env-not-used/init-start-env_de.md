# Umgebung initialisieren

## Einführung

In dieser Übung prüfen und starten wir alle Komponenten, die für die erfolgreiche Ausführung dieses Workshops erforderlich sind.

Voraussichtliche Zeit: 10 Minuten.

### Ziele

*   Initialisieren Sie die Workshop-Umgebung.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten
    *   Übung: Umgebungssetup

## Aufgabe 1: Überprüfen Sie, ob die erforderlichen Prozesse hochgefahren und gestartet sind.

**Hinweis:** Alle Screenshots zu SSH-Terminalaufgaben, die in diesem Workshop vorgestellt wurden, wurden mit dem SSH-Client _MobaXterm_ erfasst, wie in diesem Schritt beschrieben. Wenn Sie solche Aufgaben in der grafischen Remote-Desktop-Session ausführen, überspringen Sie daher Schritte, bei denen Sie sich als Benutzer _oracle_ mit _sudo su - oracle_ anmelden müssen. Der Grund dafür ist, dass sich die Remote-Desktop-Session unter dem Benutzer _oracle_ befindet.

1.  Nachdem Sie nun Zugriff auf Ihre Remote-Desktopsession haben, gehen Sie wie unten angegeben vor, um Ihre Umgebung zu validieren, bevor Sie mit der Ausführung der nachfolgenden Übungen beginnen. Die folgenden Prozesse müssen ausgeführt werden:
    
    *   Datenbank-Listener
    *   Datenbankserver (emcdb und cdb1)
    *   Enterprise Manager - Management Server (OMS)
    *   Enterprise Manager - Management Agent (emagent)
    *   Meine HR-Anwendungen auf Glassfish
2.  Im Webbrowserfenster auf der rechten Seite befindet sich eine mit _Enterprise Manager_ vorgeladene Registerkarte. Melden Sie sich mit den folgenden Zugangsdaten an, um zu prüfen, ob sie betriebsbereit ist. Wenn die Anmeldeseite bei der ersten Anmeldung beim Remote-Desktop nicht angezeigt wird, aktualisieren Sie sie, um sie neu zu laden. Es dauert ca. 15 Minuten, bis alle Prozesse vollständig gestartet sind.
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
    ![Enterprise Manager-Anmeldung](images/em-login.png "Enterprise Manager-Anmeldung")
    
3.  Öffnen Sie neue Browserregisterkarten, und bestätigen Sie die erfolgreiche Wiedergabe von _Meine HR-Anwendungen_ unten.
    
    *   PDB1
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb1</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb1</copy>
        
    
    *   PDB2
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb2</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb2</copy>
        
    
    Wenn alle erfolgreich sind, ist Ihre Umgebung bereit.
    
4.  Wenn Sie immer noch nicht alle _Enterprise Manager_\- und alle obigen Links zur erfolgreichen Wiedergabe abrufen können, öffnen Sie eine Terminalsession, und fahren Sie wie unten angegeben fort, um die Services zu validieren.
    
    *   Datenbankservices (alle Datenbanken und Standard-Listener)
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![DB-Servicestatus](images/db-service-status.png "DB-Servicestatus")
        
    *   DBSec - Lab Service (Enterprise Manager 13c und My HR Applications auf Glassfish)
        
            <copy>
            sudo systemctl status oracle-dbsec-lab
            </copy>
            
        
        ![DBSecLab Dienststatus](images/dbsec-lab-service-status.png "DBSecLab Dienststatus")
        
5.  Wenn fragwürdige Ausgaben, Fehler oder heruntergefahrene Komponenten angezeigt werden, starten Sie die entsprechenden Services entsprechend neu
    
    *   Datenbank und Listener
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   DBSec - Lab-Service
        
            <copy>
            sudo systemctl restart oracle-dbsec-lab
            </copy>
            

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Anhang 1: Startup-Services verwalten

1.  Datenbankservices (alle Datenbanken und Standard-Listener)
    
    *   Starten
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   Beenden
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   Status
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   Neustart
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  DBSec - Lab Service (Enterprise Manager 13c und My HR Applications auf Glassfish)
    
    *   Starten
    
        <copy>sudo systemctl start oracle-dbsec-lab</copy>
        
    
    *   Beenden
    
        <copy>sudo systemctl stop oracle-dbsec-lab</copy>
        
    
    *   Status
    
        <copy>sudo systemctl status oracle-dbsec-lab</copy>
        
    
    *   Neustart
    
        <copy>sudo systemctl restart oracle-dbsec-lab</copy>
        

## Anhang 2: Externer Webzugriff

Wenn Sie sich aus irgendeinem Grund von einem Standort aus anmelden möchten, der außerhalb Ihrer Remote-Desktopsession liegt, wie z.B. Ihrer Workstation/Ihrem Laptop, lesen Sie die folgenden Details.

1.  Enterprise Manager 13c-Konsole
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
        URL: <copy>http://<Your Instance public_ip>:7803/em</copy>
        
    
    *   _Hinweis:_ Möglicherweise wird beim Zugriff auf die Webkonsole ein Fehler im Browser angezeigt: "_Ihre Verbindung ist nicht privat_", wie unten gezeigt. Ignorieren Sie die Ausnahme, und fügen Sie sie hinzu, um fortzufahren.
    
    ![Externe Anmeldung bei Enterprise Manager](images/login-em-external-1.png "Externe Anmeldung bei Enterprise Manager") ![Externe Anmeldung bei Enterprise Manager](images/login-em-external-2.png "Externe Anmeldung bei Enterprise Manager")
    
2.  Meine HR-Anwendungen auf Glassfish
    
    *   PDB1
        *   Produkt: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb1`
        *   Entwicklung: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb1` (bg: rot)
    *   PDB2
        *   Produkt: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb2` (Menü: rot)
        *   Dev : `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb2` (bg: rot & Menü: rot)

## Danksagungen

*   **Autor** - Rene Fontcha, LiveLabs Platform Lead, NA-Technologie
*   **Mitwirkende** - Hakim Loumi
*   **Zuletzt aktualisiert am/um** - Marion Smith, Technical Program Manager, April 2022