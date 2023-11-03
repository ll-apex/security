# Eigenen Schlüssel zu Oracle Cloud Infrastructure verwenden (BYOK zu OCI)

## Einführung

In dieser Übung erfahren Sie, wie Sie Ihre eigenen Schlüssel außerhalb von OCI in CTM erstellen und in Ihrem Vault zu Ihrem OCI-Mandanten bringen.

Geschätzte Zeit: 15 Minuten

[Gehen Sie durch das Labor](videohub:1_grwhdvvv)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Eigenen Vault von CTM hinzufügen und verwalten
*   Verschlüsselungsschlüssel in CTM erstellen und in OCI Vault von CTM verwalten

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Zugriff auf Ihre OCI- und CTM-Accounts
*   Alle vorherigen Übungen erfolgreich abgeschlossen

## Aufgabe 1: Oracle Vaults von CTM verwalten

1.  Öffnen Sie die CipherTrust Manager-Web-UI, und klicken Sie auf die Kachel "Cloud Key Manager".
    
    ![ME](images/log-in-ctm.png "ME")
    
2.  Klicken Sie im linken Fensterbereich auf Container und Oracle Vaults.
    
    ![Oracle Vaults](images/oracle-vaults.png "Oracle Vaults")
    
3.  Klicken Sie auf "Vorhandenen Vault hinzufügen".
    
    ![Vault hinzufügen](images/add-vault.png "Vault hinzufügen")
    
4.  Fügen Sie unter "Vorhandenen Vault hinzufügen" die folgenden Parameter hinzu:
    
    *   Oracle-Verbindung: Wählen Sie die zuvor erstellte Verbindung aus. Sie sollte "OCI-Connection\_XXX" lauten, wobei "XXX" Ihre Studierendennummer ist.
    *   Compartment - Wählen Sie das Compartment "ocw23-OCI-Vault-HOL" aus.
    *   Region - Wählen Sie in der Dropdown-Liste Ihre relevante Region aus. Diese sollte mit der in der vorherigen Übung identisch sein.
    *   Vault - Wählen Sie den Vault aus, den Sie zuvor erstellt haben. Er sollte "ocw23-OCI-Vault-XXX" lauten, wobei "XXX" Ihre Studierendennummer ist. Seid vorsichtig, euren Vault auszuwählen und nicht den eines anderen Schülers! Klicken Sie anschließend auf {\\b Next}.
    
    ![Info-Vault](images/info-vault.png "Info-Vault")
    
5.  Der nächste Schritt "Bucket-Name hinzufügen, Bucket-Namespace" gilt nicht für unseren Übungsanwendungsfall. Daher werden wir ihn überspringen und direkt zu Schritt 6 gehen.
    
    ![Bucket hinzufügen](images/add-bucket.png "Bucket hinzufügen")
    
6.  Klicken Sie auf "Hinzufügen", um Ihren Vault zu Ihrem CTM-Mandanten hinzuzufügen.
    
    ![Neuer Vault](images/created-vault.png "Neuer Vault")
    
    Sie sollten eine Meldung "Erfolgreich" erhalten und sehen, dass der Vault im Bereich "Oracle Vaults" aufgeführt ist. Herzlichen Glückwunsch, jetzt können Sie Ihren Oracle Vault remote von Ihrem CTM-Mandanten verwalten.
    

## Aufgabe 2: Oracle Keys aus CTM verwalten

1.  Klicken Sie im linken Fensterbereich auf **Cloud-Schlüssel > Oracle**.
    
    ![Oracle-Schlüssel](images/oracle-keys.png "Oracle-Schlüssel")
    
2.  Klicken Sie auf die Schaltfläche "Schlüssel hinzufügen". Der erste Schritt des Fensters "Materialursprung auswählen" des Assistenten "Oracle-Schlüssel hinzufügen" wird angezeigt. Wählen Sie die Schlüsselquelle aus: In diesem Fall erstellen wir den Schlüssel lokal auf CipherTrust. Klicken Sie für "Methode auswählen" auf "Neues Schlüsselmaterial erstellen/hochladen" und dann für "Quelle auswählen" auf "CipherTrust":
    
    ![Schlüssel hinzufügen](images/add-key.png "Schlüssel hinzufügen")
    
3.  Erstellen Sie im zweiten Schritt "CipherTrust-Schlüssel konfigurieren" einen Schlüssel für OCI, indem Sie die folgenden Informationen angeben:
    
    *   **Schlüsselname** - Geben Sie "ocw23-AES-256-XXX" ein, wobei "XXX" Ihre Studierendennummer ist.
    *   **Schlüsseltyp** - Klicken Sie auf "AES".
    *   **Schlüsselgröße** - Klicken Sie auf "256".
    
    Klicken Sie dann auf "Weiter".
    
    ![AES-Schlüssel hinzufügen](images/aes-key.png "AES-Schlüssel hinzufügen")
    
4.  Im dritten Schritt "oracle key konfigurieren" müssen Sie die folgenden Informationen angeben:
    
    *   **Oracle-Schlüsselname** - Geben Sie "ocw23-AES-256-XXX" ein, wobei "XXX" Ihre Studierendennummer ist.
    *   **Oracle Compartment** - Wählen Sie in der Dropdown-Liste die Option "ocw23-OCI-Vault-HOL" aus.
    *   **Vault** - Wählen Sie "ocw23-OCI-Vault-XXX" aus, wobei "XXX" Ihre Studierendennummer aus der Dropdown-Liste ist. Stellen Sie sicher, dass das Optionsfeld "HSM" ausgewählt ist und "Next" angezeigt wird.
    
    ![AES-Schlüssel hinzufügen](images/key-compartment.png "AES-Schlüssel hinzufügen")
    
5.  Prüfen Sie die Konfiguration der Schlüsselerstellung, und klicken Sie auf die Schaltfläche "Schlüssel hinzufügen".
    
    ![Schlüssel überprüfen](images/review-key.png "Schlüssel überprüfen")
    
6.  Sobald der Schlüssel erfolgreich erstellt wurde, wird der folgende Bildschirm angezeigt. Klicken Sie auf "Schließen".
    
    ![Schlüssel erfolgreich erstellt](images/created-key.png "Schlüssel erfolgreich erstellt")
    
7.  Der Schlüssel wird nun in der Liste "Oracle Keys" angezeigt:
    
    ![Liste der erstellten Schlüssel](images/list-key.png "Liste der erstellten Schlüssel")
    
    Herzlichen Glückwunsch! Sie haben Ihren ersten Schlüssel in CTM erstellt. Dieser Schlüssel muss automatisch mit dem Vault synchronisiert worden sein, den Sie in OCI erstellt haben.
    
8.  Jetzt werden wir sicherstellen, dass der Schlüssel tatsächlich in OCI Vault enthalten ist. Dazu fungieren Sie als Sicherheitsoperator Ihres Unternehmens mit Ihrem Secops\_XXX-Benutzer (wobei "XXX" Ihre Studierendennummer ist) und melden sich bei der OCI-Konsole an. Befolgen Sie bei Bedarf die Anweisungen im vorläufigen Labor "Erste Schritte". Klicken Sie in der OCI-Konsole oben links auf das Hamburger-Menü, und wählen Sie "Identität und Sicherheit" und dann "Vault" aus.
    
    ![Auf OCI-Vault zugreifen](images/accessing-oci-vault.png "Auf OCI-Vault zugreifen")
    
9.  Auf der Hauptseite von OCI Vault sollte Ihr Vault aufgelistet sein. Wenn nicht, stellen Sie sicher, dass das auf der linken Seite ausgewählte Compartment "ocw23-OCI-Vault-HOL" ist. Sobald Sie ihn gefunden haben, klicken Sie auf Ihren Vault-Namen. Er sollte "ocw23-OCI-Vault-XXX" heißen, wobei "XXX" Ihre Studentennummer ist.
    
    ![OCI-Vault](images/oci-vault.png "Vault")
    
10.  Sie sehen jetzt alle Schlüssel in dem Vault, den Sie erstellt haben. Der Schlüssel, den Sie in der Thales CTM-Schnittstelle erstellt haben, sollte in Ihrem Vault angezeigt werden. Klicken Sie auf den Namen Ihres Schlüssels.
    
    ![Schlüssel in OCI-Vault](images/keys-oci.png "Schlüssel in OCI-Vault")
    
11.  Alle Details zu diesem Schlüssel werden angezeigt. Wenn Sie nun links im Menü "Ressourcen" auf "Versionen" klicken, werden weitere wichtige Details angezeigt, einschließlich der als "Extern" angezeigten Schlüsselquelle:
    
    ![externer Schlüssel](images/external-key.png "externer Schlüssel")
    

Herzlichen Glückwunsch! Sie haben den OCI Vault, den Sie selbst erstellt haben, mit Ihrem eigenen Thales CTM-Mandanten verknüpft, und Sie haben einen Schlüssel außerhalb von OCI erstellt, der jetzt direkt in OCI verwendet werden kann. Abschließend wird in der nächsten Übung erläutert, wie Sie mit diesem Schlüssel Daten in nativen OCI-Ressourcen verschlüsseln können.

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Sonia Yuste, Juni 2023