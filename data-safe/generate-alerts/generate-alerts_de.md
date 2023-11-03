# Alerts generieren

## Einführung

Ein Alert ist eine Meldung, mit der Sie benachrichtigt werden, wenn ein bestimmtes Auditereignis in einer Zieldatenbank auftritt. In Oracle Data Safe können Sie Alert-Policys für Ihre Zieldatenbanken bereitstellen, Alerts anzeigen und verwalten, vordefinierte Alertberichte anzeigen und benutzerdefinierte Alertberichte erstellen.

Prüfen Sie zunächst die vordefinierten Alert-Policys in Oracle Data Safe, und stellen Sie dann zwei davon bereit. Führen Sie mit Database Actions Aktivitäten in der Zieldatenbank aus, um Alerts in Oracle Data Safe zu verursachen. Prüfen Sie die generierten Alerts, und erstellen Sie einen benutzerdefinierten Alertbericht. Laden Sie den Bericht als PDF herunter.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Oracle Data Safe-Alert-Policys prüfen
*   Alert-Policys in der Zieldatenbank bereitstellen
*   Aktivitäten in der Zieldatenbank ausführen, um Alerts in Oracle Data Safe zu generieren
*   Alerts in Oracle Data Safe prüfen
*   Details für einen Alert anzeigen und schließen
*   Benutzerdefinierten Alertbericht erstellen
*   Benutzerdefinierten Alertbericht als PDF generieren und herunterladen
*   Historie des Alertberichts anzeigen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account abgerufen und bei Oracle Cloud Infrastructure angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank wurde bei Oracle Data Safe registriert. Stellen Sie sicher, dass das `ADMIN`\-Kennwort für die Zieldatenbank verfügbar ist (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database)).
*   Auditdatenerfassung für die Zieldatenbank in Oracle Data Safe gestartet (siehe [Auditdatenbankaktivität](?lab=audit-database-activity))

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.

## Aufgabe 1: Alert-Policys von Oracle Data Safe prüfen

1.  Klicken Sie im **Sicherheitscenter** auf **Benachrichtigungen**.
    
    Die Seite **Alerts** wird angezeigt. Das Alert-Dashboard enthält keine Daten, weil Sie noch keine Alert-Policys aktiviert haben.
    
    ![Alerts-Dashboard ohne Daten](images/alerts-dashboard-no-data.png "Alerts-Dashboard ohne Daten")
    
2.  Klicken Sie unter **Zugehörige Ressourcen** auf **Alert-Policys**.
    
3.  Prüfen Sie die Liste der verfügbaren Alert-Policys, die von Oracle Data Safe bereitgestellt werden. Sie lauten wie folgt:
    
    *   Nicht erfolgreiche Anmeldungen durch den Admin-Benutzer
    *   Profiländerungen
    *   Änderungen an Datenbankparametern
    *   Audit-Policy-Änderungen
    *   Benutzererstellung/Änderung
    *   Änderungen an Benutzerberechtigungen
    *   Datenbankschemaänderungen
    
    ![Oracle Data Safe-Alert-Policys](images/oracle-data-safe-alert-policies.png "Oracle Data Safe-Alert-Policys")
    
4.  Klicken Sie auf die Alert-Policy **Benutzererstellung/Änderung**, und prüfen Sie die zugehörigen Details.
    
    Die Seite **Alert-Policy-Details** wird für die Alert-Policy **Benutzererstellung/-änderung** angezeigt.
    
    ![Alert-Policy-Details für Benutzererstellungsänderung](images/user-creation-modification-alert-policy-details.png "Alert-Policy-Details für Benutzererstellungsänderung")
    
5.  Klicken Sie neben **Auf Zieldatenbanken angewendete Policy** auf **Liste anzeigen**, um die mit der Alert-Policy verknüpften Zieldatenbanken anzuzeigen.
    
    Auf der Seite **Ziel-Policy-Verknüpfungen** wird der Filter **Policy-Name** auf **Benutzererstellung/-änderung** gesetzt. Da Sie die Alert-Policy noch nicht mit einer Zieldatenbank verknüpft haben, wird in der Tabelle **Keine Ziel-Policy-Verknüpfungen verfügbar** angezeigt.
    
    ![Keine Ziel/Policy-Verbindungen verfügbar](images/no-target-policy-associations.png "Keine Ziel/Policy-Verbindungen verfügbar")
    

## Aufgabe 2: Alert-Policys in der Zieldatenbank bereitstellen

1.  Wählen Sie in der Dropdown-Liste **Policy-Name** unter **Filter** die Option **Alle**.
    
2.  Klicken Sie auf der Seite **Ziel-Policy-Verknüpfungen** auf **Policy anwenden**.
    
    Der Fensterbereich **Alert-Policy auf Zieldatenbanken anwenden und aktivieren** wird angezeigt.
    
3.  Wählen Sie **Nur ausgewählte Ziele** aus.
    
4.  Klicken Sie gegebenenfalls auf **Compartment ändern**, und wählen Sie Ihr Compartment aus.
    
5.  Wählen Sie in der Dropdown-Liste die Zieldatenbank.
    
6.  Wählen Sie **Nur ausgewählte Policys** aus.
    
7.  Wählen Sie in der Dropdown-Liste jeweils die Alert-Policys **Benutzererstellung/-änderung** und **Nicht erfolgreiche Anmeldungen durch Admin-Benutzer** aus.
    
8.  Klicken Sie auf **Policy anwenden**, und warten Sie, bis eine Meldung angibt, dass Sie den Bereich schließen können.
    
    ![Bereich "Alert-Policys einspielen und aktivieren" für Zieldatenbanken](images/apply-and-enable-alert-policy-panel.png "Bereich "Alert-Policys einspielen und aktivieren" für Zieldatenbanken")
    
9.  Klicken Sie auf **Schließen**.
    
    Die beiden Ziel-Policy-Verknüpfungen für die Zieldatenbank werden auf der Seite aufgelistet und sind aktiviert und aktiv. Wenn keine Ziel-Policy-Verknüpfung angezeigt wird, löschen Sie den Filter für den Policy-Namen, wenn er noch auf **Benutzererstellung/Änderung** gesetzt ist.
    
    ![Zwei Ziel-Policy-Verbindungen für die Zieldatenbank](images/two-target-policy-associations-for-target.png "Zwei Ziel-Policy-Verbindungen für die Zieldatenbank")
    

## Aufgabe 3: Aktivität in der Zieldatenbank ausführen, um Alerts in Oracle Data Safe zu generieren

In dieser Aufgabe führen Sie Aktivitäten für die Zieldatenbank in Database Actions aus, um einige Auditdaten zu generieren. Versuchen Sie zunächst absichtlich, sich als Benutzer `ADMIN` mit falschen Kennwörtern anzumelden. Melden Sie sich an, und erstellen Sie ein Benutzerkonto.

1.  Kehren Sie zum SQL-Arbeitsblatt in Database Actions zurück.
    
2.  Wenn Ihre Sitzung abgelaufen ist, ist das in Ordnung. Klicken Sie auf **OK** und dann auf **Verlassen**. Andernfalls wählen Sie in der Dropdown-Liste in der oberen rechten Ecke die Option **Abmelden**, und klicken Sie im Dialogfeld auf **Beenden**.
    
    Die Seite **Anmelden** wird angezeigt. Das Feld "Benutzername" wird vorab mit dem Benutzer `ADMIN` aufgefüllt.
    
3.  Führen Sie dies zweimal aus: Geben Sie ein falsches Kennwort ein, und klicken Sie auf **Anmelden**.
    
    Die Meldung **Ungültige Zugangsdaten** wird angezeigt.
    
    ![Ungültige Datenbankkennwortmeldung](images/invalid-database-password.png "Ungültige Datenbankkennwortmeldung")
    
4.  Geben Sie das richtige Kennwort ein, und klicken Sie auf **Anmelden**.
    
5.  Klicken Sie bei Bedarf unter **Entwicklung** auf **SQL**.
    
6.  Löschen Sie das Arbeitsblatt, und fügen Sie dann das folgende SQL-Skript ein. Ersetzen Sie `your-password` durch ein Kennwort Ihrer Wahl. Das Kennwort muss zwischen 12 und 30 Zeichen lang sein und mindestens einen Großbuchstaben, einen Kleinbuchstaben und ein numerisches Zeichen enthalten. Sie kann Ihren Benutzernamen oder das doppelte Anführungszeichen (") nicht enthalten.
    
        <copy>drop user MALFOY cascade;
        create user MALFOY identified by your-password;
        grant PDB_DBA to MALFOY;</copy>
        
7.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen**, und warten Sie, bis die Ausführung des Skripts abgeschlossen ist.
    
8.  Prüfen Sie in der Skriptausgabe, ob der Benutzer `MALFOY` erfolgreich gelöscht und dann neu erstellt wurde.
    
9.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück, und warten Sie einige Minuten, bis Oracle Data Safe die Alerts generiert.
    

## Aufgabe 4: Alerts in Oracle Data Safe prüfen

1.  Klicken Sie links unter **Sicherheitscenter** auf **Alerts**.
    
2.  Wählen Sie links unter **Filter** die Zieldatenbank aus.
    
3.  Beachten Sie, dass das Alerts Dashboard jetzt Daten enthält.
    
    *   Das Diagramm **Alerts - Übersicht** zeigt, dass vier Alerts vorhanden sind. Zwei sind ein kritisches Risiko und zwei ein mittleres Risiko.
    *   Das Diagramm **Offene Alerts** zeigt, dass am aktuellen Tag vier Alerts vorhanden sind.
    *   In der Registerkarte **Alerts - Übersicht** wird die Anzahl der kritischen, hohen und mittleren Alerts zusammen mit der Anzahl der Zieldatenbanken angezeigt. Außerdem wird die Gesamtanzahl der Alerts und Zieldatenbanken angezeigt.
    *   In der Registerkarte **Zielübersicht** wird die Anzahl der offenen, kritischen, hohen und mittleren Alerts angezeigt.
    
    ![Alerts-Dashboard mit Daten](images/alerts-dashboard-with-data.png "Alerts-Dashboard mit Daten") ![Registerkarte "Zielübersicht"](images/targets-summary.png "Registerkarte "Zielübersicht"")
    
4.  Klicken Sie unter **Zugehörige Ressourcen** auf **Berichte**.
    
5.  Klicken Sie rechts in der Spalte **Berichtsname** auf den Bericht **Alle Alerts**, um ihn anzuzeigen.
    
    ![Bericht "Alle Alerts"](images/alerts-reports.png "Bericht "Alle Alerts"")
    
6.  Prüfen Sie den Bericht.
    
    *   Der Bericht wird automatisch gefiltert, um alle Alerts für alle Zieldatenbanken im ausgewählten Compartment für die letzte Woche anzuzeigen. Um benutzerdefinierte Filter manuell zu erstellen, können Sie den **SCIM-Abfragegenerator** verwenden.
    *   Sie können mehrere Summen anzeigen, darunter die Gesamtanzahl der Zieldatenbanken, die Gesamtanzahl der offenen und geschlossenen Alerts sowie die Gesamtanzahl der kritischen, hohen, mittleren und niedrigen Alerts. Sie können auf die Summe der **Ziele** klicken, um die Liste der Zieldatenbanken anzuzeigen. Sie können auf die anderen Summen klicken, um einen Filter in der Liste der Alerts umzuschalten.
    *   Unten im Bericht können Sie die Liste der Alerts anzeigen. Standardmäßig enthält die Tabelle den Alertnamen, den Alertstatus, den Alertschweregrad, die Zieldatenbanken, in denen das auditierte Ereignis aufgetreten ist, und den Zeitpunkt, zu dem der Alert erstellt wurde.
    *   Sie haben die Möglichkeit, einen PDF- oder XLS-Bericht zu erstellen, einen benutzerdefinierten Bericht zu erstellen, einen benutzerdefinierten Bericht zu planen, Alerts zu öffnen und zu schließen und anzugeben, welche Tabellenspalten auf der Seite angezeigt werden sollen.
    
    ![Bericht "Alle Alerts"](images/all-alerts-report.png "Bericht "Alle Alerts"")
    
7.  Klicken Sie oben im Bericht auf **\+ Weiterer Filter**. Erstellen Sie den Filter **Zieldatenbanken = Ihr Ziel-Datenbankname**, und klicken Sie auf **Anwenden**.
    
    In der Tabelle werden nur Alerts aufgeführt, die sich auf Ihre Zieldatenbank beziehen.
    
8.  Klicken Sie auf **\+ Weiterer Filter**. Erstellen Sie den Filter **Alertname = Benutzererstellung/-änderung**, und klicken Sie auf **Anwenden**.
    
    In der Tabelle werden nur Alerts aufgeführt, die sich auf die Benutzererstellung/-änderung beziehen.
    
9.  Prüfen Sie die für die **Benutzererstellung/-änderung** generierten Alerts.
    
    ![Benutzererstellungs-/Änderungswarnungen](images/alerts-user-creation-modification.png "Benutzererstellungs-/Änderungswarnungen")
    

## Aufgabe 5: Details für einen Alert anzeigen und schließen

1.  Klicken Sie auf einen der Alertnamen, um weitere Details anzuzeigen.
    
2.  Prüfen Sie die folgenden Informationen zum Alert:
    
    *   Alertname (Instanz des Alerts)
    *   Zieldatenbank, für die der Alert gilt
    *   Alertdringlichkeit
    *   Alertstatus: Gibt an, ob der Alert offen oder geschlossen ist
    *   Alerttyp: Aktuell sind alle Alerttypen AUDITING
    *   Policy, die den Alert generiert hat
    *   Benutzervorgang, der den Alert generiert hat
    *   Vorgangszeit und -status
    *   Zeitpunkt der Erstellung und Aktualisierung des Alerts
    *   Oracle Cloud-ID (OCID) für den Alert
    *   Compartment, in dem sich der Alert befindet
    *   Vorgangsdetails
    
    ![Seite "Alertdetails"](images/alert-details-page.png "Seite "Alertdetails"")
    
3.  Klicken Sie auf **Schließen**, um den Alert zu schließen.
    
    Der Alertstatus wird sofort auf **GESCHLOSSEN** gesetzt.
    

## Aufgabe 6: Benutzerdefinierten Alertbericht erstellen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Alle Alerts**, um zum Bericht "Alle Alerts" zurückzukehren.
    
2.  Fügen Sie zusätzlich zum bereits festgelegten Standardfilter zwei weitere Filter hinzu:
    
    *   **Zieldatenbanken = Ihr Ziel-Datenbankname**
    *   **Alertname = Nicht erfolgreiche Anmeldungen durch den Admin-Benutzer**
3.  Klicken Sie auf **Benutzerdefinierten Bericht erstellen**.
    
    Das Dialogfeld **Benutzerdefinierten Bericht erstellen** wird angezeigt.
    
4.  Geben Sie unter **Anzeigename** die Option **Nicht erfolgreiche Anmeldungen durch Admin-Benutzer für Ihren Ziel-Datenbanknamen** ein. (Optional) Geben Sie eine Beschreibung ein. Wählen Sie Ihr Compartment aus. Klicken Sie auf **Benutzerdefinierten Bericht erstellen**, und warten Sie, bis der Bericht generiert wird.
    
    ![Dialogfeld "Benutzerdefinierten Bericht erstellen"](images/create-custom-report-dialog-box.png "Dialogfeld "Benutzerdefinierten Bericht erstellen"")
    
5.  Klicken Sie auf den Link **Hier klicken**, um den Bericht anzuzeigen.
    

## Aufgabe 7: Benutzerdefinierten Alertbericht als PDF generieren und herunterladen

1.  Klicken Sie auf der benutzerdefinierten Berichtsseite auf **Bericht generieren**.
    
    Das Dialogfeld **Bericht generieren** wird angezeigt.
    
2.  Lassen Sie **PDF** ausgewählt.
    
3.  Geben Sie den Anzeigenamen **Nicht erfolgreiche Admin-Anmeldungen für your-target-database-name** ein.
    
4.  (Optional) Geben Sie unter **Beschreibung** die Option **Nicht erfolgreiche Anmeldungen durch den Admin-Benutzer für den Datenbank-Ihr-Ziel-Datenbanknamen** ein.
    
5.  Lassen Sie das Compartment ausgewählt, lassen Sie das Zeilenlimit auf 10000 gesetzt, und lassen Sie die Berichtsstartzeit unverändert.
    
6.  Klicken Sie auf **Bericht generieren**, und warten Sie, bis der PDF-Bericht generiert wurde.
    
    Wenn die Berichtsgenerierung abgeschlossen ist, wird eine entsprechende Meldung angezeigt.
    
    ![Dialogfeld "Bericht generieren"](images/generate-report-dialog-box.png "Dialogfeld "Bericht generieren"")
    
7.  Klicken Sie auf den Link **hier**, um den Bericht herunterzuladen.
    
8.  Falls erforderlich, speichern Sie den Bericht auf Ihrem lokalen Computer.
    
9.  Öffnen Sie den PDF-Bericht, und zeigen Sie ihn an. Schließen Sie anschließend die Browserregisterkarte.
    
    ![PDF-Bericht zu nicht erfolgreichen Admin-Anmeldungen](images/failed-admin-logins-report-pdf.png "PDF-Bericht zu nicht erfolgreichen Admin-Anmeldungen")
    
10.  Klicken Sie im Dialogfeld **Bericht generieren** auf **Schließen**.
    

## Aufgabe 8: Alertberichtshistorie anzeigen

1.  Klicken Sie unter **Zugehörige Ressourcen** auf **Alertberichtshistorie**.
    
2.  Beachten Sie, dass Ihr benutzerdefinierter Bericht aufgeführt wird. Sie können den Status, die Beschreibung, den Zeitpunkt der Generierung, die von Ihnen (`GENERATED`) oder vom Scheduler generierte Datei, das zum Download verfügbare Dateiformat und ein Download-Symbol anzeigen. Oracle Data Safe hält Ihren Bericht bis zu drei Monate lang verfügbar.
    
    ![Alertberichtshistorie](images/alert-report-history.png "Alertberichtshistorie")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Alerts - Überblick](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-37F8AC38-44D4-42D1-AE93-9775DCF21511)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023