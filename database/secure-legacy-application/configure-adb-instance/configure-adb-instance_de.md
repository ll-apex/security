# Autonomous Database-Instanz konfigurieren

## Einführung

In dieser Übung untersuchen wir Oracle Cloud Platform (OCI) und konfigurieren die Autonomous Transaction Processing-Datenbankinstanz (ATP).

Weitere Informationen zur Autonomous Transaction Processing-Datenbank finden Sie [hier](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/).

### Ziele

In dieser Übung führen Sie die folgenden Aufgaben aus:

*   ATP-Datenbankinstanz erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud Infrastructure-(OCI-)Mandantenaccount

## Aufgabe 1: ATP-Datenbankinstanz erstellen

1.  Navigieren Sie bei geöffneter OCI zum ATP-Portal, indem Sie das Hamburger-Menü in der oberen linken Ecke auswählen. So können Sie **Oracle Database** und dann **Autonomous Transaction Processing** auswählen.
    
    ![Verfügbarkeitsprüfung im OCI-Menü auswählen](images/select-atp-menu.png)
    
2.  Wählen Sie **Autonomous Database erstellen** aus.
    
    ![Wählen Sie "Create Autonomous Database" aus.](images/create-autonomous-database.png)
    
3.  Verwenden Sie ein Compartment Ihrer Wahl, und geben Sie den Anzeigenamen und den Datenbanknamen **MyHRAppDB** ein.
    
    ![Datenbanknamen eingeben](images/myhrapp-db-name.png)
    
4.  Erstellen Sie ein Kennwort für Admin-Zugangsdaten.
    
    ![Administratorzugangsdaten eingeben](images/atp-password.png)
    
5.  Ändern Sie den Netzwerkzugriff auf **nur zulässige IPs und VCNs**, und ändern Sie den IP-Notationstyp in **CIDR-Block. Geben Sie den CIDR-Wert 0.0.0.0/0 in das leere Feld ein.** Stellen Sie sicher, dass die Option **Gegenseitige TLS-(mTLS-)Authentifizierung erforderlich bleibt** nicht aktiviert ist.
    
    ![Administratorzugangsdaten eingeben](images/secure-access.png)
    
6.  Wählen Sie **Lizenz enthalten** und dann unten die Option **Autonomous Database erstellen** aus.
    
    ![Schaltfläche "DB erstellen" unten](images/create-atp.png)
    
7.  Navigieren Sie zurück zur erstellten Verfügbarkeitsinstanz. Wählen Sie oben auf der Seite **DB-Verbindung** aus.
    
    ![Zu DB-Verbindung navigieren](images/db-connection.png)
    
8.  Scrollen Sie nach unten zum Abschnitt **Verbindungszeichenfolgen** des Menüs. Stellen Sie unter "TLS-Authentifizierung" sicher, dass **TLS** und nicht **Gegenseitige TLS** ausgewählt ist. Kopieren Sie dann die erste Verbindungszeichenfolge in eine Zwischenablage Ihrer Wahl. In den folgenden Schritten speichern Sie diese Zeichenfolge an einem bestimmten Speicherort.
    
    ![Verbindungszeichenfolge kopieren](images/copy-connection-string.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Ethan Shmargad, North America Specialists Hub
*   **Ersteller** - Richard Evans, Senior Principle Product Manager
*   **Zuletzt aktualisiert am/um** - Ethan Shmargad, September 2022