# Oracle Database Vault (DV)

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen von Oracle Database Vault (DV) vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie diese Funktionen konfiguriert werden, um zu verhindern, dass nicht autorisierte privilegierte Benutzer auf sensible Daten zugreifen.

Geschätzte Zeit: 45 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Sehen Sie sich eine Vorschau von "_LiveLabs - Oracle Database Vault (Mai 2022)_" an[](youtube:M5Kn-acUHRQ)

### Ziele

*   Database Vault im Container und in der integrierbaren Datenbank `PDB1` aktivieren
*   Sensible Daten mit einer Database Vault-Realm schützen
*   Schutz von Serviceaccounts mit vertrauenswürdigem Pfad
*   Database Vault-Kontrollen im Simulationsmodus testen
*   Integrierbare Datenbanken vor Containeradministratoren schützen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur bezahlte Mandanten)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Database Vault aktivieren | 5 Minuten |
| 2 | Einfache Realm erstellen | 10 Minuten |
| 3 | Vertrauenswürdigen Pfad/Multifaktor-Autorisierung erstellen | 10 Minuten |
| 4 | Simulationsmodus | 10 Minuten |
| 5 | Betriebskontrolle | 5 Minuten |
| 6 | Database Vault deaktivieren | <5 Minuten |

## Aufgabe 1: Database Vault aktivieren

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/database-vault</copy>
        
3.  Aktivieren Sie zunächst Database Vault in der Containerdatenbank **cdb1**
    
        <copy>./dv_enable_on_cdb.sh</copy>
        
    
    **Hinweis**: Um DB Vault zu aktivieren, wird die Datenbank neu gestartet.
    
    ![DB-Vault](./images/dv-001.png "DB Vault aktivieren")
    
4.  Aktivieren Sie es als Nächstes in der integrierbaren Datenbank. Aktivieren Sie es jetzt einfach auf **pdb1**
    
        <copy>./dv_enable_on_pdb.sh pdb1</copy>
        
    
    Der Status sollte wie folgt angezeigt werden:
    
    ![DB-Vault](./images/dv-002.png "DB Vault aktivieren")
    
5.  Jetzt ist Database Vault in der Containerdatenbank sowie in pdb1 aktiviert.
    

## Aufgabe 2: Einfache Realm erstellen

1.  Öffnen Sie ein Webbrowserfenster für _`http://dbsec-lab:8080/hr_prod_pdb1`_, um auf Ihre Glassfish-App zuzugreifen
    
    **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
    
    ![DB-Vault](./images/dv-029.png "HR-App - Anmeldung")
    
2.  Melden Sie sich bei der Anwendung als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![DB-Vault](./images/dv-030.png "HR-App - Anmeldung")
    
3.  Klicken Sie auf **Mitarbeiter suchen**
    
    ![DB-Vault](./images/dv-031.png "HR-App - Mitarbeiter suchen")
    
4.  Klicken Sie auf \[**Suchen**\]
    
    ![DB-Vault](./images/dv-032.png "HR-App - Mitarbeiter suchen")
    
5.  Kehren Sie zu Ihrer Terminalsession zurück, und führen Sie den Befehl aus, um die Details zur Glassfish-Session anzuzeigen
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![DB-Vault](./images/dv-003.png "HR App - Mitarbeiterdaten")
    
6.  Erstellen Sie jetzt die **Realm** `PROTECT_EMPLOYEESEARCH_PROD`, um Objekte im Schema `EMPLOYEESEARCH_PROD` vor böswilligen Aktivitäten zu schützen.
    
        <copy>./dv_create_realm.sh</copy>
        
    
    ![DB-Vault](./images/dv-004.png "Erstellen Sie die Realm PROTECT_EMPLOYEESEARCH_PROD.")
    
7.  Objekte zum Schutz der Realm hinzufügen (hier fügen Sie alle Objekte des Schemas hinzu)
    
        <copy>./dv_add_obj_to_realm.sh</copy>
        
    
    ![DB-Vault](./images/dv-005.png "Objekte zum Schutz der Realm hinzufügen")
    
8.  Stellen Sie sicher, dass Sie über einen autorisierten Benutzer in der Realm verfügen. In diesem Schritt fügen wir `EMPLOYEESEARCH_PROD` als autorisierten Realm-Eigentümer hinzu
    
        <copy>./dv_add_auth_to_realm.sh</copy>
        
    
    ![DB-Vault](./images/dv-006.png "Fügen Sie EMPLOYEESEARCH_PROD als autorisierten Realm-Eigentümer hinzu")
    
9.  Führen Sie die SQL-Abfrage erneut aus, um anzuzeigen, dass `SYS` jetzt die Fehlermeldung **unzureichende Berechtigungen** empfängt
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![DB-Vault](./images/dv-007a.png "Jetzt erhält der SYS-Benutzer die Fehlermeldung mit unzureichenden Berechtigungen")
    
10.  Wenn Sie diese Übung abgeschlossen haben, können Sie die Realm löschen.
    
        <copy>./dv_drop_realm.sh</copy>
        
    
    ![DB-Vault](./images/dv-007b.png "Bereich löschen")
    

## Aufgabe 3: Vertrauenswürdigen Pfad/Multifaktor-Autorisierung erstellen

1.  Kehren Sie zur Glassfish-App zurück, und klicken Sie erneut auf \[**Mitarbeiter suchen**\]
    
    ![DB-Vault](./images/dv-031.png "HR-Anwendung - Mitarbeiter suchen")
    
2.  Und klicken Sie auf \[**Suchen**\]
    
    ![DB-Vault](./images/dv-032.png "HR-Anwendung - Suche")
    
3.  Kehren Sie zu Ihrer Terminalsession zurück, und führen Sie diese Abfrage aus, um die Sessioninformationen anzuzeigen, die mit der Glassfish-Anwendung verknüpft sind
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![DB-Vault](./images/dv-019.png "Mit der HR-App verknüpfte Sessioninformationen anzeigen")
    
4.  Fragen Sie jetzt die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` mit dem Eigentümer `EMPLOYEESEARCH_PROD` ab, um zu zeigen, dass darauf zugegriffen werden kann
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![DB-Vault](./images/dv-020.png "Abfragetabellen")
    
5.  Schutz der Anwendungszugangsdaten durch Erstellen einer Database Vault-Regel beginnen
    
        <copy>./dv_create_rule.sh</copy>
        
    
    ![DB-Vault](./images/dv-021.png "Database Vault-Regel erstellen")
    
    **Hinweis**: Wir autorisieren als Trusted Path-App nur den Zugriff von Glassfish Web App (JDBC Thin Client) über den Schemaeigentümer `EMPLOYEESEARCH_PROD`!
    
6.  Die Database Vault-Regel wird verwendet, indem sie einem **DV-Regelset** hinzugefügt wird
    
    *   Sie können eine oder mehrere Regeln im Regelset haben
    *   Wenn mehrere Regeln vorhanden sind, können Sie zwischen dem Regelset wählen, das alle Regeln auswertet, muss "true" sein, oder die Regel `ANY` muss "true" sein
    *   Stellen Sie sich den Unterschied zwischen `IN` und `EXISTS` vor: `IN` enthält alle, während `EXISTS` gestoppt wird, sobald ein Ergebnis gefunden wird, das übereinstimmt
    
        <copy>./dv_create_rule_set.sh</copy>
        
    
    ![DB-Vault](./images/dv-022.png "Database Vault-Regelset erstellen")
    
7.  Erstellen Sie eine Befehlsregel in "**CONNECT**", um den Benutzer `EMPLOYEESEARCH_PROD` zu schützen
    
        <copy>./dv_create_command_rule.sh</copy>
        
    
    ![DB-Vault](./images/dv-023.png "Befehlsregel bei CONNECT erstellen")
    
    **Hinweis**: Sie können "`CONNECT`" nur als `EMPLOYEESEARCH_PROD` verwenden, wenn Sie das erstellte Regelset abgleichen.
    
8.  Kehren Sie zu Ihrer Glassfish-App zurück, aktualisieren Sie einige Male, und führen Sie einige Abfragen aus, indem Sie auf \[**Suchen**\] klicken und Mitarbeiterdaten durchsuchen
    
    **Hinweis**: Da Sie die Glassfish-App als Trusted Path-App verwenden, können Sie auf die Daten zugreifen.
    
9.  Kehren Sie zu Ihrer Terminalsession zurück, und führen Sie unsere Abfrage der Anwendungsnutzung erneut aus, um zu prüfen, ob sie noch funktioniert
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![DB-Vault](./images/dv-024.png "Mitarbeiter suchen")
    
10.  Jetzt versuchen Sie, die Tabelle `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` mit dem Eigentümer `EMPLOYEESEARCH_PROD` abzufragen... **Sie sollten blockiert werden**!
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![DB-Vault](./images/dv-025.png "Abfragetabellen")
    
    **Hinweis**: Da Sie über eine nicht "Vertrauenswürdige Pfad"-App abfragen, können Sie nicht auf die Daten zugreifen.
    
11.  Nachdem Sie die Übung erfolgreich abgeschlossen haben, können Sie die **Befehlsregel**, das **Regelset** und die **Regel** aus Database Vault löschen.
    
        <copy>./dv_del_trusted_path.sh</copy>
        
    
    ![DB-Vault](./images/dv-026.png "Trusted Path löschen")
    

## Aufgabe 4: Simulationsmodus

1.  Fragen Sie zunächst das Simulationsprotokoll ab, um zu zeigen, dass es keine aktuellen Werte enthält
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![DB-Vault](./images/dv-008.png "Simulationsprotokoll abfragen")
    
2.  Erstellen Sie als Nächstes eine Befehlsregel, die das Blockieren aller Verbindungen zur Datenbank simuliert. Dies ist eine einfache Möglichkeit für uns zu identifizieren, wer sich verbindet und von wo aus sie sich verbinden.
    
        <copy>./dv_command_rule_sim_mode.sh</copy>
        
    
    ![DB-Vault](./images/dv-009.png "Befehlsregel erstellen")
    
3.  Führen Sie ein Skript aus, um einige DB-Verbindungen zu erstellen und einige Logeinträge zu generieren
    
        <copy>./dv_run_queries.sh</copy>
        
    
    ![DB-Vault](./images/dv-010.png "Traffic generieren")
    
4.  Nun fragen wir das Simulationsprotokoll erneut ab, um zu sehen, welche neuen Einträge wir haben. Denken Sie daran, dass wir eine Befehlsregel erstellt haben, um blockierende Benutzerverbindungen zu simulieren!
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![DB-Vault](./images/dv-011.png "Simulationsprotokoll abfragen")
    
    Das Log zeigt alle Benutzer an, die sich angemeldet haben und von der Regel blockiert worden wären. Außerdem wird angezeigt, von wo aus sie eine Verbindung hergestellt haben und welchen Client sie verwendet haben, um eine Verbindung herzustellen.
    
5.  Führen Sie dieses Skript aus, um eine Liste der eindeutigen Benutzernamen in den Simulationsprotokollen abzurufen
    
        <copy>./dv_distinct_users_sim_logs.sh</copy>
        
    
    ![DB-Vault](./images/dv-012a.png "Liste der eindeutigen Benutzernamen in den Simulationsprotokollen")
    
6.  Obwohl wir den Simulationsmodus nur für eine **CONNECT**\-Regel verwendet haben, hätten wir dies auf einer Realm verwenden können, um zu zeigen, welche Verletzungen wir gehabt hätten
    
7.  Bevor Sie zur nächsten Übung wechseln, werden die Simulationsprotokolle bereinigt und die Befehlsregel entfernt
    
        <copy>./dv_purge_sim_logs.sh</copy>
        
    
    ![DB-Vault](./images/dv-012b.png "Simulationsprotokolle löschen")
    
        <copy>./dv_drop_command_rule.sh</copy>
        
    
    ![DB-Vault](./images/dv-012c.png "Befehlsregel löschen")
    

## Aufgabe 5: Ops-Steuerung

1.  Status von Database Vault and Operations Control prüfen
    
        <copy>./dv_status.sh</copy>
        
    
    ![DB-Vault](./images/dv-013.png "Prüfen Sie den Database Vault-Status")
    
    **Hinweis**: Er ist noch nicht konfiguriert.
    
2.  Als Nächstes führen wir dieselben Abfragen wie die integrierbaren Datenbanken **pdb1** und **pdb2** aus...
    
    *   ... als `DBA_DEBRA`
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![DB-Vault](./images/dv-014.png "Abfrage als DBA DEBRA")
    
    *   ... als `C##SEC_DBA_SAL`
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![DB-Vault](./images/dv-015.png "Abfrage als DBA SAL")
    
    **Hinweis**:
    
    *   Die Abfrageergebnisse sind identisch
    *   Der allgemeine Benutzer `C##SEC_DBA_SAL` hat Zugriff auf Daten in den integrierbaren Datenbanken, genau wie der PDB-Admin.
3.  Aktivieren Sie Database Vault 19c **Operations Control**, und führen Sie die Abfragen erneut aus
    
    **Hinweis**: Beachten Sie, wer die `EMPLOYEESEARCH_PROD`\-Schemadaten jetzt abfragen kann und wer nicht... `SAL` darf nicht mehr auf Daten zugreifen.
    
        <copy>./dv_enable_ops_control.sh</copy>
        
    
    ![DB-Vault](./images/dv-016a.png "OPS-Steuerung aktivieren")
    
        <copy>./dv_status.sh</copy>
        
    
    ![DB-Vault](./images/dv-016b.png "Prüfen Sie den Database Vault-Status")
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![DB-Vault](./images/dv-017.png "Abfrage als DBA DEBRA")
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![DB-Vault](./images/dv-018a.png "Abfrage als DBA SAL")
    
4.  Wenn Sie diese Übung abgeschlossen haben, deaktivieren Sie Ops Control.
    
        <copy>./dv_disable_ops_control.sh</copy>
        
    
    ![DB-Vault](./images/dv-018b.png "OPS-Steuerelement deaktivieren")
    

## Aufgabe 6: Database Vault deaktivieren

1.  Deaktivieren Sie die integrierbare Datenbank **pdb1**
    
        <copy>./dv_disable_on_pdb.sh pdb1</copy>
        
    
    **Hinweis**: `DV_ENABLE_STATUS` für pdb1 muss **FALSE sein**
    
    ![DB-Vault](./images/dv-027.png "Database Vault deaktivieren")
    
2.  Deaktivieren Sie jetzt Database Vault in der Containerdatenbank **cdb1**
    
        <copy>./dv_disable_on_cdb.sh</copy>
        
    
    ![DB-Vault](./images/dv-028.png "Database Vault deaktivieren")
    
    **Hinweis**:
    
    *   Um DB Vault zu deaktivieren, wird die Datenbank neu gestartet.
    *   `DV_ENABLE_STATUS` für cdb muss **FALSE** sein.
3.  Jetzt ist Database Vault in der Containerdatenbank sowie in pdb1 deaktiviert.
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Oracle Database Vault bietet Kontrollen, um zu verhindern, dass nicht autorisierte privilegierte Benutzer auf sensible Daten zugreifen und nicht autorisierte Datenbankänderungen verhindern.

Die Oracle Database Vault-Sicherheitskontrollen schützen Anwendungsdaten vor unbefugtem Zugriff und erfüllen Datenschutz- und gesetzliche Anforderungen.

![DB-Vault](./images/dv-concept.png "DB-Vault-Konzept")

Sie können Kontrollen bereitstellen, um den Zugriff privilegierter Accounts auf Anwendungsdaten zu blockieren und sensible Vorgänge in der Datenbank mit der Trusted Path-Autorisierung zu kontrollieren.

Durch die Analyse von Berechtigungen und Rollen können Sie die Sicherheit vorhandener Anwendungen erhöhen, indem Sie Best Practices mit geringsten Berechtigungen verwenden.

Oracle Database Vault schützt vorhandene Datenbankumgebungen transparent und macht teure und zeitaufwendige Anwendungsänderungen unnötig.

Mit Oracle Database Vault können Sie eine Gruppe von Komponenten erstellen, um die Sicherheit für Ihre Datenbankinstanz zu verwalten.

Folgende Komponenten sind verfügbar:

*   **Bereiche**

Eine Realm ist eine Schutzzone innerhalb der Datenbank, in der Datenbankschemas, -objekte und -rollen gesichert werden können. Beispiel: Sie können eine Reihe von Schemas, Objekten und Rollen sichern, die sich auf Buchhaltung, Vertrieb oder Personalwesen beziehen. Nachdem Sie diese in einer Realm gesichert haben, können Sie mit der Realm die Verwendung von System- und Objektberechtigungen für bestimmte Accounts oder Rollen kontrollieren. Auf diese Weise können Sie fein granulierte Zugriffskontrollen für alle Benutzer bereitstellen, die diese Schemas, Objekte und Rollen verwenden möchten.

*   **Befehlsregeln**

Eine Befehlsregel ist eine spezielle Sicherheits-Policy, die Sie erstellen können, um zu steuern, wie Benutzer fast jede SQL-Anweisung ausführen können, einschließlich SELECT-, ALTER SYSTEM-, DDL-(Database Definition Language-) und DML-(Data Manipulation Language-)Anweisungen. Befehlsregeln müssen mit Regelsets arbeiten, um zu bestimmen, ob die Anweisung zulässig ist.

*   **Faktoren**

Ein Faktor ist eine benannte Variable oder ein benanntes Attribut, z.B. ein Benutzerspeicherort, eine Datenbank-IP-Adresse oder ein Sessionbenutzer, den Oracle Database Vault als vertrauenswürdigen Pfad erkennen und verwenden kann. Sie können Faktoren in Regeln verwenden, um Aktivitäten wie die Autorisierung von Datenbankaccounts für die Anmeldung bei der Datenbank oder die Ausführung eines bestimmten Datenbankbefehls zu steuern, um die Sichtbarkeit und Verwaltbarkeit von Daten einzuschränken. Jeder Faktor kann eine oder mehrere Identitäten aufweisen. Eine Identität ist der tatsächliche Wert eines Faktors. Ein Faktor kann abhängig von der Factor-Abrufmethode oder seiner Identity Mapping-Logik mehrere Identitäten aufweisen.

*   **Regelsets**

Ein Regelset besteht aus mindestens einer Regel, die Sie mit einer Realm-Autorisierung, Befehlsregel, Faktorzuweisung oder sicheren Anwendungsrolle verknüpfen können. Die Auswertung des Regelsets ergibt "true" oder "false" je nach Auswertung jeder enthaltenen Regel und je nach Auswertungstyp (alle True oder alle True). Die Regel innerhalb eines Regelsets ist ein PL/SQL-Ausdruck, der als wahr oder falsch ausgewertet wird. Sie können dieselbe Regel in mehreren Regelsets verwenden.

*   **Sichere Anwendungsrollen**

Eine sichere Anwendungsrolle ist eine spezielle Oracle Database-Rolle, die basierend auf der Auswertung eines Oracle Database Vault-Regelsets aktiviert werden kann.

Um diese Komponenten zu erweitern, stellt Oracle Database Vault eine Reihe von PL/SQL-Schnittstellen und -Packages bereit. Im Allgemeinen besteht der erste Schritt darin, eine Realm zu erstellen, die aus den Datenbankschemas oder Datenbankobjekten besteht, die Sie sichern möchten. Sie können die Realm weiter sichern, indem Sie Regeln, Befehlsregeln, Faktoren, Identitäten, Regelsets und sichere Anwendungsrollen erstellen. Darüber hinaus können Sie Berichte zu den Aktivitäten ausführen, die diese Komponenten überwachen und schützen.

### **Vorteile der Verwendung von Database Vault**

*   Behebt Compliance-Vorschriften zum Sicherheitsbewusstsein
*   Schützt privilegierte Benutzerkonten vor vielen Sicherheitsverletzungen und Datenstehlen, sowohl extern als auch intern
*   Unterstützt Sie bei der Entwicklung flexibler Sicherheits-Policys für Ihre Datenbank
*   Behebt Bedenken hinsichtlich der Datenbankkonsolidierung und Cloud-Umgebungen, um Kosten zu senken und das Risiko sensibler Anwendungsdaten für diejenigen zu reduzieren, die keinen echten Wissensbedarf haben
*   Funktioniert in einer mehrmandantenfähigen Umgebung und erhöht die Konsolidierungssicherheit

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Database Vault 19 c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

Video:

*   _Oracle Database Vault - Anwendungsfälle (Part1) (Oktober 2019)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault - Anwendungsfälle (Part2) (November 2019)_[](youtube:hh-cX-ubCkY)
*   _Oracle Database Vault (März 2019)_[](youtube:oVidZw7yWIQ)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Richard Evans
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023