# Sensible Daten maskieren

## Einführung

Mit der Datenmaskierung können Sie sensible Daten maskieren, damit die Daten für Nicht-Produktionszwecke sicher sind. Beispielsweise müssen Unternehmen häufig Kopien ihrer Produktionsdaten erstellen, um Entwicklungs- und Testaktivitäten zu unterstützen. Durch einfaches Kopieren der Produktionsdaten werden sensible Daten für neue Benutzer freigegeben. Um ein Sicherheitsrisiko zu vermeiden, können Sie mit der Datenmaskierung sensible Daten durch realistische, aber fiktive Daten ersetzen.

Maskieren Sie die sensiblen Daten, die Sie in der Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data) ermittelt haben, indem Sie die vom Datenmaskierungsfeature generierte Standardmaskierungs-Policy verwenden. Zeigen Sie die Vorher-Nachher-Auswirkung auf die maskierten Daten mit Database Actions an.

Geschätzte Laborzeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erteilen Sie die Rolle "Datenmaskierung" in der Zieldatenbank
*   Vertrauliche Daten in der Zieldatenbank anzeigen
*   Maskierungs-Policy für die Zieldatenbank erstellen
*   Vertrauliche Daten in der Zieldatenbank mit Datenmaskierung maskieren
*   Data Masking-Bericht anzeigen
*   PDF des Datenmaskierungsberichts erstellen
*   Maskierte Daten in der Zieldatenbank validieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank wurde bei Oracle Data Safe registriert. Stellen Sie sicher, dass das Kennwort `ADMIN` für die Datenbank verfügbar ist (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database)).
*   Erstellt ein Modell für sensible Daten (siehe [Sensible Daten ermitteln](?lab=discover-sensitive-data))

### Annahmen

*   Ihre Datenwerte können sich von den Werten in den Screenshots unterscheiden.

## Aufgabe 1: Rolle "Datenmaskierung" in der Zieldatenbank erteilen

Um das Datenmaskierungsfeature mit **Serverlos von Autonomous Database (mit sicherem Zugriff von überall aus)** zu verwenden, müssen Sie zuerst die Datenmaskierungsrolle dem vordefinierten Oracle Data Safe-Serviceaccount in der Datenbank erteilen. Wenn Sie eine andere Art von Zieldatenbank verwenden, finden Sie im Handbuch _Oracle Data Safe verwalten_ Anweisungen zum Erteilen der erforderlichen Rollen.

1.  Greifen Sie in Database Actions auf das SQL-Arbeitsblatt zu. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an. Löschen Sie das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
2.  Geben Sie im SQL-Arbeitsblatt den folgenden Befehl ein, um dem Oracle Data Safe-Serviceaccount in der Zieldatenbank die Datenmaskierungsrolle zu erteilen.
    
        <copy>EXECUTE DS_TARGET_UTIL.GRANT_ROLE('DS$DATA_MASKING_ROLE');</copy>
        
3.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil), um die Abfrage auszuführen.
    
    ![Schaltfläche "Anweisung ausführen" in Symbolleiste](images/run-statement-button.png "Schaltfläche "Anweisung ausführen" in Symbolleiste")
    
4.  Prüfen Sie, ob die Skriptausgabe wie folgt lautet:
    
    `PL/SQL procedure successfully completed`
    
    Sie können jetzt sensible Daten in der Zieldatenbank maskieren.
    
5.  Löschen Sie die Arbeitsblatt- und Skriptausgabe.
    

## Aufgabe 2: Sensible Daten in der Zieldatenbank anzeigen

In der Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data) haben Sie erfahren, dass die Tabelle `HCM1.EMPLOYEES` sensible Daten enthält. In dieser Aufgabe zeigen Sie die tatsächlichen sensiblen Daten in dieser Tabelle an.

1.  Wählen Sie in der Registerkarte **Navigator** in der ersten Dropdown-Liste das Schema **HCM1** aus.
    
2.  Ziehen Sie die Tabelle `EMPLOYEES` in das Arbeitsblatt.
    
    ![EMPLOYEES (Tabelle)](images/drag-employees-table-to-worksheet.png "EMPLOYEES (Tabelle)")
    
3.  Wenn Sie zur Auswahl eines Einfügetyps aufgefordert werden, klicken Sie auf **Auswählen** und dann auf **Anwenden**.
    
    ![Typ des Einfüge-Dialogfelds auswählen](images/insertion-type-select.png "Typ des Einfüge-Dialogfelds auswählen")
    
4.  Zeigen Sie die SQL-Abfrage im Arbeitsblatt an.
    
    ![Registerkarte "Arbeitsblatt" mit Tabelle EMPLOYEES](images/query-employees-table.png "Registerkarte "Arbeitsblatt" mit Tabelle EMPLOYEES")
    
5.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen**.
    
    ![Schaltfläche "Skript ausführen"](images/run-script.png "Schaltfläche "Skript ausführen"")
    
6.  Prüfen Sie auf der Registerkarte **Skriptausgabe** die Abfrageergebnisse.
    
    *   Daten wie `EMPLOYEE_ID`, `FIRST_NAME`, `LAST_NAME`, `EMAIL`, `PHONE_NUMBER` und `HIRE_DATE` werden als sensible Daten betrachtet und müssen maskiert werden, wenn sie für Nicht-Produktionszwecke freigegeben werden.
7.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück. Lassen Sie diesen Browser-Tab geöffnet, weil Sie später wieder darauf zugreifen.
    

## Aufgabe 3: Maskierungs-Policy für die Zieldatenbank erstellen

Die Datenmaskierung kann eine Maskierungs-Policy für die Zieldatenbank basierend auf dem Modell für sensible Daten generieren. Es wird automatisch versucht, ein Standardmaskierungsformat für jede sensible Spalte auszuwählen. Sie können diese Standardauswahl bearbeiten und bei Bedarf andere auswählen. Gelegentlich werden Sie möglicherweise aufgefordert, Probleme (sofern vorhanden) in den Maskierungsformaten zu beheben.

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Data Safe**.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Datenmaskierung**.
    
3.  Klicken Sie unter **Zugehörige Ressourcen** auf **Maskierungs-Policys**.
    
4.  Wählen Sie links unter **Listengeltungsbereich** Ihr Compartment aus. Auf der Seite **Maskierungs-Policys** wird angezeigt, dass keine Maskierungs-Policys für die Zieldatenbank verfügbar sind.
    
    ![Maskierungs-Policys (Seite)](images/no-masking-policies-available.png "Maskierungs-Policys (Seite)")
    
5.  Klicken Sie auf **Maskierungs-Policy erstellen**.
    
    Der Bereich **Migrations-Policy erstellen** wird angezeigt.
    
6.  Konfigurieren Sie die Maskierungs-Policy wie folgt:
    
    *   Name: **Mask SDM1**
    *   Compartment: **Wählen Sie Ihr Compartment aus**
    *   Beschreibung: **Maskierungs-Policy für SDM1**
    *   Wählen Sie aus, wie Sie die Maskierungs-Policy erstellen möchten: Lassen Sie **Modell für sensible Daten verwenden** ausgewählt.
    *   Modell für sensible Daten: Wählen Sie **SDM1\[Ihr Ziel-Datenbankname\]** aus. Wenn dieses Modell für sensible Daten nicht vorhanden ist, wird auf die Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data) verwiesen.
    
    ![Bereich "Maskierungs-Policy erstellen" mit SDM1](images/create-masking-policy-sdm1.png "Bereich "Maskierungs-Policy erstellen" mit SDM1")
    
7.  Klicken Sie auf **Maskierungs-Policy erstellen**.
    
    _Wichtig: Schließen Sie den Bereich nicht. Sie wird nach Abschluss aller Vorgänge automatisch geschlossen. Wenn Sie den Bereich schließen, bevor die Vorgänge abgeschlossen sind, wird der Vorgang zum Hinzufügen von Spalten zur Maskierungs-Policy nicht initiiert._
    
    Die Seite **Maskierungs-Policy-Details** wird angezeigt.
    
8.  Prüfen Sie die Maskierungs-Policy.
    
    *   Auf der Registerkarte **Informationen zur Maskierungs-Policy** können Sie den Namen der Maskierungs-Policy anzeigen (und bearbeiten), die Oracle Cloud-ID (OCID) für die Maskierungs-Policy, das Compartment, in dem die Maskierungs-Policy gespeichert wird, einen Link zu die Arbeitsanforderung für die Maskierungs-Policy, einen Link zu Maskierungsoptionen, die Zieldatenbank und das Modell für sensible Daten, mit dem die Maskierungs-Policy verknüpft ist, und das Datum/die Uhrzeit, zu dem/der die Maskierungs-Policy erstellt und zuletzt aktualisiert wurde.
    *   In der Tabelle **Maskierungsspalten** werden alle Maskierungsspalten und ihre Maskierungsformate aufgeführt. Bei Bedarf können Sie ein anderes Maskierungsformat für jede Maskierungsspalte auswählen. Sie können auf das Stiftsymbol neben einem Maskierungsformat klicken, um es zu bearbeiten.
    
    ![Seite "Maskierungs-Policy-Details" für Maske SDM1 - oben](images/masking-policy-details-page-mask-sdm1-top.png "Seite "Maskierungs-Policy-Details" für Maske SDM1 - oben") ![Seite "Maskierungs-Policy-Details" für Maske SDM1 - unten](images/masking-policy-details-page-mask-sdm1-bottom.png "Seite "Maskierungs-Policy-Details" für Maske SDM1 - unten")
    
9.  Klicken Sie links unter **Ressourcen** auf **Zu bearbeitende Maskierungsspalten**.
    
    Der Abschnitt **Zu bearbeitende Maskierungsspalten** wird unten auf der Seite angezeigt. In diesem Abschnitt werden Sie über Maskierungsspalten informiert, die kein ordnungsgemäß konfiguriertes Maskierungsformat aufweisen. Der folgende Screenshot zeigt ein Beispiel, in dem keine Maskierungsspalten vorhanden sind, die Aufmerksamkeit erfordern.
    
    ![Abschnitt "Zu bearbeitende Maskierungsspalten"](images/masking-columns-needing-attention.png "Abschnitt "Zu bearbeitende Maskierungsspalten"")
    

## Aufgabe 4: Sensible Daten in der Zieldatenbank mit Datenmaskierung maskieren

Nachdem Sie eine Maskierungs-Policy erstellt haben, können Sie einen Datenmaskierungsjob für die Zieldatenbank auf der Seite **Maskierungs-Policy-Details** ausführen. Sie können einen Datenmaskierungsjob auch auf der Seite **Datenmaske** ausführen.

1.  Klicken Sie auf der Seite **Maskierungs-Policy-Details** auf **Maskierungsziel**.
    
    Der Bereich **Vertrauliche Daten maskieren** wird angezeigt.
    
2.  Wählen Sie in der Dropdown-Liste **Zieldatenbank** die Zieldatenbank aus, und klicken Sie auf **Daten maskieren**.
    
    ![Bereich "Sensible Daten maskieren"](images/mask-sensitive-data-panel.png "Bereich "Sensible Daten maskieren"")
    
    Die Seite **Arbeitsanforderung** wird angezeigt.
    
3.  Überwachen Sie den Fortschritt der Arbeitsanforderung, indem Sie die Logmeldungen in der Tabelle **Logmeldungen** anzeigen.
    
    ![Logmeldungen für Datenmaskierungsarbeitsanforderung](images/masking-log-messages.png "Logmeldungen für Datenmaskierungsarbeitsanforderung")
    
4.  Warten Sie, bis der Status **Erfolgreich** lautet.
    
    ![Seite "Arbeitsanforderung" für Maskierungsjob erfolgreich](images/work-request-masking-job-succeeded.png "Seite "Arbeitsanforderung" für Maskierungsjob erfolgreich")
    

## Aufgabe 5: Datenmaskierungsbericht anzeigen

1.  Klicken Sie auf der Seite **Arbeitsanforderung** auf der Registerkarte **Arbeitsanforderungsinformationen** neben **Maskierungsbericht** auf **Details anzeigen**.
    
    Die Seite **Maskierungsberichtsdetails** wird angezeigt.
    
2.  Prüfen Sie den Maskierungsbericht.
    
    *   Auf der Registerkarte **Maskierungsberichtsinformationen** werden der Name der Zieldatenbank, der Name der Maskierungs-Policy (Sie können auf einen Link klicken, um ihn anzuzeigen), die Oracle Cloud-ID (OCID) für den Maskierungsbericht, das Datum und die Uhrzeit des Starts und Endes des Datenmaskierungsjobs sowie die Anzahl der maskierten sensiblen Typen, Schemas, Tabellen, Spalten und Werte angezeigt. Sie können auf einen Link klicken, um Maskierungsoptionen anzuzeigen. Außerdem gibt es ein Tortendiagramm, in dem die Prozentwerte für maskierte Werte für jeden sensiblen Typ angezeigt werden. Sie können auf ein Kreissegment klicken, um einen Drilldown in das Diagramm durchzuführen.
    *   In der Tabelle **Maskierte Spalten** werden alle maskierten sensiblen Spalten sowie das jeweilige Schema, die Tabelle, das Maskierungsformat, der sensible Typ, die übergeordnete Spalte und die Gesamtanzahl der maskierten Werte aufgeführt.
    
    ![Maskierungsbericht oben](images/masking-report-top2.png "Maskierungsbericht oben") ![Maskierungsbericht unten](images/masking-report-bottom.png "Maskierungsbericht unten")
    

## Aufgabe 6: PDF des Datenmaskierungsberichts erstellen

1.  Klicken Sie oben auf der Seite **Maskierungsberichtsdetails** auf **Bericht generieren**.
    
    Das Dialogfeld **Bericht generieren** wird angezeigt.
    
2.  Lassen Sie **PDF** ausgewählt, und klicken Sie auf **Bericht generieren**. Warten Sie, bis der Bericht generiert wird, und klicken Sie dann auf den Link **hier**, um den Bericht herunterzuladen.
    
    ![PDF-Bericht für maskierte Daten generieren](images/generate-pdf-masked-data.png "PDF-Bericht für maskierte Daten generieren")
    
3.  Öffnen Sie den PDF-Bericht, prüfen Sie ihn, und schließen Sie ihn.
    
    ![Datenmaskierung - PDF-Bericht](images/data-masking-pdf-report.png "Datenmaskierung - PDF-Bericht")
    

## Aufgabe 7: Maskierte Daten in der Zieldatenbank validieren

1.  Kehren Sie zum SQL-Arbeitsblatt in Database Actions zurück. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an. Die `SELECT`\-Anweisung für die Tabelle `EMPLOYEES` sollte im Arbeitsblatt angezeigt werden. Die Registerkarte **Skriptausgabe** sollte weiterhin die ursprünglichen Daten enthalten. Nehmen Sie sich einen Moment Zeit, um es zu untersuchen.
    
2.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (nicht auf die Schaltfläche **Skript ausführen**), um die Abfrage auszuführen.
    
3.  Prüfen Sie die maskierten Daten auf der Registerkarte **Abfrageergebnis** unten auf der Seite. Sie können die Größe des Bereichs ändern, um weitere Daten anzuzeigen, und Sie können nach unten und nach rechts scrollen.
    
    ![Maskierte EMPLOYEE-Daten](images/masked-query-results.png "Maskierte EMPLOYEE-Daten")
    
4.  Um die maskierten Daten mit den ursprünglichen Daten zu vergleichen, klicken Sie auf die Registerkarte **Skriptausgabe**, die noch die ursprünglichen Daten enthält.
    

## Weitere Informationen

*   [Data Masking](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-masking.html)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023