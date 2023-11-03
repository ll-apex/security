# Oracle Autonomous Database bereitstellen

## Einführung

In dieser Übung stellen Sie die Oracle Autonomous Database-(ADB-)Instanz bereit und stellen als neuer Benutzer eine Verbindung zur Datenbank her.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen kurzen Spaziergang durch das Labor zu machen.

[](youtube:dhCPeTAErtY)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Oracle Autonomous Transaction Processing-(ATP-)Instanz bereitstellen
*   Neuen Datenbankbenutzer mit Database Actions erstellen
*   Verbindung zur Oracle Autonomous Transaction Processing-Datenbank als neuer Benutzer von SQL Developer Web herstellen

### Voraussetzungen

Dieser Workshop geht davon aus, dass Sie:

*   LiveLabs Cloud-Account

## Aufgabe 1: Verfügbarkeitsinstanzen bereitstellen

1.  Wenn Sie einen kostenlosen Account verwenden und Ressourcen vom Typ "Immer kostenlos" verwenden möchten, müssen Sie sich in einer Region befinden, in der Ressourcen vom Typ "Immer kostenlos" verfügbar sind. Ihre aktuelle Standard-**Region** wird oben rechts auf der Seite angezeigt.
    
    ![Wählen Sie die Region ganz oben rechts auf der Seite aus.](./images/task3-1.png " ")
    
2.  Sobald Sie angemeldet sind, können Sie das Cloud-Services-Dashboard anzeigen, in dem alle Services für Sie verfügbar sind. Klicken Sie auf das Navigationsmenü, suchen Sie **Oracle Database**, und wählen Sie **Autonomous Transaction Processing** (ATP) aus.
    
    **Hinweis:** Sie können auch direkt auf Ihren Autonomous Transaction Processing-Service im Abschnitt **Schnellaktionen** des Dashboards zugreifen.
    
    ![](./images/task3-2.png " ")
    
3.  Wählen Sie im Dropdown-Menü "Compartment" das **Compartment** aus, um die ATP-Instanz zu erstellen. Diese Konsole zeigt an, dass noch keine Datenbanken vorhanden sind. Wenn eine lange Liste von Datenbanken vorhanden ist, können Sie die Liste nach dem **Status** der Datenbanken filtern (Verfügbar, Gestoppt, Beendet usw.). Sie können auch nach **Workload-Typ** sortieren. Hier ist der Workload-Typ **Transaktionsverarbeitung** ausgewählt.
    
    ![](./images/task3-31.png " ") ![](./images/task3-32.png " ")
    
4.  Klicken Sie auf **Autonomous Database erstellen**, um den Instanzerstellungsprozess zu starten.
    
    ![Klicken Sie auf "Create Autonomous Database".](./images/task3-4.png " ")
    
5.  Daraufhin wird der Bildschirm **Autonomous Database erstellen** angezeigt. Geben Sie die Konfiguration der Instanz an:
    
    *   **Compartment**: Wählen Sie ein Compartment für die Datenbank in der Dropdown-Liste aus.
    *   **Anzeigename** - Geben Sie einen unvergesslichen Namen für die Datenbank zu Anzeigezwecken ein. In dieser Übung wird **DEMOATP** als Anzeigename für Oracle Autonomous Database verwendet.
    *   **Database Name** - Verwenden Sie nur Buchstaben und Zahlen, die mit einem Buchstaben beginnen. Die maximale Länge beträgt 14 Zeichen. (Unterstriche werden anfänglich nicht unterstützt.) In dieser Übung wird **DEMOATP** als Datenbankname verwendet.
    
    ![Geben Sie Details ein.](./images/task3-5.png " ")
    
6.  Workload-Typ und Deployment-Typ auswählen und Datenbank konfigurieren:
    
    *   **Workload-Typ auswählen** - Wählen Sie in dieser Übung **Transaktionsverarbeitung** als Workload-Typ aus.
    *   **Deployment-Typ auswählen** - Wählen Sie für diese Übung die Option **Gemeinsam verwendete Infrastruktur** als Deployment-Typ aus.
    *   **Immer kostenlos** - Wenn Ihr Cloud-Account ein Account vom Typ "Immer kostenlos" ist, können Sie diese Option auswählen, um eine immer kostenlose Oracle Autonomous Database zu erstellen. Eine immer kostenlose Datenbank verfügt über 1 CPU und 20 GB Speicher. In dieser Übung empfehlen wir Ihnen, **Immer kostenlos** zu aktivieren.
    *   **Datenbankversion auswählen**: Wählen Sie eine der verfügbaren Datenbankversionen 19c oder 21c aus.
    *   **OCPU-Anzahl** - Anzahl der CPUs für Ihren Service. Lassen Sie, wie es ist. Eine Datenbank vom Typ "Immer kostenlos" verfügt über 1 CPU.
    *   **Storage (TB)** - Speicherkapazität in Terabyte. Lassen Sie, wie es ist. Eine Datenbank vom Typ "Immer kostenlos" verfügt über 20 GB Speicher.
    *   **Automatische Skalierung** - Lassen Sie für diese Übung die Option "Automatische Skalierung" deaktiviert.
    
    ![Wählen Sie die übrigen Parameter aus.](./images/task3-61.png " ") ![](./images/task3-62.png " ")
    
7.  Erstellen Sie Administratorzugangsdaten, wählen Sie Netzwerkzugriff und Lizenztyp aus, und klicken Sie auf **Autonomous Database erstellen**.
    
    *   **Kennwort**: Geben Sie das Kennwort für den **ADMIN**\-Benutzer der Serviceinstanz an. Für diese Übung verwenden wir das Kennwort **_WElcome123##_**.
    *   **Kennwort bestätigen**: Geben Sie das Kennwort erneut ein, um es zu bestätigen. Notieren Sie sich das Kennwort. Geben Sie für diese Übung das Kennwort erneut ein, und bestätigen Sie es: **_WElcome123##_**.
    *   **Netzwerkzugriff auswählen**: In dieser Übung übernehmen Sie die Standardeinstellung **Sicheren Zugriff von überall aus zulassen**.
    *   **Lizenztyp auswählen** - Wählen Sie für diese Übung die Option **Lizenz inklusive** aus.
    
    ![](./images/task3-71.png " ") ![](./images/task3-72.png " ")
    
8.  Ihre Instanz beginnt mit dem Provisioning. In wenigen Minuten wechselt der Status von Provisioning zu Verfügbar. Jetzt ist Ihre Oracle Autonomous Transaction Processing-Datenbank einsatzbereit! Sehen Sie sich hier die Details Ihrer Instanz an, einschließlich Name, Datenbankversion, OCPU-Anzahl und Speichergröße.
    
    ![Homepage der Datenbankinstanz.](./images/task3-81.png " ")
    
    ![Homepage der Datenbankinstanz.](./images/task3-82.png " ")
    

## Aufgabe 2: Neuen Benutzer mit Database Actions erstellen

1.  Klicken Sie auf der Seite mit den DEMOATP-Instanzdetails auf die Registerkarte **Extras**, und wählen Sie **Datenbankaktionen** aus. Eine neue Registerkarte wird geöffnet. ![](./images/task4-1.png " ")
    
2.  Geben Sie den **Benutzernamen - ADMIN** an, und klicken Sie auf **Weiter**. ![](./images/task4-2.png " ")
    
3.  Geben Sie jetzt das **Kennwort - _WElcome123##_** für den ADMIN-Benutzer an, den Sie beim Provisioning der Oracle Autonomous Database-Instanz erstellt haben, und klicken Sie auf **Anmelden**, um sich bei Database Actions anzumelden. ![](./images/task4-3.png " ")
    
4.  Wählen Sie im Menü "Oracle Database Actions" unter "Administration" die Option **Datenbankbenutzer** aus. ![](./images/task4-4.png " ")
    
5.  Klicken Sie auf **Benutzer erstellen**, um einen neuen Benutzer für den Zugriff auf die Datenbank zu erstellen. ![](./images/task4-5.png " ")
    
6.  Geben Sie auf der Seite "Benutzer erstellen" unter der Registerkarte "Benutzer" die folgenden Details an:
    
    *   **Benutzername** - Geben Sie dem neuen Benutzer einen Benutzernamen. Beim Benutzernamen muss die Groß-/Kleinschreibung beachtet werden. In der Übung nennen wir den Benutzer **Benutzername - DEMOUSER**.
    *   **Kennwort**: Geben Sie dem neuen Benutzer ein Kennwort ein, und bestätigen Sie das Kennwort. In dieser Übung geben wir für die Benutzerfreundlichkeit dasselbe Kennwort wie der Admin-Benutzer **Kennwort - _WElcome123##_** und bestätigen das Kennwort.
    *   **Quota für Tablespace DATA**: Legen Sie einen Wert für die Quota für Tablespace DATA für den Benutzer fest. Klicken Sie auf die Dropdown-Liste, und wählen Sie **500M** aus.
    *   **Webzugriff** - Aktivieren Sie das Optionsfeld "Webzugriff", um auf SQL Developer Web zuzugreifen.
    *   **Erweiterte Features für Webzugriff** - Erweitern Sie die erweiterten Features für Webzugriff, und deaktivieren Sie das Optionsfeld "Autorisierung erforderlich", um die Autorisierung für `demouser`\-REST-Services zu deaktivieren.
    
    ![](./images/task4-6.png " ")
    
7.  Suchen Sie auf der Seite "Benutzer erstellen" auf der Registerkarte "Erteilte Rollen" nach allen drei dieser Rollen: **CONNECT**, **RESOURCE**, **DWROLE**. Aktivieren Sie für jedes Kontrollkästchen die Kontrollkästchen "Erteilt" und "Standard".
    
    ![](./images/task4-71.png " ") ![](./images/task4-72.png " ") ![](./images/task4-73.png " ")
    
8.  Klicken Sie auf **Benutzer erstellen**.
    
    ![](./images/task4-81.png " ")
    
    Beachten Sie, dass der neue Benutzer erfolgreich erstellt wurde. ![](./images/task4-82.png " ")
    
9.  Klicken Sie auf die Schaltfläche "Kopieren" für den REST-Link DEMOUSER, und speichern Sie ihn in einem Notizblock. Bearbeiten Sie den Link im Notizblock, indem Sie `/ords/demouser/_sdw/` am Ende des Links entfernen und als Oracle Autonomous Database-URL speichern. Stellen Sie sicher, dass die gespeicherte URL am Ende keinen umgekehrten Schrägstrich aufweist.
    
    Der gespeicherte Link sollte wie folgt aussehen:
    
        https://c7arcf7q2d0tmld-demoatp.adb.us-ashburn-1.oraclecloudapps.com
        
    
    ![](./images/task4-9.png " ")
    

## Aufgabe 3: Verbindung zu Verfügbarkeitsprüfung als neuer Benutzer herstellen

1.  Navigieren Sie zurück zur Registerkarte mit der Oracle Cloud-Konsole. Klicken Sie auf der Seite mit den DEMOATP-Instanzdetails auf die Registerkarte **Extras**, und wählen Sie **Datenbankaktionen** aus. Eine neue Registerkarte wird geöffnet.
    
    ![](./images/task4-1.png " ")
    
2.  Geben Sie den **Benutzernamen - DEMOUSER** ein, und klicken Sie auf **Weiter**. ![](./images/task3-2-new.png " ")
    

3.  Geben Sie auf der Anmeldeseite von Database Actions den **Benutzernamen - DEMOUSER**, das **Kennwort - _WElcome123##_** an, und klicken Sie auf **Anmelden**.
    
    ![](./images/task5-3.png " ")
    
4.  Nachdem Sie sich als DEMOUSER angemeldet haben, klicken Sie auf **SQL**, um zum SQL Developer Worksheet zu navigieren.
    
    ![](./images/task3-4-new.png " ")
    

Herzlichen Glückwunsch! Jetzt sind Sie als DEMOUSER bei der ATP-Instanz angemeldet.

## Danksagungen

*   **Autor** - Anoosha Pilli, Datenbankproduktmanager
*   **Mitwirkende** - Anoosha Pilli, Brianna Ambler, Produktmanagerin, Oracle Database
*   **Zuletzt aktualisiert am/um** - Marion Smith, April 2022