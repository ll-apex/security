# Policy-Prüfungskampagnen erstellen

## Einführung

Access Governance-Administratoren (Pamela Green) können eine Policy-Prüfungskampagne erstellen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Access Governance-Administrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Policy-Überprüfungskampagnen für OCI-IAM-Policys erstellen

## Aufgabe 1: Policy-Prüfungskampagne erstellen

1.  Gehen Sie in Ihrem Browser mit der in _Übung 3: Aufgabe 1_ genannten URL zur Oracle Access Governance-Konsole.
    
2.  Benutzernamen und Kennwort für **Oracle Access Governance Administrator** eingeben (Pamela Green)
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

3.  Scroll down and select the **“Let’s create some work and define a new campaign”** tile. Alternatively, you can select **Navigation Menu -> Access Reviews -> Campaigns.** On the **Campaigns** page, click the **Create a campaign** button.

![Access Governance-Homepage](images/ag-homepage-campaign.png)

*   Wählen Sie im Schritt "Auswahlkriterien" die Kachel **Welche Cloud-Provider?** aus. Eine Liste der verfügbaren Cloud-Mandanten wird angezeigt.

![Cloud-Provider auswählen](images/select-cloud-providers.png)

*   Wählen Sie einen geeigneten Cloud-Mandanten aus. Wählen Sie in diesem Tutorial Ihren Cloud-Mandanten aus. Ein grünes Häkchen wird gegen Ihre Auswahl markiert.

![Cloud-Provider auswählen](images/green-tick-cloud-provider.png)

*   Klicken Sie auf **Weitere definieren**. Sie können Ihre Auswahl weiter eingrenzen, indem Sie ein bestimmtes Compartment und eine Domain auswählen, um domänenspezifische Policy-Prüfungen auszuführen.

![Cloud-Provider auswählen](images/click-refine.png)

*   Geben Sie die unten genannten **Domain**\- und **Compartment**\-Details ein, und klicken Sie auf **Anwenden**
    
    *   Domain: ag-Domain
    *   Fach: ag-compartment

![Cloud-Provider auswählen](images/click-apply-refine.png)

*   Fahren Sie mit dem nächsten Schritt fort, um Policys auszuwählen, die Sie prüfen möchten. Wählen Sie die Kachel **Welche Policys?** aus. In der ausgewählten Domain wird eine Liste der verfügbaren Policys angezeigt.

![Access Governance-Homepage](images/select-which-policies.png)

*   Wählen Sie die Policys aus, die Sie prüfen möchten. Wählen Sie in diesem Tutorial die folgenden Policys aus, und klicken Sie auf **Meine Auswahl anwenden**.
    
    *   Auditoren-Politik
    *   network-admins-policy
    *   Sicherheits-Admins-Policy
    
    ![Access Governance-Homepage](images/select-the-policies.png)
    
*   Fahren Sie mit dem Schritt **Workflow zuweisen** fort. Klicken Sie dazu auf **Ich bin gut, gehen Sie zu Workflows.** Hier können Sie den Genehmigungsworkflow für Ihre Beurteilungsaufgaben definieren und auf **Weiter** klicken.
    

![Access Governance-Homepage](images/choose-workflow.png)

![Access Governance-Homepage](images/click-next-workflow.png)

*   Im Schritt **Details hinzufügen** können Sie definieren, wie oft (einmalig oder regelmäßig) eine Zugriffsprüfungskampagne ausgeführt werden soll. Außerdem können Sie der Kampagne einen sinnvollen Namen geben, eine unterstützende Beschreibung hinzufügen und zusätzlichen Attributen Werte zuweisen, z.B. wer Eigentümer der Kampagne ist und wann die Kampagne gestartet oder beendet werden soll.
    
*   Nehmen Sie für dieses Tutorial die folgenden Änderungen im Schritt **Details hinzufügen** vor:
    
    **Wie oft soll dieser Prozess ausgeführt werden?** : Einmalig
    
    **Wie soll diese Kampagne genannt werden?**: Policy-Review-OCI-IAM
    
    **Wie soll die Kampagne beschrieben werden?**: Policy-Review-OCI-IAM
    
    **Wer ist Eigentümer dieser Kampagne?**: Ich
    
    **Wie möchten Sie Ihre Kampagne planen?** : Jetzt ausführen (beginnt 10 Minuten nach Erstellung)
    
*   Klicken Sie auf **Weiter**
    

![Access Governance-Homepage](images/campaign-information.png)

*   Im Schritt **Prüfen und weiterleiten** werden die Informationen angezeigt, die Sie in den vorherigen Schritten hinzugefügt haben. Wählen Sie **Erstellen** aus, um die Kampagne zu erstellen. Ihre Kampagne ist geplant und wird auf der Seite **Kampagnen** angezeigt. Es wird 10 Minuten nach der Erstellung ausgeführt.

![OCI - Details eingeben](images/click-create-new-campaign.png)

![OCI - Details eingeben](images/campaign-scheduled.png)

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023