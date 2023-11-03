# Aufgaben zur Zugriffsprüfung ausführen

## Einführung

Zugriffsprüfungen können von Benutzern (Mark Hernandez, Harlan Bullard, Jerry Poland) über die Oracle Access Governance-Konsole durchgeführt werden und werden vom Access Governance-Administrator (Pamela Green) geprüft

*   Persona: Kampagnenprüfer und Kampagnenadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_0sz90jrj)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Zugriffsprüfungsanforderungen als Access Governance-Benutzer erstellen
*   Kampagne als Kampagnenadministrator erstellen
*   Zugriffsprüfungsanforderungen als Access Governance-Kampagnenadministrator genehmigen

## Aufgabe 1: Oracle Access Governance als Mitarbeiterbenutzer anmelden, um Zugriffsanforderungen zu erstellen

1.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Mark Hernandez mit dem unten angegebenen Benutzernamen und Kennwort.
    
    **Benutzername:**
    
        <copy>mhernandez</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

3.  Wählen Sie auf der Homepage der Oracle Access Governance-Konsole im Navigationsmenü **Meine Inhalte -> Neuen Zugriff anfordern**
    
    ![Zugriffsprüfung](images/navigate-access.png)
    
4.  Wählen Sie unter **Welchen Zugriff fordern Sie an?** die Option **Welchen Zugriff möchten?** aus.
    
    ![Zugriffsprüfung](images/which-access.png)
    
5.  Fordern Sie einen neuen Zugang an -> Wählen Sie "Ja". Klicken Sie auf **Weiter**.
    
    ![Zugriffsprüfung](images/click-yes.png)
    
6.  Wählen Sie unter "What would you like to" -> **DB-Manage-Access** aus, und klicken Sie auf **Next** (Weiter).
    

    ![Access Review](images/select-db-manage-access.png)
    

7.  Unter Für diese Anforderung benötigen wir einige weitere Informationen -> Begründung angeben.
    
    ![Zugriffsprüfung](images/provide-justification.png)
    
8.  Klicken Sie auf **Anforderung weiterleiten**
    
9.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Harlan Bullard mit dem unten angegebenen Benutzernamen und Kennwort.
    

    **Username:**
    ```
    <copy>harlan.bullard</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

10.  Wählen Sie auf der Homepage der Oracle Access Governance-Konsole im Navigationsmenü **Meine Inhalte -> Neuen Zugriff anfordern**
    
11.  Wählen Sie unter **Welchen Zugriff fordern Sie an?** die Option **Welchen Zugriff möchten?** aus.
    
12.  Fordern Sie einen neuen Zugang an -> Wählen Sie "Ja". Klicken Sie auf **Weiter**.
    
13.  Wählen Sie unter "What would you like to" -> **DB-Manage-Access** aus, und klicken Sie auf **Next** (Weiter).
    
14.  Unter Für diese Anforderung benötigen wir einige weitere Informationen -> Begründung angeben.
    
15.  Klicken Sie auf **Anforderung weiterleiten**
    
16.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Jerry Poland mit dem unten angegebenen Benutzernamen und Kennwort.
    

    **Username:**
    ```
    <copy>jerry.poland</copy>
    ```
    
    **Password:**
    ```
    <copy>Oracl@123456</copy>
    ```
    

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

17.  Wählen Sie auf der Homepage der Oracle Access Governance-Konsole im Navigationsmenü **Meine Inhalte -> Neuen Zugriff anfordern**
    
18.  Wählen Sie unter **Welchen Zugriff fordern Sie an?** die Option **Welchen Zugriff möchten?** aus.
    
19.  Fordern Sie einen neuen Zugang an -> Wählen Sie "Ja". Klicken Sie auf **Weiter**.
    
20.  Wählen Sie unter "What would you like to" -> **DB-Manage-Access** aus, und klicken Sie auf **Next** (Weiter).
    
21.  Unter Für diese Anforderung benötigen wir einige weitere Informationen -> Begründung angeben.
    
22.  Klicken Sie auf **Anforderung weiterleiten**
    

## Aufgabe 2: Oracle Access Governance als Access Governance-Administrator anmelden, um Zugriffsanforderungen zu genehmigen

1.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Pamela Green mit dem unten angegebenen Benutzernamen und Kennwort.
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

2.  Navigieren Sie zu MyStuff -> Approvals.You zeigt Anfragen von Benutzer Harlan Bulllard, Mark Hernandez und Jerry Poland für **DB-Manage-Access** an.
    
3.  Klicken Sie unter Aktionen auf Genehmigen und Genehmigen des Antrags für die Benutzer Harlan Bullard, Mark Hernandez und Jerry Poland.
    

## Aufgabe 3: Manuelle Dataloads ausführen

1.  Navigieren Sie auf der Homepage der Access Governance-Konsole zu "Service Administration -> Connected System".
    
    ![Policy erstellen](images/connected-system.png)
    
2.  Wählen Sie auf der Seite "Verbundene Systeme" das verbundene System **OAG-DB**.
    
    ![Policy erstellen](images/select-system.png)
    
3.  Klicken Sie auf Aktionen -> Daten jetzt laden. Dadurch wird ein manueller Dataload ausgeführt.
    
    ![Policy erstellen](images/load-date.png)
    
4.  Sobald der Dataload abgeschlossen ist, wird der Status als "Erfolgreich" angezeigt.
    

## Aufgabe 4: Oracle Access Governance als Access Governance-Administrator beim Erstellen einer Kampagne anmelden

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

14.  Navigieren Sie zu "Access Reviews" -> "My Access Reviews".

![Zugriffsprüfung](images/view-access-review-request.png)

15.  Es werden Anfragen der Benutzer Harlan Bulllard, Mark Hernandez und Jerry Poland für DB-Manage-Access angezeigt. Klicken Sie auf "Anzeigen", um die Insights der Anforderungen zu prüfen.

![Zugriffsprüfung](images/harlan-user.png)

![Zugriffsprüfung](images/jerry-user.png)

![Zugriffsprüfung](images/mark-user.png)

16.  Klicken Sie unter Aktionen auf Akzeptieren und akzeptieren Sie die Anfrage für die Benutzer Harlan Bullard, Mark Hernandez und Jerry Poland.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Juli 2023