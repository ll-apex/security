# Validieren

## Einführung

In dieser Übung erfahren Sie, wie Sie den SSO-Fluss zu Ihrer OAS-Anwendung über **OCI IAM-Identitätsdomains** testen können.

### Ziele

*   **Analyitcs-Benutzer** in der neu erstellten **Identitätsdomain** erstellen _Beispiel: WebLogic-Admin-Benutzer_
*   App-Gatewaykonfiguration validieren
*   Single Sign-On mit Oracle Analytics Server testen

## Aufgabe 1: Erstellen Sie einen Admin-Benutzer, um Single Sign-On zu testen.

1.  Navigieren Sie zu der neu erstellten Identitätsdomain, und klicken Sie im linken Menü auf die Option **Benutzer**.
    
    ![Domain-Home](./images/domain-home.png "Domain-Home")
    
2.  Klicken Sie jetzt auf **Benutzer erstellen**, fügen Sie die folgenden Details für den Benutzer hinzu, und **erstellen** Sie ihn.
    
    1.  _Vorname_ - OAS
    2.  _Nachname_ - Admin
    3.  Deaktivieren Sie die Option **E-Mail-Adresse als Benutzernamen verwenden**
    4.  _Benutzername_ - Admin
    5.  _E-Mail_ - Gültige E-Mail zum Zurücksetzen des Kennworts für diesen Benutzer
    
    ![Benutzer erstellen](./images/create-user.png "Benutzer erstellen")
    

**Hinweis**: Der **Benutzername** muss der WebLogic-Admin-Benutzername sein, den Sie während des **Stack 1**\-Deployments erstellt haben.

3.  Sie erhalten eine E-Mail zum **Aktivieren** Ihres Admin-Benutzers. Aktivieren Sie den Benutzer, indem Sie in der E-Mail auf die Option **Kennwort zurücksetzen** klicken.

## Aufgabe 2: App-Gatewaykonfiguration validieren

Nachdem der **Stack 2 - Konfigurieren** erfolgreich bereitgestellt und der **Admin-Benutzer** erstellt und aktiviert wurde, greifen Sie bitte auf die unten genannte App-Gateway-URL zu.

*   **https://publicIP\_App Gateway: 4443/Analysen**

Der Browser muss Sie zur Anmeldeseite der Identitätsdomains weiterleiten.

## Aufgabe 3: Single Sign-On mit Oracle Analytics Server über das App-Gateway testen

1.  SSO mit dem App-Gatewaylink testen
    
    1.  Öffnen Sie ein Browserfenster, und geben Sie die URL für das App-Gateway ein - **https://publicIP\_App Gateway:4443/analytics**
        
    2.  Die Seite **OCI IAM-Identitätsdomains-Signatur** wird angezeigt. Melden Sie sich mit dem **Benutzernamen und Kennwort** des zuvor erstellten Benutzers an.
        
    3.  Nach erfolgreicher Authentifizierung wird der Benutzer zur Homepage von **Oracle Analyitcs Server** umgeleitet, ohne **OAS-Zugangsdaten** eingeben zu müssen.
        
        ![oas-homepage](./images/oas-home-page.png "oas-homepage")
        
    4.  Wenn die **Oracle Analytics Server**\-Homepage angezeigt wird, prüfen Sie den **angemeldeten Benutzernamen**, indem Sie auf die Option **Mein Account** klicken.
        
        ![verify-loggedin-user](./images/verify-loggedin-user.png "verify-loggedin-user")
        
    5.  Melden Sie sich bei Oracle OAS ab. Der Browser wird zur **OCI IAM-Identitätsdomains-Signatur** auf der Seite "In" umgeleitet.
        

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Chetan Soni, Sagar Takkar
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Chetan Soni August 2023