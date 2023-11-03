# Aufgaben nach dem Upgrade

## Einführung

In dieser Übung werden die Schritte zum Neustarten der Server und zum Abschließen des One-Hop-Upgradeprozesses beschrieben.

_Geschätzte Laborzeit_: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Starten Sie die Server neu, um das Upgrade abzuschließen
*   Prüfen des Upgradeprozesses

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Pre-Upgrade-Anforderungen
    *   Übung: One-Hop-Upgrade

## Aufgabe 1: 12c-Admin-Server und Managed Server starten

1.  Navigieren Sie zum 12c-Domainverzeichnis, und starten Sie die Server in der unten angegebenen Reihenfolge
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
2.  Admin-Server starten
    
        <copy>nohup ./startWebLogic.sh &</copy>
        
3.  Wenn der Admin Server hochgefahren ist, greifen Sie über Ihren Browser auf die Weblogic-Konsole zu. Klicken Sie auf das Lesezeichen _Workshop-Links_, und klicken Sie auf _WLS12c_ "ODER", um die folgende URL in den Browser einzufügen:
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  Klicken Sie in der Weblogic-Konsole unter _Umgebung_ auf _Server_, und beachten Sie, dass alle Server (OIM,SOA) den Status "SHUTDOWN" aufweisen.
    
5.  Öffnen Sie eine weitere Registerkarte im Terminal. Navigieren Sie zum Pfad _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_, und starten Sie den SOA-Server.
    
        <copy>nohup ./startManagedWebLogic.sh soa_server1 t3://onehopiam:7005 -Dbpm.enabled=true &</copy>
        
    
    Aktualisieren Sie die Weblogic-Konsole, und beachten Sie, dass sich der SOA-Server jetzt im Status "RUNNING" befindet. Der Start des SOA-Servers kann etwa 5-8 Minuten dauern.
    
6.  Öffnen Sie eine weitere Registerkarte im Terminal. Navigieren Sie zum Pfad _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_, und starten Sie den OIM-Server.
    
        <copy>nohup ./startManagedWebLogic.sh oim_server1 t3://onehopiam:7005 &</copy>
        
    
    Dieses Mal wird der OIM-Bootstrap-Prozess ausgeführt. Nach dem erfolgreichen Bootstrap wird OIM Managed Server automatisch heruntergefahren.
    
7.  Nachdem der OIM-Server automatisch heruntergefahren wurde, stellen Sie sicher, dass keine OIM-Prozesse ausgeführt werden, indem Sie den folgenden Befehl absetzen:
    
        <copy>ps -ef |grep oim_server1</copy>
        
8.  Stoppen Sie den SOA-Server. Navigieren Sie zum Pfad _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_, und stoppen Sie den SOA-Server.
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
9.  Admin-Server stoppen
    
        <copy>./stopWebLogic.sh</copy>
        

## Aufgabe 2: Upgradeprozess prüfen

1.  Führen Sie das Skript _startDomain12c.sh_ aus, um die 12c-Domain neu zu starten. Der Start des Admin-Servers dauert etwa 3-4 Minuten. Der Start der SOA- und OIM-Server kann etwa 10 Minuten dauern.
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./startDomain12c.sh</copy>
        
2.  Greifen Sie über Ihren Browser auf die Weblogic-Konsole 12c zu. Klicken Sie auf das Lesezeichen _Workshop-Links_, und klicken Sie auf _WLS12c_ "ODER", um die folgende URL in den Browser einzufügen:
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    Klicken Sie in der Weblogic-Konsole unter _Umgebung_ auf _Server_, und prüfen Sie, ob alle Server (OIM,SOA) den Status "RUNNING" aufweisen.
    
3.  Greifen Sie auf die Identity Self Service-Konsole zu. Klicken Sie auf das Lesezeichen _Workshop-Links_, und klicken Sie auf _OIG12c_ "ODER", um die folgende URL in den Browser einzufügen:
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  Klicken Sie in der oberen rechten Ecke auf _xelsysadm_, und klicken Sie in der Dropdown-Liste auf _Info_. Prüfen Sie, ob die OIM-Version 12c ist
    
    ![](images/1-identity.png)
    
5.  Klicken Sie oben rechts auf _Verwalten_. Klicken Sie anschließend auf _Benutzer_, und beachten Sie, dass die drei Benutzer _TUSER1, TUSER2, TUSER3_ von 11g zu 12c migriert werden.
    
    ![](images/2-users.png)
    

Ein One-Hop-Upgrade auf Oracle Identity Manager 12c ist abgeschlossen.

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Juni 2021