# Einführung

## Über diesen Workshop

In den Übungen zum Workshop wird die aktuelle Version von Oracle Identity Cloud Service (IDCS) demonstriert.

Identity Cloud Service ist die umfassende Sicherheits- und Identitätsplattform der nächsten Generation von Oracle, die einen innovativen, vollständig integrierten Service bietet, der über eine Cloud-Plattform "as a Service" alle wichtigen Identitäts- und Zugriffsverwaltungsfunktionen bereitstellt. Das Design von Identity Cloud Service (IDCS) basiert auf einer Microservicearchitektur, die natürlich auf die Cloud-Prinzipien Skalierbarkeit, Elastizität, Resilienz, Bereitstellungsfreundlichkeit, funktionale Agilität, technische Einführung und Organisationsausrichtung abgestimmt ist.

Auf hoher Ebene bietet Oracle Identity Cloud Service die folgenden Funktionen:

*   Identity and Access Management
*   Integration mit lokalen Active Directory- oder 3rd-Party-Identitätssystemen
*   Single Sign-On (SSO)
*   Benutzerauthentifizierungsservice
*   Identity Federation Service (SAML)
*   OAuth-Services
*   Auditing- und Reportingservices

Geschätzte Laborzeit: 5 Stunden

## IDCS-Architektur

Oracle Identity Cloud Service ist ein neuer Plattformservice, der als Teil des umfassenden PaaS-Servicesportfolios von Oracle verfügbar ist. IDCS wird als eine Reihe von Microservices implementiert, und die Designphilosophie ist API-First mit Unterstützung für moderne Standards wie SCIM, OAUTH, SAML und OpenID Connect.

Das folgende Diagramm veranschaulicht die IDCS-Architektur, die als eine Reihe von Serviceebenen organisiert ist.

![Bild](images/W001.png)

## Laborkonfiguration und Details

Dieser Workshop wird in der Oracle Public Cloud (OPC) veranstaltet. Es umfasst eine Kombination aus Cloud-Services, gehosteter On-Premises-Software und 3rd-Party-Software. Neben Identity Cloud Service sind die restlichen Komponenten enthalten, um die Integration zu demonstrieren, einschließlich Identitätssynchronisierung, Föderation, Authentifizierung, SSO und mehr.

Im Folgenden werden die Komponenten in unserer Werkstatt zusammengefasst:

*   Oracle Cloud Infrastructure (OCI)
*   Oracle Identity Cloud Service (IDS)

SaaS-Anwendungen von Drittanbietern

*   Salesforce - wird für die IDCS-Anwendungsintegration verwendet
*   Google - für IDCS Federation verwendet
*   Okta - wird für IDCS Federation verwendet

Fremdfirmenkomponenten

*   Postman - für die Benutzerverwaltung der IDCS-REST-API

## Konzepte und Begriffe

Dieser Abschnitt enthält einen kurzen Überblick über einige der Konzepte und Begriffe, die während des gesamten Workshops verwendet werden.

*   Oracle Public Cloud (OPC) - OPC beschreibt das gesamte Cloud-Portfolio von Oracle, das aus dem branchenweit umfassendsten und umfassendsten Portfolio an Cloud-Services SaaS, PaaS und IaaS besteht. OPC-Services existieren weltweit über Dutzende von globalen Rechenzentren.
    
*   SaaS / PaaS / IaaS - Dies sind die drei Servicekategorien von Oracle Cloud-Services. SaaS steht für Software-as-a-Service und im Allgemeinen für Cloud-Services wie Oracle Customer Experience (CX), Salesforce.com, WorkDay, Office365 usw. PaaS steht für Platform-as-a-Service und bezieht sich auf Middleware-Services wie Java, Datenbank, Integration, Identität usw. IaaS steht für Infrastructure-as-a-Service und stellt Services wie Compute, Netzwerk und Speicher dar. Zu den Top-Anbietern für IaaS gehören Microsoft (Azure), Amazon (AWS) und Oracle.
    
*   Mandant - Ein Mandant stellt ein Abonnement für einen Cloud-Service dar. Viele der Cloud-Services von Oracle (einschließlich Identity Cloud Service) sind mehrmandantenfähig. Das bedeutet, dass mehrere Kunden (z.B. Abteilungen, Unternehmen, Organisationen, Agenturen usw.) einen gemeinsamen Cloud-Service abonnieren und von diesem in OPC betrieben werden. In einem Multi-Tenant-Szenario verfügt jeder Mandant über eigene Daten, Konfigurationseinstellungen, Benutzer und andere servicebezogene Artefakte.
    
*   OAUTH 2.0 - OAUTH ist ein Standardprotokoll zur Delegierung der Autorisierung. Eine Entität kann so autorisiert werden, auf Ressourcen (Services, API, Daten) zuzugreifen, die in einem Remoteprovider gespeichert sind. Beispiel: Eine On-Premise-Anwendung kann die IDCS-REST-API verwenden, um Informationen zu Benutzern oder Gruppen aus IDCS zur Verwendung in ihrer Anwendung abzurufen. Die REST-API wird durch den OAUTH-Service geschützt, um sicherzustellen, dass die Clientanwendung für einen bestimmten Geltungsbereich der REST-API registriert und autorisiert ist. Daher verhindert OAUTH, dass internetsichtbare Services (wie IDCS-REST-API) von einem nicht autorisierten Benutzer oder einer nicht autorisierten Anwendung genutzt werden.
    
*   SAML 2.0 - SAML steht für Security Assertion Mark-up Language. Es ist ein Standard für die Föderation der Benutzerauthentifizierung. SAML definiert zwei teilnehmende Entitys, den Serviceprovider und den Identitätsprovider. Wenn ein Benutzer versucht, auf eine Anwendung oder einen Service (im Serviceprovider) zuzugreifen, die für SAML konfiguriert ist, anstatt den Benutzer über lokale ID/PW anzumelden, bewirkt der SP, dass die Benutzerauthentifizierung vom Identitätsprovider ausgeführt wird. Daher besteht ein Vertrauensverhältnis zwischen SP und IDP. Oracle IDCS kann als Serviceprovider und/oder Identitätsprovider konfiguriert werden. Als Serviceprovider ermöglicht IDCS die Selfservice-Profilverwaltung, das Zurücksetzen von Kennwörtern usw.
    
*   Identitätsprovider - Dieser Providertyp, auch Identity Assertion-Provider genannt, stellt IDs für Benutzer bereit, die mit Oracle Identity Cloud Service über eine Website außerhalb von Oracle Identity Cloud Service interagieren möchten.
    

## Vorteile der Oracle-Lösung

In diesem Abschnitt werden einige der wirtschaftlichen, geschäftlichen und technischen Vorteile der Oracle Identity Cloud Service-Lösung kurz vorgestellt:

*   **Offen und standardbasiert** - Integrieren Sie Cloud- und On-Premise-Anwendungen schnell mit einer zu 100% offenen und standardbasierten Lösung. Beispiele für unterstützte Standards sind System for Cross-Domain Identity Management (SCIM), Open Authorization (OAUTH 2.0), Security Assertion Markup Language (SAML 2.0), Representational State Transfer (REST), OpenID Connect und andere!
    
*   **Sichere Verteidigung - ausführlich** - Profitieren Sie von Verteidigungsebenen mit dem Identity Cloud Service von Oracle, der als Oracle Public Cloud-(OPC-)Service gehostet und in Ihre On-Premises-Unternehmensfunktionen integriert ist.
    
*   **Hybride Identität** - Verwalten Sie Benutzeridentitäten für Cloud- und On-Premise-Anwendungen mit Hybrid-Deployments der Unternehmensklasse. Es gibt mehrere Optionen für die Integration und den Austausch von Daten zwischen Ihren On-Premise- und IDCS-Umgebungen.
    
*   **Marktführerschaft von Oracle im Bereich Cloud Identity** - Oracle Identity Cloud Service ist nicht der erste Einstieg von Oracle in den Cloud Identity-Markt. In den letzten 4 Jahren haben wir Cloud-Identitätsservices für eine Vielzahl von Oracle Public Cloud-(OPC-)Services bereitgestellt. Angesichts der Tatsache, dass Identitätsservices die Anforderungen von 35.000 Kunden in großem Maßstab mit über 30 Millionen täglichen Anmeldungen erfüllen, erkennen Sie, dass Oracle vor der Einführung von IDCS ein Marktführer im Bereich Cloud-Identität ist!
    
*   **Moderne Architektur** - Während die Idee einiger Anbieter von Cloud-Services darin besteht, ihre On-Premises-Anwendungen einfach in einem "Cloud"-Data Center bereitzustellen, hat Oracle unsere Lösung von Grund auf neu geschrieben. Die Architektur basiert auf API-First und einer Microservices-Architektur, die offene Standards nutzt. Es bietet eine echte Plattform für Identitätsdienste.
    
*   **Servicebreit** - Einige Anbieter der ersten Generation haben Nischenprobleme bei der Bereitstellung von Identity Cloud-Services gelöst (z.B. SSO für SaaS-Anwendungen usw.). Die Realität ist jedoch, dass Unternehmen jeder Größe keine integrierte branchenführende Lösung in der Cloud haben wollen. Die überwiegende Mehrheit wählt einen Servicepartner mit der richtigen Lösung aus, die Unternehmensanforderungen erfüllt. Ebenso wichtig ist es, einen Geschäftspartner zu finden, der sowohl Ihre On-Premises- als auch Ihre Cloud-Identitätsinfrastruktur unterstützen und den Übergang zur Cloud in Ihrem Tempo und in Ihrem Zeitrahmen ermöglichen kann. Dieser Anbieter ist Oracle.
    

## Ziele

Am Ende dieses Workshops werden Sie ein gutes Verständnis von

*   Benutzer- und Gruppenverwaltung
*   Katalogverwendung und SSO-Konfiguration
*   SAML-basierte und Social Federation
*   Bedingte und adaptive MFA

## Voraussetzungen

Im Folgenden werden die externen Komponenten in unserer Werkstatt zusammengefasst:

1.  Google-Account
2.  Salesforce-Entwickleraccount
3.  Okta Integrator-Account
4.  Postman-native Anwendung

### Gmail-Konto erstellen

Um sich für Gmail anzumelden, erstellen Sie ein Google-Konto

1.  Gehen Sie zur [Seite zum Erstellen von Google-Konten](https://accounts.google.com/SignUp)
2.  Befolgen Sie die Schritte auf dem Bildschirm, um Ihr Konto einzurichten.
3.  Verwenden Sie den von Ihnen erstellten Account, um sich bei Gmail anzumelden.

### Salesforce-Entwickleraccount erstellen

Salesforce bietet eine kostenlose Entwickler-Edition. Schritte zum Erstellen eines kostenlosen Entwickleraccounts in salesforce.com

1.  Öffnen Sie einen Browser, und rufen Sie die Anmeldeseite für den [Salesforce-Entwickleraccount](https://developer.salesforce.com/signup) auf.
    
2.  Füllen Sie das Anmeldeformular aus:
    
    *   Geben Sie Ihren Vor- und Nachnamen für _E-Mail_ ein. Geben Sie Ihre E-Mail-Adresse ein (Sie müssen eine Aktivierungs-E-Mail öffnen). HINWEIS: Verwenden Sie den Gmail-Account, den Sie in Schritt 1 erstellt haben.
    *   Geben Sie unter _Benutzername_ einen eindeutigen Benutzernamen in Form einer E-Mail-Adresse an
    *   Aktivieren Sie das Kontrollkästchen "Masterabonnementvertrag", und klicken Sie auf die Schaltfläche _Registrieren_. ![Bild](images/PR01.png)
3.  Überprüfen Sie Ihre E-Mail-Adresse. Sie erhalten eine Aktivierungs-E-Mail für Ihr Developer Edition-Konto.
    
4.  Klicken Sie auf den Link in der Aktivierungs-E-Mail. Geben Sie Ihre neuen Kennwortinformationen ein, und klicken Sie auf _Speichern_.
    

#### Benutzerdefinierte Domain in Salesforce registrieren

Eine benutzerdefinierte Domain oder "Meine Domain" ist eine Möglichkeit, eine eigene benutzerdefinierte Salesforce-Org-URL anstelle einer allgemeinen Salesforce-URL zu verwenden.

Um einige Features in Salesforce zu verwenden, ist "Meine Domain" erforderlich. Zu den Merkmalen gehören

*   _Lightning-Komponenten_ in Lightning-Komponentenregisterkarten, Lightning-Seiten, Lightning-App-Builder oder Lightning-Standalone-Apps anzeigen.
*   Integrieren Sie _Social Sign-On mit Authentifizierungsanbietern_ wie Facebook und Google.
*   _Single Sign-On (SSO)_ mit externen Identitätsprovidern verwenden. In unserem Workshop-Szenario wird dies _Oracle Identity Cloud Service_ sein. Hinweis: Sie müssen "Zu Salesforce Classic wechseln" auswählen.

1.  Navigieren Sie von Ihrer _Salesforce-Homepage_ zu Ihrem Profil (das Symbol "Profil anzeigen" oben rechts).
    
2.  Wählen Sie im Menü _Optionen_ die Option "Zu Salesforce classic wechseln".
    
    ![Bild](images/PR02.png)
    
3.  Navigieren Sie zu _Setup(classic)_ -> _Verwalten_ -> _Domainverwaltung_ -> wählen Sie _Meine Domain_ aus.
    
    ![Bild](images/PR03.png)
    
    ![Bild](images/PR04.png)
    
4.  Geben Sie den Subdomainnamen ein, den Sie in der generischen Salesforce-URL verwenden möchten. Dabei kann es sich um den Accountnamen handeln, der in der Oracle Cloud-Accountregistrierung verwendet wird (zur Identifizierung Ihres Cloud-Accounts).
    
    ![Bild](images/PR05.png)
    
5.  Klicken Sie auf die Schaltfläche "Verfügbarkeit prüfen". (Wenn Ihr Name bereits vergeben ist, wählen Sie einen anderen Namen aus.) Klicken Sie auf die Schaltfläche "Domain registrieren".
    
    ![Bild](images/PR06.png)
    
6.  Salesforce aktualisiert seine Domain-Registrys mit Ihren neuen. Anschließend erhalten Sie eine Bestätigungs-E-Mail. Es kann einige Minuten dauern, bis Ihre Domain verfügbar ist.
    
7.  Nachdem Sie die Aktivierungs-E-Mail erhalten haben, klicken Sie in der E-Mail auf den Link, um zum Anmeldebildschirm mit der benutzerdefinierten Domain-URL zurückzukehren.
    

### Okta Integrator-Account erstellen

1.  Öffnen Sie einen Browser, und rufen Sie die [Okta Integrator-Seite](https://www.okta.com/integrate/signup/) auf.
    
2.  Füllen Sie das Anmeldeformular aus:
    
    *   Geben Sie Ihre _E-Mail_ ein. Verwenden Sie den Gmail-Account, den Sie in Schritt 2 erstellt haben.
    *   Geben Sie _Vorname_ und _Nachname_ ein
    *   Geben Sie den Namen des _Unternehmens_ ein. Dabei kann es sich um den Accountnamen handeln, der bei der Oracle Cloud-Accountregistrierung verwendet wird (zur Identifizierung Ihres Cloud-Accounts).

![Bild](images/PR07.png)

3.  Überprüfen Sie Ihre E-Mail-Adresse. Okta sendet Ihnen eine Bestätigungs-E-Mail. Bitte gehen Sie zu Ihrem Posteingang und überprüfen Sie Ihr Konto unter continue.Click auf _Mein Konto aktivieren_ in der Bestätigungs-E-Mail.
    
    ![Bild](images/PR08.png)
    
4.  Legen Sie ein neues Kennwort fest, und aktivieren Sie Ihren Account.
    

### Postman-native Anwendung installieren

1.  Postman ist als native App für macOS-, Windows- und Linux-Betriebssysteme verfügbar.

Um Postman zu installieren, gehen Sie zur [Anwendungsseite](https://learning.getpostman.com/docs/postman/launching_postman/installation_and_updates/), und klicken Sie je nach Plattform auf _Herunterladen_ für macOS / Windows / Linux.

_Installation von macOS_

Nachdem Sie die App heruntergeladen und entpackt haben, doppelklicken Sie auf Postman. Sie werden aufgefordert, die Datei in den Ordner "Anwendungen" zu verschieben. Klicken Sie auf "In Anwendungsordner verschieben", um sicherzustellen, dass zukünftige Updates korrekt installiert werden können. Die App wird nach der Eingabeaufforderung geöffnet.

![Bild](images/PR09.png)

_Windows-Installation_

Nachdem Sie die Installationsdatei heruntergeladen haben, führen Sie sie aus und befolgen Sie die Anweisungen auf dem Bildschirm.

_Linux-Installation_

Führen Sie für die Installation unter Linux die folgenden Schritte aus:

1.  Zuerst die Datei herunterladen und entpacken
2.  Erstellen Sie dann eine Desktopdatei mit dem Namen Postman.desktop. Erstellen Sie die Datei Postman.desktop im folgenden Verzeichnis:

    ~/.local/share/applications/Postman.desktop
    

Verwenden Sie den Inhalt unten in der obigen Datei:

      [Desktop Entry]
      Encoding=UTF-8
      Name=Postman
      Exec=YOUR_INSTALL_DIR/Postman/app/Postman %U
      Icon=YOUR_INSTALL_DIR/Postman/app/resources/app/assets/icon.png
      Terminal=false
      Type=Application
      Categories=Development;
    

Nachdem die Datei Postman.desktop erstellt wurde, kann die Postman-App mit Anwendungsstartern geöffnet werden. Sie können Ihren Desktop überprüfen und auf das Postman-Symbol doppelklicken.

Sie können mit der ersten Übung fortfahren.

## Weitere Informationen

Diese Arbeitsmappe enthält in erster Linie die erforderlichen Anweisungen und den Kontext, mit denen Sie die Übungen im Oracle Identity Cloud Service-Workshop abschließen können. Wenn Sie weitere Informationen zur Oracle-Lösung wünschen, können Sie sich an Ihr lokales Oracle-Accountteam wenden und/oder einige der folgenden öffentlich verfügbaren Informationen zur Lösung überprüfen.

*   [Identity Cloud Service-Website](https://cloud.oracle.com/en_US/identity)
*   [Lösungsdatenblatt](http://www.oracle.com/technetwork/middleware/id-mgmt/overview/idcs-datasheet-3097388.pdf)
*   [Produktdokumentation](http://docs.oracle.com/cloud/latest/identity-cloud/index.html)
*   [Blogs](https://blogs.oracle.com/imc/)

## Danksagungen

*   **Autor** - SEHub Sicherheits- und Verwaltbarkeitsteam
*   **Zuletzt aktualisiert am/um** - Lucian Ionescu, Principal Solution Engineer, 15.09.2020