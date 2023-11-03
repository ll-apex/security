# Netzwerkfirewall-Policy konfigurieren

## Einführung

In dieser Übung konfigurieren Sie erforderliche Ressourcen für die Netzwerkfirewall-Policy, Regeln zur Untersuchung verschiedener Firewallfeaturesets und unterstützende Konfiguration für **Übung 4**.

Geschätzte Zeit: 30 Minuten.

### Ziele

*   Network Firewall-Policy-Regeln konfigurieren
*   Anwendungen, URLs und IP-Adresslisten konfigurieren
*   Entschlüsselungsregeln für SSL-/HTTPS-Traffic, eingehende SSL-Prüfung, Proxy weiterleiten konfigurieren
*   Konfigurieren von Sicherheitsregeln zur Unterstützung von Zulassen, Blockieren, Ablehnen, IPS/IDS-Traffic, URL-Filterung
*   Aktualisieren der Netzwerkfirewall-Policy auf Netzwerkfirewall demonstrieren
*   Aktivieren von Netzwerkfirewalllogs demonstrieren
*   Netzwerkfirewall-Metriken und Arbeitsanforderungen kennenlernen

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Netzwerkfirewall-Policy klonen

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Netzwerkfirewall-Policys**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Zum Fenster "Netzwerkfirewall" navigieren](../common/images/network-firewall-window.png " ")
    
2.  Klicken Sie auf das zuvor erstellte **network-firewall-policy-demo**.
    
    ![Klicken Sie auf "Create Network Firewall Policy".](../common/images/click-created-network-firewall-policy.png " ")
    
3.  Die folgende Tabelle stellt dar, was Sie erstellen. Klicken Sie auf das Symbol **Policy klonen**, um eine Kopie der **Netzwerkfirewall-Policy** zu erstellen:
    
    | Neuer Policenname | Kommentar |
    | --- | --- |
    | network-firewall-policy-demo-latest | Diese Übung enthält Aufgaben zum Aktualisieren der Richtlinie. |
    
    ![Network Firewall-Policy klonen](../common/images/clone-network-firewall-policy.png " ")
    
4.  Füllen Sie das Dialogfeld aus, und klicken Sie auf **Weiter**:
    
    *   **Policy-Name**: Geben Sie einen Namen an
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    
    ![Grundlegende Informationen zur Clone Network Firewall Policy](../common/images/clone-network-firewall-policy-basic-information.png " ")
    
5.  Fahren Sie mit **nächste Aufgabe** fort, um **Anwendungslisten** zu erstellen.
    

## Aufgabe 2: Anwendungslisten erstellen

1.  Fahren Sie mit dem Workflow **Netzwerkfirewall-Policy-Klon erstellen** fort, und klicken Sie auf **Anwendungsliste hinzufügen**:
    
    ![Schaltfläche "Anwendungsliste hinzufügen" in Netzwerkfirewall-Policy klonen](../common/images/clone-network-firewall-add-application-button.png " ")
    
2.  In der folgenden Tabelle werden die **Anwendungslisten** zur Unterstützung von Verkehrswerten erstellt:
    
    | Name der Anwendungsliste | Protokoll | Kommentar |
    | --- | --- | --- |
    | ssh-http-https-Liste | Wählen Sie TCP mit Port 22, 80, 443 | Dieser Ports-Traffic wird validiert |
    | ICMP-Liste | Echo Request and Echo Reply ICMP Protocol auswählen | ICMP-Traffic wird validiert |
    
3.  Sie fügen Listen nacheinander hinzu, indem Sie auf **Anwendungsliste hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **Listenname**: Geben Sie einen Namen an
    *   **Protokolleinstellung für Anwendungsliste**: Wählen Sie **UDP/TCP aus**
        *   **App1**:
            *   **Protokoll** Wählen Sie in der Dropdown-Liste die Option **TCP** aus.
            *   **Portbereich**: Geben Sie **22-22 ein**
        *   **App2**:
            *   **Protokoll** Wählen Sie in der Dropdown-Liste die Option **TCP** aus.
            *   **Portbereich**: Geben Sie "**80-80" ein.**
        *   **App3**:
            *   **Protokoll** Wählen Sie in der Dropdown-Liste die Option **TCP** aus.
            *   **Portbereich**: Geben Sie **443-443 ein.**
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Anwendungsliste hinzufügen**.
    
    ![Netzwerkfirewall-Policy klonen TCP/UDP-Anwendungslisten hinzufügen](../common/images/clone-network-firewall-add-application-tcp-udp-lists.png " ")
    
5.  Erstellen Sie eine weitere Anwendungsliste zur Unterstützung von **ICMP**\-Traffics gemäß _Step 3_\-Tabelle:
    
    *   **Listenname**: Geben Sie einen Namen an
    *   **Protokolleinstellung für Anwendungsliste**: Wählen Sie **UDP/TCP aus**
        *   **App1**:
            *   **Protokoll** Wählen Sie in der Dropdown-Liste die Option **ICMP/ICMP6** aus.
            *   **ICMP-Typ**: Wählen Sie **0 - Echo Reply**
            *   **ICMP-Code**: Wählen Sie **0 - Echo Reply**
        *   **App2**:
            *   **Protokoll** Wählen Sie in der Dropdown-Liste die Option **ICMP/ICMP6** aus.
            *   **ICMP-Typ**: Wählen Sie **8 - Echo**
            *   **ICMP-Code**: Wählen Sie **0 - Echo Request**
    
    ![Netzwerkfirewall-Policy klonen Anwendungs-ICMP-Listen hinzufügen](../common/images/clone-network-firewall-add-application-icmp-lists.png " ")
    
6.  Fahren Sie mit der nächsten Aufgabe fort, um **URL-Listen** zu erstellen.
    

## Aufgabe 3: URL-Listen erstellen

1.  Fahren Sie mit dem Workflow **Netzwerkfirewall-Policy-Klon erstellen** fort, und klicken Sie auf **URL-Liste hinzufügen**:
    
2.  In der folgenden Tabelle werden die **URL-Listen** zur Unterstützung des Trafficflusses erstellt:
    
    | Name der URL-Liste | Adressen | Kommentar |
    | --- | --- | --- |
    | http-info-url-espn-traffic | URLs eingeben: info.cern.ch, espn.com | URL-Filtervalidierung: Info-Cern für HTTP und ESPN für SSL-/HTTPS-Traffic |
    
3.  Sie fügen Listen nacheinander hinzu, indem Sie auf **Anwendungsliste hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **Listenname**: Geben Sie einen Namen an
    *   **URLs**: Geben Sie die URLs **info.cern.ch** und **espn.com** ein.
4.  Prüfen Sie alle Informationen, und klicken Sie auf **URL-Liste hinzufügen**.
    
    ![Netzwerkfirewall-Policy klonen URLs-Listen hinzufügen](../common/images/clone-network-firewall-add-url-lists.png " ")
    
5.  Fahren Sie mit der nächsten Aufgabe fort, um **IP-Adresslisten** zu erstellen.
    

## Aufgabe 4: IP-Adresslisten erstellen

1.  Fahren Sie mit dem Workflow **Netzwerkfirewall-Policy-Klon erstellen** fort, und klicken Sie auf **Adressliste hinzufügen**.
    
2.  In der folgenden Tabelle wird dargestellt, was Sie zur Unterstützung von Verkehrswerten als **IP-Adressliste** erstellen:
    
    | Listenname der IP-Adresse | IP-Adressen | Kommentar |
    | --- | --- | --- |
    | Client-Subnetz-CIDR | 10.10.1.0/24 | Clientsubnetz-CIDR in Firewall-VCN |
    | Server-Subnetz-CIDR | 10.10.2.0/24 | Serversubnetz-CIDR in Firewall-VCN |
    | Spoke-VCN-Subnetz-CIDRs | 24.10.20.1.0, 10.20.2.0/20 | CIDR von Spoke-VCN-Subnetzen in Spoke-VCN |
    | ServerA - Subnetz-CIDR | 10.10.1.0/24 | ServerA Subnetz-CIDR in Spoke-VCN |
    | ServerB - Subnetz-CIDR | 10.10.2.0/24 | ServerB Subnetz-CIDR in Spoke-VCN |
    | MyPublic-IP | 98\. XX/32 | Geben Sie Ihre öffentliche IP-Adresse ein |
    
3.  Sie fügen Listen nacheinander hinzu, indem Sie auf **Anwendungsliste hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **IP-Adresslistenname**: Geben Sie einen Namen an
    *   **IP-Adressen**: Geben Sie den Wert gemäß der Tabelle für jede Liste ein.
        *   Beispiel: Die IP-Adressliste **Clinet-Subnet-CIDR** geben Sie **10.10.1.0/24** ein.
4.  Prüfen Sie alle Informationen, und klicken Sie auf **IP-Adressliste hinzufügen**.
    
    ![Netzwerkfirewall-Policy klonen - CIDR-Liste des IP-Adressclients hinzufügen](../common/images/clone-network-firewall-add-ip-address-client-cidr-lists.png " ")
    
5.  Wiederholen Sie _Schritt 3-4_, und fügen Sie die IP-Adressliste **Server-Subnet-CIDR** hinzu.
    
    ![Netzwerkfirewall-Policy klonen - IP-Adressserver-CIDR-Liste hinzufügen](../common/images/clone-network-firewall-add-ip-address-server-cidr-lists.png " ")
    
6.  Wiederholen Sie _Schritt 3-4_, und fügen Sie die IP-Adressliste **Spoke-VCN-Subnetz-CIDRs** hinzu.
    
    ![Netzwerkfirewall-Policy klonen - IP-Adress-Spoke-CIDR-Liste hinzufügen](../common/images/clone-network-firewall-add-ip-address-spoke-cidr-lists.png " ")
    
7.  Wiederholen Sie _Schritt 3-4_, und fügen Sie die IP-Adressliste **ServerA-Subnet-CIDR** hinzu.
    
    ![Netzwerkfirewall-Policy klonen - CIDR-Liste der IP-Adresse ServerA hinzufügen](../common/images/clone-network-firewall-add-ip-address-servera-cidr-lists.png " ")
    
8.  Wiederholen Sie _Schritt 3-4_, und fügen Sie die IP-Adressliste **ServerB-Subnet-CIDR** hinzu.
    
    ![Netzwerkfirewall-Policy klonen - CIDR-Liste der IP-Adresse ServerB hinzufügen](../common/images/clone-network-firewall-add-ip-address-serverb-cidr-lists.png " ")
    
9.  Wiederholen Sie _Step 3-4_, und fügen Sie die IP-Adressliste **MyPublic-IP** hinzu.
    
    ![Netzwerkfirewall-Policy klonen IP-Adresse hinzufügen - Meine öffentliche IP-CIDR-Liste](../common/images/clone-network-firewall-add-ip-address-my-public-cidr-lists.png " ")
    
10.  Fahren Sie mit der nächsten Aufgabe fort, um **Zugeordnete Secrets und Entschlüsselungsprofile** zu erstellen.
    

## Aufgabe 5: Zugeordnete Secrets und Entschlüsselungsprofile erstellen

1.  Für unseren Anwendungsfall ist eine SSL Forward Proxy-(HTTPS-)Trafficvalidierung erforderlich. Daher müssen Sie **Aufgabe 1-4: Zertifikat speichern** in den offiziellen Dokumenten der **Netzwerkfirewall** befolgen:
    
    [Aufgabe 1-4: Vault erstellen, Zertifikat erstellen, Zertifikat speichern](https://docs.oracle.com/en-us/iaas/Content/network-firewall/setting-up-certificate-authentication.htm)
    

> **Lesen**: Sie können auch in _Schritt 2-4_ ein **Zertifikat**, ein **Secret** und ein **CA-Zertifikat aktualisieren** erstellen, um ein **zugeordnetes Secret** zu erstellen. Wenn Sie _Step1_ ausgefüllt haben, können Sie mit _Step6_ fortfahren.

2.  Zu Ihrer Referenz können Sie das Zertifikatskript auf Ihre **Client-VM** herunterladen. Speichern Sie diese [Datei](https://github.com/oracle-quickstart/oci-network-firewall/blob/master/scripts/create-certificate.sh) als **certificate.sh** im Home-Verzeichnis.
    
3.  Führen Sie bash scrip **./certificate.sh** mit den Parametern **forward** und **network firewall ip** aus, um das Zertifikat zu generieren:
    
    ![Zertifikatskript ausführen, um SSL-Weiterleitungsproxy zu generieren](../common/images/run-certificate-script-generate-ssl-foreward-proxy.png " ")
    
4.  In dieser Abbildung wird dargestellt, wie Sie das **Secret erstellen** mit der Dateiausgabe **...ssl-forward-proxy.json** erstellen.
    
    ![Secret in OCI Vault mit SSL-Weiterleitungsproxy-JSON-Ausgabe erstellen](../common/images/create-secret-oci-vaule-with-certificate.png " ")
    
5.  Sie müssen **CA-Cert** aktualisieren. Im Folgenden finden Sie ein Beispielbild mit Befehlen, die Sie ausführen müssen:
    
    ![Clientzertifikat zu CA Cert-Bundle auf Linux-VM hinzufügen](../common/images/add-client-certificate-to-ca-cert-bundle.png " ")
    
6.  Nachdem der Import des Zertifikats in **OCI Vault** erfolgreich abgeschlossen wurde, fahren Sie mit dem Workflow **Netzwerkfirewall-Policy-Klon erstellen** fort, und klicken Sie auf **Mappped Secret hinzufügen**.
    
7.  In der folgenden Tabelle wird dargestellt, was Sie **Zugeordnetes Secret** zur Unterstützung des **SSL/HTTPS**\-Trafficflusses erstellen:
    
    | Name des zugeordneten Secret | Typ des zugeordneten Secret | Kommentar |
    | --- | --- | --- |
    | ssl-forward-proxy-mapped-secret | SSL-Forward-Proxy | Secret-Import des Proxyzertifikats mit SSL-Weiterleitung in OCI Vault auswählen |
    
8.  Sie fügen ein zugeordnetes Secret hinzu, indem Sie auf **Zugeordnetes Secret hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **Zugeordneter Secret-Name**: Geben Sie einen Namen an
    *   **Zugeordneter Secret-Typ**: Wählen Sie **SSL-Forward-Proxy aus**
    *   **Vault**: Wählen Sie **Vault** aus dem Compartment aus
    *   **Secret**: Wählen Sie **Secret** in Ihrem Vault aus
    *   **Version**: Secret-**Version** aus Ihrem Vault auswählen
9.  Prüfen Sie alle Informationen, und klicken Sie auf **Zugeordnetes Secret hinzufügen**.
    
    ![Netzwerkfirewall-Policy klonen Zugeordnetes Secret hinzufügen](../common/images/clone-network-firewall-add-mapped-secret.png " ")
    
10.  Sie fügen ein Entschlüsselungsprofil hinzu, indem Sie auf **Entschlüsselungsprofil hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **NAME des Entschlüsselungsprofils**: Geben Sie einen Namen an
    *   **Entschlüsselungsprofiltyp**: **SSL-Weiterleitungsproxy auswählen**
11.  Prüfen Sie alle Informationen, und klicken Sie auf **Entschlüsselungsprofil hinzufügen**.
    

![Netzwerkfirewall-Policy klonen - Entschlüsselungsprofil hinzufügen](../common/images/clone-network-firewall-add-decrpytion-profile.png " ")

12.  Fahren Sie mit der nächsten Aufgabe fort, um die **Entschlüsselungsregel** zu erstellen.

## Aufgabe 6: Entschlüsselungsregeln erstellen

1.  Fahren Sie mit dem Workflow **Netzwerkfirewall-Policy-Klon erstellen** fort, und klicken Sie auf **Entschlüsselungsregel hinzufügen**.
    
2.  In der folgenden Tabelle wird dargestellt, was Sie zur Unterstützung des Trafficflusses als **Entschlüsselungsregel** erstellen:
    
    | Entschlüsselungsregelname |
    | --- |
    | Client-SSL-Traffic-Entschlüsselungsregel |
    
3.  Sie erstellen eine Entschlüsselungsregel, indem Sie auf **Entschlüsselungsregel hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **NAME der Entschlüsselungsregel**: Geben Sie einen Namen an
    *   **Übereinstimmungsbedingung**:
        *   **Quell-IP-Adresse**: Wählen Sie **Client-Subnet-CIDR aus**
        *   **Ziel-IP-Adresse**: **beliebige IP-Adresse eingeben**
    *   **Regelaktion**:
        *   **Aktion**: Wählen Sie "Traffic mit SSL-Forward-Proxy entschlüsseln" aus
        *   **Entschlüsselungsprofil**: Wählen Sie ein früher erstelltes SSL-Forward-Proxy-Decryption-Profil aus
        *   **Zugeordnetes Secret**: Wählen Sie ein früher erstelltes zugeordnetes Secret aus
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Änderungen speichern**.
    
    ![Netzwerkfirewall-Policy klonen - Entschlüsselungsregel hinzufügen](../common/images/clone-network-firewall-add-decrpytion-rule.png " ")
    
5.  Fahren Sie mit der nächsten Aufgabe fort, um **Sicherheitsregeln** zu erstellen.
    

## Aufgabe 7: Sicherheitsregeln erstellen

1.  Navigieren Sie zur nächsten Seite des Workflows **Netzwerkfirewall-Policy-Klon erstellen**, und klicken Sie auf **Sichere Regel hinzufügen**, um eine Sicherheitsregel zu erstellen:
    
2.  In der folgenden Tabelle werden die **Sicherheitsregeln** zur Unterstützung des Trafficflusses erstellt:
    
    | Regelname | Quell-IP-Adressen | Ziel-IP-Adressen | Anwendungen | Adressen | Regelaktion |
    | --- | --- | --- | --- | --- | --- |
    | rule-inbound-my-ip-to-client-vm-allow-all-traffic | MyPublic-IP | Client-Subnetz-CIDR | Beliebiges Protokoll | Jede URL | Traffic zulassen |
    | rule-client-server-vm-allow-ssh-traffic | Client-Subnetz-CIDR | Server-Subnetz-CIDR | UDP/TCP mit ssh-http-https-list | Jede URL | Traffic zulassen |
    | Rule-Client-Server-vm-reject-icmp-Verkehr | Client-Subnetz-CIDR | Server-Subnetz-CIDR | ICMP/ICMPv6 mit ICMP-Liste | Jede URL | Traffic ablehnen |
    | rule-client-serverB-vm-reject-all-traffic | Client-Subnetz-CIDR | ServerB - Subnetz-CIDR | Beliebiges Protokoll | Jede URL | Traffic ablehnen |
    | rule-client-serverA-vm-allow-all-traffic | Client-Subnetz-CIDR | ServerA - Subnetz-CIDR | Beliebiges Protokoll | Jede URL | Traffic zulassen |
    | rule-client-allow-http-https-url-filtering-traffic | Client-Subnetz-CIDR | Beliebige IP-Adresse | UDP/TCP mit ssh-http-https-list | http-info-url-espn-traffic | Traffic zulassen |
    | Regel-Client-zu-Internet-Intrusion-Erkennung-Verkehr | Client-Subnetz-CIDR | Beliebige IP-Adresse | UDP/TCP mit ssh-http-https-list | Jede URL | Angriffserkennung |
    | rule-client-to-outbound-allow-all-traffic | Client-Subnetz-CIDR | Beliebige IP-Adresse | Beliebiges Protokoll | Jede URL | Traffic zulassen |
    | rule-serverA-to-serverB-allow-icmp-traffic | ServerA - Subnetz-CIDR | ServerB - Subnetz-CIDR | ICMP/ICMPv6 mit ICMP-Liste | Jede URL | Traffic zulassen |
    | rule-serverB-to-serverA-allow-ssh-http-https-Verkehr | ServerB - Subnetz-CIDR | ServerA - Subnetz-CIDR | UDP/TCP mit ssh-http-https-list | Jede URL | Traffic zulassen |
    | rule-spoke-vcn-to-firewall-vcn-allow-all-traffic | Clientsubnetz-CIDR, Spoke-VCN-Subnetz-CIDRs, Serversubnetz-CIDR | Clientsubnetz-CIDR, Spoke-VCN-Subnetz-CIDRs, Serversubnetz-CIDR | Beliebiges Protokoll | Jede URL | Traffic zulassen |
    | rule-default-drop-all-traffic | Beliebige IP-Adresse | Beliebige IP-Adresse | Beliebiges Protokoll | Jede URL | Traffic entfernen |
    
3.  Sie fügen Regeln einzeln hinzu, indem Sie auf **Sicherheitsregel hinzufügen** klicken und das Dialogfeld ausfüllen:
    
    *   **Regelname**: Geben Sie einen Namen an
    *   **Übereinstimmungsbedingung**:
        *   **Quell-IP-Adresse**: Geben Sie sie gemäß Tabelle ein
        *   **Ziel-IP-Adresse**: Geben Sie sie gemäß Tabelle ein
        *   **Anwendungen**: Eingabe gemäß Tabelle
        *   **URLs**: Eingabe gemäß Tabelle
    *   **Regelaktion**: Wählen Sie diese Option gemäß Tabelle aus
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Änderungen speichern**.
    
    ![Netzwerkfirewall-Policy klonen Sicherheitsregel für eingehende Internetverbindung hinzufügen](../common/images/clone-network-firewall-add-security-rule-inbound-connection.png " ")
    
5.  \[Optional\] Sie können frühere erstellte Standard-**allow-rule** aus der Automatisierung entfernen, wenn verfügbar:
    
    ![Sicherheitsregel zum Löschen der Netzwerkfirewall-Policy klonen](../common/images/clone-network-firewall-delete-security-rule.png " ")
    
6.  Wiederholen Sie _Step 3-4_ gemäß _Step 2_\-Tabelleneinträgen:
    
7.  Stellen Sie sicher, dass alle **Sicherheitsregeln** erfolgreich erstellt wurden. Klicken Sie auf **Weiter**:
    
    ![Netzwerkfirewall-Policy klonen Alle Sicherheitsregeln hinzufügen](../common/images/clone-network-firewall-add-all-security-rules.png " ")
    
8.  Beenden Sie die Erstellung der Netzwerkfirewall-Policy, indem Sie auf die Schaltfläche **Netzwerkfirewall-Policy erstellen** klicken:
    
    ![Prüfung der Netzwerkfirewall-Policy klonen und Workflow abschließen](../common/images/clone-network-firewall-review-final-updates.png " ")
    

## Aufgabe 8: Netzwerkfirewall-Policy aktualisieren

1.  Navigieren Sie zur Seite **Netzwerkfirewall**, um die **Netzwerkfirewall-Policy** zu aktualisieren:
    
2.  In der folgenden Tabelle wird dargestellt, was Sie mit der Policy verknüpften **Netzwerkfirewall** aktualisieren, um den Trafficfluss zu unterstützen:
    
    | Network Firewall-Policy | Kommentar |
    | --- | --- |
    | network-firewall-policy-demo-latest | Neue Policy für die Verknüpfung mit der Netzwerkfirewall |
    
3.  Klicken Sie auf das Symbol **Bearbeiten**, und wählen Sie in der Dropdown-Liste die Option **network-firewall-demo-latest** aus.
    
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Änderungen speichern**.
    
    ![Network Firewall Policy aktualisieren](../common/images/network-firewall-update-policy.png " ")
    
5.  Policy-Updates dauern derzeit fast **7-10** Minuten. Sie können Ihre Arbeitsanforderung auf der Ressourcenseite der **Netzwerkfirewall** verfolgen.
    

## Aufgabe 9: Netzwerkfirewalllogs aktivieren

1.  Navigieren Sie zur Seite **Netzwerkfirewall > Ressourcen > Logs**, um **Traffic**\- und **Bedrohungslogs** zu aktivieren:
    
    ![Netzwerkfirewalllogs aktivieren](../common/images/network-firewall-enable-logs.png " ")
    
2.  Klicken Sie auf die Umschaltflächen neben **Bedrohungslog**, um Logs zu aktivieren.
    
3.  Prüfen Sie alle Informationen, und klicken Sie auf **Log aktivieren**.
    
    ![Bedrohungslog für Netzwerkfirewall aktivieren](../common/images/network-firewall-enable-threat-logs.png " ")
    
4.  Klicken Sie auf die Umschaltflächen neben **Trafficlog**, um Logs zu aktivieren.
    
5.  Prüfen Sie alle Informationen, und klicken Sie auf **Log aktivieren**.
    
    ![Log für Netzwerkfirewalltraffic aktivieren](../common/images/network-firewall-enable-traffic-logs.png " ")
    
6.  Sie haben Netzwerkfirewalllogs erfolgreich aktiviert.
    

## Aufgabe 10: Netzwerkfirewallmetriken untersuchen

1.  Navigieren Sie zur Seite **Netzwerkfirewall > Ressourcen > Metriken**, um **Metriken** zu prüfen:
    
    ![Network Firewall-Metriken kennenlernen](../common/images/network-firewall-metrics.png " ")
    
2.  Erkunden Sie verschiedene Metriken, entweder mit dem Mauszeiger darüber oder erkunden Sie die **Optionen**, um bei Bedarf Alarme zu erstellen:
    
    ![Anzahl der Sicherheitstreffer von Netzwerkfirewallmetriken untersuchen](../common/images/network-firewall-security-hit-counts.png " ")
    

## Aufgabe 11: Netzwerkfirewall-Arbeitsanforderungen untersuchen

1.  Navigieren Sie zur Seite **Netzwerkfirewall > Ressourcen > Arbeitsanforderungen**, um **Arbeitsanforderungen** zu prüfen:
    
    ![Network Firewall-Arbeitsanforderungen untersuchen](../common/images/network-firewall-work-requests.png " ")
    
2.  Klicken Sie auf die Anforderung **Firewall aktualisieren**, um mehr über **Log**, **Fehler** und **zugeordnete Ressourcen** zu erfahren:
    
    ![Netzwerkfirewall-Arbeitsanforderungen kennenlernen - Firewall aktualisieren](../common/images/network-firewall-work-requests-update-firewall.png " ")
    

_**Glückwunsch! Sie haben die Übung abgeschlossen.**_

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

1.  [OCI-Schulungen](https://www.oracle.com/cloud/iaas/training/)
2.  [Vertrautheit mit der OCI-Konsole](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Networking - Überblick](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
4.  [Überblick über OCI Network Firewall](https://docs.oracle.com/en-us/iaas/Content/network-firewall/overview.htm)
5.  [OCI Network Firewall Cloud - Seite "Sicherheit"](https://www.oracle.com/security/cloud-security/network-firewall/)
6.  [OCI-Intra-VCN-Routingfunktionen](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingroutetables.htm)

## Danksagungen

*   **Autor** - Arun Poonia, Principal Solutions Architect
*   **Angepasst von** - Oracle
*   **Mitwirkende** - nicht zutreffend
*   **Zuletzt aktualisiert am/um** - Arun Poonia, Aug 2023