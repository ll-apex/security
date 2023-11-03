# Umgebung initialisieren

## Einführung

In dieser Übung prüfen und richten wir alle Komponenten ein, die für ein erfolgreiches Upgrade von IAM 11.1.2.3 auf die Version 12.2.1.4 erforderlich sind.

_Geschätzte Laborzeit_: 30 Minuten

### Ziele

*   Initialisieren Sie die IAM-Baselineworkshopumgebung 11.1.2.3.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup

## Aufgabe 1: Prüfen, ob erforderliche Prozesse hochgefahren und gestartet sind

1.  Nachdem Sie nun Zugriff auf Ihre Remote-Desktopsession haben, gehen Sie wie unten angegeben vor, um Ihre Umgebung zu validieren, bevor Sie mit der Ausführung der nachfolgenden Übungen beginnen. Die folgenden Prozesse müssen ausgeführt werden:
    
    *   Datenbank-Listener
        *   LISTENER
    *   Datenbankserverinstanz
        *   orcl
    
    ![Remote-Desktop-Landingpage](./images/login.png " ")
    
2.  Führen Sie in der _Terminal_\-Session den folgenden Befehl aus, um zu prüfen, ob die erwarteten Prozesse hochgefahren sind.
    
    *   Datenbankservice
        
            <copy>
            systemctl status oracle-database.service
            </copy>
            
        
        ![Prüfung des Terminal-DB-Dienststatus](./images/db-service-status.png " ")
        
    *   IAM-Service
        
            <copy>
            systemctl status oracle-iam.service
            </copy>
            
        
        ![Prüfung des Terminal-IAM-Dienststatus](./images/iam-service-status.png " ")
        
    
    Wenn alle erwarteten Prozesse in der Ausgabe wie oben dargestellt angezeigt werden, ist Ihre Umgebung für die nächste Aufgabe bereit.
    
3.  Testen Sie die Basisinstallation, indem Sie auf die _Identity Manager-Admin-Konsole_ zugreifen, die im Fenster _Chrome_ Ihrer Remote-Desktopsession vorab gestartet wurde. Verwenden Sie dazu die folgenden Zugangsdaten:
    
        URL: <copy>http://wsidmhost.idm.oracle.com:7778/oim</copy>
        
    
        User: <copy>xelsysadm</copy>
        
    
        Password: <copy>IAMUpgrade12c##</copy>
        
    
    ![Anmeldung bei Identity Manager-Admin-Konsole](./images/login1.png " ")
    
    ![Homepage der Identity Manager-Admin-Konsole](./images/oim-landing.png " ")
    

## Aufgabe 2: ReadMe.txt und Binärdateien prüfen

1.  Weitere Informationen zum Setup finden Sie in den Umgebungsdetails. Navigieren Sie wie unten gezeigt zum Dateibrowser, und öffnen Sie _ReadMe.txt_.
    
    ![Prüfen Sie readme.txt](./images/review.png " ")
    
2.  Alle Softwarebinärdateien, die während des gesamten Workshops benötigt werden, wurden zu Ihrer Bequemlichkeit auf der Instanz bereitgestellt. Weitere Informationen finden Sie unten.
    
    *   12c Binärdateien prüfen
    
        <copy>ls -ltrh /home/oracle/Downloads/12cbits </copy>
        
    
    *   11g Binärdateien prüfen
    
        <copy>ls -ltrh /home/oracle/Downloads/11gbits </copy>
        
    
    ![Ordnerstruktur von Binärdateien](./images/review2.png " ")
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Anhang 1: Startup-Services verwalten

1.  Datenbankservice (Datenbank- und Standard-Listener).
    
    *   Starten
    
        <copy>
        sudo systemctl start oracle-database
        </copy>
        
    
    *   Beenden
    
        <copy>
        sudo systemctl stop oracle-database
        </copy>
        
    
    *   Status
    
        <copy>
        systemctl status oracle-database
        </copy>
        
    
    *   Neustart
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
2.  IAM-Service
    
    *   Starten
    
        <copy>
        sudo systemctl start oracle-iam.service
        </copy>
        
    
    *   Beenden
    
        <copy>
        sudo systemctl stop oracle-iam.service
        </copy>
        
    
    *   Status
    
        <copy>
        systemctl status oracle-iam.service
        </copy>
        
    
    *   Neustart
    
        <copy>
        sudo systemctl restart oracle-iam.service
        </copy>
        

## Weitere Informationen

Über diese Links erhalten Sie weitere Informationen zu Oracle Identity and Access Management:

*   [Oracle Identity Management 12.2.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html).

## Danksagungen

*   **Autor** - Anbu Anbarasu, Direktor, Cloud Platform COE
*   **Mitwirkende** - Eric Pollard - Sustaining Engineering, Ajith Puthan - IAM Support, Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Sahaana Manavalan, LiveLabs Developer, NA Technology, Mai 2022