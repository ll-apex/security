# Umgebung initialisieren

## Einführung

In dieser Übung prüfen und starten wir alle Komponenten, die für die erfolgreiche Ausführung dieses Workshops erforderlich sind.

_Geschätzte Laborzeit_: 20 Minuten

### Über Produkt/Technologie

Oracle Identity Governance (OIG) ist ein leistungsstarkes und flexibles Identity Management-System für Unternehmen, das die Zugriffsberechtigungen von Benutzern innerhalb von Unternehmens-IT-Ressourcen automatisch verwaltet.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Initialisieren Sie die Workshop-Umgebung.
*   Prüfen Sie den Datenbankstatus.
*   Prüfen Sie den Status der für das Upgrade verfügbaren 11g-Domain.
*   Prüfen Sie den Status der 12c-Domain.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account

_Hinweis: Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert. Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar. **[Klicken Sie hier, um die häufig gestellte Fragen zu Free Tier aufzurufen.](https://www.oracle.com/cloud/free/faq.html)**_

## Aufgabe 1: Überprüfen Sie, ob die erforderlichen Prozesse hochgefahren und gestartet sind.

1.  Nachdem Sie nun Zugriff auf Ihre Remote-Desktopsession haben, gehen Sie wie unten angegeben vor, um Ihre Umgebung zu validieren, bevor Sie mit der Ausführung der nachfolgenden Übungen beginnen. Die folgenden Prozesse müssen ausgeführt werden:
    
    *   Datenbank-Listener
    *   Datenbankserver
    *   Admin-Server (Admin-Server dauert etwa 3-4 Minuten zum Starten)
    *   OIM- und SOA-Server (SOA- und OIM-Server werden in 10-12 Minuten gestartet)
2.  Wenn nicht, aktualisieren Sie im _Firefox_\-Fenster auf der rechten vorab geladenen Weblogic 11g-Konsole die Seite, oder warten Sie 2-3min, um den Admin-Server zu starten. Klicken Sie dann auf das Feld _Benutzername_, und wählen Sie die gespeicherten Zugangsdaten für die Anmeldung aus. Diese Zugangsdaten wurden in _Firefox_ gespeichert und werden im Folgenden als Referenz bereitgestellt.
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/oig-vnc.png " ")
    
3.  Der Start des Admin-Servers dauert etwa 3-4 Minuten. Es kann etwa 10-12 Minuten dauern, bis die SOA- und OIM-Server gestartet werden.
    
    *   Klicken Sie in der Weblogic 11g-Konsole unter _Umgebung_ auf _Server_, und prüfen Sie, ob alle Server (OIM,SOA) den Status "Wird ausgeführt" aufweisen. ![](images/oig-vnc2.png " ")
        
    *   Im _Firefox_\-Fenster auf der rechten vorgeladenen 3. TAB befindet sich die Weblogic 12c-Konsole.
        
            Username: <copy>weblogic</copy>
            
        
            Password: <copy>Welcom@123</copy>
            
    
    ![](images/oig-vnc3.png " ")
    
    *   Klicken Sie unter _Umgebung_ auf _Server_, und prüfen Sie, ob alle Server (OIM,SOA) den Status "Wird ausgeführt" aufweisen. ![](images/oig-vnc4.png " ")
    
    Wenn der Vorgang erfolgreich war, wird die obige Seite mit der Ausführung des gesamten Servers angezeigt, und Ihre Umgebung ist jetzt bereit.
    
4.  Wenn Sie sich immer noch nicht anmelden können oder die Anmeldeseite nach dem erneuten Laden aus dem Lesezeichenordner _Workshop-Links_ nicht funktioniert, öffnen Sie eine Terminalsession, und fahren Sie wie unten angegeben fort, um die Services zu validieren.
    
    *   Datenbank und Listener
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![](images/1-database.png " ")
        
    *   WLS-Admin-Server, OIM-Server
        
            <copy>
            sudo systemctl status oig-11g.service
            </copy>
            
        
        ![](images/oig-11gservice.png " ")
        
            <copy>
            sudo systemctl status oig-12c.service
            </copy>
            
        
        ![](images/oig-12cservice.png " ")
        

## Aufgabe 2: 11g- und 12c-Domain prüfen

1.  Greifen Sie auf die Identity Self Service-Konsole zu. Klicken Sie auf das Lesezeichen _Workshop-Links_, und klicken Sie auf _OIG11g_ "ODER", um die folgende URL in den Browser einzufügen:
    
        <copy>http://onehopiam:14000/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/5-identity-console.png)
    
2.  Klicken Sie in der oberen rechten Ecke auf _xelsysadm_, und klicken Sie in der Dropdown-Liste auf _Info_. Prüfen Sie, ob die OIM-Version 11g ist
    
    ![](images/6-identity-console.png)
    
3.  Klicken Sie oben rechts auf _Verwalten_. Klicken Sie anschließend auf _Benutzer_, und beachten Sie, dass 3 Testbenutzer erstellt wurden (_TUSER1, TUSER2, TUSER3_)
    
    ![](images/7-users.png)
    
    ![](images/8-users.png)
    

4.  Greifen Sie auf die Identity Self Service OIG-12c-Konsole zu. Klicken Sie auf das Lesezeichen _Workshop-Links_, und klicken Sie auf _OIG12c_ "ODER", um die folgende URL in den Browser einzufügen:
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/11-oim12c.png)
    
5.  Klicken Sie in der oberen rechten Ecke auf _xelsysadm_, und klicken Sie in der Dropdown-Liste auf _Info_. Prüfen Sie, ob die OIM-Version 12c ist
    
    ![](images/12-oim12c.png)
    
6.  Klicken Sie oben rechts auf _Verwalten_. Klicken Sie anschließend auf _Benutzer_, und beachten Sie, dass keine neuen Benutzer erstellt wurden.
    
    ![](images/13-oim12c.png)
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Anhang 1: Startup-Services verwalten

1.  Datenbankservice (Datenbank und Listener).
    
        Start: <copy>sudo systemctl start oracle-database</copy>
        
    
        Stop: <copy>sudo systemctl stop oracle-database</copy>
        
    
        Status: <copy>sudo systemctl status oracle-database</copy>
        
    
        Restart: <copy>sudo systemctl restart oracle-database</copy>
        
2.  OIG-Service - 11g-Version (WLS-Admin-Server OIG-Server)
    
        Start: <copy>sudo systemctl start oig-11g.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-11g.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-11g.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-11g.service</copy>
        
3.  OIG-Service - 12c-Version (WLS-Admin-Server OIG-Server)
    
        Start: <copy>sudo systemctl start oig-12c.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-12c.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-12c.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-12c.service</copy>
        

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Ashish Kumar, NATD Solution Engineering, Juni 2021