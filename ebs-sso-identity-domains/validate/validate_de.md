# Validieren

## Einführung

In dieser Übung erfahren Sie, wie Sie den SSO-Ablauf zu Ihrer EBS-Anwendung über **OCI IAM-Identitätsdomains** testen können.

### Ziele

*   EBS Asserter-Konfiguration validieren
*   Single Sign-On mit Oracle E-Business Suite testen

## Aufgabe 1: EBS Asserter-Konfiguration validieren

Nachdem **Stack 2 - Konfigurieren** erfolgreich bereitgestellt und die EBS-Profiländerungen durchgeführt wurden, greifen Sie auf die unten genannte EBS Asserter-URL zu.

*   **https://ebsasserter.example.com:7004/ebs/validate**

Der Browser zeigt das Ergebnis der EBS Asserter-Konfigurationen an.

![1 erfassen](./images/capture1.png "1 erfassen")

## Aufgabe 2: Standardkennwort für SSO-Testbenutzer ändern

Im Folgenden werden die Benutzer aufgeführt, die als SSO-Beispieltestbenutzer erstellt wurden.

![3 erfassen](./images/capture3.png "3 erfassen")

    **Default Password** - "Welcome@1234567890"
    

## Aufgabe 3: Single Sign-On mit Oracle E-Business Suite testen

1.  SSO mit dem direkten EBS Asserter-URL-Link testen
    
    1.  Öffnen Sie ein Browserfenster, und geben Sie die URL für EBS Asserter ein - **https://ebsasserter.example.com:7004/ebs**
    2.  Die Seite **OCI IAM-Identitätsdomains-Signatur** wird angezeigt. Melden Sie sich mit dem **Benutzernamen und Kennwort** des zuvor erstellten Benutzers an.
    3.  Nach erfolgreicher Authentifizierung wird der Benutzer zur **Oracle E-Business Suite**\-Homepage weitergeleitet, ohne **EBS-Zugangsdaten** eingeben zu müssen.
    4.  Wenn die **Oracle EBS**\-Homepage angezeigt wird, prüfen Sie den **angemeldeten Benutzernamen**.
    5.  Melden Sie sich bei Oracle EBS ab. Der Browser wird zur **OCI IAM-Identitätsdomains-Signatur** auf der Seite "In" umgeleitet.
    
    ![Erfassen 2](./images/capture2.png "Erfassen 2")
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023