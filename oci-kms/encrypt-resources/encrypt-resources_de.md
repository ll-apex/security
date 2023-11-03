# OCI-Ressourcenverschlüsselung mit einem extern verwalteten Schlüssel konfigurieren

## Einführung

In dieser Übung werden Sie durch die Erstellung verschiedener OCI-Ressourcen und die Konfiguration ihrer Verschlüsselung mit dem Schlüssel geführt, den Sie extern in OCI in Thales CipherTrust Manager erstellt haben.

Sie sind Ihr Unternehmensdatenmanager. Sie erstellen einen Speicherplatz sowie eine Autonomous Database. Sie können diese Ressourcen mit dem Schlüssel verschlüsseln, den Sie in der vorherigen Übung in Thales CipherTrust Manager erstellt haben. Sie laden Daten in diese beiden Ressourcen hoch und greifen darauf zu, um sicherzustellen, dass die Verschlüsselungs-/Entschlüsselungsvorgänge korrekt funktionieren.

Geschätzte Zeit: 15 Minuten

[Gehen Sie durch das Labor](videohub:1_4nvcdn5d)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Stellen Sie eine Verbindung zu Ihrem OCI-Mandanten her, und erstellen Sie einen verschlüsselten Speicher-Bucket und Autonomous Database.
*   Testen Sie den Zugriff auf die verschlüsselten Daten, und bestätigen Sie, dass die richtigen Benutzer korrekt auf Klartextdaten im Speicher-Bucket und in Autonomous Database zugreifen können.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Alle vorherigen Übungen erfolgreich abgeschlossen

## Aufgabe 1: Bucket mit eigenen Verschlüsselungsschlüsseln erstellen

1.  Melden Sie sich von OCI ab, falls noch nicht geschehen.

![Abmelden](./images/oci-log-out.png "Abmelden")

2.  Melden Sie sich bei Ihrem OCI-Mandanten mit dem Benutzer **Data\_Manager\_XXX** an, bei dem XXX Ihre Anmeldung für Studierende ist. Informationen hierzu finden Sie bei Bedarf in der Übung "Erste Schritte". Beispiel: Wenn Sie Studentennummer 007 sind:

![Datenmanager-Anmeldung](./images/data-manager-login.png "Datenmanager-Anmeldung")

3.  Erstellen wir zunächst einen Bucket in OCI Object Storage. Melden Sie sich dazu bei der OCI-Konsole an, und navigieren Sie durch das Haupt-Hamburger-Menü zu _"Storage > Object Storage & Archive Storage > Buckets"_.

![Speicherbereiche](./images/buckets.png "Speicherbereiche")

2.  Erstellen Sie einen Bucket im Compartment "ocw23-OCI-Vault-HOL", indem Sie das Compartment links in der Dropdown-Liste auswählen und dann auf die Schaltfläche **Bucket erstellen** klicken.

![Bucket erstellen](./images/create-bucket.png "Bucket erstellen")

3.  Benennen Sie sie wie folgt:
    
    *   **"ocw23-OCI-bucket-XXX"**, wobei "XXX" Ihre Studierendennummer ist.
    
    Wählen Sie die Option **Mit vom Kunden verwalteten Schlüsseln verschlüsseln** aus. Wenn Sie diese Option auswählen, werden neue Felder angezeigt, die ausgefüllt werden sollen.
    
    *   Wählen Sie den Vault aus, den Sie in Übung 1 mit dem Namen "ocw23-OCI-Vault-XXX" erstellt haben. Dabei ist "XXX" Ihre Studierendennummer.
    *   Wählen Sie den Verschlüsselungsschlüssel, den Sie in Thales CTM erstellt haben, mit dem Namen "ocw23-AES-256-XXX", wobei "XXX" Ihre Studentennummer ist.
    
    Klicken Sie dann auf **Erstellen**. ![Bucket-Informationen](./images/bucket-info.png "Bucket-Informationen")
    
4.  Herzlichen Glückwunsch, Sie haben jetzt einen neuen Speicher-Bucket in OCI erstellt, in dem alle von Ihnen hochgeladenen Daten jederzeit automatisch mit dem Verschlüsselungsschlüssel verschlüsselt werden, den Sie auf Thales CTM erstellt haben:
    

![Neues Bucket](./images/new-bucket.png "Neues Bucket")

## Aufgabe 2: Daten in den Bucket hochladen und Sichtbarkeit prüfen

Sie laden eine Datei hoch, die Ihnen in den Bucket, den Sie kürzlich in Object Storage erstellt haben, bereitgestellt wird.

1.  Datei nicht laden [ocw23-sample-file.csv](https://objectstorage.eu-frankfurt-1.oraclecloud.com/p/4B-I7ermCb2D5OfuLn9XyvSCaEI9knzz35jIcQEnkinsuFOfg94HtJnuimhWjISj/n/frnj6sfkc1ep/b/ocw23-resources/o/ocw23-sample-file.csv)
    
2.  Klicken Sie in den zuletzt erstellten Bucket, und klicken Sie im Abschnitt **Objekte** auf **Hochladen**:
    

![Objekt hochladen](./images/upload-object.png "Objekt hochladen")

3.  Lassen Sie den Objektpräfixnamen leer, und wählen Sie die heruntergeladene Datei aus. Klicken Sie auf **Hochladen**:
    
    ![Datei hochladen](./images/upload-file.png "Datei hochladen")
    
4.  Schließen Sie das Fenster, und jetzt können Sie die Datei unter Abschnitt **Objekte** in Ihren Bucket hochgeladen sehen:
    

![Objekt](./images/object.png "Objekt")

5.  Jetzt erstellen Sie eine vorab authentifizierte Anforderung, um in einer bestimmten Zeit auf die Datei zugreifen zu können. Sie können diese URL auch mit anderen teilen. Klicken Sie dazu im Menü "Ressourcen" auf der linken Seite auf **Vorab authentifizierte Anforderungen**:

![Vorab authentifizierte Anforderungen](./images/par.png "Vorab authentifizierte Anforderungen")

6.  Klicken Sie auf **Vorab authentifizierte Anforderung erstellen**:

![Vorab authentifizierte Anforderung erstellen](./images/create-par.png "Vorab authentifizierte Anforderung erstellen")

7.  Füllen Sie die folgenden Parameter aus:
    
    *   Name: Geben Sie "ocw23-sample-file" ein.
    *   Ziel für vorab authentifizierte Anforderung: Klicken Sie auf die Kachel "Objekt"
    *   Objektname: Geben Sie "ocw23-sample-file.csv" ein
    *   Zugriffstyp: Wählen Sie "Objektlesezugriffe zulassen"
    *   Ablauf: Übernehmen Sie den Standardwert, oder wählen Sie eine beliebige Zeit aus.
    
    Klicken Sie dann auf **Vorab authentifizierte Anforderung erstellen**: ![Informationen zur vorab authentifizierten Anforderung hinzufügen](./images/par-info.png "Informationen zur vorab authentifizierten Anforderung hinzufügen")
    
8.  In einem Fenster wird die URL der vorab authentifizierten Anforderung angezeigt. Kopieren Sie diese URL, und speichern Sie sie lokal. Sie benötigen sie später in Übung 4. Klicken Sie auf **Schließen**.
    

![URL der vorab authentifizierten Anforderung](./images/par-created.png "URL der vorab authentifizierten Anforderung")

9.  Um zu prüfen, ob die Datei in Ihrem Bucket sichtbar ist, öffnen Sie einen anderen Browser, und gehen Sie zu der URL, die Sie vorherrschend kopiert haben. Die Datei sollte automatisch heruntergeladen werden und Sie können sie mit Microsoft Excel öffnen und sich den Inhalt ansehen: sie ist entschlüsselt. Ein guter Job!

## Aufgabe 3: Autonomous Database mit Ihren eigenen Verschlüsselungsschlüsseln erstellen

Erstellen wir nun die Autonomous Database und konfigurieren Sie sie mit dem Schlüssel, den Sie in Thales CTM erstellt haben.

Um die vom Kunden verwaltete Verschlüsselung für Autonomous Database zu verwenden, müssen Berechtigungen erstellt werden, damit Autonomous Database mit dem OCI Vault-Service kommunizieren kann. Für diese Übung haben wir eine dynamische Gruppe und die zugehörige Policy vorkonfiguriert. Um alle Schritte zu verstehen, können Sie sich die Konfiguration unten ansehen. Wenn Sie keine Zeit haben, können Sie direkt zu Schritt 1 dieser Aufgabe springen.

Wir haben eine dynamische Gruppe erstellt, die automatisch alle Ressourcen dieses Compartments enthält. Um die Konfiguration anzuzeigen, klicken Sie in der Oracle Cloud Infrastructure-Konsole auf _"Identität und Sicherheit"_, und klicken Sie unter _"Identität"_ auf _"Dynamische Gruppen"_.

![Dynamische Gruppen](./images/dynamic-groups.png "Dynamische Gruppen")

Suchen Sie nach einer Gruppe namens "ocw23ToVault", und klicken Sie auf den Namen, um alle Details anzuzeigen.

![Liste "Dynamische Gruppen"](./images/dynamic-groups-list.png "Liste der dynamischen Gruppen")

Im Detailbereich wird die erstellte Regel angezeigt:

    resource.compartment.id = 'your_Compartment_OCID'
    

wobei <your\_Compartment\_OCID> die OCID des Compartments ocw23-OCI-Vault-HOL ist. Das bedeutet, dass jede neue Ressource, die im Compartment "ocw23-OCI-Vault-HOL" erstellt wird, automatisch zu dieser Gruppe gehört. Dies ist eine Best Practice zur Vereinfachung, da alle neu erstellten Autonomous Database Teil davon sind und von der zugehörigen Policy profitieren. Wenn Sie die dynamische Gruppe erstellen, ist die OCID für die neue Datenbank noch nicht verfügbar.

![Details dynamische Gruppe](./images/dynamic-group-details.png "Details dynamische Gruppe")

Dann mussten wir eine Policy-Anweisung für die dynamische Gruppe schreiben, um den Zugriff auf OCI Vault-Ressourcen zu ermöglichen: Vaults und Schlüssel. Um die für diese Übung geschriebene Policy zu prüfen, klicken Sie in der OCI-Konsole auf _"Identität und Sicherheit"_, und klicken Sie unter _"Identität"_ auf _"Policys"_:

![Sicherheits-Policys](./images/policies.png "Sicherheits-Policys")

Suchen Sie nach einer Policy namens "ocw23-Dynamic-Access-to-Vault-Policy", und klicken Sie auf den Namen, um alle Details anzuzeigen.

![Liste der Sicherheits-Policys](./images/policies-list.png "Liste der Sicherheits-Policys")

Im Detailbereich wird angezeigt, dass eine Policy wie folgt geschrieben wurde:

    Allow dynamic-group ocw23ToVault to use vaults in compartment ocw23-OCI-Vault-HOL
    Allow dynamic-group ocw23ToVault to use keys in compartment ocw23-OCI-Vault-HOL
    

![Policy erstellen](./images/create-policy.png "Policy erstellen")

Diese Policy-Konfiguration ist sehr wichtig. Standardmäßig können keine Services auf Vaults und Schlüssel zugreifen. Es liegt in Ihrer Verantwortung als CISO-/Sicherheitsadministrator von OCI, zu entscheiden, wer/was auf welche Vaults und welche Verschlüsselungsschlüssel zugreifen kann. Auf diese Weise können Sie eine sehr erweiterte und granulare Verschlüsselungsarchitektur in OCI definieren und gleichzeitig Ihre vorhandenen KMS/HSM-Assets On-Premise nutzen, da der Schlüssel, den wir in dieser Übung verwenden, von Ihnen in Ihrem vorhandenen Thales CTM-Mandanten Ihres Unternehmens außerhalb von OCI erstellt wurde.

1.  Jetzt können Sie Autonomous Database erstellen. Navigieren Sie durch das Hamburger-Hauptmenü zu _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Klicken Sie auf "Create Autonomous Database":
    
    ![Autonomous Database erstellen](./images/create-autonomous-database.png "Autonomous Database erstellen")
    
3.  Füllen Sie die Parameter wie folgt aus:
    
    *   Compartment: ocw23-OCI-Vault-HOL
    *   Anzeigename: ocw23-OCI-adb-XXX, wobei XXX Ihre Studierendennummer ist.
    *   Datenbankname: ocw23OCIadbXXX, wobei XXX Ihre Studierendennummer ist.
    *   Workload-Typ: Transaktionsverarbeitung
    *   Bereitstellungstyp: Serverlos
    *   Datenbank konfigurieren: <Als Standard beibehalten>
    *   Administratorzugangsdaten: <Ihr ADMIN-Kennwort>
    *   Netzwerkzugriff: Sicherer Zugriff von überall
    *   Lizenztyp: BYOL (Bring Your Own License)
    *   Oracle Database Edition: Oracle Database Standard Edition (SE)
    
    Klicken Sie auf den Link _"Erweiterte Optionen anzeigen"_. Ein neuer Abschnitt für den Verschlüsselungsschlüssel wird angezeigt. Wählen Sie die Option _"Mit einem vom Kunden verwalteten Schlüssel in diesem Mandanten verschlüsseln"_ aus, und geben Sie den zuvor erstellten Vault- und Masterverschlüsselungsschlüssel ein.
    
    ![Verschlüsselung in Autonomous Database](./images/adb-encryption.png "Verschlüsselung in Autonomous Database")
    
4.  Klicken Sie auf **Autonomous Database erstellen**. Warten Sie dann, bis der Datenbankstatus auf Grün und AKTIV gesetzt ist.
    

![Autonomous Database aktiv](./images/adb-created.png "Autonomous Database aktiv")

## Aufgabe 4: Daten in Autonomous Database hochladen und Sichtbarkeit prüfen

In dieser Aufgabe laden Sie die vorherige CSV-Datei, die Sie in den Bucket geladen haben, in die zuvor erstellte Autonomous Database.

1.  Navigieren Sie zur Seite "Autonomous Database" in der OCI-Konsole, gehen Sie zum Database Actions-Launchpad, und klicken Sie im angezeigten Menü auf "SQL":

![Datenbankaktionen](./images/db-actions.png "Datenbankaktionen")

2.  Sie werden aufgefordert, die Zugangsdaten für den ADMIN-Benutzer und das Kennwort einzugeben, die Sie während der Erstellung von Autonomous Database angegeben haben:

![Admin-Anmeldung](./images/admin-login.png "Admin-Anmeldung")

3.  Die Web SQL Development-UI ist geöffnet. Jetzt können Sie Daten in die Datenbank laden, indem Sie oben rechts auf **Dataload** klicken:

![Klicken Sie auf "Dataload".](./images/data-load.png "Klicken Sie auf "Dataload".")

4.  Ziehen Sie die zuvor heruntergeladene Datei "ocw23-sample-file.csv" per Drag-and-Drop in das entsprechende Fenster, und klicken Sie auf die Schaltfläche **Run all** (Alle ausführen). Diese wird als grünes Symbol in der oberen linken Ecke angezeigt:

![Datei wird geladen](./images/drag-and-drop.png "Datei wird geladen")

5.  Nach Abschluss des Uploads wird der Status **Hochgeladen** neben der Datei angezeigt. Klicken Sie auf **Schließen**:
    
    ![Datei geladen](./images/upload-done.png "Datei geladen")
    
6.  Aktualisieren Sie den Browser, und die neu erstellte Tabelle wird angezeigt:
    

![Tabelle erstellt](./images/new-table.png "Tabelle erstellt")

7.  Um die Daten in der Tabelle anzuzeigen, klicken Sie mit der rechten Maustaste auf die Tabelle, und klicken Sie auf **Öffnen**:

![Tabelle öffnen](./images/see-data.png "Tabelle öffnen")

8.  Klicken Sie im neuen Fenster auf die Registerkarte Daten:

![Daten ansehen](./images/data.png "Daten ansehen")

Herzlichen Glückwunsch, Sie haben Ihre Datei in Object Storage und Ihre Daten in Autonomous Database verschlüsselt. Jetzt sehen Sie in den nächsten Labors die Simulation eines Notfalls aufgrund einer potenziellen Sicherheitsverletzung. Wir werden gemeinsam sehen, wie Sie Ihre Daten in einem solchen Szenario schützen können!

## Weitere Informationen

*   [OCI-Autonomous Database](https://www.oracle.com/autonomous-database/)
*   [Verschlüsselungsschlüssel in Autonomous Database verwalten](https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/autonomous-database-shared/doc/autonomous-encrypt-set-rotate-keys.html)
*   [Überblick über OCI Object Storage](https://docs.oracle.com/en-us/iaas/Content/Object/Concepts/objectstorageoverview.htm)
*   [OCI Object Storage-Bucket-Verschlüsselung verwenden](https://blogs.oracle.com/cloud-infrastructure/post/using-oci-object-storage-bucket-encryption)

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Damien Rilliard, Juli 2023