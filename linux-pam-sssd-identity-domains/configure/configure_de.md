# ORM-Stack zur Konfiguration des Linux-PAM-Moduls und der Identitätsdomain bereitstellen

## Einführung

Mit diesem Stack können wir **Linux Server und Identitätsdomain** konfigurieren. Im Rahmen dieses Stacks wird eine vertrauliche Anwendung unter **Identitätsdomain** erstellt, und Beispielbenutzer werden erstellt.

_Geschätzte Zeit:_ 20 Minuten

### Ziele

*   **Linux-PAM mit SSSD** installieren und konfigurieren
*   Erstellen Sie die **Vertrauliche Anwendung** unter **Identitätsdomain**, und erstellen Sie die _POSIX_\-Beispielbenutzer und eine POSIX-Gruppe.

### Voraussetzungen

*   Nachdem die Datei **Stack2- Configure.zip** heruntergeladen wurde, verwenden Sie die aktualisierte Datei **SSH.key**, die beim Deployment von **Stack1- Deploy.zip** erstellt wurde.

## Aufgabe 1: Konfigurationsstack über Resource Manager bereitstellen

1.  Nachdem Sie sich bei der OCI-Konsole angemeldet haben, navigieren Sie zu **Entwicklerservices**, und wählen Sie unter **Resource Manager** die Option **Stacks** aus. Klicken Sie jetzt auf **Stack erstellen**
    
    ![Stapel](./images/stacks.png "Stapel")
    
    ![Create-Stacks](./images/create-stacks.png "Create-Stacks")
    
2.  Wählen Sie im Assistenten zum Erstellen von Stacks die Option **Stack 2- Configure.zip** aus, und laden Sie den Stack **Bereitstellen** hoch, den Sie in der vorherigen Übung heruntergeladen haben. Klicken Sie jetzt auf **Weiter**
    
    ![Hochladen](./images/upload.png "Hochladen")
    
3.  Geben Sie jetzt im Abschnitt **Variablen konfigurieren** die unten genannten Werte ein, und klicken Sie auf **Weiter**
    
    ![Konfigurations-Variablen](./images/configure-variables.png "Konfigurations-Variablen")
    
    1.  _Öffentliche IP-Adresse der Linux Compute-Instanz_, die zuvor erstellt wurde
    2.  _Identitätsdomain-URL_: Domain-URL der bereitgestellten Domain. **Hinweis** Entfernen Sie **:443** vom Ende der Domain-URL.
    3.  _Client-ID_ - Geben Sie die Client-ID Ihrer vertraulichen OCI-IAM-App ein
    4.  _Client Secret_ - Geben Sie das Client Secret der vertraulichen OCI-IAM-App ein
4.  Prüfen Sie nun die Konfigurationen unter **Details prüfen**, und klicken Sie auf **Erstellen**. Stellen Sie sicher, dass **Apply ausführen** ausgewählt ist.
    
    ![überprüfen](./images/review.png "überprüfen")
    

**Hinweis** Die Ausführung des Stacks kann rund 15 Minuten in Anspruch nehmen. Warten Sie, bis der **Job** erfolgreich ist.

## Abschluss

In dieser Übung konnten wir das Linux Pluggable Authentication Module (PAM) erfolgreich auf dem Linux-Server bereitstellen und konfigurieren und die Identitätsdomain so konfigurieren, dass eine vertrauliche Anwendung und _POSIX_\-Benutzer sowie eine _POSIX_\-Gruppe erstellt werden.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023