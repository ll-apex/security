# Stack zur Konfiguration von EBS, EBS Asserter und Identitätsdomain bereitstellen

## Einführung

Mit diesem Stack können wir **EBS, EBS Asserter Server und Identitätsdomain** konfigurieren. Im Rahmen dieses Stacks wird eine vertrauliche Anwendung unter **Identitätsdomain** erstellt, und Beispielbenutzer werden erstellt.

## Ziele

1.  **EBS-Anwendung - 12.2.11** konfigurieren
2.  **EBS Asserter-Server** konfigurieren
3.  Erstellen Sie die **Vertrauliche Anwendung** unter **Identitätsdomain**.

## Voraussetzungen

Nachdem die ZIP-Datei **Stack2- Configure.zip** heruntergeladen wurde, dekomprimieren Sie sie, und ersetzen Sie den Inhalt der **.pem**\-Dateien (ebs.pem und ebsasserter.pem) durch den entsprechenden Inhalt des Private Keys.

## Aufgabe 1: Konfigurationsstack über Resource Manager bereitstellen

1.  Nachdem Sie sich bei der OCI-Konsole angemeldet haben, navigieren Sie zu **Entwicklerservices**, und wählen Sie unter **Resource Manager** die Option **Stacks** aus. Klicken Sie jetzt auf **Stack erstellen**

**Hinweis** Wählen Sie beim Erstellen des Stacks nicht das Compartment **Root** aus.

![1 erfassen](./images/image10.png "1 erfassen")

![Erfassen 2](./images/image11.png "Erfassen 2")

2.  Wählen Sie im Assistenten zum Erstellen von Stacks die Option **Stack 2- Configure.zip** aus, und laden Sie den Stack **Bereitstellen** hoch, den Sie in der vorherigen Übung heruntergeladen haben. Klicken Sie jetzt auf **Weiter**
    
    ![Bild 1](./images/image1.jpg "Bild 1")
    
    ![Bild 2](./images/image2.jpg "Bild 2")
    
    ![Bild 3](./images/image3.jpg "Bild 3")
    

**Hinweis** Der Stack nimmt automatisch das Arbeitsverzeichnis auf, stellt dem Stack einen Namen bereit, und das Arbeits-Compartment wird ausgewählt. Stackname und Compartment können bei Bedarf geändert werden.

3.  Geben Sie jetzt im Abschnitt **Variablen konfigurieren** die unten genannten Werte ein, und klicken Sie auf **Weiter**
    
    1.  _Öffentliche IP-Adresse Ihres Asserter-Servers_
    2.  _Öffentliche IP-Adresse Ihres EBS-Servers_
    3.  _Geben Sie das Kennwort WebLogic ein._**Hinweis** Dies ist dasselbe Kennwort, das Sie in das Secret Ihres Vaults gelegt haben, das in Stack1 - Deploy.zip verwendet wird.
    4.  _Identitätsdomain-URL_: Domain-URL der bereitgestellten Domain. **Hinweis** Entfernen Sie **:443** vom Ende der Domain-URL.
    5.  _Client-ID_ - Geben Sie die Client-ID Ihrer IDCS Confidential App ein
    6.  _Client Secret_ - Geben Sie das Client Secret Ihrer vertraulichen IDCS-App ein
    
    ![3 erfassen](./images/image12.png "3 erfassen")
    
4.  Prüfen Sie nun die Konfigurationen unter **Details prüfen**, und klicken Sie auf **Erstellen**. Stellen Sie sicher, dass **Apply ausführen** nicht ausgewählt ist.
    
    ![4 erfassen](./images/image13.png "4 erfassen")
    
5.  Klicken Sie im erstellten Stack jetzt auf die Option **Planen**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![Bild 7](./images/image7.jpg "Bild 7")
    
    ![Bild 8](./images/image8.jpg "Bild 8")
    
6.  Klicken Sie im erstellten Stack jetzt auf die Option **Anwenden**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![Bild 9](./images/image9.jpg "Bild 9")
    

**Hinweis** Die Ausführung des Stacks kann etwa 15 Minuten dauern. Warten Sie, bis der **Job** erfolgreich ist.

## Abschluss

In dieser Übung konnten wir EBS-Anwendung, EBS Asserter Server und Identitätsdomain erfolgreich bereitstellen und konfigurieren.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023