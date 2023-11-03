# Scanning- und Sicherheitslückenberichte untersuchen

## Einführung

In dieser Übung validieren Sie Scan- und Sicherheitslückenberichte von den Plattformen **VSS**, **CloudGuard** und **Qualys VMDR**.

> **Bitte lesen**: Qualys führt alle **vier Stunden** einen Scan von OCI-Hosts durch, sodass Sie Berichte im **Qualys**\-Portal ungefähr in vier Stunden anzeigen können. Zeigen Sie die Qualys-Scanergebnisse in der OCI-Konsole innerhalb von **12 Stunden** nach dem Erstellen des neuen Scanziels an

Geschätzte Zeit: 10 Minuten.

### Ziele

*   Scan- und Sicherheitslückenberichte aus OCI VSS validieren
*   Scanning- und Sicherheitslückenberichte aus OCI validieren CloudGuard
*   Scanning- und Sicherheitslückenberichte von Qualys VMDR Platform validieren

### Voraussetzungen

*   Oracle Cloud Infrastructure-Accountzugangsdaten (Benutzer, Kennwort, Mandant und Compartment)
*   Der Benutzer muss über die erforderlichen Berechtigungen und die Quota zum Bereitstellen/Anzeigen von Ressourcen verfügen.

## Aufgabe 1: Scanning- und Sicherheitslückenberichte aus OCI VSS validieren

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **Scannen**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus.
    
2.  Navigieren Sie zu den **Scanning-Berichten**, um einzelne **Hostziel**berichte anzuzeigen.
    
    ![Scanberichte für Compute-Hosts](../common/images/compute-target-scanning-reports.png " ")
    
3.  Navigieren Sie zu Ihren **Sicherheitslückenberichten**, um einzelne **CVE: QID**\-Berichte anzuzeigen.
    
    ![Berichte zu Sicherheitslücken bei Compute-Hosts](../common/images/compute-target-vulnerabilities-reports.png " ")
    
4.  Klicken Sie auf die Sicherheitslücke, um mehr über und zugehörige Details zu erfahren. Beispiel: **QID: 159493**:
    
    ![Sicherheitslückendetails für Compute-Hosts](../common/images/compute-target-vulnerabilities-details-reports.png " ")
    

## Aufgabe 2: Scan- und Sicherheitslückenberichte von OCI untersuchen CloudGuard

1.  Klicken Sie im Menü "OCI-Services" unter **Identität und Sicherheit** auf **CloudGuard**. Wählen Sie Ihre Region rechts auf dem Bildschirm aus.
    
    ![Homepage der Compute-Hosts CloudGuard](../common/images/cloudguard-home-page.png " ")
    
2.  Navigieren Sie zu den **Problemen**, um einzelne **Hostziel**probleme anzuzeigen.
    
    ![Probleme mit CloudGuard von Compute-Hosts](../common/images/cloudguard-problem-page.png " ")
    

## Aufgabe 3: Scanning- und Sicherheitslückenberichte von Qualys VMDR validieren

1.  Stellen Sie eine Verbindung zu **Qualys VMDR** her, und navigieren Sie zum **Dashboard**, um einen Überblick über **Sicherheitslücken** und **Assets** anzuzeigen:
    
    ![Qualys VMDR-Dashboard](../common/images/qualys-vmdr-vulnerabilities.png " ")
    
2.  Navigieren Sie zu **Verbindlichkeiten > Anlagen**, um einzelne **Anlagendetails** anzuzeigen:
    
    ![Qualys VMDR-Sicherheitslücken](../common/images/qualys-vmdr-vulnerabilities-hosts.png " ")
    
3.  Navigieren Sie zur Registerkarte **Sicherheitslücken > Sicherheitslücke**, um weitere Berichte anzuzeigen:
    
    ![Qualys VMDR-Sicherheitslücken](../common/images/qualys-vmdr-vulnerabilities-hosts-details.png " ")
    
4.  Klicken Sie auf einzelne Sicherheitsanfälligkeit, um mehr über die Details zu erfahren und die erforderliche Aktion auszuführen:
    
    ![Qualys VMDR-Sicherheitslücken](../common/images/qualys-vmdr-vulnerability.png " ")
    

_**Glückwunsch! Sie haben die Übung abgeschlossen.**_

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

1.  [OCI-Schulungen](https://www.oracle.com/cloud/iaas/training/)
2.  [Vertrautheit mit der OCI-Konsole](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Überblick über OCI Vulnerability Scanning Service](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [OCI Vulnerability Scanning-Serviceseite](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Funktionen von OCI CloudGuard](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Danksagungen

*   **Autor** - Arun Poonia, Principal Solutions Architect
*   **Angepasst von** - Oracle
*   **Mitwirkende** - nicht zutreffend
*   **Zuletzt aktualisiert am/um** - Arun Poonia, Aug 2023