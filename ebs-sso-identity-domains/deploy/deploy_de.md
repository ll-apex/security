# Stellen Sie den zu installierenden Stack und EBS, EBS Asserter und die Identitätsdomain bereit

## Einführung

Mit diesem Stack können wir **EBS - 12.2.11, EBS Asserter Server und Identitätsdomain** bereitstellen/installieren. Die erstellte Identitätsdomain hat den Typ **Oracle Apps Premium**. In der EBS-Anwendung konfigurieren wir EBS mit **Web Entry Point** und einigen Demo-Benutzern.

## Ziele

1.  **EBS-Anwendung** bereitstellen und konfigurieren
2.  **EBS Asserter-Server** bereitstellen
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

3.  Laden Sie jetzt im Abschnitt **Variablen konfigurieren** den **SSH-Public Key** hoch. Stellen Sie sicher, dass **OCI-Policys** aktiviert sind
    
    ![Bild 4](./images/image4.jpg "Bild 4")
    

**Hinweis** Der SSH-Public Key muss als Voraussetzung generiert werden.

4.  Wählen Sie im Abschnitt **Virtuelles Cloud-Networking** das **Netzwerk-Compartment** mit dem vorhandenen **VCN** aus, und wählen Sie dann das **Subnetz-Compartment** mit dem vorhandenen **öffentlichen Subnetz** aus. Wählen Sie unter **Vorhandenes Subnetz für Weblogic-Server** die Option **Regionales Subnetz** aus, und wählen Sie das vorhandene regionale Subnetz aus.
    
    ![Bild 5](./images/image5.jpg "Bild 5")
    
5.  Geben Sie unter **Weblogic-Domainkonfiguration** die **Weblogic-Zugangsdaten** an, und wählen Sie unter **Java Development Kit** die Option **jdk8** aus.
    
    ![Bild 6](./images/image6.jpg "Bild 6")
    

**Hinweis** Der Weblogic-Benutzername verwendet das im _Vault Secret_ gespeicherte Kennwort.

6.  Wählen Sie im Abschnitt **EBS-Compute-Instanz** das **Compartment** aus, in dem die **EBS-Anwendung** bereitgestellt werden soll. Laden Sie das **SSH Public** für Ihre EBS-Anwendung hoch, und wählen Sie das **vorhandene öffentliche Subnetz** für die EBS-Instanz aus.
    
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
    

**Hinweis** Die Ausführung des Stacks kann zwischen 30 und 40 Minuten in Anspruch nehmen. Warten Sie, bis es erfolgreich erstellt wurde.

## Aufgabe 2: Validierung der erstellten Ressourcen.

Nachdem der **Stack** erfolgreich bereitgestellt wurde, aktualisieren wir die Datei **Hosts** auf unserem lokalen System und validieren dann die bereitgestellten Ressourcen.

    1. Public Ip Address of Asserter  ebsasserter.example.com
    2. Public IP Address of EBS Instance  demoebs.example.com
    

**Hinweis** Verwenden Sie den Wert **ebsasserter.example.com** für EBS Asserter und **demoebs.example.com** für die EBS-Anwendung während der gesamten Übung.

1.  Prüfen Sie den SSH-Zugriff auf Ihre EBS-Instanz und den Asserter-Server.

_Mit dem Private Key dieser Instanz sollten Sie SSH in diese Systeme verwenden können._

2.  EBS-Instanz prüfen

Versuchen Sie, über diese URL auf Ihre EBS-Instanz zuzugreifen: _http://demoebs.example.com:8000/OA\_HTML/AppsLogin_

**Benutzername** - _SYSADMIN_

**Kennwort**: Geben Sie Ihr _SYSADMIN-Kennwort_ ein. Verwenden Sie _SYSADMIN@1234_ als Passwort für die Anmeldung.

3.  Navigieren Sie in der OCI-Konsole unter **Identität und Sicherheit** zu **Domains**, um zu prüfen, ob Ihre Domain des Typs **Oracle Apps Premium** erstellt wurde.

## Abschluss

In dieser Übung konnten wir EBS-Anwendung, EBS Asserter Server und Identitätsdomain erfolgreich bereitstellen und validieren.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023