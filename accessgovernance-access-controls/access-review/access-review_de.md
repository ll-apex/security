# Zugriffsprüfungen für Datenbank ausführen

## Einführung

Zugriffsprüfungen können von Benutzern (Mark Hernandez, Harlan Bullard, Jerry Poland) über die Oracle Access Governance-Konsole durchgeführt werden und werden vom Access Governance-Administrator (Pamela Green) geprüft

*   Persona: Kampagnenprüfer und Kampagnenadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_0sz90jrj)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Kampagne als Kampagnenadministrator erstellen
*   Zugriffsprüfungsanforderungen als Access Governance-Kampagnenadministrator genehmigen

## Aufgabe 1: Kampagne erstellen

1.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Pamela Green mit dem unten angegebenen Benutzernamen und Kennwort.
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        
2.  Navigieren Sie zu "Zugriffsprüfungen -> Kampagnen". Klicken Sie auf **Kampagne erstellen**.
    

![Zugriffsprüfung](images/navigate-campaigns.png)

![Zugriffsprüfung](images/create-campaign.png)

3.  Unter Welchen Typ von Zugriffsprüfungskampagne möchten Sie ausführen? -> Wählen Sie **Von Access Governance verwaltete Systeme prüfen** aus.

![Zugriffsprüfung](images/access-review-ag.png)

4.  Wählen Sie **Wer hat Zugriff?** aus. Suchen Sie nach der **Qualitätssicherung** der Organisation. Klicken Sie auf **Meine Auswahl anwenden**.

![Zugriffsprüfung](images/who-has-access.png)

![Zugriffsprüfung](images/quality-assurance.png)

5.  Klicken Sie auf **Ich bin gut für den Workflow**

![Zugriffsprüfung](images/select-workflow.png)

6.  Wählen Sie **Ich wähle meinen eigenen Workflow aus**. Klicken Sie auf **Weiter**.
    
7.  Wählen Sie **Wie viele Genehmigungsebenen möchten Sie?** -> Genehmigungsworkflow mit einer Ebene
    
8.  Wählen Sie **Wer ist der erste Prüfer?** -> Eigentümer. Klicken Sie auf "Save" und dann auf "Next".
    

![Zugriffsprüfung](images/edit-workflow.png)

9.  Wählen Sie unter **Wie möchten Sie Ihre Kampagne planen?** -> **Jetzt ausführen** aus. Geben Sie **Wie möchten Sie diese Kampagne beschreiben?** an, und klicken Sie auf **Weiter**

![Zugriffsprüfung](images/create-workflow.png)

10.  Klicken Sie auf "Erstellen". Jetzt wurde die Kampagne erstellt.
    
11.  Um die erstellte Kampagne anzuzeigen, navigieren Sie zu "Zugriffsprüfungen -> Kampagnen"
    

![Zugriffsprüfung](images/campaign-name.png)

12.  Klicken Sie auf die Kampagne, um Details anzuzeigen.

![Zugriffsprüfung](images/view-campaign.png)

13.  Klicken Sie auf "Weitere Details", um weitere Informationen zur erstellten Kampagne anzuzeigen.

![Zugriffsprüfung](images/campaign-details.png)

## Aufgabe 2: Zugriff prüfen

1.  Navigieren Sie zu "Access Reviews" -> "My Access Reviews".
    
    ![Zugriffsprüfung](images/view-access-review-request.png)
    
2.  Sie sehen Zugriffsprüfungsaufgaben für Benutzer Harlan Bulllard, Mark Hernandez und Jerry Poland für DB-Manage-Access. Klicken Sie auf "Anzeigen", um die Insights der Beurteilungsaufgaben zu prüfen.
    

![Zugriffsprüfung](images/harlan-user.png)

![Zugriffsprüfung](images/jerry-user.png)

![Zugriffsprüfung](images/mark-user.png)

3.  Klicken Sie unter Aktionen auf Akzeptieren für die Anfrage der Benutzer Harlan Bullard, Mark Hernandez und Jerry Poland.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi
*   **Mitwirkende** - Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anuj Tripathi, Oktober 2023