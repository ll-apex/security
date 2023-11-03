# Einführung

## Über diesen Workshop

In diesem Workshop wird die Funktionalität der Oracle Transport Layer Security-(TLS-)Netzwerkverschlüsselung vorgestellt. Es gibt dem Benutzer die Möglichkeit zu lernen, wie man diese Funktion konfiguriert, um seine Daten in Bewegung zu verschlüsseln und zu sichern.

_Geschätzte Workshopzeit_: 45 Minuten

### Über TLS

TLS ist der standardbasierte Ansatz zur Verschlüsselung von Daten in Bewegung. Da TLS eine einseitige oder wechselseitige Authentifizierung bereitstellt, minimiert es die Wahrscheinlichkeit einer Verletzung.

### Ziele

*   Ihre Datenbankkommunikation erfolgreich mit 1-Wege-TLS schützen
*   Prüfen Sie vor der Konfiguration von TLS, ob der Netzwerkverkehr unverschlüsselt ist
*   Root-Wallet und selbstsigniertes Root-CA-Zertifikat erstellen
*   Wallet des Datenbankservers erstellen und Zertifikatanforderung erstellen
*   Datenbankzertifikat mit Root-CA-Zertifikat signieren
*   CA-Root-Zertifikat und Datenbankserverzertifikat dem Datenbank-Wallet hinzufügen
*   CA-Root-Zertifikat in Client-Truststore importieren (nur Linux, Windows)
*   Für TLS-Netzwerkverschlüsselung konfigurieren
*   Stellen Sie eine Verbindung mit TLS-Netzwerkverschlüsselung her, und prüfen Sie, ob der Datenverkehr verschlüsselt ist
*   Erstellen Sie einen neuen BS-Benutzer, und verschlüsseln Sie SQL-Traffic.
*   (Optional) Verschlüsselung deaktivieren

Das gesamte DB Security PMs Team wünscht Ihnen einen ausgezeichneten Workshop!

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Danksagungen

*   **Autor** - Stephen Stuart & Alpha Diallo, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Alpha Diallo, April 2023