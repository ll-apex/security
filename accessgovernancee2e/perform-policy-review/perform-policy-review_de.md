# Policy-Prüfungskampagnen erstellen und ausführen

## Einführung

Access Governance Campaign Reviewer (Harlan Bullard) kann Policy Review-Aufgaben ausführen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Kampagnenprüfer

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Von der Kampagne ausgeführte Policy-Prüfungsaufgaben untersuchen
*   Evaluieren Sie Policy-Prüfungsaufgaben, die Ihnen als Kampagnenprüfer zugewiesen sind

## Aufgabe 1: Policy-Prüfungsaufgaben ausführen

In dieser Aufgabe prüfen und zertifizieren Sie OCI IAM-Prüfaufgaben, die von der in der vorherigen Aufgabe erstellten Kampagne ausgelöst wurden.

1.  Wählen Sie auf der Homepage der Oracle Access Governance-Konsole im Navigationsmenü **Zugriffsprüfungen -> Meine Zugriffsprüfungen** aus.
    
2.  Um von Ihrer Policy-Prüfungskampagne erstellte Prüfungsaufgaben anzuzeigen, klicken Sie auf die Registerkarte **Policy-Prüfaufgaben**. Alle Ihnen zugewiesenen Aufgaben zur Prüfung des Policy-Zugriffs werden als Prüfer angezeigt. Oracle Access Governance verwendet ein internes analytisch basiertes Intelligence-System, um Akzeptanz-/Prüfempfehlungen bereitzustellen.
    

![Access Governance-Homepage](images/my-access-reviews.png)

3.  In diesem Tutorial prüfen wir die Empfehlungen von Oracle Access Governance.

*   network-admins-policy ist zur Prüfung markiert
*   security-admins-policy ist zur Prüfung markiert
*   Auditoren-Policy ist für Akzeptiert markiert

4.  Sehen wir uns die von Oracle Access Governance generierten Insights an. Klicken Sie unter **Auditoren-Policy** in der Spalte **Insights** auf die entsprechenden Links **Aktionen**.
    
5.  Auf der Seite **Insights** können Sie unsere Empfehlung für die Policy-Prüfungsaufgabe anzeigen. Im linken Bereich können Sie die Policy-Informationen anzeigen. Auf der rechten Seite können Sie eine vollständige Liste der umsetzbaren und nicht umsetzbaren Policy-Anweisungen anzeigen, Policy-Details anzeigen, um festzustellen, wem und wofür die Policy-Anweisung Zugriff erteilt, und entsprechende Entscheidungen für jede Anweisung treffen.
    

    ![Access Governance Homepage](images/view-policy.png)
    

6.  Klicken Sie neben der Policy-Anweisung auf die Schaltfläche "View", um die mit den Policys verknüpften Ressourcen anzuzeigen. Klicken Sie auf Summmary und Details, um die Informationen anzuzeigen. Klicken Sie auf "Close".
    
    ![Access Governance-Homepage](images/summary.png)
    
    ![Access Governance-Homepage](images/details.png)
    
7.  Um eine Prüfungsentscheidung zu treffen, können Sie entweder alle umsetzbaren Anweisungen in dieser Policy auf einmal widerrufen oder alle umsetzbaren Anweisungen in dieser Policy akzeptieren oder eine Entscheidung für jede Policy-Anweisung einzeln treffen. Lassen Sie uns für dieses Tutorial 2 Anwendungsfälle validieren:
    

    **Usecase 1:**  Revoke policy statement from a policy - **auditors-policy**
    
      - Let’s revoke the policy statement **Allow group Auditors to read audit-events in compartment Quality-Assurance** from the policy  **auditors-policy**. 
    
      ![Access Governance Homepage](images/auditor-policy.png)
    
      - Click on the cross button under Actions column for the policy statement **Allow group Auditors to read audit-events in compartment Quality-Assurance**
    
      ![Access Governance Homepage](images/click-revoke-auditor-policy.png)
    
      - Click on **Apply**
    
      ![Access Governance Homepage](images/click-apply-auditor-policy.png)
    
      -  Enter justification for why you revoke the access review items and accept the remaining items then click on **Submit** This will trigger the auto-remediation process in the Oracle Access Governance system.
    
      ![Access Governance Homepage](images/policy-review-list-new.png)
    
    
    **Usecase 2:**  Revoke an entire policy - **security-admins-policy** 
    
      - Let's revoke the entire policy **security-admins-policy** 
    
       ![Access Governance Homepage](images/security-policy.png)
    
      - Click on the Revoke All button. 
    
       ![Access Governance Homepage](images/revoke-all-security-policy.png)
    
      - Click **Apply.** The **Confirmation** dialog box is displayed.
    
        ![Access Governance Homepage](images/apply-security-policy.png)
    
     - Provide justification and then click **Submit.** The closed loop access remediation will take place automatically.
    

7.  Melden Sie sich bei der Identitätsdomain an: ag-domain-OCI-Konsole als Identitätsdomainadministrator. Naviagte zu Identität und Sicherheit -> Identität -> Richtlinien.

*   Prüfen Sie, ob die gesamte Policy - **security-admins-policy** - erfolgreich aus der Liste der Policys widerrufen wurde.

Vor der Policy-Überprüfung:

![Access Governance-Homepage](images/before-security-policy.png)

Nach der Policy-Prüfung:

![Access Governance-Homepage](images/after-security-policy.png)

*   Prüfen Sie, ob die Policy-Anweisung **Lesen von Auditereignissen durch Gruppenaudits in Compartment Quality-Assurance** der Policy **auditors-policy** erfolgreich widerrufen wurde.

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

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023