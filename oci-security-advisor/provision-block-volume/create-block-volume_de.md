# Sicheres Block-Volume mit OCI Security Advisor erstellen

## Einführung

In dieser Übung werden die Schritte zum Erstellen eines sicheren Block-Volumes mit Security Advisor beschrieben. Dabei werden nicht nur das Block-Volume erstellt, sondern auch ein Vault und ein Schlüssel erstellt, mit dem die Block-Volume-Daten verschlüsselt werden und die von Sicherheitszonen festgelegten Mindestsicherheitsanforderungen erfüllt werden. Vorhandener Schlüssel kann auch zum Importieren in einen Vault und zum Erstellen eines sicheren Block-Volumes verwendet werden.

Voraussichtliche Zeit: 15 Minuten

### Ziele

In dieser Übung lernen Sie Folgendes:

*   Kundenverwalteten Verschlüsselungsschlüssel erstellen oder importieren
*   Sicheres Block-Volume mit OCI Security Advisor erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen kostenlosen Oracle Cloud-Account oder einen LiveLabs-Account
*   IAM-Policys zum Erstellen von Ressourcen im Compartment

## Aufgabe 1: Sicheres Block-Volume erstellen

1.  Öffnen Sie das Navigationsmenü, und klicken Sie auf **Identität und Sicherheit** und dann auf **Security Advisor**.

![](./images/bvimage1.png " ")

2.  Das Security Advisor-Fenster wird wie folgt angezeigt. Security Advisor verwendet einen Workflow, der die Verwendung unterstrichener Best Practices zur Verwendung von vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge durchsetzt, die von **Sicherheitszonen** und **Compartment für maximale Sicherheit** durchgesetzt werden. Klicken Sie auf die Schaltfläche **Sicheres Block-Volume erstellen**, um den Security Advisor-Workflow zu starten. Mit Security Advisor-Workflows können Sie Ressourcen erstellen, die den erweiterten Sicherheitsanforderungen dieser Umgebungen entsprechen.

![](./images/bvimage2.png " ")

3.  Der Security Advisor-Workflow ist ein Prozess in fünf Schritten, bei dem ein sicheres Block-Volume mit vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge erstellt wird, um sensible Daten in OCI zu sichern. In den Fenstern "Erste Schritte" werden die erforderlichen Schritte aufgeführt, die vor dem Provisioning oder Erstellen des Block-Volumes ausgeführt werden müssen, wie im folgenden Bild dargestellt.

![](./images/bvimage3.png " ")

4.  Klicken Sie auf **Beispiel für erforderliche Policys anzeigen**, um die Liste der Policys abzurufen, die zum Erstellen eines Block-Volumes mit Security Advisor erforderlich sind. Diese erforderlichen Schritte werden in Übung 1, Aufgabe 1 dieses Workshops durchgeführt. Überprüfen Sie die Details und klicken Sie auf Weiter.

![](./images/bvimage4.png " ")

5.  Wählen Sie das Compartment aus, in dem sich der Vault befindet, und wählen Sie dann den Vault aus, wie im folgenden Bild gezeigt.

![](./images/bvimage5.png " ")

6.  Geben Sie die Eingabeparameter wie folgt an:
    
    *   Klicken Sie auf "Name", und geben Sie einen Namen zur Identifizierung des Schlüssels ein. Geben Sie keine vertraulichen Informationen ein.
    *   Klicken Sie auf "Schlüsselform: Algorithmus", und wählen Sie einen der folgenden Algorithmen aus:
        *   **AES**: Advanced Encryption Standard-(AES-)Schlüssel sind symmetrische Schlüssel, mit denen Sie ruhende Daten verschlüsseln können.
        *   **RSA**: Rivest-Shamir-Adleman-(RSA-)Schlüssel sind asymmetrische Schlüssel, auch als Schlüsselpaare bezeichnet, die aus einem Public Key und einem Private Key bestehen, mit denen Sie Daten während der Übertragung verschlüsseln, Daten signieren und die Integrität signierter Daten prüfen können.
        *   **ECDSA**: Elliptic Curve Cryptography Digital Signature Algorithm-(ECDSA-)Schlüssel sind asymmetrische Schlüssel, mit denen Sie Daten signieren und die Integrität signierter Daten prüfen können.
    *   Wählen Sie **Schlüsselausprägung: Länge** basierend auf **Schlüsselausprägung: Algorithmus** aus. Bei AES-Schlüsseln unterstützt der Vault-Service Schlüssel mit genau 128 Bit, 192 Bit oder 256 Bit Länge.
    *   Wählen Sie die Option "Externen Schlüssel importieren" aus, um den vom Kunden verwalteten vorhandenen Schlüssel zu verwenden. Dadurch können Sie die Datei aus dem lokalen Dateisystem auswählen und in OCI Valut hochladen.

![](./images/bvimage6.png " ")

7.  Geben Sie auf der Seite "Block-Volume erstellen" die Attribute des Block-Volumes wie folgt an, und klicken Sie auf "Weiter":
    *   Block-Volume-Name: Geben Sie den Namen Ihrer Wahl ein
    *   In Compartment erstellen: Wählen Sie das Compartment aus, das in Übung 1, Aufgabe 1 dieses Workshops erstellt wurde.
    *   Availability-Domain: Wählen Sie AD aus der Werteliste aus, wie in der Abbildung dargestellt
    *   Volume-Größe: Standard/Benutzerdefiniert

![](./images/bvimage7.png " ")

8.  Auf der Übersichtsseite werden alle Details angezeigt, die in den vorherigen Schritten als Teil des Security Advisor-Workflows angegeben wurden. Prüfen Sie die Details, und klicken Sie auf **Sicheres Block-Volume erstellen**.

![](./images/bvimage8.png " ")

9.  Object Storage Block Volume wird mit dem ausgewählten Verschlüsselungsschlüssel erstellt und verschlüsselt.

![](./images/bvimage9.png " ")

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   Weitere Informationen zu OCI Security Cloud Advisor finden Sie [hier](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Danksagungen

*   **Autor** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Mitwirkende** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Zuletzt aktualisiert am/um** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering, September 2021