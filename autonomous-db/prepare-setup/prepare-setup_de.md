# Umgebung vorbereiten

## Einführung

In dieser Übung werden Sie durch die Schritte geführt, mit denen Sie mit der Verwendung von Oracle Autonomous Database beginnen und diese zur Verwendung von DB Vault initialisieren können.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Umgebung vorbereiten](videohub:1_3krv0mxe)

### Ziele

*   Erfahren Sie, wie Sie eine neue Autonomous Database bereitstellen
*   Eigentümer erstellen und Dataset laden, um die Übung durchzuführen

## Aufgabe 1: Autonomous Database bereitstellen

**Hinweis:** Wenn Sie eine vorhandene Autonomous Database in Ihrem eigenen Mandanten verwenden möchten oder eine von Oracle bereitgestellte Umgebung verwenden, können Sie diesen Schritt überspringen.

1.  Bei Oracle Cloud Infrastructure anmelden
    
2.  Sobald Sie angemeldet sind, gelangen Sie zum Cloud-Service-Dashboard, in dem Sie alle für Sie verfügbaren Services anzeigen können. Klicken Sie oben links auf das Navigationsmenü, um Navigationsoptionen der obersten Ebene anzuzeigen.
    
    **Hinweis:** Sie können auch direkt auf Ihren Autonomous Database-Service im Abschnitt **Schnellaktionen** des Dashboards zugreifen.
    
    ![](./images/adb-set_001.png " ")
    
3.  Die folgenden Schritte gelten gleichermaßen für Autonomous Data Warehouse (ADW) oder Autonomous Transaction Processing (ATP). Klicken Sie also **auf das Provisioning von Autonomous Database Ihrer Wahl** (hier wählen wir ein Oracle Autonomous Data Warehouse aus, aber Sie können auch Oracle Autonomous Transaction Processing auswählen, wenn Sie möchten).
    
    ![](./images/adb-set_002.png " ")
    
4.  Wählen Sie in der Dropdownliste **Compartment** Ihr Compartment aus.
    

**Hinweis:** - Diese Konsole zeigt an, dass noch keine Datenbanken vorhanden sind. Wenn eine lange Liste von Datenbanken vorhanden ist, können Sie die Liste nach dem **Status** der Datenbanken filtern (Verfügbar, Gestoppt, Beendet usw.). Sie können auch nach **Workload-Typ** sortieren (hier haben wir **Alle** ausgewählt)

         ![](./images/adb-set_003.png " ")
    

5.  Klicken Sie auf \[**Autonomous Database erstellen**\]
    
    ![](./images/adb-set_004.png " ")
    
6.  Geben Sie auf der Seite **Autonomous Database erstellen** grundlegende Informationen für die Datenbank an:
    
    *   **Compartment** - Wählen Sie gegebenenfalls Ihr Compartment aus
        
    *   **Anzeigename** - Geben Sie zu Anzeigezwecken einen wiederkehrenden Namen für die Datenbank ein. Verwenden Sie in dieser Übung _`ADB Security`_
        
    *   **Datenbankname** - Geben Sie _`ADBSEC01`_ ein. Es ist wichtig, nur Buchstaben und Zahlen zu verwenden, die mit einem Buchstaben beginnen (die maximale Länge beträgt 14 Zeichen und Unterstriche werden nicht unterstützt).
        
    *   **Workload Type** - Wählen Sie den Typ Ihrer Autonomous Database-Instanz entsprechend Schritt 3 oben aus (hier wählen Sie "Data Warehouse")
        
    *   **Deployment Type**: Lassen Sie _`Shared Infrastructure`_ ausgewählt.
        
        ![](./images/adb-set_005.png " ")
        
7.  Konfigurieren Sie die Datenbank:
    
    *   **Always Free**: Wählen Sie diese Option aus, indem Sie den Schieberegler nach rechts verschieben.
        
    *   **Datenbankversion** - Lassen Sie _`19c`_ ausgewählt
        
    *   **OCPU-Anzahl**: Wählen Sie _`1`_ aus.
        
    *   **Speicher** - Lassen Sie _`1`_ aktiviert
        
    *   **Autoscaling** - Lassen Sie dieses Kontrollkästchen aktiviert
        
        **Hinweis:** Autonomous Database vom Typ "Immer kostenlos" kann nicht vertikal oder horizontal skaliert werden
        
8.  Administratorzugangsdaten erstellen:
    
    *   **Kennwort** und **Kennwort bestätigen**: Geben Sie ein Kennwort für den ADMIN-Datenbankbenutzer an, und notieren Sie es. Das Kennwort muss zwischen 12 und 30 Zeichen lang sein und mindestens einen Großbuchstaben, einen Kleinbuchstaben und ein numerisches Zeichen enthalten. Sie kann Ihren Benutzernamen oder das doppelte Anführungszeichen (") nicht enthalten. Beispiel: _`WElcome_123#`_
        
            <copy>WElcome_123#</copy>
            
        
        ![](./images/adb-set_007.png " ")
        
9.  Wählen Sie den Netzwerkzugriff und den Lizenztyp aus:
    
    *   **Netzwerkzugriff** - Lassen Sie _`Allow secure access from everywhere`_ aktiviert
        
    *   **Lizenztyp** - Wählen Sie _`License Included`_ aus.
        
        ![](./images/adb-set_008.png " ")
        
10.  Klicken Sie auf \[**Autonomous Database erstellen**\]
    
11.  Ihre Instanz beginnt mit dem Provisioning. In wenigen Minuten wechselt der Status von Provisioning zu Verfügbar. Jetzt ist Ihre Autonomous Database einsatzbereit! Sehen Sie sich hier die Details Ihrer Instanz an, einschließlich Name, Datenbankversion, OCPU-Anzahl und Speichergröße.
    

    ![](./images/adb-set_009.png " ")
    

## Aufgabe 2: Anwendungsschema und Benutzer einrichten

Obwohl Sie mit lokalen PC-Desktoptools wie Oracle SQL Developer eine Verbindung zu Autonomous Database herstellen können, können Sie das browserbasierte SQL Worksheet direkt von Oracle Autonomous Data Warehouse oder Oracle Autonomous Transaction Processing aus aufrufen.

1.  Klicken Sie auf der Detailseite der Datenbank "`ADB Security`" auf die Registerkarte **Extras**.
    
    ![](./images/adb-set_010.png " ")
    
2.  Auf der Seite "Tools" können Sie auf Datenbankadministration und Entwicklertools für Autonomous Database zugreifen: Database Actions, Oracle Application Express, Oracle ML User Administration und SODA-Treiber. Klicken Sie im Feld "Datenbankaktionen" auf \[**Datenbankaktionen öffnen**\]
    
    ![](./images/adb-set_011.png " ")
    
3.  Für Database Actions wird eine Anmeldeseite geöffnet. Verwenden Sie für diese Übung einfach den Standardadministratoraccount der Datenbankinstanz, Benutzername "_`admin`_", und klicken Sie auf \[**Weiter**\]
    
    ![](./images/adb-set_012.png " ")
    
4.  Geben Sie das Admin-Kennwort ein, das Sie beim Erstellen der Datenbank angegeben haben, hier _`WElcome_123#`_
    
        <copy>WElcome_123#</copy>
        
    
    ![](./images/adb-set_013.png " ")
    
5.  Klicken Sie auf \[**Anmelden**\]
    
6.  Die Seite "Datenbankaktionen" wird geöffnet. Klicken Sie im Feld "Entwicklung" auf \[**SQL**\]
    
    ![](./images/adb-set_014.png " ")
    

**Hinweis:** Beim ersten Öffnen von SQL Worksheet werden die Hauptfeatures in einer Reihe von Popup-Informationsfeldern vorgestellt. Klicken Sie auf \[**Weiter**\], um eine Tour durch die Informationsfelder zu machen

7.  Kopieren Sie die folgenden SQL-Abfragen, und fügen Sie sie in SQL Worksheet ein
    
    *   So erstellen Sie das Arbeitsschema
        
            <copy>
            -- Create SH1 schema
            CREATE USER sh1 IDENTIFIED BY WElcome_123#;
            GRANT CREATE SESSION, CREATE TABLE TO sh1;
            GRANT UNLIMITED TABLESPACE TO sh1;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sh1'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sh1'), p_auto_rest_auth => TRUE);
            END;
            /
            CREATE TABLE sh1.customers AS SELECT * FROM sh.customers;
            CREATE TABLE sh1.countries AS SELECT * FROM sh.countries;
            
            </copy>
            
    *   So erstellen Sie die aktiven Benutzer
        
            <copy>
            -- Create DBA_DEBRA user
            CREATE USER dba_debra IDENTIFIED BY WElcome_123#;
            GRANT PDB_DBA TO dba_debra;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('dba_debra'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('dba_debra'), p_auto_rest_auth => TRUE);
            END;
            /
            -- Create APPUSER user
            CREATE USER appuser IDENTIFIED BY WElcome_123#;
            GRANT CREATE SESSION, READ ANY TABLE TO appuser;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('appuser'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('appuser'), p_auto_rest_auth => TRUE);
            END;
            /
            
            </copy>
            
    *   Drücken Sie \[**F5**\], oder klicken Sie auf das Symbol "Run Scripts".
        
        ![](./images/adb-set_015.png " ") ![](./images/adb-set_016.png " ")
        
    *   Prüfen, ob keine Fehler vorhanden sind
        
8.  **Ihre Umgebung ist einsatzbereit!** Sie können jetzt mit der nächsten Übung fortfahren
    

## Sie möchten mehr erfahren?

Klicken Sie auf [Autonomer Workflow](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/user/autonomous-workflow.html#GUID-5780368D-6D40-475C-8DEB-DBA14BA675C3), um die Dokumentation zum typischen Workflow für die Verwendung von Autonomous Data Warehouse anzuzeigen.

## Danksagungen

*   **Autor** - Hakim Loumi, PM für Datenbanksicherheit
*   **Mitwirkende** - Rene Fontcha
*   **Zuletzt aktualisiert am/um** - Hakim Loumi, Datenbanksicherheit PM - September 2021