# Sichere Virtual-Machine-Instanz mit OCI Security Advisor erstellen

## Einführung

In dieser Übung werden die Schritte zum Erstellen einer Secure Virtual Machine-Instanz mit Security Advisor beschrieben. Dabei wird nicht nur die virtuelle Maschine erstellt, sondern auch ein Vault und ein Schlüssel erstellt, mit dem die VM-Daten verschlüsselt werden und die von Sicherheitszonen festgelegten Mindestsicherheitsanforderungen erfüllt werden. Vorhandener Schlüssel kann auch zum Importieren in einen Vault und zum Erstellen einer sicheren VM-Instanz verwendet werden.

Voraussichtliche Zeit: 15 Minuten

### Ziele

In dieser Übung lernen Sie Folgendes:

*   Kundenverwalteten Verschlüsselungsschlüssel erstellen oder importieren
*   Sichere Virtual-Machine-Instanz mit OCI Security Advisor erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen kostenlosen Oracle Cloud-Account oder einen LiveLabs-Account
*   IAM-Policys zum Erstellen von Ressourcen im Compartment

## Aufgabe 1: Sichere VM-Instanz erstellen

1.  Öffnen Sie das Navigationsmenü, und klicken Sie auf **Identität und Sicherheit** und dann auf **Security Advisor**.

![](./images/instanceimage1.png " ")

2.  Das Security Advisor-Fenster wird wie folgt angezeigt. Security Advisor verwendet einen Workflow, der die Verwendung unterstrichener Best Practices zur Verwendung von vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge durchsetzt, die von **Sicherheitszonen** und **Compartment für maximale Sicherheit** durchgesetzt werden. Klicken Sie auf die Schaltfläche **Sichere Instanz erstellen**, um den Security Advisor-Workflow zu starten. Mit Security Advisor-Workflows können Sie Ressourcen erstellen, die den erweiterten Sicherheitsanforderungen dieser Umgebungen entsprechen.

![](./images/instanceimage2.png " ")

3.  Security Advisor Workflow ist ein Prozess in fünf Schritten, mit dem Sie einen sicheren Bucket mit vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge erstellen können, um sensible Daten in OCI zu sichern. In den Fenstern "Erste Schritte" werden die erforderlichen Schritte aufgeführt, die vor dem Provisioning oder Erstellen der sicheren Instanz ausgeführt werden müssen, wie im folgenden Bild dargestellt.

![](./images/instanceimage3.png " ")

4.  Klicken Sie auf **Beispiel für erforderliche Policys anzeigen**, um die Liste der Policys abzurufen, die zum Erstellen einer sicheren Compute-Instanz mit Security Advisor erforderlich sind. Diese erforderlichen Schritte werden in Übung 1, Aufgabe 2 dieses Workshops durchgeführt. Überprüfen Sie die Details und klicken Sie auf Weiter.

![](./images/instanceimage4.png " ")

5.  Wählen Sie das Compartment aus, in dem sich der Vault befindet, und wählen Sie dann den Vault aus, wie im folgenden Bild gezeigt.

![](./images/instanceimage5.png " ")

6.  Geben Sie die Eingabeparameter wie folgt an:
    
    *   Klicken Sie auf "Name", und geben Sie einen Namen zur Identifizierung des Schlüssels ein. Geben Sie keine vertraulichen Informationen ein.
    *   Klicken Sie auf "Schlüsselform: Algorithmus", und wählen Sie einen der folgenden Algorithmen aus:
        *   **AES**: Advanced Encryption Standard-(AES-)Schlüssel sind symmetrische Schlüssel, mit denen Sie ruhende Daten verschlüsseln können.
        *   **RSA**: Rivest-Shamir-Adleman-(RSA-)Schlüssel sind asymmetrische Schlüssel, auch als Schlüsselpaare bezeichnet, die aus einem Public Key und einem Private Key bestehen, mit denen Sie Daten während der Übertragung verschlüsseln, Daten signieren und die Integrität signierter Daten prüfen können.
        *   **ECDSA**: Elliptic Curve Cryptography Digital Signature Algorithm-(ECDSA-)Schlüssel sind asymmetrische Schlüssel, mit denen Sie Daten signieren und die Integrität signierter Daten prüfen können.
    *   Wählen Sie **Schlüsselausprägung: Länge** basierend auf **Schlüsselausprägung: Algorithmus** aus. Bei AES-Schlüsseln unterstützt der Vault-Service Schlüssel mit genau 128 Bit, 192 Bit oder 256 Bit Länge.
    *   Wählen Sie die Option "Externen Schlüssel importieren" aus, um den vom Kunden verwalteten vorhandenen Schlüssel zu verwenden. Dadurch können Sie die Datei aus dem lokalen Dateisystem auswählen und in OCI Valut hochladen.

![](./images/instanceimage6.png " ")

7.  Geben Sie auf der Seite "Compute-Instanz erstellen" die Attribute der Instanz wie folgt an, und klicken Sie auf "Weiter":
    *   Name: Geben Sie den Namen Ihrer Wahl ein
    *   In Compartment erstellen: Wählen Sie das Compartment aus, das in Übung 1, Aufgabe 1 dieses Workshops erstellt wurde
    *   Image oder Betriebssystem: Betriebssystemimage auswählen
    *   Availability-Domain: AD gemäß Ihrer Auswahl aus der Liste auswählen

![](./images/instanceimage7.png " ")

8.  Wählen Sie im Rahmen von "Networking konfigurieren" das sichere Compartment ZOne, das sichere VCN und das sichere Subnetz aus (siehe Abbildung unten). Subnetz darf nur das private Subnetz sein, um die Sicherheits-Guidelones einzuhalten, die von der Zone mit maximaler Sicherheit und dem Compartment mit maximaler Sicherheit durchgesetzt werden.

![](./images/instanceimage8.png " ")

9.  Wählen Sie im folgenden Abschnitt bei Bedarf das Custome-Boot-Volume aus. Im Abschnitt _SSH-Schlüssel für ADD_ muss der Public Key ausgewählt werden, der für die Verbindung zu dieser Instanz in einem privaten Subnetz verwendet wird. Wählen Sie den öffentlichen SSH-Schlüssel, und klicken Sie auf **Weiter**.

![](./images/instanceimage9.png " ")

10.  Auf der Übersichtsseite werden alle Details angezeigt, die in den vorherigen Schritten als Teil des Security Advisor-Workflows angegeben wurden. Prüfen Sie die Details, und klicken Sie auf **Secure Virtual Machine-Instanz erstellen**.

![](./images/instanceimage10.png " ")

11.  Die sichere Virtual-Machine-Instanz wird erstellt, und die Daten werden mit dem ausgewählten Verschlüsselungsschlüssel verschlüsselt.

![](./images/instanceimage11.png " ")

    **Congratulations !!! You Have Completed Successfully The Workshop .**
    

## Weitere Informationen

*   Weitere Informationen zu OCI Security Cloud Advisor finden Sie [hier](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Danksagungen

*   **Autor** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Mitwirkende** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Zuletzt aktualisiert am/um** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering, September 2021