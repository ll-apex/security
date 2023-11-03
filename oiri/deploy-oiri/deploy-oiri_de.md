# OIRI auf dem lokalen Kubernetes-Knoten bereitstellen

## Einführung

In dieser Übung werden die Schritte zur Konfiguration von Oracle Identity Role Intelligence erläutert. Dazu gehören das Einrichten der Konfigurationsdateien, das Erstellen des Wallets und die Installation des Helm-Diagramms.

_Geschätzte Laborzeit_: 40 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OIRI-Docker-Bilder laden
*   Kubernetes-Konfigurationsdateien einrichten
*   OIRI und Ding Wallets erstellen
*   Datenbankschema erstellen und vordefinieren
*   OIRI Helm-Diagramm installieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Kubernetes-Cluster bereitstellen und OIG-Server starten

## Aufgabe 1: Laden der OIRI-Dockerbilder

Der OIRI-Dienst besteht aus vier Bildern wie folgt:

*   OIRI: OIRI-Service
*   OIRI-cli: OIRI-Befehlszeilenschnittstelle
*   oiri-ding: Für Datenimport
*   oiri-ui: Identity Role Intelligence-Benutzeroberfläche

Führen Sie die folgenden Schritte aus, um diese Docking-Bilder zu laden.

1.  Öffnen Sie eine Terminalsession, und laden Sie die OIRI-Dockerbilder.
    
        <copy>cd /u01/setup/oiri/oiri-12.2.1.4.210423</copy>
        
    
        <copy>docker load --input oiri-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ui-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-cli-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ding-12.2.1.4.210423.tar</copy>
        
    
    ![Terminalbefehle zum Laden der OIRI-Dockerbilder](images/load-docker-images.png)
    
    _Hinweis_:
    
    Sie können die Docker-Images auf eine der folgenden Arten laden:
    
    *   [OIRI-Docker-Images aus der Shared Zip-Datei verwenden](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-174D3A93-752E-4E5A-AF3F-0648E87BC0F0)
    *   \[Docker-Images aus der Container Registry verwenden\] (https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-B23F0B59-AF19-4C7F-A268-8B6F3B5FC6B0)
2.  Prüfen Sie die Bilder.
    
        <copy>docker images | grep 12.2.1.4.210423</copy>
        
    
    ![Bilder überprüfen](images/verify-docker-images.png)
    

## Aufgabe 2: Konfigurationsdateien einrichten

Richten Sie die Dateien ein, die für die Konfiguration von Datenimport (oder Datenaufnahme) und Helm-Diagramm erforderlich sind.

1.  Erstellen Sie die Verzeichnisse auf NFS. NFS ist eine Prequiste und wird zum Erstellen persistenter Volumes für die Verwendung auf allen Knoten verwendet.
    
        <copy>mkdir /nfs/ding</copy>
        
    
        <copy>mkdir /nfs/oiri</copy>
        
    
    Erstellen Sie das folgende Verzeichnis zum Generieren der Datei values.yaml, die vom Helm-Diagramm verwendet werden soll. Dieses Verzeichnis muss nicht auf dem NFS vorhanden sein, da es nur zum Speichern der Datei values.yaml verwendet wird, die nicht im Cluster freigegeben werden muss.
    
        <copy>mkdir /u01/k8s/</copy>
        
    
    Stellen Sie die Schreibberechtigungen für die erstellten Verzeichnisse sicher.
    
        <copy>chmod -R 775 /nfs/ding/</copy>
        
    
        <copy>chmod -R 775 /nfs/oiri/</copy>
        
    
        <copy>chmod -R 775 /u01/k8s/</copy>
        
2.  Notieren Sie sich die private IP der Instanz, wie in der hosts-Datei angegeben.
    
        <copy>vi /etc/hosts</copy>
        

Beispiel:

    ![Private IP of your instance](images/ip.png)
    

3.  Führen Sie den Container _oiri-cli_ aus.
    
        <copy>docker run -d --name oiri-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
\--group-add 54321  
oiri-cli:12.2.1.4.210423  
tail -f /dev/null \`\`\`

    Notice *--group-add 54321* where, 54321 is the `GROUP_ID`.
    `GROUP_ID` is the ID of the group having access to the volumes.
    

4.  Kopieren Sie den Kube-Konfigurationsinhalt aus dem Kubernetes-Cluster. Kopieren Sie den Inhalt der Datei _/home/oracle/.kube/config_ in ein Notizbuch oder eine Zwischenablage. Verkleinern und kopieren Sie alle Zeilen in der Datei.
    
        <copy>vi /home/oracle/.kube/config</copy>
        
    
    ![Kube-Konfigurationsinhalt aus dem Kubernetes-Cluster kopieren](images/config.png)
    
5.  Gehen Sie zum Container _oiri-cli_.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
6.  Erstellen Sie die Kube-Konfigurationsdatei. Fügen Sie den aus der Datei _/home/oracle/.kube/config_ kopierten Inhalt ein, und speichern Sie die Datei.
    
        <copy>vi /app/k8s/config</copy>
        
    
        <copy>chmod 400 /app/k8s/config</copy>
        
    
    ![Erstellen Sie die Kube-Konfigurationsdatei, und fügen Sie den kopierten Inhalt ein](images/kube-config.png)
    
7.  Prüfen Sie die kubectl- und helm-Version. Stellen Sie sicher, dass die Befehle _helm version_ und _kubectl version_ ohne Warnung oder Fehler erfolgreich ausgeführt werden, und zeigen Sie die Version an.
    
        <copy>kubectl version</copy>
        
    
        <copy>helm version</copy>
        
    
    ![kubectl und helm-Version prüfen](images/kube-version.png)
    

_Hinweis: Wenn ein Fehler auftritt, kann dies bedeuten, dass der Inhalt der Kube-Konfigurationsdatei nicht korrekt kopiert wird. Kopieren Sie den Inhalt der Kube-Konfigurationsdatei gemäß den vorherigen Schritten 4-6, und fügen Sie ihn ein, und versuchen Sie es erneut_

8.  Konfigurationsdateien einrichten Ersetzen Sie den Parameter `<VM IP>` durch die private IP-Adresse der Instanz, die in Schritt 2 aus der Datei _/etc/hosts_ angegeben wurde.
    
        <copy>./setupConfFiles.sh -m prod \
        

\--oigdbhost \--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost \--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://:6443  
\--oigserverurl http://:1400\`

Beispiel:

    ```
    

./setupConfFiles.sh -m prod  
\--oigdbhost 10.0.0.231  
\--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost 10.0.0.231  
\--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://10.0.0.231:6443  
\--oigserverurl http://10.0.0.231:14000\`\`\`\`

    ![Terminal commands to set up configuration files](images/setup.png)
    

9.  Prüfen Sie, ob die Konfigurationsdateien generiert wurden.
    
        <copy>ls /app/data/conf/</copy>
        
    
        <copy>ls /app/oiri/data/conf</copy>
        
10.  Richten Sie die Datei values.yaml ein, die für das Helm-Diagramm verwendet werden soll. Ersetzen Sie den Parameter `<VM IP>` durch die private IP-Adresse der Instanz, die in Schritt 2 aus der Datei _/etc/hosts_ angegeben wurde.
    
        <copy>./setupValuesYaml.sh \
        

\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver \--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver \--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingresshostname oiri  
\--sslsecretname "oiri-tls-cert" \`\`\`

Beispiel:

    ```
    

./setupValuesYaml.sh  
\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver 10.0.0.231  
\--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver 10.0.0.231  
\--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingcrethostname oiri  
\-certoir

11.  Prüfen Sie, ob values.yaml generiert wurde, und beenden Sie den Container.
    
        <copy>ls /app/k8s/</copy>
        
    
        <copy>exit</copy>
        

## Aufgabe 3: Entityparameter für Datenimport aktualisieren

1.  Führen Sie den folgenden Befehl aus, um Entityparameter für den Datenimport aktualisieren zu können.
    
        <copy>docker run -d --name ding-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
oiri-ding:12.2.1.4.210423  
tail -f /dev/null \`\`\`

## Aufgabe 4: Wallet-Erstellung

1.  Importieren Sie das OIG-Zertifikat in den Keystore. Exportieren Sie dazu das OIG-Zertifikat zur Signaturprüfung.
    
        <copy>cd /u01/oracle/config/domains/oig_domain/config/fmwconfig/</copy>
        
    
        <copy>keytool -export -rfc -alias xell -file xell.pem -keystore default-keystore.jks</copy>
        

Geben Sie das Keystore-Kennwort ein, wenn Sie dazu aufgefordert werden. `Password: <copy>Welcome1</copy>`

![Terminalbefehle zum Exportieren des OIG-Zertifikats zur Signaturprüfung](images/keystore.png)

_default-keystore.jks_ befindet sich unter _DOMAIN\_HOME/config/fmwconfig_. Das hier exportierte Zertifikat schützt die OIG-REST-API. Sie ist nicht mit dem OIG-Serverzertifikat identisch.

2.  Kopieren Sie die aus dem OIG-Keystore exportierte Datei _xell.pem_ in das Verzeichnis _/nfs/oiri/data/keystore/_.
    
        <copy>sudo cp /u01/oracle/config/domains/oig_domain/config/fmwconfig/xell.pem /nfs/oiri/data/keystore/</copy>
        
    
        <copy>sudo chown opc:users /nfs/oiri/data/keystore/xell.pem</copy>
        
    
        <copy>sudo ls -latr /nfs/oiri/data/keystore</copy>
        
    
    ![Kopieren Sie die aus dem OIG-Keystore exportierte Datei xell.pem in /nfs/oiri/data/keystore/](images/opc.png)
    
3.  Generieren Sie einen Keystore im Container _oiri-cli_.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>keytool -genkeypair \
        

\-alias oirikey  
\-keypass Welcome1  
\-keyalg RSA  
\-keystore /app/oiri/data/keystore/keystore.jks  
\-storetype pkcs12  
\-storepass Welcome1 \`\`\`

Die Ausgabe ist:

    ```
    What is your first and last name?
    [Unknown]:
    What is the name of your organizational unit?
    [Unknown]:
    What is the name of your organization?
    [Unknown]:
    What is the name of your City or Locality?
    [Unknown]:
    What is the name of your State or Province?
    [Unknown]:
    What is the two-letter country code for this unit?
    [Unknown]:
    Is CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown correct?
    [no]:  yes
    ```
    
    ![Terminal commands to Generate a keystore inside the oiri-cli container](images/keytool.png)
    

4.  Importieren Sie das Zertifikat in den OIRI-Keystore.
    
        <copy>keytool -import \
        -alias xell \
        -file /app/oiri/data/keystore/xell.pem \
        -keystore /app/oiri/data/keystore/keystore.jks</copy>
        
    
    Geben Sie auf Aufforderung das Keystore-Kennwort ein.
    
        Password: <copy>Welcome1</copy>
        
    
    Die Ausgabe ist:
    
        Trust this certificate? [no]: yes
        
    
    ![Zertifikat in OIRI-Keystore importieren](images/import-keytool.png)
    
5.  Erstellen Sie das Wallet.
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml wallet create</copy>
        
    
    Geben Sie bei der Aufforderung folgende Informationen ein:
    
    *   OIRI DB UserName-Präfix. Beispiel: Entwicklungs-, Test-, Produktionsumgebung. Schemaname ist \_OIRI: **DEV**
    *   OIRI-DB-Kennwort: **Welcome1**
    *   OIG DB UserName: **DEV\_OIM**
    *   OIG-DB-Kennwort: **Welcome1**
    *   OIG-Servicekonto UserName: **xelsysadm**
    *   Kennwort für den OIG-Serviceaccount: **Welcome1**
    *   OIRI KeyStore-Kennwort: **Welcome1**
    *   OIRI JWT-Schlüsselalias: **oirikey**
    *   Kennwort des OIRI-JWT-Schlüssels: **Welcome1**
    
    ![Wallet erstellen](images/wallet.png)
    
6.  Prüfen Sie, ob die OIRI- und Ding-Wallets erstellt wurden.
    
        <copy>ls /app/data/wallet</copy>
        
    
        <copy>ls /app/oiri/data/wallet</copy>
        
    
    ![Erstellte OIRI- und Ding-Wallets prüfen](images/createdb-wallet.png)
    

## Aufgabe 5: OIRI-Datenbankschema erstellen und vordefinieren

1.  Erstellen Sie das Datenbankbenutzerschema.
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema create /app/data/conf/dbconfig.yaml</copy>
        

Geben Sie auf Aufforderung das SYS-Benutzerkennwort ein.

    ```
    Password: <copy>Welcome1</copy>
    ```
    
    ![Terminal command to Create the database user schema](images/createdb-wallet.png)
    

2.  Datenbankschema vordefinieren
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema migrate /app/data/conf/dbconfig.yaml</copy>
        
3.  Prüfen Sie das Wallet.
    
        <copy>./verifyWallet.sh</copy>
        
    
    ![Terminalbefehl zum Prüfen des Wallets](images/verifywallet.png)
    
    Hinweis: Wenn die Verifizierung des Wallets nicht erfolgreich verläuft, beheben Sie den gemeldeten Eintrag mit dem Befehl _oiri-cli --config=/app/data/conf/config.yaml wallet update_
    

## Aufgabe 6: OIRI-Helm-Diagramm installieren

1.  Erstellen Sie die folgenden Namespaces.
    
        <copy>kubectl create namespace oiri</copy>
        
    
        <copy>kubectl create namespace ding</copy>
        
    
        <copy>exit</copy>
        
2.  Aktivieren Sie SSL von einem Docker-Containerhostrechner außerhalb des oiri-cli-Containers.
    
        <copy>cd ~</copy>
        
    
        <copy>openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=oiri"</copy>
        
    
        <copy>kubectl create secret tls oiri-tls-cert --key="tls.key" --cert="tls.crt"</copy>
        
3.  Installieren Sie das Helm-Diagramm.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>helm install oiri /helm/oiri -f /app/k8s/values.yaml</copy>
        
    
    ![Terminalbefehle zum Installieren des Helm-Diagramms](images/helm.png)
    
4.  Listen Sie die Pods auf, und stellen Sie sicher, dass alle Pods ausgeführt werden. Prüfen Sie außerdem, ob die Pods unter dem Namespace ding und oiri den Status RUNNING und READY 1/1 aufweisen.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
        <copy>kubectl get pods -n ding</copy>
        
    
        <copy>kubectl get pods -n oiri</copy>
        
    
        <copy>exit</copy>
        
    
    ![Pods auflisten](images/running-pods.png)
    
    ![Prüfen Sie, ob die Pods unter dem Namespace ding und oiri den Status RUNNING und READY 1/1 aufweisen](images/ready-pods.png)
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering, Dezember 2022