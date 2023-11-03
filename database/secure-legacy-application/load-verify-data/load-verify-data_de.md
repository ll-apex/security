# Laden und Überprüfen der Daten in der Glassfish-Anwendung

## Einführung

In dieser Übung füllen wir die Glassfish-Anwendung mit Daten auf und prüfen dann, ob die HR-Anwendung weiterhin ordnungsgemäß funktioniert. Dies umfasst das Laden der Schemaobjekte `EMPLOYEESEARCH_PROD` in die ATP-Instanz.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Erstellen Sie das `EMPLOYEESEARCH_PROD`\-Schema mit `SQL*Plus` aus dem Glassfish App Server.
*   Aktualisieren Sie die Verbindungszeichenfolge.
*   Starten Sie die Glassfish-Anwendung.
*   Prüfen Sie die Funktionen der HR-App mit der Glassfish-App **öffentliche IP**.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Abschluss der folgenden vorherigen Übungen: Autonomous Database-Instanz konfigurieren, Verbindung zur Legacy-Anwendung Glassfish HR herstellen

## Aufgabe 1: Erstellen Sie das EMPLOYEESEARCH\_PROD-Schema mit SQL\*Plus aus dem Glassfish App Server, und starten Sie die Glassfish-Anwendung.

1.  Führen Sie das Skript `load_app_data.sh` aus, um Daten in die ATP-Instanz zu laden.
    
        <copy>./load_app_data.sh</copy>
        
    
    ![App-Daten laden](images/load-app-data.png)
    
2.  Aktualisieren Sie die Anwendungsverbindungszeichenfolge mit dem Skript `update_app_connection_string.sh`.
    
        <copy>./update_app_connection_string.sh</copy>
        
    
    ![Verbindungszeichenfolge aktualisieren](images/update-connection-string.png)
    
3.  Starten Sie die Glassfish-Anwendung mit dem Skript `startGlassfish.sh`.
    
        <copy>./startGlassfish.sh</copy>
        
    
    ![Glassfish-App starten](images/glassfish-start.png)
    
    _Hinweis: Wenn Sie auf die Produktions- oder Entwicklungslinks klicken, sind diese erst nach dem nächsten Schritt zugänglich, sodass Ingress auf Port 8080 möglich ist_
    

## Aufgabe 2: Prüfen Sie die Funktionen der HR-App mit der öffentlichen IP-Adresse der Glassfish-App.

1.  Minimieren Sie das Cloud Shell-Terminal, und navigieren Sie mit dem Hamburger-Menü unter **Compute>Instanzen** zurück zu Ihrer Glassfish-App-Instanz in OCI.
    
    ![Aktive Instanz](images/instance-running.png)
    
2.  Wählen Sie im Abschnitt **Primäre VNIC** das für Sie erstellte Subnetz aus.
    
    ![Subnetz suchen](images/subnet.png)
    
3.  Wählen Sie unter "Sicherheitslisten" die **Standardsicherheitsliste** für Ihr Subnetz aus.
    
    ![Definition auswählen SL](images/default-list.png)
    
4.  Wählen Sie unter "Ingress-Regeln" die Option **Ingress-Regeln hinzufügen** aus.
    
5.  Geben Sie die Informationen gemäß der Abbildung unten ein, und wählen Sie **Ingress-Regeln hinzufügen** aus.
    
    ![Ingress-Regel hinzufügen](images/add-ingress.png)
    
6.  Navigieren Sie zurück zum Cloud Shell-Terminal. Suchen Sie die Ausgabe des Skripts `startGlassfish.sh`, und suchen Sie die URLs **Production** und **Development**, die Sie am Ende der Ausgabe erhalten haben.
    
    ![Glassfish-App starten](images/glassfish-start.png)
    
    ![myhrapp öffnen](images/front-page-prod.png)
    
    ![myhrapp öffnen](images/front-page-dev.png)
    

**Wenden Sie die folgenden Schritte auf die Produktions- und Entwicklungsumgebungen an:**

7.  Navigieren Sie oben auf der Seite zur Menüleiste, und wählen Sie **Anmelden**. Verwenden Sie die folgenden Anmeldeinformationen, um sich bei der Glassfish-Anwendung anzumelden.
    
        Username:<copy>hradmin</copy>
        
    
        Password:<copy>Oracle123</copy>
        
8.  Prüfen Sie nach der Anmeldung, ob die Daten in der Glassfish-Anwendung gefunden wurden. Navigieren Sie dazu in der Menüleiste auf der rechten Seite zu **Mitarbeiter suchen**. Suchen Sie unter den Suchparametern die Zeile **Aktiv**, und wählen Sie **Aktiv** und dann **Suchen** aus, um die Daten anzuzeigen.
    
    ![Mitarbeiter suchen](images/search-emp.png)
    
    ![Aktives Mitarbeiter auswählen](images/select-active.png)
    
    ![myhrapp öffnen](images/verify-data.png)
    
9.  Wählen Sie im Menü oben auf der Seite die Option **Home**, um zur Homepage zurückzukehren.
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Mitwirkende** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022