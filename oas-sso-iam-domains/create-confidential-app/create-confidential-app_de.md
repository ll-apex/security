# Aktivierung der Identitätsdomain und Erstellen einer vertraulichen App für REST-API-Aufrufe

## Einführung

Während des Stackausführungsprozesses wird eine Oracle-Identitätsdomain vom Typ **Oracle Apps Premium** erstellt. Das von Ihnen verwendete Administratorkonto erhält eine automatisch generierte E-Mail zum Zurücksetzen des Kennworts für den jeweiligen Benutzer. In diesem Modul werden die Schritte zum Zurücksetzen des Kennworts für die Admin-Benutzer und das Verfahren zur Anmeldung bei der neu erstellten Oracle Apps Premium-Domain vorgestellt.

Dieses Modul unterstützt Sie auch beim Erstellen einer vertraulichen Anwendung unter der neu erstellten Identitätsdomain, die für REST-API-Aufrufe im Backend verwendet wird.

## Ziele

1.  **Zurücksetzen** Sie das Kennwort für den Admin-Benutzer, und melden Sie sich bei der neu erstellten Identitätsdomain an.
2.  Erstellen Sie eine vertrauliche Anwendung mit dem zulässigen Berechtigungstyp **Clientzugangsdaten**

## Aufgabe 1: Kennwort für den Admin-Benutzer zurücksetzen und sich bei der neu erstellten Domain anmelden

1.  Während des Stackausführungsprozesses wird eine automatisch generierte E-Mail mit der Schaltfläche **Account aktivieren** angezeigt. Bitte klicken Sie darauf.
    
    **Beispiel-E-Mail:** ![Beispiel-E-Mail](./images/sample-email.jpg "Beispiel-E-Mail")
    
2.  Setzen Sie auf der geladenen Seite das **Kennwort für Ihren Admin-Account unter Ihrer neu erstellten Domain zurück**.
    
    ![Kennwort zurücksetzen](./images/password-reset.jpg "Kennwort zurücksetzen")
    
3.  Versuchen Sie nun, sich mit dem Admin-Account, den Sie gerade abgeschlossen haben, bei Ihrer neuen Domain anzumelden.
    
    ![Anmelden](./images/sign-in.jpg "Anmelden")
    
4.  Nach erfolgreicher Authentifizierung wird die Seite **Meine Konsole** angezeigt. Klicken Sie im Menü auf die Option **OCI-Konsole**, um zum Bildschirm **Infrastrukturkonsole** zu gelangen.
    
    ![Konsole](./images/myconsole.jpg "Konsole")
    
5.  Sie sollten auf dem unten genannten Bildschirm landen, in dem Sie die Details zu Ihrer neu erstellten Domain des Typs **Oracle Apps Premium** erhalten.
    
    ![Domain-Überblick](./images/domain-overview.jpg "Domain-Überblick")
    

## Aufgabe 2: Vertrauenswürdige Anwendung erstellen

Wir registrieren eine vertrauliche Anwendung unter einer neu erstellten OCI-IAM-Domain mit dem zulässigen Berechtigungstyp **Clientzugangsdaten**

1.  Kopieren Sie unter der neu erstellten **Identitätsdomain** die **Identitätsdomain-URL** aus dem Abschnitt **Domaininformationen**. Klicken Sie im Navigationsmenü auf **Anwendungen**.
    
    ![Domain-URL](./images/domain-url.jpg "Domain-URL")
    
    ![Anwendung hinzufügen](./images/add-application.jpg "Anwendung hinzufügen")
    

**Hinweis** Schließen Sie **:443** bitte am Ende der **Domain-URL aus**

2.  Wählen Sie **Vertikale Anwendung** aus, und klicken Sie auf **Workflow starten**.
    
    ![create-confidential-application](./images/create-confidential-application.jpg "create-confidential-application")
    
3.  Fügen Sie der Anwendung den Namen hinzu, und klicken Sie auf **Weiter**
    
    ![Anwendungsname](./images/app-name.jpg "Anwendungsname")
    
4.  Wählen Sie **Clientzugangsdaten** als Berechtigungstyp aus, und klicken Sie auf **Weiter**.
    
    ![Berechtigungstyp](./images/grant-type.jpg "Berechtigungstyp")
    
5.  Wählen Sie unter **Anwendungsrolle** die Option **Identitätsdomainadministrator** aus, und **Hinzufügen**.
    
    ![Anwendungsrollen](./images/app-roles.jpg "Anwendungsrollen")
    
6.  Klicken Sie schließlich auf **Fertigstellen**.
    
    ![Ende](./images/finish.jpg "Ende")
    
7.  Klicken Sie auf **Anwendung aktivieren**.
    
    ![aktivieren](./images/activate.jpg "aktivieren")
    
8.  Rufen Sie jetzt die **Client-ID** und das **Secret** ab, um sie in weiteren Konfigurationen zu verwenden.
    
    ![Client-ID-Secret](./images/client-id-secret.jpg "Client-ID-Secret")
    

## Abschluss

In dieser Übung haben wir den Admin-Benutzer unter Ihrer neu erstellten Identitätsdomain vom Typ Oracle Apps Premium aktiviert und dann eine vertrauliche Anwendung für die REST-API erstellt, die weiter in den Terraform-Skripten verwendet werden soll.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Chetan Soni, Sagar Takkar
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Chetan Soni August 2023