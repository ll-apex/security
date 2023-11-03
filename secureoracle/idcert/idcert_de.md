# Identitätszertifizierung

## Einführung

Bei der Identitätszertifizierung werden Benutzerberechtigungen und Zugriffsberechtigungen in einem Unternehmen geprüft, um sicherzustellen, dass Benutzer keine Berechtigungen erworben haben, für die sie keine Berechtigung haben. Außerdem muss jede Zugriffsberechtigung genehmigt (zertifiziert) oder abgelehnt (entzogen) werden. In dieser Übung prüfen wir die Benutzerzertifizierung mit benutzerdefinierten Prüfern.

Folgende Anwendungsfälle sind verfügbar:

*   Benutzerzertifizierung mit benutzerdefinierten Prüfern

_Geschätzte Laborzeit_: 60 Minuten

### Ziele

*   Machen Sie sich mit Identitätszertifizierungen vertraut

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   SSH-Private Key für den Zugriff auf den Host über SSH
*   Sie haben abgeschlossen:
    *   Übung: SSH-Schlüssel generieren (nur _Free-tier_ und _bezahlte Mandanten_)
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Zugriff auf erforderliche Komponenten und Anwendungen validieren

1.  Stellen Sie sicher, dass Sie auf die Konsolen OIM Admin und Self Service, Roundcube E-Mail-Client und My HR Application zugreifen können. Die folgenden Links sind auch für Firefox auf Ihrem Remote-Desktop mit Lesezeichen versehen. Weitere Informationen finden Sie unter _Übung: Umgebung initialisieren_.
    
    Oracle Identity Manager-Admin-Konsole:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Self Service -
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    E-Mail-Webclient (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    Meine IGA-Anwendung
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        

## Aufgabe 2: Benutzerzertifizierung mit benutzerdefinierten Prüfern

Mit der Benutzerzertifizierung können Manager den Mitarbeiterzugriff auf Rollen, Accounts und Berechtigungen zertifizieren. In der Regel prüft jeder Manager in einer Organisation die Zugriffsberechtigungen der Personen, die diesem Manager direkt unterstellt sind. Alternativ können benutzerdefinierte Prüfer für Benutzerzertifizierungen angegeben werden, indem Zertifizierungsregeln in der Tabelle **`CERT_CUSTOM_ACCESS_REVIEWERS`** in der Oracle Identity Manager-Datenbank definiert werden.

In der Umgebung SecureOracle enthält "Meine IGA-Anwendung" eine benutzerdefinierte UI zur Verwaltung der Tabelle **`CERT_CUSTOM_ACCESS_REVIEWERS`**, sodass Administratoren benutzerdefinierte Prüfer einfach definieren können.

1.  Melden Sie sich bei "Meine IGA-Anwendung" an, um Kundenprüfer zu definieren.
    
    Beispiel: Verwenden Sie den folgenden Link und die folgenden Zugangsdaten:
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        
2.  Klicken Sie im Randleistenmenü auf die Option **Benutzerdefinierte Prüfer**, um die Seite "Benutzerdefinierte Prüfer" zu öffnen. Fahren Sie fort, um bei Bedarf benutzerdefinierte Prüfer und Benutzer zu definieren.
    
    Beispiel: Zur Vereinfachung wurde bereits ein benutzerdefinierter Prüfer zusammen mit zwei Benutzern definiert, die zertifiziert werden.
    
        	MAP NAME         REVIEWER LOGIN   USER LOGIN   ACCESS TYPE
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        
    
    **Hinweis:** Sie können gerne weitere Prüfer und Benutzer hinzufügen. OIM generiert eine Zertifizierungsaufgabe für jeden **Zuordnungsnamen**. Weitere Informationen zu Regeln und Definitionen finden Sie in der offiziellen Dokumentation unter [Benutzerdefinierter Prüfer für Benutzerzertifizierungen](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A).
    
    ![Meine benutzerdefinierten Prüfer der IGA-Anwendung](./images/img-custom-reviewer.png " ")
    
    Abbildung 1. Benutzerdefinierte Prüfer
    
3.  Melden Sie sich von "Meine IGA-Anwendung" ab.
    
4.  Konfigurieren Sie die Zertifizierungsoptionen, indem Sie sich als Administrator bei OIM Self Service anmelden.
    
    Beispiel: Verwenden Sie den folgenden Link und die folgenden Zugangsdaten:
    
        	URL         http://secureoracle.oracledemo.com:14000/identity
        	User        xelsysadm
        	Password    Oracle123
        
5.  Gehen Sie zu **`Compliance -> Identity Certification -> Certification Configuration`**, und validieren Sie die Konfiguration wie folgt.
    
    Beispiel: Verwenden Sie die folgende Konfiguration als Referenz:
    
        Password required on sign-off                : [checked]
        Allow comments on certify operations         : [unchecked]
        Allow comments on all non-certify operations : [checked]
        Verify employee access                       : [checked]
        Prevent self certification                   : [checked]
        	                                               Alternate Reviewer : User Manager
        
        User and Account Selections         : Include any user with active accounts
        Allow advanced delegation           : [unchecked]
        Allow multi-phased review           : [unchecked]
        Allow reassignment                  : [unchecked]
        Allow auto-claim                    : [checked]
        Perform closed loop remediation     : [checked]
        
        Enable Interactive Excel            : [unchecked]
        Enable Certification Reports        : [checked]
        
6.  Klicken Sie auf die Schaltfläche **Verbindung testen**, um die Kommunikation mit der BI Publisher-Komponente zu testen. Wenn Sie Änderungen vornehmen, speichern Sie die Änderungen. Andernfalls klicken Sie auf **Abbrechen**, um die Seite zu schließen.
    
7.  Fahren Sie mit der Erstellung einer Zertifizierungsdefinition fort. Zur Vereinfachung haben wir bereits eine Zertifizierungsdefinition erstellt. Als Referenz enthält die folgende Tabelle die in der Definition verwendeten Parameter.
    
    Beispiel: Parameter in der Zertifizierungsdefinition:
    
        Name              : 2019 Q4 Review
        Type              : User
        Description       : Custom Reviewers Certification
        
        In Base Selection
        Only Users from Select Organization : Oracle Users
        	                                      Finance
        	                                      Sales
        Constrains                          : Users with Any Level of Risk
        
        In Content Selection
        Include users with no accounts      : [checked]
        
        Limit the role-assignments to certify for each user : All Roles
        Include accounts with no certifiable attributes     : [checked]
        
        Limit the application-instance-assignments to certify for each user : All Application Instances
        Limit the entitlement-assignments to certify for each user          : All Entitlements
        
        In Configuration
        Accept the defaults settings
        
        In Reviewers
        Reviewer                        : Custom Access Reviewer
        Custom Access Reviewer Map Name : 2019 Q4 Review
        
        In Incremental
        Accept the defaults settings
        
        In Summary, review results
        Name              : 2019 Q4 Review
        Description       : Custom Reviewers Certification
        Type              : User
        Reviewer          : Custom Access Reviewer
        Incremental       : No
        Base Selection    : 3 Organizations Selected
        
        	Users with Any Level of Risk
        Content Selection : All Roles
        	                    All Application Instances
        	                    All Entitlements
        
    
    **Hinweis**: Beachten Sie den Abschnitt **Prüfer**, in dem der Zuordnungsname **`2019 Q4 Review`** angegeben wurde, der in der Tabelle **`CERT_CUSTOM_ACCESS_REVIEWERS`** definiert ist. Nachdem eine Zertifizierungsdefinition erstellt wurde, bietet OIM die Möglichkeit, automatisch einen Job für die neue Zertifizierung zu erstellen. Im obigen Beispiel hat OIM den Job **`Cert_2019 Q4 Review`** erstellt.
    
8.  Fahren Sie mit der Ausführung der Zertifizierung fort, indem Sie sich bei OIM Self Service als Benutzer **XELSYSADM** anmelden. Gehen Sie zu **`Compliance -> Identity Certification -> Definitions`**.
    
9.  Wählen Sie die Zertifizierung **`2019 Q4 Review`** aus, und klicken Sie auf die Schaltfläche **Jetzt ausführen**.
    
10.  OIM startet die Zertifizierungsaufgabe und benachrichtigt den Prüfer in diesem Fall den Benutzer **MGRAFF**. Sie können sich beim E-Mail-Webclient (Roundcube) anmelden, um die E-Mail-Benachrichtigung zu prüfen.
    
    Beispiel: Verwenden Sie den folgenden Link und die Zugangsdaten
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        mgraff
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    ![E-Mail-Benachrichtigung über Roundcube-Prüfung](./images/img-cert-email.png " ")
    
    Abbildung 2. Zertifizierungs-E-Mail
    
11.  Fahren Sie mit der Anmeldung bei OIM-Selfservice als Benutzer **MGRAFF** fort. Gehen Sie zu **`Self Service -> Certifications`**. Auf der Seite **Ausstehende Zertifizierungen** wird die neue Zertifizierung aufgeführt. Klicken Sie auf den Linknamen **`2019 Q4 Review [Molly Graff]`**, um die Benutzer zu prüfen und zu zertifizieren.
    
    ![OIM Fenster "Selfservice - Anstehende Zertifizierungen"](./images/user-certifications.png " ")
    
    Abbildung 3. Benutzerzertifizierung
    
12.  Nachdem alle Benutzer standardmäßig zertifiziert wurden, fordert OIM den Prüfer **MGRAFF** auf, seine Zugangsdaten einzugeben, um die Zertifizierungsaufgabe zu genehmigen.
    
13.  Fahren Sie mit der Abmeldung von OIM Self Service fort.
    

## **Anhang**: Benutzerdefinierte Prüfer

Sie können Ihren eigenen benutzerdefinierten Zugriffsprüfer für Benutzerzertifizierungen definieren, indem Sie Zertifizierungsregeln für bestimmte Benutzeraccounts, Rollen, Berechtigungen oder Anwendungsinstanzen oder eine Kombination dieser Entitäten mit einem bestimmten Prüfer angeben. Darüber hinaus können Sie die Zertifizierungsregeln gruppieren und der Zertifizierungsregel einen Zuordnungsnamen zuweisen, um einen Prüfer für diesen Zuordnungsnamen anzugeben.

Diese Regeln können in der Tabelle **`CERT_CUSTOM_ACCESS_REVIEWERS`** in der Oracle Identity Manager-Datenbank definiert werden.

Die folgenden Bedingungen müssen erfüllt sein, damit der benutzerdefinierte Zugriffsprüfer für die Benutzerzertifizierungsfunktion verwendet werden kann:

*   Die Prüfertabelle unterstützt keine Platzhalter für Felder/Spalten.
*   Für die Tabelle "Prüfer" sind Zuordnungen für jeden Benutzer definiert, der in die Zertifizierung aufgenommen werden soll.
*   Informationen zur Anwendungsinstanz sind für alle Konto- und Anspruchszuordnungen erforderlich.
*   Pro Zuordnungsname ist nur eine Instanz der Zuordnung von Standardprüfer und alternativem Prüfer zulässig.

In der Umgebung SecureOracle enthält "Meine IGA-Anwendung" eine benutzerdefinierte UI zur Verwaltung der Tabelle **`CERT_CUSTOM_ACCESS_REVIEWERS`**, sodass Administratoren benutzerdefinierte Prüfer einfach definieren können. Diese Option ist im Menü "Meine IGA-Anwendung" unter der Option **Kundenprüfer** verfügbar.

Weitere Details finden Sie in der offiziellen Dokumentation unter [Benutzerdefinierter Prüfer für Benutzerzertifizierungen](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A).

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Weitere Informationen

Über diese Links erhalten Sie weitere Informationen zu Oracle Identity and Access Management:

*   [Oracle Identity Management-Website](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Governance-Dokumentation](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Oracle Access Management-Dokumentation](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Danksagungen

*   **Autor** - Ricardo Gutierrez, Solution Engineering - Sicherheit und Management
*   **Mitwirkende** - Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Sahaana Manavalan, LiveLabs Developer, NA Technology, März 2022