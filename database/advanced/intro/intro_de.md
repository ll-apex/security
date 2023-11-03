# Einführung

## Über diesen Workshop

### Überblick

_Geschätzte Zeit für den Abschluss des Workshops_: 425 Minuten

Dieser Workshop ist der ZWEITE TEIL der praktischen Übungen zu den Oracle Database Security-Features und -Funktionalitäten. Informationen zum ersten Workshop finden Sie unter _DB Security Basics_.

Basierend auf einer OCI-Architektur, die in wenigen Minuten mit einer einfachen Internetverbindung bereitgestellt wird, können Sie DB Security-Anwendungsfälle in einer vollständigen Umgebung testen, die bereits vom Oracle Database Security Product Manager-Team vorkonfiguriert ist.

Jetzt benötigen Sie weder wichtige Ressourcen auf Ihrem PC (Speicher, CPU oder Speicher) noch komplexe Tools, die Sie beherrschen können. So können Sie alle neuen DB-Sicherheitsfunktionen zu Ihrem Rhythmus entdecken.

Vorschau von "_Livelabs - Database Security Advanced (Mai 2022)_" ansehen[](youtube:h4gXFpOxWZU)

### Komponenten

Die vollständige Architektur der **Praktischen Übungen zur DB-Sicherheit (v5.3 - September 2023)** lautet wie folgt:

![DBSec LiveLabs Archi](./images/dbseclab-archi.png "DBSec LiveLabs Archi")

Es besteht aus 5 VMs:

*   **DBSec-Lab VM** (obligatorisch für alle Workshops: Baseline- und Advanced-Workshops)
*   **Audit Vault Server VM** (nur für Advanced Workshop)
*   **DB Firewall Server VM** (nur für Advanced Workshop)
*   **Key Vault Server VM** (nur für Advanced-Workshop)
*   **DB23c VM** (nur für SQL Firewall-Workshop)

In diesem 2. Workshop verwenden Sie verschiedene Ressourcen, um mit diesen VMs zu interagieren:

*   SSH-Terminalclient
*   Glassfish HR-App
*   Oracle Enterprise Manager 13 c
*   Oracle SQL Developer
*   Oracle Golden Gate Web Console (nur für AVDF-Übungen)
*   Oracle AVDF Web Console (nur für AVDF-Übungen)
*   Oracle Key Vault-Webkonsole (nur für OKV-Übungen)

Damit Ihre Erfahrung mit diesem Workshop optimal ist, sollten Sie "Übung: _Umgebung initialisieren_" NICHT VERGESSEN, um sicherzustellen, dass alle diese Ressourcen korrekt eingestellt sind!

### Ziele

In diesen praktischen Übungen erfahren Sie, wie Sie die DB-Sicherheitsfunktionen konfigurieren, um ihre Datenbanken von der Baseline bis zur Maximum Security Architecture (MSA) zu schützen und zu sichern.

In diesem zweiten Teil werden die erweiterten Produkte und Lösungen von Oracle Database Security wie folgt angezeigt:

*   Oracle Advanced Security-Option
    *   **Oracle Transparent Data Encryption** (TDE)
    *   **Oracle-Datenverdeckung**
*   **Oracle Database Vault** (DV)
*   **Oracle Label Security** (OLS)
*   **Oracle Data Masking and Subsetting** (DMS)
*   **Oracle Audit Vault and Database Firewall** (AVDF)
*   **Oracle Key Vault** (OKV)

Das gesamte DB Security PMs Team wünscht Ihnen einen ausgezeichneten Workshop!

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - September 2023