# Notfall-Simulationstest: Blockdatenzugriff

## Einführung

Szenario: Sie sind Ihr Unternehmensmanager für Security Operations. Eine schwere Sicherheitswarnung in Ihrem Unternehmen ist gerade passiert. Nach Gesprächen mit dem CEO des Unternehmens hat der CISO die Entscheidung getroffen, den Zugriff auf die in der Cloud gespeicherten Daten einzustellen. Der CISO fordert Sie daher auf, sicherzustellen, dass auf alle in OCI, Ihrer Unternehmens-Cloud, gespeicherten verschlüsselten Daten nicht zugegriffen werden kann.

Um den Zugriff auf die Daten zu blockieren, deaktivieren Sie den Verschlüsselungsschlüssel, den Sie zu Beginn dieser praktischen Übung erstellt haben. Da dieser Schlüssel zum Verschlüsseln von Daten in dem von Ihnen erstellten Bucket sowie in der Autonomous Database verwendet wird, wird dadurch wiederum jede Möglichkeit deaktiviert, auch für OCI-Administratoren auf die verschlüsselten Daten sowie auf die Objekte selbst zuzugreifen.

Als Datenadministrator testen Sie den Zugriff auf die Daten, um zu erkennen, dass Sie weder auf die Daten noch auf die Objekte mehr zugreifen können.

Geschätzte Zeit: 10 Minuten

[Spaziergang durch das Labor](videohub:1_jhm25js1)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Simulieren Sie eine Notfallsituation, in der Sie als Kunde den Zugriff auf Ihre Daten in OCI vollständig blockieren möchten
*   Deaktivieren Sie den Verschlüsselungsschlüssel über die externe CipherTrust-Schlüsselverwaltungskonsole
*   Testen Sie den Zugriff auf die verschlüsselten Daten, und bestätigen Sie, dass Benutzer nicht mehr auf Daten im Speicher-Bucket und in Autonomous Database zugreifen können

## Aufgabe 1: Schlüssel in CipherTrust Manager deaktivieren

1.  Melden Sie sich bei der CipherTrust Manager-Konsole an.
    
    Wenn Sie den Link verloren haben: Um auf CipherTrust Manager as a Service zuzugreifen, müssen Sie die URL erstellen, um auf Ihren eigenen privaten Mandanten zuzugreifen. Dazu müssen Sie die folgende URL kopieren und in die Adressleiste Ihres Browsers einfügen: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM"** und **XXX** durch Ihre Kursteilnehmernummer ersetzen. Beispiel: Wenn Ihre Studierendennummer 001 lautet, lautet die vollständige URL zu Ihrem eigenen privaten CTM-Mandanten: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001"**.
    
    ![URL-Erstellung in der Adressleiste](images/ctm-address-bar.png "URL-Erstellung in der Adressleiste")
    
    Nachdem Sie Ihr Anmeldefenster geöffnet haben, melden Sie sich mit Ihrem Benutzer **"Secops\_XXX"** mit dem Kennwort an, das Ihnen zur Verfügung gestellt wurde. Wenn Sie diese Informationen nicht finden können, wenden Sie sich bitte an einen der Trainer, um Ihnen zu helfen.
    
    ![Melden Sie sich bei CipherTrust Manager an.](images/ctm-login.png "Melden Sie sich bei CipherTrust Manager an.")
    
2.  Geben Sie die Zugangsdaten ein, die Ihnen zur Verfügung gestellt wurden. Sie sind jetzt bei der CipherTrust Manager-Webkonsole angemeldet. Klicken Sie auf das Symbol "Cloud Key Manager":
    
    ![CipherTrust Manager-Webkonsole](images/ctm-page.png "CipherTrust Manager-Webkonsole")
    
3.  Klicken Sie im linken Fensterbereich auf **Cloud-Schlüssel > Oracle**.
    
    ![Oracle-Schlüssel](images/list-key.png "Oracle-Schlüssel")
    
4.  Klicken Sie auf die drei Punkte rechts neben der Schlüsselzeile, und wählen Sie **Deaktivieren** aus:
    
    ![Schlüssel deaktivieren](images/to-disable.png "Schlüssel deaktivieren")
    
5.  Ein neues Fenster fordert Sie zur Bestätigung auf. Klicken Sie auf **Deaktivieren**:
    
    ![Schlüssel deaktivieren](images/disable.png "Schlüssel deaktivieren")
    
6.  Klicken Sie auf **Alle aktualisieren**:
    
    ![Alles aktualisieren](images/disabling.png "Alles aktualisieren")
    
    Ein neues Fenster fordert Sie erneut zur Bestätigung auf. Klicken Sie auf **Alle aktualisieren**:
    
    ![Aktualisieren bestätigen](images/refresh.png "Aktualisieren bestätigen")
    
7.  Warten Sie, bis sich die Schlüssel im Status "Deaktiviert" befinden:
    
    ![Deaktivierte Schlüssel](images/disabled.png "Deaktivierte Schlüssel")
    

## Aufgabe 2: Sicherstellen, dass der Datenzugriff auf den Bucket nicht möglich ist

1.  Melden Sie sich beim OCI-Cloud-Mandanten als Data\_Manager\_XXX an, wobei "XXX" Ihre Studierendennummer ist (gehen Sie bitte zu Abschnitt _"Erste Schritte"_, um sich bei OCI anzumelden), und navigieren Sie durch das Haupt-Hamburger-Menü zu _"Storage > Object Storage > Buckets"_.
    
    ![Speicherbereiche](./images/buckets.png "Speicherbereiche")
    
2.  Wie Sie sehen können, ist der Bucket, den Sie mit einem externen Schlüssel erstellt haben, nicht mehr zugänglich, und selbst ein Benutzer wie Data Manager-Benutzer, der über vollständige Berechtigungen zur Verwaltung dieses Buckets verfügt, kann nicht auf Konfigurationselemente zugreifen oder Parameter wie Sichtbarkeit und Standard-Storage Tier anzeigen.
    
    ![Speicherbereiche](./images/inaccessible-bucket.png "Speicherbereiche")
    
    Wenn Sie auf Ihren Bucket klicken, können Sie nicht auf Folgendes zugreifen:
    
    ![Kein Zugriff](./images/no-access-to-bucket.png "Kein Zugriff")
    
    und ocw23 - Auf den Ressourcen-Bucket kann weiterhin zugegriffen werden, weil er von Oracle verwaltete Schlüssel standardmäßig konfiguriert wurde. Dies ist eine Best Practice, die Kunden verwenden können, wenn sie die Schlüssel und den Schlüssellebenszyklus für Ressourcen, die keine sensiblen Daten enthalten, nicht verwalten möchten. Auf diese Weise ermöglicht OCI Unternehmen eine sehr granulare und leistungsstarke Schlüsselmanagementlösung für alle ihre OCI-Ressourcen.
    
3.  Jetzt prüfen wir, ob alle vorab authentifizierten Anforderungen (PAR), die erstellt wurden, auch nicht mehr funktionieren, weil der Schlüssel deaktiviert wurde. Sie müssen die URL einer vorab authentifizierten Anforderung gespeichert haben, um auf die Excel-Datei zuzugreifen, die Sie in Übung 3, Aufgabe 2 in Ihren Bucket hochgeladen haben. Kopieren Sie diese gespeicherte Adresse und fügen Sie sie in Ihren Browser ein:
    
    ![Kein Zugriff auf PAR](./images/no-access-par.png "Kein Zugriff auf PAR")
    
    Wie Sie sehen können, funktioniert die vorab authentifizierte Anforderung nicht mehr, und die Fehlermeldung erklärt dies eindeutig, weil der Schlüssel für den Bucket nicht zugänglich ist.
    

## Aufgabe 3: Datenzugriff auf Autonomous Database prüfen

1.  Navigieren Sie durch das Hamburger-Hauptmenü zu _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Hier können zwei Situationen passieren. Entweder wird Autonomous Database gestoppt angezeigt, und das Ergebnis wird erwartet. Dies liegt daran, dass der verwendete Schlüssel den Status "Deaktiviert" aufweist und die Datenbank den Status des Masterverschlüsselungsschlüssels bereits geprüft hat. Wenn das der Fall ist, herzlichen Glückwunsch! Sie haben diese Übung abgeschlossen. Sie können das Ende überspringen und zur letzten Übung gehen.
    
    Da Autonomous Database Service diese Prüfung jedoch alle 15 Minuten ausführt, kann das Ergebnis in diesem Schritt variieren, je nachdem, wie lange Sie zwischen dem Deaktivieren der Schlüssel und dem Starten dieser Aufgabe und ob bereits eine Prüfung erfolgt ist oder nicht. Möglicherweise wird der folgende Bildschirm angezeigt, auf dem angezeigt wird, dass die Datenbank noch ausgeführt wird:
    
    ![Autonomous Database](./images/adb-running.png "Autonomous Database")
    
3.  In diesem Fall können Sie entweder ein wenig warten, wenn Sie Zeit haben, oder das einfachste für den Zweck der Übung ist, als Data Manager-Benutzer, die Datenbank zu stoppen und zu versuchen, sie zu starten, um zu bestätigen, dass es unmöglich ist. Klicken Sie auf den Namen Ihrer Autonomous Database:
    
    ![Autonomous Database](./images/adb-running.png "Autonomous Database")
    
    und klicken Sie auf **Weitere Aktionen** und dann auf **Stoppen**:
    
    ![Autonomous Database stoppen](./images/stop-adb.png "Autonomous Database stoppen")
    
    Ein Fenster fordert Sie zur Bestätigung auf. Klicken Sie auf **Stoppen**:
    
    ![Autonomous Database stoppen](./images/confirm-stop.png "Autonomous Database stoppen")
    
4.  Warten Sie, bis die Datenbank vollständig gestoppt ist:
    
    ![Autonomous Database gestoppt](./images/stopped-adb.png "Autonomous Database gestoppt")
    
5.  Versuchen Sie, die Datenbank erneut zu starten, indem Sie auf **Weitere Aktionen** und **Starten** klicken:
    
    ![Autonomous Database starten](./images/re-start.png "Autonomous Database starten")
    
    Wie Sie sehen können, ist es völlig unmöglich, die Datenbank zu starten oder Aktionen an ihrem Inhalt oder ihrer Konfiguration durchzuführen, da der Security Operation Manager den Schlüssel über die Thales CipherTrust Manager-Konsole remote deaktiviert hat:
    
    ![Autonomous Database starten](./images/try-start-adb.png "Autonomous Database starten")
    
    Wenn Sie auf **Start** klicken, kehren Sie immer zu diesem Bildschirm zurück, bis der Schlüssel in OCI Vault aktiviert wird. Dies wird in der nächsten Übung angezeigt.
    

Herzlichen Glückwunsch! Sie haben den Zugriff auf Ihre in OCI gespeicherten Unternehmensdaten vollständig blockiert, sodass Sie sie vor potenziellen Bedrohungen schützen, über die Ihr CISO benachrichtigt wurde. Jetzt können Sie zur nächsten Übung gehen und erfahren, wie Sie den Zugriff nach Ablauf der Warnung erneut aktivieren können.

## Weitere Informationen

*   [Daten mit Oracle Database Actions aus lokalen Dateien laden](https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/autonomous-database-shared/doc/load-data-sqldeveloper-web.html)
    
*   [Daten in Object Storage laden](https://docs.oracle.com/en-us/iaas/vision/vision-tutorials/vision/tutorials/Using_Pretrained_Models_in_the_Console/load_data.htm)
    
*   In diesem Beispiel konnte die Datenbank nicht gestartet werden, wenn der Schlüssel deaktiviert wurde. Dies liegt daran, dass der Schlüssel tatsächlich deaktiviert wurde und der Autonomous Database-Service eine klare Antwort vom OCI Vault-Service oder externen KMS erhält, dass sich der Schlüssel in einem deaktivierten Status befindet. Diese Situation unterscheidet sich stark von der Situation, in der OCI Vault an sich nicht verfügbar wäre. Um Produktionsdatenbanken vor jeder Art von Ereignissen zu schützen, die verhindern würden, dass die Datenbank auf Oracle Cloud Infrastructure Vault oder das externe KMS zugreift, wie z.B. einen Netzwerkausfall, behandelt Autonomous Database den Ausfall wie folgt:
    
        * There is a 2-hour grace period where the database remains up and running.
        
        * If Oracle Cloud Infrastructure Vault is not reachable at the end of the 2-hour grace period, the database Lifecycle State is set to Inaccessible. In this state existing connections are dropped and new connections are not allowed.
        
        * If Autonomous Data Guard is enabled, during or after the 2-hour grace period you can manually try to perform a failover operation. Autonomous Data Guard automatic failover is not triggered when you are using customer-managed encryption keys and the Oracle Cloud Infrastructure Vault is unreachable.
        
        * If Autonomous Database is stopped, then you cannot start the database when the Oracle Cloud Infrastructure Vault is unreachable. For this case, the work request shown when you click Work Requests on the Oracle Cloud Infrastructure console under Resources shows: 
        
        ```
        The Vault service is not accessible. 
        Your Autonomous Database could not be started. Please contact Oracle Support.
        ```
        
    
    Alle Einzelheiten dazu finden Sie in der [Dokumentation](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/manage-keys-notes.html).
    

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Damien Rilliard, Juli 2023