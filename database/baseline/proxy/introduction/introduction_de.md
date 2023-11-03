# Einführung

## Über diesen Workshop

In diesem Workshop wird die Funktionalität der Proxyauthentifizierung vorgestellt. Mit der Proxyauthentifizierung kann sich ein Benutzer (der Proxybenutzer) im Namen eines anderen Benutzers (der Zielbenutzer) bei der Datenbank anmelden. In diesem Workshop werden Entwicklern die korrekte Verwendung, Konfiguration und Best Practices bei der Proxyauthentifizierung gezeigt.

_Geschätzte Workshopzeit_: 45 Minuten

### Proxyauthentifizierung

Bei der Proxyauthentifizierung wird eine Middle Tier für die Benutzerauthentifizierung verwendet. Sie können einen Middle Tier-Server zum sicheren Proxy von Clients mit den folgenden drei Formen der Proxyauthentifizierung entwerfen:

*   Der Middle Tier-Server authentifiziert sich beim Datenbankserver und einem Client. In diesem Fall authentifiziert sich ein Anwendungsbenutzer oder eine andere Anwendung beim Middle Tier-Server. Clientidentitäten können bis zur Datenbank verwaltet werden.
    
*   Der Client, also ein Datenbankbenutzer, wird nicht vom Middle Tier-Server authentifiziert. Die Identität des Clients und das Datenbankkennwort werden zur Authentifizierung über den Middle Tier-Server an den Datenbankserver übergeben.
    
*   Der Client, also ein globaler Benutzer, wird vom Middle Tier-Server authentifiziert und übergibt entweder einen Distinguished Name (DN) oder ein Zertifikat über die Middle Tier, um den Benutzernamen des Clients abzurufen.
    

In allen Fällen muss ein Administrator den Middle Tier-Server autorisieren, einen Client als Proxy zu verwenden, d.h. im Namen des Clients zu handeln.

### Ziele

Entwickler können auf die Anwendungsdaten zugreifen, ohne das Kennwort des Anwendungsschemas zu kennen. Der erstellte Entwickler übernimmt die Berechtigungen des Anwendungsschemas.

Das gesamte DB Security PMs Team wünscht Ihnen einen ausgezeichneten Workshop!

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Danksagungen

*   **Autor** - Stephen Stuart & Noah Galloso, Solution Engineers, North America Specialist Hub
*   **Mitwirkende** - Richard C. Evans, Produktmanager für Datenbanksicherheit
*   **Zuletzt aktualisiert am/um** - Stephen Stuart & Noah Galloso, August 2023