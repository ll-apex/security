# Linkerstellung zwischen OCI und der Erstellung Ihres HSM- und Masterverschlüsselungsschlüssels

## Einführung

In dieser Übung konfigurieren Sie eine Verbindung zwischen dem Thales CipherTrust Manager und Ihrem Oracle Cloud Infrastructure-(OCI-)Cloud-Mandanten.

Geschätzte Zeit: 10 Minuten

[Walkthough the Lab](videohub:1_4f41vj1f)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Stellen Sie eine Verbindung zu Ihrem OCI-Mandanten her, und erstellen Sie Ihren eigenen Vault in OCI Vault
*   Richten Sie einen Link zwischen Thales CipherTrust Manager und OCI Vault ein

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben Ihre Anmeldung und Ihr Kennwort für beide Umgebungen ordnungsgemäß erhalten

## Aufgabe 1: Verbindung zu OCI herstellen und eigenen Vault in OCI Vault erstellen

Sie benötigen die folgenden Parameter, um die OCI-Verbindung für die Integration mit CTM zu konfigurieren.

*   **Mandanten-OCID:** OCID des Mandanten.
*   **Benutzer-OCID:** OCID des Benutzers.
*   **Region:** Eine Oracle Cloud Infrastructure-Region.
*   **Fingerprint:** Fingerprint des Public Key, der diesem Benutzer hinzugefügt wurde.
*   **Schlüsseldatei:** Private-Key-Datei für die OCI-Verbindung im PEM-Format. Laden Sie die Schlüsseldatei hoch, oder fügen Sie den Dateiinhalt ein.

Sie müssen einen Vault erstellen, um Ihre Schlüssel und Secrets zu speichern. Es gibt zwei Arten von Vaults: Standard und Virtual Private.

*   Standard-Vaults verwenden eine HSM-Partition gemeinsam.
*   Virtual Private Vault verwenden eine isolierte Partition in einem HSM. Jeder Vault verfügt über einen Verwaltungsendpunkt und einen Kryptografieendpunkt. Führen Sie die nächsten Schritte aus, um einen Vault zu erstellen.

1.  Melden Sie sich bei Ihrem OCI-Account an, indem Sie die Schritte in Abschnitt _"Erste Schritte"_ ausführen.
    
    > **Wichtig:** Beachten Sie, dass Sie sich als lokaler OCI-Benutzer und nicht als föderierter Benutzer anmelden müssen. Bitte gehen Sie zum genannten Abschnitt für weitere Details.
    
2.  Navigieren Sie durch das Hamburger-Hauptmenü zu _"Identität und Sicherheit > Vault"_
    
    ![Gehe zu Vault](images/vault-menu.png "Gehe zu Vault")
    
3.  Wählen Sie das Fach im linken Menü. Klicken Sie auf das Anzeigemenü, und wählen Sie das bereits erstellte Sub-Compartment "ocw23-OCI-Vault-HOL" aus.
    
    ![Compartment auswählen](images/select-compartment.png "Compartment auswählen")
    
    Dann klicken Sie auf "Vault erstellen".
    
    ![Vault erstellen](images/select-compartment-create-vault.png "Vault erstellen")
    
4.  Geben Sie einen Namen für den Vault ein. Bitte folgen Sie der Benennungskonvention: ocw23-OCI-Vault-XXX, wobei XXX Ihre Nummer Student ist. Klicken Sie NICHT auf "Make it a virtual private vault". Klicken Sie auf die Schaltfläche "Vault erstellen", um diesen Schritt abzuschließen.
    
    ![Namen für Vault eingeben](images/create-name-vault.png "Namen für Vault eingeben")
    
5.  Jetzt wird Ihr Vault erstellt. Nach der Erstellung wird der Status in der OCI-Konsole als "Grün" und "Aktiv" angezeigt:
    
    ![Vault erfolgreich erstellt](images/vault-created.png "Vault erfolgreich erstellt")
    
6.  Um die Verbindung zwischen dem soeben erstellten Vault und Thales CipherTrust Manager (CTM) zu konfigurieren, müssen Sie einen API-Schlüssel (ein RSA-Schlüsselpaar) für den Benutzer hinzufügen. CTM verwendet den Private Key, um eine Verbindung zu OCI herzustellen und seine APIs aufzurufen. Klicken Sie dazu in der OCI-Konsole auf das obere rechte Benutzerprofilsymbol, und wählen Sie **Benutzereinstellungen** aus.
    
    ![Benutzereinstellungen](images/user-settings.png)
    
7.  Navigieren Sie im linken Menü zu Ressourcen und API-Schlüsseln. Klicken Sie auf _"API-Schlüssel hinzufügen"_.
    
    ![API-Schlüssel hinzufügen](images/add-apikey.png "Benutzereinstellungen")
    
8.  In einem Fenster werden Sie gefragt, wie Sie diese API-Schlüssel erstellen möchten. Sie können in diesem Schritt das API-Schlüsselpaar "direclty" generieren oder auch zuvor erstellte Schlüssel importieren. In diesem Fall generieren wir das API-Schlüsselpaar in diesem Schritt und laden den Private Key herunter. Wählen Sie _"API-Schlüsselpaar generieren"_ und _"Private Key herunterladen"_ aus. Speichern Sie den Private Key in einem lokalen Verzeichnis, wie Sie ihn später benötigen. Klicken Sie auf "Hinzufügen".
    
    ![API-Schlüssel generieren](images/generate-apikey.png "API-Schlüssel generieren")
    
9.  Nachdem Sie auf _"Hinzufügen"_ geklickt haben, wird die Vorschau der Konfigurationsdatei wie folgt angezeigt:
    
    ![Konfigurationsdatei](images/configuration-file.png "Konfigurationsdatei")
    

Kopieren Sie alle Informationen auf Notepad, um eine Verbindung zwischen Oracle und CTM herzustellen.

## Aufgabe 2: CipherTrust Manager-Verbindung zu Oracle konfigurieren

In dieser Aufgabe erstellen Sie eine Verbindung von Ihrem CipherTrust Manager-(CTM-)Mandanten in Thales Cloud außerhalb von Oracle Cloud Infrastructure zu dem Vault, den Sie gerade in OCI erstellt haben.

1.  Um auf CipherTrust Manager as a Service zuzugreifen, müssen Sie die URL für den Zugriff auf Ihren eigenen privaten Mandanten erstellen. Dazu müssen Sie die folgende URL kopieren und in die Adressleiste Ihres Browsers einfügen: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM"** und **XXX** durch Ihre Kursteilnehmernummer ersetzen. Beispiel: Wenn Ihre Studierendennummer 001 lautet, lautet die vollständige URL zu Ihrem eigenen privaten CTM-Mandanten: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001"**.
    
    ![URL-Erstellung in der Adressleiste](images/ctm-address-bar.png "URL-Erstellung in der Adressleiste")
    
    Nachdem Sie Ihr Anmeldefenster geöffnet haben, melden Sie sich mit Ihrem Benutzer **"Secops\_XXX"** mit dem Kennwort an, das Ihnen zur Verfügung gestellt wurde. Wenn Sie diese Informationen nicht finden können, wenden Sie sich bitte an einen der Trainer, um Ihnen zu helfen.
    
    ![Melden Sie sich bei CipherTrust Manager an.](images/ctm-login.png "Melden Sie sich bei CipherTrust Manager an.")
    
2.  Sie sind jetzt bei der CipherTrust Manager-Webkonsole angemeldet.
    
    ![CipherTrust Manager-Webkonsole](images/ctm-page.png "CipherTrust Manager-Webkonsole")
    
3.  Blenden Sie im linken Bereich "Access Management" ein, und klicken Sie auf "Connections".
    
    ![Verbindung](images/create-connection.png "Verbindung")
    
4.  Klicken Sie auf die Schaltfläche "+ Verbindung hinzufügen", um den Assistenten "Verbindung hinzufügen" zu öffnen. Der Assistent besteht aus mehreren Schritten.
    
    ![Verbindung hinzufügen](images/add-connection.png "Verbindung hinzufügen")
    
5.  Verbindungstyp auswählen: Wählen Sie "Cloud" : "Oracle Cloud Infrastructure", und klicken Sie auf **Weiter**.
    
    ![Verbindungstyp](images/connection-type.png "Verbindungstyp")
    
6.  Allgemeine Informationen: Geben Sie einen Namen im folgenden Format an: _"OCI\_Connection\_XXX"_, wobei XXX Ihre Studierendennummer ist. Fügen Sie eine Beschreibung (optional) für die neue Verbindung hinzu, und klicken Sie auf **Weiter**.
    
    ![Verbindungsname](images/name-connection.png "Verbindungsname")
    
7.  Sie befinden sich jetzt im Schritt **Verbindung konfigurieren**. Sie müssen die Informationen, die Sie beim Erstellen des API-Schlüsselpaars gespeichert haben, in Aufgabe 1.9 dieser Übung eingeben. **SORGFÄLTIG**, die Informationen werden nicht in derselben Reihenfolge aufgeführt. Beispiel: "Benutzer-OCID" wird zuerst angegeben, und "Mandanten-OCID" wird an dritter Stelle angegeben, wobei hier zuerst "Mandanten-OCID" gefragt wird. Sie müssen also sicherstellen, dass Sie die relevanten Informationen einfügen. Sie müssen auch die Private-Key-Datei hochladen, die Sie im selben Schritt gespeichert haben.
    
    Kopieren Sie die folgenden Parameter, die Sie in der Konfigurationsdatei hatten:
    
    ![Konfigurationsdatei](images/configuration-file-red-squares.png "Konfigurationsdatei")
    
    *   Mandanten-OCID: OCID des Mandanten.
    *   Benutzer-OCID: OCID des Benutzers.
    *   Region: Eine Oracle Cloud Infrastructure-Region.
    *   Fingerprint: Fingerprint des Public Key, der diesem Benutzer hinzugefügt wurde.
    *   Schlüsseldatei: Private-Key-Datei für die OCI-Verbindung im PEM-Format. Laden Sie die Schlüsseldatei hoch, oder fügen Sie den Dateiinhalt ein.
    *   Dateiupload: Wählen Sie einen Private Key aus, und klicken Sie auf "Upload Private Key", um die Schlüsseldatei von Ihrem Rechner hochzuladen.
    *   Text: Wählen Sie den Zertifikatsinhalt aus, und fügen Sie ihn in das Textfeld ein.
    *   Passphrase: Lassen Sie es leer, wenn Sie keine Passphrase für die verschlüsselte Schlüsseldatei festgelegt haben.
    
    ![Verbindungsdetails](images/connection-details.png "Verbindungsdetails")
    
    Klicken Sie auf Verbindung testen. Die Meldung Status: OK wird angezeigt:
    
    ![Erfolgreiche Verbindung](images/successful-connection.png "Erfolgreiche Verbindung")
    
    Klicken Sie dann auf **Weiter**.
    
8.  Der letzte Schritt ist "Produkte hinzufügen": Verwenden Sie die Kontrollkästchen in der Produktliste, um "Cloud Key Manager" auszuwählen und klicken Sie auf "Verbindung hinzufügen":
    
    ![Produkt hinzufügen](images/add-cloudkeymanager.png "Produkt hinzufügen")
    
9.  Sie sollten eine Benachrichtigung "Erfolgreich" erhalten und werden zum Bereich **Verbindungen** von CTM umgeleitet. Hier müssen Sie die Verbindung erneut testen, indem Sie am Ende Ihrer neu erstellten Verbindung auf die drei Punkte klicken und im Menü auf "Verbindung testen" klicken:
    
    ![Verbindung testen](images/test-connection.png "Verbindung testen")
    
10.  Der Verbindungsstatus muss **Bereit** lauten.
    
    ![Verbindung bereit](images/tested-connection.png "Verbindung bereit")
    

Herzlichen Glückwunsch, Sie haben erfolgreich einen direkten Link zwischen Ihrem eigenen Thales CipherTrust-Mandanten und dem Vault erstellt, den Sie in OCI erstellt haben. Jetzt sehen wir, wie Sie damit Ihre OCI-Mandantenverschlüsselung in Übung 2 remote kontrollieren können.

## Weitere Informationen

### Über THALES CipherTrust Manager

CipherTrust Die Cloud Key Manager-(CCKM-)Lösung ist Teil des Thales CipherTrust Managers. Es wurde entwickelt, um die Unternehmensanforderungen für die Verschlüsselung von Daten in der Cloud unter Beibehaltung der Verantwortung für Verschlüsselungsschlüssel zu erfüllen und Datensicherheitsanforderungen in Cloud-Speicherumgebungen zu erfüllen. Die Lösung verwendet einen bereits installierten CipherTrust Manager (CM) als zugrunde liegende Appliance, die Verschlüsselungsschlüssel generiert, speichert und abruft, die von den CCKM-Servern verwendet werden. Die Schlüssel und CCKM werden über eine webbasierte grafische Benutzeroberfläche (Management Console), Befehlszeilenschnittstellen (CLI) und Befehlszeilenutilitys verwaltet. Die Verschlüsselungsschlüssel werden in der Thales CipherTrust Manager Appliance verwaltet. CipherTrust Cloud Key Manager wird als virtuelle Appliance bereitgestellt, die entweder On Premise oder in der Cloud installiert werden kann. Die Features und Funktionen sind für beide Deployment-Szenarios identisch. In dieser Übung wird CipherTrust Manager außerhalb von Oracle Cloud Infrastructure gehostet. Der Mandant, den Sie erhalten haben, ist eine Thales Cloud-Lösung. Das Konzept ist jedoch mit einer CipherTrust Manager-Instanz, die Sie bereits in Ihrem Unternehmen DataCenter ausführen, identisch. In beiden Fällen bietet die Verwendung eines KMS außerhalb von OCI eine größere Aufgabentrennung, die erweiterte Complianceanforderungen erfüllt, die in Ihren Sicherheits-Policys oder Complianceanforderungen des Unternehmens enthalten sind.

### Informationen zu OCI Vault

Oracle Cloud Infrastructure Vault ist ein Schlüsselverwaltungsservice, der Masterverschlüsselungsschlüssel und Secrets für den sicheren Zugriff auf Ressourcen speichert und verwaltet. Mit Vault können Sie Masterverschlüsselungsschlüssel und Secrets sicher speichern, die Sie sonst in Konfigurationsdateien oder im Code speichern könnten. Je nach Schutzmodus werden Vault-Schlüssel entweder auf dem Server gespeichert oder auf hochverfügbaren und dauerhaften Hardwaresicherheitsmodulen (HSM) gespeichert, die der Sicherheitszertifizierung gemäß Federal Information Processing Standards (FIPS) 140-2 Security Level 3 entsprechen. Zu den Schlüsselverschlüsselungsalgorithmen, die der Vault-Service unterstützt, gehören der Advanced Encryption Standard (AES), der Rivest-Shamir-Adleman-(RSA-)Algorithmus und der digitale Signaturalgorithmus für elliptische Kurven (ECDSA). Sie können symmetrische AES-Schlüssel und asymmetrische RSA-Schlüssel für die Verschlüsselung und Entschlüsselung erstellen und verwenden. Sie können auch asymmetrische RSA- oder ECDSA-Schlüssel zum Signieren digitaler Nachrichten verwenden. Mit dem Vault-Service können Sie die folgenden Ressourcen erstellen und verwalten:

*   **Vaults**
*   **Schlüssel**
*   **Secrets**

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Sonia Yuste, August 2023