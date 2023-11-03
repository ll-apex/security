# Notfall-Simulationstest: Datenzugriff erneut aktivieren

## Einführung

Szenario: Die Sicherheitswarnung ist jetzt vorbei. Nach allen erforderlichen Prüfungen hat der CISO die Entscheidung getroffen, den Zugriff auf in der Cloud gespeicherte Daten erneut zu aktivieren. Der CISO fordert Sie daher auf, als Ihr Teammanager für Unternehmens-Sicherheitsvorgänge den normalen Betrieb und den Datenzugriff in OCI, Ihrer Unternehmens-Cloud, wiederherzustellen.

Um den Zugriff auf die Daten erneut zu aktivieren, aktivieren Sie den Verschlüsselungsschlüssel, den Sie zu Beginn dieser praktischen Übung erstellt haben. Da dieser Schlüssel zum Verschlüsseln von Daten im erstellten Bucket sowie in Autonomous Database verwendet wird, wird der Zugriff auf die Daten vollständig erneut aktiviert.

Als Datenadministrator testen Sie jetzt, ob der ordnungsgemäße Zugriff auf die Daten möglich ist, und normale Vorgänge können neu gestartet werden, nachdem der Sicherheitsalert beendet ist. Dies markiert das Ende dieser praktischen Übung!

Geschätzte Zeit: 10 Minuten

[Spaziergang durch das Labor](videohub:1_cg98w5b0)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Aktivieren Sie den in OCI verwendeten Verschlüsselungsschlüssel in der externen CipherTrust-Schlüsselverwaltungskonsole erneut, wenn der Sicherheitsalert abgelaufen ist
*   Testen Sie den Zugriff auf die verschlüsselten Daten, und bestätigen Sie, dass Benutzer sowohl im Speicher-Bucket als auch in Autonomous Database erneut auf Daten zugreifen können

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Alle vorherigen Übungen erfolgreich abgeschlossen

## Aufgabe 1: Schlüssel in CipherTrust Manager erneut aktivieren

1.  Gehen Sie zurück zur CipherTrust Manager-Konsole. Wenn Sie ihn geschlossen haben oder den Link verloren haben: Um auf CipherTrust Manager as a Service zuzugreifen, müssen Sie die URL erstellen, um auf Ihren eigenen privaten Mandanten zuzugreifen. Dazu müssen Sie diese URL "https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM" in die Adressleiste Ihres Browsers kopieren und einfügen und am Ende der URL Ihre Kursteilnehmernummer hinzufügen. Beispiel: Wenn Ihre Studierendennummer 001 lautet, lautet die vollständige URL zu Ihrem eigenen privaten CTM-Mandanten: "https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001". Sobald Sie auf Ihr Anmeldefenster zugreifen, melden Sie sich bitte mit Ihrem Benutzer "Secops\_XXX" mit dem Passwort an, das Ihnen zur Verfügung gestellt wurde. Wenn Sie diese Informationen nicht finden können, wenden Sie sich bitte an einen der Trainer, um Ihnen zu helfen.
    
    ![Melden Sie sich bei CipherTrust Manager an.](images/ctm-login.png "Melden Sie sich bei CipherTrust Manager an.")
    
    Geben Sie die Zugangsdaten ein, die Ihnen zur Verfügung gestellt wurden. Sie sind jetzt bei der CipherTrust Manager-Webkonsole angemeldet. Klicken Sie auf das Symbol "Cloud Key Manager":
    
    ![CipherTrust Manager-Webkonsole](images/ctm-page.png "CipherTrust Manager-Webkonsole")
    
2.  Klicken Sie im linken Fensterbereich auf **Cloud-Schlüssel > Oracle**.
    
    ![Oracle-Schlüssel](images/menu-keys.png "Oracle-Schlüssel")
    
3.  Klicken Sie auf die drei Punkte rechts neben der Schlüsselzeile, und wählen Sie **Aktivieren** aus:
    
    ![Tasten aktivieren](images/to-enable.png "Tasten aktivieren")
    
4.  Ein neues Fenster fordert Sie zur Bestätigung auf. Klicken Sie auf **Aktivieren**:
    
    ![Tasten aktivieren](images/enable-key.png "Tasten aktivieren")
    
5.  Klicken Sie auf **Alle aktualisieren**:
    
    ![Alles aktualisieren](images/refresh-all.png "Alles aktualisieren")
    
    Ein neues Fenster fordert Sie erneut zur Bestätigung auf. Klicken Sie erneut auf **Alle aktualisieren**:
    
    ![Alles aktualisieren](images/refresh.png "Alles aktualisieren")
    
6.  Warten Sie, bis sich die Schlüssel im Status "Aktiviert" befinden:
    
    ![Aktivierte Schlüssel](images/enabled-key.png "Aktivierte Schlüssel")
    

## Aufgabe 2: Sicherstellen, dass der Datenzugriff auf den Bucket möglich ist

1.  Melden Sie sich beim OCI-Cloud-Mandanten als Data\_Manager\_XXX an. Dabei ist "XXX" Ihre Kursteilnehmernummer (in der Übung "TODO" erfahren Sie, wie Sie sich bei OCI anmelden), und navigieren Sie durch das Hamburger-Hauptmenü zu _"Storage > Object Storage > Buckets"_.
    
    ![Speicherbereiche](./images/buckets.png "Speicherbereiche")
    
2.  Wie Sie sehen können, ist der Bucket, den Sie mit einem externen Schlüssel erstellt haben, wieder zugänglich:
    
    ![Speicherbereiche](./images/bucket-visible.png "Speicherbereiche")
    
    Wenn Sie auf Ihren Bucket klicken, können Sie auf die unten aufgeführten Objekte im Bucket zugreifen und diese anzeigen:
    
    ![Zugriff](./images/upload-object.png "Zugriff")
    
3.  Jetzt prüfen wir, ob die von Ihnen erstellte vorab authentifizierte Anforderung (PAR) wieder funktionsfähig ist, wenn der Schlüssel aktiviert ist. Kopieren Sie die URL, die Sie in Übung 3, Aufgabe 2 gespeichert haben, und fügen Sie sie erneut in Ihren Browser ein. Bestätigen Sie, dass Sie das Dokument herunterladen können. Daher haben wir jetzt bestätigt, dass die erneute Aktivierung des Schlüssels aus der externen CipherTrust Manager-Instanz ein voll funktionsfähiges Verhalten in den OCI-Speicher-Bucket zurückbringt.
    

## Aufgabe 3: Datenzugriff auf Autonomous Database prüfen

1.  Navigieren Sie durch das Hamburger-Hauptmenü zu _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Hier können Sie zwei Situationen haben. Entweder wurde die Datenbank bereits neu gestartet, wenn der Autonomous Database-Service bereits geprüft hat, ob der Schlüssel erneut aktiviert ist. Oder höchstwahrscheinlich wird die Datenbank noch gestoppt:
    
    ![Autonomous Database gestoppt](./images/stopped-adb.png "Autonomous Database gestoppt")
    
3.  Sie starten die Datenbank, weil der Schlüssel erneut aktiviert wird, indem Sie auf **Weitere Aktionen** und **Starten** klicken:
    
    ![Autonomous Database starten](./images/re-start.png "Autonomous Database starten")
    
    Wenn Sie auf **Start** klicken, wird die Datenbank gestartet:
    
    ![Autonomous Database starten](./images/starting-adb.png "Autonomous Database starten")
    
    Warten Sie, bis die Datenbank gestartet wurde:
    
    ![Autonomous Database verfügbar](./images/adb-available.png "Autonomous Database verfügbar")
    
    Wie Sie sehen können, ist es jetzt möglich, die Datenbank zu starten, da der Schlüssel erneut aktiviert und vom Autonomous Database-Service erreichbar ist.
    
4.  Um sicherzustellen, dass die Daten entschlüsselt werden können, versuchen wir, auf die Daten in der Datenbank zuzugreifen. Klicken Sie dazu auf das Launchpad für **Datenbankaktionen**:
    
    ![Datenbankaktionen](./images/db-actions.png "Datenbankaktionen")
    
5.  Klicken Sie anschließend auf das **SQL**\-Feld unter "Development":
    
    ![SQL-Entwicklung](./images/sql.png "SQL-Entwicklung")
    
6.  Web SQL Development UI ist geöffnet. Jetzt können Sie die Daten in der Tabelle anzeigen, mit der rechten Maustaste auf die Tabelle klicken und auf **Öffnen** klicken:
    
    ![Tabelle öffnen](./images/see-data.png "Tabelle öffnen")
    
7.  Klicken Sie im neuen Fenster auf die Registerkarte **Daten**:
    
    ![Daten ansehen](./images/data.png "Daten ansehen")
    
    Wie Sie sehen können, haben Sie jetzt wieder vollständige Sichtbarkeit der Daten in der Datenbank, da der Schlüssel erneut aktiviert wurde.
    

Herzlichen Glückwunsch! Sie haben dieses praktische Labor abgeschlossen! Bitte rufen Sie einen der Trainer an, um Ihren Abschluss zu zeigen und Fragen zu stellen. Wir hoffen, Sie haben es genossen und etwas gelernt! Das Team ist hier, um Ihre Fragen zu beantworten. Und wenn es dir gefällt, sprich darüber um dich herum! Bis bald!

## Weitere Informationen

*   [Eigene Schlüssel in Vault für serverseitige Verschlüsselung verwenden](https://docs.oracle.com/en-us/iaas/Content/Object/Tasks/encryption.htm#UsingYourKMSKeys)
*   [Verschlüsselungsschlüssel in Autonomous Database verwalten](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-encrypt-set-rotate-keys.html#GUID-0795135D-B057-4DBC-92C9-368AF4C82D0A)

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Sonia, August 2023