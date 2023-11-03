# Datenbankbenutzer bewerten

## Einführung

Mit der Benutzerbewertung können Sie die Sicherheit Ihrer Datenbankbenutzer bewerten und potenzielle Benutzer mit hohem Risiko identifizieren. Standardmäßig generiert Oracle Data Safe automatisch Benutzerbewertungen für Ihre Zieldatenbanken und speichert sie in der Bewertungshistorie. Sie können Bewertungsdaten in allen Zieldatenbanken und für jede Zieldatenbank analysieren. Sie können die Sicherheitsabweichung in Ihren Zieldatenbanken überwachen, indem Sie die letzte Bewertung mit einer Baseline oder einer anderen Bewertung vergleichen.

In dieser Übung untersuchen Sie die Benutzerbewertung.

Geschätzte Zeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Übersichtsseite für Benutzerbewertung anzeigen
*   Benutzer in der neuesten Benutzerbewertung analysieren
*   Benutzer und Berechtigungen in der Zieldatenbank ändern
*   Letzte Benutzerbewertung aktualisieren
*   Benutzerbewertungshistorie für die Zieldatenbank anzeigen
*   Letzte Benutzerbewertung mit anfänglicher Benutzerbewertung vergleichen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet
*   Zieldatenbank bei Oracle Data Safe registriert

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Datenbankbenutzer bewerten](videohub:1_fvykqng1)

## Aufgabe 1: Übersichtsseite für Benutzerbewertung anzeigen

1.  Navigieren Sie zu **Benutzerbewertung**. Klicken Sie dazu im Navigationspfad oben auf der Seite auf **Security Center**. Klicken Sie links auf **Benutzerbewertung**.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
3.  Prüfen Sie oben auf der Überblickseite die vier Diagramme.
    
    *   Das Diagramm **Potenzielles Benutzerrisiko** zeigt die Anzahl und den Prozentsatz der Benutzer an, die potenziell **Kritisches Risiko**, **Hohes Risiko**, **Mitteles Risiko** und **Niedriges Risiko** sind.
    *   Im Diagramm **Benutzerrollen** wird die Anzahl der Benutzer mit den Rollen **DBA**, **DV-Admin** und **Auditadministrator** angezeigt.
    *   Im Diagramm **Last Password Change** wird die Anzahl der Benutzer angezeigt, die ihre Kennwörter in den letzten 30 Tagen, in den letzten 30 bis 90 Tagen oder vor mehr als 90 Tagen geändert haben.
    *   Das Diagramm **Letzte Anmeldung** zeigt die Anzahl der Benutzer an, die sich innerhalb der letzten 24 Stunden, innerhalb der letzten Woche, im aktuellen Monat, im aktuellen Jahr oder vor mindestens einem Jahr angemeldet haben.
    
    ![Benutzerbewertung - Übersichtsseite - Erste drei Diagramme](images/ocw/ua-dashboard-charts1.png "Benutzerbewertung - Übersichtsseite - Erste drei Diagramme")
    
    ![Letztes Diagramm der Benutzerbewertungsübersichtsseite](images/ocw/ua-dashboard-charts2.png "Letztes Diagramm der Benutzerbewertungsübersichtsseite")
    
4.  Prüfen Sie die Registerkarte **Risk Summary**.
    
    *   Die Registerkarte **Risikoübersicht** konzentriert sich auf potenzielle Risiken in allen ausgewählten Zieldatenbanken. Sie zeigt potenzielle Risikostufen, die Anzahl der Zieldatenbanken, die Gesamtanzahl der Benutzer auf jeder Risikostufe, die Gesamtanzahl der privilegierten Benutzer auf jeder Risikostufe und die Anzahl der DBAs, DV-Administratoren und Auditadministratoren an.
    *   Potenzielle Risikostufen werden als **Kritisch**, **Hoch**, **Mittel** und **Niedrig** kategorisiert.
    
    ![Registerkarte "Benutzerbewertungsrisiko - Übersicht"](images/ocw/ua-risk-summary-tab.png "Registerkarte "Benutzerbewertungsrisiko - Übersicht"")
    
5.  Klicken Sie auf die Registerkarte **Zielübersicht**. Diese Registerkarte enthält die folgenden Informationen:
    
    *   Anzahl kritische Benutzer mit hohem Risiko, DBAs, DV-Administratoren und Auditadministratoren
    *   Datum und Uhrzeit der letzten Benutzerbewertung
    *   Gibt an, ob die letzte Benutzerbewertung von der Baseline abweicht (sofern eine festgelegt ist)
    
    ![Registerkarte "Benutzerbewertungsziel - Übersicht"](images/ocw/ua-target-summary-tab.png "Registerkarte "Benutzerbewertungsziel - Übersicht"")
    

## Aufgabe 2: Benutzer in der neuesten Benutzerbewertung analysieren

Die letzte Benutzerbewertung wurde bei der Registrierung der Zieldatenbank automatisch von Oracle Data Safe generiert.

1.  Klicken Sie auf der Registerkarte **Zielübersicht** auf **Bericht anzeigen**, um die neueste Benutzerbewertung für die Zieldatenbank anzuzeigen.
    
2.  Prüfen Sie im Bericht oben auf der Registerkarte **Überblick** die Diagramme **Potenzielles Benutzerrisiko**, **Benutzerrollen**, **Letzte Kennwortänderung** und **Letzte Anmeldung**.
    
    ![Benutzerbewertung - Letzte Diagramme](images/ocw/ua-latest-charts1.png "Benutzerbewertung - Letzte Diagramme")
    
    ![Benutzerbewertung - Letzte Diagramme](images/ocw/ua-latest-charts2.png "Benutzerbewertung - Letzte Diagramme")
    
3.  Klicken Sie auf die Registerkarte **Bewertungsinformationen**, und prüfen Sie die Details.
    
    ![Registerkarte "Bewertungsinformationen"](images/ocw/ua-assessment-information-tab.png "Registerkarte "Bewertungsinformationen"")
    
4.  Scrollen Sie nach unten, und prüfen Sie den Abschnitt **Benutzerdetails**. Diese Tabelle enthält die folgenden Informationen zu jedem Benutzer:
    
    *   Benutzername
    *   Benutzertyp (z.B. PRIVILEGED, SCHEMA)
    *   Angabe, ob der Benutzer ein DBA, ein DV-Administrator oder ein Auditadministrator ist
    *   Potenzielles Risikoniveau (z.B. LOW, HIGH oder CRITICAL)
    *   Status des Benutzers (z.B. OPEN, LOCKED oder EXPIRED\_AND\_LOCKED)
    *   Datum und Zeit der letzten Anmeldung des Benutzers bei der Zieldatenbank
    *   Benutzerprofil
    *   Auditdatensätze für den Benutzer
    
    ![Letzte Bewertungsdetails der Benutzerbewertung](images/ocw/ua-latest-assessment-details.png "Letzte Bewertungsdetails der Benutzerbewertung")
    
5.  Klicken Sie in der Spalte **Benutzername** auf einen Benutzer, der ein potenzielles **Kritisches** Risiko darstellt, z.B. **EVIL\_RICH**.
    
    Im Bereich **Benutzerdetails** werden die folgenden Informationen zum Benutzer angezeigt:
    
    *   Zieldatenbankname
    *   Benutzername
    *   Benutzerprofil
    *   Benutzertyp (z.B. PRIVILEGED)
    *   Status (z.B. OPEN)
    *   Potenzielles Risiko (z.B. KRITISCH) - Bewegen Sie den Mauszeiger über das **i**, um anzuzeigen, was einen Benutzer mit kritischem Risiko ausmacht.
    *   Datum und Uhrzeit der letzten Anmeldung
    *   Datum und Uhrzeit der Erstellung des Benutzers
    *   Datum und Uhrzeit der Kennwortänderung
    *   Privilegierte Rollen (die Admin-Rollen, die dem Benutzer erteilt wurden)
    *   Rollen: Blenden Sie **Alle Rollen** ein, um alle Rollen anzuzeigen, die dem Benutzer erteilt wurden.
    *   Berechtigungen: Blenden Sie **Alle Berechtigungen** ein, um alle Berechtigungen anzuzeigen, die dem Benutzer erteilt wurden.
    
    ![ADMIN-Benutzerdetails](images/ocw/ua-admin-user-details.png "ADMIN-Benutzerdetails")
    
6.  Klicken Sie auf **Schließen**.
    
7.  Um den Bericht so zu filtern, dass nur potenziell kritische Risikobenutzer angezeigt werden, gehen Sie wie folgt vor: Klicken Sie auf die Registerkarte **Überblick**. Klicken Sie im Diagramm **Potenzielles Benutzerrisiko** auf den Abschnitt **Kritisch** des Diagramms. Ein Filter wird automatisch erstellt.
    
    ![Kritisches Risiko - Benutzerfilter](images/ocw/ua-critical-risk-users-filter.png "Kritisches Risiko - Benutzerfilter")
    
8.  Klicken Sie zum Entfernen des Filters auf das **X** neben dem Filter.
    

## Aufgabe 3: Benutzer und Berechtigungen in der Zieldatenbank ändern

1.  Rufen Sie das SQL-Arbeitsblatt in **Database Actions** auf.
    
2.  Löschen Sie bei Bedarf das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
3.  Geben Sie im SQL Worksheet die folgenden Befehle ein:
    
        <copy>DROP USER evil_rich;
        CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen** (grüner Kreis mit weißem Pfeil), um die Abfrage auszuführen.
    
    ![Schaltfläche "Skript ausführen"](images/run-script.png "Schaltfläche "Skript ausführen"")
    
5.  Prüfen Sie auf der Registerkarte **Skriptausgabe** unten auf der Seite, ob der Benutzer `EVIL_RICH` gelöscht wurde, der Benutzer `JOE_SMITH` erstellt wurde und die Berechtigung erfolgreich erteilt wurde.
    

## Aufgabe 4: Letzte Benutzerbewertung aktualisieren

1.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück. Sie haben die letzte Benutzerbewertung auf der Seite **Benutzerbewertungsdetails** abgebrochen.
    
2.  Klicken Sie auf die Schaltfläche **Jetzt aktualisieren**.
    
    Der Bereich **Jetzt aktualisieren** wird angezeigt.
    
3.  Behalten Sie den Standardnamen bei, und klicken Sie auf **Jetzt aktualisieren**. Warten Sie, bis der Status der letzten Benutzerbewertung **SUCCEEDED** lautet. Oracle Data Safe speichert automatisch eine statische Kopie der Bewertung in der Bewertungshistorie.
    
    ![Bereich "Jetzt aktualisieren" für Benutzerbewertung](images/ocw/ua-refresh-now-panel.png "Bereich "Jetzt aktualisieren" für Benutzerbewertung")
    
4.  Prüfen Sie die aktualisierte letzte Bewertung.
    

## Aufgabe 5: Letzte Benutzerbewertung mit der ersten Benutzerbewertung vergleichen

Sie können eine Benutzerbewertung auswählen, die mit der letzten Benutzerbewertung verglichen werden soll. Mit dieser Option müssen Sie keine Baseline festlegen. Diese Option ist nur verfügbar, wenn Sie die letzte Benutzerbewertung anzeigen. Beachten Sie, dass Sie möglicherweise eine Baseline festgelegt und die letzte Bewertung mit ihr verglichen haben.

1.  Klicken Sie beim Anzeigen der letzten Benutzerbewertung links unter **Ressourcen** auf **Bewertungen vergleichen**.
    
2.  Scrollen Sie nach unten zum Abschnitt **Vergleich mit anderen Bewertungen**.
    
3.  Wenn Ihr Compartment nicht angezeigt wird, klicken Sie auf **Compartment ändern**, und wählen Sie Ihr Compartment aus.
    
4.  Wählen Sie in der Dropdown-Liste **Bewertung auswählen** die erste Bewertung für die Zieldatenbank (zweite Bewertung in der Liste). Sobald Sie es auswählen, wird der Vergleichsvorgang gestartet.
    
5.  Prüfen Sie die Ergebnisse.
    
    *   Es wurde ein neuer Benutzer hinzugefügt und ein Benutzer gelöscht.
    *   Das Ergebnis "Neuer Benutzer" wird als potenzielles **Kritisches** Risiko identifiziert.
    
    ![Benutzerbewertungsvergleichsbericht](images/ocw/ua-comparison-report2.png "Benutzerbewertungsvergleichsbericht")
    
6.  Klicken Sie in der Spalte **Vergleichsergebnisse** auf einen der Links **Details öffnen**, um weitere Informationen anzuzeigen.
    
    Der Bereich **Vergleichsdetails** wird angezeigt.
    
    ![Fensterbereich "Vergleichsdetails"](images/ocw/ua-comparison-details-panel.png "Fensterbereich "Vergleichsdetails"")
    
7.  Prüfen Sie die Informationen, und klicken Sie auf **Schließen**. An dieser Stelle können Sie eine Baselinebewertung festlegen.
    

## Weitere Informationen

*   [Überblick über Benutzerbewertung](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-6BF46EE2-F7B5-4710-A09C-069EA95F8052)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 17. August 2023