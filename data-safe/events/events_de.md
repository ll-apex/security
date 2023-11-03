# Lassen Sie sich über Sicherheitsabweichungen in Ihren Zieldatenbanken benachrichtigen, indem Sie Oracle Data Safe-Ereignisse einrichten

In dieser Übung konfigurieren Sie den Events-Service so, dass Sie per E-Mail benachrichtigt werden, wenn Sicherheitsabweichungen in der Zieldatenbank auftreten.

Geschätzte Laborzeit: 20 Minuten

## Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Aktuelle Benutzerbewertung prüfen
*   Letzte Benutzerbewertung als Baseline festlegen
*   Benachrichtigungsthema und Abonnement erstellen
*   Regel im Events-Service erstellen
*   Aktivität in der Zieldatenbank generieren
*   Letzte Benutzerbewertung aktualisieren und Ergebnisse analysieren
*   Vergleichsbericht für Benutzerbewertung generieren
*   Prüfen Sie Ihre E-Mail-Benachrichtigung

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   _Administratorberechtigungen_ in Ihrem Mandanten
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))
*   Eine gültige E-Mail-Adresse

### Annahmen

*   Ihre Datenwerte können sich von den Werten in den Screenshots unterscheiden.

## Aufgabe 1: Letzte Benutzerbewertung prüfen

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Oracle Database**, **Data Safe - Datenbanksicherheit** aus.
    
2.  Klicken Sie unter **Sicherheitscenter** auf **Benutzerbewertung**.
    
    Das Benutzerbewertungs-Dashboard wird angezeigt.
    
3.  Klicken Sie auf die Registerkarte **Zielübersicht**.
    
4.  Klicken Sie in der Spalte **Zuletzt bewertet am** auf **Bericht anzeigen**, um die letzte Benutzerbewertung anzuzeigen.
    
5.  Prüfen Sie die Diagramme auf der Registerkarte **Überblick**.
    
    ![Letzte Benutzerbewertung - Registerkarte "Überblick"](images/latest-ua-overview-tab.png "Letzte Benutzerbewertung - Registerkarte "Überblick"")
    
6.  Scrollen Sie nach unten, und prüfen Sie die Informationen im Abschnitt **Benutzerdetails**.
    
    ![Letzte Benutzerbewertung - Abschnitt "Benutzerdetails"](images/latest-ua-user-details-section.png "Letzte Benutzerbewertung - Abschnitt "Benutzerdetails"")
    

## Aufgabe 2: Letzte Benutzerbewertung als Baseline festlegen

1.  Während Sie noch die letzte Benutzerbewertung anzeigen, klicken Sie auf **Als Baseline festlegen**.
    
    Das Dialogfeld **Als Baseline festlegen?** wird mit der Frage angezeigt, ob Sie sicher sind.
    
2.  Klicken Sie auf **Ja**, und bleiben Sie auf der Seite, bis die folgende Meldung angezeigt wird:
    
    `Baseline has been set.`
    
3.  Klicken Sie auf **Historie anzeigen**. Beachten Sie, dass in Ihrem Compartment eine Baselinebewertung vorhanden ist.
    
    ![Bewertungshistorie (Seite)](images/assessment-history.png "Bewertungshistorie (Seite)")
    

## Aufgabe 3: Benachrichtigungsthema und Abonnement erstellen

Um ein Benachrichtigungsthema zu erstellen, benötigen Sie einen Mandantenadministrator.

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure **Entwicklerservices**, **Anwendungsintegration** **Benachrichtigungen** aus.
    
    Die Seite **Benachrichtigungen** wird angezeigt.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist.
    
3.  Klicken Sie auf **Thema erstellen**.
    
    Der Bereich **Thema erstellen** wird angezeigt.
    
4.  Geben Sie einen Themennamen ein. Beispiel: **security-drift**.
    
5.  Klicken Sie auf **Erstellen**.
    
6.  Klicken Sie auf den Namen des Themas.
    
    Die Seite **Themendetails** wird angezeigt.
    
7.  Klicken Sie auf **Abonnement erstellen**.
    
    Der Bereich **Abonnement erstellen** wird angezeigt.
    
8.  Lassen Sie für **Protokoll** **E-Mail** ausgewählt.
    
9.  Geben Sie unter **E-Mail** Ihre E-Mail-Adresse ein.
    
10.  Klicken Sie auf **Erstellen**.
    
    Der Status des Abonnements lautet **Ausstehend**.
    
11.  Öffnen Sie Ihre E-Mail-Anwendung, und suchen Sie die E-Mail von Oracle. Klicken Sie in der E-Mail auf **Abonnement bestätigen**.
    
    Im Browser wird die Seite **Abonnement bestätigt** angezeigt.
    
12.  Aktualisieren Sie die Seite **Themendetails**. Beachten Sie, dass der Status des Abonnements jetzt auf **Aktiv** gesetzt ist.
    
    ![Aktives Abonnement](images/active-subscription.png "Aktives Abonnement")
    

## Aufgabe 4: Regel im Events-Service erstellen

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Optionen **Observability and Management**, **Events-Service** aus.
    
2.  Stellen Sie unter **Listengeltungsbereich** sicher, dass das Compartment ausgewählt ist.
    
3.  Klicken Sie auf **Regel erstellen**.
    
4.  Geben Sie unter **Anzeigename** **Sicherheitsabweichung** ein.
    
5.  Geben Sie unter **Beschreibung** **E-Mail-Benachrichtigung senden, wenn eine Benutzerbewertung mit einer Baselinebewertung verglichen wird und eine Differenz gefunden wird** ein.
    
6.  Lassen Sie im Abschnitt **Regelbedingungen** **Ereignistyp** als Bedingung ausgewählt.
    
7.  Wählen Sie unter **Servicename** die Option **Data Safe** aus.
    
8.  Wählen Sie unter **Ereignistyp** die Option **Benutzerbewertungsabweichung von Baseline** aus.
    
9.  Klicken Sie auf **Beispielereignisse (JSON) anzeigen**, und prüfen Sie die Regellogik. Dies sind die Informationen, die Sie in Ihrer E-Mail erhalten. Klicken Sie auf **Abbrechen**, um den Bereich zu schließen.
    
10.  Wählen Sie unter **Aktionstyp** die Option **Benachrichtigungen** aus.
    
11.  Wählen Sie unter **Benachrichtigungen - Compartment** Ihr Compartment aus.
    
12.  Wählen Sie unter **Thema** das soeben erstellte Thema aus (z.B. **security-drift**).
    
    ![Regel erstellen](images/create-rule-page.png "Regel erstellen")
    
13.  Klicken Sie auf **Regel erstellen**.
    
    Die Seite **Sicherheitsabweichung** wird angezeigt.
    
    ![Sicherheitsabweichung](images/security-drift-page.png "Sicherheitsabweichung")
    

## Aufgabe 5: Aktivität in der Zieldatenbank generieren

In dieser Aufgabe erstellen Sie einen Benutzer in der Zieldatenbank mit der Rolle `PDB_DBA`.

1.  Greifen Sie in Database Actions auf das SQL-Arbeitsblatt zu. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an.
    
2.  Löschen Sie bei Bedarf das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
3.  Geben Sie im Arbeitsblatt folgenden Befehl ein:
    
        <copy>CREATE USER joe_smith identified by Oracle123_Oracle123;
        GRANT PDB_DBA to joe_smith;</copy>
        
4.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil).
    
    ![Schaltfläche "Anweisung ausführen"](images/run-statement-button.png "Schaltfläche "Anweisung ausführen"")
    

## Aufgabe 6: Letzte Benutzerbewertung aktualisieren und Ergebnisse analysieren

1.  Kehren Sie zur Browserregisterkarte für Oracle Cloud Infrastructure zurück.
    
2.  Wählen Sie im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus.
    
3.  Klicken Sie unter **Sicherheitscenter** auf **Benutzerbewertung**.
    
4.  Klicken Sie auf die Registerkarte **Zielübersicht**.
    
5.  Klicken Sie auf **Bericht anzeigen**, um die letzte Benutzerbewertung anzuzeigen.
    
6.  Klicken Sie oben im letzten Benutzertest auf **Jetzt aktualisieren**, um die neuesten Daten abzurufen.
    
    Der Bereich **Jetzt aktualisieren** wird angezeigt.
    
7.  Behalten Sie den Standardbewertungsnamen bei, und klicken Sie auf **Jetzt aktualisieren**. Warten Sie, bis der Status **Erfolgreich** lautet.
    
    *   Mit dieser Aktion werden die Daten in der letzten Benutzerbewertung für die Zieldatenbank aktualisiert. Außerdem wird eine Kopie der Bewertung in der Bewertungshistorie gespeichert.
    *   Der Aktualisierungsvorgang dauert etwa eine Minute.
8.  Klicken Sie auf **Historie anzeigen**.
    
9.  Vergleichen Sie die Risikowerte zwischen der Baselinebewertung und der soeben generierten neuen Bewertung. Gibt es Unterschiede?
    
    ![Bewertungshistorie nach](images/assessment-history-after.png "Bewertungshistorie nach")
    
10.  Klicken Sie auf **Schließen**.
    

## Aufgabe 7: Vergleichsbericht für Benutzerbewertung generieren

Nachdem Sie einen Vergleichsbericht generiert haben, muss der Events-Service eine E-Mail-Benachrichtigung auslösen, wenn eine Sicherheitsabweichung vorliegt (da Sie einen privilegierten Benutzer hinzugefügt haben sollten).

1.  Wenn die letzte Benutzerbewertung angezeigt wird, klicken Sie links unter **Ressourcen** auf **Mit Baseline vergleichen**. Oracle Data Safe beginnt automatisch mit der Verarbeitung des Vergleichs.
    
2.  Wenn der Vergleichsvorgang abgeschlossen ist, prüfen Sie den Bericht **Vergleich**. Klicken Sie auf **Details öffnen**, um weitere Informationen anzuzeigen.
    
    ![Fensterbereich "Vergleichsdetails"](images/comparison-details-panel.png "Fensterbereich "Vergleichsdetails"")
    

## Aufgabe 8: E-Mail-Benachrichtigung prüfen

1.  Öffnen Sie Ihre E-Mail-Anwendung.
    
2.  Suchen und öffnen Sie die E-Mail-Benachrichtigung von Oracle. Die Meldung enthält folgenden Text:
    
        <copy>{
        "eventType" : "com.oraclecloud.datasafe.userassessmentdriftfrombaseline",
        "cloudEventsVersion" : "0.1",
        "eventTypeVersion" : "2.0",
        "source" : "DataSafe",
        "eventTime" : "2023-02-16T19:54:52Z",
        "contentType" : "application/json",
        "data" : {
            "compartmentId" : "ocid1.compartment.oc1...",
            "compartmentName" : "compartment-name",
            "resourceName" : "userAssessment",
            "resourceId" : "not applicable",
            "availabilityDomain" : "ad3",
            "additionalDetails" : {
            "targetName" : "ATP2000",
            "comparedWith" : "ocid1.datasafeuserassessment.oc1..."
            }
        },
        "eventID" : "e46fad7b-ac11...",
        "extensions" : {
            "compartmentId" : "ocid1.compartment.oc1..."
        }
        }
        
        --
        You are receiving notifications as a subscriber to the topic: security-drift (Topic OCID: ocid1.onstopic.oc1.eu-frankfurt-1.aaaaa...). 
        To stop receiving notifications from this topic, unsubscribe: https://cell1.notification.eu-frankfurt-1.oci.oraclecloud.com/20181201/subscriptions/ocid1.onssubscription.oc1.eu-frankfurt-1.aaaaa.../unsubscription?token=YVpsOE4weTU4TTdKSGxoTkVwR3kyaU8...==&protocol=EMAIL
        
        Please do not reply directly to this email. If you have any questions or comments regarding this email, contact your account administrator.
        
        Oracle Corporation - Worldwide Headquarters
        2300 Oracle Way, Austin, Texas 78741 USA</copy>
        

## Weitere Informationen

*   [Ereignistypen für Oracle Data Safe](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-A8D65EBC-9A53-43EC-B335-0DA0E2F9CDC8)
*   [Ereignisse in Oracle Cloud Infrastructure](https://docs.oracle.com/en-us/iaas/Content/Events/home.htm)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Mitwirkende** - Bettina Schaeumer
*   **Zuletzt aktualisiert am/um** - Jody Glover, 11. April 2023