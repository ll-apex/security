# Schutz sensibler Daten in REST-GET-Aufrufen mit Oracle Data Redaction

## Einführung

Oracle Data Redaction ist eine erweiterte Sicherheitsfunktion, mit der Sie sensible Daten in Echtzeit maskieren und vor unbefugter Offenlegung schützen können. Dieses Feature ist in Ihrem Autonomous Database-Abonnement enthalten und eignet sich insbesondere für schreibgeschützte Szenarios, wie das Anzeigen sensibler Informationen in Berichten oder das Senden an andere Anwendungen über GET-APIs.

Mit dem Package `DMBS_REDACT PL/SQL` werden Verdeckungs-Policys verwaltet und die spezifischen Spalten und Verdeckungsformate konfiguriert.

In diesem Workshop lernen Sie, wie Sie mit Oracle Data Redaction mit Oracle Rest Data Services (ORDS) Daten in einer GET-Antwort verdecken und den Datenschutz sensibler Daten gewährleisten. Der Prozess umfasst REST, um die Tabelle zu aktivieren, die Sie über ORDS verfügbar machen möchten, Verdeckungs-Policys für bestimmte Spalten und Tabellen zu erstellen und die zu verwendende Verdeckungsfunktion anzugeben. Sie können die Antwort, die Daten enthält, klar gegenüber der Antwort, für die sensible Daten verdeckt sind, gegenüberstellen.

![Laborarchitektur](images/lab-architecture.png)

Geschätzte Workshop-Zeit: 42 Minuten

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Konfigurieren Sie die Autonomous Database-Umgebung.
*   Mit "Verdeckung" können Sie REST-GET-Aufrufe anonymisieren.
*   Setzen Sie Ihre Umgebung zurück.

### Voraussetzungen

In diesem Workshop wird davon ausgegangen, dass Sie:

*   Einen Oracle Cloud Infrastructure-Mandantenaccount
*   Vertrautheit mit der Datenbank ist erwünscht
*   Ein gewisses Verständnis von Cloud- und Datenbankbegriffen ist hilfreich
*   Die Vertrautheit mit Oracle Cloud Infrastructure (OCI) ist hilfreich
*   Grundlegendes Verständnis von RESTFUL-Services ist hilfreich

_Hinweis: Wenn Sie in diesem Workshop Schwierigkeiten haben, Ihre Ressourcen in Oracle Cloud zu finden, stellen Sie sicher, dass Compartment und Region dem Ort entsprechen, an dem Sie die Ressource erstellt haben._

## Möchten Sie mehr über Oracle Data Redaction erfahren?

*   [Einführung in Oracle Data Redaction](https://docs.oracle.com/en/database/oracle/oracle-database/21/asoag/introduction-to-oracle-data-redaction.html#GUID-82EA9712-387C-4D3A-BB72-F64A707C67CA)
*   [Oracle Data Redaction - Häufig gestellte Fragen](https://www.oracle.com/technetwork/database/options/data-masking-subsetting/learnmore/faq-security-asdr-external-3215961.pdf)

## Danksagungen

*   **Autoren** - Alpha Diallo & Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Pedro Lopes, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Alpha Diallo & Ethan Shmargad, Februar 2023