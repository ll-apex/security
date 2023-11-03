# Prüfen, wer Zugriff auf was für mich selbst oder meine direkt unterstellten Mitarbeiter hat

## Einführung

Benutzer können prüfen, welchen Zugriff sie haben oder welchen Zugriff sie auf ihre direkt unterstellten Mitarbeiter haben. **Manager** können Details der **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen, die ihren direkt unterstellten Mitarbeitern zugewiesen sind. **Mitarbeiterbenutzer** können Details der sich selbst zugewiesenen **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Benutzermanager

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Prüfen, wer Zugriff auf was hat](videohub:1_tfcwfcju)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Details der **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen, die meinen direkt unterstellten Mitarbeitern zugewiesen wurden
*   Details der mir zugewiesenen **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen

## Aufgabe 1: Oracle Access Governance als Benutzermanager anmelden

1.  Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
    
2.  Melden Sie sich bei **Oracle Access Governance** als **Benutzermanager - Harlan Bullard** mit dem unten angegebenen Benutzernamen und Kennwort an.
    
    **Benutzername:**
    
        <copy>harlan.bullard</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        
    
    ![Access Governance-Anmeldung](images/manager-ag-logon.png)
    
3.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/manager-ag-homepage.png)
    

## Aufgabe 2: Zugriff meines direkt unterstellten Mitarbeiters prüfen

1.  Klicken Sie auf das Menü **Oracle Access Governance**, gehen Sie zu **Zugriffsberechtigter**, und wählen Sie **Mein direkter Zugriff** aus. ![Mein direktes Menü](images/manager-open-menu-direct.png)
2.  Eine Liste der Benutzer wird angezeigt, die dem aktuellen Benutzermanager unterstellt sind. Sie können einen Benutzer auswählen. Beispiel: Wählen Sie im folgenden Bildschirm die Option **Hernandez markieren** aus. **Beachten Sie, dass sich Mitarbeiterbenutzer in Ihrem zugewiesenen System möglicherweise vom LiveLabs-Schritt-Screenshot unterscheiden.** ![Direkte Liste prüfen](images/manager-review-direct-list.png)
3.  Eine Liste der Anwendungen, auf die **Mark Hernandez** Zugriff hat, wird aufgeführt. Sie können jede Anwendung auswählen und die Berechtigungen prüfen, die dem Benutzer in der ausgewählten Anwendung zugewiesen sind. Prüfen Sie für jede **Anwendung** Ihres Mitarbeiters **Konten**, **Berechtigung**, **Zuteilungstyp**, **Zuteilungsdatum**, **Zuteilung bis** usw. ![Bewerbung prüfen](images/manager-review-individual-app.png)
4.  Wählen Sie im Dropdown-Menü **Gruppieren nach** die Option **Rollen**, um die Liste der Rollen anzuzeigen, die einem Benutzer zugewiesen sind. ![Rolle prüfen](images/manager-review-individual-role.png)
5.  Prüfen Sie die den Benutzern zugewiesenen **Rollen** und die Details für jede Rolle.

## Aufgabe 3: Meinen Zugriff prüfen

1.  Klicken Sie auf das Menü **Oracle Access Governance**, gehen Sie zu **Zugriffsberechtigter**, und wählen Sie **Mein Zugriff** aus. ![Mein direktes Menü](images/manager-open-direct.png)
2.  Sie können eine Liste der **Anwendungen** prüfen, auf die der angemeldete Benutzer Zugriff hat. Sie können jede Anwendung auswählen und die dem Benutzer zugewiesenen Berechtigungen prüfen. ![Meinen Zugriff prüfen](images/manager-review-my-access.png)
3.  Wählen Sie im Dropdown-Menü **Gruppieren nach** die Option **Rollen**, um eine Liste der Rollen anzuzeigen, die dem Benutzer zugewiesen sind. Sie können auch auf jede **Rolle** klicken, um Details anzuzeigen.
4.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole als **Benutzermanager** navigiert, um Ihre direkt unterstellten Mitarbeiter und Ihre eigenen Zugriffsberechtigungen aufzulisten. Dies ist eine bewährte Sicherheitsmethode und Teil der **Due Care / Due Diligence** der Mitarbeiter.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance mit Zugriff auf Folgendes](https://docs.oracle.com/en/cloud/paas/access-governance/yhaty/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023