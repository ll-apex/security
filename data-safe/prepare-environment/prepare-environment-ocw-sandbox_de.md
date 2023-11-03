# Umgebung vorbereiten

## Einführung

In dieser Übung bereiten Sie Ihre Umgebung in Oracle Cloud Infrastructure vor. Die Sandbox LiveLabs bietet Ihnen einen Mandanten, ein Compartment, einen Oracle Cloud-Account im LiveLabs-Mandanten und eine vorab bereitgestellte Autonomous Database.

Geschätzte Laborzeit: 5 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Zeigen Sie Ihre LiveLabs-Reservierungsinformationen an, und melden Sie sich an
*   Oracle Database Actions aufrufen
*   Beispieldaten in die Datenbank laden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account abgerufen und sich bei der Oracle Cloud Infrastructure-Konsole unter `https://cloud.oracle.com` angemeldet.

## Aufgabe 1: LiveLabs Sandbox-Reservierungsinformationen anzeigen und anmelden

1.  Klicken Sie in der oberen linken Ecke der Seite mit den Übungsanweisungen (diese Seite) auf den Link **Anmeldeinformationen anzeigen**.
    
    Der Bereich **Reservierungsinformationen** wird angezeigt.
    
2.  Prüfen Sie die Informationen. In Oracle Cloud Infrastructure erhalten Sie Folgendes:
    
    *   Zugriff auf einen der LiveLab-Mandanten
    *   Ein Link, der Sie zur Anmeldeseite für Oracle Cloud Infrastructure weiterleitet (Schaltfläche **OCI starten**)
    *   Ein Benutzername und ein Kennwort für die Anmeldung beim Mandanten LiveLabs. Wenn Sie sich das erste Mal anmelden, werden Sie aufgefordert, Ihr Kennwort zu ändern.
    *   Ein eigenes Fach. Wir bezeichnen dieses Fach im gesamten Workshop als "Ihr Fach". Notieren Sie sich den Namen Ihres Compartments, da Sie ihn häufig im gesamten Workshop auswählen müssen.
    *   Eine Autonomous Database in Ihrem Compartment. Sie erhalten das Kennwort für den Account `ADMIN` in der Datenbank.
3.  Notieren Sie sich Ihren Benutzernamen, und klicken Sie auf die Schaltfläche **Kennwort kopieren** für Oracle Cloud Infrastructure.
    
4.  Klicken Sie im Bereich **Reservierungsinformationen** auf die Schaltfläche **OCI starten**.
    
    Eine neue Browserregisterkarte wird geöffnet, und die Anmeldeseite für den Mandanten LiveLabs wird angezeigt.
    
5.  Geben Sie den Benutzernamen ein, und fügen Sie das Kennwort in das Feld **Kennwort** ein. Klicken Sie dann auf **Anmelden**.
    
    Die Seite **Kennwort ändern** wird angezeigt.
    
6.  Fügen Sie im Feld **Aktuelles Kennwort** Ihr Kennwort ein. Geben Sie in den Feldern **Neues Kennwort** und **Neues Kennwort bestätigen** ein neues Kennwort ein. Beachten Sie die Kennwortanforderungen, die auf der Seite angegeben werden. Klicken Sie auf **Neues Kennwort speichern**.
    
    Sie sind jetzt bei Ihrer LiveLabs Sandbox in Oracle Cloud Infrastructure angemeldet.
    
7.  Rufen Sie die Zieldatenbank auf: Wählen Sie im Navigationsmenü (Hamburger-Menü in der oberen linken Ecke) die Optionen **Oracle Database**, **Autonomous Transaction Processing** aus. Wählen Sie unter **Listengeltungsbereich** das Compartment im Ordner **LiveLabs** aus. Klicken Sie in der Tabelle rechts auf den Namen der Zieldatenbank.
    

## Aufgabe 2: Oracle Database Actions aufrufen

Mit Database Actions können Sie SQL-Befehle in der Zieldatenbank ausführen. Die schrittweisen Anweisungen für den Zugriff auf Database Actions werden hier beschrieben. Nachfolgende Übungen sagen einfach, "auf das SQL-Arbeitsblatt in Database Actions zuzugreifen". Bei Bedarf können Sie auf diese Schritte zurückgreifen.

1.  Klicken Sie oben auf der Seite **Autonomous Database-Details** auf **Datenbankaktionen**.
    
    Die Seite **Anmelden** wird angezeigt.
    
2.  Melden Sie sich bei Bedarf als Benutzer `ADMIN` an. Verwenden Sie unbedingt das Datenbankkennwort, das Ihnen von LiveLabs bereitgestellt wird.
    
    Eine Browserregisterkarte mit dem Namen **Oracle Database Actions** wird geöffnet.
    
3.  Klicken Sie im Abschnitt **Entwicklung** auf **SQL**.
    
    Der Name der Browserregisterkarte wird in **SQL | Oracle Database Actions** geändert.
    
4.  Schließen Sie die Warnungs- und Hilfedialogfelder.
    
5.  Prüfen Sie die Schnittstelle. So verwenden Sie Database Actions während des Workshops:
    
    *   Wählen Sie im Bereich **Navigator** auf der linken Seite Tabellen aus dem Schema **HCM1** in der Zieldatenbank aus.
    *   Im **Arbeitsblatt** auf der rechten Seite führen Sie SQL-Befehle und -Skripte aus.
    *   In den Registerkarten **Abfrageergebnis** und **Skriptausgabe** unten auf der Seite prüfen Sie die Abfrageergebnisse und die Ausgabe, die aus ausgeführten Skripten generiert wurden.
    
    ![SQL-Arbeitsblatt in Oracle Database-Aktionen](images/database-actions.png "SQL-Arbeitsblatt in Oracle Database-Aktionen")
    

## Aufgabe 3: Beispieldaten in die Datenbank laden

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