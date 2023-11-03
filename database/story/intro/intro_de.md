# Einführung

## Über diesen Workshop

Dieser Workshop ist eine praktische Übung zu den Features und Funktionen der Oracle Database-Sicherheit, um die häufigsten Cyberangriffe auf Oracle Databases zu verhindern, zu erkennen und zu mindern. Weitere Informationen zu den einzelnen Produkten finden Sie in den folgenden Workshops:

*   [DB-Sicherheitsgrundlagen](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=698)
*   [DB Security Advanced](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=726)\-Übungen.

In dieser Übung untersuchen wir anhand eines Ransomware-Angriffs, wie Angreifer agieren und welche Datenbankfunktionen Sie verwenden sollten, um Datenexfiltrationsrisiken zu verhindern, zu erkennen und zu mindern.

Ein typischer Ransomware-Angriff umfasst Diebstahl und Exfiltration von Daten. Der Diebstahl geschieht normalerweise, bevor Sie Ihr System verschlüsseln, um es funktionsunfähig zu machen, mit den gestohlenen Daten, die von der Ransomware-Bande verwendet werden, um Ihnen zu helfen, das Lösegeld zu bezahlen.

![Ransomware-Angriffsmeldung](./images/intro-hack-01.png "Ransomware-Angriffsmeldung")

Um dies zu ermöglichen, stellen wir Ihnen die erforderlichen Infrastrukturkomponenten basierend auf einer OCI-Architektur zur Verfügung, die in wenigen Minuten bereitgestellt wird, damit Sie die häufigsten Angriffe testen können, die Hacker mit einer einfachen Internetverbindung auf einer Datenbank ausnutzen.

Da alle diese Komponenten in der VM DBSec-Lab des Workshops gespeichert sind, können Sie Ihren Angriff ohne Risiko und ohne Angst durchführen, um Anwendungsfälle für die Datenbanksicherheit in einer vom Oracle Database Security Product Manager-Team vorkonfigurierten Umgebung zu testen.

_Geschätzte Zeit_: 40 Minuten

> Wenn Sie mit Ransomware nicht vertraut sind, nehmen Sie sich bitte einen Moment Zeit, um den Abschnitt "Weitere Informationen" unten zu lesen.

### Über dieses Produkt/diese Technologie

In dieser Übung verwenden Sie die folgenden Ressourcen:

*   SSH-Terminalclient
*   Oracle-Datenbanken
*   Glassfish HR-App
*   Audit Vault-Webkonsole
*   OEM Cloud Control (DBA-Webkonsole)

Beachten Sie, dass die Glassfish HR-Anwendung eine fiktive Webanwendung für das Mitarbeitermanagement ist, die auf eine ungesicherte Oracle Database mit dem Namen PDB1 verweist. In unserem Szenario enthält diese Datenbank sensible Daten, die von den Angreifern verwendet werden könnten, um eine Ransomware-Zahlung zu erpressen, oder im Dark Web für Gewinn verkauft werden.

Während des Fortschritts Ihres Angriffsprotokolls testen Sie dieselben Befehle von denselben Schnittstellen, diesmal jedoch auf eine andere Oracle Database namens PDB2. Die von Oracle empfohlenen Sicherheitskontrollen schützen PDB2. Sie werden sehen, wie eine gut konfigurierte Datenbank die häufigsten Angriffe blockieren kann, die zum Einbrechen und Stehlen von Daten verwendet werden.

_In dieser Übung getestete Versionen:_ Oracle DB EE 19.13, OEM 13.5, AVDF 20.5

### Ziele

In dieser Übung lernen Sie einige der wichtigsten Sicherheitsfeatures von Oracle Database kennen.

Folgende Themen werden behandelt:

*   Datenexfiltration verhindern, erkennen und mindern
*   Eskalation und Missbrauch von Berechtigungen verhindern und erkennen
*   Verhindern und mindern Sie die Ausnutzung von Schwachstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Ein Oracle Cloud-Account - Auf der Landingpage LiveLabs dieses Workshops können Sie sehen, welche Umgebungen unterstützt werden
    
    _Hinweis:_
    
    *   _Wenn Sie über ein **kostenloses Testkonto** verfügen, wird Ihr Account nach Ablauf der kostenlosen Testversion in einen Account vom Typ "**Immer kostenlos**" konvertiert._
    *   _Sie können keine Free Tier-Workshops durchführen, es sei denn, die Umgebung vom Typ "Immer kostenlos" ist verfügbar_
    *   _**[Klicken Sie hier für die FAQ-Seite zu Free Tier](https://www.oracle.com/cloud/free/faq.html)**_
*   Damit Ihre Erfahrung mit diesem Workshop optimal ist, sollten Sie "Übung: _Umgebung initialisieren_" NICHT VERGESSEN, um sicherzustellen, dass alle diese Ressourcen korrekt eingerichtet sind!
    

Das gesamte Database Security Team wünscht Ihnen einen ausgezeichneten Workshop!

Sie können jetzt mit der nächsten Übung fortfahren.

## Weitere Informationen

Lassen Sie uns vor dem Start erläutern, warum wir uns für einen **Ransomware-Angriff** entschieden haben, um die Datenbanksicherheitsfunktionen von Oracle zu demonstrieren.

Cyberkriminelle werden immer mehr gerüstet und besser vorbereitet. Sie haben jetzt ein großes technologisches Arsenal, das es ihnen ermöglicht, Angriffe gegen Sie von fast überall aus zu starten, wenn Sie nicht bereit sind, mit ihnen umzugehen.

Die Motivation des Angreifers zu verstehen und zu verhindern, dass er die gestohlenen Daten gegen Sie verwenden kann, hilft Ihnen, den Angriff besser zu überleben und macht es möglich, das Lösegeld NICHT zu bezahlen.

Laut einem aktuellen [ENISA-Bericht](https://www.enisa.europa.eu/publications/enisa-threat-landscape-2021) ist Ransomware eine der Hauptbedrohungen der Welt heute (die Anzahl der Angriffe nimmt täglich zu und alle Sektoren sind ausnahmslos betroffen). Ransomware hat sich von Anfang an zum bevorzugten Angriff entwickelt, um die sensiblen Daten eines Unternehmens zu stehlen.

Frühe Ransomware-Angriffe unterbrachen und blockierten die Aktivitäten eines Unternehmens. Noch 2019 definierte NIST Ransomware als "**eine Art von böswilligem Angriff, bei dem die Angreifer die Daten des Unternehmens verschlüsseln und die Zahlung verlangen, um den Zugriff wiederherzustellen**". Das Verschlüsseln von Daten und das Zurückhalten des Verschlüsselungsschlüssels war ein Auftakt zu einer Zahlungsaufforderung!

Von da an standen Ihnen nur noch folgende Optionen zur Verfügung:

*   **Bezahlen Sie das Lösegeld**, um zu hoffen, den Entschlüsselungsschlüssel zu erhalten und so Ihre Daten wiederherzustellen ... aber ohne Garantie, dass es funktioniert, oder sogar, dass die Angreifer es später nicht erneut versuchen werden.
*   **Zahlen Sie nicht das Lösegeld** und stellen Sie Ihr System aus Ihren Backups wieder her... in der Hoffnung, dass die Backups physisch noch verfügbar, konsistent und schnell genug wiederherstellbar sind, um Ihr Unternehmen zu erhalten.

Wenn Sie nicht auf diese Eventualität vorbereitet waren und nicht erwartet hatten, damit umgehen zu müssen, dann war die Störung dieser Art von Angriff für Ihr Unternehmen eine ziemliche Herausforderung zu überwinden. Wenn Sie diese Nachricht unten sehen, befinden Sie sich wahrscheinlich in einem extremen Stresszustand!

![Ransomware-Bildschirm](./images/intro-hack-02.png "Ransomware-Bildschirm")

Ransomware entwickelte sich schnell. Laut [MS-ISAC im Jahr 2020](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjAo9Wi_fX3AhUG_IUKHVTIDcUQFnoECBUQAQ&url=https%3A%2F%2Fwww.cisa.gov%2Fsites%2Fdefault%2Ffiles%2Fpublications%2FCISA_MS-ISAC_Ransomware%2520Guide_S508C.pdf&usg=AOvVaw1T7xwLzdEx9zlCoNSNytU0) "haben böswillige Akteure ihre Ransomware-Taktik im Laufe der Zeit angepasst, um die Opfer unter Druck zu setzen, zu zahlen, indem sie gestohlene Daten freizugeben drohen, wenn sie sich weigern, Opfer zu bezahlen, und Opfer öffentlich als sekundäre Formen von Erpressung zu benennen und zu beschämen".

Mit anderen Worten, bevor Sie Ihr System verschlüsseln, um es nicht verfügbar zu machen, stehlen die Angreifer Ihre sensiblen Daten und sind bereit, sie zu lecken und zu verkaufen, wenn Sie sich weigern, das Lösegeld zu zahlen! Die Diebe änderten ihre Taktik, um es wahrscheinlicher zu machen, dass Sie das Lösegeld bezahlen würden, anstatt sich nur von der Sicherung zu erholen. Es gibt mehrere Techniken, mit denen die Diebe die Daten stehlen, wie unten gezeigt.

![Angriffsfläche einer Datenbank](./images/intro-hack-03.png "Angriffsfläche einer Datenbank")

Glücklicherweise ist die Oracle Database die am besten ausgestattete Datenbank auf dem Markt für die Prävention, Erkennung und Analyse von Cyberbedrohungen.

### Weiter: Die Phasen eines Ransomware-Angriffs

Mehrere Schritte charakterisieren einen Cyberangriff, und Angreifer folgen ihnen in der Regel in der gleichen Reihenfolge.

Ransomware ist ein großes Geschäft - in Bezug auf das Einkommen kann es groß genug sein, um in die Global 100 aufgenommen zu werden! Moderne Ransomware-Banden sind professionell organisiert und operativ isoliert. Spezialisierte Gruppen werden Experten in bestimmten Phasen des Ransomware-Prozesses. Diese Gruppen verkaufen ihre Dienstleistungen oder die Ergebnisse ihrer Missetaten an andere Gruppen, die entweder nicht über die spezialisierten Fähigkeiten verfügen oder Zeit sparen möchten - wie jedes andere große Unternehmen. Dieser agile Ansatz bietet Ransomware-Banden eine größere Kapazität, um Einnahmen mit geringeren Betriebskosten zu generieren und gleichzeitig das Risiko zu begrenzen, erwischt und strafrechtlich verfolgt zu werden.

![Die Phasen eines Angriffs](./images/intro-hack-04.png "Die Phasen eines Angriffs")

#### **Schritt 1-5: Die Ruhe vor dem Sturm**

Die ersten vier Phasen eines Cyberangriffs können passieren, ohne dass Sie ihn kommen sehen, da diese Angriffe auf Systeme abzielen, bei denen sie am verwundbarsten sind, oft beginnend mit Benutzern. Beispiel: Angreifer zielen auf Mitarbeiter eines Unternehmens ab, die soziale Netzwerke verwenden, um das Zielsystem zu infiltrieren, indem sie Phishing-E-Mail-Angriffe senden, bösartige Websites einrichten, gefälschte Softwareupgrade-Alerts senden, infizierte USB-Laufwerke anbieten usw. (**1 - Aufklärung**) und daran arbeiten, dass sie auf einen Link klicken, einen Download eines Datei, sehen Sie sich ein Video oder eine andere Aktion an (**2 - Waffnung**), mit der Angreifer ein kleines Paket von Malware (Exploit Kit) auf ein System im Netzwerk des Unternehmens herunterladen können (**3 - Bereitstellung**), um vorhandene Sicherheitslücken auf das System auszunutzen, um einen besseren Stand zu erhalten (**4 - Ausnutzung**). Sobald die Malware Ihr System infiltriert, richtet der Schadcode eine Hintertür ein - eine Kommunikationsleitung, die einen dauerhaften Zugriff auf den Angreifer ermöglicht (**5 - Installation**). An diesem Punkt kann die Malware für Tage, Wochen oder Monate verborgen und inaktiv sein, bevor der Angreifer den Angriff initiiert oder diese Hintertür an einen anderen Angreifer verkauft!

Die Sensibilisierung und Schulung der Mitarbeiter für diese Risiken ist unerlässlich, um diese ersten Phasen zu bekämpfen. Ihre Mitarbeiter sollten Best Practices für die Sicherheit verstehen, insbesondere in sozialen Netzwerken. Auf technischer Ebene sollte sich das Unternehmen mit Web Application Firewalls, Anti-Spam-, Anti-Virus- und Intrusion Detection-Systemen ausstatten. Regelmäßige Penetrationstests und Sicherheitsaudits werden ebenfalls eine große Hilfe sein.

#### **Schritt 6: Der Wind steigt**

Sobald Angreifer einen Strandkopf in Ihrem Netzwerk haben, aktivieren sie eine Hintertür, um sich auf den tatsächlichen Angriff vorzubereiten, indem sie remote mit dem Exploit-Kit kommunizieren, das ihnen einen "hands-on-Tastaturzugriff" im Netzwerk des Ziels bietet (**6 - Befehl und Kontrolle (C2)**).

Hier beginnen sie sich zu verbreiten und andere Systeme anzugreifen, um mehrere Hintertüren einzurichten und das Netzwerk zu scannen, um die goldenen Assets zu entdecken, die gestohlen werden sollen (**6a - Seitliche Bewegung**). Eines der Systeme mit dem höchsten Angriffswert ist Ihr Unternehmensverzeichnis (in der Regel Microsoft Active Directory) und der DNS-Server. Das erfolgreiche Eindringen dieser beiden Systeme führt zur Fähigkeit, viele andere Systeme zu durchdringen.

So können sie die Architektur des Systems, das sie vor sich haben, abbilden und herausfinden, welche Server für sie von großem Nutzen sind, welche sie kompromittieren müssen und welche sie vermeiden müssen. Sie werden natürlich versuchen, neue Sicherheitslücken zu identifizieren, und sie werden alles tun, um ihre Berechtigungen (**6b - Berechtigungseskalation**) durch Harvesting oder Diebstahl von Zugangsdaten zu erhöhen. Diese Phase kann sehr zeitaufwändig sein, ist aber entscheidend für den endgültigen Erfolg des Angriffs. Um sich genügend Zeit zu geben, um sich im gesamten Unternehmen zu verbreiten, versuchen Angreifer, nicht entdeckt zu werden, indem sie intelligent unter dem Radar arbeiten.

In jeder Phase des Angriffs können Sie eine Übergabe zwischen verschiedenen Gruppen von Angreifern sehen, da eine spezialisierte Gruppe ihre Ergebnisse an eine andere Gruppe verkauft, die den Angriff weiterführt.

#### **Schritt 7: Mitten im Sturm**

Jetzt kennen die Angreifer das gezielte System, haben die richtigen Werkzeuge und haben genug Berechtigungen, um zu handeln, so dass alles vorhanden ist, um ihre Mission zu erfüllen, und der eigentliche Angriff kann beginnen. Dieser Angriff kann jederzeit erfolgen, wenn der Angreifer Ihre Organisation auswählt und vollständig außer Kontrolle bringt (**7 - Aktion bei Zielen**). Sobald der Angriff begonnen hat, kann es ein Wettlauf gegen die Zeit sein, dass Ihr Unternehmen sogar erkennt, dass der Angriff stattfindet, damit Minderungs- und Wiederherstellungsbemühungen in Aktion treten können.

Sobald ein Angriff gestartet wurde, sind Ihr System und Ihre Daten gefährdet. Ohne einen Minderungs- und Wiederherstellungsplan können Ihre sensiblen Daten geändert oder offengelegt werden, und Ausfallzeiten auf Ihren Systemen können von Tagen bis Monaten reichen. In den meisten Fällen werden einige Daten nie wiederhergestellt. Die Ergebnisse eines Ransomware-Angriffs sind teuer und können katastrophal sein, sowohl für Ihr Endergebnis als auch für Ihren Markenimage.

Erwarten Sie, dass Angreifer Ihre Datenbanken hinter sich lassen. Datenbanken sind Repositorys großer Mengen sensibler Daten, die in Systemen gespeichert werden, die speziell entwickelt wurden, um ihre Nutzung und Analyse zu erleichtern! Warum sollte ein Angreifer Energie verschwenden, um Informationen aus Hunderten von Dateien auf mehreren Servern zu stehlen, wenn er in eine Datenbank einbrechen und Millionen von Datensätzen in einer einzigen Abfrage exfiltrieren kann (**7a - Datenexfiltration**)? Heute haben sich einige Hacker auf Datenexfiltration mit starken Datenbankfähigkeiten spezialisiert und verkaufen ihre Dienste für gezielte Angriffe an andere kriminelle Gruppen, die diese Fähigkeiten nicht haben.

Es gibt mehrere Möglichkeiten, sensible Daten zu extrahieren:

*   im Transit durch schnüffelnden Netzwerkverkehr
*   im Ruhezustand, direkt vom Datenträger (Datendateien, Export- oder Backupdateien), um Datenbankzugriffskontrollen zu umgehen
*   aus einer duplizierten Datenbank wie den Nicht-Produktionsdatenbanken
*   aus der Anwendung mit einem SQL-Injection-Angriff, beispielsweise
*   aus der Produktionsdatenbank durch Ausnutzung einer Sicherheitslücke, einer fehlerhaften Einstellung usw. oder durch Kompromittierung gültiger Zugangsdaten für die Verwendung

Sobald die Daten gesammelt sind, senden die Angreifer sie über einen HTTPS-Tunnel zu einer dedizierten Plattform im Dark Net zurück an die Command and Control-Server (C2), die in der Regel langsam verkauft werden können. Zu Ihrer Verteidigung sieht es so aus, als würde jemand eine Website durchsuchen, die mit SSL geschützt ist.

Das ist es, Ihre Daten befinden sich jetzt außerhalb Ihrer Firewall, die für wen verwendet wird - und jetzt verschlüsseln die Angreifer Ihre Systeme, einschließlich Backups, und fordern ein Lösegeld (**7b - Ransomware bereitstellen**)! Wenn Sie nicht bezahlen möchten, senden sie Ihnen eine Probe der Daten, die sie exfiltriert haben, um Sie unter Druck zu setzen, indem sie drohen, sie öffentlich zu machen.

Unglücklicherweise, ob Sie das Lösegeld zahlen oder nicht, sind Ihre sensiblen Daten jetzt da draußen, und die Wahrscheinlichkeit, dass es durchgesickert oder weiterverkauft wird, ist immer noch sehr hoch. Es ist zu spät, denn "die Zahnpasta kann nicht mehr in die Tube passen"!

**Führen Sie daher die in dieser Übung besprochenen Schritte aus, um Ihre sensiblen Daten zu schützen und Datendiebstahl zu vermeiden. Auf diese Weise werden Sie sich nicht mit diesen Problemen befassen, nachdem Sie angegriffen wurden**.

## Danksagungen

*   **Autor** - Hakim Loumi, Senior Principal PM für Datenbanksicherheit
*   **Mitwirkende** - Russ Lowenthal
*   **Zuletzt aktualisiert am/um** - Hakim Loumi - August 2022