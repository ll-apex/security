# Autonomous Database bei Oracle Data Safe registrieren

## Einführung

Um eine Datenbank mit Oracle Data Safe zu verwenden, müssen Sie sie zuerst bei Oracle Data Safe registrieren. Eine registrierte Datenbank wird in Oracle Data Safe als _Zieldatenbank_ bezeichnet.

Erkundigen Sie sich zunächst nach Optionen für die Registrierung von Zieldatenbanken, und registrieren Sie dann die Datenbank mit dem Assistenten. Navigieren Sie als Nächstes zu Oracle Data Safe, und zeigen Sie die Liste der registrierten Zieldatenbanken an, um zu bestätigen, dass Ihre Zieldatenbanken aufgeführt sind. Entdecken Sie das Security Center, den zentralen Hub für Oracle Data Safe, in dem Sie auf Sicherheitsbewertung, Benutzerbewertung, Daten-Discovery, Datenmaskierung, Aktivitätsauditing, Alerts und das Oracle Data Safe-Dashboard zugreifen können.

Geschätzte Laborzeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Optionen für die Zieldatenbankregistrierung untersuchen
*   Registrieren Sie die Datenbank mit dem Assistenten bei Oracle Data Safe
*   Greifen Sie auf Oracle Data Safe zu, und zeigen Sie die Liste der registrierten Zieldatenbanken an
*   Sicherheitscenter erkunden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account erhalten
*   Die Umgebung für diesen Workshop wurde vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment)). _Stellen Sie sicher, dass Beispieldaten in die Datenbank geladen sind._

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.
*   Bitte ignorieren Sie die Datumsangaben für die Daten. Screenshots werden zu verschiedenen Zeiten gemacht und können zwischen den Labors und innerhalb der Labors variieren.

## Aufgabe 1: Registrierungsoptionen für Zieldatenbanken untersuchen

Sie haben drei Optionen für die Registrierung Ihrer Autonomous Database:

*   Verwenden Sie den Link **Registrieren** auf der Seite **Autonomous Database-Details** (Ein-Klick-Methode ohne Interaktion).
*   Verwenden Sie den Assistenten für autonome Datenbanken auf der Seite **Überblick** für den Oracle Data Safe-Service (geführte Methode mit Anpassungsoptionen).
*   Registrieren Sie die Zieldatenbank manuell auf der Seite **Registrierte Ziele** (erweiterte Methode ohne Anleitung).

1.  Kehren Sie zur Browserregisterkarte **Autonomous Database | Oracle Cloud Infrastructure** zurück. Sie haben zuletzt auf der Seite **Autonomous Database-Details** aufgehört.
    
    Wenn Sie diese Seite verlassen haben: Wählen Sie im Navigationsmenü **Oracle Database**, **Autonomous Transaction Processing** aus. Wählen Sie gegebenenfalls das Compartment aus, und klicken Sie auf den Namen der Datenbank.
    
2.  Scrollen Sie auf der Seite nach unten, und beachten Sie unter **Data Safe**, dass die Option **Registrieren** vorhanden ist. Klicken Sie nicht auf den Link, sondern zeigen Sie die anderen Optionen an.
    
    ![Registrierungsoption für Ihre Datenbank](images/register-database.png "Registrierungsoption für Ihre Datenbank")
    
3.  Wählen Sie im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus. Die Seite **Überblick** wird angezeigt. Wenn das Dialogfeld **Willkommen bei Data Safe** angezeigt wird, klicken Sie auf **Tour stoppen**.
    
    Auf dieser Seite gibt es Assistenten zum Registrieren der folgenden Datenbanktypen:
    
    *   Autonome Datenbanken
    *   Oracle Cloud-Datenbanken
    *   On-Premise-Oracle-Datenbanken
    *   Oracle-Datenbanken auf Compute
    *   Oracle Cloud@Customer-Datenbanken
    
    ![Registrierungsassistenten für Oracle Data Safe](images/registration-wizards.png "Registrierungsassistenten für Oracle Data Safe")
    
4.  Klicken Sie links unter **Data Safe** auf **Zieldatenbanken**.
    
5.  Klicken Sie rechts auf **Datenbank registrieren**. Hier können Sie Registrierungsdetails konfigurieren. Bei dieser Methode wird davon ausgegangen, dass Sie die erforderlichen Vorregistrierungsaufgaben für Ihre Datenbank bereits abgeschlossen haben.
    
    ![Manuelle Zielregistrierung](images/manual-target-registration.png "Manuelle Zielregistrierung")
    
6.  Klicken Sie auf **Abbrechen**.
    

## Aufgabe 2: Datenbank mit dem Assistenten bei Oracle Data Safe registrieren

Um eine andere Datenbank als eine ATP-Datenbank für diesen Workshop zu registrieren, befolgen Sie die für Ihren Datenbanktyp spezifischen Registrierungsanweisungen im Handbuch _Oracle Data Safe verwalten_. Weitere Informationen finden Sie im Abschnitt **Weitere Informationen** unten auf dieser Seite.

1.  Klicken Sie auf **Datenbank über Assistenten registrieren**.
    
    Die Seite **Übersicht** wird angezeigt.
    
2.  Klicken Sie in der Kachel **Autonome Datenbanken** auf **Assistent starten**.
    
    Die erste Seite im Assistenten mit dem Namen **Datenbank auswählen** wird angezeigt.
    
3.  Wählen Sie in der ersten Dropdown-Liste die Datenbank aus. Klicken Sie bei Bedarf auf **Compartment ändern**, wählen Sie das Compartment aus, und wählen Sie die Datenbank aus.
    
4.  (Optional) Ändern Sie den Standardanzeigenamen für die Zieldatenbank. Dieser Name wird in Ihren Oracle Data Safe-Berichten angezeigt.
    
5.  (Optional) Wählen Sie ein anderes Compartment aus, in dem die Zieldatenbank gespeichert werden soll. In der Regel wählen Sie das Compartment aus, in dem sich die Datenbank befindet.
    
6.  (Optional) Geben Sie eine Beschreibung für die Zieldatenbank ein.
    
7.  Beachten Sie die Meldung unten auf der Seite: **Die ausgewählte Datenbank ist so konfiguriert, dass sie von überall aus sicher zugänglich ist. Die Schritte 2 ("Konnektivitätsoption") und 3 ("Sicherheitsregel hinzufügen") sind nicht erforderlich und werden übersprungen.** Wenn Ihre Datenbank über eine private IP-Adresse verfügt, müssen Sie einen privaten Oracle Data Safe-Endpunkt und Sicherheitsregeln konfigurieren.
    
    ![Autonomous Database-Registrierungsassistent - Seite "Datenbank auswählen"](images/ADB-wizard-select-database.png "Autonomous Database-Registrierungsassistent - Seite "Datenbank auswählen"")
    
8.  Klicken Sie auf **Weiter**.
    
9.  Prüfen Sie die Informationen auf der Seite **Prüfen und weiterleiten**. Um eine Änderung vorzunehmen, können Sie zur Seite **Datenbank auswählen** zurückkehren.
    
    ![Autonomous Database-Registrierungsassistent - Seite "Prüfen und weiterleiten"](images/ADB-wizard-review-submit.png "Autonomous Database-Registrierungsassistent - Seite "Prüfen und weiterleiten"")
    
10.  Klicken Sie auf **Registrieren**.
    
    Die Seite **Registrierungsfortschritt** wird kurz angezeigt. Anschließend wird die Seite **Zieldatenbankdetails** angezeigt.
    
11.  Warten Sie, bis der Status der Zieldatenbank **ACTIVE** lautet. Das bedeutet, dass die Zieldatenbank vollständig registriert ist. Prüfen Sie als Nächstes die Informationen und Optionen auf der Seite.
    
    *   Sie können den Namen und die Beschreibung der Zieldatenbank anzeigen/bearbeiten.
    *   Sie können die Oracle Cloud-ID (OCID) beim Registrieren der Zieldatenbank, den Compartment-Namen bei der Registrierung der Zieldatenbank, den Datenbanktyp (Autonomous Database) und das Verbindungsprotokoll (TLS) anzeigen. Die Informationen variieren je nach Zieldatenbanktyp.
    *   Sie haben Optionen zum Bearbeiten der Verbindungsdetails (Ändern des Verbindungsprotokolls), zum Verschieben der Zieldatenbank in ein anderes Compartment, zum Aufheben der Registrierung der Zieldatenbank und zum Hinzufügen von Tags.
    
    ![Details zur Zieldatenbank](images/target-database-details-page.png "Details zur Zieldatenbank")
    

## Aufgabe 3: Auf Oracle Data Safe zugreifen und die Liste der registrierten Zieldatenbanken anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Zieldatenbanken**.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist. Die registrierte Zieldatenbank ist auf der rechten Seite aufgelistet.
    
    *   Eine Zieldatenbank mit dem Status **Aktiv** bedeutet, dass sie derzeit bei Oracle Data Safe registriert ist.
    *   Eine Zieldatenbank mit dem Status **Gelöscht** bedeutet, dass sie nicht mehr bei Oracle Data Safe registriert ist. Das Angebot wird 45 Tage nach Aufhebung der Registrierung der Zieldatenbank entfernt.
    
    ![Seite "Zieldatenbanken" in OCI](images/target-databases-page-oci.png "Seite "Zieldatenbanken" in OCI")
    

## Aufgabe 4: Sicherheitscenter erkunden

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Data Safe**.
    
    Die Seite **Übersicht** wird angezeigt.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Dashboard**, und prüfen Sie das Dashboard. Führen Sie einen Bildlauf nach unten durch, um alle Diagramme anzuzeigen. Stellen Sie sicher, dass das Compartment unter **Listengeltungsbereich** ausgewählt ist. Wählen Sie in der Dropdown-Liste **Zieldatenbanken** die Zieldatenbank aus, sodass die Daten im Dashboard nur für die Zieldatenbank gelten.
    
    *   Im Sicherheitscenter können Sie auf alle Oracle Data Safe-Features zugreifen, einschließlich Dashboard, Sicherheitsbewertung, Benutzerbewertung, Daten-Discovery, Datenmaskierung, Aktivitätsauditing und Alerts.
    *   Wenn Sie eine Zieldatenbank registrieren, erstellt Oracle Data Safe automatisch eine Sicherheitsbewertung und eine Benutzerbewertung für Sie. Aus diesem Grund haben die Diagramme **Sicherheitsbewertung**, **Benutzerbewertung**, **Featureverwendung** und **Vorgangsübersicht** im Dashboard bereits Daten.
    *   Während der Registrierung ermittelt Oracle Data Safe auch Audittrails in der Zieldatenbank. Aus diesem Grund wird im Diagramm **Audittrails** im Dashboard ein Audittrail mit dem Status **In Übergang** für Autonomous Database angezeigt. Später starten Sie diesen Audittrail, um Auditdaten in Oracle Data Safe zu erfassen.
    
    ![Anfängliches Dashboard - oberstes Halbjahr](images/dashboard-initial-top.png "Anfängliches Dashboard - oberstes Halbjahr")
    
    ![Anfängliches Dashboard - untere Hälfte](images/dashboard-initial-bottom.png "Anfängliches Dashboard - untere Hälfte")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Zieldatenbankregistrierung](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)
*   [Oracle Data Safe-Dashboard](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B4D784B8-F3F7-4020-891D-49D709B9A302)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023