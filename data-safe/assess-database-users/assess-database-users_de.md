# Datenbankbenutzer bewerten

## Einführung

Mit der Benutzerbewertung können Sie die Sicherheit Ihrer Datenbankbenutzer bewerten und potenzielle Benutzer mit hohem Risiko identifizieren. Standardmäßig generiert Oracle Data Safe automatisch Benutzerbewertungen für Ihre Zieldatenbanken und speichert sie in der Bewertungshistorie. Sie können Bewertungsdaten in allen Zieldatenbanken und für jede Zieldatenbank analysieren. Sie können die Sicherheitsabweichung in Ihren Zieldatenbanken überwachen, indem Sie die letzte Bewertung mit einer Baseline oder einer anderen Bewertung vergleichen.

In dieser Übung untersuchen Sie die Benutzerbewertung.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Übersichtsseite für Benutzerbewertung anzeigen
*   Benutzer in der neuesten Benutzerbewertung analysieren
*   Prüfen Sie die Auditdatensätze des `ADMIN`\-Benutzers
*   Benutzer auf der Zieldatenbank erstellen
*   Letzte Benutzerbewertung aktualisieren und umbenennen
*   Benutzerbewertungshistorie für die Zieldatenbank anzeigen
*   Letzte Benutzerbewertung mit anfänglicher Benutzerbewertung vergleichen
*   Laden Sie die neueste Benutzerbewertung als PDF-Bericht herunter
*   Benutzerbewertungshistorie für alle Zieldatenbanken anzeigen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))
*   Auditdatenerfassung für die Zieldatenbank in Oracle Data Safe gestartet (siehe [Auditdatenbankaktivität](?lab=audit-database-activity)). Die Erfassung von Auditdaten ist erforderlich, wenn Sie die Auditdatensätze von Benutzern in der Benutzerbewertung anzeigen möchten.

### Annahmen

*   Ihre Datenwerte können sich von den Werten in den Screenshots unterscheiden.

[Datenbankbenutzer bewerten](videohub:1_fvykqng1)

## Aufgabe 1: Übersichtsseite für Benutzerbewertung anzeigen

1.  Navigieren Sie zu **Benutzerbewertung**. Klicken Sie dazu im Navigationspfad oben auf der Seite auf **Security Center**. Klicken Sie links auf **Benutzerbewertung**.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
3.  Prüfen Sie oben auf der Überblickseite die vier Diagramme.
    
    *   Das Diagramm **Potenzielles Benutzerrisiko** zeigt die Anzahl der Benutzer an, die potenziell **Kritisch**, **Hoch**, **Mittel** und **Niedrig** sind.
    *   Im Diagramm **Benutzerrollen** wird die Anzahl der Benutzer mit den Rollen **DBA**, **DV-Admin** und **Auditadministrator** angezeigt.
    *   Im Diagramm **Last Password Change** wird die Anzahl der Benutzer angezeigt, die ihre Kennwörter in den letzten 30 Tagen, in den letzten 30 bis 90 Tagen oder vor mehr als 90 Tagen geändert haben.
    *   Das Diagramm **Letzte Anmeldung** zeigt die Anzahl der Benutzer an, die sich innerhalb der letzten 24 Stunden, innerhalb der letzten Woche, im aktuellen Monat, im aktuellen Jahr oder vor mindestens einem Jahr angemeldet haben.
    
    ![Diagramme der Überblickseite für Benutzerbewertungen](images/ua-dashboard-charts.png "Diagramme der Überblickseite für Benutzerbewertungen")
    
4.  Prüfen Sie die Registerkarte **Risk Summary**.
    
    *   Die Registerkarte **Risikoübersicht** konzentriert sich auf potenzielle Risiken in allen ausgewählten Zieldatenbanken. Sie zeigt potenzielle Risikostufen, die Anzahl der Zieldatenbanken, die Gesamtanzahl der Benutzer auf jeder Risikostufe, die Gesamtanzahl der privilegierten Benutzer auf jeder Risikostufe und die Anzahl der DBAs, DV-Administratoren und Auditadministratoren an.
    *   Potenzielle Risikostufen werden als **Kritisch**, **Hoch**, **Mittel** und **Niedrig** kategorisiert.
    
    ![Registerkarte "Benutzerbewertungsrisiko - Übersicht"](images/ua-risk-summary-tab.png "Registerkarte "Benutzerbewertungsrisiko - Übersicht"")
    
5.  Klicken Sie auf die Registerkarte **Zielübersicht**. Diese Registerkarte enthält die folgenden Informationen:
    
    *   Anzahl kritische Benutzer mit hohem Risiko, DBAs, DV-Administratoren und Auditadministratoren
    *   Datum und Uhrzeit der letzten Benutzerbewertung
    *   Gibt an, ob die letzte Benutzerbewertung von der Baseline abweicht (sofern eine festgelegt ist)
    
    ![Registerkarte "Benutzerbewertungsziel - Übersicht"](images/ua-target-summary-tab.png "Registerkarte "Benutzerbewertungsziel - Übersicht"")
    

## Aufgabe 2: Benutzer in der neuesten Benutzerbewertung analysieren

Die letzte Benutzerbewertung wird automatisch von Oracle Data Safe generiert, wenn Sie die Zieldatenbank registrieren.

1.  Klicken Sie auf der Registerkarte **Zielübersicht** auf **Bericht anzeigen**, um die neueste Benutzerbewertung für die Zieldatenbank anzuzeigen.
    
2.  Prüfen Sie im Bericht oben auf der Registerkarte **Überblick** die Diagramme **Potenzielles Benutzerrisiko**, **Benutzerrollen**, **Letzte Kennwortänderung** und **Letzte Anmeldung**.
    
    ![Benutzerbewertung - Letzte Diagramme](images/ua-latest-charts.png "Benutzerbewertung - Letzte Diagramme")
    
3.  Klicken Sie auf die Registerkarte **Testinformationen**. Sie können die folgenden Informationen anzeigen:
    
    *   Der Name der letzten Benutzerbewertung
    *   Die OCID der letzten Benutzerbewertung
    *   Das Compartment, zu dem die letzte Benutzerbewertung gehört
    *   Der Zieldatenbankname
    *   Datum und Uhrzeit der Bewertung
    *   Der Zeitplan für die letzte Bewertung
    *   Ob die letzte Bewertung als Baselinebewertung festgelegt ist
    *   Gibt an, ob die letzte Bewertung der Baselinebewertung entspricht (sofern festgelegt)
    
    ![Registerkarte "Bewertungsinformationen" für die letzte Benutzerbewertung](images/ua-assessment-information-tab-latest-assessment.png "Registerkarte "Bewertungsinformationen" für die letzte Benutzerbewertung")
    
4.  Scrollen Sie nach unten, und prüfen Sie den Abschnitt **Benutzerdetails**. Standardmäßig enthält diese Tabelle die folgenden Informationen zu jedem Benutzer:
    
    *   Benutzername
    *   Benutzertyp (z.B. PRIVILEGED, SCHEMA)
    *   Angabe, ob der Benutzer ein DBA, ein DV-Administrator oder ein Auditadministrator ist
    *   Potenzielles Risikoniveau (z.B. LOW, HIGH oder CRITICAL)
    *   Status des Benutzers (z.B. OPEN, LOCKED oder EXPIRED\_AND\_LOCKED)
    *   Datum und Zeit der letzten Anmeldung des Benutzers bei der Zieldatenbank
    *   Benutzerprofil
    *   Auditdatensätze für den Benutzer
    
    ![Letzte Bewertungsdetails der Benutzerbewertung](images/ua-latest-user-details.png "Letzte Bewertungsdetails der Benutzerbewertung")
    
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
    
    ![EVIL RICH Benutzerdetails](images/ua-evil-rich-user-details.png "EVIL RICH Benutzerdetails")
    
6.  Klicken Sie auf **Schließen**.
    
7.  Um den Bericht so zu filtern, dass nur potenziell kritische Risikobenutzer angezeigt werden, gehen Sie wie folgt vor: Klicken Sie auf die Registerkarte **Überblick**. Klicken Sie im Diagramm **Potenzielles Benutzerrisiko** auf den Abschnitt **Kritisch** des Diagramms. Ein Filter wird automatisch erstellt.
    
    ![Kritisches Risiko - Benutzerfilter](images/ua-critical-risk-users-filter.png "Kritisches Risiko - Benutzerfilter")
    
8.  Klicken Sie zum Entfernen des Filters auf das **X** neben dem Filter.
    

## Aufgabe 3: Auditdatensätze des Benutzers `ADMIN` prüfen

1.  Identifizieren Sie die Zeile in der Tabelle für den Benutzer `ADMIN`. Klicken Sie in der Spalte **Auditdatensätze** für den Benutzer `ADMIN` auf **Aktivität anzeigen**.
    
    ![ADMIN-Benutzerauditdatensätze](images/ua-admin-user-audit-records.png "ADMIN-Benutzerauditdatensätze")
    
    Der Bericht **Alle Aktivitäten** für den Benutzer `ADMIN` wird angezeigt.
    
2.  Untersuchen Sie den Bericht.
    
    *   Der Bericht wird automatisch gefiltert, um Auditdatensätze für die letzte Woche, den Benutzer `ADMIN` und die Zieldatenbank anzuzeigen.
    *   Oben im Bericht können Sie Summen für **Ziele**, **DB-Benutzer**, **Clienthosts**, **DMLs**, **Berechtigungsänderungen**, **DDLs**, **Benutzer-/Berechtigungsänderungen**, **Anmeldefehler**, **Anmeldeerfolge** und **Ereignisse gesamt** anzeigen.
    *   In der Spalte **Ereignis** in der Tabelle werden die vom Benutzer `ADMIN` ausgeführten Aktivitätstypen angezeigt. Beispiel: `GRANT`, `LOGON`, `CREATE USER` usw.
    *   Unten auf der Seite können Sie auf die Seitennummern klicken, um weitere Auditdatensätze anzuzeigen.
    
    ![Bericht über alle Aktivitäten für den ADMIN-Benutzer](images/ua-all-activity-report-admin-user.png "Bericht über alle Aktivitäten für den ADMIN-Benutzer")
    

## Aufgabe 4: Benutzer in der Zieldatenbank erstellen

1.  Rufen Sie das SQL-Arbeitsblatt in **Database Actions** auf.
    
2.  Löschen Sie bei Bedarf das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
3.  Geben Sie im SQL Worksheet die folgenden Befehle ein:
    
        <copy>CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen** (grüner Kreis mit weißem Pfeil), um die Abfrage auszuführen.
    
    ![Schaltfläche "Skript ausführen"](images/run-script.png "Schaltfläche "Skript ausführen"")
    
5.  Prüfen Sie auf der Registerkarte **Skriptausgabe** unten auf der Seite, ob der Benutzer `JOE_SMITH` erstellt wurde und die Berechtigung erfolgreich erteilt wurde.
    

## Aufgabe 5: Neueste Benutzerbewertung aktualisieren und umbenennen

1.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Benutzerbewertung**.
    
3.  Klicken Sie auf die Registerkarte **Zielübersicht**.
    
4.  Klicken Sie für die Zieldatenbank auf **Bericht anzeigen**, um die letzte Benutzerbewertung zu öffnen.
    
5.  Um die neueste Benutzerbewertung zu aktualisieren, klicken Sie auf die Schaltfläche **Jetzt aktualisieren**.
    
    ![Schaltfläche "Jetzt aktualisieren" für Benutzerbewertung](images/ua-refresh-now-button.png "Schaltfläche "Jetzt aktualisieren" für Benutzerbewertung")
    
6.  Behalten Sie im Bereich **Jetzt aktualisieren** den Standardnamen bei, und klicken Sie auf **Jetzt aktualisieren**. Warten Sie, bis der Status der letzten Benutzerbewertung **SUCCEEDED** lautet. Oracle Data Safe speichert automatisch eine statische Kopie der Bewertung in der Bewertungshistorie.
    
    ![Bereich "Jetzt aktualisieren" für Benutzerbewertung](images/ua-refresh-now-panel.png "Bereich "Jetzt aktualisieren" für Benutzerbewertung")
    
7.  Prüfen Sie die aktualisierte letzte Bewertung. Beachten Sie, dass der gerade erstellte Benutzer `JOE_SMITH` aufgeführt wird.
    
8.  Klicken Sie auf die Registerkarte **Bewertungsinformationen** und dann auf das Symbol **Stift** neben dem Bewertungsnamen. Ändern Sie den Namen in **Letzte Benutzerbewertung**, und klicken Sie auf das Symbol **Speichern**. Warten Sie, bis sich der Status von **Aktualisierung** in **Erfolgreich** ändert. Der Name wird auf der Seite aktualisiert.
    
    ![Neueste Benutzerbewertung umbenannt](images/ua-renamed-latest-assessment.png "Neueste Benutzerbewertung umbenannt")
    

## Aufgabe 6: Benutzerbewertungshistorie für die Zieldatenbank anzeigen

1.  Klicken Sie oben auf der Seite **Letzte Benutzerbewertung** auf **Historie anzeigen**.
    
2.  Stellen Sie sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
3.  Prüfen Sie die Liste der Tests.
    
    ![Bewertungshistorie für die letzte Benutzerbewertung](images/user-assessment-history-page.png "Bewertungshistorie für die letzte Benutzerbewertung")
    
4.  Klicken Sie auf **Schließen**, um zur letzten Benutzerbewertung zurückzukehren.
    
    Wenn Sie die letzte Benutzerbewertung verlassen haben, können Sie wie folgt zu ihr zurückkehren: Klicken Sie im Navigationspfad auf **Benutzerbewertung**. Klicken Sie auf die Registerkarte **Zielübersicht**. Klicken Sie für die Zieldatenbank auf **Bericht anzeigen**.
    

## Aufgabe 7: Letzte Benutzerbewertung mit der ersten Benutzerbewertung vergleichen

Sie können eine Benutzerbewertung auswählen, die mit der letzten Benutzerbewertung verglichen werden soll. Mit dieser Option müssen Sie keine Baseline festlegen. Diese Option ist nur verfügbar, wenn Sie die letzte Benutzerbewertung anzeigen.

1.  Klicken Sie beim Anzeigen der letzten Benutzerbewertung links unter **Ressourcen** auf **Bewertungen vergleichen**.
    
2.  Scrollen Sie nach unten zum Abschnitt **Vergleich mit anderen Bewertungen**.
    
3.  Wenn Ihr Compartment nicht angezeigt wird, klicken Sie auf **Compartment ändern**, und wählen Sie Ihr Compartment aus.
    
4.  Wählen Sie in der Dropdown-Liste **Bewertung auswählen** die erste Bewertung für die Zieldatenbank (zweite Bewertung in der Liste). Sobald Sie es auswählen, wird der Vergleichsvorgang gestartet.
    
5.  Prüfen Sie den Bericht **Vergleich**.
    
    *   Der Bericht gibt an, dass ein neuer Benutzer hinzugefügt wurde.
    *   Das Ergebnis **Neuer Benutzer** ist ein potenzielles **Kritisches** Risiko.
    
    ![Benutzerbewertungsvergleichsbericht](images/ua-comparison-report.png "Benutzerbewertungsvergleichsbericht")
    
6.  Klicken Sie in der Spalte **Vergleichsergebnisse** auf die Links **Details öffnen**, um weitere Informationen anzuzeigen.
    
    Der Bereich **Vergleichsdetails** wird angezeigt.
    
    ![Fensterbereich "Vergleichsdetails"](images/ua-comparison-details-panel.png "Fensterbereich "Vergleichsdetails"")
    
7.  Prüfen Sie die Informationen, und klicken Sie auf **Schließen**.
    

## Aufgabe 8: Neueste Benutzerbewertung als PDF-Bericht herunterladen

1.  Klicken Sie oben auf der letzten Benutzerbewertungsseite im Menü **Weitere Aktionen** auf **Bericht generieren**.
    
    Das Dialogfeld **Bericht generieren** wird angezeigt.
    
2.  Lassen Sie **PDF** als Berichtsformat ausgewählt, und klicken Sie auf **Bericht generieren**.
    
3.  Warten Sie auf die Meldung, dass die **PDF-Berichtsgenerierung abgeschlossen ist**. Klicken Sie dann auf den Link **hier**.
    
    ![Dialogfeld "Bericht generieren" in der Benutzerbewertung](images/ua-generate-report-dialog.png "Dialogfeld "Bericht generieren" in der Benutzerbewertung")
    
4.  Öffnen Sie die PDF, und prüfen Sie sie.
    
    ![Letzte Benutzerbewertung im PDF-Format Seite 1](images/ua-pdf-report-page1.png "Letzte Benutzerbewertung im PDF-Format Seite 1")
    
    ![Letzte Benutzerbewertung im PDF-Format Seite 2](images/ua-pdf-report-page2.png "Letzte Benutzerbewertung im PDF-Format Seite 2")
    
5.  Schließen Sie die PDF-Datei, und kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück.
    

## Aufgabe 9: Benutzerbewertungshistorie für alle Zieldatenbanken anzeigen

Auf der Seite "Benutzerbewertungshistorie" können Sie eine Liste aller gespeicherten Benutzerbewertungen für alle Ihre Zieldatenbanken anzeigen.

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Benutzerbewertung**.
    
2.  Klicken Sie unter **Zugehörige Ressourcen** auf **Bewertungshistorie**.
    
3.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist.
    
4.  Beachten Sie, dass Ihre gespeicherten Benutzerbewertungen hier aufgelistet sind.
    
    *   Sie können die Anzahl der kritischen Risiken, hohen Risiken, DBAs, DV-Administratoren und Auditadministratoren in allen Zieldatenbanken in den ausgewählten Compartments vergleichen.
    *   Sie können auch schnell Benutzerbewertungen identifizieren, die als Baselines festgelegt sind.
    
    ![Bewertungshistorie für alle Zieldatenbanken](images/ua-assessment-history-all-targets.png "Bewertungshistorie für alle Zieldatenbanken")
    
5.  Um die Liste nach Zieldatenbank zu sortieren, klicken Sie auf die Spaltenüberschrift **Zieldatenbank**.
    
6.  Klicken Sie auf den Namen einer Benutzerbewertung für die Zieldatenbank. Beachten Sie, dass Sie die Daten in einer gespeicherten Benutzerbewertung nicht aktualisieren können.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Überblick über Benutzerbewertung](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-6BF46EE2-F7B5-4710-A09C-069EA95F8052)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023