# Zugriffsprüfungsaufgabe ausführen

## Einführung

**Zugriffsprüfungen** können von Benutzern mit den folgenden Rollen über die **Oracle Access Governance**\-Konsole ausgeführt werden. Diese Rollen basieren auf Datenattributen, die aus dem verbundenen System abgeleitet sind:

*   **Benutzer** (Zugriff prüfen, der mir selbst zugewiesen ist)
*   **Manager** (Zugriff prüfen, der Benutzern in meinem Team zugewiesen ist)
*   **Eigentümer** (Zugriff prüfen, der Benutzern für Ressourcen zugewiesen ist, für die ich verantwortlich bin)

Basierend auf dem Workflowsetup in der ersten Übung **Zugriffsprüfungskampagne erstellen** verteilt **Oracle Access Governance** Zugriffsprüfungen an die entsprechenden Prüfer in der ausgewählten Organisation. In dieser Übung ist **Mitarbeiterbenutzer** der Prüfer der ersten Ebene und **Benutzermanager** der Prüfer der zweiten Ebene. Durch die Nutzung der in Zugriffsprüfungen eingebetteten **präskriptiven Analysen** und **Informationen** können Mitarbeiter und Benutzermanager fundierte Entscheidungen über Zugriffsberechtigungen treffen. Benutzer können Artikel mit geringem Risiko basierend auf **AI/ML-Empfehlungen** aus dem System auch global genehmigen.

*   Geschätzte Zeit: 20 Minuten
*   Persona: Mitarbeiter und Benutzermanager

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Zugriffsprüfung ausführen](videohub:1_kui9p56v)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Akzeptieren oder widerrufen Sie die mir zugewiesene **Zugriffsprüfungsaufgabe** aus der **Zertifizierungskampagne** als **Mitarbeiterbenutzer**
*   Akzeptieren oder entziehen Sie die mir zugewiesene **Zugriffsprüfungsaufgabe** aus der **Zertifizierungskampagne** als **Benutzermanager**

## Aufgabe 1: Oracle Access Governance als Mitarbeiterbenutzer anmelden

1.  Öffnen Sie den Chrome-Browser, und gehen Sie basierend auf Ihrer **Gruppenzuweisung** zu der **Oracle Access Governance**\-URL.
    *   [Oracle Access Governance LiveLabs Gruppe 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die Identitätsdomain **accessgov\_iam** ausgewählt ist.
3.  Melden Sie sich bei **Oracle Access Governance** als **Mitarbeiterbenutzer** mit einem durch die Anweisung LiveLabs angegebenen Benutzernamen und Kennwort an. **Bitte beachten Sie, dass sich der Benutzername im LiveLabs-Schritt-Screenshot möglicherweise vom Benutzernamen unterscheidet, den Sie erhalten haben.** ![Access Governance-Anmeldung](images/ag-logon.png)
4.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/ag-homepage.png)

## Aufgabe 2: Zugriffsprüfungsaufgabe ausführen (Mitarbeiterbenutzerprüfung)

1.  Wählen Sie eine der Kacheln für Zugriffsprüfungsaufgaben aus. Klicken Sie in dieser Übung auf die Schaltfläche **Auswählen** der Kachel **Ich fühle mich ehrgeizig, lassen Sie uns alle prüfen...** ![Zugriffsprüfung - Aufgaben](images/open-menu-review.png)
2.  Eine Liste der **Zugriffsprüfungsaufgaben** wird Ihnen über die **Zugriffsprüfungskampagnen** angezeigt, die in der ersten Übung geplant sind. Sie können nach den Zugriffsprüfungsaufgaben suchen, die aus der ersten Übung erstellt wurden, basierend auf dem Wert der **Prüfquelle** (**Kampagnenname**) in der mittleren Spalte der Tabelle, den Sie in **Übung 1** notieren. Falls die **Kampagne** aus der ersten Übung noch nicht gestartet wurde, können Sie auch eine **Prüfaufgabe** aus einer vorkonfigurierten Kampagne auswählen. In diesem Fall wählen Sie die Zugriffsprüfungsaufgaben mit **Quelle prüfen** als **... Beispiel für die Prüfung des Organisationszugriffs**. Beispiel: **Beispiel für die Prüfung des Organisationszugriffs**. Um den Zugriff zu überprüfen, führen Sie die folgenden Schritte aus:
    *   Prüfen Sie die Prüfungsaufgabeninformationen, wie **Identitätsname**, **Zuweisungsname**, **Managername**, **Zuweisungstyp** und **Fälligkeitstage**, für die die Aufgabe ausgelöst wird.
    *   Filtern Sie die Prüfungsaufgabenliste, indem Sie **Empfehlen Sie "Akzeptieren"** oder **Prüfung empfehlen** auswählen. Basierend auf **präskriptiven Analysen** basierend auf dem ML-Algorithmus empfiehlt **Oracle Access Governance** Aktionen für jedes Prüfelement basierend auf berechneten Risikoscores und Analysen.
    *   Sie können das Prüfelement akzeptieren, indem Sie in der Spalte **Aktionen** auf **Akzeptieren** klicken. Diese Aktion wird nur für Elemente vom Typ **Empfehlen Sie "Akzeptieren"** empfohlen.
    *   Wenn Sie die analytischen Insights anzeigen möchten, insbesondere für Elemente, die als **Prüfung empfehlen** gekennzeichnet sind, können Sie auf die Spalte **Anzeigen** in **Insights** klicken, um eine Aufgabe zu prüfen. ![Zugriffsprüfung - Aufgaben](images/select-review-recommended.png) Insights umfassen:
    *   KI-/ML-gestützte Einblicke mit **Ausrichtungsscore** verwenden KI/ML-**Peergruppenanalyse**, die von **Oracle Access Governance** durchgeführt wird, um dieses Element für **Prüfen** oder **Akzeptieren** zu empfehlen
    *   Beschreibung der Prüfungsaufgabe
    *   Access-Review-Trail
    *   Aktuelle Änderungen im Benutzerprofil ![Zugriffsprüfung - Aufgaben](images/review-insight-analytics.png)
3.  Decide (Accept or Revoke): Review all insights and select to **accept** or **revoke** this access privilege. In this lab, you may pick one access review with **Recommend Review**, view the detail, and **Accept** it. Enter **justification** for why you accept this access review item, which will be logged in **Access review trail**. **Accept** the review task item will trigger the **current review task** assigned to the second-level reviewer, which is the **user manager** in the next task. On the contrary, **revoke** access by an **employee user** will not trigger next-level access review by the **manager user**.  
    ![Zugriffsprüfung - Aufgaben](images/revoke-accept-with-insights.png)
4.  Massenaktion basierend auf Empfehlung: Sie können auch mehrere Prüfungsaufgaben auswählen und diese Berechtigungen akzeptieren oder entziehen. Beispiel: Wenn Sie den Filter **Empfehlen Sie "Akzeptieren"** auswählen, wird eine Liste der Zugriffsprüfungselemente zurückgegeben, die von **Oracle Access Governance** für **Akzeptieren** basierend auf **rezeptiven Analysen** empfohlen werden. ![Zugriffsprüfung](images/bulk-review.png)
5.  Massenauswahl: Wählen Sie alle **Empfohlene Annahme**\-Elemente aus, und klicken Sie auf die Schaltfläche **Akzeptieren**. Wählen Sie auch alle **Recommend Review**\-Elemente aus, und klicken Sie dann auf die Schaltfläche **Revoke**. ![Zugriffsprüfung - Aufgaben](images/bulk-review-selection.png)
6.  Massenaktion mit Begründung: Geben Sie eine Begründung für **Akzeptieren** oder **Entziehen** an, und klicken Sie dann auf **Weiterleiten**. ![Zugriffsprüfung - Aufgaben](images/bulk-accept-justification.png)

## Aufgabe 3: Oracle Access Governance als Benutzermanager anmelden

1.  Öffnen Sie den Chrome-Browser, und gehen Sie basierend auf Ihrer **Gruppenzuweisung** zu der **Oracle Access Governance**\-URL.
    *   [Oracle Access Governance LiveLabs Gruppe 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die Identitätsdomain **accessgov\_iam** ausgewählt ist.
3.  Melden Sie sich bei **Oracle Access Governance** als **Managerbenutzer** mit einem durch die Anweisung LiveLabs angegebenen Benutzernamen und Kennwort an. **Bitte beachten Sie, dass sich der Benutzername im LiveLabs-Schritt-Screenshot möglicherweise vom Benutzernamen unterscheidet, den Sie erhalten haben.** ![Access Governance-Anmeldung](images/ag-logon.png)
4.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/ag-homepage.png)

## Aufgabe 4: Zugriffsprüfungsaufgabe ausführen (Benutzermanagerprüfung)

1.  In dieser Übung ist der Benutzermanager der Prüfer der zweiten Ebene. Als Benutzermanager werden die Zugriffsprüfungselemente angezeigt, die Ihre Mitarbeiterbenutzer in der vorherigen Aufgabe akzeptiert haben. Klicken Sie auf die Schaltfläche **Auswählen** der Kachel **Ich fühle mich ehrgeizig, lassen Sie uns alle überprüfen...**. Alternativ können Sie auch auf die Schaltfläche **Auswählen** für die Kachel **Ich bin beschäftigt, lass uns nur prüfen...** klicken, um nur Elemente mit **hohem Risiko** zu prüfen. ![Alternativer Bildtext](images/open-menu-manager-review.png)
2.  Es wird eine Liste der Zugriffsprüfungsaufgaben angezeigt, die Ihnen über Zugriffsprüfungskampagnen oder die Ergebnisse der Zugriffsprüfung Ihres Mitarbeiters zugewiesen wurden. Sie sind der Prüfer der zweiten Ebene als Manager. Durchsuchen Sie die Prüfungsaufgabe, die von **Aufgabe 2: Zugriffsprüfungsaufgabe (Mitarbeiterbenutzerprüfung)** ausgelöst wird, nach **Identitätsname** und **Arbeitsstellenname**. Sie können die Liste auch eingrenzen, indem Sie die Zugriffsprüfungsaufgaben basierend auf dem Wert **Prüfquelle** (**Kampagnenname**) in der mittleren Spalte der Tabelle durchsuchen. Für Prüfungsaufgaben:
    *   Prüfen Sie die Prüfungsaufgabeninformationen, wie **Identitätsname**, **Zuweisungsname**, **Managername**, **Zuweisungstyp** und **Fälligkeitstage**, für die die Aufgabe ausgelöst wird.
    *   Filtern Sie die Prüfungsaufgabenliste, indem Sie **Empfehlen Sie "Akzeptieren"** oder **Prüfung empfehlen** auswählen. Basierend auf **präskriptiven Analysen** basierend auf dem **KI/ML-Algorithmus** empfiehlt **Oracle Access Governance** eine Aktion für jedes Prüfelement basierend auf berechneten Risikoscores und Analysen.
    *   Sie können das Prüfelement akzeptieren oder widerrufen, indem Sie in der Spalte **Aktionen** auf **Akzeptieren** oder **Entziehen** klicken. Die Aktion **Akzeptieren** wird nur für Elemente vom Typ **Akzeptieren empfehlen** empfohlen.
    *   Wenn Sie die analytischen Insights anzeigen möchten, insbesondere für Artikel, die als **Prüfung empfehlen** gekennzeichnet sind, können Sie auf die Spalte **Anzeigen** in **Insights** klicken, um eine Aufgabe zu prüfen. ![Zugriffsprüfung - Aufgaben](images/access-review-manager.png) Insights umfassen:
    *   KI-/ML-gestützte Einblicke mit **Ausrichtungsscore** verwenden KI/ML-**Peergruppenanalyse**, die von **Oracle Access Governance** durchgeführt wird, um dieses Element für **Prüfen** oder **Akzeptieren** zu empfehlen
    *   Beschreibung der Prüfungsaufgabe
    *   Greifen Sie auf den Prüfpfad zu. In der vorherigen Aufgabe sollte die **Begründung** angezeigt werden, die von Ihrem Mitarbeiterselbstprüfer eingegeben wurde.
    *   Aktuelle Änderungen im Benutzerprofil ![Zugriffsprüfung - Aufgaben](images/access-review-insights-manager.png)
3.  Entscheiden (Akzeptieren oder widerrufen): Prüfen Sie alle Insights, und wählen Sie **Akzeptieren** oder **Entziehen** für diese Zugriffsberechtigung. In dieser Übung können Sie eine Zugriffsprüfung mit **Prüfung empfehlen** auswählen, die Details anzeigen und **widerrufen**. Dadurch wird der automatische Korrekturprozess im **Oracle Access Governance**\-System ausgelöst.
4.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole navigiert, um **Zugriffsprüfungsaufgaben** auszuwählen, die Ihnen als **Mitarbeiter** und **Managerbenutzer** zugewiesen sind, **rezeptive Analysen** und **Empfehlung** von **Oracle Access Governance** anzuzeigen und fundierte Entscheidungen für Prüfungsaufgaben basierend auf der **Peergruppenanalyse** und **Erfahrungen** zu treffen.**Akzeptieren** oder **Entziehen**
5.  Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Kampagne "Zugriffsprüfung ausführen"](https://docs.oracle.com/en/cloud/paas/access-governance/aarrs/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autor** - Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Zuletzt aktualisiert am/um** - Edward Lu, Oracle IAM Product Management, Oktober 2022