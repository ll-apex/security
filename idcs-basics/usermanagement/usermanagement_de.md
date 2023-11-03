# Instanz bereitstellen

## Einführung

Es ist oft erforderlich, dass Organisationen einen Mitarbeiter oder Auftragnehmer oder eine Identität manuell in ein zentrales Identitätssystem wie IDCS integrieren. In diesem Anwendungsfall fügt ein IDCS-Benutzeradministrator manuell einen neuen Benutzer im IDCS-Service hinzu. In der Regel erfolgt dies durch automatisiertes Provisioning, Bulk-Flat-File-Import oder Synchronisierung mit Ihrem On-Premise-Active Directory.

Die IDCS-Features werden zu 100% mit dem IDCS-REST-API-Service erstellt. Daher kann jede Aufgabe, die wir interaktiv in der Weboberfläche ausführen, auch über eine benutzerdefinierte App mit denselben REST-API-Aufrufen ausgeführt werden. Es ist ein klarer Vorteil der API-First-Architektur von Oracle mit IDCS.

IDCS unterstützt das Onboarding von Benutzern (auch Gruppen) aus On-Premise-Active Directory über Dateiupload, REST-API, On-Premise-Oracle Identity Management-Lösung oder manuell über die IDCS-Admin-Konsole. Für den Workshop verwenden wir die Option zum Hochladen von Dateien und API-Aufrufe zur Benutzerverwaltung.

Geschätzte Laborzeit: 90 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Benutzer in UI erstellen
*   Benutzer mit CSV importieren
*   Benutzer mit REST-APIs erstellen

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Postman-native Anwendung lokal installiert

## Aufgabe 1: Benutzer in UI erstellen

*   _Personen_:
    *   Administrator

1.  Gehen Sie mit den Zugangsdaten Ihres Administratoraccounts zur IDCS-Admin-Konsole. Wählen Sie das Menü _Benutzer_ auf der linken Seite, und klicken Sie auf _+Add_, oder wählen Sie im Dashboard das Symbol _Benutzer hinzufügen_ aus.
    
    ![Bild](images/L1001.png)
    
2.  Füllen Sie alle erforderlichen Felder aus, und klicken Sie auf "Fertig stellen". Verwenden Sie in den Übungen eine konsistente Benennungskonvention. Beispiel:
    
    *   Vorname: Vorname
    *   Nachname: Nachname-UI
    *   Benutzername/E-Mail: firstname.lastname+ui@domain.tld
    
    ![Bild](images/L1002.png)
    
3.  Benutzererstellung prüfen. Gehen Sie dazu in der Admin-Konsole zur Registerkarte _Benutzer_. Prüfen Sie, ob die neuen Benutzer in der Konsole angezeigt werden.
    
    ![Bild](images/L1003.png)
    
        Once a new user is created in the IDCS service, the user receives an email invitation to finish first time login formalities such as setting a password to logging into IDCS.
        

## Aufgabe 2: Benutzer mit CSV importieren

*   _Personen_:
    *   Administrator

Als Identitätsdomainadministrator oder Benutzeradministrator können Sie Benutzeraccounts per Batchimport mit einer CSV-Datei (kommagetrennte Werte) importieren.

1.  Laden Sie eine CSV-Datei hoch. Wählen Sie dazu das Menü _Benutzer_ auf der linken Seite, und klicken Sie auf _Importieren_. Klicken Sie im Abschnitt "Hilfe" auf der rechten Seite auf _Beispieldatei herunterladen_.
    
    ![Bild](images/L1004.png)
    
    **Alternativ** können Sie eine Liste vorhandener Benutzer exportieren: Filtern Sie den in der UI erstellten Benutzer, und klicken Sie dann auf "Exportieren" -> "Alle exportieren". Bestätigen Sie den Export, gehen Sie zum Job, und laden Sie die Datei herunter
    
    ![Bild](images/L1005.png)
    
    Extrahieren Sie die ZIP-Datei, und öffnen Sie _Users.csv_, oder öffnen Sie die Datei _UserExport... .csv_. Überprüfen Sie den Inhalt der Datei in Ihrem bevorzugten Editor. Sie können die Standardbeispiele verwenden, die Sie in der CSV-Datei finden, oder Sie können mehr ausfüllen, wenn Sie möchten, aber sie ähneln den Beispielen. Beispiel: Wenn Sie den exportierten vorhandenen Benutzer verwenden, ändern Sie das Suffix (UI) in einen anderen (z.B. CSV).
    
2.  Gehen Sie mit den Zugangsdaten Ihres Administratoraccounts zur IDCS-Admin-Konsole. Wählen Sie das Menü _Benutzer_ auf der linken Seite, oder wählen Sie das Symbol _Benutzer_ im Dashboard aus.
    
    ![Bild](images/L1006.png)
    
3.  Klicken Sie auf die Schaltfläche _Importieren_.
    
    ![Bild](images/L1007.png)
    
4.  Das Fenster "Benutzer importieren" wird angezeigt. Klicken Sie auf _Durchsuchen_. Wählen Sie die CSV-Datei aus. Klicken Sie auf _Importieren_.
    
    ![Bild](images/L1008.png)
    
        By uploading the sample file, sample users will be uploaded into IDCS. In order to upload different users, the csv sample file needs to be altered.
        
5.  Gehen Sie zur Menüoption _Jobs_, und prüfen Sie, ob der Importjob erfolgreich abgeschlossen wurde. Klicken Sie auf _Details anzeigen_, um zu prüfen, ob alle Benutzer importiert wurden. Außerdem wird Ihnen die Liste aller Benutzer von csv angezeigt, zusammen mit allen Attributdetails und dem Erstellungsstatus.
    
    ![Bild](images/L1009.png)
    
6.  Prüfen Sie die Benutzererstellung, indem Sie in der Admin-Konsole zur Registerkarte _Benutzer_ gehen. Prüfen Sie, ob die neuen Benutzer in der Konsole angezeigt werden.
    
    ![Bild](images/L1010.png)
    
7.  Klicken Sie auf den Ziel-Endbenutzer (z.B. Vorname Nachname-CSV), und prüfen Sie die detaillierten Attributinformationen des Benutzers
    
8.  Ändern Sie aus Spaß die E-Mail-Adresse für den Benutzer in eine, die Sie steuern, und wählen Sie _Benutzer aktualisieren_ aus. Klicken Sie dann auf die Schaltfläche _Kennwort zurücksetzen_.
    
    ![Bild](images/L1011.png)
    
9.  Prüfen Sie Ihre E-Mail-Adresse auf die Nachricht zum Zurücksetzen, und ändern Sie das Kennwort.
    
    ![Bild](images/L1012.png)
    
10.  Öffnen Sie dann einen anderen Browser (wenn Sie Chrome verwenden, wählen Sie Inkognito-Fenster) und melden Sie sich als Administrator bei Ihrer IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
    
    ![Bild](images/L1013.png)
    
    Sie sehen, dass der Benutzer noch keinen Zugriff auf Anwendungen hat. In Übung 2 zeigen wir Ihnen, wie ein Benutzer Anwendungen anfordern kann.
    
    ![Bild](images/L1014.png)
    

## Aufgabe 3: API-Benutzer mit REST-APIs erstellen

*   _Personen_:
    *   Administrator

Dieser Anwendungsfall beinhaltet API-Aufrufe an IDCS mit einem REST-Client, in diesem Fall Postman.

Die Postman-Sammlung relevanter REST-API-Aufrufe wird jedem Teilnehmer bereitgestellt.

    This use case will demonstrate using a common utility, Postman, for connecting to Identity Cloud Service via the out-of-the-box REST API.
    

### Registrieren Sie eine Client-POSTMAN-Anwendung in IDCS

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Wählen Sie die Registerkarte _Anwendungen_ im IDCS-Dashboard aus, das nach der Anmeldung angezeigt wird
    
    ![Bild](images/L1015.png)
    
3.  Klicken Sie auf die Schaltfläche _Hinzufügen_, um eine neue Anwendung für Postman zu erstellen. Damit Postman IDCS-REST-APIs aufrufen kann, ist zunächst eine CLIENT\_ID und CLIENT\_SECRET erforderlich, die Postman dazu autorisieren. Dies kann erreicht werden, indem Sie einen vertraulichen Anwendungstyp in IDCS mit bestimmten Autorisierungsberechtigungstypen erstellen, wie Sie in den folgenden Schritten sehen.
    
    ![Bild](images/L1016.png)
    
4.  Wählen Sie im Popup-Menü der Anwendungstypen die Option _Vertrauliche Anwendung_ aus.
    
    ![Bild](images/L1017.png)
    
5.  Setzen Sie den _Namen_ auf "Postman-<\>", und klicken Sie auf _Weiter_.
    
    ![Bild](images/L1018.png)
    
6.  Klicken Sie auf _Diese Anwendung jetzt als Client konfigurieren_, um die Autorisierungsberechtigungstypen und die API-Rolle anzugeben.
    
    ![Bild](images/L1019.png)
    
7.  Aktivieren Sie alle Kontrollkästchen _Zulässige Berechtigungstypen_, und setzen Sie _URL umleiten_ auf https://localhost.. Gehen Sie zu _Clientzugriff auf Identity Cloud Service-Admin-APIs erteilen_ (unten auf der Seite), und fügen Sie die folgende API-Rolle _Identitätsdomainadministrator_ hinzu. Auf diese Weise erteilen Sie grundsätzlich Zugriff für diese IDCS-App auf die gesamten IDCS-APIs. Klicken Sie anschließend auf _Weiter_.
    
    ![Bild](images/L1020.png)
    
8.  Nehmen Sie keine Änderungen an _Ressourcen_ vor, und klicken Sie auf _Weiter_.
    
    ![Bild](images/L1021.png)
    
9.  Nehmen Sie in der _Web Tier Policy_ keine Änderungen vor, und klicken Sie auf _Weiter_.
    
    ![Bild](images/L1022.png)
    
10.  Um die IDCS-App abzuschließen, die Postman per Auth2.0-Standard API-Rollen erteilt, klicken Sie auf _Fertigstellen_.
    
    ![Bild](images/L1023.png)
    
11.  Notieren Sie sich nach dem Erstellen der Anwendung die _Client-ID_ und das _Client Secret_, und klicken Sie auf _Schließen_. Diese werden von der Postman-Desktopanwendung verwendet, um IDCS-APIs mit dem Auth2.0-Standardprotokoll aufzurufen.
    
    ![Bild](images/L1024.png)
    
12.  Klicken Sie auf die Schaltfläche _Aktivieren_
    
    ![Bild](images/L1025.png)
    
13.  Anwendungsaktivierung bestätigen
    
    ![Bild](images/L1026.png)
    
14.  Die Anwendung ist jetzt aktiv und einsatzbereit.
    
    ![Bild](images/L1027.png)
    
15.  Bei IDCS anmelden
    

### Postman konfigurieren

1.  Öffnen Sie Postman. Ignorieren Sie gegebenenfalls alle Startnachrichten.
    
    ![Bild](images/L1028.png)
    
2.  Zuerst müssen IDCS Postman-Umgebungsvariablen, globale Variablen und IDCS-API-Collection importiert werden. Klicken Sie in der linken oberen Ecke auf die Schaltfläche _Import_ (Importieren).
    
    ![Bild](images/L1029.png)
    
3.  Wählen Sie _Aus Link importieren_ aus, und geben Sie die URL für die [Beispielumgebung](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/example_environment.json) in das Feld ein, um die Umgebungsvariablen zu importieren. Klicken Sie auf _Importieren_
    
    ![Bild](images/L1030.png)
    
4.  Um die REST-API-Postman-Collection für Oracle Identity Cloud Service zu importieren, klicken Sie auf der Postman-Hauptseite erneut auf _Importieren_.
    
5.  Wählen Sie im Dialogfeld "Importieren" die Option _Aus Link importieren_ aus, geben Sie die [IDCS-Postman-Collection](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/REST_API_for_Oracle_Identity_Cloud_Service.postman_collection.json)\-URL in das Feld ein, und klicken Sie auf _Importieren_.
    
        The left pane of Postman has a sub-tab called Collections.
        With Collections selected, you will see the various REST statements which have been pre-configured for today’s workshop.
        
6.  Klicken Sie zum Importieren der globalen Variablendatei auf _Importieren_.
    
7.  Wählen Sie im Dialogfeld "Importieren" die Option _Aus Link importieren_ aus, geben Sie die URL für die [globalen IDCS-Variablen](https://github.com/oracle/idm-samples/raw/master/idcs-rest-clients/oracle_identity_cloud_service_postman_globals.json) in das Feld ein, und klicken Sie auf _Importieren_.
    
8.  Wählen Sie die Umgebung _Oracle Identity Cloud Service-Beispielumgebung mit Variablen_ aus, und klicken Sie auf die Schaltfläche _Einstellungen_ (Zahnradsymbol), um _Umgebungen verwalten_
    
    ![Bild](images/L1032.png)
    
9.  Klicken Sie auf die neu erstellte Umgebung, die _Oracle Identity Cloud Service-Beispielumgebung mit Variablen_ entspricht. Sie ist die einzige, die in Ihren Fällen bei einer neuen Postman-Installation verfügbar ist.
    
    ![Bild](images/L1033.png)
    
10.  Legen Sie die folgenden Parameter für Anfangs- und aktuelle Werte fest, um ein IDCS-Zugriffstoken abrufen zu können:
    
    | Parameter | Wert |
    | --- | --- |
    | HOST | Ihre IDCS-Mandanten-URL. Beispiel: https://IDCS-8b000000000000000000000000000000.identity.oraclecloud.com |
    | CLIENT\_ID | Postman IDCS-Clientanwendung CLIENT\_ID |
    | CLIENT\_SECRET | Postman IDCS-Clientanwendung CLIENT\_SECRET |
    
    ![Bild](images/L1034.png)
    
11.  Klicken Sie nach dem Abschluss auf _Aktualisieren_.
    

### Zugriffstoken anfordern

    The steps performed above are being done to obtain the Access Token, which is shown in the following screen shot.
    
    There are several types of OAUTH requests supported by IDCS, including client credentials.  With a client credentials request, the client ID and client secret are used by the calling application to identify itself and to request an Access Token.  The Access Token is available for use by the application until the time that it expires.
    
    This Access Token establishes trust between your application and IDCS.
    

1.  Blenden Sie auf der Registerkarte _Sammlungen_ _OAuth_, _Token_ ein.
    
2.  Wählen Sie _access\_token (Clientzugangsdaten) abrufen_ aus, und klicken Sie auf _Senden_. Das Zugriffstoken wird in der Antwort von Oracle Identity Cloud Service zurückgegeben.
    
    ![Bild](images/L1035.png)
    
3.  Prüfen Sie, ob der Status _200 OK_ angezeigt wird. ![Bild](images/L1036.png)
    
4.  Markieren Sie den Inhalt des Zugriffstokens zwischen den Anführungszeichen, und klicken Sie mit der rechten Maustaste (). Wählen Sie im Kontextmenü _Set: Oracle Identity Cloud Service Example Environment with Variables_ im sekundären Menü _access\_token_ aus. Der hervorgehobene Inhalt wird als Zugriffstokenwert zugewiesen.
    
    ![Bild](images/L1037.png)
    

### IDCS-Benutzer über API erstellen

1.  Blenden Sie auf der Registerkarte _Sammlungen_ die Optionen _Benutzer_, _Erstellen_ ein.
    
2.  Wählen Sie _Benutzer erstellen_ aus. Die Anforderungsinformationen werden mit einigen Standardwerten angezeigt, wie unter dem Druckbildschirm zu sehen ist. Sie können sie entweder nach Ihren Wünschen ändern oder einfach die Standardwerte verwenden. Beispiel:
    

*   givenName: Vorname
*   familyName: Nachname-REST
*   Benutzername/E-Mail->Wert (x2): firstname.lastname+REST@domain.tld

3.  Klicken Sie auf _Hauptteil_ und dann auf _Senden_.
    
    ![Bild](images/L1038.png)
    
4.  Bestätigen Sie in der Antwort, dass der Status _Erstellt 201_ angezeigt wird und dass der Antwortbody Details zu dem Benutzer anzeigt, der erfolgreich in Oracle Identity Cloud Service erstellt wurde.
    
5.  In der IDCS-UI wird der Benutzer als erstellt angezeigt.
    
    ![Bild](images/L1039.png)
    

Sie können Benutzer auch _ändern_ und _löschen_ und vieles mehr über die REST-API. Weitere Informationen finden Sie in der folgenden Dokumentation.

Sie können jetzt mit der nächsten Übung fortfahren

## Weitere Informationen

Tutorial 1: [Oracle Identity Cloud Service: Erster REST-API-Aufruf](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_1stcall_obe/rest_1stcall.html)

Tutorial 2: [Oracle Identity Cloud Service: Benutzer mit REST-API-Aufrufen verwalten](http://www.oracle.com/webfolder/technetwork/tutorials/obe/cloud/idcs/idcs_rest_users_obe/rest_users.html)

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020