# Stack für die Installation von OAS, App-Gatewayinstanz und Identitätsdomain bereitstellen

## Einführung

Mit diesem Stack können wir **OAS-Version 6.4, App-Gatewayserver und Identitätsdomain** bereitstellen/installieren. Die erstellte Identitätsdomain hat den Typ **Oracle Apps Premium**.

## Ziele

1.  **OAS-Anwendungsversion 6.4** bereitstellen und konfigurieren
2.  **App-Gatewayserver** bereitstellen
3.  Stellen Sie die **Identitätsdomain** vom Typ **Oracle Apps Premium** bereit
4.  Validieren Sie die erstellten Ressourcen über den Webbrowser und über SSH-Zugriff.

## Voraussetzungen

Nachdem die ZIP-Datei **Stack1- Deploy.zip** heruntergeladen wurde, dekomprimieren Sie sie, und ersetzen Sie den Inhalt der Datei **SSH.key** durch den entsprechenden Inhalt des Private Keys.

**Hinweis** Der Name der Datei darf nicht geändert werden.

## Aufgabe 1: Stack über Resource Manager bereitstellen

1.  Nachdem Sie sich bei der OCI-Konsole angemeldet haben, navigieren Sie zu **Entwicklerservices**, und wählen Sie unter **Resource Manager** die Option **Stacks** aus. Klicken Sie jetzt auf **Stack erstellen**

**Hinweis** Wählen Sie beim Erstellen des Stacks nicht das Compartment **Root** aus.

![1 erfassen](./images/image21.png "1 erfassen")

![Erfassen 2](./images/image22.png "Erfassen 2")

2.  Wählen Sie im Assistenten zum Erstellen von Stacks die Option **.zip** aus, und laden Sie den Stack **Bereitstellen** hoch, den Sie in der vorherigen Übung heruntergeladen haben. Klicken Sie jetzt auf **Weiter**
    
    ![Bild 1](./images/image1.jpg "Bild 1")
    
    ![Bild 2](./images/image2.jpg "Bild 2")
    
    ![Bild 3](./images/image3.jpg "Bild 3")
    

**Hinweis** Der Stack nimmt automatisch das Arbeitsverzeichnis auf, stellt dem Stack einen Namen bereit, und das Arbeits-Compartment wird ausgewählt. Stackname und Compartment können bei Bedarf geändert werden.

3.  Geben Sie im Abschnitt **Compute-Instanz von Oracle Anlytics Server** den **Anzeigenamen** für die OAS-Instanz an, und wählen Sie das **Compartment** für die Instanz aus, in dem die Instanz platziert werden soll. Wählen Sie dann die **Availability-Domain** für die Instanz aus. Wählen Sie die **Ausprägung** und die **Boot-Volume-Größe** für die Instanz aus, und laden Sie dann den **SSH Publik Key** hoch.
    
    ![Bild 4](./images/image4.jpg "Bild 4")
    

**Hinweis** Der SSH-Public Key muss als Voraussetzung generiert werden.

4.  Wählen Sie im Abschnitt **Netzwerkkonfiguration** das **VCN-Compartment** mit dem **vorhandenen Netzwerk** aus, und wählen Sie dann das **Subnetz** aus. Aktivieren Sie das Kontrollkästchen **Öffentliche IP-Adresse der Compute-Instanz zuweisen**, wenn Sie das öffentliche Subnetz ausgewählt haben.
    
    ![Bild 5](./images/image5.jpg "Bild 5")
    
5.  Aktivieren Sie im Abschnitt **Oracle Analytics Server-Domainkonfiguration** das Kontrollkästchen, um die OAS-Domain zu erstellen. Geben Sie den **Benutzernamen des Analytics-Administrators** und das **Analytics-Administratorkennwort** an. Dieser Benutzername und dieses Kennwort werden für die Anmeldung bei der auf der OAS-Instanz bereitgestellten WebLogic verwendet. Geben Sie dann die **Datenbankverbindungszeichenfolge** im angegebenen Format an, und die DB muss das Oracle Cloud-VM-DB-System sein. Geben Sie den **Benutzernamen des Datenbankadministrators** aus dem von Ihnen erstellten Oracle VM-DB-System an. Geben Sie außerdem das **Datenbankadministratorkennwort** an, um eine Verbindung zur Datenbank herzustellen. Geben Sie dann das **Datenbankschemapräfix** und das **Datenbankschemakennwort** an, um die Domainkonfiguration abzuschließen.
    
    ![Bild 6](./images/image6.jpg "Bild 6")
    
6.  Wählen Sie im Abschnitt **App-Gateway-Compute-Instanz** das **Instanz-Compartment** aus, um die App-Gatewayinstanz zu platzieren. Geben Sie den **Namen Ihrer Instanz** für das App-Gateway an. Laden Sie dann Ihren **SSH-Public Key** hoch. Wählen Sie jetzt die **Availability-Domain** aus, um die Instanz beizubehalten. Wählen Sie dann **Netzwerk-Compartment** aus, um das **vorhandene Netzwerk** für das App-Gateway auszuwählen. Wählen Sie außerdem das **Vorhandene öffentliche Subnetz für Instanz** aus.
    
    ![Bild 7](./images/image7.jpg "Bild 7")
    
7.  Wählen Sie im Abschnitt **OCI-Identitätsdomain** das **Compartment** aus, in dem die **Identitätsdomain** erstellt werden soll. Geben Sie einen gültigen **Domainnamen**, eine gültige **Admin-E-Mail-Adresse** sowie grundlegende Admin-Details an. Klicken Sie jetzt auf **Weiter**.
    
    ![Bild 8](./images/image8.jpg "Bild 8")
    
8.  Prüfen Sie nun die Konfigurationen unter **Details prüfen**, und klicken Sie auf **Erstellen**. Stellen Sie sicher, dass **Apply ausführen** nicht ausgewählt ist.
    
    ![Bild 9](./images/image9.jpg "Bild 9")
    
    ![Bild 10](./images/image10.jpg "Bild 10")
    
    ![Bild 11](./images/image11.jpg "Bild 11")
    
    ![Bild 12](./images/image12.jpg "Bild 12")
    
9.  Klicken Sie im erstellten Stack jetzt auf die Option **Planen**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![Bild 13](./images/image13.jpg "Bild 13")
    
    ![Bild 14](./images/image14.jpg "Bild 14")
    
10.  Klicken Sie im erstellten Stack jetzt auf die Option **Anwenden**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![Bild 15](./images/image15.jpg "Bild 15")
    

**Hinweis** Die Ausführung des Stack-Deployments dauert 2 Minuten, die Erstellung der OAS-Domain dauert jedoch etwa 30-40 Minuten.

## Aufgabe 2: Validierung der erstellten Ressourcen.

Prüfen Sie die SSH-Verbindung zur OAS-Instanz und zur App-Gatewayinstanz.

_Mit dem Private Key dieser Instanz sollten Sie SSH in diese Systeme verwenden können._

Nachdem der **Stack** erfolgreich bereitgestellt wurde, können Sie eine SSH-Verbindung zur OAS-Instanz herstellen und es unten prüfen.

1.  Navigieren Sie zum Verzeichnis **/u01/app/OAS-scripts**, und suchen Sie nach der Datei **oas\_install.finish**. Diese Datei gibt an, dass die Installation der OAS-Domain abgeschlossen ist.
    
2.  Navigieren Sie zum Verzeichnis **/var/log**, und prüfen Sie in den Logdateien **oas\_cloudinit.log** und **oas\_create\_domain.log**, ob die Domain erfolgreich erstellt wurde.
    
3.  OAS-Instanz prüfen
    

Versuchen Sie, über diese URL auf Ihre OAS-Instanz zuzugreifen: _http://OAS\_Instance\_Public\_IP:9500/console_

**Benutzername** - _Analytics-Administratorbenutzername_

**Kennwort**: _Kennwort des Analytics-Administrators_

3.  Navigieren Sie in der OCI-Konsole unter **Identität und Sicherheit** zu **Domains**, um zu prüfen, ob Ihre Domain des Typs **Oracle Apps Premium** erstellt wurde.

## Abschluss

In dieser Übung konnten wir OAS Application, App Gateway Server und Identitätsdomain erfolgreich bereitstellen und validieren.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Sagar Takkar, Chetan Soni
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Sagar Takkar August 2023