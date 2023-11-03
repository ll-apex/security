# Umgebung prüfen und initialisieren

## Einführung

In dieser Übung prüfen wir das Setup der Umgebung und starten alle Services, die zum erfolgreichen Ausführen dieses Workshops erforderlich sind.

_Geschätzte Laborzeit_: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Umgebungsinitialisierung](videohub:1_9mdts598)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Verifizieren Sie den Docker
*   OIG-Services starten
*   Status aller Server prüfen
*   Private IP-Adresse der Compute-Instanz prüfen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein gültiger Oracle OCI-Mandant mit OCI-Administratorberechtigungen.
    
    _Hinweis:_ Ab dieser Übung bis zum Abschluss des Workshops müssen alle Schritte nur innerhalb der Workshop-URL ausgeführt werden.
    

## Aufgabe 1: Prüfen, ob Docker hochgefahren und gestartet ist

1.  Öffnen Sie eine Terminal Session.
    
    ![Terminal öffnen](images/open-terminal.png)
    
    Die Terminalsession wurde gestartet.
    
    ![Terminalfenster](images/terminal-window.png)
    
2.  Überprüfen Sie die Version des Dockers.
    
        <copy>docker -v</copy>
        
    
    ![Überprüfen Sie die Version des Dockers](images/docker-version.png)
    
        Expected output: Docker version 23.0.0, build e92dd87
        
3.  Status validieren, um zu prüfen, ob der Dockerservice hochgefahren/ausgeführt ist
    
        <copy>systemctl status docker</copy>
        
    
    ![Status des Dockers validieren](images/docker-info.png)
    
    Geben Sie **Ctrl+C** ein, um zur Eingabeaufforderung zurückzukehren
    

## Aufgabe 2: Oracle Identity Governance-(OIG-)Services starten

1.  Wechseln Sie in das Verzeichnis, in dem sich die Skriptdateien befinden.
    
        <copy>cd /scratch/idmqa/scripts</copy>
        
    
    ![In Speicherort von Skriptdateien verschieben](images/script-file.png)
    
2.  Listen Sie die Dateien im Verzeichnis auf.
    
        <copy>ls</copy>
        
    
    ![Liste der Dateien im Verzeichnis](images/list-files.png)
    
3.  Starten Sie DB und alle Server manuell mit den folgenden Skripten.
    
        <copy>./start_db.sh</copy>
        
    
    Warten Sie, bis DB gestartet wird. Danach starten Sie alle Server.
    
    ![DB-Server gestartet](images/start-db.png)
    
    ![DB-Server gestartet](images/db-started.png)
    
        <copy>./start_all_servers.sh</copy>
        
    
    ![Alle Server werden gestartet](images/start-all-servers.png)
    

## Aufgabe 3: Private IP-Adresse der Compute-Instanz prüfen

1.  Starten Sie ein Browserfenster. Melden Sie sich mit der unten angegebenen URL bei der OCI-Konsole an. Die Anmeldeseite für den OCI-Account wird angezeigt. Geben Sie den Benutzernamen und das Kennwort ein, die bei der Registrierung angegeben wurden.
    
        <copy>https://console.us-ashburn-1.oraclecloud.com/</copy>
        
2.  Klicken Sie in der oberen linken Ecke auf das Symbol "Navigationsmenü", um das _Navigationsmenü anzuzeigen._ Wählen Sie im _Navigationsmenü_ die Option _Compute_ aus. Wählen Sie _Instanzen_ aus der Produktliste.
    
    ![In Speicherort von Skriptdateien verschieben](images/oci-console.png) ![Liste der Dateien im Verzeichnis](images/compute-instance.png)
    
3.  Notedown die private IP-Adresse der Compute-Instanz als Referenz. Wir werden sie in den weiteren Labors verwenden müssen.
    
    ![Liste der Dateien im Verzeichnis](images/private-ip.png)
    
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