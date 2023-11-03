# Legacy-Anwendung mit Oracle Database Vault in Oracle Autonomous Database sichern

## Einführung

Neben den Funktionen zur Selbstverwaltung lässt sich Oracle Autonomous Database auch in Oracle Database Vault integrieren. Oracle Database Vault ist ein Tool zur Verbesserung der Sicherheit, das eine zusätzliche Sicherheitsebene bietet, indem der Zugriff auf sensible Daten eingeschränkt und verhindert wird, dass nicht autorisierte Benutzer darauf zugreifen oder diese bearbeiten. Durch die Kombination der selbstverwaltenden Funktionen von Oracle Autonomous Database mit den Sicherheitsfunktionen von Oracle Database Vault können Unternehmen sowohl von verbesserter Performance als auch von verbesserter Sicherheit profitieren.

Eines der Hauptfeatures von Oracle Autonomous Database ist die Fähigkeit, sich automatisch für optimale Performance zu optimieren. Dies wird durch den Einsatz von Algorithmen für maschinelles Lernen erreicht, die Datenbank-Workloads kontinuierlich analysieren und die Datenbankkonfiguration in Echtzeit anpassen. Auf diese Weise kann Oracle Autonomous Database eine konstant hohe Performance bereitstellen, selbst wenn sich Workloads und Datenmengen im Laufe der Zeit ändern.

Insgesamt sind Oracle Autonomous Database und Oracle Database Vault wertvolle Tools für Unternehmen, die ihre Datenbankperformance optimieren und die Sicherheit verbessern möchten. Durch die Kombination dieser beiden Technologien können Unternehmen die selbstverwaltenden Funktionen von Oracle Autonomous Database nutzen und gleichzeitig sensible Daten mit den Sicherheitsfunktionen von Oracle Database Vault schützen.

![Laborarchitektur](images/intro-architecture.png)

Geschätzte Zeit: 90 Minuten

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   Verbinden Sie sich mit der Legacy-HR-Anwendung Glassfish
*   Autonomous Database-Instanz konfigurieren
*   Laden und Überprüfen der Daten in der Glassfish-Anwendung
*   Database Vault aktivieren und HR-Anwendung prüfen
*   Verbindungen zum EMPLOYEESEARCH\_PROD-Schema identifizieren
*   Funktionen der Glassfish HR-Anwendung mit aktiviertem Database Vault kennenlernen

### Voraussetzungen

Dieser Workshop geht davon aus, dass Sie:

*   Ein Oracle Cloud Infratructure Tanancy-Account
*   Vertrautheit mit der Datenbank ist erwünscht
*   Ein gewisses Verständnis von Cloud- und Datenbankbegriffen ist hilfreich
*   Die Vertrautheit mit Oracle Cloud Infrastructure (OCI) ist hilfreich
*   Ein grundlegendes Verständnis von Datenschutz und Sicherheit ist ein Plus
*   Einige Kenntnisse mit Linux/Bash-Befehlen sind hilfreich

_Hinweis: Wenn Sie in diesem Workshop Schwierigkeiten haben, Ihre Ressourcen in Oracle Cloud zu finden, stellen Sie sicher, dass Compartment und Region dem Ort entsprechen, an dem Sie die Ressource erstellt haben._

## Möchten Sie mehr über Oracle Database Vault erfahren?

*   [Oracle Database Vault-Landingpage](https://www.oracle.com/security/database-security/database-vault/)
*   [Einführung in Oracle Database Vault](https://docs.oracle.com/database/121/DVADM/dvintro.htm#DVADM001)
*   [Zusätzlicher Database Vault LiveLab](https://apexapps.oracle.com/pls/apex/r/dbpm/livelabs/view-workshop?wid=682&clear=RR,180&session=100352880546347)

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022