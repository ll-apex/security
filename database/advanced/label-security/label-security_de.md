# Oracle Label Security (OLS)

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen von Oracle Label Security (OLS) vorgestellt. Es gibt dem Benutzer die Möglichkeit zu lernen, wie diese Funktionen zum Schutz seiner sensiblen Daten konfiguriert werden, um die Nachverfolgung von Einwilligungen zu unterstützen und die Einschränkung der Verarbeitung gemäß regulatorischen Anforderungen wie der Datenschutz-Grundverordnung durchzusetzen.

_Geschätzte Laborzeit:_ 30 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Sehen Sie sich eine Vorschau von "_LiveLabs - Oracle Label Security (Mai 2022)_" an[](youtube:7hBsg0ygZt4)

### Ziele

In dieser Übung erfahren Sie, wie Sie mit Oracle Label Security die Einwilligung nachverfolgen und die Einschränkung der Verarbeitung gemäß den Anforderungen der Datenschutz-Grundverordnung durchsetzen können. Um eine ähnliche Funktionalität zu erreichen, könnten verschiedene OLS-Strategien eingesetzt werden. Die hier angegebenen Details dienen lediglich als Beispiel.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Einfache CRM-Anwendung | 10 Minuten |
| 2 | Glassfish-Anwendung schützen | 20 Minuten |

## Aufgabe 1: Einfache CRM-Anwendung

### **Vor Beginn**

Unterschiedliche Anwendungen haben unterschiedliche Zwecke:

*   **Benutzeranwendung** - Anwendung, bei der Benutzer ihre Voreinstellungen für die Zustimmung zum Marketing festlegen, Daten verarbeiten oder vergessen werden müssen - Wird mit Benutzerlabel ausgeführt: `NCNST::DP` und verwendet den Datenbankbenutzer: `APPPREFERENCE`
    
*   **E-Mail-Marketing** - Anwendung, die nur auf Benutzer zugreifen kann, die der Verarbeitung ihrer Daten und insbesondere für das E-Mail-Marketing zugestimmt haben. Wird mit Benutzerlabel ausgeführt: `CONS::EMAIL` und verwendet den Datenbankbenutzer: `APPMKT`
    
*   **Business Intelligence** - Anwendung, die auf alle Benutzer zugreifen kann, die der Verarbeitung ihrer Daten zugestimmt haben - Wird mit Benutzerlabel `CONS::DP` ausgeführt und verwendet den Datenbankbenutzer: `APPBI`
    
*   **Anonymizer** - Batchprozess zum Anonmyisieren von Benutzerdatensätzen und Festlegen des Datenlabels auf `ANON::` - Wird mit Benutzerlabel ausgeführt: `FORGET::` und verwendet den Datenbankbenutzer: `APPFORGET`
    

### **Wie man durch das Labor geht**

*   Während wir Skripte bereitstellen, um die gesamte Übung von Anfang bis Ende automatisiert auszuführen, wird dringend empfohlen, dass Sie einen nach dem anderen öffnen und die Codeblöcke einzeln kopieren/ausführen
*   Auf diese Weise erhalten Sie ein besseres Verständnis der Bausteine dieser Übung
*   Wenn Sie sich für die Skriptausführung entscheiden, können Sie die Details in den Logdateien (.out) jederzeit prüfen.

### **Die Labors**

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/label-security</copy>
        
3.  Zunächst müssen Sie die Label Security-Umgebung einrichten
    
        <copy>./ols_setup_env.sh</copy>
        
    
    ![Lebenslauf](./images/ols-001a.png "Einrichten der Label Security-Umgebung") ![Lebenslauf](./images/ols-001b.png "Einrichten der Label Security-Umgebung")
    
    **Hinweis**:
    
    *   Dieses Skript erstellt den Benutzer `C##OSCAR_OLS`, erstellt eine Tabelle, lädt Daten, erstellt Benutzer, mit denen Differenzszenarios dargestellt werden, konfiguriert und aktiviert OLS
    *   Dieses SQL-Skript ruft das Skript `load_crm_customer_data.sql` auf, um die Tabelle `CRM_CUSTOMER` im Schema `APPCRM` zu erstellen und **391 Zeilen** einzufügen.
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_setup_env.out`")
4.  Als Nächstes erstellen Sie die Label Security Policy. Eine Policy besteht aus Ebenen, Gruppen und/oder Compartments. Die einzige obligatorische Komponente einer Policy ist mindestens eine Ebene
    
        <copy>./ols_create_policy.sh</copy>
        
    
    ![Lebenslauf](./images/ols-002.png "Label Security-Policy erstellen") ![Lebenslauf](./images/ols-002b.png "Label Security-Policy erstellen")
    
    **Hinweis**:
    
    *   Dieses Skript erstellt Policy (Ebenen, Gruppen und Labels), legt Ebenen und Gruppen für Benutzer fest und wendet die Policy auf die Tabelle `APPCRM.CRM_CUSTOMER` an
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_create_policy.out`")
5.  Dann müssen wir die Daten beschriften... Wir verwenden die Policy, die wir erstellt haben, und wenden eine Ebene und optional ein oder mehrere Compartments und optional eine oder mehrere Gruppen an
    
        <copy>./ols_label_data.sh</copy>
        
    
    ![Lebenslauf](./images/ols-003.png "Daten beschriften")
    
    **Hinweis**:
    
    *   Dieses Skript aktualisiert Datenlabels, um verschiedene Labels zu erstellen, die in den Szenarios verwendet werden
    *   In realen Szenarien wäre es ratsam, eine Labeling-Funktion zu erstellen, die Labels basierend auf anderen vorhandenen Tabellendaten (andere Spalten) zuweisen würde.
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_label_data.out`")
6.  Dann sehen wir die Label Security in Aktion
    
        <copy>./ols_label_sec_in_action.sh</copy>
        
    
    ![Lebenslauf](./images/ols-004.png "Siehe Label Security in Aktion")
    
    **Hinweis**:
    
    *   Dieses Skript stellt eine Verbindung her, da verschiedene Apps eine Verbindung herstellen würden
    *   Jede App würde nur Datensätze sehen, die sie verarbeiten könnten
    *   Beispiel: AppMKT (Anwendung, die für E-Mails an Kunden verwendet wird) kann nur Datensätze mit der Bezeichnung `CNST::EMAIL` anzeigen. `AppBI` kann Datensätze mit der Bezeichnung `ANON` und `CNST::ANALYTICS` anzeigen (Zeilen mit der Bezeichnung `CNST` und Teil von Group ANALYTICS - funktionieren auch für `CNST::ANALYTICS,EMAIL`)
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_label_sec_in_action.out`")
7.  Jetzt wird der Status von **UserID(100)** in "vergessen" geändert
    
        <copy>./ols_to_be_forgotten.sh</copy>
        
    
    ![Lebenslauf](./images/ols-005.png "Status der UserID 100 ändern, um vergessen zu werden")
    
    **Hinweis**:
    
    *   Dieses Skript simuliert eine App, die Datensätze verarbeitet, die als vergessen markiert sind
    *   Es wird eine gespeicherte Prozedur erstellt, um Datensätze anzuzeigen, die als "Vergessen" markiert sind (mit `FRGT::` gekennzeichnet)
    *   Außerdem wird eine Prozedur unter einem AppPreference-App-Schema erstellt, die dazu dienen würde, einen bestimmten Kunden zu vergessen.
    *   AppPreference kann auf alle Daten zugreifen, und die Prozedur `forget_me(p_id)` beschriftet eine bestimmte Kunden-ID-Zeile `FRGT::` "verschiebt" einen Datensatz von "Einwilligung" in "vergessen". In unserem Beispiel ändern wir den Status der UserID(100): `forget_me(100)`
    *   Danach überprüfen wir, ob der Status richtig geändert wurde, um vergessen zu werden
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_to_be_forgotten.out`")
8.  Schließlich können wir die Umgebung bereinigen (die OLS-Policy und die Benutzer löschen)
    
        <copy>./ols_clean_env.sh</copy>
        
    
    ![Lebenslauf](./images/ols-006.png "Umwelt reinigen")
    

## Aufgabe 2: Glassfish-Anwendung schützen

1.  Richten Sie zunächst die Glassfish App-Umgebung ein ... und stellen Sie sicher, dass die OLS-Änderungen nicht bereits in der Anwendung bereitgestellt sind
    
        <copy>./ols_setup_glassfish_env.sh</copy>
        
    
    ![Lebenslauf](./images/ols-007.png "HR-Anwendungsumgebung einrichten")
    
2.  Richten Sie als Nächstes die OLS-Richtlinie für Glassfish ein
    
        <copy>./ols_setup_glassfish_policy.sh</copy>
        
    
    ![Lebenslauf](./images/ols-008a.png "OLS-Policy für HR-App einrichten") ![Lebenslauf](./images/ols-008b.png "OLS-Policy für HR-App einrichten")
    
    **Hinweis**:
    
    *   Dieses Skript aktiviert OLS, sodass die DB neu gestartet wird
    *   Then, it creates the OLS policy named `OLS_DEMO_HR_APP` as well as the levels (`PUBLIC`, `CONFIDENTIAL`, `HIGHLY CONFIDENTIAL`), compartments (`HR`, `FIN`, `IP`, `IT`) and the OLS groups (`GLOBAL`, `USA`, `CANADA`, `LATAM`, `EU`, `GERMAN`)
    *   Außerdem werden die verwendeten Datenlabels generiert
    *   Dadurch können wir die Nummern unserer gewünschten `label_tag` zuweisen.
    *   Für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen (Beispiel "`more ols_setup_glassfish_policy.out`")
3.  Erstellen Sie die Umgebung **EMPLOYEESEARCH**
    
        <copy>./ols_config_employeesearch_app.sh</copy>
        
    
    ![Lebenslauf](./images/ols-009.png "EMPLOYEESEARCH-App-Umgebung erstellen")
    
    **Hinweis**:
    
    *   Dieses Skript erstellt eine benutzerdefinierte Tabelle für die Anwendungsbenutzerlabels, `EMPLOYEESEARCH_PROD.DEMO_HR_USER_LABELS`, und füllt sie mit allen Zeilen aus `EMPLOYEESEARCH_PROD.DEMO_HR_USERS` auf
    *   Außerdem erstellt das Skript einige zusätzliche Benutzer, die in dieser Übung verwendet werden, wie `CAN_CANDY`, `EU_EVAN`, und erteilt dann allen Anwendungsbenutzern die entsprechenden OLS-Benutzerlabels.
4.  Öffnen Sie ein Webbrowserfenster für _`http://dbsec-lab:8080/hr_prod_pdb1`_, um auf Ihre Glassfish-App zuzugreifen
    
    **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
    
5.  Melden Sie sich bei der Anwendung als _`can_candy`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Wählen Sie "**Mitarbeiter suchen**", und klicken Sie auf \[**Suchen**\].
    *   Ergebnis vor Aktivierung der OLS-Policy anzeigen
    
    ![Lebenslauf](./images/ols-017.png "Mitarbeiter als CAN_CANDY suchen")
    
6.  Melden Sie sich ab, und melden Sie sich als _`eu_evan`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Wählen Sie "**Mitarbeiter suchen**", und klicken Sie auf \[**Suchen**\].
    *   Sie können alle Mitarbeiterdaten ohne geografische Einschränkung anzeigen
    
    ![Lebenslauf](./images/ols-018.png "Mitarbeiter als EU_EVAN suchen")
    
7.  Kehren Sie zur Terminalsession zurück, und wenden Sie die OLS-Policy auf die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` an
    
        <copy>./ols_apply_policy.sh</copy>
        
    
    ![Lebenslauf](./images/ols-010.png "OLS-Policy anwenden")
    
    **Hinweis**: Nachdem eine OLS-Policy auf eine Tabelle angewendet wurde, können nur Benutzer mit autorisierten Labels oder OLS-Berechtigungen Daten anzeigen.
    
8.  Aktualisieren Sie jetzt die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`, um die Spalte `OLSLABEL` mit dem entsprechenden numerischen OLS-Label zu füllen.
    
        <copy>./ols_set_row_labels.sh</copy>
        
    
    ![Lebenslauf](./images/ols-011.png "Tabelle aktualisieren")
    
    **Hinweis**:
    
    *   Dazu führen Sie den Vorgang basierend auf der Spalte `CITY` in der Tabelle aus.
    *   Beispiel: "`Berlin`" erhält das OLS-Label `P::GER`, weil sie zur Gruppe DEUTSCHLAND gehören
9.  Prüfen Sie, wie die Policy-Ausgabe aussieht (für jeden Schritt können Sie die Ausgabe des ausgeführten Skripts prüfen, Beispiel "`more ols_verify_our_policy.out`":
    
        <copy>./ols_verify_our_policy.sh</copy>
        
    
    ![Lebenslauf](./images/ols-012.png "OLS-Policy prüfen")
    
    ...und gehen Sie durch die Daten, um die verschiedenen Datenlabels und deren Anzeige basierend auf dem "Anwendungsbenutzer" zu demonstrieren, der darauf zugreift:
    
    *   für DB USer und Schemaeigentümer `EMPLOYEESEARCH_PROD`
    
    ![Lebenslauf](./images/ols-013.png "Abfrage als EMPLOYEESEARCH_PROD")
    
    *   für den App-Benutzer `HRADMIN`
    
    ![Lebenslauf](./images/ols-014.png "Als HRADMIN abfragen")
    
    *   für den App-Benutzer `EU_EVAN`
    
    ![Lebenslauf](./images/ols-015.png "Abfrage als EU_EVAN")
    
    *   für den App-Benutzer `CAN_CANDY`
    
    ![Lebenslauf](./images/ols-016.png "Abfrage als CAN_CANDY")
    
10.  Schließlich nehmen wir Änderungen an den Konfigurationsdateien der Glassfish App vor, um die OLS-Policy einzubetten... Dieses Skript führt Sie durch alle Ergänzungen, die wir vornehmen müssen
    
        <copy>./ols_upd_glassfish.sh</copy>
        
    
    ![Lebenslauf](./images/ols-019.png "HR-App einrichten")
    
11.  Gehen Sie zurück zu Ihrer Glassfish-App, und melden Sie sich als _`can_candy`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Wählen Sie "**Mitarbeiter suchen**", und klicken Sie auf \[**Suchen**\].
    *   Jetzt wird ein Unterschied angezeigt, nachdem die OLS-Policy aktiviert wurde: `CAN_CANDY` kann nur **Benutzer mit Kanada** anzeigen!
    
    ![Lebenslauf](./images/ols-020.png "Mitarbeiter als CAN_CANDY suchen")
    
12.  Melden Sie sich ab, und melden Sie sich als _`eu_evan`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Wählen Sie "**Mitarbeiter suchen**", und klicken Sie auf \[**Suchen**\].
    *   Beachten Sie, dass `EU_EVAN` nur **Benutzer mit EU-Kennzeichnung** angezeigt werden kann.
    
    ![Lebenslauf](./images/ols-021.png "Mitarbeiter als EU_EVAN suchen")
    
13.  Melden Sie sich ab, und melden Sie sich als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Wählen Sie "**Mitarbeiter suchen**", und klicken Sie auf \[**Suchen**\].
    *   Beachten Sie, dass `HRADMIN` entsprechend der OLS-Policy weiterhin **alle Benutzer** anzeigen kann.
    
    ![Lebenslauf](./images/ols-022.png "Mitarbeiter als HRADMIN suchen")
    
14.  Nachdem Sie die Übung abgeschlossen haben, können Sie die Policys entfernen und die Glassfish-JSP-Dateien in ihren ursprünglichen Zustand zurückversetzen.
    
        <copy>./ols_restore_glassfish_env.sh</copy>
        
    
    ![Lebenslauf](./images/ols-023a.png "Umgebung wiederherstellen") ![Lebenslauf](./images/ols-023b.png "Umgebung wiederherstellen")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

OLS vergleicht das Zeilenlabel mit den Labelautorisierungen eines Benutzers, damit Sie vertrauliche Informationen einfach auf autorisierte Benutzer beschränken können.

Auf diese Weise können Benutzer mit unterschiedlichen Berechtigungsebenen (z.B. Manager und Vertriebsmitarbeiter) auf bestimmte Datenzeilen in einer Tabelle zugreifen. Sie können OLS-Policys auf eine oder mehrere Anwendungstabellen anwenden. Das Design von OLS ähnelt Oracle Virtual Private Database (VPD). Im Gegensatz zu VPD bietet OLS jedoch sofort die Funktionen zur Zugriffsvermittlung, Data Dictionary-Tabellen und Policy-basierte Architektur, wodurch benutzerdefinierte Codierung eliminiert wird und ein konsistentes label-basiertes Zugriffskontrollmodell bereitgestellt wird, das von mehreren Anwendungen verwendet werden kann.

![Lebenslauf](./images/ols-concept.png "OLS-Konzept")

OLS basiert auf Multi-Level Security-(MLS-)Anforderungen, die in Regierungs- und Verteidigungsorganisationen zu finden sind. OLS-Software wird standardmäßig installiert, aber nicht automatisch aktiviert. Sie können OLS entweder in SQLPlus oder mit dem Oracle Database Configuration Assistant (DBCA) aktivieren. Der Standardadministrator für OLS ist der Benutzer `LBACSYS`. Zur Verwaltung von OLS können Sie entweder eine Gruppe von PL/SQL-Packages und Standalone-Funktionen auf Befehlszeilenebene oder Oracle Enterprise Manager Cloud Control verwenden. Um Informationen zu OLS-Policys zu erhalten, können Sie Data Dictionary Views `ALL_SA_*`, `DBA_SA_*` oder `USER_SA_*` abfragen.

Eine OLS-Policy enthält ein Standardset von Komponenten wie folgt:

*   **Labels**: Labels für Daten und Benutzer steuern zusammen mit Autorisierungen für Benutzer und Programmeinheiten den Zugriff auf bestimmte geschützte Objekte. Labels bestehen aus:
    *   **Ebenen**: Ebenen zeigen den Sensibilitätstyp an, den Sie der Zeile zuweisen möchten (z.B. `SENSITIVE` oder `HIGHLY SENSITIVE`). Ebenen sind erforderlich.
    *   **Compartments (optional)**: Daten können dieselbe Ebene haben (z.B. `PUBLIC`, `CONFIDENTIAL` und `SECRET`), jedoch verschiedenen Projekten in einem Unternehmen angehören (z.B. `ACME Merger` und `IT Security`). Compartments stellen die Projekte in diesem Beispiel dar, mit denen genauere Zugriffskontrollen definiert werden können. Sie werden am häufigsten in Regierungsumgebungen verwendet.
    *   **Gruppen (optional)**: Gruppen identifizieren Organisationen, die Eigentümer der Daten sind oder auf diese zugreifen (Beispiel: `UK`, `US`, `Asia`, `Europe`). Gruppen werden sowohl im kommerziellen als auch im staatlichen Umfeld verwendet und aufgrund ihrer Flexibilität häufig anstelle von Compartments verwendet.
*   **Policy**: Eine Policy ist ein Name, der diesen Labels, Regeln, Autorisierungen und geschützten Tabellen zugewiesen ist.

### **OLS - Vorteile**

OLS bietet mehrere Vorteile zur Kontrolle der Verwaltung von Zeilenebenen:

*   Es ermöglicht die Datenklassifizierung auf Zeilenebene und bietet eine sofort einsatzbereite Zugriffsvermittlung basierend auf der Datenklassifizierung und der Benutzerlabelautorisierung oder Sicherheitsfreigabe.
*   Damit können Sie Datenbankbenutzern und Anwendungsbenutzern Labelautorisierungen oder Sicherheitsfreigaben zuweisen.
*   Es bietet sowohl APIs als auch eine grafische Benutzeroberfläche zum Definieren und Speichern von Datenklassifizierungslabels und Benutzerlabelautorisierungen.
*   Sie ist in Oracle Database Vault und Oracle Advanced Security Data Redaction integriert, sodass Sicherheitsfreigaben sowohl in Database Vault-Befehlsregeln als auch in Data Redaction-Policy-Definitionen verwendet werden können.

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Label Security 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/olsag/part1.html)

Video:

*   _Oracle Label Security - Erläuterungen (April 2020)_[](youtube:o4-XpUQWfaM)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Alan Williams
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023