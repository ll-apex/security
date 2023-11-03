# Bedingte MFA in IDCS aktivieren

## Einführung

MFA (Multi-Factor Authentication) ist eine Authentifizierungsmethode, bei der mehrere Faktoren zur Verifizierung der Benutzeridentität verwendet werden müssen.

Wenn MFA in Oracle Identity Cloud Service aktiviert ist und sich ein Benutzer bei einer Anwendung anmeldet, wird er aufgefordert, seinen Benutzernamen und sein Kennwort einzugeben. Dies ist der erste Faktor, den er kennt. Der Benutzer muss dann einen zweiten Verifizierungstyp angeben. Dies wird als 2-Schritt-Verifizierung bezeichnet. Die beiden Faktoren arbeiten zusammen, um eine zusätzliche Sicherheitsebene hinzuzufügen, indem sie entweder zusätzliche Informationen oder ein zweites Gerät verwenden, um die Identität des Benutzers zu überprüfen und den Anmeldeprozess abzuschließen.

Benutzer sind zunehmend miteinander verbunden und greifen von überall auf ihre Konten und Anwendungen zu. Wenn Sie als Administrator MFA zusätzlich zum herkömmlichen Benutzernamen und Kennwort hinzufügen, können Sie den Zugriff auf Daten und Anwendungen schützen. Dies reduziert auch die Wahrscheinlichkeit von Online-Identitätsdiebstahl und -betrug, was Ihre Geschäftsanwendungen sichert, selbst wenn ein Accountkennwort kompromittiert wird.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   MFA-Faktoren auswählen, die Sie aktivieren möchten
*   Anmelde-Policys für Salesforce definieren
*   Mit MFA als Endbenutzer anmelden
*   Blockzugriff über Policy
*   Aktualisierte Policy testen

### Voraussetzungen

*   Free Tier- oder kostenpflichtige Oracle-Accounts
*   Ein Google-Account
*   Ein Salesforce-Entwickleraccount
*   Übung 2 wurde abgeschlossen

## Aufgabe 1: MFA-Faktoren aktivieren

*   _Personen_:
    *   Administrator

1.  Melden Sie sich als Administrator bei der IDCS-Admin-Konsole an:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Navigieren Sie zu _Sicherheit_, und wählen Sie _MFA_ aus.
    
3.  Wählen Sie die _MFA-Faktoren_ aus, die Sie aktivieren möchten, und klicken Sie dann auf _Speichern_.
    
    ![Bild](images/L5001.png)
    

## Aufgabe 2: Anmelde-Policy für Salesforce definieren

*   _Personen_:
    *   Administrator

Mit einer Anmelde-Policy können Identitätsdomainadministratoren, Sicherheitsadministratoren und Anwendungsadministratoren Kriterien definieren, anhand derer Oracle Identity Cloud Service bestimmt, ob ein Benutzer sich bei Oracle Identity Cloud Service anmelden oder den Zugriff eines Benutzers auf Oracle Identity Cloud Service verhindern kann.

Mit Oracle Identity Cloud Service erhalten Sie eine Standardanmelde-Policy, die eine Standardanmelderegel enthält. Oracle Identity Cloud Service wertet die Kriterien der Regel für jeden Benutzer aus, der versucht, sich bei Oracle Identity Cloud Service anzumelden. Standardmäßig können sich alle Benutzer mit dieser Regel bei Oracle Identity Cloud Service anmelden. Dies bedeutet, dass die Authentifizierung, die der Benutzer verwendet, entweder die lokale Authentifizierung durch Angabe eines Benutzernamens und Kennworts oder die Authentifizierung mit einem externen Identitätsprovider ausreicht.

Sie können jedoch auf dieser Policy aufbauen, indem Sie weitere Anmelderegeln hinzufügen. Durch Hinzufügen dieser Regeln können Sie verhindern, dass sich einige Benutzer bei Oracle Identity Cloud Service anmelden. Sie können die Anmeldung auch zulassen, aber einen zusätzlichen Faktor für den Zugriff auf Ressourcen angeben, die durch Oracle Identity Cloud Service geschützt sind, wie die Konsole "Mein Profil" oder die Identity Cloud Service-Konsole.

1.  Navigieren Sie zu _Sicherheit_, und wählen Sie _Netzwerkperimeter_ aus.
    
    ![Bild](images/L5002.png)
    
        A network perimeter contains a list of IP addresses.
        
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        
        You can also configure Oracle Identity Cloud Service so that users can log in, using only IP addresses contained in the network perimeter. This is known as whitelisting, where users who attempt to sign in to Oracle Identity Cloud Service with these IP addresses will be accepted. Whitelisting is the reverse of blacklisting, the practice of identifying IP addresses that are suspicious, and as a result, will be denied access to Oracle Identity Cloud Service.
        
2.  Klicken Sie auf _Hinzufügen_, um einen neuen Netzwerkperimeter hinzuzufügen. Geben Sie einen IP-Bereich ein, wie unten dargestellt. Legen Sie im Moment einen Dummy-Bereich oder sogar eine Dummy-Adresse (z.B. 10.0.0.0) fest, damit keine Authentifizierung abgelehnt wird, und klicken Sie auf _Speichern_.
    
    ![Bild](images/L5003.png)
    
3.  Navigieren Sie zu _Sicherheit_, und wählen Sie _Anmelde-Policys_ aus. Um eine Anmelde-Policy für die Salesforce-App zu definieren, klicken Sie auf _Hinzufügen_.
    
    ![Bild](images/L5004.png)
    
        Note that a Default Sign-On Policy exists and is enabled by default. The default policy applies to all application that have not been explicitly mapped to a sign on policy.
        
4.  Geben Sie im Abschnitt "Details" einen _Policy-Namen_ und eine _Beschreibung_ ein, und klicken Sie auf die Schaltfläche _\>_, um zum nächsten Abschnitt zu wechseln.
    
    ![Bild](images/L5005.png)
    
5.  Klicken Sie im Abschnitt _Anmelderegeln_ auf _Regeln hinzufügen_, um eine neue Regel hinzuzufügen.
    
    ![Bild](images/L5006.png)
    
6.  Erstellen Sie eine Regel, die zur Eingabe von MFA auffordert, wenn ein Benutzer zur Gruppe "Mitarbeiter" gehört. Stellen Sie sicher, dass die folgenden Parameter festgelegt sind:
    
    | Parameter | Wert |
    | --- | --- |
    | Regelname | MFA für Mitarbeiter |
    | Und Mitglied dieser Gruppen ist | Mitarbeiter |
    | Prompt für zusätzlichen Faktor | Aktiviert |
    | Anmeldung | Optional |
    | Häufigkeit zusätzlicher Faktoren bei Verwendung eines vertrauenswürdigen Geräts | Einmal pro Session |
    
7.  Klicken Sie auf _Speichern_, um die Regel zu speichern.
    
    ![Bild](images/L5007.png)
    
8.  Klicken Sie auf _Hinzufügen_, um eine zweite Regel hinzuzufügen.
    
    ![Bild](images/L5008.png)
    
9.  Stellen Sie sicher, dass die folgenden Parameter festgelegt sind:
    
    | Parameter | Wert |
    | --- | --- |
    | Regelname | IP auf Sperrliste |
    | Und die Client-IP-Adresse des Benutzers lautet | In einem oder mehreren dieser Netzwerke gesperrte IPs |
    | Zugriff ist | abgelehnt |
    
10.  Klicken Sie auf _Speichern_, um die Regel zu speichern.
    
    ![Bild](images/L5009.png)
    
        Note the Sign-On Rules are ordered. The actions (allow/deny/prompt for MFA) are determined by the first rule where the conditions match. If none of the rule conditions match the default action is deny.
        
11.  Um die Regel _Sperrte IP_ nach oben zu verschieben, klicken Sie auf das Symbol "Neu anordnen", und ziehen Sie die Regel nach oben. Klicken Sie auf _Fertigstellen_.
    
    ![Bild](images/L5010.png)
    
        The above rules are evaluated as below
        
        a. if user access is from a blacklisted IP the deny access.
        b. if user is a member of Salesforce Employee group, then prompt for MFA once per session.
        c. for all other users deny access.
        
12.  Klicken Sie auf die Registerkarte _Apps_ und dann auf die Schaltfläche _Zuweisen_.
    
    ![Bild](images/L5011.png)
    
13.  Wählen Sie die _Salesforce_\-Anwendung aus, und klicken Sie auf _OK_.
    
    ![Bild](images/L5012.png)
    
14.  Salesforce wurde der Anmelde-Policy erfolgreich hinzugefügt.
    
    ![Bild](images/L5013.png)
    
15.  Navigieren Sie zurück zu _Anmelde-Policys_, und klicken Sie auf _Aktivieren_, um die _Salesforce_\-Policy zu aktivieren.
    
    ![Bild](images/L5014.png)
    

## Aufgabe 3: Mit MFA als Endbenutzer anmelden

*   _Personen_:
    *   Endbenutzer

1.  Melden Sie sich bei der IDCS-Konsole mit einem Benutzeraccount an, der der Salesforce-Anwendung zugewiesen wurde.
    
2.  Klicken Sie auf _Salesforce Chatter_.
    
    ![Bild](images/L5015.png)
    
3.  Sie werden aufgefordert, die 2-Schritt-Verifizierung für Ihr Konto zu aktivieren. Klicken Sie auf _Aktivieren_, um die 2-Schritt-Verifizierung für den Account zu aktivieren. Hinweis: Die Anmeldung wurde in der Anmelde-Policy auf "Optional" gesetzt. Der Endbenutzer kann diesen Schritt überspringen.
    
    ![Bild](images/L5016.png)
    

### MFA-Optionen in Oracle Identity Cloud Service: E-Mail

    Email:
    Users receive an email message that contains a temporary passcode that must be used during log in.
    

A: Oracle legt Ihren Haupt-E-Mail-Account automatisch als MFA-Option fest. Es wird jedoch empfohlen, auf die Option _Als Standard festlegen_ zu klicken.

![Bild](images/L5017.png)

B. Geben Sie den Code ein, der an Ihre E-Mail-Adresse gesendet wurde, und klicken Sie auf _Überprüfen_.

![Bild](images/L5018.png)

C. Jetzt ist Ihre E-Mail-Adresse erfolgreich registriert. Es wird empfohlen, eine zusätzliche Methode einzurichten. Um mit der MFA-Konfiguration fortzufahren, wählen Sie eine andere Methode aus, d.h. _App_. Sie können auch auf _Fertig_ klicken und die MFA-Konfiguration beenden.

![Bild](images/L5019.png)

    Note: Under *My Profile* and under the tab *Security*, further MFA options can be configured at a later point of time.
    

### MFA-Optionen in Oracle Identity Cloud Service: Mobile Authenticator-Anwendung

    Mobile Authenticator Application:
    Users generate an OTP on the OMA App on their device that must be used during log in.
    

A: Laden Sie die _Oracle Mobile Authenticator_\-App auf Ihr Mobiltelefon herunter.

B. Wenn Ihr Handy Internetverbindung hat, scannen Sie den QR-Code.

![Bild](images/L5020.png)

C. Wenn Ihr Mobiltelefon keine Internetverbindung hat oder Sie einen 3rd-Party-Authentikator (z. B. Google Authenticator) verwenden. Aktivieren Sie das Kontrollkästchen _Offline-Modus oder andere Authentikator-App verwenden_, und scannen Sie den neuen generierten Offline-QR-Code. Nachdem Sie den QR-Code gescannt haben, generiert die mobile App alle 30 Sekunden OTPs. ii. Geben Sie das OTP in das Passwortfeld ein, und klicken Sie auf _Verifizieren_.

![Bild](images/L5021.png)

D. Sie können eine zusätzliche 2-Schritt-Verifizierungsmethode anmelden, indem Sie auf eine andere Methode klicken. In diesem Tutorial fahren wir mit _Mobiltelefonnummer_ fort.

### MFA-Optionen in Oracle Identity Cloud Service: Textnachricht (SMS)

    Text Message (SMS):
    Users receive a temporary passcode as a text message (SMS) on their device that must be used during log in.
    

A: Klicken Sie auf _Mobiltelefonnummer_.

B. Geben Sie Ihre Mobiltelefonnummer ein, und klicken Sie auf _Senden_.

![Bild](images/L5022.png)

C. Eine _SMS_ (Textnachricht) mit einem OTP wird an Ihre Mobiltelefonnummer gesendet.

D. Geben Sie das OTP in das Passwortfeld ein, und klicken Sie auf _Verifizieren_.

### MFA-Optionen in Oracle Identity Cloud Service: Sicherheitsfragen

    Security Questions:
    Users are prompted during the sign-in process to correctly answer a defined number of security questions to verify their identity.
    

A: Klicken Sie auf _Sicherheitsfragen_.

B. Wählen Sie die Frage aus, die Sie verwenden möchten, geben Sie die Antworten ein, die Sie speichern möchten, und geben Sie optional Treffer für jede Antwort ein, und klicken Sie auf _Speichern_.

![Bild](images/L5023.png)

C. Klicken Sie auf _Fertig_, um den Prozess abzuschließen. Sie werden nun zu Salesforce weitergeleitet.

![Bild](images/L5024.png)

### MFA-Optionen in Oracle Identity Cloud Service: Backupverifizierungsmethode

    Backup verification method:
    Oracle makes it possible to use a backup verification method in case the current MFA option cannot be entered, i.e. when the configured MFA device is not reachable.
    

A. Greifen Sie auf IDCS zu, und wählen Sie unter "Meine Apps" Salesforce aus. Eine vorkonfigurierte MFA-Option wird angezeigt. In diesem Fall handelt es sich um die _App Authenticator Application_.

B. Klicken Sie auf _Backupverifizierungsmethode verwenden_. Die Liste der 2-Schritt-Verifizierungsmethoden für registriertes Backup wird angezeigt.

![Bild](images/L5025.png)

C. Wählen Sie in diesem Beispiel _Sicherheitsfragen_ aus.

D. Beantworten Sie die Sicherheitsfrage, und klicken Sie auf _Verifizieren_.

![Bild](images/L5026.png)

## Aufgabe 4: Zugriff über Richtlinien blockieren

*   _Personen_:
    
    *   Administrator
    
        Block blacklisted IPs
        After creating a network perimeter, you can prevent users from signing in to Oracle Identity Cloud Service if they use one of the IP addresses in the network perimeter. This is known as blacklisting. A blacklist contains IP addresses or domains that are suspicious. As an example, a user may be trying to sign in to Oracle Identity Cloud Service with an IP address that comes from a country where hacking is rampant.
        

1.  Navigieren Sie zu _Sicherheit_, und wählen Sie _Netzwerkperimeter_ aus. Klicken Sie auf das Menü rechts neben dem zuvor erstellten Netzwerkperimeter für gesperrte IPs, und klicken Sie auf "Bearbeiten". Die zuvor konfigurierte Dummy-IP-Adresse wird angezeigt.
    
    ![Bild](images/L5027.png)
    
2.  Ändern Sie die Dummy-IP in Ihren IP-Bereich:
    

*   Öffnen Sie einen Webbrowser, und greifen Sie auf eine Website zu, die Ihre _öffentliche IP-Adresse_ anzeigt. Beispiel: https://www.whatismyip.com/
    
*   Kopieren Sie Ihre öffentliche IP-Adresse.
    
*   Kehren Sie zu IDCS zurück, ersetzen Sie die Dummy-IP-Adresse durch Ihre öffentliche IP-Adresse, und klicken Sie auf _Speichern_.
    
    ![Bild](images/L5028.png)
    

## Aufgabe 5: Aktualisierte Policy testen

*   _Personen_:
    *   Endbenutzer

1.  Melden Sie sich bei der IDCS-Konsole mit einem Benutzeraccount an, der der Salesforce-Anwendung zugewiesen wurde.
    
2.  Klicken Sie auf die Kachel _Salesforce-Chatter_.
    
    ![Bild](images/L5029.png)
    
3.  Sie verweigern den Zugriff, da Ihre Anforderung von einer IP auf der Sperrliste stammt.
    
    ![Bild](images/L5030.png)
    

Sie können jetzt mit der nächsten Übung fortfahren

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020