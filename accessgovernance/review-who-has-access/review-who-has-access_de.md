# Prüfen, wer Zugriff auf was für mich selbst oder meine direkt unterstellten Mitarbeiter hat

## Einführung

Benutzer können prüfen, welchen Zugriff sie haben oder welchen Zugriff sie auf ihre direkt unterstellten Mitarbeiter haben. **Manager** können Details der **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen, die ihren direkt unterstellten Mitarbeitern zugewiesen sind. **Mitarbeiterbenutzer** können Details der sich selbst zugewiesenen **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Benutzermanager

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Prüfen, wer Zugriff auf was hat](videohub:1_fb9lydfl)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Details der **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen, die meinen direkt unterstellten Mitarbeitern zugewiesen wurden
*   Details der mir zugewiesenen **Anwendungen**, **Berechtigungen** und **Rollen** anzeigen

## Aufgabe 1: Oracle Access Governance als Benutzermanager anmelden

1.  Da der Benutzer aus der vorherigen Übung **Zugriffsprüfungskampagne erstellen** auch ein Benutzermanager ist, können Sie in **Oracle Access Governance** bleiben, um diese Übung ohne Abmeldung und erneute Anmeldung durchzuführen. Führen Sie andernfalls die Schritte 2-4 unten aus.
2.  Öffnen Sie den Chrome-Browser, und gehen Sie basierend auf Ihrer **Gruppenzuweisung** zu der **Oracle Access Governance**\-URL.
    *   [Oracle Access Governance LiveLabs Gruppe 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
3.  Stellen Sie sicher, dass die Identitätsdomain **accessgov\_iam** ausgewählt ist.
4.  Melden Sie sich bei **Oracle Access Governance** als **Benutzermanager** mit einem durch die Anweisung LiveLabs angegebenen Benutzernamen und Kennwort an. **Bitte beachten Sie, dass sich der Benutzername im LiveLabs-Schritt-Screenshot möglicherweise vom Benutzernamen unterscheidet, den Sie erhalten haben.** ![Access Governance-Anmeldung](images/ag-logon.png)
5.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/ag-homepage.png)

## Aufgabe 2: Zugriff meines direkt unterstellten Mitarbeiters prüfen

1.  Klicken Sie auf das Menü **Oracle Access Governance**, gehen Sie zu **Zugriffsberechtigter**, und wählen Sie **Mein direkter Zugriff** aus. ![Mein direktes Menü](images/open-menu-direct.png)
2.  Eine Liste der Benutzer wird angezeigt, die dem aktuellen Benutzermanager unterstellt sind. Sie können einen Benutzer auswählen. Beispiel: Wählen Sie im folgenden Bildschirm **George Jimenez** aus. **Beachten Sie, dass sich Mitarbeiterbenutzer in Ihrem zugewiesenen System möglicherweise vom LiveLabs-Schritt-Screenshot unterscheiden.** ![Direkte Liste prüfen](images/review-direct-list.png)
3.  Eine Liste der Anwendungen, auf die **George Jimenez** Zugriff hat, wird aufgeführt. Sie können jede Anwendung auswählen und die Berechtigungen prüfen, die dem Benutzer in der ausgewählten Anwendung zugewiesen sind. Prüfen Sie für jede **Anwendung** Ihres Mitarbeiters **Konten**, **Berechtigung**, **Zuteilungstyp**, **Zuteilungsdatum**, **Zuteilung bis** usw. ![Bewerbung prüfen](images/review-individual-app.png)
4.  Wählen Sie im Dropdown-Menü **Gruppieren nach** die Option **Rollen**, um die Liste der Rollen anzuzeigen, die einem Benutzer zugewiesen sind. ![Rolle prüfen](images/review-individual-role.png)
5.  Prüfen Sie die den Benutzern zugewiesenen **Rollen** und die Details für jede Rolle. ![Rolle prüfen](images/user-roles.png)

## Aufgabe 3: Meinen Zugriff prüfen

1.  Klicken Sie auf das Menü **Oracle Access Governance**, gehen Sie zu **Zugriffsberechtigter**, und wählen Sie **Mein Zugriff** aus. ![Mein direktes Menü](images/open-menu-direct.png)
2.  Sie können eine Liste der **Anwendungen** prüfen, auf die der angemeldete Benutzer Zugriff hat. Sie können jede Anwendung auswählen und die dem Benutzer zugewiesenen Berechtigungen prüfen. ![Meinen Zugriff prüfen](images/review-my-access.png)
3.  Wählen Sie im Dropdown-Menü **Gruppieren nach** die Option **Rollen**, um eine Liste der Rollen anzuzeigen, die dem Benutzer zugewiesen sind. Sie können auch auf jede **Rolle** klicken, um Details anzuzeigen. ![Meine Rolle prüfen](images/review-my-access-role.png)
4.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole als Benutzermanager navigiert, um Ihre direkt unterstellten Mitarbeiter und Ihre eigenen Zugriffsberechtigungen aufzulisten. Dies ist eine bewährte Sicherheitsmethode und Teil der **Due Care / Due Diligence** der Mitarbeiter.
5.  Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance mit Zugriff auf Folgendes](https://docs.oracle.com/en/cloud/paas/access-governance/yhaty/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autor** - Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Zuletzt aktualisiert am/um** - Edward Lu, Oracle IAM Product Management, Oktober 2022