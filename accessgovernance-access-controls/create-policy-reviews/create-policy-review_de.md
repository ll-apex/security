# Policenprüfungskampagnen erstellen und prüfen

## Einführung

Access Governance-Administratoren (Pamela Green) können eine Policy-Prüfungskampagne erstellen und prüfen.

*   Persona: Access Governance-Administrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_yivv30ww)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Policy-Überprüfungskampagnen für OCI-IAM-Policys erstellen
*   Von der Kampagne ausgeführte Policy-Prüfungsaufgaben untersuchen
*   Evaluieren Sie Policy-Prüfungsaufgaben, die Ihnen als Kampagnenprüfer zugewiesen sind

## Aufgabe 1: Policy-Prüfungskampagne erstellen

1.  Gehen Sie in Ihrem Browser zu "Oracle Access Governance Console".
    
2.  Benutzernamen und Kennwort für **Oracle Access Governance Administrator** eingeben (Pamela Green)
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

3.  Navigieren Sie auf der Homepage von Oracle Access Governance zu **Zugriffsprüfungen** -> **Kampagnen**.

![Access Governance-Homepage](images/navigate-campaign.png)

4.  Klicken Sie auf **Kampagne erstellen**.

![Access Governance-Homepage](images/create-a-campaign.png)

5.  Wählen Sie unter **Welchen Typ von Zugriffsprüfungskampagne Sie ausführen möchten** die Option "Zugriff auf **Oracle Cloud Infrastructure** prüfen" aus.
    
    ![Access Governance-Homepage](images/select-oci-campaign.png)
    
6.  Wählen Sie im Schritt "Auswahlkriterien" die Kachel **Welche Mandanten?** aus. Eine Liste der verfügbaren Cloud-Mandanten wird angezeigt.
    
    ![Cloud-Provider auswählen](images/select-tenancies.png)
    
7.  Wählen Sie einen geeigneten Cloud-Mandanten aus. Wählen Sie in diesem Tutorial Ihren Cloud-Mandanten aus. Ein grünes Häkchen wird gegen Ihre Auswahl markiert.
    
    ![Cloud-Provider auswählen](images/which-tenancy.png)
    
8.  Klicken Sie auf **Weitere definieren**. Sie können Ihre Auswahl weiter eingrenzen, indem Sie ein bestimmtes Compartment und eine Domain auswählen, um domänenspezifische Policy-Prüfungen auszuführen.
    
    ![Cloud-Provider auswählen](images/click-refine.png)
    
9.  Geben Sie die unten genannten **Compartment**\-Details ein, und klicken Sie auf **Anwenden**
    
    Fach: ag-compartment
    
    ![Cloud-Provider auswählen](images/ag-compartment.png)
    
10.  Fahren Sie mit dem nächsten Schritt fort, um Policys auszuwählen, die Sie prüfen möchten. Wählen Sie die Kachel **Welche Policys?** aus. In der ausgewählten Domain wird eine Liste der verfügbaren Policys angezeigt.
    
    ![Access Governance-Homepage](images/select-which-policies.png)
    
11.  Wählen Sie die Policys aus, die Sie prüfen möchten. Wählen Sie in diesem Tutorial die folgenden Policys aus, und klicken Sie auf **Meine Auswahl anwenden**.
    
          - auditors-policy
          - network-admins-policy
          - security-admins-policy
        
    
    ![Access Governance-Homepage](images/select-the-policies.png)
    
12.  Fahren Sie mit dem Schritt **Workflow zuweisen** fort. Klicken Sie dazu auf **Ich bin gut, gehen Sie zu Workflows.** Hier können Sie den Genehmigungsworkflow für Ihre Beurteilungsaufgaben definieren und auf **Weiter** klicken.
    
    ![Access Governance-Homepage](images/choose-workflow.png)
    
    ![Access Governance-Homepage](images/click-next-workflow.png)
    
13.  Im Schritt **Details hinzufügen** können Sie definieren, wie oft (einmalig oder regelmäßig) eine Zugriffsprüfungskampagne ausgeführt werden soll. Außerdem können Sie der Kampagne einen sinnvollen Namen geben, eine unterstützende Beschreibung hinzufügen und zusätzlichen Attributen Werte zuweisen, z.B. wer Eigentümer der Kampagne ist und wann die Kampagne gestartet oder beendet werden soll.
    
14.  Nehmen Sie für dieses Tutorial die folgenden Änderungen im Schritt **Details hinzufügen** vor:
    
          **How often do you want this to run?** : One time
        
          **What do you want to call this campaign?**: Policy-Review-OCI-IAM
        
          **How do you want to describe this campaign?**: Policy-Review-OCI-IAM
        
          **Who owns this campaign?**: Me
        
          **How would you like to schedule your campaign?** : Run now (will start 10 minutes from creation)
        
15.  Klicken Sie auf **Weiter**
    
    ![Access Governance-Homepage](images/campaign-information.png)
    
16.  Im Schritt **Prüfen und weiterleiten** werden die Informationen angezeigt, die Sie in den vorherigen Schritten hinzugefügt haben. Wählen Sie **Erstellen** aus, um die Kampagne zu erstellen. Ihre Kampagne ist geplant und wird auf der Seite **Kampagnen** angezeigt. Es wird 10 Minuten nach der Erstellung ausgeführt.
    
    ![OCI - Details eingeben](images/click-create-new-campaign.png)
    
    ![OCI - Details eingeben](images/campaign-scheduled.png)
    

## Aufgabe 2: Policy-Prüfungsaufgaben ausführen

Hier prüfen und zertifizieren Sie OCI IAM-Prüfaufgaben, die von der in der vorherigen Aufgabe erstellten Kampagne ausgelöst wurden.

1.  Gehen Sie in Ihrem Browser zu "Oracle Access Governance Console".
    
2.  Benutzernamen und Kennwort für **Oracle Access Governance Campaign Reviewer** eingeben (Pamela Green)
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

3.  Wählen Sie auf der Homepage der Oracle Access Governance-Konsole im Navigationsmenü **Zugriffsprüfungen -> Meine Zugriffsprüfungen** aus.
    
4.  Um von Ihrer Policy-Prüfungskampagne erstellte Prüfungsaufgaben anzuzeigen, klicken Sie auf die Registerkarte **Prüfaufgaben für Zugriffskontrolle**. Alle Ihnen zugewiesenen Aufgaben zur Prüfung des Policy-Zugriffs werden als Prüfer angezeigt. Oracle Access Governance verwendet ein internes analytisch basiertes Intelligence-System, um Akzeptanz-/Prüfempfehlungen bereitzustellen.
    

![Access Governance-Homepage](images/access-control-review-tasks.png)

5.  In diesem Tutorial prüfen wir die Empfehlungen von Oracle Access Governance.

*   network-admins-policy ist zur Prüfung markiert
    
*   security-admins-policy ist zur Prüfung markiert
    
*   Auditoren-Policy ist für Akzeptiert markiert
    
    6.  Sehen wir uns die von Oracle Access Governance generierten Insights an. Klicken Sie unter **Auditoren-Policy** in der Spalte **Insights** auf die entsprechenden Links **Aktionen**.
        
    7.  Auf der Seite **Insights** können Sie unsere Empfehlung für die Policy-Prüfungsaufgabe anzeigen. Im linken Bereich können Sie die Policy-Informationen anzeigen. Auf der rechten Seite können Sie eine vollständige Liste der umsetzbaren und nicht umsetzbaren Policy-Anweisungen anzeigen, Policy-Details anzeigen, um festzustellen, wem und wofür die Policy-Anweisung Zugriff erteilt, und entsprechende Entscheidungen für jede Anweisung treffen.
        
    
    ![Access Governance-Homepage](images/view-policy.png)
    
    8.  Klicken Sie neben der Policy-Anweisung auf die Schaltfläche "View", um die mit den Policys verknüpften Ressourcen anzuzeigen. Klicken Sie auf Summmary und Details, um die Informationen anzuzeigen. Klicken Sie auf "Close".
        
        ![Access Governance-Homepage](images/summary.png)
        
        ![Access Governance-Homepage](images/details.png)
        
    9.  Um eine Prüfungsentscheidung zu treffen, können Sie entweder alle umsetzbaren Anweisungen in dieser Policy auf einmal widerrufen oder alle umsetzbaren Anweisungen in dieser Policy akzeptieren oder eine Entscheidung für jede Policy-Anweisung einzeln treffen. Lassen Sie uns für dieses Tutorial 2 Anwendungsfälle validieren:
        
    
    **Anwendungsfall 1:** Policy-Anweisung aus einer Policy entziehen - **Auditors-Policy**
    
        - Let’s revoke the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment** from the policy  **auditors-policy**. 
        
        ![Access Governance Homepage](images/auditor-policy.png)
        
        - Click on the cross button under Actions column for the policy statement **Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment**
        
        ![Access Governance Homepage](images/click-revoke-auditor-policy.png)
        
        - Click on **Apply**
        
        ![Access Governance Homepage](images/click-apply-auditor-policy.png)
        
        -  Enter justification for why you revoke the access review items and accept the remaining items then click on **Submit** This will trigger the auto-remediation process in the Oracle Access Governance system.
        
        ![Access Governance Homepage](images/policy-review-list-new.png)
        
    
    **Anwendungsfall 2:** Gesamte Policy entziehen - **security-admins-policy**
    
        - Let's revoke the entire policy **security-admins-policy** 
        
         ![Access Governance Homepage](images/security-policy.png)
        
        - Click on the Revoke All button. 
        
         ![Access Governance Homepage](images/revoke-all-security-policy.png)
        
        - Click **Apply.** The **Confirmation** dialog box is displayed.
        
          ![Access Governance Homepage](images/apply-security-policy.png)
        
    
    *   Geben Sie eine Begründung an, und klicken Sie auf **Weiterleiten.** Die Korrektur des Closed-Loop-Zugriffs erfolgt automatisch.
    
    10.  (Optional) Melden Sie sich bei der Identitätsdomain an: ag-domain-OCI-Konsole als Identitätsdomainadministrator. Naviagte zu Identität und Sicherheit -> Identität -> Richtlinien.
*   Prüfen Sie, ob die gesamte Policy - **security-admins-policy** - erfolgreich aus der Liste der Policys widerrufen wurde.
    
    Vor der Policy-Überprüfung:
    
    ![Access Governance-Homepage](images/before-security-policy.png)
    
    Nach der Policy-Prüfung:
    
    ![Access Governance-Homepage](images/after-security-policy.png)
    
*   Prüfen Sie die Policy-Anweisung **Allow group ag-domain/Auditors to read audit-events in Compartment ag-compartment** der Policy - **auditors-policy** wurde erfolgreich widerrufen.
    
    Vor der Policy-Überprüfung:
    
    ![Access Governance-Homepage](images/before-auditor-policy.png)
    
    Nach der Policy-Prüfung:
    
    ![Access Governance-Homepage](images/after-auditor-policy.png)
    
    Damit ist das Tutorial zum Erstellen und Ausführen von OCI IAM-Policy-Prüfungen abgeschlossen.
    
    Sie können jetzt **mit der nächsten Übung fortfahren**.
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anuj Tripathi, Oktober 2023