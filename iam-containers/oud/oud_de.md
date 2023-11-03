# Oracle Unified Directory (OUD) bereitstellen

## Einführung

In dieser Übung werden die Schritte zum Deployment und Ausführen von Oracle Unified Directory 12c PS4 (12.2.1.4.0) in einer Kubernetes-Umgebung beschrieben.

_Geschätzte Zeit:_ 20 Minuten

### Informationen zu <Product/Technology>

Oracle Unified Directory bietet eine umfassende Directory-Lösung für robustes Identity Management. Oracle Unified Directory ist eine All-in-One-Verzeichnislösung mit Speicher-, Proxy-, Synchronisierungs- und Virtualisierungsfunktionen. Sie vereinheitlicht den Ansatz und bietet alle Services, die für leistungsstarke Enterprise- und Carrier-Grade-Umgebungen erforderlich sind. Oracle Unified Directory gewährleistet Skalierbarkeit auf Milliarden von Einträgen, einfache Installation, elastische Deployments, Verwaltbarkeit des Unternehmens und effektive Überwachung.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OUD-Instanz in der Kubernetes-Umgebung erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Oracle Identity Governance-(OIG-)Domain bereitstellen
    *   Übung: Oracle Access Manager-(OAM-)Domain bereitstellen

## Aufgabe 1: Umgebung für Containererstellung vorbereiten

1.  Prüfen, ob das Kubernetes-Cluster bereit ist
    
        	<copy>kubectl get nodes,pods -n kube-system</copy>
        
    
    ![Kubernetes-Cluster](images/1-kube.png)
    
2.  Oracle Unified Directory-Image prüfen
    
        	<copy>docker images | grep oud</copy>
        
3.  Kubernetes-Namespace erstellen
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp oudns.yaml oudns.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oudns.yaml .</copy>
        
    
        	<copy>kubectl apply -f oudns.yaml</copy>
        
4.  Vergewissern Sie sich, dass der Namespace erstellt wurde:
    
        	<copy>kubectl get namespaces </copy>
        
5.  Secrets für Benutzer-IDs und Kennwörter erstellen
    
        	<copy>cp secrets.yaml secrets.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/secrets.yaml .</copy>
        
    
    Diese Datei enthält aktualisiertes Secret, Namespace und andere Parameter mit angegebenen Werten im Format Base64
    
    geheimnis - oudsecret
    
    Namespace - myoudns
    
    %rootUserDN% - Base64-codierter Wert von 'cn=Directory Manager' für Parameter rootUserDN
    
    %rootUserPassword% - Base64-codierter Wert des Parameters "Welcom@123" rootUserPassword
    
    %adminUID% - Base64-codierter Wert von "admin" für Parameter adminUID
    
    %adminPassword% - Base64-codierter Wert von 'Welcom@123' für Parameter adminPassword
    
    %bindDN1% - Base64-codierter Wert des Parameters 'cn=Directory Manager bindDN1
    
    %bindPassword1% - Base64-codierter Wert von 'Welcom@123 für Parameter bindPassword1
    
    %bindDN2% - Base64-codierter Wert von 'cn=Directory Manager für Parameter bindDN2
    
    %bindPassword2% - Base64-codierter Wert von 'Welcom@123' für Parameter bindPassword2
    
6.  Spielen Sie die Datei ein:
    
        	<copy>kubectl apply -f secrets.yaml</copy>
        
7.  Prüfen Sie, ob das Secret erstellt wurde:
    
        	<copy>kubectl --namespace myoudns get secret</copy>
        
8.  Hostverzeichnis für dateisystembasierte PersistentVolume vorbereiten
    
        	<copy>mkdir -p /u01/domains/oud_user_projects</copy>
        
    
        	<copy>chmod 777 /u01/domains/oud_user_projects</copy>
        
    
        	<copy>cp persistent-volume.yaml persistent-volume.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/persistent-volume.yaml .</copy>
        
9.  Spielen Sie die Datei ein:
    
        	<copy>kubectl apply -f persistent-volume.yaml</copy>
        
10.  Prüfen Sie PersistentVolume und PersistentVolumeClaim
    
        <copy>kubectl --namespace myoudns describe persistentvolume oudpv</copy>
        
    
        <copy>kubectl --namespace myoudns describe pvc oudpvc</copy>
        
    
    ![Persistentes Volume prüfen](images/2-pv.png)
    

## Aufgabe 2: Directory Server (instanceType=Directory)

Im Rahmen dieser Übung erstellen wir einen POD (oudpod1), der aus einem einzelnen Container besteht, der auf einem Oracle Unified Directory 12c PS4-(12.2.1.4.0-)Image basiert.

1.  Um den POD zu erstellen, erstellen Sie eine Kopie der Datei oud-dir-pod.yaml, die mit den entsprechenden Werten aktualisiert wird.
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oud-dir-pod.yaml .</copy>
        
    
    Diese Beispieldatei enthält die folgenden aktualisierten Parameter:
    
    Namespace - myoudns
    
    Oracle-Imagetag - oracle/oud:12.2.1.4.0
    
    Geheimname - oudsecret
    
    PV-Name - oudpv
    
    PVC-Name - oudpvc
    
    Anzahl Probeneinträge - 15
    
2.  Spielen Sie die Datei ein.
    
        	<copy>kubectl apply -f oud-dir-pod.yaml</copy>
        
3.  Prüfen Sie den Status des erstellten Pods:
    
        	<copy>kubectl get pods -n myoudns</copy>
        
    
    ![OUD-Pods](images/3-pods.png)
    
    Um die Containerlogs während der Initialisierung zu optimieren, verwenden Sie den folgenden Befehl:
    
        	<copy>kubectl --namespace myoudns logs -f -c oudds1 oudpod1</copy>
        
    
    Drücken Sie Ctrl+C, um die Containerlogs zu beenden
    
4.  Melden Sie sich beim Container an, um zu prüfen, ob die Oracle Unified Directory Directory Server-Instanz ausgeführt wird:
    
        	<copy>kubectl --namespace myoudns exec -it -c oudds1 oudpod1 /bin/bash</copy>
        
5.  Führen Sie im Container ldapsearch aus, um Einträge vom Directory-Server zurückzugeben:
    
        	<copy>cd /u01/oracle/user_projects/oudpod1/OUD/bin</copy>
        
    
        	<copy>./ldapsearch -h localhost -p 1389 -D "cn=Directory Manager" -w Welcom@123 -b "" -s sub "(objectclass=*)" dn</copy>
        
    
    ![Benutzereinträge](images/4-oud.png)
    
        	<copy>exit</copy>
        

## Weitere Informationen

*   [Referenz für Oracle Unified Directory auf Docker und Kubernetes](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## Danksagungen

*   **Autor** - Keerti R, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Januar 2022