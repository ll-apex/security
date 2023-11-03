# Sensible Daten ermitteln

## Einführung

Mit der Daten-Discovery können Sie sensible Daten in Ihren Zieldatenbanken finden. Sie teilen der Daten-Discovery mit, welche Art von sensiblen Daten gesucht werden soll. Sie prüft die tatsächlichen Daten in der Zieldatenbank und dem zugehörigen Data Dictionary und gibt dann eine Liste der sensiblen Spalten zurück. Standardmäßig kann die Data Discovery nach einer Vielzahl sensibler Daten in Bezug auf Identifikation, Biografie, IT, Finanzen, Gesundheitswesen, Beschäftigung und akademische Informationen suchen.

Verwenden Sie Oracle Data Safe, um sensible Daten in der Zieldatenbank zu erkennen und dann das Modell für sensible Daten anzupassen.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Sensible Daten mit der Daten-Discovery in der Zieldatenbank erkennen
*   Modell für sensible Daten anpassen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet
*   Zieldatenbank bei Oracle Data Safe registriert

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Sensible Daten ermitteln](videohub:1_d0fz5ep5)

## Aufgabe 1: Sensible Daten mit der Daten-Discovery in der Zieldatenbank erkennen

1.  Stellen Sie sicher, dass Sie sich auf der Browserregisterkarte für Oracle Data Safe befinden. Melden Sie sich bei Bedarf erneut an.
    
2.  Klicken Sie im Navigationspfad oben auf der Seite auf **Data Safe**.
    
3.  Klicken Sie links unter **Sicherheitscenter** auf **Daten-Discovery**.
    
4.  Wählen Sie in der Dropdownliste **Compartment** Ihr Compartment aus.
    
    Eine Übersichtsseite für die Daten-Discovery wird mit Statistiken für die fünf wichtigsten Zieldatenbanken in Ihrem Compartment angezeigt. Ihre Seite ist höchstwahrscheinlich leer, da Sie Data Discovery zum ersten Mal in diesem Workshop verwenden.
    
5.  Klicken Sie auf **Sichere Daten ermitteln**.
    
    Die Seite **Modell für sensible Daten erstellen** wird angezeigt.
    
6.  Führen Sie auf der Seite **Basisinformationen angeben** die folgenden Schritte aus, und klicken Sie auf **Weiter**.
    
    *   Geben Sie im Feld **Name** **SDM1** ein.
    *   Lassen Sie das Compartment auf Ihr Compartment setzen.
    *   Geben Sie im Feld **Beschreibung** **Modell für sensible Daten 1** ein.
    *   Wählen Sie die Zieldatenbank aus
    
    ![Allgemeine Informationen angeben](images/ocw/provide-basic-information-page.png "Allgemeine Informationen angeben")
    
7.  Lassen Sie auf der Seite **Schemas auswählen** **Nur bestimmte Schemas auswählen** aktiviert. Scrollen Sie nach unten, wählen Sie das Schema **HCM1** aus, und klicken Sie auf **Weiter**. Möglicherweise müssen Sie auf den Rechtspfeil unten auf der Seite klicken, um zu Seite 2 zu navigieren.
    
    ![Seite "Schemas auswählen"](images/ocw/select-schemas-page.png "Seite "Schemas auswählen"")
    
8.  Blenden Sie auf der Seite **Sensible Typen auswählen** alle sensiblen Kategorien ein, indem Sie den Schieberegler **Alle einblenden** nach rechts verschieben. Scrollen Sie auf der Seite nach unten, und prüfen Sie die sensiblen Typen. Beachten Sie, dass Sie einzelne sensible Typen, sensible Kategorien und alle sensiblen Typen gleichzeitig auswählen können. Aktivieren Sie oben auf der Seite das Kontrollkästchen **Alle**, und klicken Sie auf **Weiter**.
    
    ![Sensible Arten auswählen (Seite)](images/ocw/select-sensitive-types-page.png "Sensible Arten auswählen (Seite)")
    
9.  Wählen Sie auf der Seite **Discovery-Optionen auswählen** die Option **Beispieldaten erfassen, anzeigen und speichern** aus, und klicken Sie unten auf der Seite auf **Modell für sensible Daten erstellen**, um den Daten-Discovery-Prozess zu starten.
    
    ![Seite "Discovery-Optionen auswählen"](images/select-discovery-options-page.png "Seite "Discovery-Optionen" auswählen")
    
10.  Warten Sie, bis das Modell für sensible Daten erstellt wurde.
    
    Die Seite **Details zum Modell für sensible Daten** wird angezeigt.
    
11.  Prüfen Sie die Informationen auf der Seite **Details zum Modell für sensible Daten**.
    
    *   In der Registerkarte **Informationen zum Modell für sensible Daten** werden Informationen zu Ihrem Modell für sensible Daten aufgeführt, einschließlich Name und Oracle Cloud-ID (OCID), Compartment, in dem Sie es gespeichert haben, Datum und Uhrzeit, zu dem es gespeichert wurde wurde erstellt und zuletzt aktualisiert, die zugehörige Zieldatenbank, das ausgewählte Discovery-Schema (HCM1) und Summen für erkannte sensible Schemas, sensible Tabellen, sensible Spalten, sensible Typen und sensible Werte.
    *   Sie können die ausgewählten sensiblen Typen für die Discovery anzeigen (klicken Sie auf **Details anzeigen**).
    *   Sie können die Arbeitsanforderungsinformationen anzeigen (klicken Sie auf **Details anzeigen**).
    *   Das Tortendiagramm vergleicht die Anzahl der sensiblen Werte pro sensibler Kategorie und sensibler Typ.
    *   In der Tabelle **Sensible Spalten** werden die erkannten sensiblen Spalten aufgeführt. Standardmäßig wird die Tabelle im Format **Flache Ansicht** angezeigt. Für jede sensible Spalte können Sie den Schemanamen, den Tabellennamen, den Spaltennamen, den sensiblen Typ, die übergeordnete Spalte, den Datentyp, die geschätzte Zeilenanzahl und Beispieldaten anzeigen (wenn Sie Beispieldaten abrufen und vorhanden sind). Prüfen Sie die Beispieldaten, um eine Vorstellung davon zu erhalten, wie sie aussehen.
    
    ![Details (Seite)](images/ocw/sensitive-data-model-details-page-1.png "Details (Seite)") ![Details (Seite)](images/ocw/sensitive-data-model-details-page-2.png "Details (Seite)")
    

## Aufgabe 2: Modell für sensible Daten anpassen

Entfernen Sie die Spalte `DATE_OF_HIRE` aus dem Modell für sensible Daten.

1.  Klicken Sie im Abschnitt **Sensible Spalten** auf **Spalten entfernen**.
    
    Der Bereich **Spalten entfernen** wird angezeigt.
    
2.  Geben Sie im Feld **COLUMNNAME** den Wert **DATE** ein, und wählen Sie **DATE\_OF\_HIRE** aus.
    
3.  Klicken Sie auf **Suchen**.
    
4.  Aktivieren Sie das Kontrollkästchen für die Spalte **DATE\_OF\_HIRE** in der Tabelle **JOB\_HISTORY**, und klicken Sie auf **Spalten entfernen**.
    
    ![Fensterbereich "Spalten entfernen"](images/ocw/remove-columns-panel.png "Fensterbereich "Spalten entfernen"")
    

## Weitere Informationen

*   [Data Discovery](https://docs.oracle.com/en-us/iaas/data-safe/doc/data-discovery.html)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 17. August 2023