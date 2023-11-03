# Authentifizierungs-Flow validieren

## Einführung

In dieser Übung erfahren Sie, wie Sie die Authentifizierung beim Linux-Server mit OCI IAM testen können.

_Geschätzte Zeit:_ 5 Minuten

### Ziele

*   Ändern Sie das _default_\-Passwort.
*   Validieren Sie die Authentifizierung beim Linux-Server mit OCI IAM mit dem _zweiten_ Faktor.

## Aufgabe 1: Standardkennwort für die OCI IAM POSIX-Benutzer ändern

1.  Im Folgenden werden die Benutzer aufgeführt, die als SSO-Beispieltestbenutzer erstellt wurden.
    
    ![Benutzer](./images/users.png "Benutzer")
    
    **Standardpasswort** - "Welcome@1234567890"
    
2.  Melden Sie sich mit der neu erstellten Domain bei der OCI-Konsole an, und geben Sie die Zugangsdaten des _POSIX_\-Benutzers ein.
    
    ![Anmelden](./images/sign-in.png "Anmelden")
    
3.  Setzen Sie das Standardkennwort zurück.
    
    ![Kennwort zurücksetzen](./images/password-reset.png "Kennwort zurücksetzen")
    
4.  Aktivieren Sie die _sichere Verifizierung_, und registrieren Sie Ihr Mobilgerät.
    
    ![enable-mfa](./images/enable-mfa.png "enable-mfa")
    
    ![Scan-QR](./images/scan-qr-code.png "Scan-QR")
    
5.  Klicken Sie auf **Fertig**, und fahren Sie mit _Aufgabe 2_ fort.
    
    ![Registriert](./images/enrolled.png "Registriert")
    

## Aufgabe 2: Authentifizierung mit MFA validieren

Nachdem **Stack 2 - Konfigurieren** erfolgreich bereitgestellt wurde, führen Sie die unten genannten Schritte aus.

*   Stellen Sie eine SSH-Verbindung zur Linux-Umgebung her, in der das OCI IAM Linux Pluggable Authentication Module (PAM) installiert ist.
    
*   Wenn Sie dazu aufgefordert werden, geben Sie das Kennwort für den OCI-IAM-_POSIX_\-Benutzer ein. Eine _PUSH_\-Benachrichtigung wird dann an das registrierte Mobilgerät gesendet. Tippen Sie in der Benachrichtigung auf **Zulassen**, und drücken Sie dann auf dem Bildschirm die **Eingabetaste**.
    
    ![Validieren](./images/validate.png "Validieren")
    

## Abschluss

In dieser Übung konnten wir das Kennwort des Testbenutzers und die validierte Benutzerauthentifizierung zusammen mit einem _zweiten_ Faktor erfolgreich mit der Identitätsdomain in den Linux-Server ändern.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023