# Verbinden Sie sich mit der Legacy-Anwendung Glassfish HR

## Einführung

In dieser Übung stellen wir eine Verbindung zur Legacy HR-Anwendung Glassfish her. Dazu wird ein SSH-Schlüssel generiert und für die Verbindung mit der virtuellen Maschine (VM) der Anwendung verwendet.

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Richten Sie Ordner ein, und generieren Sie ein SSH-Schlüsselpaar.
*   Stellen Sie mit SSH eine Verbindung zur VM der Glassfish HR-Anwendung her.
*   Speichern Sie die ATP TLS-Verbindungszeichenfolge in der Eigenschaftendatei des Glassfish-Anwendungsservers.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount
*   Abschluss der folgenden vorherigen Übungen: Autonomous Database-Instanz konfigurieren

## Aufgabe 1: Ordner einrichten und ein SSH-Schlüsselpaar generieren.

1.  Navigieren Sie oben rechts in der OCI-Konsole, wählen Sie das Terminalsymbol mit der Bezeichnung "cloud shell" aus, und warten Sie, bis es geladen und eingerichtet wird. Cloud Shell ist ein kostenloses browserbasiertes Terminal, auf das von der Oracle Cloud-Konsole aus zugegriffen werden kann und das Zugriff auf eine Linux-Shell mit vorab authentifizierter Oracle Cloud Infrastructure-CLI und andere nützliche Entwicklertools bietet.
    
    ![cloud shell öffnen](images/open-cloud-shell.png)
    
2.  Geben Sie die folgenden Befehle ein:
    
    *   Erstellen Sie den Anwendungsordner:
        
            <copy>mkdir myhrapp</copy> 
            
    *   Gehen Sie zu dem erstellten Ordner:
        
            <copy>cd myhrapp</copy>
            
    *   Generieren Sie das SSH-Schlüsselpaar. Drücken Sie zweimal die Eingabetaste, um die Passphrase-Option zu bestätigen und wegzulassen:
        
            <copy>ssh-keygen -b 2048 -t rsa -f myhrappkey</copy>
            
    *   Legen Sie die Berechtigungen für das SSH-Tastaturpaar fest, sodass nur Sie, der Eigentümer, sowohl Lese- als auch Schreibfunktionen haben.
        
            <copy>chmod 600 myhrappkey</copy>
            
    *   Wenn Sie aufgefordert werden, einen Passcode zu erstellen, drücken Sie erneut die Eingabetaste.
        
    *   Zeigen Sie das soeben erstellte Schlüsselpaar an:
        
            <copy>ls</copy>
            
    *   Rufen Sie den Public Key ab, und kopieren Sie ihn in eine Zwischenablage Ihrer Wahl. Dies benötigen Sie in zukünftigen Schritten:
        
            <copy>cat myhrappkey.pub</copy>
            
    *   Rufen Sie den Private Key ab und kopieren Sie ihn in eine Zwischenablage Ihrer Wahl. Dies benötigen Sie in zukünftigen Schritten:
        
            <copy>cat myhrappkey</copy>
            
3.  Minimieren Sie cloud shell. Sie werden es für zukünftige Aufgaben erneut benötigen.
    

## Aufgabe 2: Erstellen Sie die Glassfish-Anwendungsinstanz.

1.  Navigieren Sie oben links mit dem Hamburger-Menü zu **Compute**, und wählen Sie **Benutzerdefinierte Images** aus.
    
    ![Zu benutzerdefiniertem Bild navigieren](images/navigate-custom-image.png)
    
2.  Wählen Sie **Image importieren** aus.
    
    ![Importimage auswählen](images/select-import-image.png)
    
3.  Füllen Sie die entsprechenden Felder aus, wie im folgenden Bild gezeigt (verwenden Sie das Compartment Ihrer Wahl), und wählen Sie **Image importieren** aus. Verwenden Sie den folgenden Link für die **Object Storage-URL**: https://objectstorage.us-ashburn-1.oraclecloud.com/p/ba8MlTYIT2N2X\_HXjVX9WltQA3-5ZhP6J1CWOKRtWS3UKAPTyfs-RY3eXChPE-SP/n/c4u04/b/livelabsfiles/o/data-management-library-files/MyHRApp-20221201-final
    
    ![Benutzerdefiniertes Image erstellen](images/create-custom-image.png)
    
4.  Sobald das benutzerdefinierte Image verfügbar ist, navigieren Sie über das Hamburger-Menü oben rechts zu **Compute**, und wählen Sie **Instanzen** aus.
    
    ![Zu Instanzen navigieren](images/navigate-instances.png)
    
5.  Wählen Sie **Instanz erstellen** aus.
    
6.  Füllen Sie die entsprechenden Felder mit den folgenden Informationen aus:
    
    *   **Name**: myhrapp
    *   **Erstellen in Compartment**: Wählen Sie ein Compartment Ihrer Wahl aus.
    *   **Platzierung**: Wählen Sie eine Platzierung Ihrer Wahl aus.
    *   **Bild**: Wählen Sie **Image ändern** aus. Ändern Sie die Bildquelle in **Benutzerdefinierte Images**. **Stellen Sie sicher, dass Ihr Compartment mit dem Compartment identisch ist, in das Sie das benutzerdefinierte Image importiert haben**. Wählen Sie das benutzerdefinierte Glassfish-Image aus, das Sie in den vorherigen Schritten erstellt haben (dies kann manchmal etwas Zeit in Anspruch nehmen).
    *   **Ausprägung**: Verwenden Sie die angegebene Standardausprägung.
    *   **Networking**: Wählen Sie **Neues virtuelles Cloud-Netzwerk erstellen** und **Neues öffentliches Subnetz erstellen** aus. Verwenden Sie den Namen `myhrapp-vcn` und ein Compartment Ihrer Wahl. Verwenden Sie für den CIDR-Block `10.0.0.0/24`. Wählen Sie als öffentliche IP-Adresse **Öffentliche IPv4-Adresse zuweisen** aus.
    *   **SSH-Schlüssel hinzufügen**: Wählen Sie **Public Keys einfügen** aus. Kopieren Sie den von Ihnen erstellten öffentlichen SSH-Schlüssel aus der Zwischenablage, und fügen Sie ihn ein (vergewissern Sie sich, dass er der Public Key ist).
    *   **Boot-Volume**: Stellen Sie sicher, dass alle Optionen **nicht** aktiviert sind.
7.  Wählen Sie **Erstellen** aus.
    
    ![Aktive Instanz](images/instance-running.png)
    

## Aufgabe 3: Speichern Sie die ATP TLS-Verbindungszeichenfolge in der Eigenschaftendatei des Glassfish-Anwendungsservers.

1.  Öffnen Sie das Cloud Shell-Terminal. Stellen Sie sicher, dass Sie sich im Verzeichnis `myhrapp` befinden.
    
        <copy>cd myhrappkey</copy>
        
2.  Stellen Sie mit der öffentlichen IP-Adresse der Instanz eine SSH-Verbindung zur Glassfish-Anwendungsinstanz her. Wenn Sie in einem Prompt angeben müssen, ob Sie die Verbindung fortsetzen möchten, geben Sie in das Terminal **yes** (ja) ein.
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    ![SSH in Instanz](images/ssh-into-instance.png)
    
    _Hinweis:_ Wenn Sie Probleme mit Ihren Schlüsseln haben, müssen Sie in Ihrem **myhrapp**\-Verzeichnis die Lese- und Schreibberechtigungen für den Eigentümer beibehalten. Wenn dieses Problem jemals auftritt, kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihr **myhrapp**\-Verzeichnis in Cloud Shell ein, um Lese- und Schreibberechtigungen zu aktualisieren.
    
        <copy>chmod 600 myhrappkey</copy>
        
3.  Wechseln Sie in das Verzeichnis `secure-legacy-app-db-vault`.
    
        <copy>cd secure-legacy-app-db-vault</copy>
        
4.  Führen Sie das Skript `save_connection_string.sh` aus, um die TLS-Verbindungszeichenfolge in Ihrer ATP-Instanz zu speichern. Fügen Sie die Verbindungszeichenfolge ein, wenn Sie dazu aufgefordert werden.
    
        <copy>./save_connection_string.sh</copy>
        
    
    ![Verbindungszeichenfolge speichern](images/connect-string.png)
    
5.  Führen Sie das Skript `test_connection_string.sh` aus, um die Anwendungsinstanzverbindung zur ATP-Instanz zu testen. Geben Sie bei entsprechender Aufforderung Ihr ATP-ADMIN-Kennwort ein, das Sie zuvor erstellt haben. Stellen Sie sicher, dass Sie die korrekte Ausgabe erhalten, die auf eine erfolgreiche Verbindung hinweist.
    
        <copy>./test_connection_string.sh</copy>
        
    
        ...
        
        SQL> SQL> Current User is...
        SQL> 
        USER
        --------------------------------------------------------------------------------
        ADMIN
        
        SQL> 
        SQL> Success!
        
    
    ![Verbindung testen](images/test-connection.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022