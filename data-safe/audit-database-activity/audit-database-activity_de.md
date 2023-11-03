# Datenbankaktivität auditieren

## Einführung

In Oracle Data Safe können Sie Audit-Policys für Ihre Zieldatenbanken bereitstellen und Auditdaten im Oracle Data Safe-Repository erfassen. Es gibt grundlegende, Administrator-, Benutzer-, vordefinierte und benutzerdefinierte Oracle-Audit-Policys sowie Audit-Policys, die Ihr Unternehmen bei der Einhaltung von Compliancestandards unterstützen. Wenn Sie eine Zieldatenbank registrieren, erstellt Oracle Data Safe automatisch ein Auditprofil, eine Audit-Policy und Audittrails, die für die Zieldatenbank relevant sind.

Prüfen Sie zunächst die globalen Einstellungen in Oracle Data Safe. Prüfen Sie anschließend das Auditprofil, die Audittrails und die Audit-Policy, die automatisch für die Zieldatenbank erstellt werden. Starten Sie die Auditdatenerfassung in der Zieldatenbank, und stellen Sie einige Audit-Policys bereit. Analysieren Sie die Auditereignisse, zeigen Sie Berichte an, erstellen Sie einen benutzerdefinierten Auditbericht, und laden Sie den benutzerdefinierten Auditbericht als PDF herunter.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Globale Einstellungen für Oracle Data Safe prüfen
*   Prüfen Sie das Auditprofil für die Zieldatenbank
*   Audit-Policy für die Zieldatenbank prüfen
*   Audittrails für die Zieldatenbank prüfen
*   Menge der in der Zieldatenbank verfügbaren Auditdatensätze für die erkannten Audittrails anzeigen
*   Auditdatenerfassung starten
*   Prüfen Sie das Aktivitätsauditing-Dashboard.
*   Provisioning von Audit-Policys in der Zieldatenbank
*   Auditereignisse für die Zieldatenbank analysieren
*   Bericht "Alle Aktivitäten" anzeigen
*   Benutzerdefinierten Auditbericht erstellen
*   Benutzerdefinierten Auditbericht als PDF generieren und herunterladen
*   Historie des Auditberichts anzeigen
*   Planen Sie Ihren benutzerdefinierten Auditbericht

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account erhalten
*   Bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

### Annahmen

*   Ihre Datenwerte können sich von denen in den Screenshots unterscheiden.

## Aufgabe 1: Globale Einstellungen für Oracle Data Safe prüfen

1.  Rufen Sie die Seite **Überblick** für Oracle Data Safe auf, indem Sie oben auf der Seite im Navigationspfad auf **Data Safe** klicken.
    
2.  Klicken Sie unter **Data Safe** auf **Einstellungen**.
    
3.  Prüfen Sie die globalen Einstellungen.
    
    *   Jeder regionale Oracle Data Safe-Service in einem Mandanten verfügt über globale Einstellungen für die kostenpflichtige Nutzung, den Onlineaufbewahrungszeitraum und den Archivaufbewahrungszeitraum.
    *   Globale Einstellungen werden auf alle Zieldatenbanken angewendet, es sei denn, sie werden von den Auditprofilen der Zieldatenbanken überschrieben.
    *   Die kostenpflichtige Nutzung wird standardmäßig für alle Zieldatenbanken aktiviert, der Onlineaufbewahrungszeitraum auf den Höchstwert von 12 Monaten und der Archivaufbewahrungszeitraum auf den Mindestwert von 0 Monaten. Beachten Sie, dass Sie die kostenpflichtige Nutzung für ein kostenloses Testkonto nicht aktivieren können.
    
    ![Globale Einstellungen](images/global-settings.png "Globale Einstellungen")
    

## Aufgabe 2: Auditprofil für die Zieldatenbank prüfen

1.  Klicken Sie im Navigationspfad auf **Data Safe**.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Aktivitätsauditing**.
    
3.  Klicken Sie unter **Zugehörige Ressourcen** auf **Auditprofile**.
    
4.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** unter **Listengeltungsbereich** ausgewählt ist.
    
5.  Prüfen Sie rechts die Auditprofilinformationen zur Zieldatenbank, und klicken Sie dann auf den Namen der Zieldatenbank, um weitere Details anzuzeigen.
    
    ![Auditprofile (Seite)](images/audit-profiles-page.png "Auditprofile (Seite)")
    
6.  Prüfen Sie die Details im Auditprofil.
    
    *   Es gibt Standardeinstellungen für die kostenpflichtige Nutzung, den Onlineaufbewahrungszeitraum und den Offlineaufbewahrungszeitraum.
    *   Alle anfänglichen Auditprofileinstellungen für die Zieldatenbank werden aus den globalen Einstellungen für Oracle Data Safe übernommen. Sie können sie hier jedoch nach Bedarf ändern.
    
    ![Auditprofil](images/audit-profile-details-page.png "Auditprofil")
    

## Aufgabe 3: Audittrails für die Zieldatenbank prüfen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie auf der linken Seite unter **Zugehörige Ressourcen** auf **Audittrails**.
    
3.  Stellen Sie sicher, dass Sie unter **Listengeltungsbereich** auf der linken Seite das Compartment ausgewählt haben.
    
4.  Wählen Sie links unter **Filter** die Zieldatenbank aus.
    
5.  Prüfen Sie rechts die Audittrails für die Zieldatenbank. Oracle Data Safe erkennt einen Audittrail für eine Autonomous Database mit dem Namen `UNIFIED_AUDIT_TRAIL`.
    
    ![Audit Trails (Seite)](images/audit-trails-page.png "Audit Trails (Seite)")
    
6.  Klicken Sie auf den Namen der Zieldatenbank für einen der Audittrails, und prüfen Sie die Informationen auf der Seite **Audittraildetails**. Hier können Sie die Auditdatenerfassung für den Audittrail verwalten. Beachten Sie, dass der Audittrail derzeit inaktiv ist.
    
    ![Audit Trail-Details (Seite)](images/audit-trail-details-page.png "Audit Trail-Details (Seite)")
    

## Aufgabe 4: Audit-Policy für die Zieldatenbank prüfen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie unter **Zugehörige Ressourcen** auf **Audit-Policys**.
    
3.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
4.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank.
    
5.  Prüfen Sie rechts die Informationen für die Audit-Policy der Zieldatenbank. Beachten Sie, dass nur für die Kategorie **Zusätzliche Policys** Policys aktiviert sind. Diese werden durch einen grünen Kreis mit einem Häkchen gekennzeichnet. Dies sind vordefinierte Oracle-Policys, die standardmäßig in einer Autonomous Transaction Processing-Datenbank aktiviert sind.
    
    ![Seite "Audit-Policys"](images/audit-policies-page.png "Seite "Audit-Policys"")
    
6.  Klicken Sie auf den Namen der Zieldatenbank, um weitere Details auf der Seite **Audit-Policy-Details** anzuzeigen. Scrollen Sie nach unten, und prüfen Sie die Liste der für die Zieldatenbank verfügbaren Audit-Policys.
    
    *   Ein grauer Kreis bedeutet, dass die Audit-Policy noch nicht in der Zieldatenbank bereitgestellt wurde. Ein grüner Kreis bedeutet, dass die Audit-Policy bereitgestellt wird.
    *   Sie können beliebig viele Audit-Policys in der Zieldatenbank bereitstellen und aktivieren und Filter für Benutzer und Rollen festlegen.
    
    ![Audit-Policy-Details (Seite)](images/audit-policies-details-page.png "Audit-Policy-Details (Seite)")
    

## Aufgabe 5: Menge der in der Zieldatenbank verfügbaren Auditdatensätze für die erkannten Audittrails anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie links unter **Zugehörige Ressourcen** auf **Auditprofile**.
    
3.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
4.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank.
    
5.  Klicken Sie rechts auf den Namen der Zieldatenbank.
    
6.  Scrollen Sie nach unten zum Abschnitt **Auditvolumen berechnen**, und klicken Sie auf **In Zieldatenbank verfügbar**.
    
    Das Dialogfeld **Verfügbares Volumen berechnen** wird angezeigt.
    
7.  Klicken Sie für das Startdatum auf das Kalenderwidget, und wählen Sie das aktuelle Datum um 00:00 UTC aus. Sie wählen das aktuelle Datum aus, weil die Zieldatenbank neu ist.
    
8.  Wählen Sie aus der Dropdown-Liste **Trailstandorte** die Option `UNIFIED_AUDIT_TRAIL` aus.
    
9.  Klicken Sie auf **Compute**, und warten Sie, bis Oracle Data Safe das verfügbare Auditvolumen berechnet.
    
    ![Dialogfeld "Verfügbares Volumen berechnen"](images/compute-available-volume-dialog-box.png "Dialogfeld "Verfügbares Volumen berechnen"")
    
10.  Zeigen Sie in der Spalte **In Zieldatenbank verfügbar** die Anzahl der Auditdatensätze für `UNIFIED_AUDIT_TRAIL` an.
    
    *   In unserem Fall ist die Anzahl der Datensätze in `UNIFIED_AUDIT_TRAIL` gering, weil Ihre Zieldatenbank gerade bereitgestellt wurde. Bei einer älteren Zieldatenbank gibt es jedoch wahrscheinlich eine große Anzahl von Auditdatensätzen.
    *   Oracle Data Safe teilt die Zahlen nach Monat auf. Mit diesen Werten können Sie ein Startdatum für den Oracle Data Safe-Audittrail bestimmen.
    *   Machen Sie sich keine Sorgen, wenn sich die Anzahl der Auditdatensätze auf Ihrem System von der unten angezeigten unterscheidet.
    
    ![In Spalte "Zieldatenbank" verfügbar](images/available-in-target-database.png "In Spalte "Zieldatenbank" verfügbar")
    

## Aufgabe 6: Auditdatenerfassung starten

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie auf der linken Seite unter **Zugehörige Ressourcen** auf **Audittrails**.
    
3.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
4.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank.
    
5.  Klicken Sie rechts auf den Namen der Zieldatenbank für `UNIFIED_AUDIT_TRAIL`.
    
    Die Seite **Protokolldetails** wird angezeigt.
    
6.  Klicken Sie auf **Start**.
    
    Das Dialogfeld **Audittrail starten: UNIFIED\_AUDIT\_TRAIL** wird angezeigt.
    
7.  Konfigurieren Sie ein Startdatum basierend auf den Daten im Bereich **Auditvolumen berechnen** des Auditprofils, das Sie in Aufgabe 5 angezeigt haben (Schritt 10). Beispiel: Wenn Sie einen Monat (Feb 2023) aufgelistet haben, können Sie das Startdatum auf Anfang Februar setzen.
    
    ![Dialogfeld "Audit Trail starten"](images/start-audit-trail-dialog-box.png "Dialogfeld "Audit Trail starten"")
    
8.  Klicken Sie auf **Start**. Warten Sie, bis sich der **Erfassungsstatus** von **Wird gestartet** in **Wird erfasst** und dann in **IDLE** ändert. Es dauert etwa eine Minute.
    
    ![Erfassungsstatus inaktiv](images/collection-state-idle.png "Erfassungsstatus inaktiv")
    

## Aufgabe 7: Prüfen des Aktivitätsauditing-Dashboards

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
    Im Dashboard "Aktivitätsauditing" wird standardmäßig eine Übersicht über die Auditereignisse der letzten Woche für alle Zieldatenbanken in Form von Diagrammen und Tabellen angezeigt. Auf der linken Seite unter **Listengeltungsbereich** und **Filter** können Sie nach Compartment, Zeitraum und Zieldatenbank filtern.
    
2.  Stellen Sie in der Dropdown-Liste **Compartments** auf der linken Seite sicher, dass das Compartment ausgewählt ist.
    
3.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank aus. Das Dashboard wird automatisch aktualisiert und enthält Auditereignisstatistiken nur für Ihre Zieldatenbank.
    
4.  Prüfen Sie die Diagramme.
    
    *   Im Diagramm **Nicht erfolgreiche Anmeldeaktivität** wird die Anzahl der nicht erfolgreichen Anmeldungen in der Zieldatenbank in der letzten Woche angezeigt. Je nachdem, wie Sie bisher in Database Actions interagiert haben, können Sie sich möglicherweise nicht erfolgreich anmelden.
    *   Im Diagramm **Admin-Aktivitäten** wird die Anzahl der Änderungen an Datenbankschema, Anmeldungen, Auditeinstellungen und Berechtigungen in der Zieldatenbank für die letzte Woche angezeigt.
    *   Im Diagramm **Alle Aktivitäten** wird die Gesamtanzahl der Auditereignisse in der Zieldatenbank für den angegebenen Zeitraum angezeigt.
    
    ![Initialdiagramme für das Dashboard "Aktivitätsprüfung"](images/activity-auditing-dashboard-charts-initial.png "Initialdiagramme für das Dashboard "Aktivitätsprüfung"")
    
5.  Prüfen Sie auf der Registerkarte **Ereignisübersicht** die Statistiken für Auditereigniskategorien.
    
    Statistiken umfassen die Anzahl der Zieldatenbanken mit einem Auditereignis in jeder Ereigniskategorie und die Gesamtanzahl der Ereignisse pro Kategorie. Da Sie nur Statistiken für die Zieldatenbank anzeigen, werden in der Spalte **Zieldatenbanken** diejenigen angezeigt.
    
    ![Aktivitätsauditing-Dashboard - Registerkarte "Ereignisse - Übersicht"](images/activity-auditing-events-summary-tab.png "Aktivitätsauditing-Dashboard - Registerkarte "Ereignisse - Übersicht"")
    
6.  Klicken Sie auf die Registerkarte **Zielübersicht**, und prüfen Sie die verschiedenen Auditereignisanzahlen pro Zieldatenbank.
    
    Auditereignisse umfassen die Anzahl der Anmeldefehler, Schemaänderungen, Berechtigungsänderungen, Änderungen der Auditeinstellungen, alle Aktivitäten (alle Auditereignisse), Database Vault-Verletzungen und Database Vault-Policy-Änderungen.
    
    ![Aktivitätsauditing-Dashboard - Registerkarte "Zielübersicht"](images/activity-auditing-dashboard-target-summary-tab.png "Aktivitätsauditing-Dashboard - Registerkarte "Zielübersicht"")
    

## Aufgabe 8: Audit-Policys bereitstellen

1.  Klicken Sie unter **Zugehörige Ressourcen** auf **Audit-Policys**.
    
2.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
3.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank.
    
4.  Klicken Sie rechts auf den Namen der Zieldatenbank.
    
5.  Beachten Sie, dass die folgenden benutzerdefinierten Audit-Policys in der Zieldatenbank bereitgestellt, jedoch noch nicht aktiviert sind:
    
    *   `APP_USER_NOT_APP_SERVER`
    *   `EMPSEARCH_SELECT_USAGE_BY_PETE`
    *   `ADB_SAAS_ADMIN_AUDIT`
    *   `EMP_RECORD_CHANGES`
6.  Klicken Sie auf **Aktualisieren und bereitstellen**.
    
    Der Fensterbereich **Audit-Policys bereitstellen** wird angezeigt.
    
7.  Wählen Sie **Data Safe-Benutzeraktivitäten ausschließen** aus.
    
8.  Wählen Sie unter **Basic Auditing** die Optionen **Datenbankschemaänderungen** und **Kritische Datenbankaktivität** aus.
    
9.  Wählen Sie unter **Admin-Aktivitätsauditing** die Option **Admin-Benutzeraktivität** aus.
    
10.  Wählen Sie unter **Benutzerdefinierte Policys** die Option **APP\_USER\_NOT\_APP\_SERVER** aus.
    
11.  Klicken Sie auf **Aktualisieren und bereitstellen**, um die ausgewählten Policys in der Zieldatenbank bereitzustellen.
    
    ![Bereich "Audit-Policys bereitstellen"](images/provision-audit-policies-panel.png "Bereich "Audit-Policys bereitstellen"")
    
12.  Warten Sie, bis das Provisioning abgeschlossen ist, und zeigen Sie dann die aktualisierten Policy-Informationen auf der Seite an. Beachten Sie, dass die aktivierten Policys jetzt grüne Kreise aufweisen.
    

![Aktivierte Policys](images/enabled-policies.png "Aktivierte Policys")

## Aufgabe 9: Auditereignisse für die Zieldatenbank analysieren

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Wählen Sie links in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank.
    
    Das Dashboard wird automatisch aktualisiert und enthält Auditereignisstatistiken für die Zieldatenbank. Haben Sie einen Unterschied in den Zahlen bemerkt?
    
    ![Aktivitätsauditing - Dashboard-Diagramme nach Provisioning von Policys](images/activity-auditing-dashboard-charts-afterprovision.png "Aktivitätsauditing - Dashboard-Diagramme nach Provisioning von Policys") ![Dashboard-Tabelle für Aktivitätsauditing nach Provisioning-Policys](images/activity-auditing-dashboard-table-afterprovision.png "Dashboard-Tabelle für Aktivitätsauditing nach Provisioning-Policys")
    
3.  Sie bemerken, dass Schemaänderungen vorliegen. Um dies zu untersuchen, klicken Sie auf der Registerkarte **Ereignisübersicht** auf **Schemaänderungen nach Admin**, um weitere Details anzuzeigen.
    
4.  Prüfen Sie auf der Seite **Schemaänderungen durch Admin** Folgendes:
    
    *   Die Filter werden oben auf der Seite festgelegt. Für **Arbeitsvorgangszeit** sind zwei Filter festgelegt, die den Zeitraum für die letzte Woche festlegen. Unter **Ziel-ID** ist ein Filter festgelegt, der die Zieldatenbank auf Ihre Datenbank setzt.
    *   Die Gesamtanzahl der Ziele, Datenbankbenutzer, Clienthosts, `CREATE`\-Anweisungen, `ALTER`\-Anweisungen und `DROP`\-Anweisungen
    *   Die Gesamtanzahl von Ereignissen
    *   Die einzelnen Auditereignisse
    
    ![Schemaänderungen von Admin (Seite)](images/schema-changes-by-admin-page.png "Schemaänderungen von Admin (Seite)")
    
5.  Klicken Sie auf den Abwärtspfeil am Ende einer beliebigen Zeile in der Ereignistabelle, um weitere Details zum Ereignis anzuzeigen. Wenn Sie auf den Abwärtspfeil klicken, wird ein Aufwärtspfeil angezeigt.
    
    ![Erweiterung der Auditereignistabelle](images/audit-event-table-expander.png "Erweiterung der Auditereignistabelle")
    
6.  Wie wurde die SQL ausgegeben?
    
    Antwort: Scrollen Sie nach unten zur Position **SQL-Text**. Hier können Sie die SQL anzeigen oder kopieren. Folgende SQL wurde abgesetzt:
    
        <copy>drop function HCM1.return_condition</copy>
        

## Aufgabe 10: Bericht "Alle Aktivitäten" anzeigen

Standardmäßig werden im Bericht "Alle Aktivitäten" Auditereignisse der letzten Woche für alle Zieldatenbanken in den ausgewählten Compartments angezeigt.

1.  Klicken Sie unter **Zugehörige Ressourcen** auf **Auditberichte**. Oracle Data Safe verfügt über die folgenden vordefinierten Auditberichte:
    
    *   Alle Aktivitäten
    *   Admin-Aktivitäten
    *   Benutzer-/Berechtigungsänderungen
    *   Audit-Policy-Änderungen
    *   Anmeldeaktivität
    *   Datenzugriff
    *   Datenänderung
    *   Datenbankschemaänderungen
    *   Data Safe-Aktivität
    *   Database Vault-Aktivität
    *   Gemeinsame Benutzeraktivität
    *   Datenbankfehler
    *   Datenextraktionsaktivität
    *   Sensible Daten - Aktivität
    
    ![Auditberichte (Seite)](images/audit-reports-page.png "Auditberichte (Seite)")
    
2.  Stellen Sie sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
3.  Klicken Sie auf den Bericht **All Activity**, um ihn anzuzeigen.
    
4.  Im Bericht festgelegte Filter anzeigen
    
    *   Standardmäßig wird der Bericht gefiltert, um Auditereignisse der letzten Woche für alle Zieldatenbanken in den ausgewählten Compartments anzuzeigen.
    *   Sie können nach Bedarf zusätzliche Filter erstellen.
5.  Zeigen Sie die Summen im Bericht an.
    
    *   Sie können auf **Ziele**, **DB-Benutzer** und **Clienthosts** klicken, um die Liste der Ziele, Datenbankbenutzer und Clienthosts anzuzeigen.
    *   Wenn Sie auf **DMLs**, **Berechtigungsänderungen**, **DDLs**, **Benutzer-/Berechtigungsänderungen**, **Anmeldefehler**, **Anmeldeerfolge** oder **Ereignisse gesamt** klicken, wird die Auditereignistabelle entsprechend gefiltert.
6.  Scrollen Sie nach unten, und zeigen Sie die einzelnen Auditereignisse an.
    
7.  Um weitere Details für ein bestimmtes Auditereignis anzuzeigen, klicken Sie auf den Nach-unten-Pfeil, um die Zeile einzublenden und Details für das jeweilige Ereignis anzuzeigen. Für einige Details können Sie ihre Werte in die Zwischenablage kopieren.
    
    ![Alle Aktivitätsberichte](images/all-activity-report.png "Alle Aktivitätsberichte")
    

## Aufgabe 11: Benutzerdefinierten Auditbericht erstellen

1.  Fügen Sie oben im Bericht **Alle Aktivitäten** die folgenden beiden Filter hinzu. Um einen Filter hinzuzufügen, klicken Sie auf **\+ Weiterer Filter**. Wenn Sie die Filterparameter festgelegt haben, klicken Sie auf **Anwenden**.
    
    *   **Ziel = your-target-database-name**
    *   **Objekteigentümer = HCM1**
2.  Klicken Sie auf **Spalten verwalten**. Wählen Sie im Bereich **Spalten verwalten** die Spalten **Ziel**, **DB-Benutzer**, **Ereignis**, **Objekt**, **Vorgangszeit** und **Unified Audit Policys** aus. Klicken Sie auf **Apply Changes**.
    
    Die Tabelle zeigt die ausgewählten Spalten an. Beachten Sie auch, dass die Summen angepasst werden.
    
    ![Alle Aktivitätsberichte](images/custom-audit-report3.png "Alle Aktivitätsberichte")
    
3.  Klicken Sie auf **Benutzerdefinierten Bericht erstellen**.
    
    Das Dialogfeld **Benutzerdefinierten Bericht erstellen** wird angezeigt.
    
4.  Geben Sie den Anzeigenamen **All Activity Report on schema: HCM1 in the target your-target-database-name** ein. Geben Sie eine optionale Beschreibung ein. Wählen Sie bei Bedarf Ihr Compartment aus. Klicken Sie auf **Benutzerdefinierten Bericht erstellen**, und warten Sie, bis der Bericht generiert wird.
    
    ![Dialogfeld "Benutzerdefinierten Bericht erstellen"](images/create-custom-report-dialog-box.png "Dialogfeld "Benutzerdefinierten Bericht erstellen"")
    
5.  Klicken Sie im Dialogfeld **Benutzerdefinierten Bericht erstellen** auf den Link **Hier klicken**, um zu Ihrem benutzerdefinierten Bericht zu navigieren.
    
    *   Wenn Sie Ihren benutzerdefinierten Bericht ändern müssen, können Sie auf **Bericht speichern** klicken, um die Änderungen zu speichern.
    *   Um Ihren benutzerdefinierten Bericht in Zukunft anzuzeigen, klicken Sie unter **Zugehörige Ressourcen** für das **Aktivitätsauditing** auf **Auditberichte**. Klicken Sie auf die Registerkarte **Benutzerdefinierte Berichte** und dann auf den Namen des benutzerdefinierten Auditberichts.

## Aufgabe 12: Benutzerdefinierten Auditbericht als PDF generieren und herunterladen

1.  Klicken Sie auf der Seite mit dem benutzerdefinierten Auditbericht auf **Bericht generieren**.
    
    Das Dialogfeld **Bericht generieren** wird angezeigt.
    
2.  Lassen Sie **PDF** ausgewählt.
    
3.  Geben Sie den Anzeigenamen **All Activity Report on schema: HCM1 in the target your-target-database-name** ein.
    
4.  (Optional) Geben Sie eine Beschreibung ein.
    
5.  Vergewissern Sie sich, dass das Compartment ausgewählt ist.
    
6.  Lassen Sie die Berichtsstartzeit unverändert.
    
7.  Klicken Sie auf **Bericht generieren**, und warten Sie, bis der PDF-Bericht generiert wurde. Es wird eine Meldung angezeigt, dass die Berichtsgenerierung abgeschlossen ist.
    
    ![PDF eines benutzerdefinierten Auditberichts generieren](images/generate-pdf-custom-audit-report.png "PDF eines benutzerdefinierten Auditberichts generieren")
    
8.  Klicken Sie auf den Link **hier**, um den Bericht herunterzuladen.
    
9.  Wenn Sie aufgefordert werden, den Bericht zu öffnen oder zu speichern, speichern Sie.
    
10.  Schließen Sie das Dialogfeld **Bericht generieren**.
    
11.  Öffnen Sie den PDF-Bericht, und zeigen Sie ihn an.
    

![Alle Aktivitäten - PDF-Bericht](images/all-activity-report-pdf.png "Alle Aktivitäten - PDF-Bericht")

12.  Um den PDF-Bericht zu schließen, schließen Sie die Browserregisterkarte.
    
13.  Klicken Sie zum Schließen des Dialogfeldes **Bericht generieren** auf **Schließen**.
    

## Aufgabe 13: Auditberichtshistorie anzeigen

1.  Klicken Sie unter **Zugehörige Ressourcen** auf **Auditberichtshistorie**.
    
2.  Zeigen Sie die Details für Ihren benutzerdefinierten Bericht an. Auf dieser Seite können Sie auf den Namen eines Berichts klicken, um dessen Berichtsdetails anzuzeigen und den Bericht als PDF- oder XLS-Dokument herunterzuladen (je nachdem, wie Sie ihn ursprünglich generiert haben). Oracle Data Safe speichert die Historie der Auditberichte für bis zu drei Monate.
    
    ![Historie für benutzerdefinierten Bericht](images/history-custom-report.png "Historie für benutzerdefinierten Bericht")
    
3.  Klicken Sie in der Spalte **Berichtsname** auf den Namen des benutzerdefinierten Berichts, um die zugehörigen Details anzuzeigen.
    
    ![Benutzerdefinierte Berichtsdetails](images/custom-report-details.png "Benutzerdefinierte Berichtsdetails")
    

## Aufgabe 14: Benutzerdefinierten Auditbericht planen

Planen Sie Ihren benutzerdefinierten Auditbericht, um jeden Sonntag um 11 Uhr UTC eine PDF-Datei zu generieren.

1.  Wählen Sie im Navigationspfad die Option **Aktivitätsauditing** aus.
    
2.  Klicken Sie unter **Zugehörige Ressourcen** auf **Auditberichte**.
    
3.  Klicken Sie auf der rechten Seite auf die Registerkarte **Benutzerdefinierte Berichte**.
    
4.  Klicken Sie in der Spalte **Berichtsname** in der Tabelle auf den Namen des benutzerdefinierten Berichts.
    
    Der benutzerdefinierte Bericht wird angezeigt.
    
5.  Klicken Sie auf **Berichtsplan verwalten**.
    
    Der Bereich **Berichtsplan verwalten** wird angezeigt.
    
6.  Geben Sie einen Namen für den Zeitplan ein. Beispiel: **All Activity HCM1 on your-database-name Schedule**.
    
7.  Vergewissern Sie sich, dass das Compartment ausgewählt ist.
    
8.  Lassen Sie **PDF** als Berichtsformat ausgewählt.
    
9.  Wählen Sie unter **Planhäufigkeit** die Option **Wöchentlich** aus.
    
10.  Wählen Sie unter **Alle** die Option **Sonntag** aus.
    
11.  Wählen Sie unter **Zeit (in UTC)** die Option **11 Uhr** aus.
    
12.  Übernehmen Sie für **Ereigniszeitspanne** **Letzte Tage** und **7** unverändert, sodass im Bericht nur Daten von einer Woche angezeigt werden.
    

![Bereich "Berichtsplan verwalten"](images/manage-report-schedule-panel.png "Bereich "Berichtsplan verwalten"")

13.  Klicken Sie auf **Plan speichern**.
    
    Der Bereich wird geschlossen, und Sie kehren zu Ihrem benutzerdefinierten Bericht zurück.
    
14.  Um den Zeitplan anzuzeigen, klicken Sie unter **Zugehörige Ressourcen** auf **Auditberichte**. Klicken Sie rechts auf die Registerkarte **Benutzerdefinierte Berichte**. Beachten Sie, dass jetzt ein Berichtsplan für Ihren benutzerdefinierten Bericht vorhanden ist. Sie können auf der Seite **Auditberichtshistorie** auf die vom Zeitplan generierten Berichte zugreifen.
    
    ![Benutzerdefinierter Bericht mit Zeitplan](images/custom-report-w-schedule.png "Benutzerdefinierter Bericht mit Zeitplan")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Überblick über Aktivitätsauditing](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-A73D8630-E59F-44C3-B467-F8E13041A680)
*   [Auditberichte anzeigen und verwalten](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-364B6431-9861-4B42-B24D-103D5F43B44A)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023