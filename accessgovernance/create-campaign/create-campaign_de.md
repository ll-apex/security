# Zugriffsprüfungskampagne erstellen - Selbst- und Benutzermanagerprüfung

## Einführung

Als Benutzer mit der Rolle **Kampagnenadministrator** können Sie Zugriffsprüfungskampagnen über die **Oracle Access Governance**\-Konsole erstellen. Sie können Auswahlkriterien für Zugriffsprüfungen basierend auf **Benutzern** (wer hat Zugriff), **Anwendungen** (worauf wird zugegriffen), **Berechtigungen** (welche Berechtigungen) und **Rollen** (welche Rollen).

*   Geschätzte Zeit: 15 Minuten
*   Persona: Kampagnenadministrator

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Zugriffsprüfungskampagne erstellen](videohub:1_p2m93d2k)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **Zugriffsprüfungskampagne** für Selbst- und Benutzermanagerprüfung erstellen
*   **Prüferworkflow** definieren
*   Jetzt ausführen oder **Zugriffsprüfungskampagne** planen

## Aufgabe 1: Oracle Access Governance als Access-Review-Kampagnenadministrator anmelden

1.  Öffnen Sie den Chrome-Browser, und gehen Sie basierend auf Ihrer **Gruppenzuweisung** zu der **Oracle Access Governance**\-URL.
    *   [Oracle Access Governance LiveLabs Gruppe 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Gruppe 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
2.  Stellen Sie sicher, dass die Identitätsdomain **accessgov\_iam** ausgewählt ist.
3.  Melden Sie sich bei **Oracle Access Governance** als **Kampagnenadministrator** mit einem durch die Anweisung LiveLabs angegebenen Benutzernamen und Kennwort an. **Bitte beachten Sie, dass sich der Benutzername im LiveLabs-Schritt-Screenshot möglicherweise vom Benutzernamen unterscheidet, den Sie erhalten haben.** ![Access Governance-Anmeldung](images/ag-logon.png)
4.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/ag-homepage.png)

## Aufgabe 2: Zugriffsprüfungskampagne als Kampagnenadministrator erstellen

1.  Scrollen Sie nach unten, und wählen Sie **Erstellen und Definieren einer neuen Kampagne zulassen** oder im Dropdown-Menü die Option **Neue Kampagne erstellen** aus. ![Kriterien auswählen](images/create-campaign.png)
2.  Sie können eine der 4 Dimensionen **Wer hat Zugriff?** (Benutzer), **Auf was zugreifen sie** (Anwendungen), **Welche Berechtigungen** (Berechtigung) und **Welche Rollen** (Rollen) auswählen. In dieser Übung können Sie zuerst die Kachel **Wer hat Zugriff?** (Benutzer) auswählen. ![Benutzer auswählen](images/select-dimensions.png)
3.  Sie können Benutzer nach **Organisation**, **Tätigkeitscode** oder **Standort** auswählen. In dieser Übung wählen Sie die **Organisation** aus, die Ihre Benutzer auf Basis der Laborzuweisung prüfen. Beispiel: Wählen Sie die Organisation **Support** aus. Weitere **Organisationen** in Laborzuweisungen umfassen **Personalwesen**, **Softwareentwicklung**, **Produktmanagement** und **Finanzen**. Danach klicken Sie auf **Meine Auswahl anwenden**. Dadurch kehren Sie zum Assistenten **Neue Zugriffsprüfungskampagne erstellen** zurück. ![Organisationen auswählen](images/select-users.png)
4.  Sie können eine der verbleibenden 3 Dimensionen auswählen: **Auf welche Dimensionen zugreifen sie** (Anwendungen), **Welche Berechtigungen** (Berechtigung) oder **Welche Rollen** (Rollen). In dieser Übung können Sie die Kachel **Auf welche Anwendungen zugreifen** (Anwendungen) auswählen. ![Nächste Kriterien auswählen](images/select-next.png)
5.  Sie können **Anwendungen** nach Namen auswählen. In dieser Übung können Sie die Anwendungen **Central Confluence**, **Corporate Badge** und **Corporate Laptop** auswählen und dann auf **Meine Auswahl anwenden** klicken. Dadurch kehren Sie zum Assistenten **Neue Zugriffsprüfungskampagne erstellen** zurück. ![Anwendungen auswählen](images/select-applications.png)
6.  In den Kreisdiagrammen in der oberen rechten Ecke können Sie den Geltungsbereich der ausgewählten Optionen **Benutzer**, **Anwendungen**, **Berechtigungen** und **Rollen** unter **Ausgewählte Elemente** prüfen. Prüfen Sie die von Ihnen getroffenen Auswahlen. In dieser Übung können Sie auf die Schaltfläche **Ich bin gut, zu Workflows wechseln** klicken. ![Diagramme anzeigen](images/view-charts.png)
7.  Prüfen Sie den automatisch ausgewählten Workflow und die Prüfer. Sie können diese bei Bedarf ändern. Beispiel: Wenn Sie auf **Ich wähle meinen eigenen Workflow** klicken, wird das Menü **Workflow konfigurieren** geöffnet. Für diese Übung müssen Sie den Workflow und die Prüfer möglicherweise nicht ändern. Übernehmen Sie den Standardwert, und klicken Sie auf **Weiter**. Bei diesem Setup werden Zugriffsprüfungen zuerst an den **Mitarbeiterbenutzer** geleitet. Nach der Selbstbeurteilung des Mitarbeiters wird der zweite **Benutzermanager** des Prüfers zur Genehmigung weitergeleitet. ![Standardworkflow](images/configure-workflow.png) ![Standardworkflow](images/default-workflow.png)
8.  Übernehmen Sie den Standardwert **Einmal** für das Feld **Wie oft soll diese Ausführung erfolgen?**. Sie können einen Kampagnennamen und eine Beschreibung Ihrer Wahl angeben. Beispiel: Geben Sie den Kampagnennamen als **Supportorganisation - Firmenzugriffsprüfung** ein, geben Sie die gewünschte Beschreibung ein, wählen Sie **Jetzt ausführen** aus, und klicken Sie auf die Schaltfläche **Weiter**, um die Kampagne zu planen. Beachten Sie den **Kampagnennamen** als Referenz in der nächsten Übung, um nach der neu erstellten Zugriffsprüfungskampagne zu suchen. Für Prüfer wird der **Kampagnenname** im Dashboard für Prüfungsaufgaben als **Prüfquelle** bezeichnet. ![Name Kampagne](images/name-campaign.png)
9.  Sie können die ausgewählten Kampagnenkriterien, den Workflow, Prüfer und den Ausführungsplan prüfen. Klicken Sie für diese Übung auf die Schaltfläche **Erstellen**, um eine Kampagne zu erstellen und zu planen. Die Kampagne beginnt etwa **10 Minuten** nach der Erstellung.  
    ![Diagramme anzeigen](images/summary.png)
10.  Im Abschnitt **Meine bevorstehenden Kampagnen** ist eine neu erstellte **Supportorg Corporate Access Review**\-Kampagne geplant. Es dauert etwa 10 Minuten, bis **die neu erstellte Kampagne** den Status **In Bearbeitung** erhält. ![Diagramme anzeigen](images/view-created-campaign.png)
11.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole navigiert und eine **Benutzerzugriffsprüfungskampagne** als **Kampagnenadministrator** erstellt.
12.  Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autor** - Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Zuletzt aktualisiert am/um** - Edward Lu, Oracle IAM Product Management, Oktober 2022