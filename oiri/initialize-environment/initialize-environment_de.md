# Umgebung initialisieren

## Einführung

In dieser Übung prüfen und starten wir alle Komponenten, die für die erfolgreiche Ausführung dieses Workshops erforderlich sind.

_Geschätzte Laborzeit_: 20 Minuten

### Über Produkt/Technologie

Oracle Identity Role Intelligence (OIRI) ist ein leistungsstarkes und flexibles Identity Management-System für Unternehmen, das die Zugriffsberechtigungen von Benutzern innerhalb von IT-Ressourcen des Unternehmens automatisch verwaltet.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Initialisieren Sie die Workshop-Umgebung.
*   Prüfen Sie den Datenbankstatus.
*   Prüfen Sie den Status der 12c-Domain.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

## Aufgabe 1: Überprüfen Sie, ob die erforderlichen Prozesse hochgefahren und gestartet sind.

1.  Nachdem Sie nun Zugriff auf Ihre Remote-Desktopsession haben, gehen Sie wie unten angegeben vor, um Ihre Umgebung zu validieren, bevor Sie mit der Ausführung der nachfolgenden Übungen beginnen. Die folgenden Prozesse müssen ausgeführt werden:
    
    *   Datenbank-Listener
    *   Datenbankserver
    *   Admin-Server (Admin-Server dauert etwa 3-4 Minuten zum Starten)
2.  Klicken Sie im Fenster _Webbrowser_ auf der rechten vorgeladenen Weblogic 12c-Konsole auf das Feld _Benutzername_, und wählen Sie die gespeicherten Zugangsdaten für die Anmeldung aus. Diese Zugangsdaten wurden im _Webbrowser_ gespeichert und werden unten zur Referenz bereitgestellt.
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Anmeldeseite der Weblogic-Konsole](images/oiri-vnc.png " ")
    
3.  Bestätigen Sie die erfolgreiche Anmeldung. Beachten Sie, dass es nach dem Instanz-Provisioning etwa 5 Minuten dauert, bis alle Prozesse vollständig gestartet sind.
    
    *   Klicken Sie in der Weblogic-Konsole unter _Umgebung_ auf _Server_, und prüfen Sie, ob der Admin-Server den Status "RUNNING" aufweist. ![Klicken Sie auf Server unter Umgebung](images/oiri-landing.png " ")
    
    Wenn der Vorgang erfolgreich war, wird die obige Seite angezeigt. Daher ist Ihre Umgebung jetzt bereit.
    
    Sie können jetzt [mit der nächsten Übung fortfahren](#next).
    
4.  Wenn Sie sich immer noch nicht anmelden können oder die Anmeldeseite nach dem erneuten Laden von der unten genannten URL nicht funktioniert, öffnen Sie eine Terminalsession, und fahren Sie wie unten angegeben fort, um die Services zu validieren.
    
        ```
        
    
    URL http://oiri.livelabs.oraclevcn.com:7001/console/login/ Benutzername weblogic Passwort Welcome1 \`\`
    
    *   Datenbank und Listener
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![Terminalfensterbefehl zur Validierung des Datenbankstatus](images/db.png " ")
        
    *   WLS-Admin-Server und Node Manager
        
            <copy>
            sudo systemctl status oiri-weblogic.service
            </copy>
            
        
        ![Terminalfensterbefehl zur Validierung des WLS-Admin-Serverstatus](images/oiri-wls-service.png " ")
        
            <copy>
            sudo systemctl status oiri-node.service
            </copy>
            
        
        ![Terminalfensterbefehl zur Validierung des Node Manager-Status](images/oiri-node-service.png " ")
        
5.  Wenn fragwürdige Ausgaben, Fehler oder heruntergefahrene Komponenten angezeigt werden, starten Sie die entsprechenden Services entsprechend neu
    
    *   Datenbank und Listener
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   WLS-Admin-Server und Node Manager
        
            <copy>
            sudo systemctl restart oiri-weblogic.service
            </copy>
            
        
            <copy>
            sudo systemctl restart oiri-node.service
            </copy>
            

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Anhang 1: Startup-Services verwalten

1.  Datenbankservice (Datenbank und Listener).
    
    *   Starten
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   Beenden
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   Status
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   Neustart
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  OIRI-Service (WLS-Admin-Server)
    
    *   Starten
    
        <copy>sudo systemctl start  oiri-weblogic.service</copy>
        
    
    *   Beenden
    
        <copy>sudo systemctl stop oiri-weblogic.service</copy>
        
    
    *   Status
    
        <copy>sudo systemctl status oiri-weblogic.service</copy>
        
    
    *   Neustart
    
        <copy>sudo systemctl restart oiri-weblogic.service</copy>
        
3.  OIRI-Service (Node Manager-Service)
    
    *   Starten
    
        <copy>sudo systemctl start oiri-node.service</copy>
        
    
    *   Beenden
    
        <copy>sudo systemctl stop oiri-node.service</copy>
        
    
    *   Status
    
        <copy>sudo systemctl status oiri-node.service</copy>
        
    
    *   Neustart
    
        <copy>sudo systemctl restart oiri-node.service</copy>
        

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering