# Oracle IAM 11.1.2.3 zu 12.2.1.4 In-Place-Upgrade

## Einführung

This section guides you through the steps to upgrade 11.1.2.3 LCM based OIM-OAM integrated environment with OUD as backend Directory Server & OHS as webserver to 12.2.1.4.

_Geschätzte Laborzeit_: 48-72 Stunden

### Oracle IAM-Upgradestrategien

Ein Oracle Identity and Access Management-(IAM-)Deployment besteht aus einer Reihe verschiedener Komponenten:

*   Eine Datenbank
*   Ein LDAP-Verzeichnis zum Speichern von Benutzerinformationen
*   Oracle Access Manager für Authentifizierung
*   Oracle Identity Governance (formell Oracle Identity Manager) für das Provisioning
*   Optional sichern Oracle HTTP Server und Webgate den Zugriff auf Oracle Access Manager und Oracle Identity Governance

Es gibt verschiedene Upgradestrategien, die Sie für ein Upgrade von Oracle Internet Directory, Oracle Unified Directory und Oracle Identity and Access Management verwenden können. Die Strategie, die Sie wählen, hängt hauptsächlich von Ihren Geschäftsanforderungen ab. In dieser Übung wird ein In-Place-Upgrade mit mehreren Hops verwendet, das im Dokument für Upgradestrategien beschrieben wird: [Oracle IAM Upgrade Strategies](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28)

Mit dem In-Place-Upgrade können Sie das vorhandene Deployment übernehmen und es aktualisieren.

*   Wenn Sie ein Upgrade durchführen, sollten Sie in jeder Phase so wenige Änderungen wie möglich vornehmen, um sicherzustellen, dass das Upgrade erfolgreich verläuft.
*   Beispiel: Es wird nicht empfohlen, mehrere Upgradeaktivitäten wie das Upgrade von Oracle Identity and Access Management, das Ändern des Verzeichnisses, das Aktualisieren des Betriebssystems usw. gleichzeitig auszuführen.
*   Wenn Sie ein solches Upgrade durchführen möchten, müssen Sie es schrittweise durchführen. Sie müssen jede Phase validieren, bevor Sie mit der nächsten fortfahren. Der Vorteil dieses Ansatzes ist, dass es Ihnen hilft, genau zu identifizieren, wo das Problem aufgetreten ist, und es zu korrigieren oder rückgängig zu machen, bevor Sie die Übung fortsetzen.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Machen Sie sich mit dem In-Place-Upgradeprozess für Oracle IAM 11.1.2.3 zu Oracle IAM vertraut 12.2.1.4

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein kostenpflichtiger oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
*   Im Rahmen der Schritte vor dem Upgrade müssen Kunden alle 10g-Agents in der oamconsole löschen. Wenn sie diese UA-Bereitschaftsprüfung nicht durchführen, weist dies darauf hin, dass 10g-Agents vorhanden sind. Löschen Sie im Rahmen der Schritte vor dem Upgrade für OAM alle 10g-Agents in der oamconsole. UA-Bereitschaftsprüfung ist nicht erfolgreich, wenn 10g Agents vorhanden sind
*   UA-Bereitschaftsprüfung bei System Components Infrastructure mit Fehler "OHS\_managed\_template.jar" fehlt. Dies kann ignoriert werden, da OHS nicht aktualisiert wird

## **Aufgabe 1**: IAM-Komponenten von 11.1.2.3 auf 12.2.1.3 upgraden

1.  Upgrade von OUD 11.1.2.3 auf OUD 12.2.1.3

*   Führen Sie alle Schritte aus, die in _Abschnitt 6.6_ der Dokumentation "Upgrade von Oracle Unified Directory" - [6.6 Upgrade einer vorhandenen Oracle Unified Directory Server-Instanz ausführen](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.3/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81) beschrieben sind
    
*   Aktualisieren Sie die Keystore-Verschlüsselungsstärke mit den folgenden Schritten, um die Keystore-Verschlüsselungsstärke zu aktualisieren und das Kennwort für das 12c-Upgrade zu ändern. Listen Sie den aktuellen Keystore-Inhalt auf, und geben Sie das Keystore-Kennwort `<copy>keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##</copy>` ein
    
*   • Erstellen Sie einen temporären Ordner mit dem Namen /tmp/keystore, und generieren Sie dann neue Schlüssel mit dem keytool-Befehl `<copy> keytool -genkeypair -keystore /tmp/keystore/default-keystore.jks -keyalg RSA -validity 3600 -alias xell -dname "CN=wsidmhost.idm.oracle.com, OU=Identity, O=Oracle, C=US" -keysize 2048 -storepass IAMUpgrade12c## -keypass IAMUpgrade12c## </copy>`.
    
*   Signaturzertifikat generieren
    
        <copy>
        keytool -certreq -alias xell -file /tmp/keystore/xell.csr -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   Zertifikat exportieren
    
        <copy>
        keytool -export -alias xell -file /tmp/keystore/xlserver.cert -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   Dem Zertifikat vertrauen
    
        <copy>
        keytool -import -trustcacerts -alias xeltrusted -noprompt -file /tmp/keystore/xlserver.cert -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
*   Zertifikat importieren
    
        <copy>
        keytool -importkeystore -srckeystore /tmp/keystore/default-keystore.jks -destkeystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -srcstorepass IAMUpgrade12c## -deststorepass IAMUpgrade12c## -noprompt
        </copy>
        
*   .cert und .csr verschieben
    
        <copy>
        cp /tmp/keystore/x*.* /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/
        </copy>
        
*   Keystore bestätigen
    
        <copy>
        keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
    
    Kennwort für xell in EM in "IAMUpgrade12c##" aktualisieren
    
    *   EM-Konsole öffnen
    *   Navigieren Sie zu "Weblogic Domain" > IAMGovernanceDomain
    *   Klicken Sie mit der rechten Maustaste auf IAMGovernanceDomain.
    *   Sicherheit > Zugangsdaten auswählen
    *   OIM-Eintrag erweitern
    *   Xell-Zeile markieren
    *   Klicken Sie auf "Bearbeiten"
    *   Kennwort aktualisieren
    *   Klicken Sie auf OK.

2.  Upgrade von OIM/OAM von 11.1.2.3 auf OIG/OAM 12.2.1.3
    
    Mit Upgrade Advisor können Sie ein Upgrade der integrierten OAM- und OIM-Umgebung durchführen. Sie müssen sich mit Ihren Anmeldedaten für den Oracle-Support anmelden. Der Link zum MOS-Dokument finden Sie unten:
    
    *   [Upgrade auf 12c (12.2.1.3) Advisor für integrierte OAM-/OIM-Umgebungen](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=318814815527407&id=2342931.2&_adf.ctrl-state=13r3ivrcxc_57)
    *   Navigieren Sie zu _Schritt 4: Konfigurieren_.
    *   Navigieren Sie zu _Upgrading Life Cycle Management Tool Setup Environments_
    *   Führen Sie alle im Abschnitt aufgeführten Schritte aus, um OAM zu aktualisieren
    *   Beachten Sie, dass _Schritt 4_ einmal für OIM und einmal für OAM ausgeführt werden muss

![Oracle Support-Upgrade-Advisor](./images/step2.png " ")

3.  Spielen Sie die im Stack-Patch-Bundle genannten 12.2.1.3-Patches ein: Wenden Sie das Stack-Patch-Bundle für Oracle Identity Management-Produkte an, indem Sie den unten angegebenen MOS-Dokumentlink verwenden:
    *   [Seite "Stack-Patch-Bundle" für OIG](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [SPB für 12.2.1.3 herunterladen und anwenden](https://support.oracle.com/epmos/faces/PatchSearchResults?_adf.ctrl-state=r390fd14k_135&_afrLoop=321341144687003)

## **Aufgabe 2**: IAM-Komponenten von 12.2.1.3 auf 12.2.1.4 upgraden

1.  Führen Sie ein Upgrade von OUD von 12.2.1.3 auf 12.2.1.4 durch. Verwenden Sie dazu die Anweisungen in _Abschnitt 6.4_ der folgenden Dokumentation.
    
    *   [Upgrade von OUD 12.2.1.3 auf 12.2.1.4](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.4/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81)
2.  OAM 12.2.1.3 auf 12.2.1.4 upgraden: Upgrade von OAM mit dem Upgrade Advisor für OAM 12cR2 PS4 (OAM 12.2.1.4.0)
    
    *   [Upgrade auf Oracle Access Manager 12cR2 PS4](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320632596387945&id=2564763.2&_adf.ctrl-state=13r3ivrcxc_167)
    *   Navigieren Sie zu _Schritt 4: Konfigurieren_, und wählen Sie _Upgrade_ aus.
    
    ![Upgrade Advisor für OAM 12cR2 PS4 (OAM 12.2.1.4.0)](./images/step6.png " ")
    
3.  Führen Sie mit dem Upgrade Advisor für OIG 12cR2 PS4 (OAM 12.2.1.4.0) ein Upgrade von OIG 12.2.1.3 auf 12.2.1.4 durch.
    
    *   [Upgrade Advisor für Oracle Identity Governance/Oracle Identity Manager 12cR2 PS4](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320673956019924&id=2667893.2&_adf.ctrl-state=13r3ivrcxc_220)
    *   Navigieren Sie zu _Schritt 4: Konfigurieren/Upgrade_.
    
    ![Upgrade Advisor für OIG 12cR2 PS4 (OAM 12.2.1.4.0)](./images/step7.png " ")
    
4.  Spielen Sie 12.2.1.4-Patches ein, die im Stack-Patch-Bundle erwähnt werden: Wenden Sie das Stack-Patch-Bundle für Oracle Identity Management-Produkte an, indem Sie den folgenden MOS-Dokumentlink verwenden:
    
    *   [Stack-Patch-Bundle für Oracle Identity Management-Produkte einspielen](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [Herunterladen und Anwenden von 12.2.1.4 SPB](https://support.oracle.com/epmos/faces/ui/patch/PatchDetail.jspx?parent=DOCUMENT&sourceId=2657920.1&patchId=32395452)

## **Aufgabe 3**: OIG und OAM mit LDAP-Connector integrieren

Konfigurieren Sie die OIG- und OAM-Integration anhand der Schritt-für-Schritt-Anweisungen in _Abschnitt 2.3_ der folgenden Dokumentation:

*   [Integration von Oracle Identity Governance und Oracle Access Manager konfigurieren](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-9FD153DD-1497-4846-8D39-813B20E29B40)

## **Aufgabe 4**: Übergang von OHS zu 12.2.1.4:

1.  Führen Sie die folgenden Schritte aus, um OHS zu installieren und zu konfigurieren:
    
    *   [OHS vorbereiten und installieren](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wtins/preparing-install-and-configure-product.html#GUID-16F78BFD-4095-45EE-9C3B-DB49AD5CBAAD)
2.  Konfigurieren Sie OAM WebGate: Führen Sie alle 6 Schritte unter _Oracle HTTP Server konfigurieren WebGate_ im folgenden Dokument aus.
    
    *   [WebGate konfigurieren](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wgins/configuring-oracle-http-server-webgate-oracle-access-manager.html#GUID-79326DB8-CCB1-47F6-8CC2-80B6402C13FC)
    *   Wir verwenden dasselbe Agent-Profil wie in 11.1.2.3 - Webgate\_IDM\_11g verwendet.
    *   Kopieren Sie alle Artefakte im Verzeichnis _OAM\_DOMAIN\_HOME/output/OAM\_Webgate\_IDM11g_ in das Verzeichnis _OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_.
    *   Nach dem Kopierbefehl muss das Verzeichnis _OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_ die folgenden Dateien und Verzeichnisse enthalten:
        *   aaa\_cert.pem
        *   aaa\_key.pem
        *   cwallet.sso
        *   ObAccessClient.xml
        *   oblog\_config\_wg.xml
        *   password.xml
        *   Wallet-Verzeichnis mit Datei cwallet.sso
        *   Einfaches Verzeichnis mit aaa\_cert.pem und aaa\_key.pem
    *   Wenn kein einfaches Verzeichnis vorhanden ist, erstellen Sie eines und kopieren Sie die \*.pem-Dateien über
    *   Navigieren Sie zu oamconsole: Konfiguration/Einstellungen/Access Manager-Einstellungen/Webgate Traffic Load Balancer/OAM-Serverport
        *   Ändern Sie den Port in den OAM-Serverport 14100
    *   Navigieren Sie zu Oamconsole: Anwendungssicherheit/Agents/Webgate\_IDM\_11g
        *   Ersetzen Sie die vorhandenen benutzerdefinierten Parameterinhalte durch die folgende Liste:
            *   maxSessionTimeUnits=Minuten
            *   OAMRestEndPointHostName=wsidmhost.idm.oracle.com
            *   client\_request\_retry\_attempts=1
            *   proxySSLHeaderVar=IS\_SSL
            *   inactiveReconfigPeriod=10
            *   OAMRestEndPointPort=14100
            *   URLInUTF8Format=true
            *   OAMServerCommunicationMode=HTTP
3.  Headergröße in OHS mit dieser MOS-Dokumentation erhöhen
    
    *   [So erhöhen Sie die HTTP-Headergröße, um Serverlimitfehler zu verhindern](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=321479834906018&id=819301.1&_adf.ctrl-state=13r3ivrcxc_273)
    *   Fügen Sie der Datei httpd.conf nach der globalen Serveranweisung "KeepAlive" die Datei LimitRequestFieldSize 16380 hinzu
4.  OHS-Server starten
    

## **Aufgabe 5**: Integrierte IAM-Umgebung 12.2.1.4 validieren

1.  Testen Sie OAM und OIG mit den Schritten in _Abschnitt 2.4.7_ in der folgenden Dokumentation
    
    *   [Integration von Access Manager und Oracle Identity Governance funktional testen](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-3803AA41-A882-41C9-B1E8-0BBCBD581CE9)
2.  OAM validieren:
    
    *   Rufen Sie die [OAM-Admin-Serverkonsole](http://wsidmhost.idm.oracle.com:7001/oamconsole) auf, und melden Sie sich als _oamadmin_ an.
        *   Validiert, ob der OAM-Admin-Server ordnungsgemäß funktioniert
    *   Auf [OAM-Serverkonsole](http://wsidmhost.idm.oracle.com:14100/oam/server/logout) zugreifen
        *   Validiert, ob der OAM-Server ausgeführt wird und erfolgreich im HTTP-Port horcht
    *   Auf [OAM-Serverkonsole](http://wsidmhost.idm.oracle.com:7777/oam/server/logout) zugreifen
        *   Validiert, ob die OHS-Proxykonfiguration für den OAM-Server funktioniert, und Webgate funktioniert
    *   Führen Sie `Netstat -a -n | grep 5575` aus.
        *   Es sollte LISTEN zurückgegeben werden
        *   Validiert, ob der OAM-Server auf dem OAP-Port horcht
3.  OIG validieren:
    
    *   Rufen Sie die Oracle Identity Governance-Seiten mit der folgenden URL auf:
        *   [Oracle Identity Self Service](http://wsidmhost.idm.oracle.com:7778/identity)
        *   [Oracle Identity-Systemadministration](http://wsidmhost.idm.oracle.com:7778/sysadmin)
    *   Überprüfen Sie, ob die Links für die Funktionen "Passwort vergessen", "Neues Konto registrieren" und "Benutzerregistrierung verfolgen" auf der Anmeldeseite angezeigt werden und dass sie funktionieren.
    *   Melden Sie sich bei [Oracle Identity Self Service](http://wsidmhost.idm.oracle.com:7778/identity) als _xelsysadm_ an.
    *   Neuen Benutzer erstellen
    *   Melden Sie sich als _xelsysadm_ ab, und melden Sie sich als der in _Schritt 4_ erstellte Benutzer an. - Wenn Sie zur Anmeldung aufgefordert werden, geben Sie gültige Zugangsdaten für den neu erstellten Benutzer an.
    *   Geben Sie ein neues Kennwort ein, und wählen Sie drei geheime Kennwortfragen und -antworten aus. - Der neue Benutzer sollte jetzt angemeldet sein.
    *   Prüfen Sie, ob die Sperr-/Deaktivierungsfunktion funktioniert, indem Sie einen neuen privaten Browser öffnen und sich als _xelsysadm_ anmelden. Sperren oder deaktivieren Sie jetzt das in Schritt 4 erstellte Benutzerkonto.
    *   Nun zurück in der Antwort, wo der neue Benutzer protokolliert wird, versuchen Sie, auf neue Links zu klicken, der Benutzer sollte zurück zur Anmeldeseite geleitet werden.
    *   Prüfen Sie, ob die SSO-Abmeldefunktion funktioniert, indem Sie sich von der Oracle Identity Self Service-Seite abmelden, auf der _xelsysadm_ noch abgemeldet ist. - Nach der Abmeldung von der Seite werden Sie zur SSO-Abmeldeseite umgeleitet.

**Glückwunsch! Sie haben diesen Workshop abgeschlossen!**

## Weitere Informationen

*   Weitere Informationen zur neuesten Version von Oracle IAM finden Sie [hier](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   Weitere Informationen zu den Upgrade-Strategien [hier](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28)

## Danksagungen

*   **Autor** - Anbu Anbarasu, Direktor, Cloud Platform COE
*   **Mitwirkende** - Eric Pollard - Sustaining Engineering, Ajith Puthan - IAM Support, Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Sahaana Manavalan, LiveLabs Developer, NA Technology, Mai 2022