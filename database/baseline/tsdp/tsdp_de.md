# Oracle Transparent Sensitive Data Protection (TSDP)

## Einführung

In diesem Workshop werden die Funktionen von Oracle Transparent Sensitive Data Protection (TSDP) vorgestellt. Es gibt dem Benutzer die Möglichkeit, zu lernen, wie er diese Funktionen konfiguriert, um den Zugriff auf sensible Daten zu schützen, indem er sie on-the-fly redigiert.

_Geschätzte Laborzeit:_ 15 Minuten

_In dieser Übung getestete Version:_ Oracle DB 19.19

### Videovorschau

Kein Video für den Moment

### Ziele

*   TSDP-Policy für sensible Daten erstellen
*   Prüfen Sie die Verdeckung dynamischer sensibler Daten, um deren Exposition außerhalb der Anwendung zu verhindern

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
| 1 | Vorbereitung der TSDP-Umgebung für die Labs | <5 Minuten |
| 2 | TSDP-Policy erstellen | 5 Minuten |
| 3 | TSDP-Laborumgebung zurücksetzen | <5 Minuten |

## Aufgabe 1: TSDP-Umgebung für die Übungen vorbereiten

1.  Öffnen Sie eine Terminalsession auf Ihrer VM **DBSec-Lab** als BS-Benutzer _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Hinweis**: Wenn Sie eine Remote-Desktopsitzung verwenden, doppelklicken Sie auf das Symbol _Terminal_ auf dem Desktop, um eine Sitzung zu starten.
    
2.  Gehen Sie zum Skriptverzeichnis
    
        <copy>cd $DBSEC_LABS/tsdp</copy>
        
3.  Erstellen Sie den TSDP-**Admin-Benutzer**, den TSDP-**Dateneigentümer**, und erstellen Sie die **Tabelle für TSDP-Übungen**.
    
        <copy>./tsdp_prepare_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-001.png "TSDP-Admin-Benutzer erstellen")
    

## Aufgabe 2: TSDP-Policy erstellen

1.  Erstellen Sie den sensiblen Typ "`CREDIT_CARD_TYPE`"
    
        <copy>./tsdp_create_sensitive_type.sh</copy>
        
    
    ![TSDP](./images/tsdp-002.png "Erstellen Sie den sensiblen Typ CREDIT_CARD_TYPE")
    
    **Hinweis:**
    
    *   Der sensible Typ ist eine Datenklasse, die Sie als vertraulich kennzeichnen
    *   Hier erstellen wir einen sensiblen Typ "`credit_card_type`" **für alle Kreditkartennummern**
2.  Identifizieren Sie die zu schützenden sensiblen Spalten (hier verwenden wir die Spalte "`CORPORATE_CARD`")
    
        <copy>./tsdp_add_sensitive_col.sh</copy>
        
    
    ![TSDP](./images/tsdp-003.png "Zu schützende sensible Spalten identifizieren")
    
    **Hinweis:** Um die zu schützenden Spalten basierend auf dem von Ihnen definierten sensiblen Typ zu identifizieren, können Sie diese Spalten entweder mit einem OEM Cloud Control Application Data Model (ADM) identifizieren oder die Prozedur `DBMS_TSDP_MANAGE.ADD_SENSITIVE_COLUMN` verwenden.
    
3.  Erstellen Sie die TSDP-Policy "`REDACT_PARTIAL_CC`" basierend auf einer **teilweisen Verdeckung**, bei der die ersten 8 Zeichen durch "\*" ersetzt werden
    
        <copy>./tsdp_create_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-004.png "Erstellen Sie die TSDP-Policy REDACT_PARTIAL_CC")
    
    **Hinweis:** Sie können die Policy erstellen, indem Sie einen anonymen Block mit den folgenden Komponenten definieren:
    
    *   Wenn Sie Oracle Data Redaction für Ihre Policy verwenden, eine Spezifikation des zu verwendenden Datenverdeckungstyps, z.B. partielle Datenverdeckung
    *   Wenn Sie Oracle Virtual Private Database für Ihre Policy verwenden, eine Spezifikation der VPD-Einstellungen, die Sie verwenden möchten
    *   Bedingungen zum Testen, wenn die Policy aktiviert ist. Beispiel: Der Datentyp der Spalte, der erfüllt werden muss, bevor die Policy aktiviert werden kann
    *   Eine benannte transparente Policy zum Schutz sensibler Daten, um diese Komponenten mit der Prozedur `DBMS_TSDP_PROTECT.ADD_POLICY` zu verknüpfen
4.  Verknüpfen Sie die TSDP-Policy "`REDACT_PARTIAL_CC`" mit dem sensiblen Typ "`CREDIT_CARD_TYPE`", der zuvor erstellt wurde
    
        <copy>./tsdp_associate_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-005.png "TSDP-Richtlinie zuordnen")
    
5.  Wählen Sie sensible Daten aus, **bevor Sie** die TSDP-Policy aktivieren
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-006.png "Wählen Sie sensible Daten aus, bevor Sie die TSDP-Policy aktivieren")
    
    **Hinweis:** Die Kreditkartennummern in der Spalte "`CORPORATE_CARD`" sind als Klartext angegeben.
    
6.  TSDP-Policy "`REDACT_PARTIAL_CC`" aktivieren
    
        <copy>./tsdp_enable_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-007.png "Aktivieren Sie die TSDP-Policy REDACT_PARTIAL_CC")
    
7.  Wählen Sie sensible Daten **nach Aktivierung** der TSDP-Policy aus
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-008.png "Sensible Daten nach Aktivierung der TSDP-Policy auswählen")
    
    **Hinweis:**
    
    *   Jetzt können Sie sehen, dass die Kreditkartennummern mit dem Format `****-****-9999-9999` verdeckt wurden.
    *   Wie Sie sehen können, verdeckt TSDP sensible Daten **sofort**, und Sie müssen **die SQL-Abfrage nicht neu starten oder neu schreiben**!

## Aufgabe 3: TSDP-Laborumgebung zurücksetzen

1.  Sobald Sie mit dem TSDP-Konzept vertraut sind, können Sie die Umgebung zurücksetzen
    
        <copy>./tsdp_reset_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-009.png "TSDP-Laborumgebung zurücksetzen")
    

Sie können jetzt mit der nächsten Übung fortfahren!

## **Anhang**: Informationen zum Produkt

### **Überblick**

Transparent Sensitive Data Protection (TSPD) ist eine Möglichkeit, Tabellenspalten zu finden und zu klassifizieren, die vertrauliche Informationen enthalten.

Mit diesem Feature können Sie die Tabellenspalten in einer Datenbank mit sensiblen Daten schnell finden, diese Daten klassifizieren und dann eine Policy erstellen, die diese Daten als Ganzes für eine bestimmte Klasse schützt. Beispiele für diese Art von sensiblen Daten sind Kreditkartennummern oder Sozialversicherungsnummern.

Die TSDP-Policy schützt dann die sensiblen Daten in diesen Tabellenspalten mit den Einstellungen von Oracle Data Redaction oder Oracle Virtual Private Database. Die TSDP-Policy gilt auf Spaltenebene der Tabelle, die Sie schützen möchten, und richtet sich an einen bestimmten Spaltendatentyp, z.B. alle Datentypen NUMBER von Spalten, die Kreditkarteninformationen enthalten. Sie können eine einheitliche TSDP-Policy für alle von Ihnen klassifizierten Daten erstellen und diese Policy bei Bedarf ändern, wenn sich die Compliancevorschriften ändern. Optional können Sie die TSDP-Policys zur Verwendung in anderen Datenbanken exportieren.

Die Vorteile von TSDP-Richtlinien sind enorm: Sie können problemlos TSDP-Richtlinien in einer großen Organisation mit zahlreichen Datenbanken erstellen und anwenden. Auf diese Weise können Auditoren den Schutz der Daten, auf die sich die TSDP-Richtlinien beziehen, erheblich schätzen. TSDP ist besonders nützlich für Regierungsumgebungen, in denen Sie möglicherweise viele Daten mit ähnlichen Sicherheitseinschränkungen haben und eine Richtlinie für alle diese Daten konsistent anwenden müssen. Die Policy könnte darin bestehen, sie zu verdecken, zu verschlüsseln, den Zugriff darauf zu kontrollieren, den Zugriff darauf zu prüfen und im Audittrail zu maskieren. Ohne TSDP müssten Sie jede Verdeckungs-Policy, Verschlüsselungskonfiguration auf Spaltenebene und Policy für virtuelle private Datenbanken spaltenweise konfigurieren.

### **Vorteile des transparenten Schutzes sensibler Daten (TSDP)**

*   **Sie konfigurieren den Schutz sensibler Daten einmal und stellen diesen Schutz nach Bedarf bereit**. Sie können transparente Policys für den Schutz sensibler Daten konfigurieren, um anzugeben, wie eine Datenklasse (z.B. Kreditkartenspalten) geschützt werden muss, ohne die Zieldaten tatsächlich angeben zu müssen. Mit anderen Worten: Wenn Sie die transparente Policy zum Schutz sensibler Daten erstellen, müssen Sie keine Referenzen auf die eigentlichen Zielspalten aufnehmen, die Sie schützen möchten. Die Policy für den transparenten Schutz sensibler Daten ermittelt diese Zielspalten basierend auf einer Liste sensibler Spalten in der Datenbank und den Verknüpfungen der Policy mit den angegebenen sensiblen Typen. Dies kann nützlich sein, wenn Sie Ihren Datenbanken weitere sensible Daten hinzufügen, nachdem Sie die transparenten Policys für den Schutz sensibler Daten erstellt haben. Nachdem Sie die Policy erstellt haben, können Sie den Schutz für die sensiblen Daten in einem einzigen Schritt aktivieren (Beispiel: Schutz basierend auf der gesamten Quelldatenbank aktivieren). Der sensible Typ der neuen Daten sowie die sensiblen Typ- und Policy-Verknüpfungen bestimmen, wie die sensiblen Daten geschützt werden. Auf diese Weise müssen Sie beim Hinzufügen neuer sensibler Daten den Schutz nicht konfigurieren, solange er die aktuellen Anforderungen der Richtlinie zum Schutz sensibler Daten erfüllt.
    
*   **Sie können den Schutz mehrerer sensibler Spalten verwalten**. Sie können den Schutz für mehrere sensible Spalten basierend auf einem geeigneten Attribut (wie der Quelldatenbank der Identifikation, dem sensiblen Typ selbst oder einem bestimmten Schema, einer Tabelle oder Spalte) aktivieren oder deaktivieren. Diese Granularität bietet ein hohes Maß an Kontrolle über die Datensicherheit. Durch das Design dieser Funktion können Sie die Datensicherheit basierend auf spezifischen Complianceanforderungen für große Datasets verwalten, die unter diese Compliancevorschriften fallen. Sie können die Datensicherheit basierend auf einer bestimmten Kategorie und nicht für jede einzelne Spalte konfigurieren. Beispiel: Sie können den Schutz für Kreditkartennummern oder Sozialversicherungsnummern konfigurieren, müssen jedoch nicht für jede Spalte in der Datenbank, die diese Daten enthält, den Schutz konfigurieren.
    
*   **Sie können die sensiblen Spalten schützen, die mit dem Oracle Enterprise Manager Cloud Control Application Data Modeling-(ADM-)Feature identifiziert werden.**Mit dem ADM-Feature von Cloud Control können Sie sensible Typen erstellen und eine Liste sensibler Spalten ermitteln. Anschließend können Sie diese Liste der sensiblen Spalten und die zugehörigen sensiblen Typen in die Datenbank importieren. Von dort aus können Sie mit diesen Informationen transparente Richtlinien zum Schutz sensibler Daten erstellen und verwalten.
    

## Sie möchten mehr erfahren?

Technische Dokumentation:

*   [Oracle Transparent Sensitive Data Protection 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/using-transparent-sensitive-data-protection.html)

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Pedro Lopes
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - November 2023