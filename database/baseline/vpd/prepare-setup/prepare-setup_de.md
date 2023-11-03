# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der Ressourcen erforderlich ist, die für die Ausführung dieses Workshops erforderlich sind. Für diesen Workshop sind Compute-Instanzen erforderlich, auf denen die Marketplace-Images von Database Security und ein virtuelles Cloud-Netzwerk (VCN) ausgeführt werden.

Geschätzte Zeit: 15 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    *   [dbsec-mkplc-basics.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/qiz1dOzRGi3FoiEcFkDWMb860cWlt8MVwt2oGd5sU0lxc4aLk9hI3sPGA9_X2ILz/n/natdsecurity/b/stack/o/dbsec-mkplc-vm01-tls.zip)
2.  Speichern Sie in Ihrem Download-Ordner.
    

Wir empfehlen dringend, mit diesem Stack ein eigenständiges/dediziertes VCN mit Ihren Instanzen zu erstellen. Fahren Sie mit _Schritt 3_ fort, um unsere Empfehlungen zu befolgen. Wenn Sie lieber ein vorhandenes VCN verwenden möchten, fahren Sie wie unten angegeben mit dem nächsten Schritt fort, um das vorhandene VCN mit den erforderlichen Egress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN zu verwenden, müssen die folgenden Ports zu Egress-Regeln hinzugefügt werden

| Hafen | Beschreibung |
| :-- | :-- |
| 22 | SSH |
| 80 | Anwendung (HTTP) |
| (6080) | noVNC Remotedesktop |
| {: title="Erforderliche Ports"} |  |

| Hafen | Beschreibung |
| :-- | :-- |
| (443) | Anwendung (HTTPS) |
| (7803) | Oracle Enterprise Manager |
| (8080) | Glassfish-Anwendung |
| (50002) | Glassfish-Anwendung |
| {: title="Optionale Ports - Für Apps-Zugriff außerhalb des Remote-Desktops noVNC"} |  |

1.  Gehen Sie zu _Networking >> Virtuelle Cloud-Netzwerke_.
2.  Wählen Sie Ihr Netzwerk aus
3.  Wählen Sie unter "Resources" die Option "Security Lists" aus.
4.  Klicken Sie unter der Schaltfläche "Create Security List" auf "Default Security Lists".
5.  Klicken Sie auf die Schaltfläche "Add Ingress Rule".
6.  Geben Sie Folgendes ein:
    *   Quell-CIDR: 0.0.0.0/0
    *   Zielportbereich: _Siehe obige Tabelle(n)_
7.  Klicken Sie auf die Schaltfläche "Add Ingress Rules".

## Aufgabe 3: Compute einrichten

Fahren Sie mit den Details aus den beiden oben genannten Schritten mit der Übung _Umgebungssetup_ fort, um Ihre Workshopumgebung mit Oracle Resource Manager (ORM) und einer der folgenden Optionen einzurichten:

*   Stack erstellen: _Compute + Networking_
*   Stack erstellen: _Nur Compute_ mit einem vorhandenen VCN, in dem Sicherheitslisten gemäß _Schritt 2_ oben aktualisiert wurden

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autor** - Rene Fontcha, Master Principal Solutions Architect bei NA Technology
*   **Mitwirkende** - Kay Malcolm, Produktmanager, Datenbankproduktmanagement
*   **Zuletzt aktualisiert am/um** - Marion Smith, Technical Program Manager, April 2022