# Autonomous Database bei Oracle Data Safe registrieren

## Einführung

Um eine Datenbank mit Oracle Data Safe zu verwenden, müssen Sie sie zuerst bei Oracle Data Safe registrieren. Eine registrierte Datenbank wird in Oracle Data Safe als _Zieldatenbank_ bezeichnet.

Registrieren Sie die Datenbank mit dem Registrierungsassistenten für autonome Datenbanken. Navigieren Sie als Nächstes zu Oracle Data Safe, und zeigen Sie die Liste der registrierten Zieldatenbanken an, um zu bestätigen, dass Ihre Zieldatenbanken aufgeführt sind. Optional können Sie das Security Center erkunden, das der zentrale Hub für Oracle Data Safe ist, wo Sie auf Sicherheitsbewertung, Benutzerbewertung, Daten-Discovery, Datenmaskierung, Aktivitätsauditing, Alerts und das Oracle Data Safe-Dashboard zugreifen können.

Voraussichtliche Zeit: 5 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Datenbank bei Oracle Data Safe registrieren
*   Registrierte Zieldatenbank anzeigen
*   (Optional) Security Center untersuchen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account erhalten
*   Wir haben Ihre Umgebung für diesen Workshop vorbereitet. _Stellen Sie sicher, dass Beispieldaten in die Datenbank geladen sind._

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.
*   Bitte ignorieren Sie die Datumsangaben für die Daten. Screenshots werden zu verschiedenen Zeiten gemacht und können zwischen den Labors und innerhalb der Labors variieren.

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Autonomous Database bei Oracle Data Safe registrieren](videohub:1_hfjj07hm)

## Aufgabe 1: Datenbank bei Oracle Data Safe registrieren

1.  Stellen Sie sicher, dass Sie sich auf der Browserregisterkarte **Autonomous Database | Oracle Cloud Infrastructure** befinden. Sie haben zuletzt auf der Seite **Autonomous Database-Details** aufgehört.
    
2.  Wählen Sie im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus. Die Seite **Überblick** wird angezeigt. Wenn das Dialogfeld **Willkommen bei Data Safe** angezeigt wird, klicken Sie auf **Tour stoppen**.
    
    Auf dieser Seite gibt es Assistenten zum Registrieren der folgenden Datenbanktypen:
    
    *   Autonome Datenbanken
    *   Oracle Cloud-Datenbanken
    *   On-Premise-Oracle-Datenbanken
    *   Oracle-Datenbanken auf Compute
    *   Oracle Cloud@Customer-Datenbanken
    
    ![Registrierungsassistenten für Oracle Data Safe](images/registration-wizards.png "Registrierungsassistenten für Oracle Data Safe")
    
3.  Klicken Sie in der Kachel **Autonome Datenbanken** auf **Assistent starten**.
    
    Die erste Seite im Assistenten, **Datenbank auswählen**, wird angezeigt.
    
4.  Wählen Sie in der ersten Dropdown-Liste die Datenbank aus. Klicken Sie bei Bedarf auf **Compartment ändern**, wählen Sie das Compartment aus, und wählen Sie die Datenbank aus.
    
5.  (Optional) Ändern Sie den Standardanzeigenamen für die Zieldatenbank. Dieser Name wird in Ihren Oracle Data Safe-Berichten angezeigt.
    
6.  (Optional) Wählen Sie ein anderes Compartment aus, in dem die Zieldatenbank gespeichert werden soll. In der Regel wählen Sie das Compartment aus, in dem sich die Datenbank befindet.
    
7.  (Optional) Geben Sie eine Beschreibung für die Zieldatenbank ein.
    
8.  Beachten Sie die Meldung unten auf der Seite: **Die ausgewählte Datenbank ist so konfiguriert, dass sie von überall aus sicher zugänglich ist. Die Schritte 2 ("Konnektivitätsoption") und 3 ("Sicherheitsregel hinzufügen") sind nicht erforderlich und werden übersprungen.** Wenn Ihre Datenbank über eine private IP-Adresse verfügt, müssen Sie einen privaten Oracle Data Safe-Endpunkt und Sicherheitsregeln konfigurieren.
    
    ![Autonomous Database-Registrierungsassistent - Seite "Datenbank auswählen"](images/ocw/ADB-wizard-select-database.png "Autonomous Database-Registrierungsassistent - Seite "Datenbank auswählen"")
    
9.  Klicken Sie auf **Weiter**.
    
10.  Prüfen Sie die Informationen auf der Seite **Prüfen und weiterleiten**. Um eine Änderung vorzunehmen, können Sie zur Seite **Datenbank auswählen** zurückkehren.
    
    ![Autonomous Database-Registrierungsassistent - Seite "Prüfen und weiterleiten"](images/ocw/ADB-wizard-review-submit.png "Autonomous Database-Registrierungsassistent - Seite "Prüfen und weiterleiten"")
    
11.  Klicken Sie auf **Registrieren**.
    
    Die Seite **Registrierungsfortschritt** wird kurz angezeigt. Anschließend wird die Seite **Zieldatenbankdetails** angezeigt.
    
12.  Warten Sie, bis der Status der Zieldatenbank **ACTIVE** lautet. Das bedeutet, dass die Zieldatenbank vollständig registriert ist. Prüfen Sie während des Wartens die auf der Seite angegebenen Informationen und Optionen.
    
    *   Sie können den Namen und die Beschreibung der Zieldatenbank anzeigen/bearbeiten.
    *   Sie können die Oracle Cloud-ID (OCID) beim Registrieren der Zieldatenbank, den Compartment-Namen bei der Registrierung der Zieldatenbank, den Datenbanktyp (Autonomous Database) und das Verbindungsprotokoll (TLS) anzeigen. Die Informationen variieren je nach Zieldatenbanktyp.
    *   Sie haben Optionen zum Bearbeiten von Verbindungsdetails (wählen Sie eine Konnektivitätsoption aus), zum Verschieben der Zieldatenbank in ein anderes Compartment, zum Aufheben der Registrierung der Zieldatenbank und zum Hinzufügen von Tags.
    
    ![Details zur Zieldatenbank](images/ocw/target-database-details-page.png "Details zur Zieldatenbank")
    

## Aufgabe 2: Registrierte Zieldatenbank anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Zieldatenbanken**.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist. Die registrierte Zieldatenbank ist auf der rechten Seite aufgelistet.
    
    *   Eine Zieldatenbank mit dem Status **Aktiv** bedeutet, dass sie derzeit bei Oracle Data Safe registriert ist.
    *   Eine Zieldatenbank mit dem Status **Gelöscht** bedeutet, dass sie nicht mehr bei Oracle Data Safe registriert ist. Das Angebot wird 45 Tage nach Aufhebung der Registrierung der Zieldatenbank entfernt.
    
    ![Seite "Zieldatenbanken" in OCI](images/ocw/target-databases-page-oci.png "Seite "Zieldatenbanken" in OCI")
    

## Aufgabe 3: (Optional) Security Center untersuchen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Data Safe**.
    
    Die Seite **Übersicht** wird angezeigt.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Dashboard**, und prüfen Sie das Dashboard. Führen Sie einen Bildlauf nach unten durch, um alle Diagramme anzuzeigen. Stellen Sie sicher, dass das Compartment unter **Listengeltungsbereich** ausgewählt ist. Wählen Sie in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank aus, sodass die Daten im Dashboard nur für die Zieldatenbank gelten.
    
    *   Im Sicherheitscenter können Sie auf alle Oracle Data Safe-Features zugreifen, einschließlich Dashboard, Sicherheitsbewertung, Benutzerbewertung, Daten-Discovery, Datenmaskierung, Aktivitätsauditing und Alerts.
    *   Wenn Sie eine Zieldatenbank registrieren, erstellt Oracle Data Safe automatisch eine Sicherheitsbewertung und eine Benutzerbewertung für Sie. Aus diesem Grund haben die Diagramme **Sicherheitsbewertung**, **Benutzerbewertung**, **Featureverwendung** und **Vorgangsübersicht** im Dashboard bereits Daten.
    *   Während der Registrierung ermittelt Oracle Data Safe auch Audittrails in der Zieldatenbank. Aus diesem Grund wird im Diagramm **Audittrails** im Dashboard ein Audittrail mit dem Status **In Übergang** für Autonomous Database angezeigt.
    
    ![Anfängliches Dashboard - oberstes Halbjahr](images/ocw/dashboard-initial-top.png "Anfängliches Dashboard - oberstes Halbjahr")
    
    ![Anfängliches Dashboard - untere Hälfte](images/ocw/dashboard-initial-bottom.png "Anfängliches Dashboard - untere Hälfte")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Zieldatenbankregistrierung](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)
*   [Oracle Data Safe-Dashboard](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B4D784B8-F3F7-4020-891D-49D709B9A302)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 17. August 2023