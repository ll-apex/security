# Umgebung vorbereiten

## Einführung

In dieser Übung bereiten Sie Ihre Umgebung in Oracle Cloud Infrastructure für den Workshop vor.

Geschätzte Laborzeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Compartment erstellen
*   Benutzergruppe erstellen und Oracle Cloud-Account zur Gruppe hinzufügen
*   IAM-Policy für die Benutzergruppe erstellen
*   Autonomous Transaction Processing-Datenbank bereitstellen
*   Oracle Database Actions aufrufen
*   Beispieldaten in die Datenbank laden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account abgerufen und sich bei der Oracle Cloud Infrastructure-Konsole unter `https://cloud.oracle.com` angemeldet.

## Aufgabe 1: Compartment erstellen

Erstellen Sie ein Compartment für sich selbst in Oracle Cloud Infrastructure Identity and Access Management (IAM). Von hier an wird dieses Compartment als "Ihr Compartment" bezeichnet. Wenn in Ihrem Mandanten ein Compartment vorhanden ist, das Sie verwenden können, können Sie diese Aufgabe überspringen.

1.  Wählen Sie im Navigationsmenü **Identität und Sicherheit**, **Compartments** aus.
    
    Die Seite **Compartments** in IAM wird angezeigt.
    
2.  Klicken Sie auf **Compartment erstellen**.
    
    Das Dialogfeld **Compartment erstellen** wird angezeigt.
    
3.  Geben Sie einen Namen für das Compartment ein.
    
4.  Geben Sie eine Beschreibung für das Compartment ein.
    
5.  Wählen Sie ein übergeordnetes Compartment aus.
    
6.  Klicken Sie auf **Compartment erstellen**.
    

## Aufgabe 2: Benutzergruppe erstellen und der Gruppe einen Oracle Cloud-Account hinzufügen

Erstellen Sie eine Benutzergruppe, und fügen Sie der Gruppe Ihren Oracle Cloud-Account hinzu. Wenn Sie ein Mandantenadministrator sind, können Sie diese Aufgabe überspringen. Andernfalls können Sie die Hilfe einer Person in Anspruch nehmen, um diese Aufgabe abzuschließen.

1.  Wählen Sie im Navigationsmenü **Identität und Sicherheit**, **Gruppen** aus.
    
    Die Seite **Gruppen** in IAM wird angezeigt.
    
2.  Klicken Sie auf **Gruppe erstellen**.
    
    Das Dialogfeld **Gruppe erstellen** wird angezeigt.
    
3.  Geben Sie einen Namen für die Gruppe ein. Beispiel: `dsg01` (kurz für Data Safe-Gruppe 1).
    
4.  Geben Sie eine Beschreibung für die Gruppe ein. Beispiel: **Benutzergruppe für datensicheren Benutzer 1**. Eine Beschreibung ist erforderlich.
    
5.  (Optional) Klicken Sie auf **Erweiterte Optionen anzeigen**, und erstellen Sie ein Tag.
    
6.  Klicken Sie auf **Erstellen**.
    
    Die Seite **Gruppe - Details** wird angezeigt.
    
7.  Klicken Sie unter **Gruppenmitglieder** auf **Benutzer zu Gruppe hinzufügen**.
    
    Das Dialogfeld **Benutzer zu Gruppe hinzufügen** wird angezeigt.
    
8.  Wählen Sie in der Dropdown-Liste den Benutzer für diesen Workshop aus, und klicken Sie auf **Hinzufügen**.
    
    Der Benutzer wird als Gruppenmitglied aufgeführt.
    

## Aufgabe 3: IAM-Policy für die Benutzergruppe erstellen

Erstellen Sie eine IAM-Policy, die Ihnen die erforderlichen Berechtigungen für den Workshop erteilt. Wenn Sie ein Mandantenadministrator sind, können Sie diese Aufgabe überspringen. Andernfalls können Sie die Hilfe einer Person in Anspruch nehmen, um diese Aufgabe abzuschließen.

1.  Wählen Sie im Navigationsmenü **Identität und Sicherheit**, **Policys** aus.
    
    Die Seite **Policys** in IAM wird angezeigt.
    
2.  Lassen Sie links unter **COMPARTMENT** das Compartment **root** ausgewählt.
    
3.  Klicken Sie auf **Policy erstellen**.
    
    Die Seite **Policy erstellen** wird angezeigt.
    
4.  Geben Sie einen Namen für die Policy ein. Es ist hilfreich, die Policy nach einem Gruppennamen zu benennen, z.B. `dsg01` .
    
5.  Geben Sie eine Beschreibung für die Policy ein. Beispiel: **Policy für Data Safe-Gruppe 1**.
    
6.  Lassen Sie in der Dropdown-Liste **COMPARTMENT** das **root**\-Compartment ausgewählt.
    
7.  Verschieben Sie im Abschnitt **Policy Builder** den Regler **Manuellen Editor anzeigen** nach rechts, um das Policy-Feld anzuzeigen.
    
8.  Geben Sie im Feld "Policy" die folgenden Policy-Anweisungen ein. Ersetzen Sie `{group name}` und `{compartment name}` durch die entsprechenden Werte.
    
        <copy>
        Allow group {group name} to manage data-safe-family in compartment {compartment name}
        Allow group {group name} to manage autonomous-database in compartment {compartment name}
        </copy>
        
9.  Klicken Sie auf **Erstellen**.
    

## Aufgabe 4: Autonomous Transaction Processing-Datenbank bereitstellen

Erstellen Sie eine Autonomous Transaction Processing-(ATP-)Datenbank in Ihrem Compartment. Bevor Sie fortfahren, stellen Sie sicher, dass in Ihrem Mandanten genügend Quota vorhanden ist, um eine (immer kostenlose) Autonomous Database zu erstellen.

> **Hinweis**: Wenn Sie eine vorhandene Verfügbarkeitsdatenbank in Ihrem Mandanten verwenden möchten, können Sie diese Aufgabe überspringen.

1.  Wählen Sie im Navigationsmenü **Oracle Database**, **Autonomous Transaction Processing** aus.
    
2.  Stellen Sie im Abschnitt **Filter** auf der linken Seite sicher, dass der Workload-Typ **Transaktionsverarbeitung** oder **Alle** lautet, damit Sie die Datenbankliste nach dem Erstellen der Datenbank anzeigen können.
    
3.  Wählen Sie in der Dropdownliste **Compartment** Ihr Compartment aus.
    
4.  Klicken Sie auf **Autonomous Database erstellen**.
    
5.  Geben Sie auf der Seite **Autonomous Database erstellen** grundlegende Informationen für die Datenbank an:
    
    *   **Compartment** - Wählen Sie bei Bedarf ein anderes Compartment aus.
    *   **Anzeigename**: Geben Sie einen unvergesslichen Namen für die Datenbank zu Anzeigezwecken ein.
    *   **Datenbankname** - Geben Sie einen Datenbanknamen ein. Es ist wichtig, nur Buchstaben und Zahlen zu verwenden, beginnend mit einem Buchstaben. Die maximale Länge beträgt 14 Zeichen. Unterstriche werden nicht unterstützt.
    *   **Workload type** - Wählen Sie **Transaction Processing** aus.
    *   **Deployment-Typ**: Lassen Sie **Serverlos** ausgewählt.
    *   **Always Free**: Wählen Sie diese Option aus, indem Sie den Schieberegler nach rechts verschieben.
    *   **Datenbankversion** - Lassen Sie **21c** ausgewählt.
    *   **OCPU-Anzahl** - Sie erhalten **1** OCPU.
    *   **Speicher** - Sie erhalten 0.02TB des Speichers.
    *   **Kennwort** und **Kennwort bestätigen**: Geben Sie ein Kennwort für den Datenbankbenutzer `ADMIN` an, und notieren Sie es. Das Kennwort muss zwischen 12 und 30 Zeichen lang sein und mindestens einen Großbuchstaben, einen Kleinbuchstaben und ein numerisches Zeichen enthalten. Es darf keinen Benutzernamen oder doppelte Anführungszeichen (") enthalten.
    *   **Zugriffstyp** - Lassen Sie **Sicherer Zugriff von überall** ausgewählt.
    *   **Lizenztyp** - Lassen Sie **Lizenz eingeschlossen** aktiviert.
6.  Klicken Sie auf **Autonomous Database erstellen**.
    
    Die Seite mit den **Autonomous Database-Details** wird angezeigt.
    
7.  Warten Sie einige Minuten, bis die Datenbankinstanz bereitgestellt ist.
    
    **Verfügbar** wird unter dem großen Verfügbarkeitssymbol angezeigt.
    
    ![Autonomous Database-Details (Seite)](images/autonomous-database-details-page.png "Autonomous Database-Details (Seite)")
    

## Aufgabe 5: Oracle Database Actions aufrufen

Mit Database Actions können Sie SQL-Befehle in der Zieldatenbank ausführen. Die schrittweisen Anweisungen für den Zugriff auf Database Actions werden hier beschrieben. Nachfolgende Übungen sagen einfach, "auf das SQL-Arbeitsblatt in Database Actions zuzugreifen". Bei Bedarf können Sie auf diese Schritte zurückgreifen.

1.  Klicken Sie oben auf der Seite **Autonomous Database-Details** auf **Datenbankaktionen**.
    
    Die Seite **Anmelden** wird angezeigt.
    
2.  Melden Sie sich bei Bedarf als Benutzer `ADMIN` an.
    
    Eine Browserregisterkarte mit dem Namen **Oracle Database Actions** wird geöffnet. _Diese Registerkarte während des gesamten Workshops geöffnet lassen._ Wenn Ihre Session abläuft, können Sie sich immer wieder anmelden.
    
    *   Wenn Ihnen ein Mandantenadministrator eine Autonomous Database bereitgestellt hat, rufen Sie das Datenbankkennwort von dieser Person ab.
3.  Klicken Sie im Abschnitt **Entwicklung** auf **SQL**.
    
    Der Name der Browserregisterkarte wird in **SQL | Oracle Database Actions** geändert.
    
4.  Schließen Sie die Warnungs- und Hilfedialogfelder.
    
5.  Prüfen Sie die Schnittstelle. So verwenden Sie Database Actions während des Workshops:
    
    *   Wählen Sie im Bereich **Navigator** auf der linken Seite Tabellen aus dem Schema **HCM1** in der Zieldatenbank aus.
    *   Im **Arbeitsblatt** auf der rechten Seite führen Sie SQL-Befehle und -Skripte aus.
    *   In den Registerkarten **Abfrageergebnis** und **Skriptausgabe** unten auf der Seite prüfen Sie die Abfrageergebnisse und die Ausgabe, die aus ausgeführten Skripten generiert wurden.
    
    ![SQL-Arbeitsblatt in Oracle Database-Aktionen](images/database-actions.png "SQL-Arbeitsblatt in Oracle Database-Aktionen")
    

## Aufgabe 6: Beispieldaten in die Datenbank laden

Führen Sie als Benutzer `ADMIN` in der Datenbank das SQL-Skript `load-data-safe-sample-data_admin.SQL` aus, um Beispieldaten in die Datenbank zu laden. Dieses Skript erstellt mehrere Tabellen mit Beispieldaten, die Sie mit den Oracle Data Safe-Features üben können. Außerdem wird eine Datenbankaktivität für den Benutzer `ADMIN` generiert.

1.  Laden Sie das Skript [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) herunter, und öffnen Sie es in einem Texteditor wie NotePad.
    
2.  Kopieren Sie das gesamte Skript in die Zwischenablage, und fügen Sie es in Database Actions in das Arbeitsblatt ein. Die letzte Zeile des Skripts lautet wie folgt:
    
    `select null as "End of script" from dual;`
    
3.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen**, und warten Sie, bis die Ausführung des Skripts abgeschlossen ist. Machen Sie sich keine Sorgen, wenn auf der Registerkarte **Skriptausgabe** Fehlermeldungen angezeigt werden. Diese werden beim ersten Ausführen des Skripts erwartet.
    
    *   Die Ausführung des Skripts dauert einige Minuten.
    *   In der unteren linken Ecke kann das Zahnrad etwa eine Minute lang still bleiben, und dann dreht es sich, wenn das Skript verarbeitet wird. Die Skriptausgabe wird angezeigt, nachdem das Skript ausgeführt wurde.
    *   Das Skript endet mit der Meldung **END OF SCRIPT**.
    
    ![Schaltfläche "Skript ausführen"](images/run-script.png "Schaltfläche "Skript ausführen"")
    
4.  Um sicherzustellen, dass die Beispieldaten erfolgreich geladen werden, prüfen Sie am Ende der Skriptausgabe die Zeilenanzahl für jede Tabelle im Schema `HCM1`. Die Anzahl sollte wie folgt lauten:
    
    *   `COUNTRIES` - 25 Zeilen
    *   `DEPARTMENTS` - 27 Zeilen
    *   `EMPLOYEES` - 107 Zeilen
    *   `EMP_EXTENDED` - 107 Zeilen
    *   `JOBS` - 19 Zeilen
    *   `JOB_HISTORY` - 10 Zeilen
    *   `LOCATIONS` - 23 Zeilen
    *   `REGIONS` - 4 Zeilen
    *   `SUPPLEMENTAL_DATA` - 149 Zeilen
    
    Wenn sich die Ergebnisse von den oben angegebenen unterscheiden, führen Sie das Skript [load-data-safe-sample-data\_admin.sql](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-data-safe-sample-data_admin.sql) erneut aus.
    
5.  Aktualisieren Sie Database Actions, indem Sie die Seite _Browser_ aktualisieren. Wenn Sie dazu aufgefordert werden, klicken Sie auf **Seite verlassen**.
    
6.  Stellen Sie sicher, dass das Schema `HCM1` in der ersten Dropdown-Liste im Bereich **Navigator** aufgeführt ist.
    
7.  _Lassen Sie die Registerkarte **SQL | Oracle Database Actions** geöffnet, weil Sie während dieses Workshops erneut darauf zugreifen._ Wenn Ihre Session abläuft, können Sie sich immer wieder anmelden. Kehren Sie zur Registerkarte **Autonomous Database | Oracle Cloud Infrastructure** zurück.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Cloud Infrastructure-Dokumentation](https://docs.oracle.com/iaas/Content/home.htm)
*   [OCI Cloud Free Tier](https://www.oracle.com/cloud/free/)
*   [Autonomous Database bereitstellen](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-provision.html)
*   [Daten in eine Autonomous Database laden](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/load-data.html)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023