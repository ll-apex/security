# OCI-Policys, -Gruppen und -Compartments erstellen

## Einführung

Als Benutzer mit der Rolle **Administrator** in der Identitätsdomain können Sie OCI-Policys, -Gruppen und -Compartments in der Übung **OCI** console.This erstellen. In dieser Übung erfahren Sie, wie Sie die ZIP-Stackdatei herunterladen, die zum Einrichten der OCI-Policys, -Gruppen und -Compartments erforderlich ist, um diese OCI-IAM-Policy-Prüfungen auszuführen.

*   Geschätzte Zeit: 15 Minuten
*   Persona: Identitätsdomainadministrator

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Laden Sie die Stack-ZIP-Datei herunter
*   OCI-Policys, -Gruppen und -Compartments mit dem Stack erstellen

## Aufgabe 1: ZIP-Datei des Policy-Stacks herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    [Test - IAM - Policys - Active.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/IAM-Policies-Sample.zip)
    
2.  Speichern Sie in Ihrem Download-Ordner.
    

## Aufgabe 2: Stack erstellen

1.  Identifizieren Sie die ZIP-Datei des Policy-Stacks, die in der vorherigen Aufgabe der Übung heruntergeladen wurde.
    
2.  Melden Sie sich bei der Identitätsdomain an: ag-domain von Oracle Cloud als Identitätsdomainadministrator.
    
3.  Navigieren Sie zu Identity -> Domains -> ag-domain. Kopieren Sie die Domain-URL, wie sie in den weiteren Schritten erforderlich ist.
    
    ![Domain-URL abrufen](images/domain-url.png)
    
4.  Öffnen Sie das Hamburger-Menü oben links. Klicken Sie auf Developer Services, und wählen Sie Resource Manager > Stacks. Wählen Sie das Compartment aus, in dem Sie den Stack installieren möchten. Klicken Sie auf "Stack erstellen".
    

![Zu Stack navigieren](images/navigate-to-stack.png)

5.  Wählen Sie "Meine Konfiguration", wählen Sie die Schaltfläche "Zip-Datei", klicken Sie auf den Link "Durchsuchen", und wählen Sie die heruntergeladene ZIP-Datei oder ziehen Sie sie per Drag-and-Drop für den Datei-Explorer.

![Klicken Sie auf "Stack erstellen".](images/click-create-stack.png)

6.  Klicken Sie auf "Next".

![Klicken Sie auf "Next".](images/click-next.png)

7.  Geben Sie unter Variablen konfigurieren die Domain-URL ein.

![Variablen konfigurieren](images/configure-variables.png)

8.  Klicken Sie auf "Erstellen".

![Klicken Sie auf "Create".](images/stack-created.png)

![Policy-Stack erstellt](images/policy-stack-created.png)

9.  Klicken Sie auf "Plan". Wenn der Job erfolgreich ausgeführt wurde, klicken Sie auf "Anwenden".

![Policy-Stack - Planen und Anwenden](images/plan-apply.png)

10.  Ihr Stack wurde jetzt erstellt, und die ausgelöste Aktion "Anwenden" wird ausgeführt, um Ihre Umgebung bereitzustellen

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Oracle Access Governance - Zugriffsprüfungskampagne erstellen](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Oracle Access Governance - Produktseite](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Oracle Access Governance - Produkttour](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Oracle Access Governance - Häufig gestellte Fragen](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Bestätigungen

*   **Autoren** - Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Zuletzt aktualisiert am/um** - Anbu Anbarasu, Mai 2023