# ORM-Stack bereitstellen, um einen Linux-Server und eine Identitätsdomain zu erstellen

## Einführung

Mit diesem Stack können wir **Linux Server und Identitätsdomain** mit Terraform bereitstellen. Die erstellte Identitätsdomain hat den Typ **Oracle Apps Premium**. Auf dem Linux-Server konfigurieren wir in der nächsten Übung das **Linux Pluggable Authentication Module (PAM)-Modul** mit einem anderen Stack.

_Geschätzte Zeit:_ 15 Minuten

### Ziele

*   **Linux-Server** bereitstellen
*   Stellen Sie die **Identitätsdomain** vom Typ **Oracle Apps Premium** bereit
*   Validieren Sie die erstellten Ressourcen über den Webbrowser und über SSH-Zugriff.

### Voraussetzungen

*   Nachdem **Stack1- Deploy.zip** heruntergeladen wurde, dekomprimieren Sie die ZIP-Datei, und ersetzen Sie den Inhalt der Datei **SSH.key** und **SSH.key.pub** durch den jeweiligen Inhalt des Private Keys und Public Keys.

**Hinweis** Der Name der Datei darf nicht geändert werden.

## Aufgabe 1: Stack über Resource Manager bereitstellen

1.  Nachdem Sie sich bei der OCI-Konsole angemeldet haben, navigieren Sie zu **Entwicklerservices**, und wählen Sie unter **Resource Manager** die Option **Stacks** aus. Klicken Sie jetzt auf **Stack erstellen**
    
    ![Stapel](./images/stack.png "Stapel")
    
    ![Erstellen - Stacks](./images/create-stack.png "Erstellen - Stacks")
    
2.  Wählen Sie im Assistenten zum Erstellen von Stacks die Option **.zip** aus, und laden Sie den Stack **Bereitstellen** hoch, den Sie in der vorherigen Übung heruntergeladen haben. Klicken Sie jetzt auf **Weiter**
    
    ![ZIP hochladen](./images/upload-zip.png "ZIP hochladen")
    
    ![Stack-Details](./images/stack-details.png "Stack-Details")
    

**Hinweis**: Stackname und Compartment können bei Bedarf geändert werden.

3.  Wählen Sie jetzt im Abschnitt **Variablen konfigurieren** das betreffende Compartment aus, in dem sich das VCN im Abschnitt **Linux-Instanz-Compartment** befindet, und laden Sie den **SSH-Public Key** hoch. Wählen Sie die jeweilige **Availability-Domain**, das **VCN** und das **Subnetz** aus, in dem die Instanz bereitgestellt werden soll.
    
    ![Linux-Instanz-Details](./images/linux-instance-details.png "Linux-Instanz-Details")
    

**Hinweis** Der SSH-Public Key muss als Voraussetzung generiert werden.

4.  Wählen Sie im Abschnitt **OCI-Identitätsdomain** das **Identitätsdomain-Compartment** aus, in dem die Domain erstellt werden muss. Geben Sie dann den Namen der Identitätsdomain ein, und geben Sie Details zum Administrator an, wie **Admin-E-Mail-Adresse**, **Vorname** und **Nachname**.
    
    ![Identität-Domain-Details](./images/identity-domain-details.png "Identität-Domain-Details")
    
5.  Prüfen Sie nun die Konfigurationen unter **Details prüfen**, und klicken Sie auf **Erstellen**. Stellen Sie sicher, dass **Apply ausführen** ausgewählt ist.
    
    ![überprüfen](./images/review.png "überprüfen")
    

**Hinweis** Die Ausführung des Stacks kann zwischen 1 und 2 Minuten in Anspruch nehmen. Warten Sie, bis der Vorgang erfolgreich abgeschlossen wurde. Nach Abschluss wird eine Benachrichtigung über die oben angegebene _Admin-E-Mail-Adresse_ gesendet.

## Aufgabe 2: Validierung der erstellten Ressourcen.

Nachdem der **Stack** erfolgreich bereitgestellt wurde, validieren wir die bereitgestellten Ressourcen.

    1. Linux Server Instance
    2. Identity Domain 
    

1.  Prüfen Sie den SSH-Zugriff auf Ihren Linux-Server mit einem beliebigen SSH-Client.

_Mit dem für die Instanz verwendeten Private Key können Sie eine SSH-Verbindung zum System herstellen._

2.  Navigieren Sie unter **Identität und Sicherheit** zu **Domains** und dem entsprechenden Compartment in der OCI-Konsole, um zu prüfen, ob Ihre Domain des Typs **Oracle Apps Premium** erstellt wurde.

## Abschluss

In dieser Übung konnten wir einen Linux-Server und eine Identitätsdomain erfolgreich bereitstellen und validieren.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023