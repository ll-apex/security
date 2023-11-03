# Sensible Daten ermitteln

## Einführung

Mit der Daten-Discovery können Sie sensible Daten in Ihren Zieldatenbanken finden. Sie teilen der Daten-Discovery mit, welche Art von sensiblen Daten gesucht werden soll. Sie prüft die tatsächlichen Daten in der Zieldatenbank und dem zugehörigen Data Dictionary und gibt dann eine Liste der sensiblen Spalten zurück. Standardmäßig kann die Data Discovery nach einer Vielzahl sensibler Daten in Bezug auf Identifikation, Biografie, IT, Finanzen, Gesundheitswesen, Beschäftigung und akademische Informationen suchen.

Prüfen Sie zunächst sensible Daten in einer der Tabellen in der Zieldatenbank mit Database Actions. Verwenden Sie dann Oracle Data Safe, um sensible Daten in der Zieldatenbank zu erkennen und ein Modell für sensible Daten zu generieren. Erstellen Sie eine PDF-Datei Ihres Modells für sensible Daten.

Geschätzte Laborzeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Sensible Daten mit der Daten-Discovery in der Zieldatenbank erkennen
*   Modell für sensible Daten analysieren
*   PDF des Berichts zum Modell für sensible Daten erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

### Annahmen

*   Ihre Datenwerte können sich von den Werten in den Screenshots unterscheiden.

## Aufgabe 1: Sensible Daten mit der Daten-Discovery in der Zieldatenbank erkennen

1.  Stellen Sie sicher, dass Sie sich auf der Browserregisterkarte für Oracle Data Safe befinden. Melden Sie sich bei Bedarf erneut an.
    
2.  Klicken Sie im Navigationspfad oben auf der Seite auf **Data Safe**.
    
3.  Klicken Sie links unter **Sicherheitscenter** auf **Daten-Discovery**.
    
4.  Wählen Sie in der Dropdownliste **Compartment** Ihr Compartment aus.
    
    Eine Übersichtsseite für die Daten-Discovery wird mit Statistiken für die fünf wichtigsten Zieldatenbanken in Ihrem Compartment angezeigt. Ihre Seite ist höchstwahrscheinlich leer, da Sie Data Discovery zum ersten Mal in diesem Workshop verwenden.
    
5.  Klicken Sie auf **Sichere Daten ermitteln**.
    
    Der Assistent **Modell für sensible Daten erstellen** wird angezeigt.
    
6.  Führen Sie auf der Seite **Basisinformationen angeben** die folgenden Schritte aus, und klicken Sie auf **Weiter**.
    
    *   Geben Sie im Feld **Name** **SDM1** ein.
    *   Lassen Sie das Compartment auf Ihr Compartment setzen.
    *   Geben Sie im Feld **Beschreibung** **Modell für sensible Daten 1** ein.
    *   Wählen Sie die Zieldatenbank aus
    
    ![Allgemeine Informationen angeben](images/provide-basic-information-page.png "Allgemeine Informationen angeben")
    
7.  Lassen Sie auf der Seite **Schemas auswählen** **Nur bestimmte Schemas auswählen** aktiviert. Scrollen Sie nach unten, wählen Sie das Schema **HCM1** aus, und klicken Sie auf **Weiter**. Möglicherweise müssen Sie auf den Rechtspfeil unten auf der Seite klicken, um zu Seite 2 zu navigieren.
    
    ![Seite "Schemas auswählen"](images/select-schemas-page.png "Seite "Schemas auswählen"")
    
8.  Blenden Sie auf der Seite **Sensible Typen auswählen** alle sensiblen Kategorien ein, indem Sie den Schieberegler **Alle einblenden** nach rechts verschieben. Scrollen Sie auf der Seite nach unten, und prüfen Sie die sensiblen Typen. Beachten Sie, dass Sie einzelne sensible Typen, sensible Kategorien und alle sensiblen Typen gleichzeitig auswählen können. Aktivieren Sie oben auf der Seite das Kontrollkästchen **Alle**, und klicken Sie auf **Weiter**.
    
    ![Sensible Arten auswählen (Seite)](images/select-sensitive-types-page.png "Sensible Arten auswählen (Seite)")
    
9.  Wählen Sie auf der Seite **Discovery-Optionen auswählen** die Option **Beispieldaten erfassen, anzeigen und speichern** aus, und klicken Sie unten auf der Seite auf **Modell für sensible Daten erstellen**, um den Daten-Discovery-Prozess zu starten.
    
    ![Seite "Discovery-Optionen auswählen"](images/select-discovery-options-page.png "Seite "Discovery-Optionen" auswählen")
    
10.  Warten Sie, bis das Modell für sensible Daten erstellt wurde. Die Seite **Details zum Modell für sensible Daten** wird angezeigt.
    

## Aufgabe 2: Modell für sensible Daten analysieren

1.  Prüfen Sie die Informationen auf der Seite **Details zum Modell für sensible Daten**.
    
    *   Auf der Registerkarte **Informationen zum Modell für sensible Daten** werden Informationen zu Ihrem Modell für sensible Daten aufgeführt, einschließlich Name und Oracle Cloud-ID (OCID), Compartment, in dem Sie es gespeichert haben, Datum und Uhrzeit der Erstellung und letzten Aktualisierung, das Mit ihr verknüpfte Zieldatenbank, das ausgewählte Schema für Discovery (HCM1), die ausgewählten sensiblen Typen für Discovery (Klicken Sie auf den Link **Details anzeigen**) und Summen für erkannte sensible Schemas, sensible Tabellen, sensible Spalten, sensible Typen und sensible Werte.
    *   Sie können die ausgewählten sensiblen Typen für die Discovery anzeigen (klicken Sie auf **Details anzeigen**).
    *   Sie können die Arbeitsanforderungsinformationen anzeigen (klicken Sie auf **Details anzeigen**).
    *   Das Tortendiagramm zeigt Prozentsätze sensibler Kategorien und sensibler Typen an.
    *   In der Tabelle **Sensible Spalten** werden die erkannten sensiblen Spalten aufgeführt. Standardmäßig wird die Tabelle im Format **Flache Ansicht** angezeigt. Für jede sensible Spalte können Sie den Schemanamen, den Tabellennamen, den Spaltennamen, den sensiblen Typ, die übergeordnete Spalte, den Datentyp, die geschätzte Zeilenanzahl, Beispieldaten (wenn Sie Beispieldaten abrufen und falls vorhanden) und Auditdatensätze anzeigen. Prüfen Sie die Beispieldaten, um eine Vorstellung davon zu erhalten, wie sie aussehen.
    
    ![Details (Seite)](images/sensitive-data-model-details-page-1.png "Details (Seite)") ![Details (Seite)](images/sensitive-data-model-details-page-2.png "Details (Seite)")
    
2.  Bewegen Sie den Mauszeiger über die Kategorie **Identifikationsinformationen** im Diagramm, um dessen Wert anzuzeigen. Ihr Prozentwert kann sich vom Wert im Screenshot unterscheiden.
    
    ![Kategorie "Identifikationsinformationen" im Modelldiagramm für sensible Daten](images/sdm-chart-identification-information.png "Kategorie "Identifikationsinformationen" im Modelldiagramm für sensible Daten")
    
3.  Halten Sie die Maus über **Identifikationsinformationen**, und klicken Sie auf das Kreissegment, um einen Drilldown durchzuführen. Beachten Sie, dass die Kategorie **Identifikationsinformationen** jetzt in zwei kleinere Kategorien unterteilt ist (**Persönliche IDs** und **Öffentliche IDs**).
    
    ![Persönliche und öffentliche IDs im Modelldiagramm für sensible Daten](images/sdm-chart-personal-public-identifiers.png "Persönliche und öffentliche IDs im Modelldiagramm für sensible Daten")
    
4.  Um einen Drilldown durchzuführen, klicken Sie im Navigationspfad des Diagramms auf den Link **Alle**.
    
5.  Wählen Sie unter **Sensible Spalten** in der Dropdown-Liste die Option **Sensible Typansicht** aus, um die sensiblen Spalten nach sensiblem Typ zu sortieren. Standardmäßig werden alle Elemente in der Ansicht eingeblendet. Sie können die Elemente ausblenden, indem Sie den Schieberegler **Alle einblenden** nach links verschieben.
    
    ![Ansicht des Modells für sensible Daten](images/sensitive-type-view-sdm1.png "Ansicht des Modells für sensible Daten")
    
6.  Wählen Sie in der Dropdown-Liste die Option **Schemaansicht** aus, um die sensiblen Spalten nach Tabellenname zu sortieren.
    
    *   Wenn eine sensible Spalte ermittelt wurde, weil sie eine Beziehung zu einer anderen sensiblen Spalte aufweist, wie sie im Data Dictionary der Datenbank definiert ist, wird die andere sensible Spalte in der **Übergeordneten Spalte** angezeigt. Beispiel: `EMPLOYEE_ID` in der Tabelle `EMP_EXTENDED` hat eine Beziehung zu `EMPLOYEE_ID` in der Tabelle `EMPLOYEES`.
    
    ![Schemaansicht des Modells für sensible Daten](images/schema-view-sdm1.png "Schemaansicht des Modells für sensible Daten")
    

## Aufgabe 3: PDF des Berichts "Modell für sensible Daten" erstellen

1.  Klicken Sie oben auf der Seite **Details zu Modellen für sensible Daten** auf **Bericht generieren**.
    
    Das Dialogfeld **Bericht generieren** wird angezeigt.
    
2.  Lassen Sie **PDF** ausgewählt, klicken Sie auf **Bericht generieren**, und warten Sie, bis der Bericht zu 100% generiert wird. Klicken Sie auf den Link **hier**, um den Bericht herunterzuladen.
    
    ![PDF-Bericht von SDM1 generieren](images/generate-pdf-report-sdm1.png "PDF-Bericht von SDM1 generieren")
    
3.  Den PDF-Bericht öffnen und prüfen.
    
    *   In der Tabelle **Übersicht** werden Summen für gescannte Spalten und Werte sowie die Anzahl für sensible Typen, sensible Tabellen, sensible Spalten und sensible Werte angezeigt.
    *   In der Tabelle **Sensible Spalten** werden die sensiblen Spalten im Modell für sensible Daten aufgeführt. Für jede sensible Spalte zeigt die Tabelle den sensiblen Typ, den Schemanamen, den Tabellennamen, den Spaltennamen, die Anzahl sensibler Werte, ob die Spaltendaten übereinstimmen (J oder N), ob der Spaltenname übereinstimmt (J oder N) und ob der Spaltenkommentar übereinstimmt (J oder N).
    
    ![PDF-Bericht von SDM1](images/pdf-report-sdm1.png "PDF-Bericht von SDM1")
    
4.  Schließen Sie den PDF-Bericht, und kehren Sie zu Oracle Data Safe zurück.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Data Discovery](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-discovery.html)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023