# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für Oracle Resource Manager-(ORM-)Stacks herunterladen, die zum Einrichten der Ressourcen für die Ausführung dieses Workshops in weiteren Übungen erforderlich ist.

_Geschätzte Zeit:_ 10 Minuten

### Ziele

*   ORM-Stack für das Deployment des **Linux-Servers und der OCI-IAM-Identitätsdomains** herunterladen
*   ORM-Stack zum Konfigurieren des **Linux-Servers und der OCI-IAM-Identitätsdomains** herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Oracle Cloud-Account mit mindestens **PAY GO**\-Abonnement
*   Ein vorhandenes _VCN_.
*   Ein _öffentliches_ Subnetz.
*   Ein _SSH-Schlüssel_paar. Ein einzelnes SSH-Schlüsselpaar, das für beide Server verwendet werden soll (Stellen Sie sicher, dass die Formate .key und .pem des Private Keys verfügbar sind)

## Aufgabe 1: ZIP-Datei des Oracle Resource Manager-(ORM-)Stacks zum Deployment herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    *   [Stack1-Deploy.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/PAMStack1%20-%20Deploy.zip)
        
    *   [Stack2-Configure.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/PAMStack2%20-%20Configure.zip)
        
2.  Speichern Sie im Ordner _downloads_.
    

Wir empfehlen dringend, diesen Stack in einem eigenständigen/dedizierten VCN mit Ihrer Instanz zu verwenden. Fahren Sie mit der nächsten Aufgabe fort, um das vorhandene VCN mit den erforderlichen Ingress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN/Subnetz zu verwenden, müssen die folgenden Regeln zur Sicherheitsliste hinzugefügt werden.

| Typ | Quellport | Quell-CIDR | Zielport | Protokoll | Beschreibung |
| :-- | :-: | :-: | :-: | :-: | :-- |
| Ingress | Alle | 0.0.0.0/0 | 22 | TCP | SSH |
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

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat
*   **Mitwirkender** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Juli 2023