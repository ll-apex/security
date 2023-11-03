# Sensible Daten maskieren

## Einführung

Mit der Datenmaskierung können Sie sensible Daten maskieren, damit die Daten für Nicht-Produktionszwecke sicher sind. Beispielsweise müssen Unternehmen häufig Kopien ihrer Produktionsdaten erstellen, um Entwicklungs- und Testaktivitäten zu unterstützen. Durch einfaches Kopieren der Produktionsdaten werden sensible Daten für neue Benutzer freigegeben. Um ein Sicherheitsrisiko zu vermeiden, können Sie mit der Datenmaskierung sensible Daten durch realistische, aber fiktive Daten ersetzen.

Die Rollen, die dem Oracle Data Safe-Serviceaccount für Ihre Zieldatenbank erteilt wurden, steuern, welche Oracle Data Safe-Features Sie mit der Datenbank verwenden können. Standardmäßig verfügt Autonomous Database Serverless über alle Oracle Data Safe-Rollen, die während der Zieldatenbankregistrierung erteilt werden, mit Ausnahme der Datenmaskierungsrolle (`DS$DATA_MASKING_ROLE`).

Erteilen Sie zunächst die Datenmaskierungsrolle für die Zieldatenbank. Maskieren Sie dann die sensiblen Daten, die Sie in der Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data-ocw) ermittelt haben, indem Sie die vom Datenmaskierungsfeature generierte Standardmaskierungs-Policy verwenden. Zeigen Sie mit Oracle Database Actions die Vorher-Nachher-Auswirkung auf die maskierten Daten an.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erteilen Sie die Rolle "Datenmaskierung" in der Zieldatenbank
*   Vertrauliche Daten in der Zieldatenbank anzeigen
*   Maskierungs-Policy für die Zieldatenbank erstellen
*   Maskierungs-Policy ändern
*   Vertrauliche Daten in der Zieldatenbank mit Datenmaskierung maskieren
*   Data Masking-Bericht anzeigen
*   Maskierte Daten in der Zieldatenbank validieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet
*   Die Zieldatenbank wurde bei Oracle Data Safe registriert. Stellen Sie sicher, dass das Kennwort `ADMIN` für die Datenbank zur Hand ist.
*   Modell für sensible Daten erstellt

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Sensible Daten maskieren](videohub:1_czbwzq21)

## Aufgabe 1: Rolle "Datenmaskierung" in der Zieldatenbank erteilen

1.  Kehren Sie zum SQL-Arbeitsblatt in Database Actions zurück.
    
2.  Wenn Sie zur Anmeldung bei der Zieldatenbank aufgefordert werden, melden Sie sich als Benutzer `ADMIN` an.
    
3.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Löschen** (Papierkorbsymbol), um das Arbeitsblatt zu löschen. Klicken Sie auf der Registerkarte **Skriptausgabe** auf die Schaltfläche **Ausgabe löschen**, um die Ausgabe zu löschen.
    
4.  Geben Sie im SQL-Arbeitsblatt den folgenden Befehl ein, um dem Oracle Data Safe-Serviceaccount in der Zieldatenbank die Datenmaskierungsrolle zu erteilen.
    
        <copy>EXECUTE DS_TARGET_UTIL.GRANT_ROLE('DS$DATA_MASKING_ROLE');</copy>
        
5.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil), um die Abfrage auszuführen.
    
    ![Schaltfläche "Anweisung ausführen" in Symbolleiste](images/run-statement-button.png "Schaltfläche "Anweisung ausführen" in Symbolleiste")
    
6.  Prüfen Sie, ob die Skriptausgabe wie folgt lautet:
    
    `PL/SQL procedure successfully completed`
    
    Sie können jetzt sensible Daten in der Zieldatenbank maskieren.
    
7.  Löschen Sie die Arbeitsblatt- und Skriptausgabe.
    

## Aufgabe 2: Sensible Daten in der Zieldatenbank anzeigen

In der Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data-ocw) haben Sie erfahren, dass die Tabelle `HCM1.EMPLOYEES` sensible Daten enthält. In dieser Aufgabe zeigen Sie die tatsächlichen sensiblen Daten in dieser Tabelle mit Database Actions an.

1.  Wählen Sie in Database Actions auf der Registerkarte **Navigator** in der ersten Dropdown-Liste das Schema **HCM1** aus.
    
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

Die Datenmaskierung kann eine Maskierungs-Policy für die Zieldatenbank basierend auf dem Modell für sensible Daten generieren. Es wird automatisch versucht, ein Standardmaskierungsformat für jede sensible Spalte auszuwählen. Sie können diese Standardauswahl bearbeiten und bei Bedarf andere auswählen. Gelegentlich werden Sie möglicherweise aufgefordert, Probleme (sofern vorhanden) in Ihren Maskierungsformaten zu beheben.

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
    *   Modell für sensible Daten: Wählen Sie **SDM1\[Ihr Ziel-Datenbankname\]** aus. Wenn dieses Modell für sensible Daten nicht vorhanden ist, wird auf die Übung [Sensible Daten ermitteln](?lab=discover-sensitive-data-ocw) verwiesen.
    
    ![Bereich "Maskierungs-Policy erstellen" mit SDM1](images/ocw/create-masking-policy-sdm1.png "Bereich "Maskierungs-Policy erstellen" mit SDM1")
    
7.  Klicken Sie auf **Maskierungs-Policy erstellen**.
    
    _Wichtig: Schließen Sie den Bereich nicht. Sie wird nach Abschluss aller Vorgänge automatisch geschlossen. Wenn Sie den Bereich schließen, bevor die Vorgänge abgeschlossen sind, wird der Vorgang zum Hinzufügen von Spalten zur Maskierungs-Policy nicht initiiert._
    
    Die Seite **Maskierungs-Policy-Details** wird angezeigt.
    
8.  Prüfen Sie die Maskierungs-Policy.
    
    *   Auf der Registerkarte **Informationen zur Maskierungs-Policy** können Sie den Namen der Maskierungs-Policy anzeigen (und bearbeiten), die Oracle Cloud-ID (OCID) für die Maskierungs-Policy, das Compartment, in dem die Maskierungs-Policy gespeichert wird, einen Link zu die Arbeitsanforderung für die Maskierungs-Policy, einen Link zu Maskierungsoptionen, die Zieldatenbank und das Modell für sensible Daten, mit dem die Maskierungs-Policy verknüpft ist, und das Datum/die Uhrzeit, zu dem/der die Maskierungs-Policy erstellt und zuletzt aktualisiert wurde.
    *   In der Tabelle **Maskierungsspalten** werden alle Maskierungsspalten und ihre Maskierungsformate aufgeführt. Bei Bedarf können Sie ein anderes Maskierungsformat für jede Maskierungsspalte auswählen. Sie können auf das Stiftsymbol neben einem Maskierungsformat klicken, um es zu bearbeiten.
    
    ![Seite "Maskierungs-Policy-Details" für Maske SDM1 oben](images/ocw/masking-policy-details-top.png "Seite "Maskierungs-Policy-Details" für Maske SDM1 oben")
    
    ![Seite "Maskierungs-Policy-Details" für Maske SDM1 unten](images/ocw/masking-policy-details-bottom.png "Seite "Maskierungs-Policy-Details" für Maske SDM1 unten")
    

## Aufgabe 4: Maskierungs-Policy ändern

Setzen Sie `SALARY` auf eine feste Zahl, z.B. 50000.

1.  Suchen Sie die Zeile für die Spalte `SALARY` in der Tabelle `EMPLOYEES`.
    
2.  Klicken Sie auf das Stiftsymbol neben dem Maskierungsformat.
    
    Die Seite **Maskierungsformat bearbeiten** wird angezeigt.
    
3.  Wählen Sie aus der Dropdown-Liste **Maskierungsformateintrag** die Option **Feste Zahl**.
    
4.  Geben Sie im Feld **Feste Zahl** **50000** ein.
    
    ![Maskierungsformat bearbeiten](images/ocw/edit-masking-format-page.png "Maskierungsformat bearbeiten")
    
5.  Klicken Sie auf **"Weiter"**.
    
    Beachten Sie, dass die aktualisierte Zeile hervorgehoben ist.
    
    ![Aktualisierte Zeile ist hervorgehoben](images/ocw/updated-row-is-highlighted.png "Aktualisierte Zeile ist hervorgehoben")
    
6.  Um das Update zu speichern, klicken Sie auf **Maskierungsformate speichern**, und warten Sie, bis der Aktualisierungsvorgang abgeschlossen ist.
    

## Aufgabe 5: Sensible Daten in der Zieldatenbank mit Datenmaskierung maskieren

Sie können einen Datenmaskierungsjob für die Zieldatenbank über die Seite **Maskierungs-Policy-Details** oder **Datenmaskierung** ausführen.

1.  Klicken Sie auf der Seite **Maskierungs-Policy-Details** auf **Maskierungsziel**.
    
    Der Bereich **Vertrauliche Daten maskieren** wird angezeigt.
    
2.  Wählen Sie in der Dropdown-Liste **Zieldatenbank** die Zieldatenbank aus, und klicken Sie auf **Daten maskieren**.
    
    ![Bereich "Sensible Daten maskieren"](images/ocw/mask-sensitive-data-panel.png "Bereich "Sensible Daten maskieren"")
    
    Die Seite **Arbeitsanforderung** wird angezeigt.
    
3.  Überwachen Sie den Fortschritt der Arbeitsanforderung, indem Sie die Logmeldungen in der Tabelle **Logmeldungen** anzeigen.
    
    ![Logmeldungen für Datenmaskierungsarbeitsanforderung](images/ocw/masking-log-messages.png "Logmeldungen für Datenmaskierungsarbeitsanforderung")
    
4.  Warten Sie, bis der Status **Erfolgreich** lautet.
    

## Aufgabe 6: Datenmaskierungsbericht anzeigen

1.  Klicken Sie auf der Seite **Arbeitsanforderung** auf der Registerkarte **Arbeitsanforderungsinformationen** neben **Maskierungsbericht** auf **Details anzeigen**.
    
    Die Seite **Maskierungsberichtsdetails** wird angezeigt.
    
2.  Prüfen Sie den Maskierungsbericht.
    
    *   Auf der Registerkarte **Maskierungsberichtsinformationen** werden der Name der Zieldatenbank, der Name der Maskierungs-Policy (Sie können auf einen Link klicken, um ihn anzuzeigen), die Oracle Cloud-ID (OCID) für den Maskierungsbericht, das Datum und die Uhrzeit des Starts und Endes des Datenmaskierungsjobs sowie die Anzahl der maskierten sensiblen Typen, Schemas, Tabellen, Spalten und Werte angezeigt. Sie können auf einen Link klicken, um Maskierungsoptionen anzuzeigen. Außerdem gibt es ein Tortendiagramm, in dem die Prozentwerte für maskierte Werte für jeden sensiblen Typ angezeigt werden. Sie können auf ein Kreissegment klicken, um einen Drilldown in das Diagramm durchzuführen.
    *   In der Tabelle **Maskierte Spalten** werden alle maskierten sensiblen Spalten sowie das jeweilige Schema, die Tabelle, das Maskierungsformat, der sensible Typ, die übergeordnete Spalte und die Gesamtanzahl der maskierten Werte aufgeführt.
    
    ![Maskierungsbericht oben](images/ocw/masking-report-top2.png "Maskierungsbericht oben") ![Maskierungsbericht unten](images/ocw/masking-report-bottom.png "Maskierungsbericht unten")
    

## Aufgabe 7: Maskierte Daten in der Zieldatenbank validieren

1.  Kehren Sie zum SQL-Arbeitsblatt in Database Actions zurück. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an. Die `SELECT`\-Anweisung für die Tabelle `EMPLOYEES` sollte im Arbeitsblatt angezeigt werden.
    
2.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil), um die Abfrage auszuführen.
    
    Wenn Sie auf die Schaltfläche **Anweisung ausführen** (anstelle der Schaltfläche **Skript ausführen**) klicken, werden die Ergebnisse auf der Registerkarte **Abfrageergebnisse** anstelle der Registerkarte **Skriptausgabe** angezeigt. Auf diese Weise können Sie einen Vor- und Nachvergleich der maskierten Daten durchführen.
    
3.  Prüfen Sie die maskierten Daten auf der Registerkarte **Abfrageergebnis** unten auf der Seite.
    
    *   Sie können die Größe des Bereichs ändern, um weitere Daten anzuzeigen, und Sie können nach unten und nach rechts scrollen.
    *   Suchen Sie die Spalte `SALARY`, und prüfen Sie, ob alle Werte 50000 sind.
    
    ![Maskierte EMPLOYEE-Daten](images/ocw/masked-query-results.png "Maskierte EMPLOYEE-Daten")
    
4.  (Optional) Klicken Sie auf die Registerkarte **Skriptausgabe**, um die ursprünglichen unmaskierten Daten anzuzeigen.
    

## Weitere Informationen

*   [Data Masking](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-masking.html)
*   [Zieldatenbankregistrierung](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 17. August 2023