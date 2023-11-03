# Einführung

### Über diesen Workshop

Wussten Sie, dass Sie Ihre Verschlüsselungsschlüssel außerhalb von Oracle Cloud Infrastructure verwalten können?

Kundengesteuertes Schlüsselmanagement ist eine wichtige Sicherheitsanforderung für viele Kunden auf der ganzen Welt, insbesondere um Souveränitätsanforderungen zu erfüllen.

In dieser praktischen Übung lernen Sie, wie Sie Ihre Verschlüsselungsschlüssel außerhalb von OCI mit einem Thales HSM verwalten. Mit den Features "Bring Your Own Key" und "Hold Your Own Key", die wir als "Externes KMS" bezeichnen, behalten Sie die Kontrolle über Ihre Schlüssel, speichern sie dort, wo Sie möchten, und Sie können weiterhin die erweiterten Features IaaS und PaaS von OCI verwenden.

In dieser praktischen Übung erfahren Sie, wie Sie das Feature aktivieren, Ihr HSM außerhalb von OCI mit Ihrem OCI-Mandanten verknüpfen und mit der Verschlüsselung von Daten in OCI zur Speicherverschlüsselung und OCI-Datenbankverschlüsselung über TDE-Support beginnen. Jetzt verlassen Ihre Masterverschlüsselungsschlüssel niemals Ihren eigenen HSM. Außerdem simulieren Sie eine Notsituation, in der Sie aufgefordert werden, den Zugriff auf Daten zu blockieren, die in OCI Storage Buckets oder Database Service gespeichert sind. Du hast die Kontrolle.

Voraussichtliche Zeit: 1,5 Stunden

> **Hinweis:** In dieser Übung verwenden Sie wirklich einen externen KMS-Service: Jeder Studierende hat einen Thales CipherTrust Manager-Mandanten. Dabei handelt es sich um einen Key Management as a Service-Mandanten für Key Management auf Basis von Thales HSM als Servicelösung.

### Ziele

In diesem Workshop erfahren Sie, wie Sie:

*   Stellen Sie eine Verbindung zu den beiden Umgebungen her, die Sie zur Durchführung der Übung verwenden müssen: ein OCI-Mandant und ein Thales CipherTrust Manager-Mandant
    
*   Erstellen Sie einen Vault in OCI, und fügen Sie eine Verbindung zwischen Thales CipherTrust Manager und OCI Vault hinzu
    
*   Oracle-Schlüssel in Thales CipherTrust Manager erstellen
    
*   Bring Your Own Key (BYOK) zu OCI
    
    > BYOK bedeutet "Bring Your Own Key". Es ist das Konzept, Oracle Cloud Infrastructure-(OCI-)Verschlüsselungsschlüssel zu verwenden, die außerhalb von OCI erstellt wurden. Dies kann erforderlich sein, um bestimmte Vorschriften einzuhalten, die vorschreiben, dass die Verschlüsselungsschlüsselerstellungszeremonie außerhalb der Cloud durchgeführt wird. BYOK ist mit jedem Schlüsselmaterial kompatibel, das Sie erstellen und mit von OCI Vault unterstützten Schlüsseltypen kompatibel ist. Eine vollständige Liste der von OCI Vault unterstützten Schlüsseltypen finden Sie unter folgendem Link: [Unterstützte Schlüsseltypen und Schlüsseldimensionen](https://docs.oracle.com/en-us/iaas/Content/KeyManagement/Tasks/importingkeys.htm)
    
*   Vom Kunden verwaltete Verschlüsselung für OCI-Ressourcen konfigurieren
    
*   Notfallsimulationstest (I): Blockieren des Datenzugriffs
    
*   Notfallsimulationstest (II): Erneutes Aktivieren des Datenzugriffs
    

Nach dieser Übung finden Sie in Ihrem eigenen OCI-Mandanten ein neues Feature namens "Externes KMS". Damit können Sie genau dasselbe tun wie in dieser Übung. Der Schlüssel wird jedoch nicht zwischen dem externen Key Management-System und OCI Vault synchronisiert. In der Tat bleibt der Masterverschlüsselungsschlüssel jederzeit in Ihrem HSM, unabhängig davon, ob er sich in Ihrem eigenen Rechenzentrum befindet oder von einem Dritten gehostet wird. Ab heute ist "Externes KMS" oder "Halten Sie Ihren eigenen Schlüssel", wie es oft genannt wird, nur mit Thales CipherTrust Manager kompatibel. Daher verwenden Sie in dieser Übung den Key Manager THALES CipherTrust.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-Account
*   Ein Thales CipherTrust Key Manager-as-a-Service-Account

Beide Umgebungen werden Ihnen von den Trainern zur Verfügung gestellt. Wenn Sie Ihre eigenen Zugangsdaten und Ihr eigenes Passwort nicht erhalten haben, fragen Sie bitte die Trainer im Zimmer.

## Laboraufschlüsselung

*   **Übung 1:** Linkerstellung zwischen OCI und der Erstellung von HSM- und Masterverschlüsselungsschlüsseln
*   **Übung 2:** Eigenen Schlüssel in Oracle Cloud Infrastructure verwenden (BYOK zu OCI)
*   **Übung 3:** Vom Kunden verwaltete Verschlüsselung für OCI-Ressourcen konfigurieren
*   **Übung 4:** Notfallsimulationstest: Datenzugriff blockieren
*   **Übung 5:** Notfallsimulationstest: Datenzugriff erneut aktivieren

## Weitere Schritte

Sie sind bereit, die Übungen zu beginnen! Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Key Management mit OCI Vault](https://www.oracle.com/security/cloud-security/key-management/)
*   [Thales CipherTrust-Manager](https://cpl.thalesgroup.com/en-gb/encryption/ciphertrust-manager)

## Danksagungen

*   **Autoren** - Damien Rilliard (OCI Security Senior Director), Sonia Yuste (OCI Security Specialist)
*   **Zuletzt aktualisiert am/um** - Damien Rilliard, Juli 2023