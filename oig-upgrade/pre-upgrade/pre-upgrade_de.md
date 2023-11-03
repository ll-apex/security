# Anforderungen vor dem Upgrade

## Einführung

Diese Übung führt Sie durch die Aufgaben vor dem Upgrade, die vor dem Upgrade auf Oracle Identity Manager 12c ausgeführt werden müssen, wie Backup, Klonen der aktuellen Umgebung, Analysieren des Pre-Upgrade-Berichts und Prüfen, ob Ihr System zertifizierte Anforderungen erfüllt.

_Geschätzte Laborzeit_: 30 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Nicht-SYSDBA-Benutzer erstellen, um den Upgradeassistenten auszuführen
*   OPSS-Verschlüsselungsschlüssel exportieren
*   Führen Sie den Upgradeassistenten aus, um die Verfügbarkeitsprüfung vor dem Upgrade auszuführen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Nicht-SYSDBA-Benutzer erstellen

Oracle empfiehlt, dass Sie einen Nicht-SYSDBA-Benutzer namens _FMW_ erstellen, um den Upgradeassistenten auszuführen. Dieser Benutzer hat die erforderlichen Berechtigungen zum Ändern von Schemas, hat jedoch keine vollständigen Administratorberechtigungen.

1.  Melden Sie sich bei der Datenbank an, und führen Sie das Skript _fmw.sql_ aus.
    
        <copy>sqlplus / as sysdba</copy>
        
    
        SQL> <copy>@/u01/scripts/FMW.sql</copy>
        
    
        SQL> <copy>exit</copy>
        

## Aufgabe 2: OPSS-Verschlüsselungsschlüssel exportieren und kopieren

Exportieren Sie den OPSS-Verschlüsselungsschlüssel aus Oracle Identity Manager 11g (11.1.2.3) setup.The. Die folgenden Schritte werden ausgeführt, um sicherzustellen, dass die verschlüsselten Daten aus 11g (11.1.2.3) OIG nach dem Upgrade auf 12c (12.2.1.4) korrekt gelesen werden. Die exportierten Schlüssel werden vom Tool oneHopUpgrade benötigt, um den Upgradeprozess abzuschließen.

1.  Navigieren Sie zum Speicherort _<11g\_(11.1.2.3\_ORACLE\_HOME>/oracle\_common/common/bin_
    
        <copy>cd /u01/oracle/middleware11g/oracle_common/common/bin</copy>
        
2.  Starten Sie das Skript _wlst.sh_.
    
        <copy>./wlst.sh</copy>
        
3.  Führen Sie den WLST-Befehl _exportEncryptionKey_ im Offlinemodus aus.
    
        <copy>exportEncryptionKey('/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/jps-config.xml', '/u01/OPSS_EncryptKey', 'Welcom@123')</copy>
        
4.  Ausfahrt WLST
    
        <copy>exit ()</copy>      
        
    
    ![](images/1-wlst.png)
    
5.  Navigieren Sie in das Verzeichnis _/u01/OPSS\_EncryptKey_, und prüfen Sie, ob die exportierten Verschlüsselungsschlüsseldateien erstellt wurden
    
        <copy>cd /u01/OPSS_EncryptKey</copy>
        
    
        <copy>ls -latr</copy>
        
6.  Kopieren Sie den .xldatabasekey aus dem Setupspeicherort 11g (11.1.2.3) in das Verzeichnis _/u01/OPSS\_EncryptKey_.
    
        <copy>cp /u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/.xldatabasekey /u01/OPSS_EncryptKey</copy>
        

## Aufgabe 3: Pre-Upgrade-Bereitschaftsprüfung

1.  Führen Sie den Upgradeassistenten im Bereitschaftsmodus aus, um eine Bereitschaftsprüfung vor dem Upgrade durchzuführen
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin</copy>
        
    
        <copy>./ua -readiness</copy>
        
    
    Der Upgradeassistent wird im Bereitschaftsmodus gestartet:
    
2.  Willkommen - Klicken Sie auf _Weiter_
    
    ![](images/2-ua.png)
    
3.  Bereitschaftsprüfungstyp - _Domainbasiert_. Navigieren Sie zum OIM-Home für 11g: _`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_
    
    ![](images/3-ua.png)
    
4.  Komponentenliste - Klicken Sie auf _Weiter_
    
    ![](images/4-ua.png)
    
5.  OPSS-Schema - Geben Sie die entsprechenden Zugangsdaten ein, und klicken Sie auf _Verbinden_, um die Verbindung zur Datenbank zu testen
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/5-ua.png)
    
6.  MDS-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
    ![](images/6-ua.png)
    
7.  UMS-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
    ![](images/7-ua.png)
    
8.  SOAINFRA-Schema - Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
    ![](images/8-ua.png)
    
9.  OIM-Schema: Derselbe Benutzername und dasselbe Kennwort werden automatisch aktualisiert. Klicken Sie auf _Weiter_
    
    ![](images/9-ua.png)
    
10.  Bereitschaftsübersicht - Klicken Sie auf _Weiter_
    
11.  Klicken Sie auf _Fertigstellen_ und anschließend auf _Schließen_, sobald die Bereitschaftsprüfung abgeschlossen ist.
    
    ![](images/10-ua.png)
    

## Aufgabe 4: Pre-Upgrade-Bericht für Oracle Identity Manager analysieren (optional)

1.  Das Utility für das Pre-Upgrade-Bericht analysiert Ihre vorhandene Oracle Identity Manager-Umgebung und stellt Informationen zu den obligatorischen Voraussetzungen bereit, die Sie vor dem Upgrade ausführen müssen. Es ist wichtig, alle im Pre-Upgrade-Bericht aufgeführten Probleme zu beheben, bevor Sie mit dem Upgrade fortfahren, da das Upgrade möglicherweise nicht erfolgreich verläuft, wenn die Probleme nicht behoben werden. Im Rahmen dieser Übung wurden bereits Musterberichte vor dem Upgrade generiert. Sie können im Verzeichnis _`/u01/Upgrade_Utils/OIM_preupgrade_reports`_ angezeigt und analysiert werden.
    
        <copy>cd /u01/Upgrade_Utils/OIM_preupgrade_reports</copy>
        
2.  Öffnen Sie die Seite _index.html_, und navigieren Sie durch die verschiedenen Berichte, um sie zu analysieren.
    
        <copy>firefox index.html</copy>
        
    
    ![](images/Reports.png)
    

## Aufgabe 5: 11g-Server und -Prozesse stoppen

Bevor Sie den Upgradeassistenten zum Upgrade der Schemas ausführen, müssen Sie alle Prozesse und Server in der 11g-OIG-Domain herunterfahren, einschließlich Administration Server, Node Manager (sofern Sie Node Manager konfiguriert haben) und aller Managed Server.

1.  Führen Sie das Skript _stopDomain11g.sh_ aus, um alle 11g-Server zu stoppen
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./stopDomain11g.sh</copy>
        

Damit sind alle auszuführenden Aufgaben vor dem Upgrade abgeschlossen.

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Juni 2021