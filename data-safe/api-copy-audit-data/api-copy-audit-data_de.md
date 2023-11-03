# Auditdaten mit der Oracle Data Safe-REST-API in den Objektspeicher kopieren

## Einführung

Wenn Sie den Audit Trail der Zieldatenbank in Oracle Data Safe starten, beginnt Oracle Data Safe mit dem Kopieren von Auditdatensätzen aus dem Audit Trail der Datenbank in das Oracle Data Safe-Repository. In dieser Übung kopieren Sie mit der Oracle Data Safe-API (Anwendungsprogrammierschnittstelle) die Auditdaten von Oracle Data Safe für die Zieldatenbank in den Objektspeicher.

Geschätzte Laborzeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Bucket zum Speichern der Auditdaten erstellen
*   Audit Trail für die Zieldatenbank in Oracle Data Safe starten
*   Menge der von Oracle Data Safe erfassten Auditdaten anzeigen
*   Greifen Sie auf Cloud Shell in Oracle Cloud Infrastructure zu, und prüfen Sie das SDK für die Java-Installation
*   SDK konfigurieren
*   Java-Datei kompilieren
*   Compartment-OCID für die Zieldatenbank abrufen
*   Kompilierte Java-Datei ausführen
*   Prüfen Sie, ob die Auditdaten in den Bucket kopiert werden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

### Annahmen

In Cloud Shell werden die folgenden Anwendungsversionen ausgeführt:

*   Java-Version 11.0.17
*   Java(TM) SE Runtime Environment 18.9 (build 11.0.17+10-LTS-269)
*   Linux 7.9
*   Oracle Cloud Infrastructure - Java-SDK 3.2.2

## Aufgabe 1: Bucket zum Speichern der Auditdaten erstellen

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Speicher**, **Buckets** aus.
    
2.  Vergewissern Sie sich, dass das Compartment ausgewählt ist.
    
3.  Klicken Sie auf **Bucket erstellen**.
    
    Der Bereich **Bucket erstellen** wird angezeigt.
    
4.  Geben Sie für den Bucket-Namen **DataSafeAuditData** ein.
    
5.  Behalten Sie die Standardeinstellungen bei, und klicken Sie auf **Erstellen**.
    

## Aufgabe 2: Audittrail für die Zieldatenbank in Oracle Data Safe starten

1.  Wählen Sie im Navigationsmenü **Oracle Database**, **Data Safe - Datenbanksicherheit** aus.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Aktivitätsauditing**.
    
3.  Klicken Sie links unter **Zugehörige Ressourcen** auf **Audittrails**.
    
4.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
5.  Klicken Sie rechts auf den Namen der Zieldatenbank für **UNIFIED\_AUDIT\_TRAIL**.
    
    Die Seite **Protokolldetails** wird angezeigt.
    
6.  Klicken Sie auf **Start**.
    
    Das Dialogfeld **Audittrail starten: UNIFIED\_AUDIT\_TRAIL** wird angezeigt.
    
7.  Setzen Sie das Startdatum auf den Anfang des aktuellen Monats.
    
    *   Wenn es sich derzeit um den ersten Tag des Monats handelt, können Sie den Vortag auswählen, um sicherzustellen, dass Sie alle Daten erfassen.
    *   Wählen Sie nicht die Option **Automatisch löschen**.
8.  Klicken Sie auf **Start**. Warten Sie, bis sich der **Erfassungsstatus** von **Wird gestartet** in **Wird erfasst** und dann in **IDLE** ändert. Es dauert etwa eine Minute.
    

## Aufgabe 3: Menge der von Oracle Data Safe erfassten Auditdaten anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie unter **Security Center** auf **Auditprofile**.
    
3.  Klicken Sie rechts auf den Namen der Zieldatenbank.
    
    Die Seite **Auditprofildetails** für die Zieldatenbank wird angezeigt.
    
4.  Scrollen Sie auf der Seite nach unten zum Abschnitt **Auditvolumen berechnen**.
    
5.  Klicken Sie auf **Von Data Safe erfasst**.
    
    Das Dialogfeld **Erfasstes Volumen berechnen** wird angezeigt.
    
6.  Setzen Sie die Felder **Startmonat** und **Endmonat** auf den ersten und letzten Tag des aktuellen Monats, und klicken Sie auf **Berechnen**.
    
7.  Notieren Sie sich in der Spalte **In Data Safe erfasst (Online)** die Anzahl der von Oracle Data Safe erfassten Auditdatensätze.
    

## Aufgabe 4: Auf Cloud Shell in Oracle Cloud Infrastructure zugreifen und SDK für Java-Installation prüfen

Das Oracle Cloud Infrastructure-SDK für Java (oci-java-sdk) stellt ein Java-SDK bereit, mit dem Sie Ihre Oracle Cloud Infrastructure-Ressourcen verwalten können.

1.  Um Cloud Shell zu öffnen, klicken Sie in der oberen rechten Ecke der Oracle Cloud Infrastructure-Konsole auf das Symbol **Entwicklertools**, und wählen Sie **Cloud Shell** aus.
    
    Wenn Sie Cloud Shell zum ersten Mal öffnen, ist Ihr aktuelles Verzeichnis Ihr Home-Verzeichnis. Beispiel: `/home/jody_glove`.
    
2.  (Optional) Setzen Sie die Cloud Shell-Umgebung zurück. Mit dem folgenden Befehl werden alle Daten im Verzeichnis `$HOME` auf dem Cloud Shell-Rechner gelöscht, und die Dateien `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` und `$HOME/.emacs` werden auf ihre Standardwerte zurückgesetzt. Geben Sie zur Bestätigung in der Eingabeaufforderung **y** ein.
    
        $ <copy>csreset --all</copy>
        
3.  Prüfen Sie das Verzeichnis `/usr/lib64/java-OCI-SDK`. Dies ist der OCI-Java-SDK-Speicherort.
    
        $ <copy>ls /usr/lib64/java-oci-sdk</copy>
        
        addons  apidocs  buildTools  CHANGELOG.md  CONTRIBUTING.md  examples  lib  LICENSE.txt  NOTICE.txt  README.md  shaded  third-party  THIRD_PARTY_LICENSES.txt
        
4.  Listen Sie den Inhalt des `/usr/lib64/java-oci-sdk/lib`\-Verzeichnisses auf. Notieren Sie sich die Version der Datei `oci-java-sdk-full-version.jar`. Im folgenden Beispiel ist die Version 3.3.0.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        jersey  jersey3  oci-java-sdk-full-3.3.0.jar  oci-java-sdk-full-3.3.0-javadoc.jar  oci-java-sdk-full-3.3.0-sources.jar
        
5.  Listen Sie die Drittanbieterbibliotheken im Verzeichnis `/usr/lib64/java-oci-sdk/third-party/lib` auf.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/third-party/lib</copy>
        
6.  Listen Sie die Beispiele im Verzeichnis `/usr/lib64/java-oci-sdk/examples` auf. Beachten Sie, dass eine `DataSafeRestAPIClientExample.java`\-Datei vorhanden ist. Dieses Java-Programm enthält Oracle Data Safe-REST-API-Befehle, mit denen Auditdaten aus einem angegebenen Compartment in einen angegebenen Bucket im Objektspeicher kopiert werden.
    
        $ <copy>ls /usr/lib64/java-oci-sdk/examples</copy>
        
        ...
        DataSafeRestAPIClientExample.java
        ...
        
7.  Prüfen Sie die Datei `DataSafeRestAPIClientExample.java`.
    
        $ <copy>cat /usr/lib64/java-oci-sdk/examples/DataSafeRestAPIClientExample.java</copy>
        

## Aufgabe 5: SDK konfigurieren

Für die Oracle Cloud Infrastructure-SDKs sind grundlegende Konfigurationsinformationen erforderlich, wie Benutzerzugangsdaten und Mandanten-OCID. In dieser Aufgabe stellen Sie diese Informationen bereit, indem Sie eine Konfigurationsdatei erstellen.

1.  Erstellen Sie ein Verzeichnis namens `.oci`, erteilen Sie sich selbst `read/write/execute`\-Berechtigungen dafür, und wechseln Sie dann zu diesem.
    
        $ <copy>mkdir ~/.oci</copy>
        $ <copy>chmod 777 ~/.oci</copy>
        $ <copy>cd ~/.oci</copy>
        
2.  Klicken Sie in der oberen rechten Ecke der Oracle Cloud Infrastructure-Konsole auf das Symbol **Profil**, und wählen Sie Ihren Benutzernamen aus.
    
3.  Klicken Sie links auf **API-Schlüssel**.
    
4.  Klicken Sie auf **API-Schlüssel hinzufügen**.
    
    Das Dialogfeld **API-Schlüssel hinzufügen** wird angezeigt.
    
5.  Lassen Sie **API-Schlüsselpaar generieren** ausgewählt, und klicken Sie auf **Private Key herunterladen**. Ein Private Key (PEM-Datei) wird in Ihren Browser heruntergeladen. Speichern Sie die Private-Key-Datei in einem lokalen Verzeichnis Ihrer Wahl auf Ihrem Computer.
    
6.  Klicken Sie auf **Add**.
    
    Das Dialogfeld **Vorschau der Konfigurationsdatei** wird mit einer Vorschau der Konfigurationsdatei angezeigt.
    
7.  Kopieren Sie den Inhalt der Konfigurationsdateivorschau in eine temporäre lokale Textdatei. Stellen Sie sicher, dass Sie `[DEFAULT]` aufnehmen. Es sollte ungefähr so aussehen:
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
8.  Klicken Sie auf **Schließen**.
    
    Der neue API-Schlüssel wird unter **API-Schlüssel** aufgeführt.
    
9.  Klicken Sie in Cloud Shell in der oberen rechten Ecke auf das Symbol **Cloud Shell-Menü**, und wählen Sie **Hochladen** aus.
    
    Das Dialogfeld "**Datei in Ihr Home-Verzeichnis hochladen**" wird angezeigt.
    
10.  Ziehen Sie die Private-Key-Datei in das Dialogfeld, und klicken Sie auf **Hochladen**.
    
    Ihre Private-Key-Datei wird in Ihr Home-Verzeichnis hochgeladen.
    
11.  Um das Dialogfeld **Dateitransfers** zu schließen, klicken Sie auf **Ausblenden**.
    
12.  Verschieben Sie die Private-Key-Datei in das Verzeichnis `~/.oci`. Im folgenden Beispiel ersetzen Sie `your-private-key-file.pem` durch Ihren eigenen Private-Key-Dateinamen.
    
        $ <copy>mv ~/your-private-key-file.pem ~/.oci/your-private-key-file.pem</copy>
        
13.  Erstellen Sie mit dem vi-Editor eine Konfigurationsdatei im Verzeichnis `~/.oci`.
    
        $ <copy>vi config</copy>
        
14.  Fügen Sie den Inhalt der Konfigurationsdatei in die Datei `config` ein. Der Inhalt sollte dem folgenden Code ähneln. Vergessen Sie nicht, `[DEFAULT]` einzuschließen.
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=ff:35...
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
15.  Ändern Sie die letzte Zeile als Pfad zu Ihrer Private-Key-Datei. Ersetzen Sie im folgenden Beispiel `your-private-key-file.pem` durch Ihren eigenen Private-Key-Dateinamen. Entfernen Sie den Text **\# TODO**.
    
        <copy>key_file=~/.oci/your-private-key-file.pem</copy>
        
16.  Speichern und schließen Sie die Datei (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie dann die **Eingabe**).
    

## Aufgabe 6: Java-Dateien kompilieren

Verwenden Sie den Befehl `javac`, um die Datei `DataSafeRestAPIClientExample.java` zu kompilieren. Die folgenden beiden Variablen sind bereits in Cloud Shell festgelegt. Sie können diese beim Kompilieren und Ausführen des Java-Programms verwenden.

*   `$OCI_JAVA_SDK_LOCATION` = `/usr/lib64/java-oci-sdk`
*   `$OCI_JAVA_SDK_FULL_JAR_LOCATION` = `/usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-version.jar`

1.  Kopieren Sie `DataSafeRestAPIClientExample.java` in das aktuelle Verzeichnis (`~/.oci`).
    
        $ <copy>cp $OCI_JAVA_SDK_LOCATION/examples/DataSafeRestAPIClientExample.java .</copy>
        
2.  Kompilieren Sie `DataSafeRestAPIClientExample.java`. Es gibt keine Ausgabe, nachdem das Programm kompiliert wurde.
    
        $ <copy>javac -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/* DataSafeRestAPIClientExample.java</copy>
        

## Aufgabe 7: Compartment-OCID für die Zieldatenbank abrufen

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Oracle Database**, **Data Safe - Datenbanksicherheit** aus.
    
2.  Klicken Sie auf der linken Seite auf **Zieldatenbanken**.
    
3.  Wählen Sie links Ihr Compartment aus.
    
4.  Klicken Sie rechts auf den Namen der Zieldatenbank.
    
    Die Seite **Zieldatenbankdetails** wird angezeigt.
    
5.  Notieren Sie sich auf der Registerkarte **Zieldatenbankdetails** das Compartment.
    
6.  Wählen Sie im Navigationsmenü die Option **Identität und Sicherheit** aus. Klicken Sie dann rechts unter **Identität** auf **Compartments**.
    
7.  Klicken Sie auf den Namen des Compartments.
    
    Die Seite **Compartment-Details** wird angezeigt.
    
8.  Klicken Sie auf der Registerkarte **Compartment-Informationen** auf den Link **Kopieren** neben **OCID**, und fügen Sie die OCID in eine temporäre lokale Textdatei ein. Sie benötigen die OCID für die nächste Aufgabe.
    

## Aufgabe 8: Kompilierte Java-Datei ausführen

1.  Kehren Sie zu Cloud Shell zurück, und führen Sie die folgenden Befehle aus, um zwei Variablen zu definieren: **BUCKET** und **COMPARTMENT**. Ersetzen Sie im folgenden Beispiel `your-compartment-ocid` durch die Compartment-OCID für die Zieldatenbank.
    
        $ <copy>export BUCKET=DataSafeAuditData</copy>
        $ <copy>export COMPARTMENT=your-compartment-ocid</copy>
        
2.  Suchen Sie die Version der Datei `oci-java-sdk-common-httpclient-jersey-version.jar`. Im folgenden Beispiel ist die Version 3.3.0.
    
        $ <copy> ls $OCI_JAVA_SDK_LOCATION/lib/jersey</copy>
        
        oci-java-sdk-common-httpclient-jersey-3.3.0.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-javadoc.jar  oci-java-sdk-common-httpclient-jersey-3.3.0-sources.jar
        
3.  Führen Sie `DataSafeRestAPIClientExample.class` aus, indem Sie den folgenden Befehl ausführen. Ersetzen Sie `version` in `oci-java-sdk-common-httpclient-jersey-version.jar` durch die Version, die Sie im vorherigen Schritt erhalten haben. Sie können den Fehler beim nicht erfolgreichen Laden der Klasse `org.slf4j.impl.StaticLoggerBinder` ignorieren.
    
        $ <copy>java -cp $OCI_JAVA_SDK_FULL_JAR_LOCATION:$OCI_JAVA_SDK_LOCATION/third-party/lib/*:$OCI_JAVA_SDK_LOCATION/third-party/jersey/lib/*:$OCI_JAVA_SDK_LOCATION/lib/jersey/oci-java-sdk-common-httpclient-jersey-version.jar:$HOME/.oci DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-14T22:01:38.325Z
        Querying for auditEvents with timeCollected Start = 2023-02-14T22:01:38.325Z, End = 2023-02-15T22:01:38.324Z
        
        
        Count35
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z  OpcRequestId: fra-1:q_rNFX-2hAnzGEoiurT376...
        
        
        Upload complete at  Wed Feb 15 22:01:40 UTC 2023 of cursor  OpcRequestId: fra-1:YpGeKJmQ7HtfJLXCVGLYKIEGCEPGbsdF...
        
4.  Prüfen Sie die Ausgabe. Die dritte letzte Ausgabezeile gibt die Anzahl der Auditdatensätze an, die in den Objektspeicher kopiert wurden. Ihr Wert kann sich von dem in diesem Beispiel gezeigten unterscheiden.
    

## Aufgabe 9: Prüfen, ob die Auditdaten in den Bucket kopiert werden

1.  Wählen Sie im Navigationsmenü **Speicher**, **Buckets** aus.
    
2.  Vergewissern Sie sich, dass das Compartment ausgewählt ist.
    
3.  Klicken Sie auf den Namen Ihres Buckets.
    
    Die Seite **Bucket-Details** für den Bucket wird angezeigt.
    
4.  Scrollen Sie nach unten zum Abschnitt **Objekte**.
    
5.  Beachten Sie, dass Sie jetzt eine Position namens `auditeventjson` haben, die den Text `noofrecords_<some-number>` enthält. Dies sind die Auditdaten, die aus dem Oracle Data Safe-Repository kopiert wurden. `<some-number>` ist die Anzahl der kopierten Auditdatensätze.
    
        <copy>auditeventjson2023-02-15T22:01:39.579619Z _noofrecords_ 35 Start =2023-02-14T22:01:38.325Z, End=2023-02-15T22:01:38.324Z</copy>
        
6.  Objekt und Cursor löschen: Klicken Sie jeweils auf die drei Punkte am Ende der Zeile, und wählen Sie **Löschen** aus. Klicken Sie im Dialogfeld **Objekt löschen** auf **Löschen**.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Überblick über Aktivitätsauditing](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [Audittrails](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [Erste Schritte (mit SDK für Java)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (auf GitHub)](https://github.com/oracle/oci-java-sdk)
*   [SDK für Java (SDK konfigurieren)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [Data Safe-API (Referenz und Endpunkte)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [Oracle Cloud Infrastructure-Java-SDK (Pakete und Klassen)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbank
*   **Berater** - Richard Evans, Bettina Schaeumer, Archana Rao, Anna Haikl
*   **Zuletzt aktualisiert am/um** - Jody Glover, 11. April 2023