# Bereinigung virtuelles Cloud-Netzwerk für Access Governance

## Einführung

Nach Abschluss Ihrer Übungen sollten Sie eine Bereinigung durchführen, um das virtuelle Access Governance Cloud-Netzwerk (ag-vcn) zu entsorgen. In dieser Übung können Sie die Ressource ordnungsgemäß löschen.

_Geschätzte Laborzeit_: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Zerstören Sie das virtuelle Cloud-Netzwerk von Access Governance (ag-vcn)

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten
    *   Übung: Umgebungssetup

## Aufgabe 1: Virtuelles Cloud-Netzwerk von Access Governance zerstören

1.  Bei Oracle Cloud anmelden
    
    ![OCI-Konsole öffnen](images/oci-homepage.png)
    
2.  Klicken Sie in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das Menü "Navigation" anzuzeigen. Klicken Sie im Navigationsmenü auf Networking. Wählen Sie "Virtuelle Cloud-Netzwerke" aus der Produktliste aus.
    
    ![Zu Access Governance navigieren](images/navigate-vcn.png)
    
3.  Klicken Sie auf der Seite "Virtuelle Cloud-Netzwerke" auf das virtuelle Cloud-Netzwerk **ag-vcn**, das Sie für diese Übung erstellt haben.
    
    ![Status des Dockers validieren](images/select-agvcn.png)
    
4.  Klicken Sie auf _Löschen_. Prompt wird angezeigt. Prüfen Sie die Option _Scannen_.
    
    ![Status des Dockers validieren](images/delete-agvcn.png)
    
5.  Klicken Sie nach Abschluss des Scans auf _Alle löschen_. Jetzt wurden die virtuellen Cloud-Netzwerke (ag-vcn) und die zugehörigen Ressourcen erfolgreich gelöscht.
    
    ![Status des Dockers validieren](images/delete-all-vcn.png)
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023