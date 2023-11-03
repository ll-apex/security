# Neueste Sicherheitsbewertung mit der Oracle Data Safe-CLI herunterladen

## Einführung

Mit der Befehlszeilenschnittstelle (CLI) in Cloud Shell können Sie Aufgaben in Oracle Data Safe ausführen. Cloud Shell ist eine kleine virtuelle Maschine, auf der eine Linux-Shell ausgeführt wird. Sie kann in Ihrem Mandanten über die Oracle Cloud Infrastructure-Konsole aufgerufen werden. Es kann in Ihrem Mandanten verwendet werden (innerhalb der monatlichen Mandantenlimits). Wenn Sie komplexe Aufgaben mit der CLI ausführen möchten, ist es hilfreich, ein SH-Skript zu erstellen, das alle CLI-Befehle enthält.

In dieser Übung aktualisieren Sie mit der CLI den neuesten Sicherheitsbewertungsbericht für die Zieldatenbank und laden ihn in ein Verzeichnis auf dem Cloud Shell-Rechner herunter. Machen Sie sich zunächst mit der CLI-Dokumentation (Command Line Interface) für Oracle Data Safe vertraut.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Lesen Sie die Dokumentation für die Oracle Data Safe-CLI
*   Auf Cloud Shell zugreifen
*   CLI-Befehle nacheinander erstellen und testen
*   Erstellen Sie eine SH-Datei mit allen CLI-Befehlen.
*   SH-Datei ausführen und Sicherheitsbewertungsbericht anzeigen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

## Aufgabe 1: Dokumentation für die Oracle Data Safe-CLI prüfen

1.  Öffnen Sie in einem Browser die [Oracle Data Safe-Befehlszeilenreferenz](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html).
    
2.  Scrollen Sie auf der Seite nach unten, und prüfen Sie die verfügbaren Befehle. Die Liste ist nach Ressourcen sortiert:
    
    *   Warnung
    *   Alert-Policy
    *   Alertübersicht
    *   Audit-Archiv-Abruf
    *   audit-event-summary
    *   Audit-Policy
    *   ...
3.  Um die Beschreibung und die verfügbaren Befehle für eine Ressource anzuzeigen, klicken Sie auf den Namen der Ressource.
    
4.  Um Details für einen Befehl anzuzeigen, klicken Sie auf den Namen des Befehls. Überprüfen Sie die Beschreibung. Im Abschnitt **Verwendung** wird Beispielcode bereitgestellt, den Sie als Ausgangspunkt verwenden können. Beachten Sie die Struktur: Sie beginnt mit `oci data-safe`, dann mit dem Ressourcennamen, dem Befehl für die Ressource und gegebenenfalls mit `[OPTIONS]`. Optionen können erforderlich oder optional sein. Im Abschnitt **Erforderliche Parameter** werden alle Parameter aufgeführt, die Sie im Befehl angeben müssen. Im Abschnitt **Optionale Parameter** werden die Parameter beschrieben, die Sie dem Befehl bei Bedarf hinzufügen können.
    
    Beispiel: Der Befehl `alerts-update` ist wie folgt strukturiert:
    
        <copy>oci data-safe alert alerts-update [OPTIONS]</copy>
        
5.  Beachten Sie bei der Angabe erforderlicher oder optionaler Parameter, dass Sie den Namen des Parameters angeben und dann dem Wert folgen müssen. Beispiel: Um alle Alerts für eine Zieldatenbank zu schließen, können Sie den folgenden Befehl ausgeben (ersetzen Sie `your-compartment-ocid` und `your-target-database-OCID` durch Ihre eigenen Werte):
    
        <copy>oci data-safe alert alerts-update --compartment-id your-compartment-ocid --status CLOSED --target-id your-target-database-OCID</copy>
        
6.  Scrollen Sie zum Ende der Seite, um Beispiele anzuzeigen.
    

## Aufgabe 2: Auf Cloud Shell zugreifen

1.  Um Cloud Shell zu öffnen, klicken Sie in der oberen rechten Ecke der Oracle Cloud Infrastructure-Konsole auf das Symbol **Entwicklertools**, und wählen Sie **Cloud Shell** aus.
    
    Wenn Sie Cloud Shell zum ersten Mal öffnen, ist Ihr aktuelles Verzeichnis Ihr Home-Verzeichnis. Beispiel: `/home/jody_glove`. In dieser Übung können Sie im Home-Verzeichnis (`~/`) arbeiten.
    
2.  (Optional) Wenn Sie Cloud Shell häufig verwenden und neu starten möchten, können Sie es zurücksetzen. Mit dem folgenden Befehl werden alle Daten in Ihrem `$HOME`\-Verzeichnis auf dem Cloud Shell-Rechner gelöscht, und die Dateien `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` und `$HOME/.emacs` werden wieder auf ihre Standardwerte zurückgesetzt. Geben Sie zur Bestätigung in der Eingabeaufforderung **y** ein.
    
        $ <copy>csreset --all</copy>
        

## Aufgabe 3: CLI-Befehle nacheinander erstellen und testen

Identifizieren Sie die Befehle und Werte, die für das SH-Skript erforderlich sind, und testen Sie jeden Befehl in Cloud Shell.

1.  Erstellen Sie eine Variable, die Ihre Compartment-OCID definiert.
    
    Suchen Sie dazu zuerst die Compartment-OCID: Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Identität und Sicherheit** aus, und klicken Sie rechts unter **Identität** auf **Compartments**. Klicken Sie auf den Namen Ihres Compartments. Klicken Sie auf der Registerkarte **Compartment-Informationen** neben **OCID** auf **Kopieren**. Geben Sie in Cloud Shell den folgenden Befehl ein, und ersetzen Sie `your-compartment-ocid` durch Ihre eigene OCID.
    
        $ <copy>export compartment_id=your-compartment-ocid</copy>
        
2.  Erstellen Sie eine Variable, die die OCID der Oracle Data Safe-Zieldatenbank definiert (nicht die Autonomous Database-OCID!).
    
    Suchen Sie dazu zuerst die Zieldatenbank-OCID: Wählen Sie im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus. Klicken Sie links unter **Data Safe** auf **Zieldatenbanken**. Wählen Sie links unter **Listengeltungsbereich** Ihr Compartment aus. Klicken Sie rechts auf den Namen der Zieldatenbank. Klicken Sie auf der Registerkarte **Zieldatenbankdetails** neben **OCID** auf **Kopieren**. Geben Sie in Cloud Shell den folgenden Befehl ein, und ersetzen Sie `your-target-database-ocid` durch Ihre eigene OCID.
    
        $ <copy>export target_id=your-target-database-ocid</copy>
        
3.  Erstellen Sie eine Variable, die den Dateinamen für den heruntergeladenen Sicherheitsbewertungsbericht definiert.
    
    Geben Sie dazu in Cloud Shell den folgenden Befehl ein, und ersetzen Sie `your-download-file-name` durch einen Namen Ihrer Wahl. Fügen Sie .pdf oder .xls ein, je nach gewünschtem Dateiformat. Hinweis: Sie können einen Pfad angeben. Beispiel: `~/examples/myreport.pdf`. Lassen Sie uns es jedoch einfach halten und einfach das Home-Verzeichnis verwenden (so ist kein Pfad erforderlich).
    
        $ <copy>export file_name=your-download-file-name</copy>
        
4.  Erstellen Sie eine Variable, die das Berichtsformat (PDF oder XLS) definiert, das Sie für die heruntergeladene Sicherheitsbewertung verwenden möchten.
    
    Geben Sie dazu in Cloud Shell den folgenden Befehl ein, und ersetzen Sie `pdf-or-xls` durch **PDF** oder **XLS**.
    
        $ <copy>export format=pdf-or-xls</copy>
        
5.  Erstellen Sie eine Sicherheitsbewertung, und rufen Sie ihre OCID ab.
    
    Geben Sie dazu in Cloud Shell den folgenden Befehl ein. Warten Sie, bis die Sicherheitsbewertung erstellt wurde (ca. 1 Minute).
    
        $ <copy>security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)</copy>
        
    
    Beachten Sie, wie Sie den `security-assessment create`\-CLI-Befehl mit den Parametern `--query data.id` und `--raw-output` verwenden. Wenn Sie Metadaten zu einer Ressource abrufen müssen, können Sie lernen, welche Metadatenwerte verfügbar sind, indem Sie die Parameter `--query data` und `--raw-output` aufnehmen. Beispiel: Wenn die obige Anweisung `--query data` anstelle von `--query data.id` verwendet, enthält der Ausgabewert alle möglichen Schlüssel/Wert-Paare.
    
        <copy>{"compartment-id": "ocid1.compartment.oc1...", "defined-tags": { "Oracle-Tags": { "CreatedBy": "jody.glove..", "CreatedOn": "2023-01-30T20:40:53.671Z" } }, "description": null, "display-name": "SA_1675111253797", "freeform-tags": {}, "id": "ocid1.datasafesecurityassessment.oc1...", "ignored-assessment-ids": null, "ignored-targets": null, "is-baseline": false, "is-deviated-from-baseline": null, "last-compared-baseline-id": null, "lifecycle-details": null, "lifecycle-state": "CREATING", "link": null, "schedule": null, "schedule-security-assessment-id": null, "statistics": null, "system-tags": {}, "target-ids": [ "ocid1.datasafetargetdatabase.oc1..." ], "target-version": null, "time-created": "2023-01-30T20:40:53.797000+00:00", "time-updated": "2023-01-30T20:40:53.797000+00:00", "triggered-by": "USER", "type": "SAVED" }</copy>
        
6.  Prüfen Sie, ob die Sicherheitsbewertung in Oracle Data Safe erstellt wurde.
    
    Wählen Sie dazu im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus. Klicken Sie links unter **Data Safe** auf **Sicherheitsbewertung**. Klicken Sie auf die Registerkarte **Zielübersicht**, suchen Sie die Zeile mit der Zieldatenbank, und klicken Sie auf **Bericht anzeigen**. Klicken Sie oben auf der neuesten Seite der Sicherheitsbewertung auf **Historie anzeigen**. Stellen Sie sicher, dass das Compartment ausgewählt ist. Suchen Sie die neue Sicherheitsbewertung für die Zieldatenbank, die im vorherigen Schritt vom CLI-Befehl generiert wurde.
    
    Sie müssen mindestens zwei Sicherheitsbewertungen haben. Die erste wurde automatisch von Oracle Data Safe erstellt, als Sie die Zieldatenbank registriert haben. Die zweite ist diejenige, die Sie gerade über die Befehlszeile erstellt haben. Wenn die zweite nicht aufgeführt ist, müssen Sie möglicherweise etwas länger warten.
    
7.  Generieren Sie die PDF-Sicherheitsbewertung.
    
    Bevor Sie eine Sicherheitsbewertung herunterladen können, müssen Sie sie zuerst generieren. Kehren Sie dazu zu Cloud Shell zurück, und geben Sie den folgenden Befehl unverändert ein. Beachten Sie, wie wir die Variablen verwenden, die wir in den vorherigen Schritten definiert haben (`$format` und `$security_assessment_id`). Warten Sie, bis der Bericht generiert wird (ca. 1 Minute).
    
        $ <copy>oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id</copy>
        
8.  Laden Sie die PDF-Datei zur Sicherheitsbewertung in Ihr Home-Verzeichnis in Cloud Shell herunter.
    
    Geben Sie dazu in Cloud Shell den folgenden Befehl ein. Beachten Sie, wie die in den vorherigen Schritten definierten Variablen (`$file_name`, `$format` und `$security_assessment_id`) verwendet werden. Warten Sie, bis der Bericht heruntergeladen ist (ca. 1 Minute).
    
        $ <copy>oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
9.  Stellen Sie sicher, dass die PDF-Datei auf den Cloud Shell-Rechner heruntergeladen wurde.
    
        $ <copy>ls</copy>
        
        your-download-file-name
        
10.  Entfernen Sie die PDF-Datei, indem Sie den folgenden Befehl eingeben. Ersetzen Sie `your-download-file-name` durch Ihren eigenen PDF-Dateinamen.
    
        $ <copy>rm your-download-file-name</copy>
        

## Aufgabe 4: SH-Datei mit allen CLI-Befehlen erstellen

Erstellen Sie eine SH-Datei, und fügen Sie alle Befehle hinzu, die Sie in der vorherigen Aufgabe getestet haben. Verwenden Sie unbedingt Ihre eigenen Werte für die Variablen.

1.  Öffnen Sie in Cloud Shell den vi-Editor, und erstellen Sie eine Datei mit dem Namen `example.sh`.
    
        $ <copy>vi example.sh</copy>
        
2.  Fügen Sie alle Befehle in die Datei ein. Fügen Sie zwei `sleep`\-Befehle ein - einen, nachdem Sie den PDF-Bericht erstellt haben, und einen, nachdem Sie ihn generiert haben, damit das Programm Zeit für den Abschluss der Vorgänge lässt. Andernfalls erhalten Sie Fehler. Sie können auch vor den `sleep`\-Befehlen in die Konsole schreiben (mit `echo`), um den Benutzer über die Wartezeit zu informieren.
    
        <copy>export compartment_id=ocid1.compartment.oc1...
        export target_id=ocid1.datasafetargetdatabase.oc1...
        export file_name=MyLatestSecurityAssessment.pdf
        export format=PDF
        security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)
        echo "Waiting for 2 minutes to allow time for the latest security assessment to be refreshed."
        sleep 120
        oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id
        echo "Waiting for 1 minute to allow time for a PDF report to be generated."
        sleep 60
        oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
3.  Speichern und beenden Sie die Datei (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie die **Eingabetaste**).
    
4.  Erteilen Sie Berechtigungen für die Datei.
    
        $ <copy>chmod 777 example.sh</copy>
        

## Aufgabe 5: SH-Datei ausführen und Sicherheitsbewertungsbericht anzeigen

Wenn Sie die SH-Datei ausführen, wird die neueste Sicherheitsbewertung auf den Cloud Shell-Rechner heruntergeladen. Von dort aus können Sie es zur Anzeige auf Ihren lokalen Computer herunterladen.

1.  Geben Sie den folgenden Befehl ein, um die SH-Datei auszuführen. Warten Sie drei Minuten, bis das Skript ausgeführt wird.
    
        $ <copy>./example.sh</copy>
        
2.  Wenn Sie eine Servicefehlermeldung erhalten, die impliziert, dass ein Schritt mehr Zeit benötigt, erhöhen Sie den entsprechenden `sleep`\-Wert im SH-Skript.
    
3.  Listen Sie die Dateien im Home-Verzeichnis auf.
    
        $ <copy>ls</copy>
        
        example.sh  your-download-file-name
        
4.  Klicken Sie in der oberen rechten Ecke von Cloud Shell auf das Symbol **Cloud Shell-Menü** (Zahnrad), und wählen Sie **Herunterladen** aus.
    
    Das Dialogfeld **Datei herunterladen** wird angezeigt.
    
5.  Geben Sie den Namen der PDF-Datei ein, und klicken Sie auf **Herunterladen**.
    
    Die Datei wird in den Browser heruntergeladen.
    
6.  Öffnen Sie die PDF-Datei, und prüfen Sie den Bewertungsbericht. Schließen Sie die Browserregisterkarte, wenn Sie fertig sind.
    
7.  Schließen Sie Cloud Shell, und klicken Sie zur Bestätigung auf **Beenden**.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Zugehörige Ressourcen

*   [Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)
*   [Oracle Data Safe-CLI](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html)
*   [Mit der CLI](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliusing.htm#Managing_CLI_Input_and_Output)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbank
*   **Berater** - Bettina Schaeumer
*   **Zuletzt aktualisiert am/um** - Jody Glover, 11. April 2023