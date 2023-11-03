# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der für die Ausführung dieses Workshops erforderlichen Ressource erforderlich ist.

_Geschätzte Laborzeit:_ 10 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    *   [oracle\_access\_governance\_stack.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/oracle_access_governance_stack.zip)
2.  Speichern Sie in Ihrem Download-Ordner.
    

Wir empfehlen dringend, mit diesem Stack ein eigenständiges/dediziertes VCN mit Ihren Instanzen zu erstellen. Gehen Sie zu _Aufgabe 3_, um unsere Empfehlungen zu befolgen. Wenn Sie ein vorhandenes VCN verwenden möchten, fahren Sie mit der nächsten Aufgabe fort, um das vorhandene VCN mit den erforderlichen Ingress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN/Subnetz zu verwenden, müssen die folgenden Regeln zur Sicherheitsliste hinzugefügt werden.

| Typ | Quellport | Quell-CIDR | Zielport | Protokoll | Beschreibung |
| :-- | :-: | :-: | :-: | :-: | :-- |
| Ingress | Alle | 0.0.0.0/0 | 22 | TCP | SSH |
| Ingress | Alle | 0.0.0.0/0 | 80 | TCP | Remotedesktop mit noVNC |
| Egress | Alle | Nicht zutreffend | 80 | TCP | Ausgehender HTTP-Zugriff |
| Egress | Alle | Nicht zutreffend | (443) | TCP | Ausgehender HTTPS-Zugriff |
| {: title="Liste der erforderlichen Netzwerksicherheitsregeln"} |  |  |  |  |  |

1.  Gehen Sie zu _Networking >> Virtuelle Cloud-Netzwerke_.
2.  Wählen Sie Ihr Netzwerk aus
3.  Wählen Sie unter "Resources" die Option "Security Lists" aus.
4.  Klicken Sie unter der Schaltfläche "Create Security List" auf "Default Security Lists".
5.  Klicken Sie auf die Schaltfläche "Add Ingress Rule".
6.  Geben Sie Folgendes ein:
    *   Quelltyp: CIDR
    *   Quell-CIDR: 0.0.0.0/0
    *   IP-Protokoll: TCP
    *   Quellportbereich: Alle (Standard beibehalten)
    *   Zielportbereich: _Aus obiger Tabelle auswählen_
    *   Beschreibung: _Wählen Sie die entsprechende Beschreibung aus der obigen Tabelle aus_
7.  Klicken Sie auf die Schaltfläche "Add Ingress Rules".
8.  Wiederholen Sie die Schritte \[5-7\], bis für jeden in der Tabelle aufgeführten Port eine Regel erstellt wird

## Aufgabe 3: Compute einrichten

Fahren Sie mit den Details aus den beiden oben genannten Aufgaben mit der Übung _Umgebungssetup_ fort, um Ihre Workshopumgebung mit Oracle Resource Manager (ORM) und einer der folgenden Optionen einzurichten:

*   Stack erstellen: _Compute + Networking_
    
*   Stack erstellen: _Nur Compute_ mit einem vorhandenen VCN, in dem Sicherheitslisten gemäß _Aufgabe 2_ oben aktualisiert wurden
    
    Sie können jetzt **mit der nächsten Übung fortfahren.**
    

## Danksagungen

*   **Autoren** - Rene Fontcha, LiveLabs Platform Lead, NA-Technologie
*   **Mitwirkende** - Meghana Banka
*   **Zuletzt aktualisiert am/um** - Indira Balasundaram, Januar 2023