# Oracle Native Network Encryption (NNE)

## Einführung

In diesem Workshop werden die Funktionen von Oracle Native Network Encryption (NNE) vorgestellt. Es gibt dem Benutzer die Möglichkeit zu lernen, wie man diese Funktion konfiguriert, um seine Daten in Bewegung zu verschlüsseln und zu sichern.

_Geschätzte Laborzeit:_ 15 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Sehen Sie sich eine Vorschau von "_LiveLabs - Oracle Native Network Encryption (Mai 2022)_" an[](youtube:N6Uz-pVTkaI)

### Ziele

*   Native Netzwerkverschlüsselung in der Datenbank aktivieren/deaktivieren
*   Prüfen Sie die Auswirkungen der Netzwerkverschlüsselung vor/nach der Aktivierung

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Aktuelle Netzwerkkonfiguration prüfen | <5 Minuten |
| 2 | SQL-Traffic generieren und erfassen | 5 Minuten |
| 3 | Netzwerkverschlüsselung aktivieren | 5 Minuten |
| 4 | Netzwerkverschlüsselung deaktivieren | <5 Minuten |

## Aufgabe 1: Prüfen der aktuellen Netzwerkkonfiguration

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/nne</copy>
        
3.  Inhalt der Datei **SQL\*Net.ora** anzeigen
    
        <copy>./nne_view_sqlnet_ora.sh</copy>
        
    
    **Hinweis**: Er muss leer sein.
    
    ![Netzwerkverschlüsselung](./images/nne-001.png "Inhalt der Datei SQLNet anzeigen")
    
4.  Prüfen, ob das Netzwerk bereits verschlüsselt ist
    
        <copy>./nne_is_sess_encrypt.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-002.png "Prüfen, ob das Netzwerk bereits verschlüsselt ist")
    
    **Hinweis**: Die Zeile "Verschlüsselungsserviceadapter" sollte nicht angezeigt werden.
    

## Aufgabe 2: SQL-Traffic generieren und erfassen

1.  Führen Sie **tcpdump** für den Datenverkehr aus, um die im Netzwerk übertragenen Pakete zu analysieren (auf das Ende der Ausführung warten)
    
        <copy>./nne_tcpdump_traffic.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-003a.png "tcpdump für den Verkehr")
    
    ![Netzwerkverschlüsselung](./images/nne-003b.png "tcpdump für den Verkehr")
    
    **Hinweis**:
    
    *   Sie führen eine Abfrage in der Tabelle `DEMO_HR_EMPLOYEES` aus.
    *   Die Ausgabe wurde in der Datei **`tcpdump.pcap`** gespeichert.
    *   Es gibt viele Tools zur Analyse von pcap-Dateien
2.  Extrahieren Sie jetzt sensible Daten aus der gerade generierten Datei tcpdump.pcap, um zu sehen, ob die Fischerei gut war
    
        <copy>./nne_tcpdump_extract.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-004.png "Vertrauliche Daten aus tcpdump-Datei extrahieren")
    
    **Hinweis**:
    
    *   Wir extrahieren alle Zeilen, die eine E-Mail oder ähnliches enthalten
    *   Da sich das Netzwerk im Klartext befindet, sind die Tabellendaten `DEMO_HR_EMPLOYEES` vollständig lesbar!
3.  Führen Sie als Nächstes **tcpflow** aus, um den Datenverkehr über die Leitung für die Glassfish-Anwendung zu erfassen
    
    *   Beginnen Sie das Capture-Skript, und **schließen Sie es nicht**.
        
            <copy>./nne_tcpflow_traffic.sh</copy>
            
        
        ![Netzwerkverschlüsselung](./images/nne-005.png "Erfassungsskript starten")
        
        **Hinweise:** Wir extrahieren alle Zeilen, die eine E-Mail oder Ähnliches enthalten (siehe Befehl "egrep").
        
    *   Öffnen Sie parallel ein Webbrowserfenster zu _`http://dbsec-lab:8080/hr_prod_pdb1`_, um auf Ihre Glassfish-App zuzugreifen
        
        **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
        
    *   Gehen Sie wie folgt vor:
        
        *   Melden Sie sich bei der HR-Anwendung als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
            
                <copy>hradmin</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Netzwerkverschlüsselung](./images/nne-006.png "Anmeldung bei der HR-Anwendung") ![Netzwerkverschlüsselung](./images/nne-007.png "Anmeldung bei der HR-Anwendung")
            
        *   Klicken Sie auf **Mitarbeiter suchen**.
            
            ![Netzwerkverschlüsselung](./images/nne-008.png "Mitarbeiter suchen")
            
        *   Klicken Sie auf \[**Suchen**\]
            
            ![Netzwerkverschlüsselung](./images/nne-009.png "Mitarbeiter suchen")
            
    *   Kehren Sie jetzt zu Ihrer Terminalsession zurück, um Trafficinhalt anzuzeigen
        
        ![Netzwerkverschlüsselung](./images/nne-010.png "Trafficinhalt anzeigen")
        
4.  Wenn Sie die unverschlüsselten Daten angezeigt haben, stoppen Sie das Skript mit "_`[Ctrl]+C`_".
    
    **Anmerkungen:** Da sich das Netzwerk im Klartext befindet, können wir während der Übertragung auf alle sensiblen Daten zugreifen!
    

## Aufgabe 3: Netzwerkverschlüsselung aktivieren

Sie aktivieren die SQL\*Net-Verschlüsselung mit dem Wert _`REQUESTED`_ für _`SQLNET.ENCRYPTION_SERVER`_

1.  Zunächst verwenden wir diese Option, da unverschlüsselte Verbindungen weiterhin eine Verbindung herstellen können. Dies hat zwar selten Auswirkungen, es ist jedoch oft wichtig, dies zu tun, damit die Änderung die Produktionssysteme nicht beeinträchtigt, die zwischen Client und Datenbank nicht verschlüsseln können!
    
        <copy>./nne_enable_requested.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-011.png "Netzwerkverschlüsselung aktivieren")
    
    **Hinweis**: Es gibt eine Alternative zur nativen Netzwerkverschlüsselung, TLS-Zertifikate, die jedoch Benutzerverwaltung und weitere Konfiguration erfordern.
    
2.  Führen Sie das Skript erneut aus, um zu prüfen, ob die Session verschlüsselt ist
    
        <copy>./nne_is_sess_encrypt.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-012.png "Prüfen, ob die Session verschlüsselt ist")
    
    **Hinweis**: Eine zusätzliche Zeile mit der Aufschrift "**AES256 Encryption Service adapter for Linux**" sollte angezeigt werden.
    
3.  Führen Sie jetzt **tcpdump** für den Datenverkehr erneut aus, um die im Netzwerk übertragenen Pakete zu analysieren (warten Sie auf das Ende der Ausführung).
    
        <copy>./nne_tcpdump_traffic.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-003a.png "tcpdump für den Verkehr")
    
    ![Netzwerkverschlüsselung](./images/nne-003b.png "tcpdump für den Verkehr")
    
4.  Extrahieren Sie wie zuvor dieselben sensiblen Daten aus der neu generierten tcpdump.pcap-Datei
    
        <copy>./nne_tcpdump_extract.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-013.png "Vertrauliche Daten aus der neuen tcpdump-Datei extrahieren")
    
    **Hinweis**:
    
    *   Wir extrahieren alle Zeilen, die eine E-Mail oder ähnliches enthalten
    *   Da die Session verschlüsselt ist, sind die Tabellendaten `DEMO_HR_EMPLOYEES` nicht lesbar.
5.  Nun führen wir den Test mit **tcpflow** für die Glassfish-Anwendung durch, um die Auswirkungen der Netzwerkverschlüsselung zu sehen
    
    *   Erfassen Sie in Ihrer Terminalsession den generierten Traffic, und **schließen Sie ihn nicht**.
        
            <copy>./nne_tcpflow_traffic.sh</copy>
            
        
        ![Netzwerkverschlüsselung](./images/nne-005.png "Mit tcpflow den generierten Traffic erfassen")
        
    *   Kehren Sie zu Ihrem Webbrowser zurück, **melden Sie sich ab** Sie die Glassfish-Anwendung an, und **melden Sie sich erneut** als _`hradmin`_ an, um zu sehen, was passiert, wenn wir diesen Traffic schnüffeln
        
        ![Netzwerkverschlüsselung](./images/nne-006.png "Glassfish-Anwendung")
        
        ![Netzwerkverschlüsselung](./images/nne-007.png "Glassfish-Anwendung")
        
    *   Klicken Sie auf **Mitarbeiter suchen**.
        
        ![Netzwerkverschlüsselung](./images/nne-008.png "Mitarbeiter suchen")
        
    *   Klicken Sie auf \[**Suchen**\]
        
        ![Netzwerkverschlüsselung](./images/nne-009.png "Mitarbeiter suchen")
        
    *   Kehren Sie jetzt zu Ihrer Terminalsession zurück, um Trafficinhalt anzuzeigen
        
        ![Netzwerkverschlüsselung](./images/nne-005.png "Trafficinhalt anzeigen")
        
    
    **Hinweis**:
    
    *   Es sollten keine Daten angezeigt werden!
    *   Wir versuchen immer noch, alle Zeilen zu extrahieren, die eine E-Mail oder ähnliches enthalten, aber da das Netzwerk verschlüsselt ist, haben wir nichts!
    *   Die Daten werden zwischen unserer Glassfish-Anwendung (JDBC Thin Client) und der Datenbank verschlüsselt
    *   Dies funktioniert sofort (oder nach einer Aktualisierung), da unsere Glassfish-Anwendung eine neue Verbindung für jede Abfrage erstellt
    *   Eine echte Anwendung muss wahrscheinlich gestoppt und neu gestartet werden, um die bestehenden Anwendungsverbindungen von der Datenbank zu trennen!
6.  Wenn Sie die Auswirkungen der Netzwerkverschlüsselung gesehen haben, verwenden Sie "_`[Ctrl]+C`_", um das Skript zu stoppen
    

## Aufgabe 4: Netzwerkverschlüsselung deaktivieren

1.  Wenn Sie die Übung abgeschlossen haben, können Sie die native Netzwerkverschlüsselung auf die Standardeinstellungen zurücksetzen
    
        <copy>./nne_disable.sh</copy>
        
    
    ![Netzwerkverschlüsselung](./images/nne-014.png "Netzwerkverschlüsselung deaktivieren")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Oracle Database bietet native **Verschlüsselung und Integrität des Datennetzwerks**, um sicherzustellen, dass die Bewegungsdaten sicher sind, während sie über das Netzwerk übertragen werden.

![Netzwerkverschlüsselung](./images/nne-concept.png "Netzwerkverschlüsselungskonzept")

Der Zweck eines sicheren Kryptosystems besteht darin, Klartextdaten auf Basis eines Schlüssels in unverständlichen Ciphertext zu konvertieren, so dass es sehr schwer (rechenbar undurchführbar) ist, Ciphertext ohne Kenntnis des richtigen Schlüssels wieder in seinen entsprechenden Klartext zu konvertieren.

In einem symmetrischen Kryptosystem wird derselbe Schlüssel sowohl zur Verschlüsselung als auch zur Entschlüsselung derselben Daten verwendet. Oracle Database stellt das symmetrische Kryptosystem **Advanced Encryption Standard (AES)** zum Schutz der Vertraulichkeit des Oracle Net Services-Datenverkehrs bereit.

Oracle SQL\*Net-Traffic kann mit folgenden Methoden verschlüsselt werden:

*   Native Netzwerkverschlüsselung
*   TLS-zertifikatbasierte Verschlüsselung

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Native Network Encryption 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/configuring-network-data-encryption-and-integrity.html)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Richard Evans
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023