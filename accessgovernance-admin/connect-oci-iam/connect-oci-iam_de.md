# Oracle Access Governance mit OCI IAM integrieren

## Einführung

Als **Access Governance-Administratoren** können sie lernen, Oracle Access Governance in OCI IAM zu integrieren.

*   Persona: Access Governance-Administrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_cupvwe5w)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

## Aufgabe 1: Neue OCI IAM Cloud Service-Verbindung in der Oracle Access Governance-Konsole konfigurieren

1.  Navigieren Sie in einem Browser mit der unter _Übung 3: Aufgabe 1_ aufgeführten URL zur Homepage des Oracle Access Governance-Service, und melden Sie sich als Benutzer mit der Anwendungsrolle **Access Governance-Administrator** an.

Geben Sie den Benutzernamen und das Kennwort für den Oracle Access Governance Campaign-Administrator ein (Pamela Green)

    **Username:**
    ```
    <copy>pamela.green</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

2.  Klicken Sie auf der Homepage des Oracle Access Governance-Service auf das Navigationsmenüsymbol, und wählen Sie **Serviceadministration → Verbundene Systeme** aus.
    
3.  Wählen Sie auf der Seite "Verbundene Systeme" die Schaltfläche **Verbundenes System hinzufügen**.
    
    ![Cloudserviceprovider auswählen](images/add-system.png)
    
4.  Wählen Sie die Kachel **Möchten Sie eine Verbindung zu einem Cloud-Serviceprovider herstellen?** aus, indem Sie auf die Schaltfläche "Hinzufügen" klicken.
    

![Cloudserviceprovider auswählen](images/select-cloud-provider.png)

5.  Wählen Sie im Schritt **System auswählen** die Kachel **Oracle Cloud Infrastructure** aus, und klicken Sie auf **Weiter**.

![Cloudserviceprovider auswählen](images/select-oci.png)

6.  Geben Sie den Namen und eine Beschreibung des verbundenen Systems ein, und klicken Sie auf **Weiter**.

Name: OCI-IAM

Beschreibung: OCI-IAM

![OCI - Details eingeben](images/enter-oci-system-name.png)

![OCI - Details eingeben](images/enter-data.png)

7.  So rufen Sie den Fingerprint des OCI-Benutzers (agcs-user) ab. Öffnen Sie ein **neues privates Browserfenster**, und melden Sie sich bei der OCI-Konsole als **Domainadministrator** an.
    
8.  Klicken Sie in der OCI-Konsole in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Klicken Sie im _Navigationsmenü_ auf _Identität und Sicherheit_. Wählen Sie _Domains_ aus der Produktliste.
    
    ![Zu Domains navigieren](images/navigate-domains.png)
    
9.  Klicken Sie auf der Seite "Domains" auf _Standarddomain_. Stellen Sie sicher, dass das **Root Compartment** ausgewählt ist.
    
    ![Zu Domains navigieren](images/default-domain.png)
    
10.  Wählen Sie _Benutzer_ aus. Klicken Sie auf _agcs-user_
    

![Benutzer erstellen](images/select-users.png)

11.  Scrollen Sie nach unten, und klicken Sie auf **API-Schlüssel**.

![OCI - Details eingeben](images/api.png)

12.  Klicken Sie auf **API-Schlüssel hinzufügen**. Klicken Sie auf **API-Schlüsselpaar generieren**.
    
    ![OCI - Details eingeben](images/add-api-key.png)
    
13.  Klicken Sie auf **Private Key herunterladen** und **Public Key herunterladen**.
    

![OCI - Details eingeben](images/click-add.png)

14.  Klicken Sie auf **Add**.
    
15.  Den **heruntergeladenen Private Key** in einem Texteditor benachrichtigen. Dies ist für den nächsten Schritt erforderlich.
    
16.  Notieren Sie sich unter **Vorschau der Konfigurationsdatei** die folgenden Details, die für den nächsten Schritt erforderlich sind.
    
    *   Benutzer-OCID
    *   Fingerprint
    *   Mandanten-OCID
    *   Region
    
    ![OCI - Details eingeben](images/config-file.png)
    
17.  Gehen Sie mit Oracle Access Governance zurück zum Browser, und geben Sie die folgenden Details ein:
    
    **Wie lautet die OCID des OCI-Benutzers?**: Geben Sie die Oracle Cloud-ID (OCID) für den OCI-Benutzer (agcs-user) ein, die im vorherigen Schritt notiert wurde.
    
    **Wie lautet der Fingerprint des OCI-Benutzers?**: Geben Sie den Fingerprint des Public Keys des API-Signaturschlüssels ein, der im vorherigen Schritt notiert wurde.
    
    **Wie lautet der SSH-Private Key des OCI-Benutzers?**: Geben Sie den heruntergeladenen SSH-Private Key (PEM-Datei) aus dem vorherigen Schritt für den API-Signaturschlüssel ein.
    
    **Wie lautet die OCI-Mandanten-OCID?**: Geben Sie die OCID für den Zielmandanten ein, die im vorherigen Schritt notiert wurde.
    
    **Wie lautet die Hauptregion des OCI-Mandanten?**: Geben Sie die Hauptregion für den OCI-Zielmandanten mit der im vorherigen Schritt notierten Regions-ID ein.
    
    ![OCI - Details eingeben](images/details-entered.png)
    
18.  Klicken Sie auf **Hinzufügen.** Klicken Sie auf "Verwalten", um den Status anzuzeigen. Wenn die Verbindungsdetails erfolgreich validiert wurden, wird der Status **Erfolgreich** für den Vorgang **Validieren** angezeigt. Der vollständige Dataload-Vorgang kann je nach den in Ihrem OCI-Mandanten verfügbaren Daten bis zu ein paar Minuten dauern. Der inkrementelle Dataload wird alle vier Stunden ausgeführt, damit dieses verbundene System die Daten synchronisiert.
    

![OCI-Verbindungsstatus](images/oci-connection-status.png)

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu