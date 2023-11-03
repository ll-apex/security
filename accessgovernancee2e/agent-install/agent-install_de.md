# Oracle Access Governance-Agent installieren und konfigurieren

## Einführung

OAG-Agent wird installiert und konfiguriert.

_Geschätzte Laborzeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Access Governance Agent installieren](videohub:1_u4xrvpak)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Agent herunterladen, um die Integration mit OIG auszuführen
*   **OAG-Agent** installieren und konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein gültiger Oracle OCI-Mandant mit OCI-Administratorberechtigungen.

## Aufgabe 1: Mit Oracle Identity Governance integrieren

1.  Klicken Sie auf der Homepage des Oracle Access Governance-Service _siehe Übung 6: Aufgabe 1_ auf das Navigationsmenüsymbol, und wählen Sie **Serviceadministration** und dann **Verbundene Systeme** aus.
    
    ![Access Governance-Konsole - Verbundene Systeme](images/connected-systems.png)
    
2.  Klicken Sie auf **Verbundenes System hinzufügen**
    
    ![Hinzufügen - Verbundenes System](images/add-connected-system.png)
    
3.  Wählen Sie in der Kachel **Möchten Sie eine Verbindung zu einem Identity Governance-System herstellen?** die Schaltfläche **Hinzufügen** aus. ![Access Governance-Konsole - Verbundene Systeme - Hinzufügen](images/connected-system-page.png)
    
4.  Klicken Sie im Popup-Fenster auf **Schließen**, um zur Seite **Identity-Governance-System hinzufügen** zu navigieren und mit der Konfiguration zu beginnen.
    
    ![Popup-Fenster schließen](images/pop-up.png)
    
5.  Wählen Sie im Schritt **System auswählen** die Kachel für **Oracle Identity Governance** aus, um den Agent für ein verbundenes Oracle Identity Governance-Zielsystem zu konfigurieren, und klicken Sie auf **Weiter**.
    
    ![Access Governance-Konsole - Verbundene Systeme - Weiter](images/select-oig.png)
    
6.  Geben Sie im Schritt **Details eingeben** die folgenden Details ein:
    
    *   **Name:** oag
    *   **Beschreibung:** oag
    *   **Klicken Sie auf "Next".**
    
    ![Access Governance-Konsole - Connected Systems - OIG](images/oag-select-system.png)
    
7.  Geben Sie im Schritt **Konfiguration** die Verbindungsdetails für das Zielsystem ein:
    
    **JDBC-URL:** Ersetzen Sie den Platzhalter in der folgenden URL durch die private IP der Compute-Instanz. Die private IP der Compute-Instanz finden Sie unter _Übung 5 zu Aufgabe 3_.
    
        <copy>jdbc:oracle:thin:@//<--privateipofyourcomputeinstance-->:1521/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM</copy>
        
    
    **Benutzername für die OIG-Datenbank:**
    
        <copy>DEV_OIM</copy>
        
    
    **Passwort:**
    
        <copy>Welcome1</copy>
        
    
    **Kennwort bestätigen:**
    
        <copy>Welcome1</copy>
        
    
    **OIG-Server-URL:** Ersetzen Sie den Platzhalter in der folgenden URL durch die private IP der Compute-Instanz. Die private IP der Compute-Instanz finden Sie unter _Übung 3: Aufgabe 3_.
    
        <copy>http://<--privateipofyourcomputeinstance-->:14000</copy>
        
    
    **Benutzername für OIG-Server:**
    
        <copy>xelsysadm</copy>
        
    
    **OIG-Serverbenutzerkennwort:**
    
        <copy>Welcome1</copy>
        
    
    **OIG-Server - Kennwort bestätigen:**
    
        <copy>Welcome1</copy>
        
    
    ![Details konfigurieren](images/oag-connection-details.png)
    
8.  Wählen Sie im Schritt "Agent herunterladen" den _Link herunterladen_ aus, und laden Sie die Agent-ZIP-Datei herunter. Die ZIP-Datei ist vorhanden in: /home/opc/Downloads
    
    ![Agent herunterladen](images/oag-download-link.png)
    
9.  Sie können die heruntergeladene Agent-ZIP-Datei prüfen.
    
    ![Zum Dateisystem navigieren](images/locate-zip.png)
    
    ![Prüfen Sie die ZIP-Datei](images/verify-zip.png)
    

## Aufgabe 2: OAG-Agent auf der Compute-Instanz installieren und konfigurieren

1.  Öffnen Sie die Terminal Session.
    
    ![Terminal-Session öffnen](images/open-terminal-window.png)
    
2.  Verschieben Sie die heruntergeladene ZIP-Datei (oag.zip) im Ordner /home/opc/Downloads in den Ordner /home/opc/zip\_oag.
    
        <copy>mv /home/opc/Downloads/oag.zip /home/opc/zip_oag</copy>
        
    
    ![Verschieben Sie den OAG-Agent in zip_oag.](images/move-oag-agent.png)
    
    Stellen Sie sicher, dass die Agent-ZIP-Adresse (oag.zip) im Ordner zip\_oag vorhanden ist.
    
        <copy>cd /home/opc/zip_oag</copy>
        <copy>ls</copy>
        
    
    ![Prüfen Sie die Agent-ZIP-Adresse oag.zip](images/env_setup.png)
    
3.  Umgebungsvariablen mit dem folgenden Befehl festlegen:
    
        <copy>cd ~</copy>
        <copy>source oag_agent.env</copy>
        
    
    ![Umgebungsvariable initialisieren](images/terminal-oag.png)
    
4.  Agent installieren
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --agentpackage /home/opc/zip_oag/oag.zip --install</copy>
        
    
    ![Agent installieren](images/agent-install.png)
    
5.  Agent starten
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --start</copy>
        
    
    ![Agent starten](images/agent-start.png)
    
6.  Agent überprüfen
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --status</copy>
        
    
    ![Agent überprüfen](images/agent-status.png)
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Mitwirkende** - Edward Lu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023