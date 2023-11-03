# So stellen Sie eine Verbindung zu Oracle Database und Oracle Identity Governance her

## Einführung

Verbindung zu Oracle Database und Oracle Identity Governance herstellen

*   Persona: Identitätsdomainadministrator

_Geschätzte Zeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Video Hub - Video ohne Größenanpassung](videohub:1_61qwyi4k)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   So stellen Sie eine Verbindung zu Oracle Database und Oracle Identity Governance her

## Aufgabe 1: Prüfen, ob Docker hochgefahren und gestartet ist

1.  Öffnen Sie eine Terminal Session.
    
2.  Überprüfen Sie die Version des Dockers.
    
        <copy>docker -v</copy>
        
    
        Expected output: Docker version 23.0.0, build e92dd87
        
3.  Status validieren, um zu prüfen, ob der Dockerservice hochgefahren/ausgeführt ist
    
        <copy>systemctl status docker</copy>
        
    
    Geben Sie **Ctrl+C** ein, um zur Eingabeaufforderung zurückzukehren
    

## Aufgabe 2: Oracle Identity Governance-(OIG-)DB-Service starten

1.  Wechseln Sie in das Verzeichnis, in dem sich die Skriptdateien befinden.
    
        <copy>cd /scratch/idmqa/scripts</copy>
        
2.  Listen Sie die Dateien im Verzeichnis auf.
    
        <copy>ls</copy>
        
3.  Starten Sie DB und alle Server manuell mit den folgenden Skripten.
    
        <copy>./start_db.sh</copy>
        
    
    Warten Sie, bis DB gestartet wird.
    
4.  Starten Sie jetzt die OIG-Services mit dem folgenden Befehl.
    
        <copy>./start_all_servers.sh</copy>
        

## Aufgabe 3: Mit Oracle Identity Governance integrieren

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
    

## Aufgabe 4: OAG-Agent auf der Compute-Instanz installieren und konfigurieren

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
    

## Aufgabe 5: Verbindung zu Oracle Database herstellen und DB-Agent herunterladen

1.  Navigieren Sie zur Seite "Verbundene Systeme" der Oracle Access Governance-Konsole, indem Sie die folgenden Schritte ausführen: Wählen Sie im Navigationsmenüsymbol von Oracle Access Governance die Option "Serviceadministration" > "Verbundene Systeme" aus. Klicken Sie auf die Schaltfläche "Verbundenes System hinzufügen", um den Workflow zu starten.
    
2.  Klicken Sie auf der Seite "Verbundenes System hinzufügen" auf der Kachel Möchten Sie eine Verbindung zu einem Datenbankverwaltungssystem herstellen? auf die Schaltfläche Hinzufügen.
    
3.  Wählen Sie im Schritt "System auswählen" des Workflows "Datenbankbenutzerverwaltung (Oracle DB)", und klicken Sie auf "Weiter".
    
4.  Geben Sie im Schritt "Details eingeben" des Workflows die Details für das verbundene System ein:
    
         * What do you want to call your database : OAG-DB
         * How do you want to describe this database: OAG-DB
        
    
    Klicken Sie auf "Next".
    
5.  Geben Sie im Schritt "Konfigurieren" des Workflows die erforderlichen Konfigurationsdetails ein, damit Oracle Access Governance eine Verbindung zur Zieldatenbank herstellen kann.
    
         * Easy Connect URL for Database: jdbc:oracle:thin:@//<—privateipaddressofcomputeinstance-->/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM
        
         * User Name: sys as sysdba
        
         * Password: Welcome1
        
         * Confirm password: Welcome1
        
6.  Klicken Sie im rechten Fensterbereich auf "Ausgewählte Elemente". Wenn Sie mit den eingegebenen Details zufrieden sind, wählen Sie "Hinzufügen", um das verbundene System zu erstellen.
    
7.  Im Schritt "Fertigstellen" des Workflows werden Sie aufgefordert, den Agent herunterzuladen, mit dem Sie eine Schnittstelle zwischen Oracle Access Governance und Oracle Database herstellen. Wählen Sie den Link "Herunterladen", um die Agent-ZIP-Datei in die Umgebung herunterzuladen, in der der der Agent ausgeführt werden soll.
    

## Aufgabe 6: DB-Agent im Zielsystem installieren

1.  Öffnen Sie das Terminal.
    
2.  Erstellen Sie das Volume.
    
        <copy>mkdir  /home/opc/vol_oag_db</copy>
        
3.  Prüfen Sie, ob die Agent-ZIP-Datei (OAG-DB.zip) im Ordner "Downloads" vorhanden ist.
    
        <copy> cd /home/opc/Downloads
        ls</copy>
        
4.  Umgebungsvariablen mit dem folgenden Befehl festlegen:
    
        <copy> cd ~
        source oag_agent.env</copy>
        
5.  Installieren Sie den Agent mit den folgenden Befehlen:
    
        <copy>curl https://raw.githubusercontent.com/oracle/docker-images/main/OracleIdentityGovernance/samples/scripts/agentManagement.sh -o agentManagement.sh; 
        

\`\`\`\`\`\` sh agentManagement.sh --volume /home/opc/vol\_oag\_db --agentpackage /home/opc/Downloads/OAG-DB.zip --install \`\` 3. Starten Sie den Agent mit dem folgenden Befehl:

      ```
      <copy>sh agentManagement.sh --volume /home/opc/vol_oag_db --start</copy>
      ``` 
    

## Aufgabe 7: Agent-Installation prüfen

1.  Melden Sie sich bei der Oracle Access Governance-Konsole an, und wählen Sie das Symbol "Navigationsmenü", um das Navigationsmenü anzuzeigen.
2.  Wählen Sie in der Oracle Access Governance-Konsole im Navigationsmenü die Option "Serviceadministration → Verbundene Systeme".
3.  Auf dem Bildschirm "Verbundene Systeme" zeigt die Kachel mit dem Namen des verbundenen Systems den Status "Warten auf erste Verbindung" an. Klicken Sie auf Verbindung verwalten.
4.  Im Aktivitätslog unten auf der Seite wird der Status des Validierungsvorgangs "Ausstehend" angezeigt, während der Agent hochgefahren wird. Wenn der Agent nicht hochgefahren wird, finden Sie in den Installations- und Vorgangslogs für den Agent Probleme.
5.  Nach dem Hochfahren des Agent wird für den Validierungsvorgang der Status "Erfolgreich" angezeigt.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Danksagungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu