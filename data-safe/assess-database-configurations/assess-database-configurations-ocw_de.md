# Datenbankkonfigurationen bewerten

## Einführung

Mit der Sicherheitsbewertung können Sie die Sicherheit Ihrer Datenbankkonfigurationen bewerten. Es analysiert Datenbankkonfigurationen, Benutzerkonten und Sicherheitskontrollen und meldet die Ergebnisse mit Empfehlungen für Korrekturaktivitäten, die Best Practices befolgen, um Risiken zu reduzieren oder zu mindern. Standardmäßig generiert Oracle Data Safe automatisch Sicherheitsbewertungen für Ihre Zieldatenbanken und speichert sie in der Bewertungshistorie. Sie können Bewertungsdaten in allen Zieldatenbanken und für jede Zieldatenbank analysieren. Sie können die Sicherheitsabweichung in Ihren Zieldatenbanken überwachen, indem Sie die letzte Bewertung mit einer Baseline oder einer anderen Bewertung vergleichen.

In dieser Übung untersuchen Sie die Sicherheitsbewertung.

Geschätzte Zeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Übersichtsseite für Sicherheitsbewertungen anzeigen
*   Zeigen Sie die letzte Sicherheitsbewertung für die Zieldatenbank an
*   Letzte Bewertung als Baselinebewertung festlegen
*   Aktivität in der Zieldatenbank generieren
*   Letzte Sicherheitsbewertung aktualisieren und Ergebnisse analysieren
*   Vergleich der Bewertung mit der Baseline

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet
*   Zieldatenbank bei Oracle Data Safe registriert

### Annahmen

*   Ihre Datenwerte unterscheiden sich höchstwahrscheinlich von denen in den Screenshots.

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Datenbankkonfigurationen bewerten](videohub:1_f7ijv58k)

## Aufgabe 1: Übersichtsseite für Sicherheitsbewertung anzeigen

1.  Klicken Sie im Sicherheitscenter auf **Sicherheitsbewertung**.
    
2.  Wählen Sie unter **Listengeltungsbereich** Ihr Compartment aus. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
    Auf der Überblickseite werden Statistiken für die Zieldatenbank angezeigt.
    
3.  Prüfen Sie oben auf der Seite die Diagramme **Risikostufe** und **Risiken nach Kategorie**.
    
    *   Das Diagramm **Risk Level** enthält eine prozentuale Aufschlüsselung der verschiedenen Risikoebenen (Hoch, Mittel, Niedrig, Hinweis und Bewerten) in allen Zieldatenbanken in den ausgewählten Compartments.
    *   Das Diagramm **Risiken nach Kategorie** enthält eine prozentuale Aufschlüsselung der verschiedenen Risikokategorien (Benutzeraccounts, Berechtigungen und Rollen, Autorisierungskontrolle, Datenverschlüsselung, fein granulierter Zugriff, Auditing und Datenbankkonfigurationen) in den Zieldatenbanken in den ausgewählten Compartments.
    
    ![Diagramme für Risikohöhe und Risiken der Sicherheitsbewertung nach Kategorie für alle Ziele](images/ocw/sa_risklevel_risksbycategory.png "Diagramme für Risikohöhe und Risiken der Sicherheitsbewertung nach Kategorie für alle Ziele")
    
4.  Prüfen Sie die Informationen auf der Registerkarte **Risikoübersicht**.
    
    *   In der Registerkarte **Risikoübersicht** wird angezeigt, welche Risiken Sie über alle Zieldatenbanken in den angegebenen Compartments hinweg haben.
    *   Sie können die Anzahl der Risikoergebnisse {\\b High}, {\\b Medium}, {\\b Low}, {\\b Advisory} und {\\b Evaluation} in allen Zieldatenbanken miteinander vergleichen sowie anzeigen, in welchen Risikokategorien die Zahlen am höchsten sind.
    *   Zu den Risikokategorien gehören Zieldatenbanken, Benutzeraccounts, Berechtigungen und Rollen, Autorisierungskontrolle, fein granulierte Zugriffskontrolle, Datenverschlüsselung, Auditing und Datenbankkonfiguration.
    
    ![Registerkarte "Sicherheitsbewertung - Risikoübersicht"](images/ocw/sa-risk-summary-tab.png "Registerkarte "Sicherheitsbewertung - Risikoübersicht"")
    
5.  Klicken Sie auf die Registerkarte **Zielübersicht**, und prüfen Sie die Informationen.
    
    *   In der Registerkarte **Zielübersicht** wird der Sicherheitsstatus jeder einzelnen Zieldatenbank angezeigt.
    *   Sie können die Anzahl der Risikoergebnisse für die einzelnen Zieldatenbanken anzeigen.
    *   Sie können das Bewertungsdatum anzeigen und herausfinden, ob die letzte Bewertung von einer Baseline abweicht (falls eine festgelegt ist).
    *   Sie können den letzten Bewertungsbericht für jede Zieldatenbank zugreifen.
    
    ![Registerkarte "Sicherheitsbewertungsziel - Übersicht"](images/ocw/sa-target-summary-tab.png "Registerkarte "Sicherheitsbewertungsziel - Übersicht"")
    

## Aufgabe 2: Letzte Sicherheitsbewertung für die Zieldatenbank anzeigen

Oracle Data Safe erstellt bei der Registrierung automatisch eine Sicherheitsbewertung der Zieldatenbank. Diese Bewertung wird als _letzte Bewertung_ bezeichnet.

1.  On the **Target Summary** tab, locate the line that has your target database, and click **View Report**.
    
    Die letzte Sicherheitsbewertung für die Zieldatenbank wird angezeigt. Beachten Sie, dass die **letzte Bewertung für die Zieldatenbank** oben auf der Seite angezeigt wird.
    
2.  Prüfen Sie die Tabelle auf der Registerkarte **Bewertungsübersicht**.
    
    *   In dieser Tabelle wird die Anzahl der Ergebnisse für jede Kategorie im Bericht verglichen und die Anzahl der Ergebnisse pro Risikohöhe gezählt (**Hohes Risiko**, **Mittles Risiko**, **Niedriges Risiko**, **Beratung**, **Bewertung** und **Erfolgreich**).
    *   Diese Werte helfen Ihnen, Bereiche zu identifizieren, die Aufmerksamkeit benötigen.
    
    ![Letzte Registerkarte "Sicherheitsbewertungsübersicht"](images/ocw/latest-sa-assessment-summary-tab.png "Letzte Registerkarte "Sicherheitsbewertungsübersicht"")
    
3.  Um Details zur Sicherheitsbewertung selbst anzuzeigen, klicken Sie auf die Registerkarte **Bewertungsinformationen**.
    
    *   Zu den Details gehören der Bewertungsname, die OCID, das Compartment, in dem die Bewertung gespeichert wurde, der Zieldatenbankname, die Zieldatenbankversion, das Bewertungsdatum, der Zeitplan, der Name der Baselinebewertung (sofern festgelegt) und das Kennzeichen "Baseline" (Ja, Nein oder Kein Baseline-Set).
    
    ![Letzte Registerkarte "Informationen zur Sicherheitsbewertung"](images/ocw/latest-sa-assessment-information-tab.png "Letzte Registerkarte "Informationen zur Sicherheitsbewertung"")
    
4.  Benennen Sie die letzte Sicherheitsbewertung um: Klicken Sie auf das Stiftsymbol rechts neben **Name**, geben Sie **SA\_target-database** ein (ersetzen Sie **target-database** durch den Namen der Zieldatenbank), und klicken Sie auf das Symbol **Speichern**.
    
    ![Letzte Sicherheitsbewertung umbenennen](images/ocw/rename-latest-sa-assessment.png "Letzte Sicherheitsbewertung umbenennen")
    
5.  Scrollen Sie nach unten, und zeigen Sie den Abschnitt **Bewertungsdetails** an.
    
    *   In diesem Abschnitt werden alle Ergebnisse für jede Risikokategorie angezeigt.
    *   Risiken sind farbcodiert, damit Sie Kategorien mit hohem Risiko leicht identifizieren können (rot).
    *   Die unter **Berechtigungen und Rollen** aufgeführten Ergebnisse mit hohem Risiko wurden eingeführt, als Sie das SQL-Skript ausgeführt haben, um die Zieldatenbank mit Beispieldaten zu füllen.
    
    ![Letzter Abschnitt "Sicherheitsbewertungsdetails"](images/ocw/latest-sa-assessment-details-section.png "Letzter Abschnitt "Sicherheitsbewertungsdetails"")
    
6.  Beachten Sie, dass Sie unter **Nach Risiken filtern** auf der linken Seite die anzuzeigenden Risikostufen auswählen können. Wählen Sie **Kennwort** aus, und klicken Sie auf **Anwenden**.
    
    ![Sicherheitsbewertungsfilter für Risikostufen](images/sa-filters-risk-levels.png "Sicherheitsbewertungsfilter für Risikostufen")
    
7.  Erweitern Sie die Kategorien, und prüfen Sie die Ergebnisse.
    
    *   Jedes Ergebnis zeigt Ihnen den Status (Risikostufe), eine Zusammenfassung des Ergebnisses, Details zu dem Ergebnis, Anmerkungen zur Risikominderung und Referenzen - ob ein Ergebnis vom Center for Internet Security (**CIS**), der Datenschutz-Grundverordnung der Europäischen Union (**DSGVO**) und/oder dem Security Technical Implementation Guide (**STIG**) empfohlen wird. Diese Referenzen erleichtern Ihnen die Identifizierung der empfohlenen Sicherheitskontrollen.
    *   Im folgenden Beispiel enthält das Ergebnis **Transparente Datenverschlüsselung** zwei Referenzen: **STIG** und **DSGVO**.
    
    ![Transparente Datenverschlüsselung - Suche](images/ocw/transparent-data-encryption-finding.png "Transparente Datenverschlüsselung - Suche")
    

## Aufgabe 3: Letzte Bewertung als Baselinebewertung festlegen

Eine Baselinebewertung zeigt Daten für alle Zieldatenbanken in einem ausgewählten Compartment zu einem bestimmten Zeitpunkt an. Da es sich jedoch nur um eine Zieldatenbank in Ihrem Compartment handelt, werden in der Baselinebewertung nur Daten für eine Zieldatenbank angezeigt.

1.  Klicken Sie oben auf der Seite auf **Als Baseline festlegen**.
    
    Das Dialogfeld **Als Baseline festlegen?** wird angezeigt.
    
    ![Dialogfeld "Als Baseline festlegen"](images/set-as-baseline-dialog-box.png "Dialogfeld "Als Baseline festlegen"")
    
2.  Klicken Sie auf **Ja**, um zu bestätigen, dass Sie diese Ergebnisse als Baseline festlegen möchten.
    
3.  _Wichtig: Bleiben Sie auf der Seite, bis die Meldung **Basisplan festgelegt** angezeigt wird._
    
    ![Sicherheitsbewertungs-Baseline wurde als Meldung festgelegt](images/sa-baseline-has-been-set-message.png "Sicherheitsbewertungs-Baseline wurde als Meldung festgelegt")
    

## Aufgabe 4: Aktivität in der Zieldatenbank generieren

In dieser Aufgabe geben Sie einen `GRANT`\-Befehl in der Zieldatenbank aus, damit Sie Bewertungen später beim Aktualisieren der letzten Sicherheitsbewertung vergleichen können.

1.  Greifen Sie in Database Actions auf das SQL-Arbeitsblatt zu. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an.
    
2.  Löschen Sie bei Bedarf das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
3.  Geben Sie im Arbeitsblatt folgenden Befehl ein:
    
        <copy>grant ALTER ANY ROLE to PUBLIC;</copy>
        
4.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil).
    
    ![Schaltfläche "Anweisung ausführen"](images/run-statement-button.png "Schaltfläche "Anweisung ausführen"")
    

## Aufgabe 5: Letzte Sicherheitsbewertung aktualisieren und Ergebnisse analysieren

1.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück.
    
2.  Klicken Sie oben in der aktuellen Sicherheitsbewertung auf **Jetzt aktualisieren**, um die neuesten Daten abzurufen.
    
    Der Bereich **Jetzt aktualisieren** wird angezeigt.
    
3.  Geben Sie im Feld **Letzte Bewertung speichern** die Option **Meine Sicherheitsbewertung** ein, und klicken Sie auf **Jetzt aktualisieren**. Warten Sie, bis der Status **Erfolgreich** lautet.
    
    *   Mit dieser Aktion werden die Daten in der letzten Sicherheitsbewertung für die Zieldatenbank aktualisiert. Außerdem wird eine Kopie der Bewertung (mit dem Namen "Meine Sicherheitsbewertung") in der Bewertungshistorie gespeichert.
    *   Der Aktualisierungsvorgang dauert etwa eine Minute.
    
    ![Bereich "Jetzt aktualisieren" für Sicherheitsbewertung](images/sa-refresh-now-panel.png "Bereich "Jetzt aktualisieren" für Sicherheitsbewertung")
    
4.  Klicken Sie auf die Registerkarte **Testinformationen**. Beachten Sie, dass das Bewertungsdatum und die Bewertungszeit gerade liegen und **Entspricht Baseline** gleich **Nein** ist.
    
    ![Sicherheitsbewertung aktuell bewertet am](images/ocw/sa-assessed-on-right-now.png "Sicherheitsbewertung aktuell bewertet am")
    
5.  Scrollen Sie nach unten, und blenden Sie **Systemberechtigungen für Public erteilt** ein.
    
    *   Dies ist ein hohes Risiko.
    *   Im Abschnitt **Details** wird angezeigt, dass die Berechtigung, die Sie in der vorherigen Aufgabe erteilt haben, identifiziert wurde.
    
    ![Systemberechtigungen, die PUBLIC-Ergebnissen erteilt werden](images/system-privileges-granted-to-public.png "Systemberechtigungen, die PUBLIC-Ergebnissen erteilt werden")
    

## Aufgabe 6: Bewertung mit Baseline vergleichen

1.  Wenn die neueste Sicherheitsbewertung angezeigt wird, klicken Sie links unter **Ressourcen** auf **Mit Baseline vergleichen**. Oracle Data Safe beginnt automatisch mit der Verarbeitung des Vergleichs.
    
    Wenn Sie die letzte Sicherheitsbewertung verlassen haben, können Sie wie folgt zur Bewertung zurückkehren: Klicken Sie im Navigationspfad auf **Sicherheitsbewertung**. Klicken Sie auf die Registerkarte **Zielübersicht**. Klicken Sie für die Zieldatenbank auf **Bericht anzeigen**.
    
    ![Option "Mit Baseline vergleichen" unter Ressourcen](images/sa-resources-compare-with-baseline-option.png "Option "Mit Baseline vergleichen" unter Ressourcen")
    
2.  Wenn der Vergleichsvorgang abgeschlossen ist, scrollen Sie auf der Seite nach unten zum Abschnitt **Mit Baseline vergleichen**, und prüfen Sie die Informationen.
    
    *   Prüfen Sie die Anzahl der Ergebnisse pro Risikokategorie für jede Risikoebene. Die Kategorien umfassen **Benutzeraccounts**, **Berechtigungen und Rollen**, **Autorisierungskontrolle**, **Datenverschlüsselung**, **fein granulierte Zugriffskontrolle**, **Auditing** und **Datenbankkonfiguration**.
    *   Sie können feststellen, wo die Änderungen in der Zieldatenbank vorgenommen wurden, indem Sie Zellen anzeigen, die das Wort **Geändert** enthalten. Die Zahl stellt die Gesamtanzahl der neuen, korrigierten und geänderten Risiken in der Zieldatenbank dar.
    *   In der Detailtabelle können Sie die Risikostufe für jedes Ergebnis, die Kategorie, zu der das Ergebnis gehört, den Ergebnisnamen und eine Beschreibung der Änderungen in der Zieldatenbank anzeigen. Die Spalte "Vergleichsbericht" ist wichtig, da sie erläutert, was seit der Generierung des Baselineberichts geändert, hinzugefügt oder aus der Zieldatenbank entfernt wurde.
    *   Beachten Sie, dass die vorgenommene Änderung in der Spalte **Vergleichsbericht** notiert wird.
    
    ![Sicherheitsbewertungsvergleichsbericht - oberster Teilsektor](images/ocw/sa-comparison-report-top2.png "Sicherheitsbewertungsvergleichsbericht - oberster Teilsektor") ![Sicherheitsbewertungsvergleich - Bericht unten](images/ocw/sa-comparison-report-bottom2.png "Sicherheitsbewertungsvergleich - Bericht unten")
    

## Weitere Informationen

*   [Überblick über Sicherheitsbewertungen](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-030B2A14-272F-49CF-80D2-5559C722E0FF)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 17. August 2023