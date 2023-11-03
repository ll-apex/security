# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der für die Ausführung dieses Workshops erforderlichen Ressource erforderlich ist. Für diesen Workshop sind eine Compute-Instanz und ein virtuelles Cloud-Netzwerk (VCN) erforderlich.

_Geschätzte Laborzeit:_ 15 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier- oder kostenpflichtige Cloud-Accounts von Oracle
*   SSH-Schlüssel

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    *   [sec-orcl-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/LtODsAADe31TuKS1e7J4qDwqXKZ37C8oiv7W-hFSujfVHh_K0mEAqmHgl9v2XKqf/n/natdsecurity/b/stack/o/sec-orcl-mkplc-freetier.zip)
2.  Speichern Sie in Ihrem Download-Ordner.
    

Wir empfehlen dringend, mit diesem Stack ein eigenständiges/dediziertes VCN mit Ihren Instanzen zu erstellen. Gehen Sie zu _Aufgabe 3_, um unsere Empfehlungen zu befolgen. Wenn Sie ein vorhandenes VCN verwenden möchten, fahren Sie mit der nächsten Aufgabe fort, um das vorhandene VCN mit den erforderlichen Ingress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN/Subnetz zu verwenden, müssen die folgenden Regeln zur Sicherheitsliste hinzugefügt werden.

### **(1) Ingress-Regeln**

| Zustandslos | Quelltyp | Quell-CIDR | IP-Protokoll | Quellportbereich | Zielportbereich | Beschreibung |
| :-- | :-: | :-: | :-: | :-: | :-: | :-- |
| False (nicht aktiviert) | CIDR | 0.0.0.0/0 | TCP | Alle | 22 | SSH |
| False (nicht aktiviert) | CIDR | 0.0.0.0/0 | TCP | Alle | (6080) | Remotedesktop mit noVNC |
| {: title="Liste der erforderlichen Netzwerksicherheitsregeln (Ingress)"} |  |  |  |  |  |  |

1.  Gehen Sie zu _Networking >> Virtuelle Cloud-Netzwerke_.
2.  Wählen Sie Ihr Netzwerk aus
3.  Wählen Sie unter "Resources" die Option "Security Lists" aus.
4.  Klicken Sie unter der Schaltfläche "Create Security List" auf "Default Security Lists".
5.  Klicken Sie auf die Schaltfläche _Ingress-Regeln hinzufügen_.
6.  Erstellen Sie eine Regel für jeden Eintrag in den obigen _Ingress_\-Tabellen.
    *   Zustandslos: Nicht aktiviert lassen (Standard)
    *   Quelltyp: CIDR
    *   Quell-CIDR: 0.0.0.0/0
    *   IP-Protokoll: TCP
    *   Quellportbereich: Alle (Standard beibehalten)
    *   Zielportbereich: _Wählen Sie aus den obigen Tabellen_
    *   Beschreibung: _Wählen Sie die entsprechende Beschreibung aus den obigen Tabellen aus_
7.  Klicken Sie auf _+Another Ingress-Regel_, und wiederholen Sie Schritt \[6\], bis für jeden in den _Ingress_\-Tabellen aufgeführten Port eine Regel erstellt wird.
8.  Klicken Sie auf die Schaltfläche "Add Ingress Rules".

### **(2) Egress-Regeln**

| Zustandslos | Quelltyp | Ziel-CIDR | IP-Protokoll | Quellportbereich | Zielportbereich | Beschreibung |
| :-- | :-: | :-: | :-: | :-: | :-: | :-- |
| False (nicht aktiviert) | CIDR | 0.0.0.0/0 | TCP | Alle | 80 | Ausgehender HTTP-Zugriff |
| False (nicht aktiviert) | CIDR | 0.0.0.0/0 | TCP | Alle | (443) | Ausgehender HTTPS-Zugriff |
| {: title="Liste der erforderlichen Netzwerksicherheitsregeln (Egress)"} |  |  |  |  |  |  |

1.  Wählen Sie im linken Pannel die Option _Egress-Regel_ aus.
2.  Klicken Sie auf die Schaltfläche "Add Egress Rule".
3.  Erstellen Sie eine Regel für jeden Eintrag in den obigen _Egress_\-Tabellen:
    *   Zustandslos: Nicht aktiviert lassen (Standard)
    *   Quelltyp: CIDR
    *   Ziel-CIDR: 0.0.0.0/0
    *   IP-Protokoll: TCP
    *   Quellportbereich: Alle (Standard beibehalten)
    *   Zielportbereich: _Wählen Sie aus den obigen Tabellen_
    *   Beschreibung: _Wählen Sie die entsprechende Beschreibung aus den obigen Tabellen aus_
4.  Klicken Sie auf _+Another Egress-Regel_, und wiederholen Sie Schritt \[3\], bis für jeden in den _Egress_\-Tabellen aufgeführten Port eine Regel erstellt wird.
5.  Klicken Sie auf die Schaltfläche "Add Egress Rules".

## Aufgabe 3: Compute einrichten

Fahren Sie mit den Details aus den beiden oben genannten Aufgaben mit der Übung _Umgebungssetup_ fort, um Ihre Workshopumgebung mit Oracle Resource Manager (ORM) und einer der folgenden Optionen einzurichten:

*   Stack erstellen: _Compute + Networking_
*   Stack erstellen: _Nur Compute_ mit einem vorhandenen VCN, in dem Sicherheitslisten gemäß _Aufgabe 2_ oben aktualisiert wurden

Sie können jetzt mit der nächsten Übung fortfahren.

## Danksagungen

*   **Autor** - Rene Fontcha, LiveLabs Platform Lead, NA-Technologie
*   **Mitwirkende** - Meghana Banka
*   **Zuletzt aktualisiert am/um** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, Dezember 2022