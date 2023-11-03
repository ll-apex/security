# Ereignisbasierte Zugriffsprüfung verwalten

## Einführung

Benutzer mit der Rolle **Kampagnenadministrator** können ereignisbasierte Zugriffsprüfungskampagnen mit der **Oracle Access Governance**\-Konsole erstellen und verwalten. Benutzer können den Fortschritt **fortlaufender Kampagnen** anzeigen, detaillierte Kampagnenanalyseberichte anzeigen und herunterladen, **vorherige Kampagnen** klonen, **fortlaufende Kampagnen** beenden usw.

*   Geschätzte Zeit: 10 Minuten
*   Persona: Kampagnenadministrator

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Ereignisbasierte Überprüfungskampagnen verwalten](videohub:1_1azcpenj)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Liste der **Zertifizierungskampagnen** anzeigen, für die Sie verantwortlich sind oder die Sie erstellt haben
*   Fortschritt der **Zertifizierungskampagnen** von Prüfern mit **Analytics-Einblicken** anzeigen

## Aufgabe 1: Oracle Access Governance als Kampagnenadministrator anmelden

1.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
2.  Melden Sie sich bei **Oracle Access Governance** als **Kampagnenadministrator - Pamela Green** mit dem unten angegebenen Benutzernamen und Kennwort an.

**Benutzername:** `<copy>pamela.green</copy>`

**Kennwort:** `<copy>Oracl@123456</copy>` ![Access Governance-Anmeldung](images/admin-login.png)

3.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/event-based-setup.png)

## Aufgabe 2: Kampagnen für ereignisbasierte Zugriffsprüfung aktivieren

1.  Wählen Sie im Navigationsmenü die Option "Ereignisbasierte Administration → Ereignisbasierte Einrichtung". ![Ereignisbasiertes Setup](images/event-based-setup.png)
    
2.  Jeder Eventtyp wird als Kachel mit dem Status "Aktiviert" oder "Deaktiviert" und einem Dropdown-Menü "Aktionen" angezeigt, das die Option "Details bearbeiten" oder "Details anzeigen" enthält. Wählen Sie "Bearbeiten" als Ereignistyp **Identität aktiviert** aus. ![Identität bearbeiten aktiviert](images/edit-identity-enabled.png)
    
3.  Auf dem Bildschirm "Ereignistyp konfigurieren": Verwenden Sie das Optionsfeld, um den Ereignistyp zu aktivieren. Wenn Sie eine Aufgabe mit geringem Risiko für diesen Eventtyp automatisch genehmigen möchten, wählen Sie "Ja" aus.
    
4.  Der Oracle Access Governance-Service stellt einen empfohlenen optimalen Workflow für den Ereignistyp bereit. Sie können "Speichern" auswählen, um den vorgeschlagenen Workflow zu akzeptieren.
    
5.  Im Fenster **Ereignistyp konfigurieren**. Wählen Sie **Speichern**, um die Änderungen an der Ereignistypkonfiguration beizubehalten.
    

![Abschluss aktivieren](images/enable-complete.png)

## Aufgabe 3: Benutzer in Oracle Identity Governance deaktivieren

1.  Melden Sie sich bei der Identity Self Service-Konsole an. Öffnen Sie eine Browserregisterkarte mit der folgenden URL, um auf die OIG Identity Console zuzugreifen.

**URL:** `<copy>http://oimhost.us.oracle.com:14000/identity</copy>` **Benutzername:** `<copy>xelsysadm</copy>`

**Kennwort:** `<copy>Welcome1</copy>`

![OIG-Anmeldung](images/oig-login-page.png)

2.  Das Haupt-Dashboard von **Oracle Identity Governance** sollte angezeigt werden ![OIG-Selfservice](images/self-service.png)
    
3.  Klicken Sie oben rechts auf Verwalten. Klicken Sie anschließend auf Users.Select **Anzeigename**, und geben Sie den Benutzernamen **Roger Young** ein. Das Benutzerprofil von **Roger Young** wird angezeigt.
    

![Selfservice verwalten](images/manage-self-service.png) 4. Klicken Sie auf die Schaltfläche **Deaktivieren**, um die Benutzeridentität **Roger Young** zu deaktivieren

![Benutzer deaktivieren](images/disable-user.png)

5.  Geben Sie die Begründung an, und klicken Sie auf _Weiterleiten_

![Benutzer deaktivieren](images/disable-submit.png)

6.  Aktualisieren Sie die Seite. Jetzt wird der Benutzer **Roger Young** mit dem Status "Deaktiviert" angezeigt.

![Benutzer deaktiviert](images/user-disabled.png)

## Aufgabe 4: Dataload in der OAG-Konsole ausführen

1.  Rufen Sie in der Oracle Access Governance-Konsole das Navigationsmenü auf, indem Sie das Symbol "Navigationsmenü" auswählen. Wählen Sie **Serviceadministration → Verbundene Systeme** aus.
    
    ![Daten laden](images/data-load.png)
    
2.  Wählen Sie auf dem Bildschirm **Verbundene Systeme** die Schaltfläche **Verwalten** für das verbundene Oracle Access Governance-System aus, das Sie verwalten möchten.
    
3.  Wählen Sie in der oberen rechten Ecke im Dropdown-Menü **Aktionen** die Option **Daten jetzt laden** aus. Dadurch wird ein Dataload initiiert, dessen Status Sie im **Aktivitätslog verfolgen können.** Aktualisieren Sie den Bildschirm, und warten Sie, bis der Status **Erfolgreich** lautet
    

## Aufgabe 5: Benutzer in Oracle Identity Governance aktivieren

1.  Melden Sie sich bei der Identity Self Service-Konsole an. Öffnen Sie eine Browserregisterkarte mit der folgenden URL, um auf die OIG Identity Console zuzugreifen.
    
    **Adresse:**
    
        <copy>http://oimhost.us.oracle.com:14000/identity</copy>
        
    
    **Benutzername:**
    
        <copy>xelsysadm</copy>
        
    
    **Passwort:**
    
        <copy>Welcome1</copy>
        

![OIG Anmeldeseite](images/oig-login-page.png) 2. Das Haupt-Dashboard von **Oracle Identity Governance** wird angezeigt. Klicken Sie oben rechts auf Verwalten. Klicken Sie anschließend auf Users.Select **Anzeigename**, und geben Sie den Benutzernamen **Roger Young** ein. Das Benutzerprofil von **Roger Young** lautet displayed.The. Der Benutzer hat den Status **Deaktiviert**.

![Benutzer aktivieren](images/enable-user.png) 3. Klicken Sie auf die Schaltfläche **Aktivieren**, um die Benutzeridentität **Roger Young** zu aktivieren. Geben Sie die Begründung an, und klicken Sie auf _Weiterleiten_ 4. Aktualisieren Sie die Seite. Jetzt wird angezeigt, dass der Benutzer **Roger Young** den Status **Aktiv** hat.

## Aufgabe 6: Dataload in OAG-Konsole erneut ausführen

1.  Rufen Sie in der Oracle Access Governance-Konsole das Navigationsmenü auf, indem Sie das Symbol "Navigationsmenü" auswählen. Wählen Sie **Serviceadministration → Verbundene Systeme** aus.
    
    ![Daten laden](images/data-load.png)
    
2.  Wählen Sie auf dem Bildschirm **Verbundene Systeme** die Schaltfläche **Verwalten** für das verbundene Oracle Access Governance-System aus, das Sie verwalten möchten.
    
3.  Wählen Sie in der oberen rechten Ecke im Dropdown-Menü **Aktionen** die Option **Daten jetzt laden** aus. Dadurch wird ein Dataload initiiert, dessen Status Sie im **Aktivitätslog verfolgen können.** Aktualisieren Sie den Bildschirm, und warten Sie, bis der Status **Erfolgreich** lautet
    

## Aufgabe 7: Oracle Access Governance als Kampagnenadministrator anmelden

1.  Rufen Sie in der Oracle Access Governance-Konsole das Navigationsmenü auf, indem Sie das Symbol "Navigationsmenü" auswählen. Wählen Sie **Meine Zugriffsprüfungen** aus
    
    ![Meine Zugriffsprüfung](images/my-access-review.png)
    
2.  Sie können das Zertifizierungsereignis anzeigen, für das Roger Identity gerade aktiviert wurde.
    
    ![Zugriffsprüfung akzeptieren](images/accpet-review.png)
    
3.  **Herzlichen Glückwunsch!** Sie beenden jetzt die **Praktische Übung zu Oracle Access Governance**. In diesem Workshop haben Sie gelernt, wie Sie:
    
    *   Zugriffskontrollkampagnen als **Kampagnenadministrator** erstellen
    *   Prüfen Sie die Benutzerberechtigungen für sich selbst und Ihre direkt unterstellten Mitarbeiter als **Benutzermanager**
    *   Aufgaben zur Zugriffsprüfung als **Mitarbeiterbenutzer** und **Benutzermanager** ausführen
    *   Überwachen und verwalten Sie Zugriffsprüfungskampagnen als **Kampagnenadministrator**
    *   Verwalten Sie ereignisbasierte Zugriffsprüfungskampagnen als **Kampagnenadministrator**

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [Oracle Access Governance - Access-Review-Kampagne verwalten](https://docs.oracle.com/en/cloud/paas/access-governance/kfdck/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023