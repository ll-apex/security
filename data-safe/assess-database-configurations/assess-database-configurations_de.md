# Datenbankkonfigurationen bewerten

## Einführung

Mit der Sicherheitsbewertung können Sie die Sicherheit Ihrer Datenbankkonfigurationen bewerten. Es analysiert Datenbankkonfigurationen, Benutzerkonten und Sicherheitskontrollen und meldet die Ergebnisse mit Empfehlungen für Korrekturaktivitäten, die Best Practices befolgen, um Risiken zu reduzieren oder zu mindern. Standardmäßig generiert Oracle Data Safe automatisch Sicherheitsbewertungen für Ihre Zieldatenbanken und speichert sie in der Bewertungshistorie. Sie können Bewertungsdaten in allen Zieldatenbanken und für jede Zieldatenbank analysieren. Sie können die Sicherheitsabweichung in Ihren Zieldatenbanken überwachen, indem Sie die letzte Bewertung mit einer Baseline oder einer anderen Bewertung vergleichen.

In dieser Übung untersuchen Sie die Sicherheitsbewertung.

Geschätzte Laborzeit: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Übersichtsseite für Sicherheitsbewertungen anzeigen
*   Zeigen Sie die letzte Sicherheitsbewertung für die Zieldatenbank an
*   Historie der Sicherheitsbewertungen für die Zieldatenbank anzeigen
*   Baselinebewertung festlegen
*   Aktivität in der Zieldatenbank generieren
*   Letzte Sicherheitsbewertung aktualisieren und Ergebnisse analysieren
*   Ergebnisse auf hoher Risikostufe auf der Übersichtsseite prüfen
*   Vergleichsbericht für Sicherheitsbewertung generieren
*   Fügen Sie einen Zeitplan hinzu, um eine Sicherheitsbewertung für die Zieldatenbank jeden Sonntag um 11:30 Uhr zu speichern
*   Historie aller Sicherheitsbewertungen für alle Zieldatenbanken anzeigen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Sie haben einen Oracle Cloud-Account erhalten und sich bei der Oracle Cloud Infrastructure-Konsole angemeldet
*   Ihre Umgebung für diesen Workshop vorbereitet (siehe [Umgebung vorbereiten](?lab=prepare-environment))
*   Die Zieldatenbank bei Oracle Data Safe registriert (siehe [Autonomous Database bei Oracle Data Safe registrieren](?lab=register-autonomous-database))

### Annahmen

*   Ihre Datenwerte können sich von den Werten in den Screenshots unterscheiden.

## Aufgabe 1: Übersichtsseite für Sicherheitsbewertung anzeigen

1.  Klicken Sie im Sicherheitscenter auf **Sicherheitsbewertung**.
    
2.  Wählen Sie unter **Listengeltungsbereich** Ihr Compartment aus. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
    Auf der Überblickseite werden Statistiken für die Zieldatenbank angezeigt.
    
3.  Prüfen Sie oben auf der Seite die Diagramme **Risikostufe** und **Risiken nach Kategorie**.
    
    *   Das Diagramm **Risk Level** enthält eine prozentuale Aufschlüsselung der verschiedenen Risikoebenen (Hoch, Mittel, Niedrig, Hinweis und Bewerten) in allen Zieldatenbanken in den ausgewählten Compartments.
    *   Das Diagramm **Risiken nach Kategorie** enthält eine prozentuale Aufschlüsselung und Anzahl der verschiedenen Risikokategorien (Benutzeraccounts, Berechtigungen und Rollen, Autorisierungskontrolle, Datenverschlüsselung, fein granulierter Zugriff, Auditing und Datenbankkonfigurationen) in den Zieldatenbanken in den ausgewählten Compartments.
    
    ![Diagramme für Risikohöhe und Risiken der Sicherheitsbewertung nach Kategorie für alle Ziele](images/sa_risklevel_risksbycategory.png "Diagramme für Risikohöhe und Risiken der Sicherheitsbewertung nach Kategorie für alle Ziele")
    
4.  Prüfen Sie die Informationen auf der Registerkarte **Risikoübersicht**.
    
    *   In der Registerkarte **Risikoübersicht** wird angezeigt, welche Risiken Sie über alle Zieldatenbanken in den angegebenen Compartments hinweg haben.
    *   Sie können die Anzahl der Risikoergebnisse {\\b High}, {\\b Medium}, {\\b Low}, {\\b Advisory} und {\\b Evaluation} in allen Zieldatenbanken miteinander vergleichen sowie anzeigen, in welchen Risikokategorien die Zahlen am höchsten sind.
    *   Zu den Risikokategorien gehören Zieldatenbanken, Benutzeraccounts, Berechtigungen und Rollen, Autorisierungskontrolle, fein granulierte Zugriffskontrolle, Datenverschlüsselung, Auditing und Datenbankkonfiguration.
    
    ![Registerkarte "Sicherheitsbewertung - Risikoübersicht"](images/sa-risk-summary-tab.png "Registerkarte "Sicherheitsbewertung - Risikoübersicht"")
    
5.  Klicken Sie auf die Registerkarte **Zielübersicht**, und prüfen Sie die Informationen.
    
    *   In der Registerkarte **Zielübersicht** wird der Sicherheitsstatus jeder einzelnen Zieldatenbank angezeigt.
    *   Sie können die Anzahl der Risikoergebnisse für die einzelnen Zieldatenbanken anzeigen.
    *   Sie können das letzte Bewertungsdatum anzeigen und herausfinden, ob die letzte Bewertung von einem Basisplan abweicht (sofern festgelegt).
    *   Sie können auf die letzte Bewertung für jede Zieldatenbank zugreifen.
    
    ![Registerkarte "Sicherheitsbewertungsziel - Übersicht"](images/sa-target-summary-tab.png "Registerkarte "Sicherheitsbewertungsziel - Übersicht"")
    

## Aufgabe 2: Letzte Sicherheitsbewertung für die Zieldatenbank anzeigen

Oracle Data Safe erstellt bei der Registrierung automatisch eine Sicherheitsbewertung der Zieldatenbank. Diese Bewertung wird als _letzte Bewertung_ bezeichnet.

1.  On the **Target Summary** tab, locate the line that has your target database, and click **View Report**.
    
    Die letzte Sicherheitsbewertung für die Zieldatenbank wird angezeigt. Beachten Sie, dass die **letzte Bewertung für die Zieldatenbank** oben auf der Seite angezeigt wird.
    
2.  Prüfen Sie die Tabelle auf der Registerkarte **Bewertungsübersicht**.
    
    *   In dieser Tabelle wird die Anzahl der Ergebnisse für jede Kategorie im Bericht verglichen und die Anzahl der Ergebnisse pro Risikohöhe gezählt (**Hohes Risiko**, **Mittles Risiko**, **Niedriges Risiko**, **Beratung**, **Bewertung** und **Erfolgreich**).
    *   Diese Werte helfen Ihnen, Bereiche zu identifizieren, die Aufmerksamkeit benötigen.
    
    ![Letzte Registerkarte "Sicherheitsbewertungsübersicht"](images/latest-sa-assessment-summary-tab.png "Letzte Registerkarte "Sicherheitsbewertungsübersicht"")
    
3.  Um Details zur Sicherheitsbewertung selbst anzuzeigen, klicken Sie auf die Registerkarte **Bewertungsinformationen**.
    
    *   Zu den Details gehören der Bewertungsname, die OCID, das Compartment, in dem die Bewertung gespeichert wurde, der Name der Zieldatenbank, die Zieldatenbankversion, das Bewertungsdatum, der Zeitplan, der Name der Baselinebewertung (sofern festgelegt) und ob die Bewertung der Baselinebewertung entspricht (Ja, Nein oder Kein Baseline-Set).
    
    ![Letzte Registerkarte "Informationen zur Sicherheitsbewertung"](images/latest-sa-assessment-information-tab.png "Letzte Registerkarte "Informationen zur Sicherheitsbewertung"")
    
4.  Benennen Sie die letzte Sicherheitsbewertung um: Klicken Sie auf das Stiftsymbol rechts neben **Name**, geben Sie **Letzte Sicherheitsbewertung** ein, und klicken Sie auf das Symbol **Speichern**.
    
    ![Letzte Sicherheitsbewertung umbenennen](images/rename-latest-sa-assessment.png "Letzte Sicherheitsbewertung umbenennen")
    
5.  Scrollen Sie nach unten, und zeigen Sie den Abschnitt **Bewertungsdetails** an.
    
    *   In diesem Abschnitt werden alle Ergebnisse für jede Risikokategorie angezeigt.
    *   Risiken sind farbcodiert, damit Sie Kategorien mit hohem Risiko leicht identifizieren können (rot).
    *   Die unter **Berechtigungen und Rollen** aufgeführten Ergebnisse mit hohem Risiko wurden eingeführt, als Sie das SQL-Skript ausgeführt haben, um die Zieldatenbank mit Beispieldaten zu füllen.
    
    ![Letzter Abschnitt "Sicherheitsbewertungsdetails"](images/latest-sa-assessment-details-section.png "Letzter Abschnitt "Sicherheitsbewertungsdetails"")
    
6.  Beachten Sie, dass Sie unter **Nach Risiken filtern** auf der linken Seite die anzuzeigenden Risikostufen auswählen können. Wählen Sie **Kennwort** aus, und klicken Sie auf **Anwenden**.
    
    Der Abschnitt **Bewertungsdetails** wird aktualisiert und enthält Ergebnisse, für die kein Risiko gefunden wurde (sie haben die Ebene **Erfolgreich**).
    
    ![Sicherheitsbewertungsfilter für Risikostufen](images/sa-filters-risk-levels.png "Sicherheitsbewertungsfilter für Risikostufen")
    
7.  Beachten Sie unter **Filter nach Referenzen** auf der linken Seite, dass Sie die Liste der Ergebnisse auch nach Empfehlungen von DISA STIG (Security Technical Implementation Guide), CIS Benchmark (Center for Internet Security), EU-DSGVO (General Data Protection Regulation) und Oracle Best Practices filtern können.
    
    ![Filtert nach Referenzen](images/filters-by-references.png "Filtert nach Referenzen")
    
8.  Blenden Sie unter **Benutzerkonten** die Option **Benutzerdetails** ein.
    
    *   Für jeden Benutzer in der Zieldatenbank zeigt die Tabelle den Benutzerstatus, das verwendete Profil, den Default Tablespace des Benutzers, ob der Benutzer Oracle Defined (Ja oder Nein) ist und wie der Benutzer authentifiziert wird (Authentifizierungstyp).
    
    ![Sicherheitsbewertung - Benutzerdetails](images/sa-user-details.png "Sicherheitsbewertung - Benutzerdetails")
    
9.  Erweitern Sie eine weitere Kategorie, und prüfen Sie die Ergebnisse.
    
    *   Jedes Ergebnis zeigt Ihnen den Status (Risikostufe), eine Zusammenfassung des Ergebnisses, Details zum Ergebnis, Anmerkungen zur Risikominderung und Referenzen. Die Referenzen erleichtern Ihnen die Identifizierung der empfohlenen Sicherheitskontrollen.
    *   Im folgenden Beispiel ist das Ergebnis **Benutzer mit leistungsstarken Rollen** ein Ergebnis mit hohem Risiko, das zwei Referenzen aufweist: **STIG** und **CIS**.
    
    ![Benutzer mit starken Rollen finden](images/users-with-powerful-roles.png "Benutzer mit starken Rollen finden")
    
10.  Blenden Sie einige Kategorien unter **Berechtigungen und Rollen** ein, und prüfen Sie die Ergebnisse.
    
11.  Blättern Sie weiter nach unten und erweitern Sie andere Kategorien. Jede Kategorie listet zugehörige Ergebnisse zu Ihrer Zieldatenbank auf und wie Sie Änderungen vornehmen können, um deren Sicherheit zu verbessern.
    

## Aufgabe 3: Historie der Sicherheitsbewertungen für die Zieldatenbank anzeigen

1.  Klicken Sie oben auf der Seite auf **Historie anzeigen**.
    
2.  Stellen Sie sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
3.  Beachten Sie, dass eine Sicherheitsbewertung für die Zieldatenbank aufgelistet ist. Dies ist eine _statische_ Kopie (separate Kopie) der neuesten Sicherheitsbewertung.
    
    ![Bewertungshistorie (Seite)](images/assessment-history.png "Bewertungshistorie (Seite)")
    

## Aufgabe 4: Baselinebewertung festlegen

Eine Baselinebewertung zeigt Daten für alle Zieldatenbanken in einem ausgewählten Compartment zu einem bestimmten Zeitpunkt an. Da es sich jedoch nur um eine Zieldatenbank in Ihrem Compartment handelt, werden in der Baselinebewertung nur Daten für eine Zieldatenbank angezeigt. Legen wir die erste Sicherheitsbewertung als Baseline fest.

1.  Klicken Sie auf der Seite **Bewertungshistorie** für die Zieldatenbank auf den Namen der Sicherheitsbewertung. Die Details der Sicherheitsbewertung werden angezeigt.
    
2.  Klicken Sie auf **Als Baseline festlegen**.
    
    Das Dialogfeld **Als Baseline festlegen?** wird angezeigt.
    
    ![Dialogfeld "Als Baseline festlegen"](images/set-as-baseline-dialog-box.png "Dialogfeld "Als Baseline festlegen"")
    
3.  Klicken Sie auf **Ja**, um zu bestätigen, dass Sie diese Ergebnisse als Baseline festlegen möchten.
    
4.  _Wichtig: Bleiben Sie auf der Seite, bis die Meldung **Basisplan festgelegt** angezeigt wird._
    
    ![Sicherheitsbewertungs-Baseline wurde als Meldung festgelegt](images/sa-baseline-has-been-set-message.png "Sicherheitsbewertungs-Baseline wurde als Meldung festgelegt")
    
5.  Klicken Sie auf **Zurück**, um zur Seite **Bewertungshistorie** zurückzukehren, und bestätigen Sie, dass die Tabelle eine neue Zeile für die Baselinebewertung enthält. Der Bewertungsname beginnt mit **SA\_baseline**.
    
    ![Bewertungshistorie mit Baselinebewertung](images/sa-assessment-history-with-baseline.png "Bewertungshistorie mit Baselinebewertung")
    
6.  Klicken Sie auf **Schließen**.
    
    Die letzte Sicherheitsbewertung wird angezeigt.
    

## Aufgabe 5: Aktivität in der Zieldatenbank generieren

In dieser Aufgabe geben Sie einen `GRANT`\-Befehl in der Zieldatenbank aus, damit Sie Bewertungen später beim Aktualisieren der letzten Sicherheitsbewertung vergleichen können.

1.  Greifen Sie in Database Actions auf das SQL-Arbeitsblatt zu. Wenn Ihre Session abgelaufen ist, melden Sie sich erneut als Benutzer `ADMIN` an.
    
2.  Löschen Sie bei Bedarf das Arbeitsblatt und die Registerkarte **Skriptausgabe**.
    
3.  Geben Sie im Arbeitsblatt folgenden Befehl ein:
    
        <copy>grant ALTER ANY ROLE to PUBLIC;</copy>
        
4.  Klicken Sie in der Symbolleiste auf die Schaltfläche **Anweisung ausführen** (grüner Kreis mit weißem Pfeil).
    
    ![Schaltfläche "Anweisung ausführen"](images/run-statement-button.png "Schaltfläche "Anweisung ausführen"")
    

## Aufgabe 6: Letzte Sicherheitsbewertung aktualisieren und Ergebnisse analysieren

1.  Kehren Sie zur Browserregisterkarte für Oracle Data Safe zurück.
    
2.  Klicken Sie oben in der aktuellen Sicherheitsbewertung auf **Jetzt aktualisieren**, um die neuesten Daten abzurufen.
    
    Der Bereich **Jetzt aktualisieren** wird angezeigt.
    
3.  Geben Sie im Feld **Letzte Bewertung speichern** die Option **Meine Sicherheitsbewertung** ein, und klicken Sie auf **Jetzt aktualisieren**. Warten Sie, bis der Status **Erfolgreich** lautet.
    
    *   Mit dieser Aktion werden die Daten in der letzten Sicherheitsbewertung für die Zieldatenbank aktualisiert. Außerdem wird eine Kopie der Bewertung (mit dem Namen "Meine Sicherheitsbewertung") in der Bewertungshistorie gespeichert.
    *   Der Aktualisierungsvorgang dauert etwa eine Minute.
    
    ![Bereich "Jetzt aktualisieren" für Sicherheitsbewertung](images/sa-refresh-now-panel.png "Bereich "Jetzt aktualisieren" für Sicherheitsbewertung")
    
4.  Klicken Sie auf die Registerkarte **Testinformationen**. Beachten Sie, dass das Bewertungsdatum und die Bewertungszeit gerade liegen und **Entspricht Baseline** gleich **Nein** ist.
    
    ![Sicherheitsbewertung aktuell bewertet am](images/sa-assessed-on-right-now.png "Sicherheitsbewertung aktuell bewertet am")
    
5.  Scrollen Sie nach unten, und blenden Sie **Systemberechtigungen für Public erteilt** ein.
    
    *   Dies ist ein hohes Risiko.
    *   Im Abschnitt **Details** wird angezeigt, dass die Berechtigung, die Sie in der vorherigen Aufgabe erteilt haben, identifiziert wurde.
    
    ![Systemberechtigungen, die PUBLIC-Ergebnissen erteilt werden](images/system-privileges-granted-to-public.png "Systemberechtigungen, die PUBLIC-Ergebnissen erteilt werden")
    

## Aufgabe 7: Vergleichsbericht für Sicherheitsbewertung generieren

1.  Wenn die neueste Sicherheitsbewertung angezeigt wird, klicken Sie links unter **Ressourcen** auf **Mit Baseline vergleichen**. Oracle Data Safe beginnt automatisch mit der Verarbeitung des Vergleichs.
    
    Wenn Sie die letzte Sicherheitsbewertung verlassen haben, können Sie wie folgt zur Bewertung zurückkehren: Klicken Sie im Navigationspfad auf **Sicherheitsbewertung**. Klicken Sie auf die Registerkarte **Zielübersicht**. Klicken Sie für die Zieldatenbank auf **Bericht anzeigen**.
    
    ![Option "Mit Baseline vergleichen" unter Ressourcen](images/sa-resources-compare-with-baseline-option.png "Option "Mit Baseline vergleichen" unter Ressourcen")
    
2.  Wenn der Vergleichsvorgang abgeschlossen ist, scrollen Sie auf der Seite nach unten zum Abschnitt **Mit Baseline vergleichen**, und prüfen Sie die Informationen.
    
    *   Prüfen Sie die Anzahl der Ergebnisse pro Risikokategorie für jede Risikoebene. Die Kategorien umfassen **Benutzeraccounts**, **Berechtigungen und Rollen**, **Autorisierungskontrolle**, **Datenverschlüsselung**, **fein granulierte Zugriffskontrolle**, **Auditing** und **Datenbankkonfiguration**.
    *   Sie können feststellen, wo die Änderungen in der Zieldatenbank vorgenommen wurden, indem Sie Zellen anzeigen, die das Wort **Geändert** enthalten. Die Zahl stellt die Gesamtanzahl der neuen, korrigierten und geänderten Risiken in der Zieldatenbank dar.
    *   In der Detailtabelle können Sie die Risikostufe für jedes Ergebnis, die Kategorie, zu der das Ergebnis gehört, den Ergebnisnamen und eine Beschreibung der Änderungen in der Zieldatenbank anzeigen. Die Spalte "Vergleichsbericht" ist wichtig, da sie erläutert, was seit der Generierung des Baselineberichts geändert, hinzugefügt oder aus der Zieldatenbank entfernt wurde.
    *   Beachten Sie die Hinweise unter **`PUBLIC: [ALTER ANY ROLE]` Änderungsdetails (hinzugefügt)** für einige der Ergebnisse. Die Änderungen, die durch die Erteilung der Maskierungsrolle an `DS$ADMIN` vorgenommen wurden, sind ebenfalls enthalten.
    
    ![Sicherheitsbewertungsvergleichsbericht - oberster Teilsektor](images/sa-comparison-report-top.png "Sicherheitsbewertungsvergleichsbericht - oberster Teilsektor") ![Sicherheitsbewertungsvergleich - Bericht unten](images/sa-comparison-report-bottom.png "Sicherheitsbewertungsvergleich - Bericht unten")
    

## Aufgabe 8: Ergebnisse auf hoher Risikostufe auf der Übersichtsseite prüfen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Sicherheitsbewertung**, um zur Überblickseite zurückzukehren. Stellen Sie sicher, dass das Compartment ausgewählt ist. Heben Sie die Auswahl von **Untergeordnete Compartments einbeziehen** auf.
    
2.  Klicken Sie in der Spalte **Risikostufe** auf **Hoch**, um alle Ergebnisse mit hohem Risiko anzuzeigen.
    
    ![Sicherheitsbewertung - Link "Hohes Risiko"](images/sa-high-risk-link.png "Sicherheitsbewertung - Link "Hohes Risiko"")
    
3.  Prüfen Sie auf der Registerkarte **Überblick** das Diagramm **Risiken nach Kategorie**. Sie können den Cursor über die Prozentwerte bewegen, um den Kategorienamen und die Anzahl anzuzeigen.
    
    ![Sicherheitsbewertung - Ergebnisse mit hohem Risiko für alle Zieldatenbanken](images/sa-high-risk-findings-all-targets.png "Sicherheitsbewertung - Ergebnisse mit hohem Risiko für alle Zieldatenbanken")
    
4.  Blenden Sie im Abschnitt **Risikodetails** die Option **Systemberechtigungen, die PUBLIC erteilt wurden** ein.
    
    *   Im Abschnitt **Bemerkungen** wird das Risiko erläutert und erläutert, wie Sie es mindern können.
    *   Im Abschnitt **Zieldatenbanken** werden die Zieldatenbanken aufgeführt, für die das hohe Risiko gilt. Beachten Sie, dass die Zieldatenbank aufgelistet ist.
    
    ![Systemberechtigungen für Sicherheitsbewertung, die der Öffentlichkeit erteilt werden](images/sa-system-privileges-granted-to-public.png "Systemberechtigungen für Sicherheitsbewertung, die der Öffentlichkeit erteilt werden")
    
5.  Klicken Sie auf den Namen der Zieldatenbank, um die Details zum Ergebnis für die Zieldatenbank anzuzeigen.
    
    *   Das Ergebnis umfasst den Namen der Zieldatenbank, die Risikostufe, eine Risikoübersicht, Details zur Zieldatenbank, Anmerkungen zur Erläuterung des Risikos und zur Risikominderung sowie Referenzen.
    *   Im Abschnitt **Übersicht** wird angegeben, wie viele Berechtigungen für `PUBLIC` vorhanden sind.
    *   Im Abschnitt **Details** wird angezeigt, dass **`PUBLIC`** die Berechtigung **`ALTER ANY ROLE`** hat, was Sie in Aufgabe 5 getan haben.
    *   Im Abschnitt **Bemerkungen** wird angegeben, dass **Privilegien, die PUBLIC erteilt wurden, für alle Benutzer verfügbar sind. Dies sollte im Allgemeinen nur wenige, wenn überhaupt, Systemberechtigungen enthalten, da diese nicht von normalen Benutzern benötigt werden, die keine Administratoren sind.**
    *   Im Abschnitt **Referenzen** wird die STIG-Regelnummer (Security Technical Information Guide) angegeben, die **RULE SV-75925R1** lautet.
    
    ![Sicherheitsbewertungssystemberechtigungen für PUBLIC-Details](images/sa-system-privileges-granted-to-public-details.png "Sicherheitsbewertungssystemberechtigungen für PUBLIC-Details")
    
6.  Um die letzte Bewertung für die Zieldatenbank anzuzeigen, scrollen Sie nach unten auf der Seite, und klicken Sie auf den Link **Hier klicken**. Sie kehren zur letzten Sicherheitsbewertung zurück.
    
    ![Klicken Sie auf den Link "Hier", um die neueste Sicherheitsbewertung anzuzeigen](images/sa-click-here-link.png "Klicken Sie auf den Link "Hier", um die neueste Sicherheitsbewertung anzuzeigen")
    

## Aufgabe 9: Zeitplan hinzufügen, um jeden Sonntag um 11:30 Uhr eine Sicherheitsbewertung für die Zieldatenbank zu speichern

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Sicherheitsbewertung**.
    
2.  Klicken Sie links unter **Zugehörige Ressourcen** auf **Zeitpläne**.
    
    Die Seite **Zeitpläne** wird angezeigt.
    
3.  Beachten Sie, dass in der Tabelle bereits ein Zeitplan vorhanden ist. Sein Typ ist der jüngste. Dies ist der Standardzeitplan, der einmal pro Woche automatisch einen Sicherheitsbewertungsjob in der Zieldatenbank ausführt. Sie können es aktualisieren und umbenennen, aber nicht löschen.
    
    ![Standardzeitplan für Sicherheitsbewertung](images/sa-default-schedule.png "Standardzeitplan für Sicherheitsbewertung")
    
4.  Klicken Sie auf **Zeitplan hinzufügen**.
    
    Der Bereich **Plan hinzufügen, um Bewertung zu speichern** wird angezeigt.
    
5.  Wenn das oben auf der Seite angezeigte Compartment nicht Ihres ist, klicken Sie auf **Compartment ändern**, und wählen Sie Ihr Compartment aus.
    
6.  Wählen Sie in der Dropdown-Liste **Zieldatenbank** die Zieldatenbank.
    
7.  Geben Sie im Feld **Planname** **Sonntagssicherheitsbewertung** ein.
    
8.  Wählen Sie in der Dropdown-Liste **Compartment zum Speichern der Bewertungen** Ihr Compartment aus.
    
9.  Wählen Sie in der Dropdown-Liste **Zeitplantyp** die Option **Wöchentlich** aus.
    
10.  Wählen Sie in der Dropdown-Liste **Every** die Option **Sonntag**.
    
11.  Klicken Sie auf das Feld **Zeit**, scrollen Sie nach unten, und wählen Sie **11:30 Uhr** aus. Sie können die Zeit auch manuell eingeben.
    
12.  Klicken Sie auf **Zeitplan hinzufügen**.
    
    ![Plan hinzufügen, um Bewertungen zu speichern (Seite)](images/sa-add-schedule-to-save-an-assessment.png "Plan hinzufügen, um Bewertungen zu speichern (Seite)")
    
    Die Seite **Plandetails** wird angezeigt.
    
13.  Beachten Sie, dass sich der Status beim Erstellen des Zeitplans in **Erfolgreich** ändert. Der Plantyp lautet **SAVED**.
    
    ![Zeitplandetails (Seite)](images/sa-schedule-details-page.png "Zeitplandetails (Seite)")
    

## Aufgabe 10: Historie aller Sicherheitsbewertungen für alle Zieldatenbanken anzeigen

1.  Klicken Sie im Navigationspfad oben auf der Seite auf **Sicherheitsbewertung**.
    
2.  Klicken Sie unter **Zugehörige Ressourcen** auf **Bewertungshistorie**.
    
3.  Wählen Sie links unter **Listengeltungsbereich** Ihr Compartment aus. Deaktivieren Sie optional **Untergeordnete Compartments einbeziehen**.
    
4.  Zeigen Sie die Liste der Sicherheitsbewertungen an.
    
    *   In der Tabelle werden der Name der Zieldatenbank, der Bewertungsname, die Angabe, ob es sich bei der Bewertung um eine Baselinebewertung handelt, das Datum und die Uhrzeit der Erstellung der Bewertung, der Status der Bewertung (z.B. "Erfolgreich") sowie die Anzahl der Risikoergebnisse "Hoch", "Mittel", "Niedrig", "Hinweis" und "Bewerten" angezeigt.
    *   Sie können auf einen Bewertungsnamen klicken, um ihn anzuzeigen.
    *   Sie können auf **Letzte Bewertung speichern unter** klicken und eine Kopie der letzten Bewertung für eine ausgewählte Zieldatenbank erstellen.
    
    ![Bewertungshistorie (Seite)](images/sa-assessment-history-page.png "Bewertungshistorie (Seite)")
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Weitere Informationen

*   [Überblick über Sicherheitsbewertungen](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-030B2A14-272F-49CF-80D2-5559C722E0FF)

## Danksagungen

*   **Autor** - Jody Glover, Consulting User Assistance Developer, Datenbankentwicklung
*   **Zuletzt aktualisiert am/um** - Jody Glover, 8. Juni 2023