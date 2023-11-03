# Federation konfigurieren - Externer Identitätsprovider (Okta)

## Einführung

Ein _Identitätsprovider_, auch als _Identity Assertion-Provider_ bezeichnet, stellt IDs für Benutzer bereit, die mit Oracle Identity Cloud Service über eine Website außerhalb von Oracle Identity Cloud Service interagieren möchten. Ein _Serviceprovider_ ist eine Website wie Oracle Identity Cloud Service, die Anwendungen hostet. Sie können einen Identitätsprovider aktivieren und einen oder mehrere Serviceprovider definieren. Ihre Benutzer können dann direkt vom Identitätsprovider auf die von den Serviceprovidern gehosteten Anwendungen zugreifen.

Für diese Übung kann eine Website Benutzern die Anmeldung bei Oracle Identity Cloud Service mit Okta-Zugangsdaten ermöglichen. Okta fungiert als Identitätsprovider und Oracle Identity Cloud Service fungiert als Serviceprovider. Okta prüft, ob der Benutzer ein autorisierter Benutzer ist, und gibt Informationen an Oracle Identity Cloud Service zurück.

Geschätzte Laborzeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Föderation in Okta konfigurieren
*   Federation in IDCS konfigurieren
*   Anmeldung bei IdP

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Ein Google-Account
*   Ein Okta Integrator-Account

## Aufgabe 1: Okta konfigurieren

*   _Personen_:
    *   Administrator

1.  1.  Gehen Sie zu Okta, und registrieren Sie sich für ein [Entwicklerkonto](https://www.okta.com/integrate/signup/). Dieser Teil wurde im Abschnitt "Voraussetzungen" behandelt.
2.  Sobald Sie die Zugangsdaten für Ihren Testaccount haben, melden Sie sich mit der URL an, die Sie nach der Anforderung der Testversion erhalten haben (die benutzerdefinierte, z.B. https://oracleworkshop.oktareview.com), und gehen Sie zu _Anwendungen_. Wählen Sie im Menü oben links die Option _Klassische Benutzeroberfläche_. ![Bild](images/L4001.png)
    
3.  Wählen Sie in der oberen Registerkarte _Anwendungen_, _Anwendungen_ aus. ![Bild](images/L4002.png)
    
4.  Klicken Sie auf die Schaltfläche _Anwendung hinzufügen_. ![Bild](images/L4003.png)
    
5.  Wählen Sie die Schaltfläche _Neue Anwendung erstellen_ aus. ![Bild](images/L4004.png)
    
6.  Wählen Sie _SAML2.0_ aus, und klicken Sie auf _Erstellen_. ![Bild](images/L4005.png)
    
7.  Geben Sie einen Anwendungsnamen an. Beispiel: **IDCS\_SP**, und klicken Sie auf _Weiter_. ![Bild](images/L4006.png)
    
8.  Im Abschnitt _SAML konfigurieren_ müssen Sie Werte aus den IDCS-Metadaten eingeben.
    
        https://<your tenant>.identity.oraclecloud.com/fed/v1/metadata
        
9.  Geben Sie die folgenden Werte aus den Metadaten im SAML-Setup von Okta ein:
    
    *   Suchen Sie in den IDCS-Metadaten unten nach _AssertionConsumerService_. Kopieren Sie die hervorgehobene URL in _Location_ (siehe unten). Geben Sie die URL als _Single Sign-On-URL_ in Okta ein ![Bild](images/L4007.png)
    *   Suchen Sie in den IDCS-Metadaten oben nach _entityID_. Kopieren Sie die hervorgehobene URL wie unten dargestellt. Geben Sie die URL als _Zielgruppen-URI (SP-Entity-ID)_ in Okta ein ![Bild](images/L4008.png)
    *   Wenn Sie fertig sind, sollten die allgemeinen Einstellungen in Okta etwa wie im folgenden Beispiel aussehen. Übernehmen Sie für die übrigen Felder die Standardwerte. ![Bild](images/L4009.png)
10.  Scrollen Sie zum Ende der Seite, und klicken Sie auf _Weiter_. ![Bild](images/L4010.png)
    
11.  Geben Sie das obligatorische Feedback ein, und wählen Sie _Fertigstellen_ aus. ![Bild](images/L4011.png)
    
12.  Klicken Sie unter _Einstellungen_ auf _Identitätsprovider-Metadaten_, um die IDP-Metadatendatei zu exportieren. ![Bild](images/L4012.png)
    
13.  Die Metadaten werden in einem anderen Browserfenster angezeigt. Speichern Sie es mit der rechten Maustaste in einer xml-Datei und wählen Sie "Speichern unter", da Sie dies in wenigen Minuten innerhalb von IDCS benötigen. ![Bild](images/L4013.png)
    
14.  Gehen Sie zurück zu _Okta_, und wählen Sie die Registerkarte _Zuweisungen_ Ihrer neu erstellten Anwendung aus, um der Anwendung Benutzer zuzuweisen. ![Bild](images/L4014.png)
    
15.  Klicken Sie auf _Zuweisen_, und wählen Sie _Zu Personen zuweisen_ aus. ![Bild](images/L4015.png)
    
16.  Wählen Sie einen Benutzer in der Liste aus, und klicken Sie auf _Zuweisen_. Geben Sie den Benutzernamen des entsprechenden IDCS-Benutzers ein, und wählen Sie _Speichern und zurück_ aus. Wenn der Benutzer zugewiesen ist, klicken Sie auf _Fertig_.
    
    ![Bild](images/L4016.png)
    
    Hinweis: Dies ist nicht der Benutzername, den Sie bei der Anmeldung bei Okta angeben. Das bleibt unverändert. Sie geben hier den Benutzernamen des entsprechenden Benutzers in IDCS ein. Dadurch wird eine Verknüpfung zwischen Accounts mit unterschiedlichen Benutzernamen in IDCS und Okta aktiviert. Die Accounts können unterschiedliche Benutzernamen haben, z.B. weil Okta sie in einem E-Mail-Format benötigt, IDCS dies jedoch nicht.
    
17.  IDCS ist jetzt als Serviceprovider in Okta konfiguriert. Öffnen Sie IDCS, um Okta als Identitätsprovider zu konfigurieren.
    

## Aufgabe 2: IDCS konfigurieren

*   _Personen_:
    *   Administrator

1.  Melden Sie sich bei der IDCS-Admin-Konsole an, und gehen Sie zu _Sicherheit_. Klicken Sie dann auf _Identitätsprovider_. Klicken Sie auf _SAML-IDP hinzufügen_. Geben Sie den Namen des Providers (z.B. "Okta-<\>") und die Beschreibung ein, und klicken Sie auf "Weiter". ![Bild](images/L4017.png) 
    
2.  Laden Sie die zuvor exportierte Okta-Metadaten-XML-Datei hoch, und klicken Sie auf _Weiter_. ![Bild](images/L4018.png)
    
3.  Legen Sie die folgenden Parameter fest:
    
    *   Benutzerattribut des Identitätsproviders = **Name-ID**
    *   Oracle Identity Cloud Service-Benutzerattribut = **Benutzername**
    *   Das angeforderte NameID-Format = **Nicht angegeben** ![Bild](images/L4019.png)
4.  Auf dem Exportbildschirm gibt es nichts zu tun, da wir die IDCS-Metadaten bereits aus der URL abgerufen haben und sie nicht erneut herunterladen müssen. Klicken Sie auf _Weiter_. ![Bild](images/L4020.png)
    
5.  Wählen Sie _Anmeldung testen_, um die Verbindung zwischen IDCS und Okta zu prüfen. ![Bild](images/L4021.png)
    
6.  Geben Sie Ihre Okta-Zugangsdaten ein. ![Bild](images/L4022.png)
    
7.  Wenn alles funktioniert, wird die folgende Meldung angezeigt. Prüfen Sie andernfalls Ihre Konfigurationen für die IDCS-Anwendung in Okta und/oder den Okta-SAML-IDP in IDCS. ![Bild](images/L4023.png)
    
8.  Wählen Sie im Ablauf "SAML IDP-Setup" die Option _Weiter_ aus. ![Bild](images/L4024.png)
    
9.  Wählen Sie _Aktivieren_, _Fertigstellen_ aus. Sie haben jetzt Okta als externen Identitätsprovider hinzugefügt und erfolgreich eine Verbindung mit IDCS hergestellt. ![Bild](images/L4025.png)
    
10.  Nachdem Sie das Setup der Okta-Anwendung erfolgreich abgeschlossen haben, kehren Sie zum Bildschirm _Identitätsprovider_ zurück. Wählen Sie das Menü für _Auf Anmeldeseite anzeigen_. ![Bild](images/L4026.png)
    
11.  Gehen Sie zu _Sicherheit_, wählen Sie _IDP-Policys_ aus, und klicken Sie auf _Standard-Identitätsprovider-Policy_. ![Bild](images/L4027.png)
    
12.  Wählen Sie die Menüoption _\+ Zuweisen_, und fügen Sie den Okta-Provider hinzu. ![Bild](images/L4028.png)
    
13.  Wählen Sie _Okta_ aus, und klicken Sie auf _OK_. ![Bild](images/L4029.png)
    
14.  _Okta_ wird in der Liste ![Bild](images/L4030.png) angezeigt
    
15.  _Abmelden_ von IDCS. ![Bild](images/L4031.png)
    

## Aufgabe 3: Externe IDP-Anmeldung prüfen

*   _Personen_:
    *   Administrator

1.  Greifen Sie auf die IDCS-Admin-Konsole zu, und Okta wird als Anmeldeoption angezeigt. Wählen Sie _Okta_ aus. ![Bild](images/L4032.png)
    
2.  Geben Sie Ihre Okta-Anmeldedaten ein. ![Bild](images/L4033.png)
    
3.  Bei erfolgreicher Authentifizierung wird die Anforderung an den IDCS-Bildschirm "Meine Apps" gesendet. ![Bild](images/L4034.png)
    
    Hinweis: Damit die Föderation funktioniert, müssen Sie sich mit einem Account anmelden, der sowohl in IDCS als auch in Okta definiert ist.
    

Sie können jetzt mit der nächsten Übung fortfahren

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020