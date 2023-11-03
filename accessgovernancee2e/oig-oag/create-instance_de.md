# Compute-Instanz mit benutzerdefiniertem Image erstellen

## Einführung

In dieser Übung verwenden wir das Feature "Benutzerdefiniertes Image" von OCI. Diese neuen Compute-Instanzen werden alle Softwarepackages und Updates enthalten, die als Teil des benutzerdefinierten Images von OCI vorinstalliert sind.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Kampagnenadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   **Compute-Instanz** aus einem vorhandenen benutzerdefinierten Image erstellen
*   SSH zu Ihrer **Compute-Instanz**

## Aufgabe 1: Bei OCI-Konsole anmelden und VCN erstellen

1.  Gehen Sie zur OCI-Konsole. Klicken Sie im Menü "OCI-Services" auf "Compute" > "Instanzen".
    
2.  Wählen Sie das Ihnen zugewiesene Compartment im Dropdown-Menü auf der linken Seite des Bildschirms aus, und klicken Sie auf "VCN-Assistenten starten".
    
3.  Klicken Sie auf **VCN mit Internetverbindung erstellen** und dann auf **VCN-Assistenten starten**. ![](images/ag-logon.png)
    
4.  Füllen Sie das Dialogfeld aus:
    
    VCN-NAME: Geben Sie einen Namen an
    
    VERGLEICH: Stellen Sie sicher, dass das COMPARTMENT ausgewählt ist
    
    VCN-CIDR-BLOCK: Geben Sie einen CIDR-BLOCK an (10.0.0/16)
    
    PUBLIC SUBNET CIDR BLOCK: Geben Sie einen CIDR-Block an (10.0.1.0/24)
    
    PRIVATES SUBNETZ-CIDR-BLOCK: Geben Sie einen CIDR-BLOCK an (10.0.2.0/24)
    
    Klicken Sie auf "Next".
    

![](images/ag-homepage.png)

5.  Prüfen Sie alle Informationen, und klicken Sie auf **Erstellen**.

Dadurch wird ein VCN mit den folgenden Komponenten erstellt.

VCN, öffentliches Subnetz, privates Subnetz, Internetgateway (IG), NAT-Gateway (NAT), Servicegateway (SG)

6.  Klicken Sie auf "Virtuelles Cloud-Netzwerk anzeigen", um die VCN-Details anzuzeigen.

## Aufgabe 2: Compute-Instanz erstellen

1.  Gehen Sie zur OCI-Konsole. Klicken Sie im Menü "OCI-Services" auf "Compute" > "Instanzen". ![](images/create-campaign.png)
2.  Klicken Sie auf **Instanz erstellen**. ![](images/select-dimensions.png)
3.  Geben Sie einen Namen für die Instanz ein, und wählen Sie das Compartment aus, das Sie zuvor zum Erstellen des VCN verwendet haben. Klicken Sie im Abschnitt "Bild und Form" auf die Schaltfläche "Bearbeiten". ![](images/select-users.png)
4.  Klicken Sie auf **Image ändern**. ![](images/select-next.png)
5.  Gehen Sie im Dialogfeld "Alle Bilder durchsuchen" wie folgt vor:

Bildquelle: Benutzerdefiniertes Bild

Compartment: Stellen Sie sicher, dass das Compartment ausgewählt ist

Klicken Sie auf Bild auswählen. ![](images/select-applications.png) 6. Klicken Sie auf **Ausprägung ändern** ![](images/view-charts.png) 7. Gehen Sie im Dialogfeld "Alle Formen durchsuchen" wie folgt vor:

Instanztyp: Virtuelle Maschine auswählen

Ausprägungsreihe: Intel

Instanzausprägung: VM.Standard3. Flexibel

Klicken Sie auf "Ausprägung auswählen". ![](images/configure-workflow.png) ![](images/default-workflow.png) 8. Scrollen Sie nach unten zum Abschnitt "Networking", und wählen Sie die Schaltfläche "Edit" aus.

Virtuelles Cloud-Netzwerk: Wählen Sie das VCN aus, das Sie in Schritt 1 erstellt haben

Subnetz: Wählen Sie das öffentliche Subnetz unter öffentlichen Subnetzen aus (es muss den Namen Public Subnet-NameOfVCN haben)

Öffentliche IPv4-Adresse zuweisen: Diese Option aktivieren

SSH-Schlüssel hinzufügen: Wählen Sie "Schlüsselpaar für mich generieren", und speichern Sie den generierten Public Key und Private Key.

Boot-Volume: Behalten Sie die Standardwerte bei, deaktivieren Sie die Werte ![](images/name-campaign.png) 9. Klicken Sie auf **Erstellen.**  
![](images/summary.png)  
10\. Warten Sie, bis die Instanz den Status **Wird ausgeführt** hat. Notieren Sie sich die öffentliche IP der Instanz. Diese benötigen Sie später. ![](images/view-created-campaign.png) 11. SSH für Ihre Compute-Instanz:

    ssh -i <sshkeyname> opc@<PUBLIC_IP_OF_COMPUTE>
    
    **HINT:** If 'Permission denied error' is seen, ensure you are using '-i' in the ssh command. You MUST type the command, do NOT copy and paste ssh command.
    

13.  Geben Sie "yes" ein, wenn Sie zur Eingabe der Sicherheitsmeldung aufgefordert werden.
14.  Prüfen Sie, ob opc@<COMPUTE\_INSTANCE\_NAME> in der Eingabeaufforderung angezeigt wird.
15.  Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autor** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Cloud Platform COE, Januar 2023