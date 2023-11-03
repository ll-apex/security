# Zugriffsprüfungskampagne erstellen - Selbst- und Benutzermanagerprüfung

## Einführung

Als Benutzer mit der Rolle **Kampagnenadministrator** können Sie Zugriffsprüfungskampagnen über die **Oracle Access Governance**\-Konsole erstellen. Sie können Auswahlkriterien für Zugriffsprüfungen basierend auf **Benutzern** (wer hat Zugriff), **Anwendungen** (worauf wird zugegriffen), **Berechtigungen** (welche Berechtigungen) und **Rollen** (welche Rollen).

*   Geschätzte Zeit: 15 Minuten
*   Persona: Kampagnenadministrator

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Zugriffsprüfungskampagne erstellen](videohub:1_9s3mt0qx)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **Zugriffsprüfungskampagne** für Selbst- und Benutzermanagerprüfung erstellen
*   **Prüferworkflow** definieren
*   Jetzt ausführen oder **Zugriffsprüfungskampagne** planen

## Aufgabe 1: Oracle Access Governance als Access-Review-Kampagnenadministrator anmelden

1.  Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
2.  Melden Sie sich bei der **Oracle Access Governance**\-Konsole an. Informationen zur URL der **Oracle Access Governance**\-Serviceinstanz finden Sie unter _Übung 4: Aufgabe 1.4_.
3.  Melden Sie sich als **Kampagnenadministrator - Pamela Green** mit dem unten angegebenen Benutzernamen und Kennwort an.

**Benutzername:** `<copy>pamela.green</copy>`

**Kennwort:** `<copy>Oracl@123456</copy>`

![Access Governance-Anmeldung](images/admin-login.png)

4.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/admin-home.png)

## Aufgabe 2: Zugriffsprüfungskampagne als Kampagnenadministrator erstellen

1.  Klicken Sie auf das Hamburger-Menüsymbol in der oberen linken Ecke. Wählen Sie im Dropdown-Menü die Option **Kampagnen** aus. ![Kriterien auswählen](images/admin-campaign.png)
2.  Klicken Sie auf **Kampagne erstellen**. ![Kriterien auswählen](images/admin-create-campaign.png)
3.  Sie können eine der 4 Dimensionen **Wer hat Zugriff?** (Benutzer), **Auf was zugreifen sie** (Anwendungen), **Welche Berechtigungen** (Berechtigung) und **Welche Rollen** (Rollen) auswählen. In dieser Übung können Sie zuerst die Kachel **Wer hat Zugriff?** (Benutzer) auswählen. ![Benutzer auswählen](images/admin-select-dimensions.png)
4.  Sie können Benutzer nach **Organisation**, **Tätigkeitscode** oder **Standort** auswählen. In dieser Übung wählen Sie die **Organisation - Qualitätssicherung und -vorgänge** aus, die Ihre Benutzer auf Basis der Übung assignment.After prüfen. Klicken Sie auf **Meine Auswahl anwenden**. Dadurch kehren Sie zum Assistenten **Neue Zugriffsprüfungskampagne erstellen** zurück. ![Organisationen auswählen](images/select-org.png) ![Nächste Kriterien auswählen](images/admin-select-next.png)
5.  In den Kreisdiagrammen in der oberen rechten Ecke können Sie den Geltungsbereich der ausgewählten Optionen **Benutzer**, **Anwendungen**, **Berechtigungen** und **Rollen** unter **Ausgewählte Elemente** prüfen. Prüfen Sie die von Ihnen getroffenen Auswahlen. In dieser Übung können Sie auf die Schaltfläche **Ich bin gut, zu Workflows wechseln** klicken. ![Workflow auswählen](images/admin-select-next.png)
6.  Prüfen Sie den automatisch ausgewählten Workflow und die Prüfer. Sie können diese bei Bedarf ändern. Beispiel: Wenn Sie auf **Ich wähle meinen eigenen Workflow** klicken, wird das Menü **Workflow konfigurieren** geöffnet. Für diese Übung müssen Sie den Workflow und die Prüfer möglicherweise nicht ändern. Übernehmen Sie den Standardwert, und klicken Sie auf **Weiter**. Bei diesem Setup werden Zugriffsprüfungen zuerst an den **Mitarbeiterbenutzer** geleitet. Nach der Selbstbeurteilung des Mitarbeiters wird der zweite Prüfer **Benutzermanager** zur Genehmigung weitergeleitet. ![Standardworkflow](images/admin-configure-workflow.png)
7.  Übernehmen Sie den Standardwert **Einmal** für das Feld **Wie oft soll diese Ausführung erfolgen?**. Sie können einen Kampagnennamen und eine Beschreibung Ihrer Wahl angeben. Beispiel: Geben Sie den Kampagnennamen **Organisationszugriffsprüfung** ein, geben Sie die gewünschte Beschreibung ein, wählen Sie **Jetzt ausführen** aus, und klicken Sie auf die Schaltfläche **Weiter**, um die Kampagne zu planen. Beachten Sie den **Kampagnennamen** als Referenz in der nächsten Übung, um nach der neu erstellten Zugriffsprüfungskampagne zu suchen. Für Prüfer wird der **Kampagnenname** im Dashboard für Prüfungsaufgaben als **Prüfquelle** bezeichnet. ![Standardworkflow - Einmalige Ausführung](images/admin-default-workflow.png)
8.  Sie können die ausgewählten Kampagnenkriterien, den Workflow, Prüfer und den Ausführungsplan prüfen. Klicken Sie für diese Übung auf die Schaltfläche **Erstellen**, um eine Kampagne zu erstellen und zu planen. Die Kampagne beginnt etwa **10 Minuten** nach der Erstellung.  
    ![Diagramme anzeigen - Erstellen](images/admin-summary.png)
9.  Eine neu erstellte Kampagne **Organisationszugriffsprüfung** wird im Abschnitt **Meine bevorstehenden Kampagnen** geplant. Es dauert etwa 10 Minuten, bis **die neu erstellte Kampagne** den Status **In Bearbeitung** erhält. ![Diagramme anzeigen - Kampagne erstellt](images/admin-view-created-campaign.png)
10.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole navigiert und eine **Benutzerzugriffsprüfungskampagne** als **Kampagnenadministrator** erstellt.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023