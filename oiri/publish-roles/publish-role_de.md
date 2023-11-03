# Rolle in OIG veröffentlichen

## Einführung

In dieser Übung veröffentlichen wir Rollen aus OIRI in OIG.

_Geschätzte Laborzeit_: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Rollen aus OIRI in OIG veröffentlichen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Kubernetes-Cluster bereitstellen und OIG-Server starten
    *   Übung: OIRI auf dem lokalen Kubernetes-Knoten bereitstellen
    *   Übung: Daten aus OIG in OIRI importieren
    *   Labor: Role Mining

## Aufgabe 1: Kandidatenrolle veröffentlichen

1.  Klicken Sie auf der Seite "Kandidatenrolle prüfen und anpassen" auf _Looks Good! Veröffentlichen Sie die Rolle_. Das Dialogfeld "Rolle veröffentlichen" wird angezeigt.
    
    ![OIRI-Konsole - Klicken Sie auf Looks Good! Rolle veröffentlichen](images/publish-role.png)
    
2.  Geben Sie im Feld "Kandidatenrollenname" einen Namen (z.B. GlobalSales) für die Kandidatenrolle ein. Dies ist ein Pflichtfeld.
    
    ![Feld "Kandidatenrollenname" eingeben](images/candidate-role.png)
    
3.  Klicken Sie auf _Veröffentlichen_.
    
4.  Kehren Sie zur Identity Role Intelligence-Homepage zurück, und klicken Sie in der Kachel _Aufgaben und Rollen explorieren_ auf _Veröffentlichte Rollen_.
    
    ![Klicken Sie auf "Veröffentlichte Rollen".](images/published-role.png)
    
5.  Klicken Sie auf die Rolle, die Sie prüfen möchten. Alternativ können Sie auf das Symbol "Rolle anzeigen" rechts klicken. Die Seite "Rollendetails" wird angezeigt.
    
    ![Klicken Sie auf das Symbol "View Role".](images/details-role.png)
    
6.  Klicken Sie auf die Registerkarte "Info". Auf dieser Registerkarte werden die Rolleninformationen wie Rollenname, Beschreibung sowie die Anzahl der Benutzer, Anwendungen und Berechtigungen in der Rolle angezeigt.
    
    ![Klicken Sie auf die Registerkarte "Info".](images/info-role.png)
    
7.  Klicken Sie auf die Registerkarte "Benutzer". Die Liste der Benutzer in der Rolle wird angezeigt. Mit dem Suchfeld können Sie nach bestimmten Benutzern suchen.
    
    ![Klicken Sie auf die Registerkarte "Benutzer".](images/user-publish-role.png)
    
8.  Klicken Sie auf die Registerkarte "Applications". Die Liste der Anwendungen in der Rolle wird angezeigt.
    
    ![Klicken Sie auf die Registerkarte "Applications".](images/application-publish-role.png)
    
9.  Klicken Sie auf die Registerkarte "Berechtigungen". Die Liste der Berechtigungen in der Rolle zusammen mit den zugehörigen Anwendungen wird angezeigt. Sie können die Berechtigungen nach Anspruchsname oder Anwendungsname filtern und mit dem Suchfeld nach bestimmten Berechtigungen suchen.
    
    ![Klicken Sie auf die Registerkarte "Entitys".](images/entitlement-publish-role.png)
    

## Aufgabe 2: Veröffentlichte Rolle in OIG prüfen

1.  Melden Sie sich bei der Identity Self Service-Konsole an. Öffnen Sie eine Browserregisterkarte mit der folgenden URL, um auf die _OIG-Identitätskonsole_ zuzugreifen.
    
        URL        http://oiri.livelabs.oraclevcn.com:14000/identity
        Username   Xelsysadm
        Password   Welcome1
        
2.  Klicken Sie auf "Self Service". Die Selfservice-Homepage wird angezeigt.
    
3.  Klicken Sie auf das Feld "Ausstehende Genehmigungen". Die Seite "Ausstehende Genehmigungen" wird angezeigt. Beachten Sie, dass die Genehmigungsanforderung für die veröffentlichte Rolle (GlobalSales) angezeigt wird.
    
    ![OIG Identity-Konsole - Homepage](images/oig-publish-role.png)
    
4.  Klicken Sie auf die Genehmigungsanforderung. Auf der Seite mit den Aufgabendetails wird eine detaillierte Ansicht der Anforderung im Abschnitt "Details", im Abschnitt "Übersichtsinformationen", in der Registerkarte "Anforderungsdetails", in der Registerkarte "Genehmigungen" und im Abschnitt "Warenkorbartikel" angezeigt. Es ermöglicht die vollständige Verwaltung der aufgelisteten Aufgabe. Klicken Sie auf _Genehmigen_.
    
    ![Klicken Sie auf "Ausstehende Genehmigungen".](images/approval-publish-role.png)
    
    ![Klicken Sie auf "Genehmigen".](images/approve-publish-role.png)
    
5.  Eine Standardgenehmigung auf Anforderungsebene wird generiert. Klicken Sie auf die Genehmigungsanforderung und dann auf _Genehmigen_. Die Aufgabe ist jetzt genehmigt und wird nicht mehr angezeigt.
    
    ![Standardgenehmigung auf Anforderungsebene wird generiert](images/request-publish-role.png)
    
    ![Klicken Sie auf "Genehmigen".](images/requestapprove-publish-role.png)
    
6.  Klicken Sie auf das Aktualisierungssymbol, und beachten Sie, dass für jeden Benutzer, der Teil der veröffentlichten Rolle war, eine Standardgenehmigungsanforderung generiert wird.
    
    ![Klicken Sie auf das Symbol "Aktualisieren".](images/refresh-publish-role.png)
    
7.  Wählen Sie alle Anforderungen aus, und klicken Sie auf _Aktionen_, und wählen Sie _Genehmigen_ aus. Geben Sie die entsprechenden Kommentare ein, und klicken Sie auf _OK_.
    
    _Hinweis: Um alle Anforderungen auszuwählen, wählen Sie eine beliebige Anforderung aus, und drücken Sie dann ctrl+A_
    
    ![Klicken Sie auf "Aktionen", und wählen Sie "Genehmigen" aus](images/select-publish-role.png)
    
    ![Geben Sie Kommentare ein, und klicken Sie auf "OK".](images/selectall-publish-role.png)
    
8.  Klicken Sie auf das Aktualisierungssymbol, und stellen Sie sicher, dass keine ausstehenden Genehmigungen vorhanden sind.
    
9.  Klicken Sie oben rechts auf Verwalten. Klicken Sie anschließend auf _Rollen und Zugriffs-Policys_, und wählen Sie _Rollen_ aus.
    
    ![Klicken Sie auf Rollen und Zugriffs-Policys. Wählen Sie "Rollen" aus](images/manage-publish-role.png)
    
10.  Beachten Sie, dass die Rolle (GlobalSales) in OIG veröffentlicht wurde.
    
    ![Rolle in OIG veröffentlicht](images/role-publish-role.png)
    
11.  Klicken Sie auf die Rolle, um die der Rolle zugeordneten Mitglieder und Zugriffs-Policy zu prüfen.
    
    ![Mit der Rolle verknüpfte Mitglieder prüfen](images/member-publish-role.png)
    
    ![Mit der Rolle verknüpfte Zugriffs-Policy prüfen](images/accesspolicy-publish-role.png)
    

## **Übersicht**

Sie haben nun den OIRI-Workshop abgeschlossen. In diesem Workshop haben wir gelernt:

*   Deployment von OIRI
*   So importieren Sie Entitydaten aus der OIG-Datenbank in OIRI
*   So führen Sie Role Mining-Aufgaben aus, um Kandidatenrollen zu ermitteln
*   So analysieren Sie Kandidatenrollen und prüfen Kandidatenrollen mithilfe der in OIRI bereitgestellten Analysen
*   So veröffentlichen Sie die Kandidatenrolle in OIG

Aus dem in diesem Workshop gewonnenen Wissen können Sie nun OIRI verwenden, um Folgendes zu implementieren:

*   Automatisierung der Rollenerkennung und -bereitstellung, um den fehleranfälligen und manuellen Prozess der Rollenerstellung zu vermeiden
*   Vorhandenen RBAC optimieren
*   Was-wäre-wenn-Analysen, die für Fusionen, Übernahmen oder das Onboarding neuer Anwendungen nützlich sind

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering, Dezember 2022