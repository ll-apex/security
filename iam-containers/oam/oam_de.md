# Oracle Access Manager-(OAM-)Domain bereitstellen

## Einführung

Diese Übung enthält Informationen zu den Schritten zum Deployment und Ausführen von OAM-Domains in einem Kubernetes-Cluster mit Hilfe von Oracle WebLogic Kubernetes Operator (3.1.0).

_Geschätzte Zeit:_ 30 Minuten

### Über Produkt/Technologie

Oracle Access Management bietet innovative neue Services, die traditionelle Zugriffsverwaltungsfunktionen ergänzen. Es bietet nicht nur Web-SSO mit MFA, grob granulierte Autorisierung und Sitzungsverwaltung, sondern bietet auch standardmäßige SAML-Föderation und OAuth-Funktionen, um einen sicheren Zugriff auf externe Cloud- und mobile Anwendungen zu ermöglichen. Sie kann einfach in Oracle Identity Cloud Service integriert werden, um hybride Zugriffsverwaltungsfunktionen zu unterstützen, mit denen Kunden On-Premise- und Cloud-Anwendungen und -Workloads nahtlos schützen können. Der Kubernetes-Operator von Oracle WebLogic Server unterstützt das Deployment von Oracle Access Management (OAM).

OAM-Domains werden mit dem Modell "Domain auf einem persistenten Volume" unterstützt, bei dem sich das Domain-Home in einem persistenten Volume (PV) befindet.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OAM in der Kubernetes-Umgebung bereitstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren
    *   Übung: Oracle Identity Governance-(OIG-)Domain bereitstellen

## Aufgabe 1: Code-Repository für die Bereitstellung von OAM-Domains einrichten

Die unten aufgeführten Komponenten wurden bereits während des OIG-Deployments (im Verzeichnis /u01/k8siam) installiert. Daher müssen Sie sie nicht neu konfigurieren. Wir können die OAM-Deployment-Skripte direkt in den Kubernetes Operator-Beispielspeicherort kopieren.

*   Oracle WebLogic Server Kubernetes Operator Docker-Image
*   WebLogic Kubernetes-Operator-Quellcode
*   OAM-Deployment-Skripte aus dem FMW-Kubernetes-Repository

1.  Kopieren Sie OAM-Deployment-Skripte in das Beispielverzeichnis von Oracle WebLogic Server Kubernetes Operator.
    
        	<copy>cd /u01/k8siam</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleAccessManagement/kubernetes/create-access-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## Aufgabe 2: Oracle WebLogic Server Kubernetes Operator installieren

1.  Führen Sie den folgenden Befehl aus, um einen Namespace für den Operator zu erstellen:
    
        	<copy>kubectl create namespace opns</copy>
        
2.  Erstellen Sie einen Serviceaccount für den Operator im Namespace des Operators, indem Sie den folgenden Befehl ausführen:
    
        	<copy>kubectl create serviceaccount -n opns op-sa</copy>
        
3.  Führen Sie den folgenden helm-Befehl aus, um den Operator zu installieren und zu starten:
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator \
        	--namespace opns --set image=weblogic-kubernetes-operator:3.1.0 \
        	--set serviceAccount=op-sa --set "domainNamespaces={}" --set "javaLoggingLevel=FINE" --wait</copy>
        
    
    ![Operatorpod installieren](images/1-helm.png)
    
4.  Prüfen Sie, ob Pod und Services des Operators ausgeführt werden
    
        	<copy>kubectl get all -n opns</copy>
        
    
    ![Operatorpod](images/2-kube.png)
    

## Aufgabe 3: RCU-Schemaerstellung

1.  Erstellen Sie einen Namespace für die Domain:
    
        	<copy>kubectl create namespace accessns</copy>
        
2.  Helper-Pod zum Ausführen von RCU erstellen:
    
        	<copy>kubectl run helperoam --image oracle/oam:12.2.1.4.0 -n accessns -- sleep infinity</copy>
        
3.  Prüfen Sie, ob der Pod ausgeführt wird:
    
        	<copy>kubectl get pods -n accessns</copy>
        
4.  Starten Sie eine bash-Shell im Helper-Pod:
    
        	<copy>kubectl exec -it helperoam -n accessns -- /bin/bash</copy>
        
5.  Ersetzen Sie _`<PRIVATE_IP>`_ durch die private IP der Instanz (siehe Übung 1), und führen Sie die folgenden Befehle aus, um die Umgebung festzulegen:
    
        	<copy>export CONNECTION_STRING=<PRIVATE_IP>:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt</copy>
        
    
    Beispiel:
    
        	export CONNECTION_STRING=11.0.2.64:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt
        
6.  Erstellen Sie die RCU-Schemas in der Datenbank. Dies kann etwa 3-4 Minuten dauern.
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$CONNECTION_STRING -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component MDS -component IAU \
        	-component IAU_APPEND -component IAU_VIEWER -component OPSS -component WLS -component STB -component OAM -f < /tmp/pwd.txt</copy>
        
    
    ![RCU-Schema erstellen](images/3-rcu.png)
    
7.  Helper-bash-Shell beenden
    
        	<copy>exit</copy>
        

## Aufgabe 4: Umgebung für die Domainerstellung vorbereiten

1.  Operator für den Domain-Namespace konfigurieren
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm upgrade --reuse-values --namespace opns --set "domainNamespaces={accessns}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  Kubernetes-Secrets für die Domain erstellen
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials</copy>
        
    
        	<copy>./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n accessns -d accessinfra -s accessinfra-domain-credentials</copy>
        
3.  Prüfen Sie, ob das Domain Secret erstellt wurde
    
        	<copy>kubectl get secret accessinfra-domain-credentials -o yaml -n accessns</copy>
        
4.  Kubernetes-Secrets für die RCU erstellen
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials</copy>
        
    
        	<copy>./create-rcu-credentials.sh -u OAMK8S -p Welcom#123 -a sys -q Welcom#123 -d accessinfra -n accessns -s accessinfra-rcu-credentials</copy>
        
5.  Prüfen Sie, ob das RCU-Secret erstellt wurde
    
        	<copy>kubectl get secret accessinfra-rcu-credentials -o yaml -n accessns</copy>
        

## Aufgabe 5: Persistentes Kubernetes-Volume und persistenten Volume Claim erstellen

1.  Erstellen Sie das erforderliche Verzeichnis für das persistente Volume.
    
        	<copy>mkdir -p /u01/domains/accessdomainpv</copy>
        
    
        	<copy>chmod 777 /u01/domains/accessdomainpv</copy>
        
2.  Erstellen Sie eine Sicherungskopie der Datei create-pv-PVC-inputs.yaml, und führen Sie das Skript aus, um die PV- und PVC-Konfigurationsdateien zu erstellen:
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml_OIG</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-pv-pvc-inputs.yaml .</copy>
        
    
        	<copy>./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output_access</copy>
        
3.  Führen Sie den folgenden Befehl aus, um anzuzeigen, dass die Dateien erstellt wurden:
    
        	<copy>ls output_access/pv-pvcs</copy>
        
4.  Führen Sie den folgenden kubectl-Befehl aus, um PV und PVC im Domain-Namespace zu erstellen:
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pv.yaml -n accessns</copy>
        
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pvc.yaml -n accessns</copy>
        
5.  Prüfen Sie das erstellte PV und PVC
    
        	<copy>kubectl describe pv accessinfra-domain-pv</copy>
        
    
        	<copy>kubectl describe pvc accessinfra-domain-pvc -n accessns</copy>
        
    
    ![Persistentes Volume prüfen](images/4-pv.png)
    

## Aufgabe 6: OAM-Domain-Namespaces erstellen

1.  Erstellen des Domain-Skripts vorbereiten
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv</copy>
        
    
        	<copy>cp create-domain-inputs.yaml create-domain-inputs.yaml.orig</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-domain-inputs.yaml .</copy>
        
2.  Bearbeiten Sie die Datei _create-domain-inputs.yaml_, um die private IP der Instanz im Parameter _rcuDatabaseURL_ zu aktualisieren. Der Parameter _rcuDatabaseURL_ befindet sich am Ende der Datei _create-domain-inputs.yaml_. Navigieren Sie zum Ende der Datei, und aktualisieren Sie den Parameter auf die private IP der Instanz.
    
        	<copy>vi create-domain-inputs.yaml</copy>
        
    
    ![Parameter rcuDatabaseURL aktualisieren](images/13-rcu.png)
    

## Aufgabe 7: Domainerstellungsskript ausführen, um domänenbezogene Kubernetes-Artefakte zu generieren

1.  Führen Sie das Skript zum Erstellen der Domain aus, und geben Sie die Eingabedatei und ein Ausgabeverzeichnis zum Speichern der generierten Artefakte an:
    
        	<copy>./create-domain.sh -i create-domain-inputs.yaml -o output_access</copy>
        

## Aufgabe 8: Kubernetes-Ressource erstellen

1.  Erstellen Sie die Kubernetes-Ressource mit dem folgenden Befehl:
    
        	<copy>cd  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv/output_access/weblogic-domains/accessinfra</copy>
        
    
        	<copy>kubectl apply -f domain.yaml</copy>
        

## Aufgabe 9: Domain prüfen

1.  Führen Sie den folgenden Befehl aus, um den Status der OAM-Pods anzuzeigen.
    
        	<copy>kubectl get pods -n accessns</copy>
        
    
    ![OAM-Pods](images/6-pods.png)
    
2.  Es dauert etwa 10-15 Minuten, bis alle oben aufgeführten Dienste angezeigt werden. Wenn ein Pod den STATUS 0/1 hat, wird der Pod gestartet, aber der zugehörige OAM-Server wird gerade gestartet. Während die Pods gestartet werden, können Sie den Startstatus in den Podlogs prüfen, indem Sie den folgenden Befehl ausführen:
    
        	<copy>kubectl logs accessinfra-adminserver -n accessns</copy>
        
    
        	<copy>kubectl logs accessinfra-oam-server1 -n accessns</copy>
        
    
    Die vom Skript erstellte Standarddomain weist die folgenden Eigenschaften auf:
    
    Ein Administrationsserver namens AdminServer, der auf Port 9001 horcht.
    
    Ein konfiguriertes OAM-Cluster mit dem Namen oam\_cluster der Größe 3.
    
    Ein konfiguriertes Policy Manager-Cluster mit dem Namen policy\_cluster der Größe 3.
    
    Einer startete den OAM Managed Server namens oam\_server1, der auf Port 14100 horcht.
    
    Einer startete den Policy Manager Managed Server oam-policy-mgr1 und horchte auf Port 15100.
    
    Logdateien befinden sich in /u01/domains/accessdomainpv/logs/accessinfra
    
3.  Beschreiben Sie die Domain:
    
        	<copy>kubectl describe domain accessinfra -n accessns</copy>
        
4.  Prüfen Sie, ob Domain, Serverpods und Services erstellt wurden und den Status READY mit dem Status 1/1 aufweisen
    
        	<copy>kubectl get all,domains -n accessns</copy>
        
    
    ![OAM-Pods](images/7-pods.png)
    
5.  Prüfen Sie die Pods. Kopieren Sie die Pod-IP-Adresse des Admin-Servers
    
        	<copy>kubectl get pods -n accessns -owide</copy>
        
    
    Beispiel:
    
    ![OAM-Pods](images/12-pods.png)
    
6.  Öffnen Sie ein Browserfenster, um über die Admin-Server-Pod-IP mit der folgenden URL auf die WebLogic-Konsole zuzugreifen.
    
        	<copy><ADMIN_IP>:9001/console</copy>
        
    
    wobei _`<ADMIN_IP>`_ die Admin-Server-Pod-IP ist
    
    ![Administrationskonsole](images/8-vnc.png)
    
    Melden Sie sich mit den folgenden Zugangsdaten bei der Konsole an:
    
    Benutzername:
    
        	<copy>weblogic</copy>
        
    
    Passwort:
    
        	<copy>Welcom@123</copy>
        
    
    Klicken Sie unter _Umgebung_ auf "Server", und prüfen Sie, ob der OAM-Server ausgeführt wird.
    
    ![Administrationskonsole](images/11-vnc.png)
    

_Hinweis: Der OAM Managed Server kann aufgrund einer begrenzten Heap-Größe im Warnungsstatus angezeigt werden. Die Heap-Größe kann durch Aktualisieren der generierten Domain-YAML-Dateien erweitert werden._

8.  Öffnen Sie eine Browserregisterkarte, um über die Admin-Server-Pod-IP mit der folgenden URL auf die OAM-Konsole zuzugreifen.
    
        	<copy><ADMIN_IP>:9001/oamconsole</copy>
        
    
    wobei _`<ADMIN_IP>`_ die Admin-Server-Pod-IP ist
    
    ![OAM-Konsole](images/9-vnc.png)
    
    Melden Sie sich mit den folgenden Zugangsdaten bei der Konsole an:
    
    Benutzername:
    
        	<copy>weblogic</copy>
        
    
    Passwort:
    
        	<copy>Welcom@123</copy>
        
    
    ![OAM-Konsole](images/10-vnc.png)
    

Sie können jetzt mit der nächsten Übung fortfahren.

## Weitere Informationen

*   [Referenz für Oracle Access Management auf Docker und Kubernetes](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Januar 2022