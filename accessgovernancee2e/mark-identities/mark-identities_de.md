# Identitäten markieren

## Einführung

Access Governance-Administratoren (Pamela Green) aktivieren die Identitäten.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Access Governance-Administrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Identitäten aktivieren

## Aufgabe 1: Bei Oracle Access Governance-Konsole anmelden

1.  Gehen Sie in Ihrem Browser mit der in _Übung 3: Aufgabe 1_ genannten URL zur Oracle Access Governance-Konsole.
    
2.  Benutzernamen und Kennwort für **Oracle Access Governance Administrator** eingeben (Pamela Green)
    
    **Benutzername:**
    
        <copy>pamela.green</copy>
        
    
    **Passwort:**
    
        <copy>Oracl@123456</copy>
        

Sie werden zur Homepage der Oracle Access Governance-Konsole weitergeleitet.

![Access Governance-Homepage](images/ag-homepage.png)

## Aufgabe 2: Identitäten aktivieren

In dieser Aufgabe wählen Sie die Identitäten aus, die Sie in Ihren Service aufnehmen möchten.

1.  Navigieren Sie in der Oracle Access Governance-Konsole zu "Service Administration" -> "Identitäten verwalten".

![Navigieren Identitäten verwalten](images/navigate-manage-identities.png)

2.  Wählen Sie die Option **Beliebige** für den Bedingungsabgleich aus.
    
    ![Identitäten verwalten (Seite)](images/select-any.png)
    
3.  Wählen Sie die folgenden Optionen für die Bedingung aus, die mit den Identitäten übereinstimmen sollen, die Sie einschließen möchten.
    
    *   Attribut auswählen: Status
    *   Operator auswählen: Enthält
    *   Attributwert: Aktiv
    
    **Eingabetaste** drücken
    
    ![Identitäten verwalten (Seite)](images/status-rule.png)
    
4.  Klicken Sie auf **Übersicht basierend auf der obigen Regel anzeigen**. Die Identitäten, die mit der Regel übereinstimmen, werden angezeigt.
    
5.  Schließen Sie das Popup, und klicken Sie auf **Speichern**
    

![Identitäten verwalten (Seite)](images/identities-user.png)

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023