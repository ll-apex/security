# Oracle-Berechtigungsanalyse

## Einführung

In diesem Workshop wird die Funktionalität von Oracle Privilege Analysis vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie Sie dieses Feature verwenden, um die Verwendung von Berechtigungen zu kennen, auf die alle Benutzer während der gesamten Datenbanklebensdauer zugreifen.

_Geschätzte Laborzeit:_ 15 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Vorschau von "_LiveLabs - Oracle Privilege Analysis (Mai 2022)_" ansehen[](youtube:OsvxpBIKoOQ)

### Ziele

*   Workload einer Datenbank erfassen
*   Bericht zur Berechtigungsanalyse generieren, um alle Benutzer-/Systemberechtigungen zu kennen, die während dieser Erfassung verwendet werden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Arbeitsfläche zur Analyse erfassen | 5 Minuten |
| 2 | Analysieren der Arbeitslast | 5 Minuten |
| 3 | Aufnahme ablegen | <5 Minuten |

## Aufgabe 1: Zu analysierende Workload erfassen

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/priv-analysis</copy>
        
3.  Stellen Sie zunächst sicher, dass der Benutzer über die Rolle "`CAPTURE_ADMIN`" verfügt, und erstellen Sie die Erfassung der Berechtigungsanalyse.
    
        <copy>./pa_create_capture.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-001.png "Berechtigungsanalyse-Capture erstellen")
    
4.  Als Nächstes starten Sie den Capture-Vorgang
    
        <copy>./pa_enable_capture.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-002.png "Capture starten")
    
    **Hinweis**: Dadurch werden alle verwendeten Berechtigungen und/oder Rollen erfasst.
    
5.  Generieren Sie einige Workloads, sodass wir nicht verwendete Rollen und Berechtigungen verwendet haben
    
        <copy>./pa_generate_workload.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-003.png "Workload generieren")
    
6.  Wir können die Erfassung deaktivieren, wenn wir der Meinung sind, dass genügend Daten zur Verfügung stehen
    
        <copy>./pa_disable_capture.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-004.png "Erfassung deaktivieren")
    

## Aufgabe 2: Erfasste Workload analysieren

1.  Berichte generieren
    
        <copy>./pa_generate_report.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-005.png "Bericht generieren")
    
    **Hinweis**:
    
    *   Er verwendet alle Berechtigungen und Rollen, die bei der Erfassung als verwendet identifiziert wurden, und vergleicht sie mit den Rollen und Berechtigungen, die jedem Benutzer erteilt wurden.
    *   Die Generierung kann je nach zu verarbeitendem Volume einige Minuten dauern
2.  Zeigen Sie als Nächstes die Berichtsergebnisse an, indem Sie die mit der Capture-Ausgabe verknüpften Ansichten abfragen.
    
        <copy>./pa_review_report.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-006.png "Auswertungsergebnisse")
    
    **Hinweis**:
    
    *   Sie können alle Berechtigungen (System und Objekte) anzeigen, die von allen aktiven Benutzern während der Erfassung verwendet und nicht verwendet wurden
    *   Dieser Schritt ist wichtig, um besser zu verstehen, was in diesem Zeitraum auf Ihrer Datenbank passiert ist, um festzustellen, ob Ihre Benutzer ihre eigenen Berechtigungen richtig verwenden oder ob Sie einige nicht wesentliche widerrufen müssen, um ein Missbrauchsrisiko zu vermeiden, insbesondere während eines Identitätsdiebstahls
    *   Beachten Sie, dass Sie diese Berechtigungsanalyseaufgabe so oft wie nötig ausführen können... Tatsächlich **wird dringend empfohlen, dies so oft wie möglich zu tun**, um immer die Kontrolle über die Aktivitätsrechte Ihrer Benutzer zu behalten und jeden Versuch potenzieller Angreifer zur Erhöhung von Berechtigungen zu vermeiden.
3.  Öffnen Sie jetzt die DB-Administrationskonsole (OEM Cloud Control), um denselben Bericht besser anzuzeigen
    
    *   Öffnen Sie einen Webbrowser unter der URL _`https://dbsec-lab:7803/em`_
        
        **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`https://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:7803/em`_ aufrufen.
        
    *   Melden Sie sich als _`SYSMAN`_ mit dem Kennwort "_`Oracle123`_" bei der Oracle Enterprise Manager 13c-Konsole an.
        
            <copy>SYSMAN</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![Berechtigungsanalyse](./images/pa-007.png "OEM - Anmeldebildschirm")
        
    *   Blenden Sie alle Datenbanken ein, und klicken Sie auf **cdb\_PDB1**
        
        ![Berechtigungsanalyse](./images/pa-008.png "OEM - Zielübersicht")
        
    *   Wählen Sie im Menü die Optionen **Sicherheit** und **Berechtigungsanalyse** aus.
        
        ![Berechtigungsanalyse](./images/pa-009.png "OEM - Menü "Berechtigungsanalyse"")
        
    *   Aktivieren Sie "**Benannt**", und wählen Sie "**PA\_ADMIN**" als Zugangsdatenname aus
        
        ![Berechtigungsanalyse](./images/pa-010.png "OEM - Anmeldung bei Berechtigungsanalyse")
        
    *   Klicken Sie auf \[**Anmelden**\].
        
    *   Wählen Sie den während der Erfassung generierten Bericht aus (hier "**Alle Datenbankerfassung**"), und klicken Sie auf \[**Bericht anzeigen**\]
        
        ![Berechtigungsanalyse](./images/pa-011.png "OEM - Berechtigungsanalyse erfasst")
        
    *   Sie können alle Benutzer anzeigen, die während des Erfassungszeitraums mit der Datenbank verbunden sind, sowie die verwendeten Berechtigungen
        
        ![Berechtigungsanalyse](./images/pa-012.png "OEM - Globale Berechtigungsanalyse - Bericht")
        
    *   Klicken Sie auf die Registerkarte "**Verwendet**", um alle Berechtigungen anzuzeigen, die während der Erfassung verwendet werden, und von wem
        
        ![Berechtigungsanalyse](./images/pa-013.png "OEM - Verwendete Berechtigungsanalyse (Bericht)")
        
        **Hinweis:** Mit der Schaltfläche "In Tabelle exportieren" können Sie diese Informationen Personen zur Verfügung stellen, die Entscheidungen über die Erteilung oder den Entzug von Rechten treffen können.
        
    *   Sie können auch nicht verwendete Berechtigungen anzeigen - dies können Berechtigungen sein, die Konten nicht benötigen. Wenn ja, ist das Entfernen dieser unnötigen Berechtigungen eine hervorragende Möglichkeit, den Schaden, den diese Konten verursachen könnten, zu verringern, wenn sie kompromittiert würden. Klicken Sie auf die Registerkarte "**Nicht verwendet**", um alle Berechtigungen anzuzeigen, die während der Erfassung nicht verwendet wurden und von denen
        
        ![Berechtigungsanalyse](./images/pa-014.png "OEM - Bericht "Berechtigungsanalyse - Nicht verwendet"")
        
        **Hinweis:** Wir können deutlich erkennen, ob ein Benutzer über zu viele Rechte oder Rechte verfügt, die er für seine Aufgaben nicht benötigt.
        

## Aufgabe 3: Capture löschen

1.  Sobald wir unseren Bericht überprüft haben und wir mit der Berechtigungsanalyse zufrieden sind, können wir die von uns erstellte Erfassung löschen
    
        <copy>./pa_drop_capture.sh</copy>
        
    
    ![Berechtigungsanalyse](./images/pa-015.png "Aufnahme ablegen")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Die Berechtigungsanalyse erhöht die Sicherheit Ihrer Anwendungen und Datenbankvorgänge, indem sie Ihnen hilft, Best Practices mit geringsten Berechtigungen für Datenbankrollen und -berechtigungen zu implementieren.

Die Berechtigungsanalyse, die im Oracle Database-Kernel ausgeführt wird, reduziert die Angriffsfläche von Benutzer-, Tooling- und Anwendungsaccounts, indem sie verwendete und nicht verwendete Berechtigungen zur Implementierung des Modells mit den geringsten Berechtigungen identifiziert.

![Berechtigungsanalyse](./images/pa-concept.png "Berechtigungsanalyse - Konzept")

Die Berechtigungsanalyse erfasst Berechtigungen, die von Datenbankbenutzern und Anwendungen verwendet werden, dynamisch. Die Verwendung von Berechtigungsanalysen kann dazu beitragen, Richtlinien mit geringsten Berechtigungen schnell und effizient durchzusetzen. Im Modell mit den geringsten Berechtigungen erhalten Benutzer nur die Berechtigungen und den Zugriff, die sie für ihre Aufgaben benötigen. Auch wenn Benutzer verschiedene Aufgaben ausführen, erhalten Benutzer häufig dieselben leistungsstarken Berechtigungen. Ohne Berechtigungsanalyse kann es schwierig sein, die Berechtigungen zu ermitteln, die jeder Benutzer haben muss, und in vielen Fällen können Benutzer einige gemeinsame Berechtigungen erhalten, obwohl sie unterschiedliche Aufgaben haben. Selbst in Organisationen, die Berechtigungen verwalten, sammeln Benutzer im Laufe der Zeit Berechtigungen und verlieren selten Berechtigungen. Die Aufgabentrennung unterteilt einen einzelnen Prozess in separate Aufgaben für verschiedene Benutzer. Mit den geringsten Berechtigungen wird die Trennung erzwungen, sodass Benutzer nur die erforderlichen Aufgaben ausführen können. Die Durchsetzung der Aufgabentrennung ist für die interne Kontrolle von Vorteil, reduziert jedoch auch das Risiko von böswilligen Benutzern, die privilegierte Zugangsdaten stehlen.

Die Berechtigungsanalyse erfasst Berechtigungen, die von Datenbankbenutzern und Anwendungen zur Laufzeit verwendet werden, und schreibt ihre Ergebnisse in Data Dictionary Views, die Sie abfragen können. Wenn Ihre Anwendungen die Rechte des Eigentümers und die Rechte des ausführenden Benutzers enthalten, erfasst die Berechtigungsanalyse die Berechtigungen, die zum Kompilieren und Ausführen einer Prozedur erforderlich sind, selbst wenn die Prozedur vor dem Erstellen und Aktivieren der Berechtigungserfassung kompiliert wurde.

Sie können verschiedene Typen von Berechtigungsanalyse-Policys erstellen, um bestimmte Ziele zu erreichen:

*   **Erfassung der rollenbasierten Berechtigungsverwendung**
    
        You must provide a list of roles. If the roles in the list are enabled in the database session, then the used privileges for the session will be captured. You can capture privilege use for the following types of roles: Oracle default roles, user-created roles, Code Based Access Control (CBAC) roles, and secure application roles.
        
*   **Erfassung der kontextbasierten Berechtigungsverwendung**
    
        You must specify a Boolean expression only with the `SYS_CONTEXT` function. The used privileges will be captured if the condition evaluates to `TRUE`. This method can be used to capture privileges and roles used by a database user by specifying the user in `SYS_CONTEXT`.
        
*   **Erfassung der rollen- und kontextbasierten Berechtigungsverwendung**
    
        You must provide both a list of roles that are enabled and a `SYS_CONTEXT` Boolean expression for the condition. When any of these roles is enabled in a session and the given context condition is satisfied, then privilege analysis starts capturing the privilege use.
        
*   **Datenbankweite Berechtigungserfassung**
    
        If you do not specify any type in your privilege analysis policy, then the used privileges in the database will be captured, except those for the user `SYS`. (This is also referred to as unconditional analysis, because it is turned on without any conditions.)
        

### **Vorteile der Berechtigungsanalyse**

*   Ermitteln unnötig erteilter Berechtigungen
*   Best Practices für geringste Berechtigungen implementieren: Die Berechtigungen des Accounts, der auf eine Datenbank zugreift, sollten auf die Berechtigungen beschränkt sein, die von der Anwendung oder dem Benutzer unbedingt benötigt werden
*   Entwicklung sicherer Anwendungen: Während der Anwendungsentwicklungsphase können einige Administratoren Anwendungsentwicklern viele leistungsstarke Systemberechtigungen und -rollen erteilen
*   Sie können Berechtigungsanalyse-Policys in einer mehrmandantenfähigen Umgebung erstellen und verwenden
*   Kann zur Erfassung der Berechtigungen verwendet werden, die für vorkompilierte Datenbankobjekte ausgeübt wurden (PL/SQL-Packages, Prozeduren, Funktionen, Views, Trigger sowie Java-Klassen und -Daten)

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Berechtigungsanalyse 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/performing-privilege-analysis-find-privilege-use.html#GUID-44CB644B-7B59-4B3B-B375-9F9B96F60186)

Video:

*   _Berechtigungsanalyse (Januar 2019)_[](youtube:3oRODVtWwbg)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Richard Evans
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023