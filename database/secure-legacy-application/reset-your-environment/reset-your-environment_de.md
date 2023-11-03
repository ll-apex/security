# Setzen Sie Ihre Umgebung zurück

## Einführung

In dieser Übung zeigen wir Ihnen, wie Sie Ihre Glassfish HR-Anwendung abschneiden und die ATP-Instanz beenden. Dadurch wird Ihre Umgebung wie zuvor zurückgesetzt.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Beenden Sie die Glassfish HR-Anwendung.
*   Benutzerdefiniertes Glassfish-Image löschen
*   Beenden Sie die Verfügbarkeitsinstanz.
*   Verknüpftes virtuelles Cloud-Netzwerk (VCN) löschen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Alle vorherigen Übungen im Workshop **Sichern einer Legacy-Anwendung mit Oracle Database Vault auf Oracle Autonomous Database** LiveLab abgeschlossen

_Warnung: Das Beenden von Ressourcen kann einige Minuten dauern_

## Aufgabe 1: Glassfish HR-Anwendung beenden

1.  Navigieren Sie in OCI über das Hamburger-Menü zu **Compute>Instanzen**.
    
2.  Wählen Sie die Auslassungspunkte rechts neben dem Bildschirm aus, der mit der erstellten `MyHrApp`\-Instanz verknüpft ist, und klicken Sie dann auf **Beenden**.
    
    ![Instanz beenden](images/terminate-instance.png)
    
    _Hinweis_: Wenn Sie Ihre Ressourcen nicht finden können, stellen Sie sicher, dass Sie sich im richtigen **Compartment** und der richtigen **Region** befinden.
    
3.  Aktivieren Sie das Kontrollkästchen **Anhängtes Boot-Volume endgültig löschen**, und wählen Sie **Instanz beenden** aus.
    
    ![Boot-Volume trennen](images/detach-boot.png)
    

## Aufgabe 2: Benutzerdefiniertes Glassfish-Image löschen

1.  Navigieren Sie mit dem Hamburger-Menü zu **Compute > Benutzerdefinierte Images**.
    
2.  Wählen Sie die Auslassungspunkte rechts neben dem Bildschirm aus, der mit dem von Ihnen erstellten benutzerdefinierten Bild verknüpft ist, und klicken Sie dann auf **Löschen**.
    
    ![Benutzerdefiniertes Image löschen](images/delete-image.png)
    

## Aufgabe 3: Verfügbarkeitsinstanz beenden

1.  Navigieren Sie mit dem Hamburger-Menü zu **Oracle Database>Autonomous Database**.
    
2.  Wählen Sie die Auslassungspunkte rechts neben dem Bildschirm aus, der mit der Datenbank verknüpft ist, die Sie für `MyHrApp` erstellt haben, und klicken Sie dann auf **Beenden**.
    
    ![Datenbank beenden](images/terminate-db.png)
    
3.  Geben Sie den **Datenbanknamen** ein, den Sie Ihrem ATP angegeben haben, und wählen Sie **Autonomous Database beenden** aus.
    
    ![DB-Namen eingeben](images/db-name.png)
    

## Aufgabe 4: Verknüpftes virtuelles Cloud-Netzwerk (VCN) löschen

1.  Navigieren Sie mit dem Hamburger-Menü zu **Networking>Virtuelle Cloud-Netzwerke**.
    
2.  Wählen Sie die Auslassungspunkte rechts auf dem Bildschirm aus, der mit dem virtuellen Cloud-Netzwerk verknüpft ist, das Sie für den Workshop erstellt haben, und **Löschen**.
    
    ![vcn löschen](images/delete-vcn.png)
    

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022