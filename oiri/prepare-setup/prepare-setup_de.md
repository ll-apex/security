# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der für die Ausführung dieses Workshops erforderlichen Ressource erforderlich ist. Für diesen Workshop ist eine Compute-Instanz erforderlich, auf der das Marketplace-Image von Oracle Identity Role Intelligence (OIRI) und ein virtuelles Cloud-Netzwerk (VCN) ausgeführt werden.

_Geschätzte Laborzeit:_ 15 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier- oder kostenpflichtige Cloud-Accounts von Oracle

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:

\- \[oiri-mkplc-freetier.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/iFttTcCqZvJ1WCvrpbo8rv-\_WHSTXg8zp-TqCfAtR40K-MnPYu9RULyEAlUQHN1p/n/natdsecurity/b/stack/o/oiri-mkplc-freetier.zip) \- \[oiri-mkplc-app-onboarding.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/DTG8immn-5boO9l5E-4MkB8VC2o-iJzTxiLHGlrZViSxhdy6jye8JWN1nICq6qoy/n/natdsecurity/b/stack/o/oiri-mkplc-app-onboarding.zip)

2.  Speichern Sie in Ihrem Download-Ordner.

Wir empfehlen dringend, mit diesem Stack ein eigenständiges/dediziertes VCN mit Ihren Instanzen zu erstellen. Fahren Sie mit _Schritt 3_ fort, um unsere Empfehlungen zu befolgen. Wenn Sie ein vorhandenes VCN verwenden möchten, fahren Sie wie unten angegeben mit dem nächsten Schritt fort, um das vorhandene VCN mit den erforderlichen Ingress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN zu verwenden, müssen die folgenden Ports zu Ingress-Regeln hinzugefügt werden

| Hafen | Beschreibung |
| :-- | :-- |
| 22 | SSH |
| (6080) | noVNC Remotedesktop |

1.  Gehen Sie zu _Networking >> Virtuelle Cloud-Netzwerke_.
2.  Wählen Sie Ihr Netzwerk aus
3.  Wählen Sie unter "Resources" die Option "Security Lists" aus.
4.  Klicken Sie unter der Schaltfläche "Create Security List" auf "Default Security Lists".
5.  Klicken Sie auf die Schaltfläche "Add Ingress Rule".
6.  Geben Sie Folgendes ein:
    *   Quell-CIDR: 0.0.0.0/0
    *   Zielportbereich: _Siehe obige Tabelle_
7.  Klicken Sie auf die Schaltfläche "Add Ingress Rules".

## Aufgabe 3: Compute einrichten

Fahren Sie mit den Details aus den beiden oben genannten Schritten mit der Übung _Umgebungssetup_ fort, um Ihre Workshopumgebung mit Oracle Resource Manager (ORM) und einer der folgenden Optionen einzurichten:

*   Stack erstellen: _Compute + Networking_
*   Stack erstellen: _Nur Compute_ mit einem vorhandenen VCN, in dem Sicherheitslisten gemäß _Schritt 2_ oben aktualisiert wurden

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Juni 2021