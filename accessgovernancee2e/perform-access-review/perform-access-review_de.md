# Zugriffsprüfungsaufgabe ausführen

## Einführung

**Zugriffsprüfungen** können von Benutzern mit den folgenden Rollen über die **Oracle Access Governance**\-Konsole ausgeführt werden. Diese Rollen basieren auf Datenattributen, die aus dem verbundenen System abgeleitet sind:

*   **Benutzer** (Zugriff prüfen, der mir selbst zugewiesen ist)
*   **Manager** (Zugriff prüfen, der Benutzern in meinem Team zugewiesen ist)
*   **Eigentümer** (Zugriff prüfen, der Benutzern für Ressourcen zugewiesen ist, für die ich verantwortlich bin)

Basierend auf dem Workflowsetup in der ersten Übung **Zugriffsprüfungskampagne erstellen** verteilt **Oracle Access Governance** Zugriffsprüfungen an die entsprechenden Prüfer in der ausgewählten Organisation. In dieser Übung ist **Mitarbeiterbenutzer** der Prüfer der ersten Ebene und **Benutzermanager** der Prüfer der zweiten Ebene. Durch die Nutzung der in Zugriffsprüfungen eingebetteten **präskriptiven Analysen** und **Informationen** können Mitarbeiter und Benutzermanager fundierte Entscheidungen über Zugriffsberechtigungen treffen. Benutzer können Artikel mit geringem Risiko basierend auf **AI/ML-Empfehlungen** aus dem System auch global genehmigen.

*   Geschätzte Zeit: 20 Minuten
*   Persona: Mitarbeiter und Benutzermanager

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Zugriffsprüfung ausführen](videohub:1_jteb4r9y)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Akzeptieren oder widerrufen Sie die mir zugewiesene **Zugriffsprüfungsaufgabe** aus der **Zertifizierungskampagne** als **Mitarbeiterbenutzer**
*   Akzeptieren oder entziehen Sie die mir zugewiesene **Zugriffsprüfungsaufgabe** aus der **Zertifizierungskampagne** als **Benutzermanager**

## Aufgabe 1: Oracle Access Governance als Mitarbeiterbenutzer anmelden

1.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
    
2.  Melden Sie sich bei **Oracle Access Governance** als **Mitarbeiterbenutzer - Mark Hernandez** mit dem unten angegebenen Benutzernamen und Kennwort an.
    
    **Benutzername:**
    
        <copy>mhernandez</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        
    
    ![Access Governance-Anmeldung](images/user-ag-logon.png)
    
3.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Access Governance-Homepage](images/user-ag-homepage.png)
    

## Aufgabe 2: Zugriffsprüfungsaufgabe ausführen (Mitarbeiterbenutzerprüfung)

1.  Wählen Sie eine der Kacheln für Zugriffsprüfungsaufgaben aus. Klicken Sie in dieser Übung auf die Schaltfläche **Auswählen** der Kachel **Ich fühle mich ehrgeizig, lassen Sie uns alle prüfen...** ![Zugriffsprüfung - Aufgaben](images/user-ag-homepage.png)
    
2.  Eine Liste der **Zugriffsprüfungsaufgaben** wird Ihnen über die **Zugriffsprüfungskampagnen** angezeigt, die in der ersten Übung geplant sind. Sie können nach den Zugriffsprüfungsaufgaben suchen, die aus der ersten Übung erstellt wurden, basierend auf dem Wert der **Prüfquelle** (**Kampagnenname**) in der mittleren Spalte der Tabelle, den Sie in **Übung 1** notieren. Falls die **Kampagne** aus der ersten Übung noch nicht gestartet wurde, können Sie auch eine **Prüfaufgabe** aus einer vorkonfigurierten Kampagne auswählen. In diesem Fall wählen Sie die Zugriffsprüfungsaufgaben mit **Quelle prüfen** als **... Beispiel für eine Organisationsüberprüfung**, z.B. **Beispiel für Organisationszugriffsprüfung**. Um den Zugriff zu überprüfen, führen Sie die folgenden Schritte aus:
    
    *   Prüfen Sie die Prüfungsaufgabeninformationen, wie **Identitätsname**, **Zuweisungsname**, **Managername**, **Zuweisungstyp** und **Fälligkeitstage**, für die die Aufgabe ausgelöst wird.
    *   Filtern Sie die Prüfungsaufgabenliste, indem Sie **Empfehlen Sie "Akzeptieren"** oder **Prüfung empfehlen** auswählen. Basierend auf **präskriptiven Analysen** basierend auf dem ML-Algorithmus empfiehlt **Oracle Access Governance** Aktionen für jedes Prüfelement basierend auf berechneten Risikoscores und Analysen.
    *   Sie können das Prüfelement akzeptieren, indem Sie in der Spalte **Aktionen** auf **Akzeptieren** klicken. Diese Aktion wird nur für Elemente vom Typ **Empfehlen Sie "Akzeptieren"** empfohlen.
    *   Wenn Sie die analytischen Insights anzeigen möchten, insbesondere für Elemente, die als **Prüfung empfehlen** gekennzeichnet sind, können Sie auf die Spalte **Anzeigen** in **Insights** klicken, um eine Aufgabe zu prüfen. ![Zugriffsprüfung - Aufgaben auswählen - Prüfung](images/user-select-review-recommended.png) Insights umfassen:
    *   KI-/ML-gestützte Einblicke mit **Ausrichtungsscore** verwenden KI/ML-**Peergruppenanalyse**, die von **Oracle Access Governance** durchgeführt wird, um dieses Element für **Prüfen** oder **Akzeptieren** zu empfehlen
    *   Beschreibung der Prüfungsaufgabe
    *   Access-Review-Trail
    *   Aktuelle Änderungen im Benutzerprofil ![Zugriff auf Prüfungsaufgaben Insights KI/ML](images/user-review-insight-analytics.png)
3.  Entscheiden (Akzeptieren oder widerrufen): Wählen Sie alle Zugriffsprüfungen aus. Klicken Sie in der Spalte **Aktionen** auf die Schaltfläche **Akzeptieren**. Geben Sie **Begründung** ein, warum Sie alle Zugriffsprüfungselemente akzeptieren, und klicken Sie auf **Weiterleiten**.
    

**Hinweis:** In diesem Übungsbeispiel werden alle _Zugriffsprüfungen_ akzeptiert. Sie können jedoch alle Insights prüfen und diese Zugriffsberechtigung **akzeptieren** oder **entziehen**. **Akzeptieren** Das Aufgabenelement "Prüfung" löst die **aktuelle Prüfungsaufgabe** aus, die dem Prüfer der zweiten Ebene zugewiesen ist. Dies ist der **Benutzermanager** in der nächsten Aufgabe. Im Gegenteil, **Entziehen** des Zugriffs durch einen **Mitarbeiterbenutzer** löst keine Zugriffsprüfung der nächsten Ebene durch den **Managerbenutzer** aus.

![Empfehlung zur Benutzerauswahl](images/user-select-review-recommended.png)

![Begründung für Massenannahme](images/user-bulk-accept-justification.png)

## Aufgabe 3: Oracle Access Governance als Benutzermanager anmelden

1.  Wenn Sie sich weiterhin als Benutzer aus der vorherigen Übung anmelden, müssen Sie sich abmelden und erneut anmelden. Stellen Sie sicher, dass die **ag-domain**\-Identitätsdomain ausgewählt ist.
    
2.  Melden Sie sich bei **Oracle Access Governance** als **Managerbenutzer - Harlan Bullard** mit dem unten angegebenen Benutzernamen und Kennwort an.
    

**Benutzername:** `<copy>harlan.bullard</copy>`

**Kennwort:** `<copy>Oracl@123456</copy>`

    ![Manager Access Governance Login](images/manager-ag-logon.png)
    

3.  Das Haupt-Dashboard von **Oracle Access Governance** sollte angezeigt werden. **Beachten Sie, dass sich die Daten im Oracle Access Governance-Haupt-Dashboard in Ihrem zugewiesenen System möglicherweise von dem LiveLabs-Schritt-Screenshot unterscheiden.** ![Manager Access Governance-Homepage](images/manager-ag-homepage.png)

## Aufgabe 4: Zugriffsprüfungsaufgabe ausführen (Benutzermanagerprüfung)

1.  In dieser Übung ist der Benutzermanager der Prüfer der zweiten Ebene. Als Benutzermanager werden die Zugriffsprüfungselemente angezeigt, die Ihre Mitarbeiterbenutzer in der vorherigen Aufgabe akzeptiert haben. Klicken Sie auf die Schaltfläche **Auswählen** der Kachel **Ich fühle mich ehrgeizig, lassen Sie uns alle überprüfen...**. Alternativ können Sie auch auf die Schaltfläche **Auswählen** für die Kachel **Ich bin beschäftigt, lass uns nur prüfen...** klicken, um nur Elemente mit **hohem Risiko** zu prüfen. ![Prüfung des Menüs "Manager öffnen"](images/manager-open-menu-manager-review.png)
    
2.  Es wird eine Liste der Zugriffsprüfungsaufgaben angezeigt, die Ihnen über Zugriffsprüfungskampagnen oder die Ergebnisse der Zugriffsprüfung Ihres Mitarbeiters zugewiesen wurden. Sie sind der Prüfer der zweiten Ebene als Manager. Durchsuchen Sie die Prüfungsaufgabe, die von **Aufgabe 2: Zugriffsprüfungsaufgabe (Mitarbeiterbenutzerprüfung)** ausgelöst wird, nach **Identitätsname** und **Arbeitsstellenname**. Sie können die Liste auch eingrenzen, indem Sie die Zugriffsprüfungsaufgaben basierend auf dem Wert **Prüfquelle** (**Kampagnenname**) in der mittleren Spalte der Tabelle durchsuchen. Für Prüfungsaufgaben:
    
    *   Prüfen Sie die Prüfungsaufgabeninformationen, wie **Identitätsname**, **Zuweisungsname**, **Managername**, **Zuweisungstyp** und **Fälligkeitstage**, für die die Aufgabe ausgelöst wird.
    *   Filtern Sie die Prüfungsaufgabenliste, indem Sie **Empfehlen Sie "Akzeptieren"** oder **Prüfung empfehlen** auswählen. Basierend auf **präskriptiven Analysen** basierend auf dem **KI/ML-Algorithmus** empfiehlt **Oracle Access Governance** eine Aktion für jedes Prüfelement basierend auf berechneten Risikoscores und Analysen.
    *   Sie können das Prüfelement akzeptieren oder widerrufen, indem Sie in der Spalte **Aktionen** auf **Akzeptieren** oder **Entziehen** klicken. Die Aktion **Akzeptieren** wird nur für Elemente vom Typ **Akzeptieren empfehlen** empfohlen.
    *   Wenn Sie die analytischen Insights anzeigen möchten, insbesondere für Artikel, die als **Prüfung empfehlen** gekennzeichnet sind, können Sie auf die Spalte **Anzeigen** in **Insights** klicken, um eine Aufgabe zu prüfen. ![Zugriffsprüfungsmanager](images/manager-access-review-manager.png) Insights umfassen:
    *   KI-/ML-gestützte Einblicke mit **Ausrichtungsscore** verwenden KI/ML-**Peergruppenanalyse**, die von **Oracle Access Governance** durchgeführt wird, um dieses Element für **Prüfen** oder **Akzeptieren** zu empfehlen
    *   Beschreibung der Prüfungsaufgabe
    *   Greifen Sie auf den Prüfpfad zu. In der vorherigen Aufgabe sollte die **Begründung** angezeigt werden, die von Ihrem Mitarbeiterselbstprüfer eingegeben wurde.
    *   Aktuelle Änderungen im Benutzerprofil ![KI/ML-Einblicke](images/manager-access-review-insights-manager.png)
3.  Entscheiden (Akzeptieren oder widerrufen): Wählen Sie die _Zugriffsprüfung_ für den Benutzer aus. _Markieren Sie Hernandez_ mit dem _Zuweisungstyp_ als _Account_, und **widerrufen** Sie diese Zugriffsberechtigung. In dieser Übung können Sie eine Zugriffsprüfung mit **Prüfung empfehlen** auswählen, die Details anzeigen und **widerrufen**. Dadurch wird der automatische Korrekturprozess im **Oracle Access Governance**\-System ausgelöst.
    
4.  Während dieser Übung haben Sie die **Oracle Access Governance**\-Konsole navigiert, um **Zugriffsprüfungsaufgaben** auszuwählen, die Ihnen als **Mitarbeiter** und **Managerbenutzer** zugewiesen sind, **rezeptive Analysen** und **Empfehlung** von **Oracle Access Governance** anzuzeigen und fundierte Entscheidungen für Prüfungsaufgaben basierend auf der **Peergruppenanalyse** und **Erfahrungen** zu treffen.**Akzeptieren** oder **Entziehen**
    

## Aufgabe 5: Oracle Identity Governance als Systemadministrator anmelden

1.  Melden Sie sich bei der Identity Self Service-Konsole an. Öffnen Sie eine Browserregisterkarte mit der folgenden URL, um auf die OIG Identity Console zuzugreifen.
    
    **Adresse:**
    
          <copy>http://oimhost.us.oracle.com:14000/identity</copy>
        
    
    **Benutzername:**
    
         <copy>xelsysadm</copy>
        
    
    **Passwort:**
    
         <copy>Welcome1</copy>
        

![OIG Anmeldeseite](images/oig-logon.png) 2. Das Haupt-Dashboard von **Oracle Identity Governance** wird angezeigt. Klicken Sie auf **Manage -> Users**. ![OIG-Homepage](images/oig-homepage.png)

3.  Klicken Sie auf "Benutzer". Suchen Sie nach dem Benutzer **Mark Hernandez**. Beachten Sie die für den Benutzer vorhandenen **Figma**\-Anwendungsberechtigungen.

![Access Governance-Homepage](images/initial-user-details.png)

4.  Klicken Sie auf **Self Service -> Provisioning Tasks -> Manual Fulfillment**. Die Seite "Manuelle Auslieferung" wird angezeigt. Beachten Sie, dass die manuelle Auslieferungsanforderung für den Benutzer angezeigt wird. ![Liste der Provisioning-Aufgaben](images/provisioning-tasks.png)
    
5.  Klicken Sie auf die Anfrage. Auf der Seite mit den Anforderungsdetails wird eine detaillierte Ansicht der Anforderung im Abschnitt "Details", im Abschnitt "Inhalt" und im Abschnitt "Warenkorbartikel" angezeigt. Es ermöglicht die vollständige Verwaltung der aufgelisteten Aufgabe. Klicken Sie auf "Abschließen". ![Manuelle Auslieferungsanforderungen](images/manual-fulfillment.png) Die Korrektur wird jetzt mit Genehmigung und manuellem Provisioning abgeschlossen. ![Manuelle Auslieferungsanforderung abschließen](images/complete-manual-fulfillment.png)
    
6.  Zurück zum Selfservice. Klicken Sie auf "Manage -> Users". Suchen Sie nach dem Benutzer **Mark Hernandez** ähnlich wie **Aufgabe 5: Schritt 2**
    
7.  Beachten Sie, dass die **Figma**\-Anwendungsberechtigungen für den Benutzer **Mark Hernandez** entfernt wurden ![Benutzerdetails - OIG](images/user-details.png)
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Kampagne "Zugriffsprüfung ausführen"](https://docs.oracle.com/en/cloud/paas/access-governance/aarrs/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023