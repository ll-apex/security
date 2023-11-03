# Zugriffsprüfungskampagne verwalten (Kampagnenverantwortlicher)

## Einführung

Benutzer mit der Rolle **Kampagnenadministrator** können Zugriffsprüfungskampagnen über die **Oracle Access Governance**\-Konsole überwachen und verwalten. Benutzer können den Fortschritt **fortlaufender Kampagnen** anzeigen, detaillierte Kampagnenanalyseberichte anzeigen und herunterladen, **vorherige Kampagnen** klonen, **fortlaufende Kampagnen** beenden usw.

*   Geschätzte Zeit: 10 Minuten
*   Persona: Kampagnenadministrator

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Zugriffsprüfungskampagnen verwalten](videohub:1_b19sk9f6)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Liste der **Zertifizierungskampagnen** anzeigen, für die Sie verantwortlich sind oder die Sie erstellt haben
*   Fortschritt der **Zertifizierungskampagnen** von Prüfern mit **Analytics-Einblicken** anzeigen

## Aufgabe 1: Oracle Access Governance als Kampagnenadministrator anmelden

1.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
2.  Melden Sie sich bei **Oracle Access Governance** als **Kampagnenadministrator - Pamela Green** mit einem durch die Anweisung LiveLabs angegebenen Benutzernamen und Kennwort an. **Bitte beachten Sie, dass sich der Benutzername im LiveLabs-Schritt-Screenshot möglicherweise vom Benutzernamen unterscheidet, den Sie erhalten haben.** ![Access Governance-Anmeldung](images/admin-login.png)
3.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/admin-home.png)

## Aufgabe 2: Laufende Zertifizierungskampagnen verwalten und überwachen

1.  Zugriffsprüfungskampagnen werden nach **Anstehenden Kampagnen**, **Laufenden Kampagnen** und **Vorherigen Kampagnen** organisiert. Klicken Sie auf die Schaltfläche **Auswählen** der Kachel **Meine laufenden Kampagnen anzeigen**. ![Menüüberwachung - Kampagne](images/admin-home.png)
2.  Es wird eine Liste der Kampagnen angezeigt, für die Sie verantwortlich sind oder die Sie erstellt haben, die **in Bearbeitung** sind. Wählen Sie die in dieser Übung erstellte Kampagne aus. ![Kampagnenliste anzeigen](images/admin-view-list-campaign.png)
3.  Sie können einen Einblick in den Fortschritt der Prüfer erhalten. Zeigen Sie an, wie viele **Beurteilende** dieser Kampagne zugewiesen sind. Wie viele Beurteilungselemente werden jedem Prüfer zugewiesen, und wie hoch ist der prozentuale Fortschritt? ![Fortschritt der Kampagne](images/admin-view-campaign-progress.png)
4.  Klicken Sie auf die Schaltfläche **Zusätzliche Details**, um weitere Details zur ausgewählten Kampagne anzuzeigen. ![Weitere Details zur Kampagne](images/admin-additional-details.png)
5.  Klicken Sie auf das Dropdown-Menü **Aktionen**, und wählen Sie **Bericht anzeigen** aus, um einen Bericht mit den Fortschrittsdetails der ausgewählten Kampagne anzuzeigen. Wenn der **Kampagnenfortschrittsbericht** noch nicht generiert wurde, wählen Sie eine vorherige Kampagne aus, um den **Fortschrittsbericht** anzuzeigen. ![Menü "Kampagnenstatus"](images/admin-view-campaign-progress-menu.png)
6.  Sie können sofort einsatzbereite Analysen und Berichte zum Kampagnenfortschritt anzeigen. Sie können den Bericht auch als PDF-Datei herunterladen. ![Kampagnenanalysen](images/admin-view-campaign-analytics.png)
7.  Klicken Sie auf **Schließen**, und kehren Sie zum Bildschirm mit den Kampagnendetails zurück. Klicken Sie auf das Dropdown-Menü **Aktionen**. Sie können die aktuelle Kampagne **beenden**. Sie haben auch die Möglichkeit, die aktuelle Kampagne zu **klonen**. ![Menü "Kampagnendetails"](images/admin-view-campaign-progress-menu.png)
8.  Klicken Sie auf **Klonen**, und geben Sie einen neuen Namen für die Kampagne ein. Wählen Sie **Jetzt ausführen** aus, und klicken Sie auf **Erstellen**. Sie erstellen jetzt eine neue Kampagne, indem Sie eine vorhandene Kampagne klonen. ![Kampagne kopieren](images/admin-clone-campaign.png)
9.  In dieser Übung haben Sie als **Kampagnenadministrator** die Analysefeatures von **Oracle Access Governance** genutzt, um Einblicke in den Status des Kampagnenfortschritts zu erhalten. Außerdem erfahren Sie, wie Sie schnell eine neue Kampagne erstellen, indem Sie eine vorhandene Kampagne klonen.

Sie können jetzt **mit der nächsten Übung fortfahren**

## Weitere Informationen

*   [Oracle Access Governance - Access-Review-Kampagne verwalten](https://docs.oracle.com/en/cloud/paas/access-governance/kfdck/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023