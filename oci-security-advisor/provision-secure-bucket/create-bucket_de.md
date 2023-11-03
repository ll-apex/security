# Einen sicheren Bucket mit OCI Security Advisor erstellen

## Einführung

In dieser Übung werden die Schritte zum Erstellen eines sicheren Buckets mit Security Advisor beschrieben. Dabei wird nicht nur der Bucket erstellt, sondern auch ein Vault und ein Schlüssel erstellt, mit dem der Bucket verschlüsselt wird und die von Sicherheitszonen festgelegten Mindestsicherheitsanforderungen erfüllt werden. Vorhandener Schlüssel kann auch zum Importieren in einen Vault und zum Erstellen eines sicheren Buckets verwendet werden.

Voraussichtliche Zeit: 30 Minuten

### Ziele

In dieser Übung lernen Sie Folgendes:

*   Zone mit maximaler Sicherheit und Compartment mit maximaler Sicherheit erstellen
*   Sicherheitszugriff in Policy erteilen, um Ressourcen im Compartment zu erstellen
*   OCI-Key Vault erstellen
*   Secure Object Storage-Bucket mit OCI Security Advisor erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Einen kostenlosen Oracle Cloud-Account oder einen LiveLabs-Account
*   IAM-Policys zum Erstellen von Ressourcen im Compartment

## Aufgabe 1: Zone mit maximaler Sicherheit und Compartment mit maximaler Sicherheit erstellen

1.  Melden Sie sich bei Oracle Cloud an.
    
2.  Sobald Sie angemeldet sind, gelangen Sie zum Cloud-Service-Dashboard, in dem Sie alle für Sie verfügbaren Services anzeigen können. Klicken Sie oben links auf das Navigationsmenü, um Navigationsoptionen der obersten Ebene anzuzeigen. Klicken Sie unter Optionen auf Identity & Security und dann auf Security Zones.
    

![](./images/image1.png " ")

3.  Geben Sie einen Namen und eine Beschreibung für die Sicherheitszone ein. Oracle Cloud erstellt ein Compartment mit demselben Namen und weist es dieser Sicherheitszone zu. Geben Sie keine vertraulichen Informationen ein. Navigieren Sie unter "Erstellen in Compartment" zu dem Compartment, in dem Sie das neue Compartment erstellen möchten, und klicken Sie auf "Sicherheitszone erstellen".

![](./images/image2.png " ")

4.  Sie können das neue Compartment für maximale Sicherheit anzeigen, das in der Sicherheitszone erstellt wurde. Klicken Sie oben links auf das Navigationsmenü, um Navigationsoptionen der obersten Ebene anzuzeigen. Klicken Sie unter Optionen auf Identität und Sicherheit und dann auf Compartments. Das erstellte Compartment und die Sicherheitszonenoption "Aktiviert" werden angezeigt, wie im folgenden Bild dargestellt.

![](./images/image3.png " ")

5.  Standard-Policys werden auf das neue Sicherheitszonen-Compartment angewendet. Öffnen Sie das Navigationsmenü, und klicken Sie auf Identity & Security.Under Security Zones, und klicken Sie auf Overview. Klicken Sie auf den Namen der Sicherheitszone und dann auf das Rezept für die Sicherheitszone.

![](./images/image4.png " ")

## Aufgabe 2: Sicherheitszugriff in Policy erteilen, um Ressourcen im Compartment zu erstellen

*   Um Oracle Cloud Infrastructure verwenden zu können, muss ein Administrator Ihnen Sicherheitszugriff in einer Policy erteilen.
    
*   Erstellen Sie als Administrator die folgenden Richtlinien:
    
        ```
        Ermöglichen Sie der Gruppe CreateSecureOSBucketGroup die Verwaltung der Objektfamilie in Compartment compartment\_name, damit die Gruppe CreateSecureOSBucketGroup Vaults in Compartment compartment\_name verwalten kann, damit die Gruppe CreateSecureOSBucketGroup Schlüssel in Compartment compartment\_name verwalten kann, damit der Service ObjectStorage Schlüssel in Compartment compartment\_name verwenden kann \`\`

## Aufgabe 3: OCI Key Vault erstellen

1.  Klicken Sie im Navigationsmenü auf Identität und Sicherheit und dann auf Vault.
    
    ![](./images/image5.png " ")
    
2.  Klicken Sie unter "Listengeltungsbereich" in der Compartment-Liste auf den Namen des Compartments, in dem Sie den Vault erstellen möchten. Klicken Sie auf "Vault erstellen", und klicken Sie im Dialogfeld "Vault erstellen" auf "Name", und geben Sie einen Anzeigenamen für den Vault ein. Optional können Sie den Vault zu einem virtuellen privaten Vault machen, indem Sie das Kontrollkästchen "Als virtuellen privaten Vault festlegen" aktivieren.
    
    ![](./images/image6.png " ")
    
3.  Nachdem der Vault erstellt wurde, werden die Details wie im folgenden Bild dargestellt angezeigt.
    
    ![](./images/image7.png " ")
    

## Aufgabe 4: Sicheren Object Storage-Bucket erstellen

1.  Öffnen Sie das Navigationsmenü, und klicken Sie auf **Identität und Sicherheit** und dann auf **Security Advisor**.

![](./images/bucket-image1.png " ")

2.  Das Security Advisor-Fenster wird wie folgt angezeigt. Security Advisor verwendet einen Workflow, der die Verwendung unterstrichener Best Practices zur Verwendung von vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge durchsetzt, die von **Sicherheitszonen** und **Compartment für maximale Sicherheit** durchgesetzt werden. Klicken Sie auf die Schaltfläche {\\b Create Secure Bucket}, um den Security Advisor-Workflow zu starten. Mit Security Advisor-Workflows können Sie Ressourcen erstellen, die den erweiterten Sicherheitsanforderungen dieser Umgebungen entsprechen.

![](./images/bucket-image2.png " ")

3.  Security Advisor Workflow ist ein Prozess in fünf Schritten, mit dem Sie einen sicheren Bucket mit vom Kunden verwalteten Verschlüsselungsschlüsseln mit hoher Länge erstellen können, um sensible Daten in OCI zu sichern. In den Fenstern "Erste Schritte" werden die erforderlichen Schritte aufgeführt, die vor dem Provisioning oder Erstellen des Buckets ausgeführt werden müssen, wie im folgenden Bild dargestellt.

![](./images/bucket-image9.png " ")

4.  Klicken Sie auf **Beispiel für erforderliche Policys anzeigen**, um die Liste der Policys abzurufen, die zum Erstellen eines Buckets mit Security Advisor erforderlich sind. Diese erforderlichen Schritte werden in Aufgabe 2 ausgeführt. Überprüfen Sie die Details und klicken Sie auf Weiter.

![](./images/bucket-image3.png " ")

5.  Wählen Sie das Compartment aus, in dem sich der Vault befindet, und wählen Sie dann den Vault aus, wie im folgenden Bild gezeigt.

![](./images/bucket-image4.png " ")

6.  Geben Sie die Eingabeparameter wie folgt an:
    
    *   Klicken Sie auf "Name", und geben Sie einen Namen zur Identifizierung des Schlüssels ein. Geben Sie keine vertraulichen Informationen ein.
    *   Klicken Sie auf "Schlüsselform: Algorithmus", und wählen Sie einen der folgenden Algorithmen aus:
        *   **AES**: Advanced Encryption Standard-(AES-)Schlüssel sind symmetrische Schlüssel, mit denen Sie ruhende Daten verschlüsseln können.
        *   **RSA**: Rivest-Shamir-Adleman-(RSA-)Schlüssel sind asymmetrische Schlüssel, auch als Schlüsselpaare bezeichnet, die aus einem Public Key und einem Private Key bestehen, mit denen Sie Daten während der Übertragung verschlüsseln, Daten signieren und die Integrität signierter Daten prüfen können.
        *   **ECDSA**: Elliptic Curve Cryptography Digital Signature Algorithm-(ECDSA-)Schlüssel sind asymmetrische Schlüssel, mit denen Sie Daten signieren und die Integrität signierter Daten prüfen können.
    *   Wählen Sie **Schlüsselausprägung: Länge** basierend auf **Schlüsselausprägung: Algorithmus** aus. Bei AES-Schlüsseln unterstützt der Vault-Service Schlüssel mit genau 128 Bit, 192 Bit oder 256 Bit Länge.
    *   Wählen Sie die Option "Externen Schlüssel importieren" aus, um den vom Kunden verwalteten vorhandenen Schlüssel zu verwenden. Dadurch können Sie die Datei aus dem lokalen Dateisystem auswählen und in OCI Valut hochladen.

![](./images/bucket-image5.png " ")

7.  Geben Sie auf der Seite "Bucket erstellen" die Attribute des Bucket wie folgt an, und klicken Sie auf "Weiter":
    *   Zeitraum: Geben Sie den Namen Ihrer Wahl ein
    *   In Compartment erstellen: In Aufgabe 1 erstelltes Compartment auswählen
    *   Storage Tier: Standard oder Archiv
    *   Objekte - Ereignisse
    *   Objektversionierung

![](./images/bucket-image6.png " ")

8.  Auf der Übersichtsseite werden alle Details angezeigt, die in den vorherigen Schritten als Teil des Security Advisor-Workflows angegeben wurden. Prüfen Sie die Details, und klicken Sie auf **Sicheren Bucket erstellen**.

![](./images/bucket-image7.png " ")

9.  Der Objektspeicher-Bucket wird mit dem ausgewählten Verschlüsselungsschlüssel erstellt und verschlüsselt.

![](./images/bucket-image8.png " ")

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   Weitere Informationen zu OCI Security Cloud Advisor finden Sie [hier](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Danksagungen

*   **Autor** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Mitwirkende** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering
*   **Zuletzt aktualisiert am/um** - Sanjay Rahane, Senior Cloud Engineer, NA Cloud Engineering, September 2021