# Oracle-Datenverdeckung

## Einführung

In diesem Workshop werden die verschiedenen Features und Funktionen von Oracle Data Redaction vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie er diese Funktionen konfiguriert, um den Zugriff auf sensible Daten zu schützen, indem er sie on-the-fly redigiert.

_Geschätzte Laborzeit:_ 15 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Sehen Sie sich eine Vorschau von "_Livelabs - Oracle ASO (Transparent Data Encryption & Data Redaction) (Mai 2022)_" an[](youtube:JflshZKgxYs)

### Ziele

Vertrauliche Daten dynamisch verdecken, sodass sie nicht außerhalb der Anwendung angezeigt werden

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

### Laborzeit (geschätzt)

| Schritt Nr. | Merkmal | Ungefähr. Zeit |
| --- | --- | --- |
| 1 | Grundlegende Datenverdeckungs-Policy erstellen | 5 Minuten |
| 2 | Vorhandene Data Redaction Policy kontextualisieren | 5 Minuten |
| 3 | Datenverdeckungs-Policy löschen | <5 Minuten |

## Aufgabe 1: Grundlegende Datenverdeckungs-Policy erstellen

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/data-redaction</copy>
        
3.  Sehen wir uns zunächst die Daten an, bevor wir sie verdecken
    
    *   **Führen Sie in SQL\*Plus** diese Abfrage aus, um die ursprünglichen Daten anzuzeigen.
        
            <copy>./dr_query_employee_data.sh</copy>
            
        
        ![Datenverdeckung](./images/dr-001.png "Originaldaten ansehen")
        
        **Hinweis**: Je nach Mitarbeiter haben sie eine SIN-, SSN- oder NINO-Nummer.
        
    *   Werfen wir einen Blick **auf Ihre Glassfish-App**
        
        *   Öffnen Sie einen Webbrowser unter der URL _`http://dbsec-lab:8080/hr_prod_pdb1`_
            
            **Anmerkungen:** Wenn Sie den Remotedesktop nicht verwenden, können Sie diese Seite auch über _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_ aufrufen.
            
        *   Melden Sie sich bei der HR-Anwendung als _`hradmin`_ mit dem Kennwort "_`Oracle123`_" an.
            
                <copy>hradmin</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Datenverdeckung](./images/dr-002.png "HR-App - Anmeldung") ![Datenverdeckung](./images/dr-003.png "HR-App - Anmeldung")
            
        *   Klicken Sie auf **Mitarbeiter suchen**.
            
            ![Datenverdeckung](./images/dr-004.png "HR-App - Mitarbeiter suchen")
            
        *   Wir filtern den Mitarbeiter "Alice - UserID 77", indem wir beispielsweise _`77`_ als Wert für **Personalnummer** eingeben und auf \[**Suchen**\] klicken.
            
            ![Datenverdeckung](./images/dr-005.png "HR-App - Mitarbeiter suchen (UserID 77)")
            
        *   Klicken Sie jetzt auf den Link **Vollständiger Name** dieses Mitarbeiters, um seine Details anzuzeigen.
            
            ![Datenverdeckung](./images/dr-006.png "HR-Anwendung - Vor- und Nachname")
            
        *   Wie Sie sehen können, sind auch in Glassfish Daten aus Spalte SIN vollständig verdeckt
            
            ![Datenverdeckung](./images/dr-007.png "HR-Anwendung - SIN-Wert")
            
4.  Kehren Sie zur Terminalsession zurück, um die Verdeckungs-Policy `PROTECT_EMPLOYEES` zu erstellen
    
        <copy>./dr_redact_for_all.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-008.png "Verdeckungs-Policy PROTECT_EMPLOYEES erstellen")
    
    **Hinweis**: Diese Policy verdeckt (**FULL**) Daten in Spalte **SIN** aus der Tabelle `DEMO_HR_EMPLOYEES` für alle Abfragen in allen Kontexten (**Ausdruck "1=1"**)
    
5.  Führen Sie nun die Abfrage erneut aus, um die verdeckten Daten anzuzeigen und zu sehen, was in der SIN-Spalte passiert ist, nachdem Sie die Data Redaction Policy erstellt haben
    
        <copy>./dr_query_employee_data.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-009.png "Verdeckte Daten anzeigen")
    
    **Hinweis**:
    
    *   Wie Sie sehen können, sind die Daten aus Spalte SIN vollständig verdeckt!
    *   Nach der Aktivierung wird die Data Redaction Policy sofort angewendet, und es ist kein Neustart erforderlich
    *   Da Data Redaction bereits in das Oracle-Kernprodukt eingebettet ist, müssen Sie nichts konfigurieren. Führen Sie die Abfrage einfach erneut aus, um die Auswirkungen der Data Redaction Policy anzuzeigen, die auf Ihre sensiblen Daten erstellt wurde
    *   Beachten Sie, dass Sie einfach auf der Datenbankseite handeln und das ist es... Keine Notwendigkeit, etwas auf Ihrer Anwendungsseite neu zu codieren!
6.  Kehren Sie jetzt zu Ihrer Glassfish-App zurück, und aktualisieren Sie einfach die Webseite (**drücken Sie \[F5\]**)
    
    ![Datenverdeckung](./images/dr-010.png "HR-Anwendung - SIN-Wert")
    
    **Hinweis**:
    
    *   Jetzt ist auch in Glassfish die Säule SIN komplett rotiert!
    *   Es ist normal, weil die Verdeckungs-Policy für Spalte **SIN** für alle Abfragen in allen Kontexten erstellt wurde (**Ausdruck "1=1"**)
    *   Nach der Aktivierung wird die Data Redaction Policy sofort angewendet, und es ist kein Neustart erforderlich

## Aufgabe 2: Vorhandene Data Redaction Policy kontextualisieren

1.  Ändern Sie jetzt die Verdeckungsrichtlinie so, dass NUR Nicht-Glassfish-Abfragen verdeckt werden (um dies zu tun, benötigen wir einen **Ausdruck mit "Regelset"**)
    
        <copy>./dr_redact_nonapp_queries.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-011.png "NUR Nicht-Glassfish-Abfragen verdecken")
    
    **Hinweis**: Jetzt verdeckt dieselbe Policy (**FULL**) Daten in Spalte **SIN** aus der Tabelle `DEMO_HR_EMPLOYEES`, jedoch nur für Abfragen, bei denen der Kontext nicht autorisiert ist.
    
2.  Um zu zeigen, wie einfach es ist, neue Spalten in einer vorhandenen Datenverdeckungs-Policy hinzuzufügen, fügen Sie der Verdeckungs-Policy zusätzliche Spalten (**SSN** und **NINO**) hinzu
    
        <copy>./dr_add_redacted_columns.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-012.png "Verdeckungs-Policy ändern")
    
    **Hinweis**: In gleicher Weise wird diese Datenverdeckungs-Policy auch für die Spalten "SVN" und "NINO" für denselben Kontext angewendet.
    
3.  Lassen Sie uns nun die Auswirkungen auf
    
    *   ... **SQL\*Plus** (eine nicht autorisierte Anwendung ab jetzt)
        
            <copy>./dr_query_employee_data.sh</copy>
            
        
        ![Datenverdeckung](./images/dr-013.png "Siehe Daten auf SQLPlus")
        
        **Hinweis**: Es sollten keine sensiblen Daten angezeigt werden.
        
    *   ... **in Ihrer Glassfish-App** (der einzigen autorisierten Anwendung ab jetzt), indem Sie einfach die Webseite aktualisieren (**drücken Sie \[F5\]**)
        
        ![Datenverdeckung](./images/dr-007.png "Daten zur HR-App anzeigen")
        
        **Hinweis**: Da Sie die einzige autorisierte App verwenden, werden die sensiblen Daten jetzt angezeigt.
        

## Aufgabe 3: Löschen der Datenverdeckungs-Policy

1.  Nach Abschluss der Übung können Sie die Verdeckungs-Policy löschen
    
        <copy>./dr_drop_redaction_policy.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-014.png "Verdeckungs-Policy löschen")
    
2.  Prüfen, ob alle Daten jetzt nicht verdeckt sind
    
        <copy>./dr_query_employee_data.sh</copy>
        
    
    ![Datenverdeckung](./images/dr-001.png "Daten prüfen")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Diese Features sind fest im Kernprodukt von Oracle Database codiert und Teil der _Advanced Security Option (ASO)_

Mit Data Redaction können Sie Daten maskieren (redact), die von Abfragen zurückgegeben werden, die von Anwendungen abgesetzt wurden. Wir können auch über die dynamische Datenmaskierung sprechen.

Sie können Spaltendaten mit einer der folgenden Methoden verdecken:

*   **Vollständige Verdeckung** Sie verdecken den gesamten Inhalt der Spaltendaten. Der verdeckte Wert, der an den abfragenden Benutzer zurückgegeben wird, hängt vom Datentyp der Spalte ab. Beispiel: Spalten des Datentyps NUMBER werden mit einer Null (0) verdeckt und Zeichendatentypen mit einem Leerzeichen.
    
*   **Partielle Verdeckung** Ein Teil der Spaltendaten wird verdeckt. Beispiel: Sie können den größten Teil einer Sozialversicherungsnummer bis auf die letzten 4 Ziffern mit Sternchen (\*) verdecken.
    
*   **Reguläre Ausdrücke** Sie können reguläre Ausdrücke sowohl bei vollständiger als auch bei teilweiser Verdeckung verwenden. Auf diese Weise können Sie Daten basierend auf einem Suchmuster für die Daten verdecken. Beispiel: Sie können reguläre Ausdrücke verwenden, um bestimmte Telefonnummern oder E-Mail-Adressen in Ihren Daten zu verdecken.
    
*   **Zufällige Verdeckung** Die dem abfragenden Benutzer präsentierten verdeckten Daten werden je nach Datentyp der Spalte bei jeder Anzeige als zufällig generierte Werte angezeigt.
    
*   **Keine Verdeckung** Mit dieser Option können Sie die interne Funktionsweise Ihrer Verdeckungs-Policys testen. Dies hat keine Auswirkungen auf die Ergebnisse von Tabellenabfragen, für die Policys definiert sind. Mit dieser Option können Sie die Definitionen der Redaction Policy testen, bevor Sie sie in einer Production-Umgebung anwenden.
    

Die Datenverdeckung führt die Verdeckung zur Laufzeit aus, d.h. in dem Moment, in dem der Benutzer versucht, die Daten anzuzeigen. Diese Funktionalität eignet sich ideal für dynamische Produktionssysteme, in denen sich Daten ständig ändern. Während die Daten verdeckt werden, kann Oracle Database alle Daten normal verarbeiten und die referenziellen Integritäts-Constraints des Backends beibehalten. Mit der Datenverdeckung können Sie Branchenvorschriften wie den Payment Card Industry Data Security Standard (PCI DSS) und den Sarbanes-Oxley Act einhalten.

![Datenverdeckung](./images/aso-concept-dr.png "Datenverdeckungskonzept")

### **Vorteile von Oracle Data Redaction**

*   Sie haben verschiedene Verdeckungsstile, aus denen Sie wählen können
*   Da die Daten zur Laufzeit verdeckt werden, eignet sich Data Redaction gut für Umgebungen, in denen sich die Daten ständig ändern.
*   Sie können die Data Redaction-Policys an einem zentralen Ort erstellen und von dort aus einfach verwalten
*   Mit den Data Redaction Policys können Sie eine Vielzahl von Funktionsbedingungen basierend auf `SYS_CONTEXT`\-Werten erstellen. Diese können zur Laufzeit verwendet werden, um zu entscheiden, wann die Data Redaction Policys für die Ergebnisse der Abfrage des Anwendungsbenutzers gelten

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Datenverdeckung 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/asoag/asopart1.html)

Video:

*   _Oracle Data Redaction (Juli 2020)_[](youtube:ssy6Hov-MAs)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023