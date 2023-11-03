# Rollen abbauen und Kandidatenrollen analysieren

## Einführung

In dieser Übung werden die Schritte zur Durchführung von Role Mining und zur Analyse von Kandidatenrollen in OIRI erläutert.

_Geschätzte Laborzeit_: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Role Mining ausführen
*   Kandidatenrollen prüfen und analysieren

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

## Aufgabe 1: Erstellen einer Role Mining Task

1.  Klicken Sie auf der Identity Role Intelligence-Homepage in der Kachel "Neue Aufgabe starten" auf _Neue Aufgabe erstellen_. Alternativ können Sie auf das Menüsymbol "Anwendungsnavigation" klicken, auf _Alle Aufgaben_ klicken und dann oben rechts auf der Seite auf "Neue Aufgabe" klicken. Die Seite "Neue Aufgabe" zum Auswählen der Daten zum Erstellen einer neuen Role Mining-Aufgabe wird angezeigt.
    
    ![OIRI-Konsole - Klicken Sie auf "Neue Aufgabe erstellen".](images/createtask-mining.png)
    
2.  Auf der Registerkarte "Benutzer" können Sie optional eine Reihe von Filtern anwenden und eine Gruppe von Benutzern auswählen, die Sie in die Role Mining-Aufgabe aufnehmen möchten. Alle Benutzer werden standardmäßig ausgewählt, wenn keine Filter angewendet werden. Nehmen wir alle Benutzer für diesen Workshop auf.
    
    ![Registerkarte "Benutzer" - Standardmäßig alle Benutzer für diesen Workshop einschließen](images/users-mining.png)
    
3.  Klicken Sie auf die Registerkarte "Applications". Die Anwendungen werden auf dieser Registerkarte basierend auf der Benutzerauswahl auf der Registerkarte "Benutzer" aufgelistet. Wir können beobachten, dass wir eine Anwendung namens _Dokumentzugriff_ haben.
    
    ![Klicken Sie auf die Registerkarte "Applications" - "Document Access".](images/application-mining.png)
    
4.  Klicken Sie auf die Registerkarte "Berechtigungen". Die Berechtigungen werden auf dieser Registerkarte basierend auf der Benutzer- und Anwendungsauswahl auf den Registerkarten "Benutzer" und "Anwendungen" aufgelistet. Auf der Registerkarte "Berechtigungen" werden die Berechtigungen aufgeführt, die Benutzern zugewiesen wurden. In dieser Registerkarte werden nicht alle Berechtigungen in der OIRI-Datenbank aufgeführt.
    
    ![Klicken Sie auf die Registerkarte "Berechtigungen" - Listet die den Benutzern zugewiesenen Berechtigungen auf](images/entitlements-mining.png)
    
5.  Klicken Sie auf _Meine Rollen_, um die Rollen basierend auf der Benutzer-, Anwendungs- und Berechtigungsauswahl in der Role Mining-Aufgabe zu ermitteln. Das Dialogfeld _Aufgaben- und Meine-Rollen speichern_ wird mit den folgenden Optionen angezeigt:
    

*   Name: Geben Sie einen Namen für die Role Mining Task ein. Dies ist ein Pflichtfeld.
    
*   Beschreibung: Geben Sie eine Beschreibung für die Role Mining-Aufgabe ein.
    
*   Schieberegler für Feinabstimmung: Ziehen Sie, um die Anzahl der Kandidatenrollen zu minimieren oder zu maximieren. Wenn Sie den Schieberegler nach links ziehen, wird die Anzahl der Kandidatenrollen minimiert. Mit anderen Worten, mehr Benutzer erhalten die von den Rollen bereitgestellten Berechtigungen. Wenn Sie den Schieberegler nach rechts ziehen, wird die Anzahl der Kandidatenrollen maximiert. Mit anderen Worten, weniger falsch ausgerichtete Berechtigungen und Benutzer, die von den Rollen bereitgestellt werden.
    
    ![Klicken Sie auf "Mine-Rollen", um die Rollen anzuzeigen](images/mineroles-mining.png)
    
    Klicken Sie auf _Meine Rollen_, um die Role Mining-Aufgabe auszuführen und Kandidatenrollen zu ermitteln. Es wird eine Meldung angezeigt, dass eine Anforderung zur Ausführung der Aufgabe weitergeleitet wurde.
    
    ![Klicken Sie auf Mine Roles - um die Role Mining-Aufgabe auszuführen](images/submit-mining.png)
    

## Aufgabe 2: Kandidatenrollen prüfen

1.  Suchen Sie auf der Seite "Aufgaben verwalten" nach der von Ihnen weitergeleiteten Role Mining-Aufgabe.
    
2.  Klicken Sie auf _Aktualisieren_, bis der Aufgabenstatus angibt, dass er abgeschlossen wurde. Klicken Sie dann auf _Kandidatenrollen anzeigen_. Die Seite "Ergebnisse für Role Mining-Aufgabe" wird angezeigt. Auf dieser Seite enthält die Zeile oben eine Übersicht über die Ausführung der Role Mining-Aufgabe. Es gibt die Anzahl der Benutzer und Berechtigungen an, für die die Aufgabe ausgeführt wurde, und die Anzahl der Kandidatenrollen.
    
    ![Klicken Sie auf "Aktualisieren" ](images/refresh-mining.png)
    
    ![Klicken Sie auf "Kandidatenrollen anzeigen".](images/complete-mining.png)
    
3.  Die Kandidatenrollen werden im Abschnitt "Kandidatenrollen" aufgeführt. Beachten Sie die unter _Prüfung nicht gestartet_ aufgeführten Rollen.
    
4.  Klicken Sie für jede aufgelistete Kandidatenrolle auf _Rolle prüfen_, um die Seite "Kandidatenrolle prüfen und anpassen" zu öffnen, auf der Sie die Kandidatenrolle vor dem Exportieren und Veröffentlichen prüfen und ändern können. Beispiel: Klicken Sie auf die erste Kandidatenrolle mit 70 Benutzern und 1 Berechtigung.
    
    ![Klicken Sie auf "Review Role".](images/review-role-mining.png)
    
5.  Die Seite _Kandidatenrolle prüfen und anpassen_ wird angezeigt.
    
6.  Der horizontale Balken "Berechtigungen" zeigt die Anzahl der Berechtigungen, die Teil der Kandidatenrolle sind, aus der Gesamtanzahl der Berechtigungen, die in der Role Mining-Aufgabe enthalten sind. Um die Berechtigungen anzuzeigen, klicken Sie auf _Anzeigen_. Beachten Sie, dass die Berechtigung (in diesem Beispiel Vertriebsdokumentzugriff) angezeigt wird.
    
    ![Klicken Sie auf "Anzeigen", um Berechtigungen anzuzeigen](images/show-mining.png)
    
    ![Zugriffsberechtigungen anzeigen](images/show-entitlement-mining.png)
    
7.  Klicken Sie auf das Symbol "Zurück", um zur Seite "Kandidatenrolle prüfen und anpassen" zurückzukehren.
    
    ![Klicken Sie auf das Symbol "Go Back".](images/candidate-role-mining.png)
    
8.  Die horizontale Leiste "Benutzer" zeigt die Anzahl der Benutzer, die Teil der Kandidatenrolle sind, von der Gesamtanzahl der Benutzer, die in der Role Mining-Aufgabe enthalten sind. Um die Benutzer anzuzeigen, klicken Sie auf _Anzeigen_. Prüfen Sie die Benutzer, die in der Role Mining-Aufgabe enthalten sind. Klicken Sie auf das Symbol "Zurück", um zur Seite "Kandidatenrolle prüfen und anpassen" zurückzukehren.
    
    ![Klicken Sie auf "Show".](images/candidate-role-mining.png)
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering, Dezember 2022