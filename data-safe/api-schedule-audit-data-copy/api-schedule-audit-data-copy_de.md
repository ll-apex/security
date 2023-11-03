# Kopieren von Auditdaten in den Objektspeicher mit der Oracle Data Safe-REST-API planen

## Einführung

Wenn Sie den Audit Trail der Zieldatenbank in Oracle Data Safe starten, beginnt Oracle Data Safe mit dem Kopieren von Auditdatensätzen aus dem Audit Trail der Datenbank in das Oracle Data Safe-Repository. In dieser Übung verwenden Sie die Oracle Data Safe-API (Application Programming Interface) und Crontab auf einer Compute-Instanz, um das Kopieren der Auditdaten von Oracle Data Safe für die Zieldatenbank in den Objektspeicher zu planen.

Wenn Sie bereits einen Bucket in Oracle Cloud Infrastructure haben und den Audittrail für die Zieldatenbank in Oracle Data Safe gestartet haben, können Sie die Aufgaben 1 und 2 überspringen.

Geschätzte Laborzeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Bucket in Ihrem Compartment erstellen
*   Audit Trail für die Zieldatenbank in Oracle Data Safe starten
*   Menge der von Oracle Data Safe erfassten Auditdaten anzeigen
*   SSH-Schlüssel in Cloud Shell erstellen
*   Virtuelles Cloud-Netzwerk (VCN) erstellen
*   Compute-Instanz mit dem Oracle Linux Cloud Developer 8-Image erstellen
*   Verbindung zu Ihrer Compute-Instanz über Cloud Shell herstellen
*   API-Schlüssel erstellen
*   Laden Sie den Private Key (PEM-Datei) in die Compute-Instanz hoch
*   Erstellen Sie eine Konfigurationsdatei
*   Java-Programm kompilieren
*   Compartment-OCID für die Zieldatenbank abrufen
*   Kompilierte Java-Klassendatei ausführen
*   Prüfen Sie, ob sich die Auditdaten im Bucket befinden
*   SH-Skript für cronjob erstellen
*   SH-Skript mit crontab planen
*   Geplante Aktivität in crontab entfernen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

## Aufgabe 1: Bucket in Ihrem Compartment erstellen

Erstellen Sie einen Bucket zum Speichern Ihrer Auditdaten. Sie verwenden den Bucket auch, um eine PEM-Datei in einer späteren Aufgabe an eine Compute-Instanz zu übertragen.

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Speicher**, **Buckets** aus.
    
2.  Wählen Sie Ihr Compartment aus.
    
3.  Klicken Sie auf **Bucket erstellen**.
    
    Das Dialogfeld **Bucket erstellen** wird angezeigt.
    
4.  Geben Sie für den Bucket-Namen **DataSafeAuditData** ein.
    
5.  Behalten Sie die Standardeinstellungen bei, und klicken Sie auf **Erstellen**.
    

## Aufgabe 2: Audittrail für die Zieldatenbank in Oracle Data Safe starten

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Oracle Database**, **Data Safe - Datenbanksicherheit** aus.
    
2.  Klicken Sie links unter **Sicherheitscenter** auf **Aktivitätsauditing**.
    
3.  Klicken Sie links unter **Zugehörige Ressourcen** auf **Audittrails**.
    
4.  Stellen Sie sicher, dass Ihr Compartment aus der Dropdown-Liste **Compartment** auf der linken Seite ausgewählt ist.
    
5.  Klicken Sie rechts auf den Namen der Zieldatenbank für **UNIFIED\_AUDIT\_TRAIL**.
    
    Die Seite **Protokolldetails** wird angezeigt.
    
6.  Klicken Sie auf **Start**.
    
    Das Dialogfeld **Audittrail starten: UNIFIED\_AUDIT\_TRAIL** wird angezeigt.
    
7.  Setzen Sie das Startdatum auf den Anfang des aktuellen Monats.
    
8.  Klicken Sie auf **Start**. Warten Sie, bis sich der **Erfassungsstatus** von **Wird gestartet** in **Wird erfasst** und dann in **IDLE** ändert. Es dauert etwa eine Minute.
    

## Aufgabe 3: Menge der von Oracle Data Safe erfassten Auditdaten anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Aktivitätsauditing**.
    
2.  Klicken Sie unter **Security Center** auf **Auditprofile**.
    
3.  Klicken Sie rechts auf den Namen der Zieldatenbank.
    
    Die Seite **Auditprofildetails** für die Zieldatenbank wird angezeigt.
    
4.  Scrollen Sie auf der Seite nach unten zum Abschnitt **Auditvolumen berechnen**.
    
5.  Klicken Sie auf **Von Data Safe erfasst**.
    
    Das Dialogfeld **Erfasstes Volumen berechnen** wird angezeigt.
    
6.  Setzen Sie die Felder **Startmonat** und **Endmonat** auf den ersten und letzten Tag des aktuellen Monats, und klicken Sie auf **Berechnen**. Notieren Sie sich die Anzahl der von Oracle Data Safe erfassten Auditdatensätze.
    
7.  Wenn die Anzahl der Auditdatensätze aus irgendeinem Grund gleich Null ist, führen Sie das SQL-Skript [**load-data-safe-sample-data\_admin.SQL**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) in Database Actions aus, um Beispieldaten in die Datenbank zu laden. Dieses Skript generiert auditierbare Datenbankaktivitäten für den Benutzer `ADMIN`. Wiederholen Sie dann die Schritte 5 und 6, um die Menge der erfassten Auditdaten anzuzeigen.
    

## Aufgabe 4: SSH-Schlüssel in Cloud Shell erstellen

Erstellen Sie in Cloud Shell ein SSH-Schlüsselpaar, mit dem Sie eine Verbindung zu Ihrer Compute-Instanz herstellen können. Der Standardname in LiveLabs-Workshops lautet `cloudshellkey`.

1.  Klicken Sie in der Symbolleiste in Oracle Cloud Infrastructure auf **Entwicklertools**, und wählen Sie **Cloud Shell** aus. Wenn Sie Cloud Shell zum ersten Mal öffnen, befinden Sie sich in Ihrem Home-Verzeichnis. Beispiel: `/home/jody_glove`.
    
2.  (Optional) Setzen Sie die Cloud Shell-Umgebung zurück. Mit dem folgenden Befehl werden alle Daten im Verzeichnis `$HOME` auf dem Cloud Shell-Rechner gelöscht, und die Dateien `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` und `$HOME/.emacs` werden auf ihre Standardwerte zurückgesetzt. Geben Sie zur Bestätigung in der Eingabeaufforderung **y** ein.
    
        $ <copy>csreset --all</copy>
        
3.  Erstellen Sie ein `.ssh`\-Verzeichnis im Home-Verzeichnis auf dem Cloud Shell-Rechner, wechseln Sie in dieses Verzeichnis, und prüfen Sie dann das Verzeichnis, in dem Sie arbeiten.
    
        $ <copy>mkdir ~/.ssh</copy>
        $ <copy>cd ~/.ssh</copy>
        $ <copy>pwd</copy>
        
        /home/your-user-name/.ssh
        
4.  Generieren Sie im Verzeichnis `.SSH` ein SSH-Schlüsselpaar. Der folgende Befehl generiert zwei Schlüssel: einen Private Key mit dem Namen `cloudshellkey` und einen Public Key mit dem Namen `cloudshellkey.pub`. Verwenden Sie die Benennungskonvention `cloudshellkey`, da es sich um einen LiveLabs-Standard handelt. Wenn Sie zur Eingabe einer Passphrase aufgefordert werden, klicken Sie einfach zweimal auf **Enter**, um keine Passphrase einzugeben.
    
        $ <copy>ssh-keygen -b 2048 -t rsa -f cloudshellkey</copy>
        
5.  Stellen Sie sicher, dass die Private Key- und Public Key-Dateien im Verzeichnis `.ssh` vorhanden sind.
    
        $ <copy>ls</copy>
        
        cloudshellkey cloudshellkey.pub
        
6.  Zeigen Sie den Inhalt des Public Keys an. Später kopieren Sie diese in die Zwischenablage und fügen sie beim Erstellen der Compute-Instanz in das Feld "SSH-Schlüssel" ein.
    
        $ <copy>cat cloudshellkey.pub</copy>
        
7.  Lassen Sie Cloud Shell geöffnet.
    

## Aufgabe 5: Virtuelles Cloud-Netzwerk (VCN) erstellen

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure **Networking**, **Virtuelle Cloud-Netzwerke** aus.
    
2.  Wählen Sie Ihr Compartment aus.
    
3.  Klicken Sie auf **VCN erstellen**.
    
4.  Geben Sie unter **Name** **labVCN** ein.
    
5.  Wählen Sie Ihr Compartment aus.
    
6.  Geben Sie unter **IPv4 CIDR-Blöcke** **10.0.0.0/24** ein.
    
7.  Lassen Sie das Kontrollkästchen **DNS-Hostnamen in diesem VCN verwenden** aktiviert.
    
8.  Geben Sie unter **DNS-Label** **labVCN** ein.
    
9.  Klicken Sie auf **VCN erstellen**.
    
10.  Klicken Sie auf **Subnetz erstellen**.
    
11.  Geben Sie als Namen **lab-public-subnet1** ein.
    
12.  Wählen Sie Ihr Compartment aus.
    
13.  Lassen Sie für den Subnetztyp **Regional** ausgewählt.
    
14.  Geben Sie unter **IPv4 CIDR-Block** **10.0.0/24** ein.
    
15.  Lassen Sie für den Subnetzzugriff die Option **Öffentliches Subnetz** aktiviert.
    
16.  Lassen Sie das Kontrollkästchen **DNS-Hostnamen in diesem SUBNET verwenden** aktiviert.
    
17.  Geben Sie unter **DNS-Label** **subnet1** ein.
    
18.  Klicken Sie auf **Subnetz erstellen**.
    
19.  Klicken Sie links auf **Internetgateways**.
    
20.  Klicken Sie auf **Internetgateway erstellen**. Der Bereich **Internetgateway erstellen** wird angezeigt.
    
21.  Geben Sie als Namen **livelabs-igw** ein.
    
22.  Wählen Sie Ihr Compartment aus.
    
23.  Klicken Sie auf **Internetgateway erstellen**.
    
24.  Klicken Sie links auf **Routentabellen**.
    
25.  Klicken Sie in der Liste der Routentabellen auf **Standardroutentabelle für labVCN**.
    
26.  Klicken Sie auf **Routingregeln hinzufügen**.
    
    Der Bereich **Routingregeln hinzufügen** wird angezeigt.
    
27.  Wählen Sie als Zieltyp **Internetgateway** aus.
    
28.  Geben Sie als Ziel-CIDR-Block **0.0.0.0/0** ein.
    
29.  Wählen Sie für das Ziel-Internetgateway die Option **livelabs-igw** aus.
    
30.  Klicken Sie auf **Routingregeln hinzufügen**.
    

## Aufgabe 6: Compute-Instanz mit dem Oracle Linux Cloud Developer 8-Image erstellen

Das Oracle Linux Cloud Developer Image wird auf allen Compute-Ausprägungen mit Ausnahme der GPU-Ausprägungen unterstützt. Für dieses Image sind mindestens 8 GB Arbeitsspeicher für alle Standard- und flexiblen Ausprägungen erforderlich. Eine Ausnahme ist VM.Standard.E2.1. Micro-Ausprägung, der nur 1 GB Arbeitsspeicher zugewiesen ist. Aufgrund der geringen Speichergröße in VM.Standard.E2.1. Mikroform, einige grafische intensive Programme sind nicht im Bild installiert.

1.  Wählen Sie im Navigationsmenü für Oracle Cloud Infrastructure die Option **Compute**, **Instanzen** aus.
    
2.  Wählen Sie Ihr Compartment aus.
    
3.  Klicken Sie auf **Instanz erstellen**.
    
4.  Geben Sie einen benutzerfreundlichen Namen für die Compute-Instanz ein.
    
5.  Lassen Sie das Compartment ausgewählt.
    
6.  Platzierung unverändert lassen.
    
7.  Klicken Sie im Abschnitt **Image und Ausprägung** auf **Bearbeiten**.
    
8.  Klicken Sie auf **Image ändern**. Lassen Sie **Oracle Linux** ausgewählt. Scrollen Sie nach unten, und wählen Sie **Oracle Linux Cloud Developer 8** aus. Aktivieren Sie das Kontrollkästchen **Ich habe die folgenden Dokumente geprüft und akzeptiere sie: Oracle LInux Cloud Developer Image Terms of Use** (Nutzungsbedingungen für Cloud-Entwicklerbild). Klicken Sie auf **Bild auswählen**.
    
9.  Klicken Sie auf **Ausprägung ändern**. Lassen Sie **Virtuelle Maschine** und die Ausprägungsreihe **Ampere** ausgewählt. Behalten Sie **VM.Standard.A1 bei. Flex**\-Bild ausgewählt. Scrollen Sie nach unten, und geben Sie **8** GB Arbeitsspeicher ein. Klicken Sie auf **Ausprägung auswählen**.
    
10.  Klicken Sie im Abschnitt **Networking** auf **Bearbeiten**, und lassen Sie **Vorhandenes virtuelles Cloud-Netzwerk auswählen** ausgewählt. Wählen Sie unter **Virtuelles Cloud-Netzwerk in Ihrem Compartment** die Option **labVCN** aus. Wählen Sie unter **Subnetz in Ihrem Compartment** die Option **lab-public-subnet1** aus. Lassen Sie für **Öffentliche IPv4-Adresse** die Option **Öffentliche IPv4-Adresse zuweisen** aktiviert.
    
11.  Wählen Sie im Abschnitt **SSH-Schlüssel hinzufügen** die Option **Public Keys einfügen** aus. Kehren Sie zur Cloud Shell zurück, und kopieren Sie den gesamten SSH-Public Key in die Zwischenablage. Er beginnt mit `SSH-rsa` und endet mit etwas ähnlichem wie `jody_glove@1e3ebc618797`. Fügen Sie im Feld "SSH-Schlüssel" den Public Key ein. Stellen Sie sicher, dass es keine harten Renditen gibt. Bei Bedarf können Sie `cat .SSH/cloudshellkey.pub` ausführen, um den Public Key erneut anzuzeigen.
    
12.  Übernehmen Sie im Abschnitt **Boot-Volume** die Standardeinstellungen unverändert.
    
13.  Klicken Sie auf **Erstellen**, und warten Sie zwei Minuten, bis die Compute-Instanz bereitgestellt wird. Die Seite **Arbeitsanforderungen** wird angezeigt, auf der Sie Informationen zu Ihrer Compute-Instanz anzeigen können.
    

## Aufgabe 7: Verbindung zu Ihrer Compute-Instanz über Cloud Shell herstellen

1.  Wenn Sie die Seite der Compute-Instanz verlassen haben, können Sie sie wie folgt erneut finden: Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Optionen **Compute**, **Instanzen** aus. Wählen Sie Ihr Compartment aus. Klicken Sie auf den Namen der Compute-Instanz.
    
2.  Kopieren Sie auf der Registerkarte **Instanzinformationen** unter **Instanzzugriff** die öffentliche IP-Adresse in die Zwischenablage.
    
3.  Geben Sie in Cloud Shell den folgenden `SSH`\-Befehl ein, um eine Verbindung zur Compute-Instanz herzustellen, und ersetzen Sie `public-ip-address` durch den Befehl, den Sie gerade in die Zwischenablage kopiert haben.
    
        $ <copy>ssh -i ~/.ssh/cloudshellkey opc@public-ip-address</copy>
        
    
    Sie erhalten eine Meldung, dass die Authentizität Ihrer Compute-Instanz nicht ermittelt werden kann. Möchten Sie mit der Verbindung fortfahren?
    
4.  Geben Sie **yes** ein, um fortzufahren.
    
    Die öffentliche IP-Adresse Ihrer Compute-Instanz wird der Liste der bekannten Hosts auf Ihrem Cloud Shell-Rechner hinzugefügt. Der Terminal-Prompt wird zu `[opc@<your-compute-name> ~]$`. Dabei ist `opc` Ihr Benutzeraccount auf der Compute-Instanz. Sie sind jetzt mit Ihrer neuen Compute-Instanz verbunden.
    

## Aufgabe 8: API-Schlüssel erstellen

Die Konfiguration des SDK umfasst drei Teile: Erstellen Sie einen API-Schlüssel, erstellen Sie eine Konfigurationsdatei, und laden Sie eine PEM-Datei in die Compute-Instanz hoch. Um das SDK für Java verwenden zu können, muss ein Schlüsselpaar zum Signieren von API-Anforderungen verwendet werden, wobei der Public Key in Oracle hochgeladen wird. Nur der Benutzer, der die API aufruft, darf den Private Key besitzen.

1.  Erstellen Sie zunächst einen API-Schlüssel. Klicken Sie dazu in der oberen rechten Ecke der Oracle Cloud Infrastructure-Konsole auf das Symbol **Profil**, und klicken Sie dann auf Ihren Benutzernamen.
    
2.  Klicken Sie links auf **API-Schlüssel**.
    
3.  Klicken Sie auf **API-Schlüssel hinzufügen**.
    
    Das Dialogfeld **API-Schlüssel hinzufügen** wird angezeigt.
    
4.  Lassen Sie **API-Schlüsselpaar generieren** ausgewählt, klicken Sie auf **Private Key herunterladen**, und speichern Sie den Private Key (PEM-Datei) in einem lokalen Verzeichnis auf Ihrem Rechner.
    
5.  Klicken Sie auf **Add**.
    
    Das Dialogfeld **Vorschau der Konfigurationsdatei** wird angezeigt. In diesem Dialogfeld wird eine Vorschau der Konfigurationsdatei angezeigt.
    
6.  Kopieren Sie den Inhalt der Konfigurationsdatei in eine temporäre Textdatei, da Sie ihn in einer späteren Aufgabe benötigen. Der Inhalt sieht ungefähr so aus:
    
        <copy>[DEFAULT]
        user=ocid1.user.oc1...
        fingerprint=your-fingerprint
        tenancy=ocid1.tenancy.oc1...
        region=eu-frankfurt-1
        key_file=<path to your private keyfile> # TODO</copy>
        
7.  Klicken Sie auf **Schließen**.
    
8.  Wechseln Sie in Cloud Shell zum Benutzer `root`.
    
        $ <copy>sudo su -</copy>
        
9.  Erstellen Sie ein `.oci`\-Verzeichnis.
    
        # <copy>mkdir ~/.oci</copy>
        

## Aufgabe 9: Private Key (PEM-Datei) in die Compute-Instanz hochladen

Laden Sie den Private Key (PEM-Datei) in den Objektspeicher hoch, und kopieren Sie ihn dann in Ihre Compute-Instanz.

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Optionen **Speicher**, **Buckets** aus. Wählen Sie Ihr Compartment aus. Klicken Sie auf den Namen des Buckets.
    
    Die Seite **Bucket-Details** wird angezeigt.
    
2.  Scrollen Sie nach unten zum Abschnitt **Objekte**, und klicken Sie auf **Hochladen**.
    
    Der Fensterbereich **Objekte hochladen** wird angezeigt.
    
3.  Ziehen Sie die Private-Key-Datei in den Bereich **Dateien von Ihrem Computer auswählen**, und klicken Sie auf **Hochladen**.
    
4.  Klicken Sie auf **Schließen**.
    
5.  Klicken Sie am Ende der Zeile für die Auflistung der Private-Key-Datei auf die drei Punkte, und wählen Sie **Vorab authentifizierte Anforderung erstellen** aus.
    
    Der Bereich **Vorab authentifizierte Anforderung erstellen** wird angezeigt.
    
6.  Klicken Sie auf **Vorab authentifizierte Anforderung erstellen**.
    
    Das Dialogfeld **Details der vorab authentifizierten Anforderung** wird angezeigt.
    
7.  Kopieren Sie die **URL der vorab authentifizierten Anforderung** in die Zwischenablage, und fügen Sie sie in eine temporäre lokale Textdatei ein. _Wichtig:_ Sie können diese URL nach dem Schließen des Dialogfelds nicht mehr anzeigen.
    
8.  Klicken Sie auf **Schließen**.
    
9.  Wechseln Sie in Cloud Shell in das Verzeichnis `.oci`.
    
        # <copy>cd ~/.oci</copy>
        
10.  Verwenden Sie den Befehl `WGET`, um die Private-Key-Datei aus dem Objektspeicher in das Verzeichnis `.oci` zu kopieren. Ersetzen Sie `pre-authenticated-request-url` durch Ihre eigene URL.
    
        # <copy>wget pre-authenticated-request-url</copy>
        
11.  Listen Sie den Inhalt des Verzeichnisses auf, um sicherzustellen, dass die Private-Key-Datei vorhanden ist.
    
        # <copy>ls</copy>
        

## Aufgabe 10: Konfigurationsdatei erstellen

In dieser Aufgabe erstellen Sie eine Konfigurationsdatei mit dem Namen `config` im Verzeichnis `.oci` für das SDK und fügen dann den API-Inhalt hinzu, den Sie aus dem API-Schlüssel erhalten haben (den Sie in einer vorherigen Aufgabe erstellt haben). Korrigieren Sie in der Konfigurationsdatei die letzte Zeile, indem Sie den tatsächlichen Pfad zur Private-Key-Datei auf der Compute-Instanz hinzufügen. Die JAVA-Datei, die Sie in einer nachfolgenden Aufgabe kompilieren, sucht nach der Konfigurationsdatei in `~/.oci/config` mit dem Profil `DEFAULT`.

1.  Erstellen Sie mit dem vi-Editor eine Konfigurationsdatei, während Sie sich noch im Verzeichnis `.oci` befinden.
    
        # <copy>vi config</copy>
        
2.  Fügen Sie den Inhalt der Konfigurationsdatei in die Datei `config` ein. Hinweis: Früher haben Sie diesen Inhalt in eine temporäre Textdatei eingefügt. Der Inhalt sieht ähnlich aus wie der folgende Code. Stellen Sie sicher, dass Sie `[DEFAULT]` oben einfügen.
    
         <copy>[DEFAULT]
         user=ocid1.user.oc1...
         fingerprint=your-fingerprint
         tenancy=ocid1.tenancy.oc1...
         region=eu-frankfurt-1
         key_file=<path to your private keyfile> # TODO</copy>
        
3.  Ändern Sie die letzte Zeile als Pfad zu Ihrer PEM-Datei auf Ihrer Compute-Instanz. Ersetzen Sie im folgenden Beispiel `your-private-key-file-name` durch Ihren eigenen Private-Key-Dateinamen.
    
        <copy>key_file=~/.oci/your-private-key-file-name</copy>
        
4.  Speichern und schließen Sie die Datei (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie die **Eingabe**).
    
5.  Listen Sie den Inhalt des aktuellen Verzeichnisses auf, und stellen Sie sicher, dass die Datei `config` vorhanden ist.
    
        # <copy>ls</copy>
        
        config  your-private-key-file-name
        

## Aufgabe 11: Java-Programm kompilieren

Im Oracle Linux Cloud Developer-Image sind das SDK und die Java-Software bereits installiert. Die OCI-JAR-Datei befindet sich in `/usr/lib64/java-OCI-sdk/lib/OCI-java-sdk-full-<version>.jar`, und Librarys von Drittanbietern befinden sich in `/usr/lib64/java-OCI-sdk/third-party/lib`. Um eine Java-Datei zu kompilieren, verwenden Sie den Befehl `javac`.

In dieser Aufgabe kompilieren Sie ein Java-Programm namens `DataSafeRestAPIClientExample.java`, das mit der SDK-Installation geliefert wird. Mit diesem Programm werden Auditdaten aus dem Oracle Data Safe-Repository in einen angegebenen Objektspeicher-Bucket kopiert. Bei Bedarf können Sie das Programm auch direkt von Github herunterladen, indem Sie den folgenden Befehl ausführen: `wget https://raw.githubusercontent.com/oracle/oci-java-SDK/master/bmc-examples/src/main/java/DataSafeRestAPIClientExample.java`.

1.  Prüfen Sie die Version der Datei `oci-java-sdk-full-version.jar`. In diesem Beispiel ist die Version 2.27.0.
    
        # <copy>ls /usr/lib64/java-oci-sdk/lib</copy>
        
        oci-java-sdk-full-2.27.0.jar  oci-java-sdk-full-2.27.0-javadoc.jar  oci-java-sdk-full-2.27.0-sources.jar
        
2.  Wechseln Sie zum Verzeichnis `/usr/lib64/java-oci-sdk`.
    
        # <copy>cd /usr/lib64/java-oci-sdk</copy>
        
3.  Kompilieren Sie die Datei `DataSafeRestAPIClientExample.java`. Stellen Sie sicher, dass Sie die richtige Version im Dateinamen `oci-java-sdk-full-<version>.jar` verwenden. Im folgenden Beispiel wird Version 2.27.0 verwendet.
    
    Es ist sehr häufig, dass ein Java-Programm von einer oder mehreren externen Bibliotheken (JAR-Dateien) abhängt. Verwenden Sie das Flag `-classpath` (oder `-cp`), um dem Compiler mitzuteilen, wo nach externen Librarys gesucht werden soll. Standardmäßig durchsucht der Compiler den Bootstrap-Classpath und die Umgebungsvariable `CLASSPATH`.
    
        # <copy>javac -cp lib/oci-java-sdk-full-2.27.0.jar:third-party/lib/* examples/DataSafeRestAPIClientExample.java</copy>
        
    
    Hinweis: Es gibt keine Ausgabe, nachdem die Datei kompiliert wurde. Sie kehren einfach zur Eingabeaufforderung zurück.
    
4.  Wechseln Sie in das Verzeichnis `examples`, und listen Sie die Dateien auf. Vergewissern Sie sich, dass Sie jetzt über eine Klassendatei mit dem Namen `DataSafeRestAPIClientExample.class` verfügen.
    
        # <copy>cd examples</copy>
        # <copy>ls</copy>
        

## Aufgabe 12: Compartment-OCID für die Zieldatenbank abrufen

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
    

## Aufgabe 13: Kompilierte Java-Klassendatei ausführen

Führen Sie die Datei `DataSafeRestAPIClientExample.class` aus, um zu testen, ob sie ohne Fehler ausgeführt wird, bevor Sie sie planen. Das Programm erfordert zwei Eingaben, die Sie im Voraus definieren können:

*   Der Name des Buckets, in dem die kopierten Auditdaten gespeichert werden sollen
*   Die Compartment-OCID der Zieldatenbank

1.  Führen Sie die folgenden Befehle aus, um zwei Variablen festzulegen: **BUCKET** und **COMPARTMENT**. Ersetzen Sie `compartment-ocid-for-target-database` durch Ihre eigene OCID.
    
        # <copy>export BUCKET=DataSafeAuditData</copy>
        # <copy>export COMPARTMENT=compartment-ocid-for-target-database</copy>
        
2.  Stellen Sie sicher, dass Sie weiterhin im Verzeichnis `/usr/lib64/java-oci-sdk/lib/examples` arbeiten.
    
3.  Führen Sie den folgenden Befehl aus, um die Klassendatei auszuführen. Im folgenden Beispiel wird `oci-java-sdk-full-2.27.0.jar` verwendet. Verwenden Sie jedoch die Version auf Ihrem System. Sie können den Fehler beim nicht erfolgreichen Laden der Klasse `org.slf4j.impl.StaticLoggerBinder` ignorieren.
    
        # <copy>java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
        SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
        SLF4J: Defaulting to no-operation (NOP) logger implementation
        SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
        Getting the namespace
        
        
        Namespace: frmwj0cqbupb
        
        
        Getting content for object cursor  from bucket: DataSafeAuditData
        
        
        ignore
        Finished reading content for object cursor, last upload's last auditEvent record's timecollected FAILED
        
        
        2023-02-15T19:03:33.225Z
        Querying for auditEvents with timeCollected Start = 2023-02-15T19:03:33.225Z, End = 2023-02-16T19:03:33.223Z
        
        
        Count43
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of auditeventjson2023-02-16T19:03:34.290935835Z _noofrecords_ 43 Start =2023-02-15T19:03:33.225Z, End=2023-02-16T19:03:33.223Z  OpcRequestId: fra-1:RMvrVLBJGMQYyKnOATHwJs6Ywthox3dK9BGYWIaZv3LD2lFq5oRaUuZKzWsJkwZf
        
        
        Upload complete at  Thu Feb 16 19:03:34 GMT 2023 of cursor  OpcRequestId: fra-1:v5KE-S9VbuBMsqnof2qx5dkabTsHgbZv50wSAJJLk-TD-b3e4cqwRmIDG6Bdwa1y
        
4.  Prüfen Sie die Ausgabe. Die dritte letzte Ausgabezeile gibt die Anzahl der Auditdatensätze an, die in den Objektspeicher kopiert wurden. Ihr Wert kann unterschiedlich sein. Wenn die Anzahl gleich Null ist, löschen Sie alle Cursor im Bucket, und wiederholen Sie Schritt 3.
    

## Aufgabe 14: Prüfen, ob sich die Auditdaten im Bucket befinden

1.  Wählen Sie im Navigationsmenü in Oracle Cloud Infrastructure die Option **Speicher**, **Buckets** aus.
    
2.  Wählen Sie Ihr Compartment aus.
    
3.  Klicken Sie auf den Namen Ihres Buckets.
    
    Die Seite **Bucket-Details** für den Bucket wird angezeigt.
    
4.  Scrollen Sie nach unten zum Abschnitt **Objekte**.
    
5.  Beachten Sie, dass Sie jetzt eine Position namens `auditeventjson` haben, die den Text `noofrecords_<some-number>` enthält. Dies sind die Auditdaten, die aus dem Oracle Data Safe-Repository kopiert wurden. `<some-number>` ist die Anzahl der kopierten Auditdatensätze.
    
    ![Objekt für erste Auditdatensätze](images/initial-audit-records-object.png "Objekt für erste Auditdatensätze")
    

## Aufgabe 15: SH-Skript für cronjob erstellen

Nachdem Sie geprüft haben, ob das kompilierte Java-Programm ordnungsgemäß funktioniert, können Sie es mit Cronjob auf Ihrer Compute-Instanz planen. Der Cron-Daemon ist ein integriertes Linux-Dienstprogramm, das Prozesse auf Ihrem System zu einem geplanten Zeitpunkt ausführt. Cron liest die crontab (Cron-Tabellen) für vordefinierte Befehle und Skripte. Sie müssen der Benutzer `root` oder ein Benutzer mit `sudo`\-Berechtigungen sein, um einen Cron-Job zu erstellen.

In dieser Aufgabe erstellen Sie ein SH-Skript, das die Variablen und den Java-Befehl zum Ausführen des Java-Programms enthält. In der nächsten Aufgabe planen Sie das SH-Skript.

1.  Wechseln Sie in das Verzeichnis `/usr/local/bin`.
    
        # <copy>cd /usr/local/bin</copy>
        
2.  Erstellen Sie mit dem vi-Editor eine SH-Datei mit dem Namen `datasafejob.SH`.
    
        # <copy>vi datasafejob.sh</copy>
        
3.  Fügen Sie der SH-Datei folgenden Inhalt hinzu. Beachten Sie, dass Sie die Klassendatei mit einem etwas anderen Java-Befehl ausführen als in Aufgabe 13. Um die Klassendatei von überall auszuführen, müssen Sie den Pfad zum Verzeichnis `examples` in den Klassenpfad aufnehmen. Auch hier verwenden wir `oci-java-sdk-full-2.27.0.jar`. Stellen Sie sicher, dass Sie die richtige Version auf Ihrem System verwenden. Ersetzen Sie `compartment-ocid-for-target-database` durch Ihre eigene Compartment-OCID.
    
        <copy>#!/bin/bash
        
        export BUCKET=DataSafeAuditData
        
        export COMPARTMENT=compartment-ocid-for-target-database
        
        java -cp /usr/lib64/java-oci-sdk/lib/oci-java-sdk-full-2.27.0.jar:/usr/lib64/java-oci-sdk/third-party/lib/*:/usr/lib64/java-oci-sdk/examples DataSafeRestAPIClientExample $BUCKET $COMPARTMENT</copy>
        
4.  Speichern und schließen Sie die Datei (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie die **Eingabe**).
    
5.  Berechtigungen für das Skript hinzufügen.
    
        # <copy>chmod 777 datasafejob.sh</copy>
        

## Aufgabe 16: SH-Skript mit crontab planen

Planen Sie zunächst, dass das SH-Skript jede Minute ausgeführt wird, damit Sie testen können, ob die Planung funktioniert. Nach der Bestätigung ändern Sie den Zeitplan so, dass er jeden Tag um 2 Uhr morgens ist.

1.  Um den cron-Job zu bearbeiten, geben Sie den folgenden Befehl ein:
    
        # <copy>crontab -e</copy>
        
2.  Fügen Sie der ersten Zeile Folgendes hinzu, und speichern Sie (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie die **Eingabetaste**):
    
        <copy>* * * * * /usr/local/bin/datasafejob.sh</copy>
        
3.  Generieren Sie einige Aktivitäten für das Audit von Oracle Data Safe. Rufen Sie dazu Database Actions für die Zieldatenbank auf. Laden Sie das Skript [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) herunter, und öffnen Sie es in einem Texteditor wie NotePad. Kopieren Sie das gesamte Skript in die Zwischenablage, und fügen Sie es in Database Actions in das Arbeitsblatt ein. Klicken Sie in der Symbolleiste auf die Schaltfläche **Skript ausführen**, und warten Sie, bis die Ausführung des Skripts abgeschlossen ist.
    
4.  Kehren Sie zum Bucket zurück, und zeigen Sie die Auditdaten an, die jede Minute erfasst werden. Es kann bis zu zehn Minuten dauern, bis die Auditdatenobjekte angezeigt werden.
    
    ![Auditdatensatzobjekte](images/audit-records-objects.png "Auditdatensatzobjekte")
    
5.  Greifen Sie in Cloud Shell auf crontab zu.
    
        # <copy>crontab -e</copy>
        
6.  Ändern Sie den Zeitplan täglich auf 2 Uhr, und speichern Sie die Datei (drücken Sie **Escape**, geben Sie **:wq** ein, und drücken Sie die **Eingabetaste**).
    
    Im folgenden Beispiel gibt `0 2 * * *` an, dass der Cron-Job jedes Mal ausgeführt wird, wenn die Systemuhr 2 Uhr zeigt.
    
        <copy>0 2 * * * /usr/local/bin/datasafejob.sh</copy>
        

## Aufgabe 17: Geplante Aktivität in crontab entfernen

1.  Greifen Sie auf crontab zu.
    
        # <copy>crontab -e</copy>
        
2.  Löschen Sie den Inhalt.
    
3.  Speichern Sie die Änderungen (drücken Sie **ESC**, geben Sie **:wq** ein, und drücken Sie die **EINGABETASTE**).
    
4.  Schließen Sie Cloud Shell.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Überblick über Aktivitätsauditing](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7)
*   [Audittrails](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-8E684604-879A-4312-8FF6-519ECD67D179)
*   [Oracle Linux Cloud Developer-Image](https://docs.oracle.com/en-us/iaas/oracle-linux/developer/index.htm)
*   [Erste Schritte (mit SDK für Java)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdkgettingstarted.htm)
*   [oci-java-sdk (auf GitHub)](https://github.com/oracle/oci-java-sdk)
*   [SDK für Java (SDK konfigurieren)](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/javasdk.htm)
*   [Data Safe-API (Referenz und Endpunkte)](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/)
*   [Oracle Cloud Infrastructure-Java-SDK (Pakete und Klassen)](https://docs.oracle.com/en-us/iaas/tools/java/3.2.2/)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Mitwirkende** - Richard Evans, Anna Haikl, Russ Lowenthal, Archana Rao, Bettina Schaeumer
*   **Zuletzt aktualisiert am/um** - Jody Glover, 11. April 2023