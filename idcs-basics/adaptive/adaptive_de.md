# Adaptive MFA in IDCS aktivieren

## Einführung

Adaptive Sicherheit ist ein erweitertes Feature, das Ihren Benutzern starke Authentifizierungsfunktionen basierend auf ihrem Verhalten in Oracle Identity Cloud Service und über mehrere heterogene On-Premise-Anwendungen und Cloud-Services hinweg bietet. Bei Aktivierung kann das Feature "Adaptive Sicherheit" das Risikoprofil eines Benutzers in Oracle Identity Cloud Service basierend auf seinem historischen Verhalten analysieren, z.B. zu viele nicht erfolgreiche Anmeldeversuche, zu viele nicht erfolgreiche MFA-Versuche und Gerätekontext in Echtzeit wie Anmeldungen von unbekannten Geräten, Zugriff von unbekannten Standorten, IP-Adressen auf der Sperrliste usw.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Risikoanbieter aktivieren
*   Risiko als Bedingung in Anmelde-Policy verwenden
*   Endbenutzerrisiko bewerten

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Ein Google-Account
*   Ein Salesforce-Entwickleraccount
*   Übung 2 wurde abgeschlossen

## Aufgabe 1: Risikoprovider aktivieren

*   _Personen_:
    *   Administrator

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Navigieren Sie zu _Sicherheit_, und wählen Sie _Adaptive Sicherheit_ aus.
    
3.  Die Liste der Risikoprovider wird angezeigt.
    
4.  Wählen Sie im Menü "Standardrisikoprovider" die Option _Bearbeiten_. ![Bild](images/L6001.png)
    
    Hinweis: Adaptive Security verwendet das Konzept von Risikoprovidern, damit Identitätsdomainadministratoren und Sicherheitsadministratoren verschiedene kontextbezogene und Bedrohungsereignisse konfigurieren können, die in Oracle Identity Cloud Service analysiert werden sollen. Außerdem können sie Benutzerrisikoscores von externen Risikoprovidern konfigurieren und nutzen.
    
    Ein Standardrisikoprovider in Oracle Identity Cloud Service wird automatisch mit einer Liste unterstützter kontextbezogener und Bedrohungsereignisse vordefiniert, wie zu viele nicht erfolgreiche Anmeldeversuche, zu viele nicht erfolgreiche MFA-Versuche und Zugriff von unbekannten Geräten. Administratoren können relevante Ereignisse aktivieren und für jedes dieser Ereignisse Gewichtung oder Schweregrad angeben. Das System verwendet die konfigurierte Gewichtung, um den Oracle Identity Cloud Service-Risikoscore des Benutzers zu berechnen.
    
5.  Auf dieser Seite können Sie die Standardrisikoproviderkonfigurationen anzeigen oder bearbeiten. ![Bild](images/L6002.png)
    
6.  Sie können den Bereich für niedriges, mittleres und hohes Risiko anpassen.
    
7.  Sie können Risikoereignisse auswählen, die Sie aktivieren/deaktivieren möchten, und dem Ereignis auch einen Risikoscore zuordnen.
    
8.  Klicken Sie auf die Navigationsschublade, navigieren Sie zurück zu _Sicherheit_, und wählen Sie _Adaptive Sicherheit_ aus.
    
9.  Klicken Sie oben auf der Seite auf den Umschalter _Adaptive Sicherheit_, um das Feature zu aktivieren. ![Bild](images/L6003.png)
    
10.  Sie können weitere Risikoprovider hinzufügen, indem Sie auf die Schaltfläche _Hinzufügen_ klicken. IDCS unterstützt derzeit die Integration mit Symantec Cloud SOC. Die Integration mit Oracle CASB und anderen Risikoprovidern der 3. Partei ist ebenfalls geplant.
    
11.  Für diese Übung verwenden wir den Standardrisikoprovider.
    

## Aufgabe 2: Risiko als Bedingung in Anmelde-Policy verwenden

*   _Personen_:
    *   Administrator

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Navigieren Sie zu _Sicherheit_, und wählen Sie _Anmelde-Policys_ aus.
    
3.  Klicken Sie auf _Bearbeiten_, um die Salesforce-Anwendungs-Policy zu bearbeiten. ![Bild](images/L6004.png)
    
4.  Navigieren Sie zur Registerkarte _Anmelderegeln_, und klicken Sie auf _Bearbeiten_, um die Regel _MFA für Mitarbeiter_ zu ändern. ![Bild](images/L6005.png)
    
5.  Fügen Sie die folgenden Parameter hinzu, und klicken Sie auf _Speichern_.
    

| Parameter | Wert |
| --- | --- |
| Operator | < (kleiner als; Standardwert ist größer) |
| Risikoprovidername | Standardrisikoanbieter |
| Bewertung | 90 |

![Bild](images/L6006.png)

Die Anmelderegeln für die Salesforce-App werden wie folgt ausgewertet: \* Wenn der Benutzerzugriff von einer IP auf der Sperrliste stammt, wird der Blockzugriff verwendet. \* Wenn der Benutzer Mitglied der Salesforce-Mitarbeitergruppe ist und der Risikoscore kleiner als 90 ist, müssen Sie einmal pro Session zur MFA auffordern. \* für alle anderen Benutzer wird der Zugriff verweigert.

Hinweis: Wenn der Benutzer Mitglied der Salesforce-Mitarbeitergruppe ist und der Risikoscore größer als 90 ist, wird der Zugriff abgelehnt.

Hinweis: Wenn Sie mehrere Risikoprovider haben, können Sie Bedingungen basierend auf einem konsolidierten Risikoscore oder Risikoscore definieren, der von einer bestimmten Gruppe von Providern generiert wird.

## Aufgabe 3: Risikobewertung durch Endbenutzer

*   _Personen_:
    *   Administrator
    *   Endbenutzer

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Navigieren Sie zu _Benutzer_, und klicken Sie auf _Hinzufügen_.
    
3.  Erstellen Sie einen Benutzer wie den folgenden, und klicken Sie auf "Fertigstellen".
    
    *   Vorname: Vorname
    *   Nachname: Nachname
    *   Benutzername/E-Mail: firstname.lastname+risk@domain.tld
    
    ![Bild](images/L6007.png)
    
4.  Melden Sie sich als Administrator bei Salesforce an, und erstellen Sie einen regulären Benutzer mit denselben Details wie oben. Der endgültige Benutzer sollte wie folgt aussehen:
    
    ![Bild](images/L6008.png)
    
5.  Wählen Sie die Gruppe _Mitarbeiter_ aus, und klicken Sie auf _Fertig stellen_. ![Bild](images/L6009.png)
    
6.  _Abmelden_ von IDCS.
    
7.  Aktivieren Sie den Benutzer, indem Sie auf den Aktivierungslink _Account aktivieren_ klicken, der an die E-Mail-Adresse des Benutzers gesendet wurde.
    
    ![Bild](images/L6010.png)
    
8.  Geben Sie bei entsprechender Aufforderung ein _Kennwort_ ein.
    
9.  Nachdem Sie die Seite "Herzlichen Glückwunsch" erhalten haben, _melden Sie sich_ bei IDCS mit Ihrem neu erstellten Benutzer an.
    
10.  Klicken Sie auf die Kachel _Salesforce Chatter_, um die Anwendung auf einer neuen Registerkarte zu starten.
    
    ![Bild](images/L6011.png)
    

Hinweis: Wenn Ihre Anmeldung basierend auf der zuvor konfigurierten IP auf der Sperrliste nicht zulässig ist, melden Sie sich erneut als Administrator an, ersetzen Sie vorhandene blockierte IPs durch einen Dummy-Wert, wie 10.0.0.0, und wiederholen Sie die obigen Schritte.

11.  Klicken Sie auf _Aktivieren_, um die 2-Schritt-Verifizierung zu aktivieren
    
12.  Registrieren Sie eine 2-Schritt-Verifizierung, d.h. eine mobile App.
    
13.  Klicken Sie auf _Fertig_, und Sie werden bei Salesforce angemeldet.
    
14.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an
    
    https:///ui/v1/adminconsole
    
15.  Klicken Sie auf "Benutzer", und navigieren Sie zu dem Benutzer, der neu zum Testen des Risikos erstellt wurde. Beachten Sie, dass jedem Benutzer jetzt ein Risiko zugeordnet ist. Klicken Sie auf diesen Benutzer.
    
    ![Bild](images/L6012.png)
    
16.  Klicken Sie auf die Registerkarte _Sicherheit_. Der Standardrisikoprovider hat für diesen Benutzer basierend auf den aktivierten und konfigurierten Risikoereignissen einen Risikoscore von 15 oder 30 generiert.
    
    ![Bild](images/L6013.png)
    

Hinweis: Wenn mehrere Risikoprovider konfiguriert wurden, wird für jeden Mitarbeiter eine Kachel angezeigt.

17.  Klicken Sie auf die Kachel "Standardrisikoprovider", und scrollen Sie dann nach unten, um Details zu Risikovorfällen anzuzeigen. Auf dieser Seite erhalten Sie eine historische Ansicht des Risikoscores und die Liste der Risikoereignisse.
    
    ![Bild](images/L6014.png)
    

## Weitere Informationen

Diese Arbeitsmappe enthält in erster Linie die erforderlichen Anweisungen und den Kontext, mit denen Sie die Übungen im Oracle Identity Cloud Service-Workshop abschließen können. Wenn Sie weitere Informationen zur Oracle-Lösung wünschen, können Sie sich an Ihr lokales Oracle-Accountteam wenden und/oder einige der folgenden öffentlich verfügbaren Informationen zur Lösung überprüfen.

*   [Identity Cloud Service-Website](https://cloud.oracle.com/en_US/identity)
*   [Lösungsdatenblatt](http://www.oracle.com/technetwork/middleware/id-mgmt/overview/idcs-datasheet-3097388.pdf)
*   [Produktdokumentation](http://docs.oracle.com/cloud/latest/identity-cloud/index.html)
*   [Blogs](https://blogs.oracle.com/imc/)

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020