# Daten in eine Instanz laden

## Einführung

Ein Identitätsprovider, der als Identity Assertion-Provider bezeichnet wird, stellt IDs für Benutzer bereit, die über eine externe Website von Oracle Identity Cloud Service mit Oracle Identity Cloud Service interagieren möchten. Ein Serviceprovider ist eine Website wie Oracle Identity Cloud Service, die Anwendungen hostet. Sie können einen Identitätsprovider aktivieren und einen oder mehrere Serviceprovider definieren. Ihre Benutzer können dann direkt vom Identitätsprovider auf die von den Serviceprovidern gehosteten Anwendungen zugreifen.

Oracle Identity Cloud Service (IDCS) unterstützt Social-Identitätsprovider, sodass sich Benutzer mit ihren Social-Zugangsdaten bei IDCS anmelden können. Mit der Social-Anmeldefunktion können Benutzer sich selbst in IDCS registrieren, wenn sie noch keinen Account haben.

Folgende Social-Provider sind mit IDCS sofort einsatzbereit -

*   Facebook
*   Google
*   LinkedIn
*   Microsoft
*   Twitter

IDCS unterstützt auch alle generischen Social-Identitätsprovider, die OpenID Connect-konform sind.

Die folgenden Links sind zu einem Video mit der Konfiguration mit Google und IDCS. Die Bildschirme sind veraltet, aber die Parameter und Konzepte sind hilfreich.

[](youtube:-OwFAGJw3vo)

Ein weiteres Video - mit mehr Details, aber mit den älteren Stil-Bildschirmen:

[](youtube:JU8ArDvzWq0)

Geschätzte Laborzeit: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Federation in Google konfigurieren
*   Federation in IDCS konfigurieren
*   Social-Anmeldung bestätigen

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Ein Google-Account

## Aufgabe 1: Google konfigurieren

*   _Personen_:
    *   Administrator

Eine kurze Einführung zur Integration der Google-Anmeldung in Ihre Webanwendung finden Sie unter diesem [Link](https://developers.google.com/identity/sign-in/web/sign-in).

Dieses Handbuch enthält auch ein ausführliches Tutorial zur Integration von Google als IdP für IDCS.

Um Google als IdP für IDCS zu verwenden, müssen wir Auth 2.0-Zugangsdaten von Google erhalten. Dies kann durch ein Entwicklerkonto bei google erfolgen und eine Anwendung zu diesem Zweck erstellen.

1.  To start, please click on the [OpenIDConnect protocol page](https://developers.google.com/identity/protocols/OpenIDConnect) on the _Google Identity Platform_ page.
    
2.  Scrollen Sie nach unten zu der Unterüberschrift _Zugangsdaten für OAuth 2.0 abrufen_, und klicken Sie auf die Hyperlinkseite _Zugangsdaten_. Es öffnet sich ein neues Fenster, in dem Sie aufgefordert werden, sich mit Ihrem Google-Konto anzumelden. Melden Sie sich mit dem Google-Konto an, das Sie im Abschnitt "Voraussetzungen" dieses Workshops erstellt haben. ![Bild](images/L3001.png)
    
3.  Klicken Sie auf _Projekt auswählen_, und wählen Sie _NEUES PROJEKT_ aus. ![Bild](images/L3002.png)
    
4.  Wählen Sie einen Namen für Ihr Projekt, und klicken Sie auf _CREATE_. ![Bild](images/L3003.png)
    
5.  Klicken Sie unter _Zugangsdaten_ auf den _OAuth-Einwilligungsbildschirm_.
    
6.  Wählen Sie einen _Anwendungsnamen_. ![Bild](images/L3004.png)
    
7.  Wählen Sie unter _autorisierte Domains_ die Option _oraclecloud.com_ aus, und klicken Sie auf _Speichern_. ![Bild](images/L3005.png)
    
8.  Klicken Sie auf _Create Credentials_, und wählen Sie _OAuth client ID_ aus. ![Bild](images/L3006.png)
    
9.  Wählen Sie _Webanwendung_ und einen _Namen_ aus.
    
10.  Setzen Sie die _autorisierte umgeleitete URL_ auf den folgenden Wert:
    
        https://<your tenant>/oauth2/v1/social/callback
        
11.  Klicken Sie auf _Erstellen_, um die Schlüssel zu erstellen. ![Bild](images/L3007.png)
    
12.  Sie erhalten die _Client-ID_ und das _Client Secret_. Speichern Sie diese für später. ![Bild](images/L3008.png)
    

## Aufgabe 2: IDCS konfigurieren

*   _Personen_:
    *   Administrator

Kehren Sie jetzt zu _IDCS_ zurück, und geben Sie die Providerparameterwerte ein, um Google als Social IdP zu konfigurieren.

1.  Gehen Sie zur IDCS-Admin-Konsole. Gehen Sie im linken Randleistenmenü zur Registerkarte _Sicherheit_ und dann zu _Identitätsprovider_.
    
2.  Klicken Sie auf _Social-IDP hinzufügen_
    
3.  Wählen Sie den Typ _Google_ aus. Geben Sie einen Namen des Providers an (z.B. "Google-<\>", und klicken Sie auf _Weiter_. ![Bild](images/L3009.png) 
    
4.  Geben Sie die zuvor abgerufenen _Client-IDs_ und _Client Secret_ ein. ![Bild](images/L3010.png)
    
5.  Klicken Sie auf _Fertigstellen_. Klicken Sie dann auf die Menüschaltfläche auf der rechten Seite, und wählen Sie _Aktivieren_ aus, um Google als IdP zu aktivieren. ![Bild](images/L3011.png)
    
    Jetzt ist Google als soziale IDP für IDCS definiert, ist aber auf der Anmeldeseite immer noch nicht sichtbar. In den nächsten Schritten wird beschrieben, wie Sie sie einer IDP-Policy zuweisen, um Google auf der IDCS-Anmeldeseite sichtbar zu machen.
    
6.  Klicken Sie erneut auf die Menüschaltfläche, und wählen Sie die Option _Auf Anmeldeseite anzeigen_, um Google als IdP auf der IDCS-Anmeldeseite anzuzeigen. ![Bild](images/L3012.png)
    
    Sobald Sie _Auf Anmeldeseite anzeigen_ eingerichtet haben, wird ein "Auge" angezeigt.
    
    ![Bild](images/L3013.png)
    
7.  Klicken Sie im Randleistenmenü auf _Sicherheit_ und dann auf _IDP-Policys_. Klicken Sie auf _Standardidentitäts-Policy_. ![Bild](images/L3014.png)
    
8.  Wählen Sie die Registerkarte _Identitätsprovider_, und klicken Sie auf _Zuweisen_, um Google als IdP zuzuweisen. ![Bild](images/L3015.png)
    
9.  Wählen Sie _Google_ aus, und klicken Sie auf _OK_. ![Bild](images/L3016.png)
    
10.  Google wird nun in der Liste der Identitätsprovider angezeigt. Google wurde jetzt als IdP für IDCS konfiguriert und zugewiesen. Der nächste Schritt besteht darin, Ihr Google-Konto mit Ihrem IDCS-Konto zu verknüpfen. Klicken Sie dazu auf _Mein Profil_. ![Bild](images/L3017.png)
    
11.  Wählen Sie unter _Mein Profil_ die Registerkarte _Social-Accounts_ aus. Klicken Sie auf _Social-Account verknüpfen_, um Ihren Gmail-Account mit Ihrem IDCS-Account zu verknüpfen. ![Bild](images/L3018.png)
    
12.  Klicken Sie auf das _Hamburger-Menü_ und dann auf _Link_. ![Bild](images/L3019.png)
    
13.  Ihr Gmail-Account ist jetzt mit Ihrem IDCS-Account verknüpft. Klicken Sie auf _Abmelden_, um sich von IDCS abzumelden. ![Bild](images/L3020.png)
    

## Aufgabe 3: Social-Anmeldung prüfen

*   _Personen_:
    *   Endbenutzer

Kehren Sie jetzt zu _IDCS_ zurück, und geben Sie die Providerparameterwerte ein, um Google als Social IdP zu konfigurieren.

1.  Gehen Sie zur IDCS-Admin-Konsole. Klicken Sie als Anmeldemethode auf _Google_. ![Bild](images/L3021.png)
    
2.  Die Anmeldeseite von Google wird angezeigt. Geben Sie Ihre Gmail-Adresse und Ihr Passwort ein, und klicken Sie auf _Weiter_. ![Bild](images/L3022.png)
    
3.  Google leitet zur Seite _Meine Apps_ von IDCS um. Die Integration von Google als IdP für IDCS wurde erfolgreich getestet. ![Bild](images/L3023.png)
    

Sie können jetzt mit der nächsten Übung fortfahren

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020