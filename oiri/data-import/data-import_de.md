# Daten aus Oracle Identity Governance in OIRI importieren

## Einführung

Beim Datenimport, auch Datenaufnahme genannt, werden Entitydaten aus einer Quelle in die Oracle Identity Role Intelligence-(OIRI-)Datenbank importiert. OIRI verwendet ein Befehlszeilentool für die Datenaufnahme (DING CLI), um Entitydaten aus externen Quellen abzurufen und zu laden, nämlich Oracle Identity Governance-(OIG-)Datenbank oder Flat Files. Im Rahmen des Datenimportprozesses werden Daten aus der Quelle, wie OIG-Datenbank oder Flat Files, in die folgenden Tabellen der OIRI-Datenbank geladen: `USERS`, `APPLICATIONS`, `ACCOUNTS`, `ENTITLEMENTS`, `ASSIGNED_ENTS`, `ROLES`, `ROLE_USER_MSHIP`, `ROLE_ENT_COMPOSI`, `ROLE_HIERARCHY`, `ORGANIZATIONS`.

_Geschätzte Laborzeit_: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Datenimport aus Oracle Identity Governance-Datenbank konfigurieren
*   Trockenen Datenimport ausführen
*   Führen Sie einen tatsächlichen Datenimport aus, um die Entitydaten aus der OIG-Datenbank zu extrahieren und in die OIRI-Datenbanktabellen zu laden
*   Prüfen und prüfen Sie den Datenimportprozess

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Kubernetes-Cluster bereitstellen und OIG-Server starten
    *   Übung: OIRI auf dem lokalen Kubernetes-Knoten bereitstellen

## Aufgabe 1: Dataload-Prozess starten

1.  Kopieren Sie ca.crt aus dem K8S Master.
    
        <copy>cp /etc/kubernetes/pki/ca.crt /nfs/ding/</copy>
        
2.  Führen Sie den ding-cli-Container aus, und aktualisieren Sie die vorhandene Dataload-Konfiguration, um Entitydaten aus der Oracle Identity Governance-Datenbank zu importieren.
    
        <copy>docker exec -it ding-cli bash</copy>
        
    
        <copy>./updateDataIngestionConfig.sh --useoigdbforetl true --entityusersenabled true --entityuserssyncmode full --entityapplicationsenabled true --entityapplicationssyncmode full --useflatfileforetl false</copy>
        
    
    ![Terminal-Docker-Befehle zum Ausführen des ding-cli-Containers und Aktualisieren der vorhandenen Dataload-Konfiguration zum Importieren von Entitydaten](images/data-load.png)
    

## Aufgabe 2: Trockenen Importlauf ausführen

1.  Führen Sie vor dem Datenimport (oder der Datenaufnahme) einen Trockenlauf aus, um zu validieren, ob die Daten in die OIRI-Datenbank passen. Dadurch werden Daten aus der Oracle Identity Governance-Datenbank abgerufen und anhand der Metadaten der OIRI-Datenbank validiert.
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion dry-run /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  Der Importprozess für trockene Daten dauert etwa 8-10 Minuten. Während der Prozess ausgeführt wird, können wir die Pods beobachten, die unter dem Namespace-Ding generiert werden, indem wir den folgenden Befehl in einer anderen Terminalregisterkarte ausführen.
    
        <copy>kubectl get pods -n ding</copy>
        
    
    Beachten Sie, dass der Ding-Treiber nach Abschluss der Dry Run-Aufgabe vom Status _Wird ausgeführt_ auf _Abgeschlossen_ umgestellt wird.
    
3.  Melden Sie sich bei der OIRI-Konsole an, und überwachen Sie den Datenimportprozess wie in den folgenden Schritten gezeigt.
    

## Aufgabe 3: Melden Sie sich bei der OIRI-Benutzeroberfläche an, und validieren Sie den Import trockener Daten.

1.  Melden Sie sich auf der Identity Role Intelligence-Benutzeroberfläche an. Starten Sie ein Browserfenster, und geben Sie die unten genannte URL ein. Ignorieren Sie die Warnmeldung, indem Sie auf _Erweitert_ und dann auf \*Risiko akzeptieren und fortfahren klicken. Die Anmeldeseite des OIRI-Accounts wird angezeigt. Geben Sie den Benutzernamen und das Kennwort ein.
    
        URL       https://oiri.livelabs.oraclevcn.com:30305/oiri/ui/v1/console/
        Username  xelsysadm
        Password  Welcome1
        
    
    ![OIRI-Konsolenhomepage](images/oiri.png)
    
2.  Klicken Sie oben links auf der Seite auf das Menüsymbol "Anwendungsnavigation", und klicken Sie auf _Datenimport_, um die Seite _Datenimport verwalten_ mit einer Liste aller Datenimportaufgaben zu öffnen.
    
    ![Klicken Sie auf das Symbol für das Anwendungsnavigationsmenü oben links auf der Seite.](images/data-import.png)
    
    ![Klicken Sie auf "Datenimport", um die Seite "Datenimport verwalten" zu öffnen](images/manage-data-import.png)
    
3.  Beachten Sie beim Ausführen des Trockenlaufs, dass neben der Aufgabe ein rotes Uhrensymbol angezeigt wird. Sobald der Dry Data Import erfolgreich abgeschlossen wurde, wird ein grünes Häkchen angezeigt und die Schaltfläche _View Results_ wird angezeigt. Klicken Sie für die Aufgabe "Daten importieren" (Probelauf) aus Oracle Identity Governance auf _Ergebnisse anzeigen_. Alternativ können Sie auf den Namen der Datenimportaufgabe klicken. Das Fenster _Ergebnisse anzeigen_ wird mit dem Ergebnis für den Datenimport aus der Oracle Identity Governance-Datenbank angezeigt.
    
    ![Klicken Sie auf "Ergebnisse für Importdaten anzeigen".](images/result-data-import.png)
    
4.  Blenden Sie jede Entity ein, um die Details des Datenimports dieser Entity zu prüfen, z.B. Anzahl doppelter Daten, Gültigkeit des Datasets und Anzahl ungültiger Datentypen.
    
    ![Ergebnisse anzeigen](images/viewresult-data-import.png)
    
    ![Ergebnisse anzeigen - Rolle](images/role-data-import.png)
    
    ![Ergebnisse anzeigen - Anwendung](images/application-data-import.png)
    
    ![Anspruch](images/entitlement-data-import.png)
    
    ![Ergebnisse anzeigen - Benutzer](images/user-data-import.png)
    
5.  Klicken Sie auf "Abbrechen", um das Fenster "Ergebnisse anzeigen" zu schließen.
    

## Aufgabe 4: Datenimport aus Oracle Identity Governance

1.  Führen Sie den tatsächlichen Datenimportprozess aus.
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion start /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  Der Datenimportprozess dauert etwa 8-10 Minuten. Während der Prozess ausgeführt wird, können wir die Pods beobachten, die unter dem Namespace-Ding generiert werden, indem wir den folgenden Befehl in einer anderen Terminalregisterkarte ausführen.
    
        <copy>kubectl get pods -n ding</copy>
        
    
    ![Terminalbefehl zur Überwachung der Pods, die unter dem Namespace-Ding generiert werden](images/getpods-data-import.png)
    
    Beachten Sie, dass der Ding-Treiber nach Abschluss der Dry Run-Aufgabe vom Status _Wird ausgeführt_ auf _Abgeschlossen_ umgestellt wird.
    

## Aufgabe 5: Datenimportaufgabe validieren

1.  Greifen Sie über das Browserfenster auf die OIRI-Konsole zu.
    
2.  Klicken Sie oben links auf der Seite auf das Menüsymbol "Anwendungsnavigation", und klicken Sie auf _Datenimport_, um die Seite "Datenimport verwalten" mit einer Liste aller Datenimportaufgaben zu öffnen.
    
3.  Beachten Sie während des Datenimports, dass neben der Aufgabe ein rotes Uhrensymbol angezeigt wird. Nachdem die Datenimportausführung erfolgreich abgeschlossen wurde, wird ein grünes Häkchen angezeigt. Daraufhin wird die Schaltfläche _"View Results"_ angezeigt. Klicken Sie auf _Ergebnisse anzeigen_ für die Aufgabe "Daten aus Oracle Identity Governance importieren". Alternativ können Sie auf den Namen der Datenimportaufgabe klicken. Das Fenster _Ergebnisse anzeigen_ wird mit dem Ergebnis für den Datenimport aus der Oracle Identity Governance-Datenbank angezeigt.
    
    ![OIRI - Fenster "Ergebnisse anzeigen"](images/view-data-import.png)
    
4.  Blenden Sie jede Entity ein, um die Details des Datenimports dieser Entity zu prüfen.
    
    ![OIRI - Details des Datenimports prüfen](images/review-data-import.png)
    
5.  Klicken Sie auf "Abbrechen", um das Fenster "Ergebnisse anzeigen" zu schließen.
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering, Dezember 2022