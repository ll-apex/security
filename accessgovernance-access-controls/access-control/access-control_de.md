# Access Controls für Datenbanken definieren

## Einführung

In dieser Übung prüfen wir Access Control von Access Governance.

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_ruy3z0dh)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Identity Collection erstellen
*   Genehmigungsworkflow mit parallelen Eskalationsregeln erstellen
*   Access Bundle erstellen
*   Zentralisierte Policy zum Provisioning von Zugriffsberechtigungen erstellen

## Aufgabe 1: Identity Collection für Genehmiger erstellen

1.  Klicken Sie auf der Homepage der Access Governance-Konsole auf die Registerkarte "Zugriffskontrollen". Klicken Sie anschließend in der Kachel {\\b Identity Collections} auf {\\b Select}.
    
    ![Identity Collection erstellen](images/ag-homepage.png)
    
    ![Identity Collection erstellen](images/identity-collections.png)
    
2.  Auf der Seite "Identitätssammlungen" werden Ihre erstellten Identitätssammlungen hier aufgelistet. Klicken Sie auf Identity Collection erstellen, um eine Identity Collection zu erstellen.
    
    ![Identity Collection erstellen](images/create-identity-collection.png)
    
3.  Geben Sie die unten genannten Details unter der Registerkarte "Details hinzufügen" ein:
    
    Wie soll der Name dieser Identity Collection lauten: IT-Management
    
    Wie soll man diese Sammlung beschreiben? : IT-Management
    
    Wer diese Identity Collection verwalten kann: pamela.green
    
    Möchten Sie Tags hinzufügen? : Genehmiger
    
    Klicken Sie auf _Weiter_
    
    ![Identity Collection erstellen](images/create-workflow.png)
    
4.  Wählen Sie unter Identitäten auswählen -> Eingeschlossene benannte Identitäten die unten genannte Option aus.
    
    Wählen Sie _Pamela Green_ aus
    
    Klicken Sie auf _Weiter_
    
    ![Identity Collection erstellen](images/include-identities.png)
    
5.  Klicken Sie auf _Erstellen_.
    

## Aufgabe 2: Identity Collection für Anforderer erstellen

1.  Klicken Sie auf der Homepage der Access Governance-Konsole auf die Registerkarte "Zugriffskontrollen". Klicken Sie anschließend in der Kachel {\\b Identity Collections} auf {\\b Select}.
    
2.  Auf der Seite "Identitätssammlungen" werden Ihre erstellten Identitätssammlungen hier aufgelistet. Klicken Sie auf Identity Collection erstellen, um eine Identity Collection zu erstellen.
    
    ![Identity Collection erstellen](images/create-identity-collection.png)
    
3.  Geben Sie die unten genannten Details unter der Registerkarte "Details hinzufügen" ein:
    
    Wie nennt man diese Identitäts-Collection? : QS-Team
    
    Wie soll ich diese Kollektion beschreiben? : QS Team
    
    Wer diese Identity Collection verwalten kann: pamela.green
    
    Möchten Sie Tags hinzufügen? : DB, Datenbank, Vorgänge
    
    Klicken Sie auf _Weiter_
    
    ![Identity Collection erstellen](images/qa-collection.png)
    
4.  Wählen Sie unter Identitäten auswählen -> Mitgliedschaftsregel die unten genannte Option aus.
    
    Wählen Sie das Kontrollkästchen _Alle_ aus.
    
    Attribut auswählen: Organisation
    
    Bedingung: Gleich
    
    Attributfeld: Qualitätssicherung
    
    ![Identity Collection erstellen](images/qa-rule.png)
    
5.  Klicken Sie auf _Erstellen_.
    
    ![Identity Collection erstellen](images/qa-collection-create.png)
    

## Aufgabe 3: Genehmigungsworkflow erstellen

1.  Klicken Sie auf der Homepage der Access Governance-Konsole auf die Registerkarte "Zugriffskontrollen". Klicken Sie anschließend auf der Kachel "Genehmigungsworkflows verwalten" auf "Auswählen".
    
    ![Genehmigungsworkflow](images/ag-homepage.png)
    
2.  Auf der Seite "Genehmigungsworkflows" werden Ihre erstellten Genehmigungsworkflows hier aufgelistet. Klicken Sie auf "Genehmigungsworkflow erstellen", um den ersten Genehmigungsworkflow zu erstellen.
    
    ![Genehmigungsworkflow](images/create-approval-workflow.png)
    
3.  Erstellen Sie jetzt Ihren Genehmigungsworkflow. Klicken Sie auf die Schaltfläche "+", und konfigurieren Sie Ihren Genehmigungsworkflow wie folgt:
    
    • Welcher Genehmigungstyp?: Identitätserfassung
    
    • Genehmigungsidentitätserfassung: IT-Management
    
    • Sollten alle Mitglieder zustimmen? : Nein
    
    • Übernehmen Sie für alle anderen Felder den Standardwert
    
    • Klicken Sie auf "Add".
    
    ![Genehmigungsworkflow](images/approval-workflow-id.png)
    
    ![Genehmigungsworkflow](images/approval-workflow-edit.png)
    
    Nachdem Sie bestätigt haben, dass Ihre Konfiguration den folgenden Anforderungen entspricht, klicken Sie auf "Weiter".
    
4.  Benennen Sie auf der Seite "Details hinzufügen" den Genehmigungsworkflow "Genehmigungs-Workflow-IT-Management". Geben Sie dann eine Beschreibung an. Klicken Sie auf Weiter, um Ihre bisherigen Konfigurationen zu prüfen, und klicken Sie dann auf Veröffentlichen.
    
    ![Genehmigungsworkflow](images/approval-workflow-name.png)
    
    ![Genehmigungsworkflow](images/approval-workflow-publish.png)
    

## Aufgabe 4: Zugriffs-Bundle erstellen

1.  Klicken Sie auf der Homepage der Access Governance-Konsole auf die Registerkarte "Zugriffskontrollen". Klicken Sie anschließend auf der Kachel "Access Bundles" auf "Select".
    
    ![Access Bundle erstellen](images/navigate-access-bundle.png)
    
2.  Klicken Sie auf Zugriffs-Bundle erstellen - DB Lesezugriff
    
    ![Access Bundle erstellen](images/create-access-bundle.png)
    
3.  Konfigurieren Sie das Bundle für Ihre Bundle-Einstellungen so, dass es Folgendes aufweist:
    
    • Für welches Ziel ist dieses Bundle bestimmt?: OAG-DB
    
    • Wer kann dieses Bundle anfordern: Jeder
    
    • Welcher Genehmigungsworkflow soll verwendet werden?: Approval-Workflow-IT-Management
    
    Klicken Sie auf "Next".
    
    ![Access Bundle erstellen](images/click-next.png)
    
4.  Wählen Sie die Berechtigungen aus, die in das Zugriffs-Bundle aufgenommen werden sollen.
    
    Welche Berechtigungen sind in diesem Bundle enthalten? : Wählen Sie unten aus, um in das Zugriffs-Bundle in der Liste aufgenommen zu werden.
    
    *   BELIEBIGE DATEIGRUPPE LESEN
        
    *   BELIEBIGE TABELLE LESEN
        
    
    ![Access Bundle erstellen](images/read-permissions.png)
    
    Klicken Sie auf "Next".
    
5.  Konfigurieren Sie im Schritt "Details hinzufügen" Folgendes:
    
    • Wie heißt dieses Bundle?: DB-Lesezugriff
    
    • Wie soll das Bundle beschrieben werden?: DB Lesezugriff
    
    • Authentifizierungstyp: PASSWORD
    
    • Default Tablespace: USERS
    
    • TemporaryTablespace: TEMP.
    
    • Profilname: DEFAULT
    
    • Alle anderen Optionen als Standard beibehalten
    
    ![Access Bundle erstellen](images/db-read.png)
    
    Klicken Sie dann auf "Weiter".
    
6.  Prüfen Sie Ihre bis zu diesem Zeitpunkt vorgenommenen Konfigurationen. Es sollte wie die unten dargestellten Konfigurationen aussehen, mit Ausnahme des Namens. Klicken Sie anschließend auf {\\b Create}.
    
    ![Access Bundle erstellen](images/db-read-create.png)
    
7.  Klicken Sie auf Zugriffs-Bundle erstellen - DB Zugriff verwalten
    
    ![Access Bundle erstellen](images/create-access-bundle.png)
    
8.  Konfigurieren Sie das Bundle für Ihre Bundle-Einstellungen so, dass es Folgendes aufweist:
    
    • Für welches Ziel ist dieses Bundle bestimmt?: OAG-DB
    
    • Wer kann dieses Bundle anfordern: Jeder
    
    • Welcher Genehmigungsworkflow soll verwendet werden?: Approval-Workflow-IT-Management
    
    Klicken Sie auf "Next".
    
9.  Wählen Sie die Berechtigungen aus, die in das Zugriffs-Bundle aufgenommen werden sollen.
    
    Welche Berechtigungen sind in diesem Bundle enthalten? : Wählen Sie unten aus, um in das Zugriffs-Bundle in der Liste aufgenommen zu werden.
    
    *   BELIEBIGES SQL-TUNING-SET VERWALTEN
        
    *   DATENBANKTRIGGER VERWALTEN
        
    *   VERWALTEN DER SCHLÜSSELVERWALTUNG
        
    *   RESOURCE MANAGER VERWALTEN
        
    *   SQL-MANAGEMENTOBJEKT VERWALTEN
        
    *   SQL TUNING SET VERWALTEN
        
    
    ![Access Bundle erstellen](images/administer-permission.png)
    
    Klicken Sie auf "Next".
    
10.  Konfigurieren Sie im Schritt "Details hinzufügen" Folgendes:
    
    • Wie heißt dieses Bundle?: DB Zugriff verwalten
    
    • Wie soll das Bundle beschrieben werden?: DB Zugriff verwalten
    
    • Authentifizierungstyp: PASSWORD
    
    • Default Tablespace: USERS
    
    • TemporaryTablespace: TEMP.
    
    • Profilname: DEFAULT
    
    • Alle anderen Optionen als Standard beibehalten
    
    ![Access Bundle erstellen](images/db-manage-access.png)
    
    Klicken Sie dann auf "Weiter".
    
11.  Prüfen Sie Ihre bis zu diesem Zeitpunkt vorgenommenen Konfigurationen. Es sollte wie die unten dargestellten Konfigurationen aussehen, mit Ausnahme des Namens. Klicken Sie anschließend auf {\\b Create}.
    
    ![Access Bundle erstellen](images/create-db-manage-access.png)
    

## Aufgabe 5: Policy erstellen

1.  Klicken Sie auf der Homepage der Access Governance-Konsole auf die Registerkarte "Zugriffskontrollen". Klicken Sie anschließend in der Kachel "Policys" auf "Auswählen".
    
    ![Policy erstellen](images/ag-homepage.png)
    
    ![Policy erstellen](images/navigate-policies.png)
    
2.  Auf der Seite "Policys" wird eine Liste der erstellten Policys angezeigt. Klicken Sie auf Policy erstellen.
    
    ![Policy erstellen](images/create-policy.png)
    
3.  Geben Sie einen Namen und eine Beschreibung wie die Folgende an:
    
    • Wie soll die Policy aufgerufen werden?:DB Policy
    
    • Wie soll die Policy-Beschreibung lauten: DB-Policy
    
    ![Policy erstellen](images/build-policy.png)
    
4.  Jetzt auf dieser Seite fügen wir eine Access Bundle Association hinzu. Klicken Sie unten auf der Seite auf die Schaltfläche "+", und wählen Sie Access Bundle Association aus.
    
    ![Policy erstellen](images/policy-access-bundle-association.png)
    
5.  Suchen Sie, welcher Identitäts-Collection Sie Zugriff gewähren möchten: QS-Team. Ihre Auswahl wird mit einem grünen Häkchen markiert. Klicken Sie auf "Next".
    
    ![Policy erstellen](images/select-qa.png)
    
6.  Suchen Sie nach dem Zugriffs-Bundle, das Sie zuweisen möchten: DB-Lesezugriff. Ihre Auswahl wird mit einem grünen Häkchen markiert.
    
    ![Policy erstellen](images/selet-db-read-access.png)
    
    Klicken Sie dann auf "Weiter".
    
7.  Auf der Seite "Prüfen und weiterleiten" können Sie rechts unten auf "Vorschau der Policy-Verknüpfung anzeigen" klicken, bevor Sie sie erstellen. Schließen Sie anschließend diese Randleiste, und klicken Sie auf Verknüpfung hinzufügen.
    
    ![Policy erstellen](images/click-add-association.png)
    
8.  Klicken Sie abschließend auf {\\b Create}.
    

## Aufgabe 6: Zugriffsanforderungen erstellen

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
    

## Aufgabe 7: Zugriffsanforderungen genehmigen

1.  Melden Sie sich bei Oracle Access Governance als Mitarbeiterbenutzer an - Pamela Green mit dem unten angegebenen Benutzernamen und Kennwort.
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

2.  Navigieren Sie zu MyStuff -> Approvals.You zeigt Anfragen von Benutzer Harlan Bulllard, Mark Hernandez und Jerry Poland für **DB-Manage-Access** an.
    
3.  Klicken Sie unter Aktionen auf Genehmigen und Genehmigen des Antrags für die Benutzer Harlan Bullard, Mark Hernandez und Jerry Poland.
    

## Aufgabe 8: Dataload ausführen

1.  Navigieren Sie auf der Homepage der Access Governance-Konsole zu "Service Administration -> Connected System".
    
    ![Policy erstellen](images/connected-system.png)
    
2.  Wählen Sie auf der Seite "Verbundene Systeme" das verbundene System **OAG-DB**.
    
    ![Policy erstellen](images/select-system.png)
    
3.  Klicken Sie auf Aktionen -> Daten jetzt laden. Dadurch wird ein manueller Dataload ausgeführt.
    
    ![Policy erstellen](images/load-date.png)
    
4.  Sobald der Dataload abgeschlossen ist, wird der Status als "Erfolgreich" angezeigt.
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi
*   **Mitwirkende** - Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anuj Tripathi, Oktober 2023