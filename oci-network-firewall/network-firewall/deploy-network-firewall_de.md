# Netzwerkfirewall und unterstützende Konfiguration bereitstellen

## Einführung

In dieser Übung erstellen Sie eine **Netzwerkfirewall-Policy** und eine **Netzwerkfirewall**. Dabei aktualisieren Sie die erforderlichen Routen und Policys, um den Traffic zwischen VCNs zu unterstützen.

Geschätzte Zeit: 45 Minuten.

### Ziele

*   Netzwerkfirewall-Policy konfigurieren
*   Netzwerkfirewall in Firewall-VCN starten
*   Aktualisierung von Routingtabellen demonstrieren
*   Routentabellenzuordnung validieren

### Voraussetzungen

*   Zugangsdaten für kostenpflichtigen Oracle Cloud Infrastructure-Account (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und Quota verfügen, um Ressourcen bereitzustellen.

## Aufgabe 1: Netzwerkfirewall-Policy konfigurieren

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Netzwerkfirewall-Policys**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Zum Fenster "Netzwerkfirewall" navigieren](../common/images/network-firewall-window.png " ")
    
2.  Die folgende Tabelle stellt dar, was Sie erstellen. Klicken Sie auf das Symbol **Policy erstellen**, um eine neue **Netzwerkfirewall-Policy** zu erstellen:
    
    | Policy-Name | Kommentar |
    | --- | --- |
    | network-firewall-policy-demo | Sie fügen die erforderliche Policy-Konfiguration in **Lab3** hinzu, sodass Sie zuerst die Standardregel **Alle zulassen** zulassen. |
    
    ![Schaltfläche "Network Firewall Policy erstellen"](../common/images/create-network-firewall-policy.png " ")
    
3.  Füllen Sie das Dialogfeld aus, und klicken Sie auf **Weiter**:
    
    *   **Policy-Name**: Geben Sie einen Namen an
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    
    ![Grundlegende Informationen zur Network Firewall Policy erstellen](../common/images/create-network-firewall-policy-basic-information.png " ")
    
4.  Behalten Sie den Standardwert bei, und klicken Sie auf **Weiter**:
    
    ![Erstellen von Netzwerkfirewall-Policy-Listeninformationen](../common/images/create-network-firewall-policy-lists-information.png " ")
    
5.  Behalten Sie den Standardwert bei, und klicken Sie auf **Weiter**:
    
    ![Zu Netzwerkfirewall-Policy zugeordnete Secret-Informationen erstellen](../common/images/create-network-firewall-policy-secrets-information.png " ")
    
6.  Sie fügen eine **allow-all**\-Regel hinzu. Klicken Sie auf **Sicherheitsregel hinzufügen**, und füllen Sie das Dialogfeld aus:
    
    *   **Regelname**: Geben Sie einen Namen an
    *   **Übereinstimmungsbedingung**:
        *   **Quell-IP-Adresse**: Stellen Sie sicher, dass **Beliebige IP-Adresse** ausgewählt ist
        *   **Ziel-IP-Adresse**: Stellen Sie sicher, dass **Beliebige IP-Adresse** ausgewählt ist
        *   **Anwendung**: Stellen Sie sicher, dass **Beliebiges Protokoll** ausgewählt ist
        *   **URLs**: Stellen Sie sicher, dass **Beliebige URL** ausgewählt ist
    *   **Abgleichsbedingung**: Wählen Sie die Aktion **Traffic zulassen** aus.
7.  Prüfen Sie alle Informationen, und klicken Sie auf **Save Changes** und dann auf **Next**.
    
    ![Informationen zu Sicherheitsregeln für Netzwerkfirewall-Policy erstellen](../common/images/create-network-firewall-policy-rules-information.png " ")
    
    ![Nächste Seite Sicherheitsregeln für Netzwerkfirewall-Policy erstellen](../common/images/create-network-firewall-policy-rules-click-next-page.png " ")
    
8.  Klicken Sie auf **Netzwerk-Policy erstellen**, um die anfängliche Policy zu erstellen:
    
    ![Netzwerkfirewall-Policy erstellen - Seite "Prüfinformationen abschließen"](../common/images/create-network-firewall-policy-complete-page.png " ")
    
9.  Dadurch wird eine Netzwerkfirewall-Policy mit den folgenden Komponenten erstellt.
    
    _Netzwerkfirewall-Policy mit Regel "Alle zulassen"_
    

## Aufgabe 2: Netzwerkfirewall in Firewall-VCN erstellen

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Netzwerkfirewall**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus:
    
    ![Zum Fenster "Netzwerkfirewall" navigieren](../common/images/network-firewall-window.png " ")
    
2.  Die folgende Tabelle zeigt, was Sie erstellen. Klicken Sie auf das Symbol **Netzwerkfirewall erstellen**, um eine neue **Netzwerkfirewall** zu erstellen:
    
    | Firewallname | Kommentar |
    | --- | --- |
    | OCI-Netzwerk-Firewall-Demo | Sie erstellen eine Netzwerkfirewall in **Firewallsubnetz**, um öffentliche und OCI-Workloads zu schützen. |
    
    ![Schaltfläche "Netzwerkfirewall erstellen"](../common/images/create-network-firewall-button-click.png " ")
    
3.  Füllen Sie das Dialogfeld aus:
    
    *   **Firewallname**: Geben Sie einen Namen an
    *   **Compartment**: Stellen Sie sicher, dass das Compartment ausgewählt ist
    *   **Netzwerkfirewall-Policy**: Wählen Sie in der Dropdown-Liste die Option **network-firewall-policy-demo** aus.
    *   **Enforcement Point**:
        *   **Virtuelles Cloud-Netzwerk**: Wählen Sie in der Dropdown-Liste die Option **firewall-vcn** aus.
        *   **Subnetz**: Wählen Sie in der Dropdown-Liste die Option **firewall-subnet** aus.
        *   **\[Optional\] Availability-Domain**: Wählen Sie die gewünschte **AD** aus der Dropdown-Liste aus.
    
    ![Netzwerkfirewall erstellen](../common/images/create-network-firewall.png " ")
    
4.  Prüfen Sie alle Informationen, und klicken Sie auf **Netzwerkfirewall erstellen**.
    
5.  Dadurch wird eine Netzwerkfirewall mit den folgenden Komponenten erstellt.
    
    _Netzwerkfirewall innerhalb des Firewallsubnetzes_
    
    > **Lesen Sie**: Das Deployment der OCI Network Firewall dauert anfänglich fast **30-35 Minuten**. Stellen Sie sicher, dass diese Aufgabe abgeschlossen ist, bevor Sie fortfahren.
    
6.  Prüfen Sie die IP-Adresse der **Netzwerkfirewall**, die Sie in der nächsten Aufgabe aktualisieren müssen.
    
    ![Netzwerkfirewall erfolgreich erstellt](../common/images/created-network-firewall.png " ")
    

## Aufgabe 3: Routentabellen in Firewall-VCN aktualisieren

1.  Navigieren Sie zur **firewall-vcn**, und wählen Sie die Routentabelle **VCN-INGRESS-RouteTable** aus.
    
2.  Klicken Sie auf **Routingregeln hinzufügen**.
    
3.  Wählen Sie als Zieltyp **Private IP** aus, und geben Sie die sekundäre IP-Adresse ein, die mit der IP-Adresse der **Netzwerkfirewall** verknüpft ist.
    
4.  Geben Sie den **Ziel-CIDR-Block** ein
    
    *   In diesem Fall speichern Sie den gesamten Standard-CIDR **0.0.0.0/0**, bei dem es sich um eingehenden Traffic von Spoke-VCNs über **DRG** handelt, in die aktive Firewall. Sie können bei Bedarf auch den CIDR-Block für die Firewall-VCNs oder öffentliche/On-Premise-Subnetze eingeben. Beispiel: 10.10.1.0/24 oder 10.10.2.0/24
5.  **Beschreibung** hinzufügen.
    
    ![Routentabelle für VCN-Ingress in Firewall-VCN aktualisieren](../common/images/updated-vcn-ingress-route-table-entries-firewall-vcn.png " ")
    
6.  Klicken Sie auf **Routingregeln hinzufügen**, um den Vorgang abzuschließen.
    
7.  Navigieren Sie zur **firewall-vcn**, und wählen Sie die Routentabelle **ClientSubnetRouteTable** aus.
    
8.  Klicken Sie auf **Routingregeln hinzufügen**.
    
9.  Geben Sie die erforderlichen Einträge wie folgt ein, und klicken Sie bei Bedarf auf **Weitere Routingregel**:
    
    *   **Erster Eintrag**
        
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall legen Sie **10.10.2.0/24** für ausgehenden Traffic des Serversubnetzes über **OCI Network Firewall** fest.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
    *   **Zweiter Eintrag**
        
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall legen Sie **10.20.0.0/16** für ausgehenden Traffic des Serversubnetzes über **OCI Network Firewall** fest.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
    *   **Dritter Eintrag**
        
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall speichern Sie **0.0.0.0/0**, um sicherzustellen, dass ausgehender Traffic über die **OCI Network Firewall** geleitet wird.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
        
        > **Lesen**: Auch wenn die Routingregel **0.0.0.0/0** hinzugefügt wird, muss der **erste Eintrag** hinzugefügt werden, wenn Sie die Prüfung auf Subnetzen auf Intra-vcn-Ebene prüfen möchten. Jede Routentabelle hat ihren lokalen Eintrag. Daher ist ein erster Eintrag erforderlich, der Vorrang hat.
        
10.  Fügen Sie für jeden Eintrag eine **Beschreibung** hinzu.
    

![Routentabelle für Clientsubnetz in Firewall-VCN aktualisieren](../common/images/update-client-subnet-route-table-entries-firewall-vcn.png " ")

11.  Klicken Sie auf **Routingregeln hinzufügen**, um den Vorgang abzuschließen.
    
12.  Navigieren Sie zur **firewall-vcn**, und wählen Sie die Routentabelle **ServerSubnetRouteTable** aus.
    
13.  Klicken Sie auf **Routingregeln hinzufügen**.
    
14.  Geben Sie die erforderlichen Einträge wie folgt ein, und klicken Sie bei Bedarf auf **Weitere Routingregel**:
    
    *   **Erster Eintrag**
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall verwenden Sie **10.10.1.0/24** für ausgehenden Traffic des Clientsubnetzes über **OCI Network Firewall**.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
    *   **Zweiter Eintrag**
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall legen Sie **10.20.0.0/16** für ausgehenden Traffic des Serversubnetzes über **OCI Network Firewall** fest.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
15.  Fügen Sie für jeden Eintrag eine **Beschreibung** hinzu.
    

![Routentabelle für Serversubnetz in Firewall-VCN aktualisieren](../common/images/update-server-subnet-route-table-entries-firewall-vcn.png " ")

16.  Klicken Sie auf **Routingregeln hinzufügen**, um den Vorgang abzuschließen.
    
17.  Navigieren Sie zur **firewall-vcn**, und wählen Sie die Routentabelle **FirewallSubnetRouteTable** aus.
    
18.  Klicken Sie auf **Routingregeln hinzufügen**.
    
19.  Geben Sie die erforderlichen Einträge wie folgt ein, und klicken Sie bei Bedarf auf **Weitere Routingregel**:
    
    *   **Erster Eintrag**
        *   Wählen Sie als Zieltyp **Private IP** aus.
        *   Geben Sie den **Ziel-CIDR-Block** ein
            *   In diesem Fall legen Sie **10.20.0.0/16** für ausgehenden Traffic des Serversubnetzes über **OCI Network Firewall** fest.
        *   **Zielauswahl**: Geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
20.  Fügen Sie für jeden Eintrag eine **Beschreibung** hinzu.
    

![Routentabelle für Firewallsubnetz in Firewall-VCN aktualisieren](../common/images/update-firewall-subnet-route-table-entries-firewall-vcn.png " ")

21.  Klicken Sie auf **Routingregeln hinzufügen**, um den Vorgang abzuschließen.
    
22.  Navigieren Sie zur **firewall-vcn**, und wählen Sie die Routentabelle **SGWRouteTable** aus.
    
23.  Klicken Sie auf **Routingregeln hinzufügen**.
    
24.  Wählen Sie als Zieltyp **Private IP** aus, und geben Sie die IP-Adresse der **Netzwerkfirewall** ein.
    
25.  Geben Sie den **Ziel-CIDR-Block** ein
    
    *   In diesem Fall legen Sie das gesamte Standard-CIDR **0.0.0.0/0** fest, sodass der gesamte Return Traffic vom Servicegateway über Firewall geleitet wird.
26.  Fügen Sie für jeden Eintrag eine **Beschreibung** hinzu.
    

![Routentabelleneinträge für Servicegateway in Firewall-VCN aktualisieren](../common/images/update-sgw-route-table-entry-firewall-vcn.png " ")

## Aufgabe 4: Mit Subnetzen und Gateways verknüpfte Routentabellen prüfen

1.  Die folgende Tabelle enthält die erforderlichen Subnetze in jedem **VCN** und stellt sicher, dass die **Routentabelle** an die richtigen Subnetze und das richtige Servicegateway angehängt ist.
    
    | VCN | Ressourcenname/Typ | Routentabellenname |
    | --- | --- | --- |
    | Firewall-vcn | Firewallsubnetz | FirewallSubnetRouteTable |
    | Firewall-vcn | Clientsubnetz | ClientSubnetRouteTable |
    | Firewall-vcn | Serversubnetz | ServerSubnetRouteTable |
    | Firewall-vcn | VCN-/DRG-Firewall-VCN-Anhang der Firewall | VCN-INGRESS-RouteTable |
    | Firewall-vcn | HubServiceGateway/Servicegateway | SGWRouteTable |
    | Speichen-vcn | serverA-Subnetz | ServerASubnet Routentabelle für Spoke-vcn |
    | Speichen-vcn | serverB-Subnetz | ServerBSubnet Routentabelle für Spoke-vcn |
    
2.  Im folgenden Beispiel wird dargestellt, wie die richtige Routentabelle basierend auf der obigen Tabelle an eine der Ressourcen **Servicegateway** angehängt wird:
    
    ![Servicegatewayroutentabelle zu Servicegateway in Firewall-VCN hinzufügen](../common/images/add-service-gateway-route-table-sgw-firewall-vcn.png " ")
    
3.  Im folgenden Beispiel wird dargestellt, wie die richtige Routentabelle basierend auf der obigen Tabelle an eine der Ressourcen **FirewallSubnetRouteTable** angehängt wird:
    
    ![Routentabelle für Firewallsubnetz zu Firewallsubnetz in Firewall-VCN hinzufügen](../common/images/add-firewall-subnet-route-table-firewallsubnet-firewall-vcn.png " ")
    

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