# Stellen Sie den Stack bereit, um OAS, AppGate und Identitätsdomain zu konfigurieren

## Einführung

Mit diesem Stack können wir **OAS, App-Gateway und Identitätsdomain** konfigurieren. Im Rahmen dieses Stacks wird eine Enterprise-Anwendung unter **Identitätsdomain** erstellt.

## Ziele

1.  \*\*OAS-Anwendung konfigurieren \*\*
2.  **App-Gateway** konfigurieren
3.  **Unternehmensanwendung** unter **Identitätsdomain** erstellen

## Voraussetzungen

Nachdem die ZIP-Datei **Stack2- Configure.zip** heruntergeladen wurde, dekomprimieren Sie sie, und ersetzen Sie den Inhalt der **.pem**\-Dateien (AppGate\_PrivateKey.pem und OAS\_PrivateKey.pem) durch den entsprechenden Inhalt des Private Keys.

## Aufgabe 1: Konfigurationsstack über Resource Manager bereitstellen

1.  Nachdem Sie sich bei der OCI-Konsole angemeldet haben, navigieren Sie zu **Entwicklerservices**, und wählen Sie unter **Resource Manager** die Option **Stacks** aus. Klicken Sie jetzt auf **Stack erstellen**

**Hinweis** Wählen Sie beim Erstellen des Stacks nicht das Compartment **Root** aus.

    ![stacks](./images/stacks.png "stacks")
    
    ![create-stacks](./images/create-stacks.png "create-stacks")
    

2.  Wählen Sie im Assistenten zum Erstellen von Stacks die Option **Stack 2- Configure.zip** aus, und laden Sie den Stack **Bereitstellen** hoch, den Sie in der vorherigen Übung heruntergeladen haben. Klicken Sie jetzt auf **Weiter**
    
    ![browse_zip ](./images/browse_zip.jpg "browse_zip")
    
    ![uploaded_zip](./images/uploaded_zip.jpg "uploaded_zip")
    
    ![default_details](./images/default_details.jpg "default_details")
    

**Hinweis** Der Stack nimmt automatisch das Arbeitsverzeichnis auf, stellt dem Stack einen Namen bereit, und das Arbeits-Compartment wird ausgewählt. Stackname und Compartment können bei Bedarf geändert werden.

3.  Geben Sie jetzt im Abschnitt **Variablen konfigurieren** die unten genannten Werte ein, und klicken Sie auf **Weiter**
    
    1.  _Öffentliche IP-Adresse des AppGate-Servers_
    2.  _Identitätsdomain-URL - Domain-URL der bereitgestellten Domain. **Hinweis** Entfernen Sie **:443** vom Ende der Domain-URL._
    3.  _Client-ID - Geben Sie die Client-ID Ihrer vertraulichen IDCS-Anwendung ein_
    4.  _Client Secret - Geben Sie das Client Secret Ihrer vertraulichen IDCS-App ein_
    5.  _OAS-Landingpage-URL - Dies kehrt von Stack eins zurück, nachdem der OAS-Server bereitgestellt wurde_
    6.  _OAS-Ursprungsserver-URL - Dies tritt aus Stack eins auf, nachdem der OAS-Server bereitgestellt wurde_
    7.  _OAS Weblogic-IP-Adresse - Diese wird vom Stack eins nach dem Deployment des OAS-Servers übernommen_
    8.  _Benutzername für OAS-Weblogic-Admin - Identischer Benutzername, den Sie in Satck verwendet haben_
    9.  _Geben Sie das WebLogic-Kennwort ein - Identischer Benutzername, den Sie in Satck one verwendet haben_
    
    ![configure_variables](./images/configure_variables.png "configure_variables")
    
4.  Prüfen Sie nun die Konfigurationen unter **Details prüfen**, und klicken Sie auf **Erstellen**. Stellen Sie sicher, dass **Apply ausführen** nicht ausgewählt ist.
    
    ![überprüfen](./images/review.png "überprüfen")
    
    ![created_stack](./images/created_stack.png "created_stack")
    
5.  Klicken Sie im erstellten Stack jetzt auf die Option **Planen**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![initiate_plan](./images/initiate_plan.png "initiate_plan")
    
    ![plan_successful](./images/plan_successful.png "plan_successful")
    
6.  Klicken Sie im erstellten Stack jetzt auf die Option **Anwenden**. Sie sollten eine Ausgabe für **Erfolgreich** erhalten.
    
    ![initiate_apply](./images/initiate_apply.png "initiate_apply")
    
    ![apply_successful](./images/apply_successful.png "apply_successful")
    

**Hinweis** Die Ausführung des Stacks kann etwa 35 Minuten dauern. Warten Sie, bis der **Job** erfolgreich ist.

## Abschluss

In dieser Übung konnten wir OAS Application, App Gateway Server und Identity Domain erfolgreich bereitstellen und konfigurieren.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Chetan Soni, Sagar Takkar
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Chetan Soni August 2023