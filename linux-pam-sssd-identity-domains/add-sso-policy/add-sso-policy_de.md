# SSO-Policy zur Einführung von MFA erstellen

## Einführung

In dieser Übung wird gezeigt, wie Sie eine Single Sign-On-Policy hinzufügen, um MFA in die Regel aufzunehmen.

_Geschätzte Zeit:_ 5 Minuten

### Ziele

*   Erstellen Sie eine Single Sign-On-(SSO-)Policy.
*   Weisen Sie die vertrauliche Anwendung der SSO-Policy zu.

## Aufgabe 1: Single Sign-On-(SSO-)Policy erstellen und Anwendung hinzufügen

1.  Melden Sie sich bei Ihren OCI-IAM-Identitätsdomains an, um auf die **OCI-Konsole** zuzugreifen. Nachdem Sie sich angemeldet haben, navigieren Sie unter **Identität und Sicherheit** zu **Domains**.Wählen Sie jetzt die zuvor bereitgestellte **Identitätsdomain** aus.
    
    ![Identität&amp;Sicherheit](./images/identity-security.png "Identität&amp;Sicherheit")
    
    ![Domains](./images/domains.png "Domains")
    
2.  Klicken Sie auf die **Anmelde-Policys** und dann auf **Anmelde-Policy erstellen**.
    
    ![Anmelde-Policy](./images/sign-on-policy.png "Anmelde-Policy")
    
3.  Geben Sie im Abschnitt **Policy hinzufügen** einen _Namen_ der Policy an. Geben Sie eine entsprechende **RuleName** an, und scrollen Sie nach unten zum Abschnitt **Aktionen**, um **Beliebiger Faktor** und **Jedes Mal** in der Option _Häufigkeit_ auszuwählen.
    
    ![Policy-Name](./images/policy-name.png "Policy-Name")
    
    ![Häufigkeit](./images/frequency.png "Häufigkeit")
    
    ![Regel](./images/rule.png "Regel")
    
4.  Clieck Wählen Sie neben dem Abschnitt **Apps hinzufügen** die _vertrauliche App_ aus, die zuvor von _Stack2 -Bereitstellen_ erstellt wurde. Wählen Sie anschließend **Schließen**, **Anmelde-Policy aktivieren** aus.
    
    ![Add-Apps](./images/add-apps.png "Add-Apps")
    
    ![Schließen](./images/close.png "Schließen")
    
    ![aktivieren](./images/activate.png "aktivieren")
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023