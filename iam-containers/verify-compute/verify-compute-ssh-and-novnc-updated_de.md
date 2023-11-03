# Setup der Compute-Instanz prüfen

## Einführung

In dieser Übung erfahren Sie, wie Sie sich bei Ihrer vorab erstellten Compute-Instanz anmelden, die auf Oracle Cloud ausgeführt wird.

_Geschätzte Laborzeit_: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Compute-Instanzsetup prüfen](videohub:1_x7kr6rj0)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erfassen Sie die erforderlichen Details für die Verbindung mit Ihrer Instanz (öffentliche IP-Adresse)
*   Erfahren Sie, wie Sie mit dem Remotedesktop- oder SSH-Protokoll eine Verbindung zu Ihrer Compute-Instanz herstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Reservierung mit der Option **Auf LiveLabs ausführen** erfolgreich weitergeleitet
*   Ihre Reservierung wurde erfolgreich ausgeführt, und in der Ausgabe wird eine gültige Remote-Desktop-URL angegeben
*   Ein LiveLabs Cloud-Account und ein zugewiesenes Compartment
*   Die IP-Adresse und der Instanzname für Ihre Compute-Instanz
*   Erfolgreich bei Ihrem LiveLabs-Account angemeldet
*   Ein gültiger SSH-Schlüssel (nur für _SSH-Terminalverbindungen_)

## Aufgabe 1: Auf den grafischen Remotedesktop zugreifen

Um die Ausführung dieses Workshops zu vereinfachen, wurde Ihre VM-Instanz mit einem Remote-Grafikdesktop vorkonfiguriert, auf den Sie mit einem modernen Browser auf Ihrem Laptop oder Ihrer Workstation zugreifen können. Gehen Sie wie unten beschrieben vor, um sich anzumelden.

1.  Nachdem Ihre Instanz durch Provisioning bereitgestellt wurde, navigieren Sie zu **Meine Reservierungen**. Suchen Sie die Anforderung, die Sie in der angezeigten Liste weitergeleitet haben. (Wenn dies Ihre erste Anforderung ist, wird nur ein Element angezeigt.)
    
    ![meine Reservierung](images/my-reservations.png "meine Reservierung")
    
2.  Klicken Sie auf **Workshop starten**, nachdem das Reservierungs-Provisioning abgeschlossen wurde.
    
    ![meine Reservierung abgeschlossen](images/my-reservation-completed.png "meine Reservierung abgeschlossen")
    
3.  Klicken Sie auf **Anmeldeinformationen anzeigen** und dann auf den Link **Remote-Desktop**.
    
    ![Anmeldeinformationen](images/login-info.png "Anmeldeinformationen")
    
    Dadurch gelangen Sie mit nur einem Klick direkt zu Ihrem Remote-Desktop.
    
    ![noVNC Remote-Desktop starten](images/novnc-launch-get-started-2.png "noVNC Remote-Desktop starten ")
    
    > **Hinweis:** In seltenen Fällen wird je nach Browsertyp ein Fehler mit dem Titel **Deceptive Site Ahead** oder ähnlich angezeigt.
    
    Öffentliche IP-Adressen, die für das Provisioning von LiveLabs verwendet werden, stammen aus einem Pool wiederverwendbarer Adressen. Dieser Fehler ist darauf zurückzuführen, dass die Adresse zuvor von einer Compute-Instanz lange beendet wurde, aber nicht ordnungsgemäß gesichert, kompromittiert und gekennzeichnet wurde.
    
    Sie können dies ignorieren und fortfahren, indem Sie auf **Details** und schließlich auf **Diese unsichere Seite besuchen** klicken.
    
    ![Website-Fehlermeldung](images/novnc-deceptive-site-error.png "Website-Fehlermeldung ")
    

## Anhang 1: Mit SSH-schlüsselbasierter Authentifizierung beim Host anmelden (optional) - Pfad auswählen

Der Zugriff über das SSH-Terminal ist optional. Wählen Sie bei Bedarf einen Pfad aus den folgenden 3 Methoden aus. Wenn Sie eine LiveLab ausführen, die vollständig in einem Terminal ausgeführt werden kann, wird empfohlen, Oracle Cloud Shell (Anhang 1A) zu wählen.

Sie haben folgende Optionen:

1.  Anhang 1A: Verbindung mit Cloud Shell herstellen _(empfohlen)_
2.  Anhang 1B: Verbindung mit MAC oder einem Windows CYGWIN-Emulator herstellen
3.  Anhang 1C: Verbindung mit Putty herstellen _(Erfordert die Installation von Anwendungen auf Ihrem Computer)_

## Anhang 1A: Schlüssel in Cloud Shell hochladen und verbinden

1.  Gehen Sie zu _**Compute >> Instanzen**_, und wählen Sie die erstellte Instanz aus (vergewissern Sie sich, dass Sie das richtige Compartment auswählen).
    
2.  Um Oracle Cloud Shell zu starten, gehen Sie zu Ihrer Cloud-Konsole, und klicken Sie oben rechts auf der Seite auf das Cloud Shell-Symbol.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellopen.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellsetup.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshell.png " ")
    
3.  Klicken Sie auf das Hamburger-Symbol für Cloud Shell, und wählen Sie **Hochladen** aus, um Ihren Private Key hochzuladen. Beachten Sie, dass der Private Key keine `.pub`\-Erweiterung aufweist.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key.png " ")
    
4.  Um eine Verbindung zur Compute-Instanz herzustellen, die für Sie erstellt wurde, müssen Sie den Private Key laden. Dies ist die Hälfte des Schlüsselpaares, das _keine_ `.pub`\-Erweiterung hat. Suchen Sie die Datei auf Ihrem Rechner, und klicken Sie auf **Hochladen**, um sie zu verarbeiten.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select.png " ")
    
5.  Haben Sie Geduld, während die Schlüsseldatei in Ihr Cloud Shell-Verzeichnis hochgeladen wird. ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-2.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-3.png " ")
    
6.  Führen Sie den folgenden Befehl aus, um zu prüfen, ob der SSH-Schlüssel hochgeladen wurde. Verschieben Sie es in Ihr .ssh-Verzeichnis, und ändern Sie die Berechtigungen.
    
        <copy>
        ls
        </copy>
        
    
        mkdir ~/.ssh
        mv <<keyname>> ~/.ssh
        chmod 600 ~/.ssh/<privatekeyname>
        ls ~/.ssh
        
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-finished.png " ")
    
7.  Secure Shell in der Compute-Instanz mit Ihrem hochgeladenen Schlüsselnamen (dem Private Key).
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

Wenn Sie nicht in der Lage sind, sshin, überprüfen Sie die Tipps zur Fehlerbehebung unten.

Sie können jetzt mit der nächsten Übung fortfahren

## Anhang 1B: Verbindung über MAC oder Windows CYGWIN Emulator

Je nach Workshop müssen Sie möglicherweise über einen Secure Shell-Client (SSH) eine Verbindung zur Instanz herstellen. Wenn Sie in den nächsten Übungen angewiesen werden, Aufgaben über ein SSH-Terminal auszuführen, prüfen Sie die unten stehenden Optionen, und wählen Sie diejenige aus, die Ihren Anforderungen am besten entspricht.

1.  Gehen Sie zu _**Compute >> Instanzen**_, und wählen Sie die erstellte Instanz aus (stellen Sie sicher, dass Sie das richtige Compartment auswählen)
    
2.  Suchen Sie auf der Instanzhomepage die öffentliche IP-Adresse für Ihre Instanz.
    
3.  Öffnen Sie als opc-Benutzer einen Terminal-(MAC-) oder Cygwin-Emulator. Geben Sie bei entsprechender Aufforderung "Ja" ein.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

Sie können jetzt mit der nächsten Übung fortfahren

## Anhang 1C: Verbindung über Windows mit Putty herstellen

Unter Windows können Sie PuTTY als SSH-Client verwenden. Mit PuTTY können Windows-Benutzer über SSH und Telnet eine Verbindung zu Remote-Systemen über das Internet herstellen. SSH wird in PuTTY unterstützt, stellt eine Secure Shell bereit und verschlüsselt Informationen, bevor sie übertragen werden.

1.  Laden Sie PuTTY herunter, und installieren Sie es. [http://www.putty.org](http://www.putty.org)
    
2.  Führen Sie das Programm PuTTY aus. Gehen Sie auf Ihrem Rechner zu **All Programs > PuTTY > PuTTY**.
    
3.  Wählen Sie die folgenden Informationen aus, oder geben Sie sie ein:
    
    *   Kategorie: _Session_
    *   IP-Adresse: _Die öffentliche IP-Adresse Ihrer Serviceinstanz_
    *   Port: _22_
    *   Verbindungstyp: _SSH_
    
    ![](images/7c9e4d803ae849daa227b6684705964c.jpg " ")
    

### **Automatische Anmeldung konfigurieren**

1.  Klicken Sie im Kategorieabschnitt auf **Verbindung** und dann auf **Daten auswählen**.
    
2.  Geben Sie Ihren Benutzernamen für die automatische Anmeldung ein. Geben Sie **opc** ein.
    
    ![](images/36164be0029033be6d65f883bbf31713.jpg " ")
    

### **Private Key hinzufügen**

1.  Klicken Sie im Kategorieabschnitt auf **Authentifizierung**.
    
2.  **Klicken**, um die Private-Key-Datei zu suchen, die dem Public Key Ihrer VM entspricht. Dieser Private Key muss die Erweiterung .ppk aufweisen, damit PuTTy funktioniert.
    
3.  Wenn Sie nicht über die Erweiterung .ppk verfügen, finden Sie im [Anhang](#Appendix:TroubleshootingTips) Anweisungen zum Konvertieren Ihres Private Keys in das .ppk-Format mit PuttyGen.
    
    ![](images/df56bc989ad85f9bfad17ddb6ed6038e.jpg " ")
    

### **So speichern Sie alle Einstellungen:**

1.  **Klicken** Sie im Kategorieabschnitt auf die Session.
2.  Benennen Sie im Abschnitt "Gespeicherte Sessions" Ihre Session, z.B. EM13C-ABC, und klicken Sie auf **Speichern**.

Sie können jetzt mit der nächsten Übung fortfahren

## Anhang: Tipps zur Fehlerbehebung

Wenn während der Übung Probleme aufgetreten sind, führen Sie die folgenden Schritte aus, um sie zu beheben. Wenn Sie das Problem nicht beheben können, springen Sie bitte zum Abschnitt **Hilfe benötigen**, um Ihr Problem über unser Hilfeforum zu übermitteln.

### Problem 1: Anmeldung bei Instanz nicht möglich

Teilnehmer kann sich nicht bei Instanz anmelden

#### Tipps zur Behebung des Problems #1

Es kann mehrere Gründe geben, warum Sie sich nicht bei der Instanz anmelden können. Hier sind einige häufige, die wir von den Workshop-Teilnehmern gesehen haben

*   Berechtigungen sind zu offen für den Private Key. Achten Sie darauf, die Datei mit `chmod 600 ~/.ssh/<yourprivatekeyname>` zu modifizieren
*   Falsch formatierter SSH-Schlüssel (siehe oben zur Korrektur)
*   Der Benutzer hat sich vom MAC-Terminal, von Putty usw. angemeldet, und die Instanz wird von einem Unternehmens-VPN blockiert (VPNs herunterfahren und versuchen, auf Cloud Shell zuzugreifen oder sie zu verwenden)
*   Falscher Name für SSH-Schlüssel angegeben (verwenden Sie nicht sshkeyname, verwenden Sie den angegebenen Schlüsselnamen)
*   @ vor opc-Benutzer platziert (@-Zeichen entfernen und im obigen Format anmelden)
*   Stellen Sie sicher, dass Sie der Benutzer "oracle" sind (geben Sie den zu prüfenden Befehl _whoami_ ein, wenn Sie nicht _sudo su - oracle_ eingeben, um zum Benutzer "oracle" zu wechseln)
*   Stellen Sie sicher, dass die Instanz ausgeführt wird (geben Sie den Befehl _ps -ef | grep oracle_ ein, um zu prüfen, ob die oracle-Prozesse ausgeführt werden)

### Problem 2: Benötigen Sie einen ppk-Schlüssel

Der Teilnehmer führt PuTTY unter Windows aus und benötigt einen privaten SSH-Schlüssel im _ppk_\-Format.

#### Tipps zur Behebung des Problems #2

Wenn Sie mit Putty eine Verbindung zu Ihrem Server herstellen möchten, müssen Sie Ihren SSH-Schlüssel in ein mit Putty kompatibles Format konvertieren. Um Ihren Schlüssel in das erforderliche .ppk-Format zu konvertieren, können Sie PuTTYgen verwenden.

[PuTTYgen herunterladen](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwigtZLx47DwAhUYKFkFHf99BmAQFjAAegQIAxAD&url=https%3A%2F%2Fwww.puttygen.com%2F&usg=AOvVaw1fagG6hM51oZWfQB_rqn2t)

Gehen Sie folgendermaßen vor, um einen Schlüssel mit PuTTYgen in das .ppk-Format zu konvertieren:

1.  Öffnen Sie PuTTYgen, gehen Sie zu **Konvertierungen**, und klicken Sie auf **Importschlüssel**. PuTTYgen zeigt ein Fenster zum Laden des Schlüssels an.
2.  Navigieren Sie zu Ihrem **SSH Private Key**, wählen Sie die Datei aus, und klicken Sie auf **Open**. Ihr SSH-Private Key befindet sich möglicherweise im Verzeichnis Users\[user\_name\].SSH.
3.  Geben Sie die mit dem Private Key verknüpfte Passphrase ein, oder lassen Sie das Feld leer, wenn keiner angegeben ist, und klicken Sie auf **OK**. _Beachten Sie, dass der Schlüsselfingerabdruck die Anzahl der Bits auf 4096 festlegt._
4.  Gehen Sie zu **Datei**, und klicken Sie dann auf **Private Key speichern**, um den Schlüssel imppk-Format zu speichern.

## Danksagungen

*   **Autor** - Rene Fontcha, LiveLabs Platform Lead, NA-Technologie
*   **Zuletzt aktualisiert am/um** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, Juni 2021