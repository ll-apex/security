# Oracle Identity Governance-(OIG-)Domain bereitstellen

## Einführung

Diese Übung enthält Informationen zu den Schritten zum Deployment und Ausführen von OIG-Domain in einem Kubernetes-Cluster mit Hilfe von Oracle WebLogic Kubernetes Operator (3.1.0)

_Geschätzte Zeit:_ 30 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Oracle Identity Governance-(OIG-)Domain bereitstellen](videohub:1_furulwif)

### Über Produkt/Technologie

Oracle Identity Governance (OIG) ist ein leistungsstarkes und flexibles Identity Management-System für Unternehmen, das die Zugriffsberechtigungen von Benutzern innerhalb von Unternehmens-IT-Ressourcen automatisch verwaltet. Der Oracle WebLogic-Kubernetes-Operator unterstützt das Deployment von Oracle Identity Governance (OIG). OIG-Domains werden mit dem Modell "Domain auf einem persistenten Volume" unterstützt, bei dem sich das Domain-Home in einem persistenten Volume (PV) befindet.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   OIG in der Kubernetes-Umgebung bereitstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Docker-Image für Oracle WebLogic Server-Kubernetes-Operator installieren und ausführen

Das Docker-Image des Oracle WebLogic Server Kubernetes-Operators muss auf dem Masterknoten und jedem Worker-Knoten in Ihrem Kubernetes-Cluster installiert sein. Alternativ können Sie das Image in einer Docker-Registry speichern, auf die Ihr Cluster zugreifen kann.

1.  Rufen Sie das Oracle WebLogic Server-Kubernetes-Operatorimage ab, indem Sie den folgenden Befehl auf dem Masterknoten ausführen.
    
        	<copy>docker pull ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0</copy>
        
2.  Führen Sie den docker tag-Befehl wie folgt aus:
    
        	<copy>docker tag ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0 weblogic-kubernetes-operator:3.1.0</copy>
        
3.  Prüfen Sie die Bilder für den WebLogic-Kubernetes-Operator.
    
        	<copy>docker images | grep operator</copy>
        

## Aufgabe 2: Code-Repository für das Deployment von Oracle Identity Governance-Domains einrichten

1.  Erstellen Sie ein Arbeitsverzeichnis, und laden Sie den Oracle WebLogic Kubernetes Operator 3.1.0-Quellcode aus dem Operator-Github-Projekt herunter
    

mkdir -p /u01/k8siam cd /u01/k8siam git clone https://github.com/oracle/weblogic-kubernetes-operator.git --branch release/3.1.0 \`\`

2.  Klonen Sie die Oracle Identity Governance-Deployment-Skripte aus dem OIG-Repository, und kopieren Sie sie in das Beispielverzeichnis des Operators WebLogic
    
        	<copy>git clone https://github.com/oracle/fmw-kubernetes.git</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleIdentityGovernance/kubernetes/create-oim-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## Aufgabe 3: Oracle WebLogic Kubernetes Operator installieren

1.  Führen Sie den folgenden Befehl aus, um einen Namespace für den Operator zu erstellen
    
        	<copy>kubectl create namespace operator</copy>
        
2.  Serviceaccount für den Operator im Namespace des Operators erstellen
    
        	<copy>kubectl create serviceaccount -n operator operator-serviceaccount</copy>
        
3.  Führen Sie den folgenden helm-Befehl aus, um den Operator zu installieren und zu starten:
    
        	<copy>
        

cd weblogic-kubernetes-operator/ helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator  
\--namespace-operator  
\--set image=weblogic-kubernetes-operator:3.1.0  
\--set serviceAccount=operator-serviceaccount  
\--set "domainNamespaces={}" \`\`

    ![Install operator pod](images/7-helm.png)
    

4.  Helm-Installation für den Operator prüfen
    
        	<copy>helm ls -n operator</copy>
        
5.  Prüfen Sie, ob der Pod des Operators mit dem Status 1/1 READY ausgeführt wird
    
        	<copy>kubectl get all -n operator</copy>
        
    
    ![Operator-Podstatus](images/8-operator.png)
    

## Aufgabe 4: RCU-Schemaerstellung

1.  Führen Sie den folgenden Befehl aus, um einen Namespace für die Domain zu erstellen:
    
        	<copy>kubectl create namespace oimcluster</copy>
        
2.  Führen Sie den folgenden Befehl aus, um einen Helper-Pod zu erstellen:
    
        	<copy>kubectl run helper --image oracle/oig:12.2.1.4.0 -n oimcluster -- sleep infinity</copy>
        
3.  Helper-Pod prüfen
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
4.  Starten Sie eine bash-Shell im Helper-Pod. Dadurch gelangen Sie in eine bash-Shell im laufenden rcu-Pod:
    
        	<copy>kubectl exec -it helper -n oimcluster -- /bin/bash</copy>
        
5.  Führen Sie in der Helper-bash-Shell die folgenden Befehle aus, um die Umgebung festzulegen. Ersetzen Sie den Parameter _`<PRIVATE_IP>`_, der aus der vorherigen Übung "Umgebung initialisieren", Aufgabe 2, Schritt 2 (private IP der Instanz) kopiert wurde.
    
        	<copy>
        	export DB_HOST=<PRIVATE_IP>
        	</copy>
        
    
        	<copy>
        	export DB_PORT=1521
        	export DB_SERVICE=orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OIGK8S
        	export RCU_SCHEMA_PWD=Welcom#123
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt
        	export RCU_SCHEMA_PWD=Welcom#123
        	</copy>
        
    
    Beispiel:
    
        	export DB_HOST=11.0.2.64
        

    export DB_PORT=1521
    export DB_SERVICE=orcl.livelabs.oraclevcn.com
    export RCUPREFIX=OIGK8S
    export RCU_SCHEMA_PWD=Welcom#123
    echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
    cat /tmp/pwd.txt
    export RCU_SCHEMA_PWD=Welcom#123
    ```
    

6.  Führen Sie in der Helper-bash-Shell die folgenden Befehle aus, um die RCU-Schemas in der Datenbank zu erstellen:
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$DB_HOST:$DB_PORT/$DB_SERVICE -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component OIM -component MDS -component SOAINFRA -component OPSS \
        	-f < /tmp/pwd.txt</copy>
        
    
    _**Hinweis: Es kann etwa 5-8 Minuten dauern, bis RCU-Schemas erstellt werden.**_
    
    ![RCU-Schemas erstellen](images/9-rcu.png)
    
7.  Helper-bash-Shell beenden
    
        	<copy>exit</copy>
        

## Aufgabe 5: Umgebung für Domainerstellung vorbereiten

1.  Oracle WebLogic-Kubernetes-Operator zur Verwaltung der Domain im Domain-Namespace konfigurieren
    
        	<copy>helm upgrade --reuse-values --namespace operator --set "domainNamespaces={oimcluster}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  Erstellen Sie ein Kubernetes-Secret für die Domain mit dem Skript "create-weblogic-credentials" im selben Kubernetes-Namespace wie die Domain:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials ./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n oimcluster -d oimcluster -s oimcluster-domain-credentials \`\`

    where:
    
    -u weblogic is the WebLogic username
    
    -p <pwd> is the password for the WebLogic user
    
    -n <domain_namespace> is the domain namespace
    
    -d <domain_uid> is the domain UID to be created. The default is domain1 if not specified
    
    -s <kubernetes_domain_secret> is the name you want to create for the secret for this namespace. The default is to use the domainUID if not specified
    

3.  Prüfen Sie, ob das Domain Secret erstellt wurde
    
        	<copy>kubectl get secret oimcluster-domain-credentials -o yaml -n oimcluster</copy>
        
    
    ![Domänen-Secret](images/10-secret.png)
    
4.  Erstellen Sie mit dem Skript create-weblogic-credentials.sh ein Kubernetes-Secret für RCU in demselben Kubernetes-Namespace wie die Domain:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials ./create-rcu-credentials.sh -u OIGK8S -p Welcom#123 -a sys -q Welcom#123 -d oimcluster -n oimcluster -s oimcluster-rcu-credentials \`\`

    where:
    
    -u <rcu_prefix> is the name of the RCU schema prefix created previously
    
    -p <rcu_schema_pwd> is the password for the RCU schema prefix
    
    -q <sys_db_pwd> is the sys database password
    
    -d <domain_uid> is the domain_uid that you created earlier
    
    -n <domain_namespace> is the domain namespace
    
    -s <kubernetes_rcu_secret> is the name of the rcu secret to create
    

5.  Prüfen Sie, ob das RCU-Secret erstellt wurde
    
        	<copy>kubectl get secret oimcluster-rcu-credentials -o yaml -n oimcluster</copy>
        
    
    ![RCU-Secret](images/11-secret.png)
    

## Aufgabe 6: Persistentes Kubernetes-Volume und Persistent Volume Claim erstellen

1.  Erstellen Sie das erforderliche Verzeichnis für das persistente Volume.
    
        	<copy>
        

mkdir -p /u01/domains/oimclusterdomainpv chmod 777 /u01/domains/oimclusterdomainpv \`\`

2.  Erstellen Sie eine Sicherungskopie der Datei create-pv-PVC-inputs.yaml, und führen Sie das Skript create-pv-PVC.sh aus, um die PV- und PVC-Konfigurationsdateien zu erstellen:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-pv-pvc-inputs.yaml. ./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output\_oimcluster \`\`

    ![Create persistent volume](images/12-pv.png)
    

3.  Führen Sie den folgenden Befehl aus, um anzuzeigen, dass die Dateien erstellt wurden:
    
        	<copy>ls output_oimcluster/pv-pvcs</copy>
        
4.  Führen Sie den folgenden kubectl-Befehl aus, um PV und PVC im Domain-Namespace zu erstellen:
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pv.yaml -n oimcluster</copy>
        
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pvc.yaml -n oimcluster</copy>
        
5.  Prüfen Sie das erstellte PV und PVC
    
        	<copy>kubectl describe pv oimcluster-oim-pv -n oimcluster</copy>
        
    
    ![Persistenten Datenträger beschreiben](images/13-pv.png)
    
        	<copy>kubectl describe pvc oimcluster-oim-pvc -n oimcluster</copy>
        
    
    ![Persistenten Datenträger beschreiben](images/14-pv.png)
    

## Aufgabe 7: OIG-Domains erstellen

1.  Erstellen Sie eine Kopie der Datei create-domain-inputs.yaml:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-oim-domain/domain-home-on-pv cp create-domain-inputs.yaml create-domain-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-domain-inputs.yaml. \`\`\`

3.  Bearbeiten Sie den Parameter _rcuDatabaseURL_, um die private IP der Instanz aufzunehmen (siehe vorherige Übung "Umgebung initialisieren", Aufgabe 2, Schritt 2). Alternativ können Sie die private IP auch abrufen, indem Sie den Befehl _cat /etc/hosts_ angeben. Der Parameter _rcuDatabaseURL_ befindet sich am Ende der Datei _create-domain-inputs.yaml_. Navigieren Sie zum Ende der Datei, und aktualisieren Sie den Parameter auf die private IP der Instanz.

_Hinweis: Um eine Datei zu bearbeiten, verwenden Sie den Befehl "vi ", nach dem Sie mit den Pfeiltasten zu dem Speicherort wechseln können, an dem Änderungen in der Datei vorgenommen werden müssen (als Tastenkombination können Sie "Umschalt + G" verwenden, um direkt zum Ende der Datei zu gelangen). Drücken Sie auf der Tastatur "i", um in den Einfügemodus zu wechseln. Sie können die erforderlichen Änderungen eingeben ODER wenn Sie kopierten Inhalt einfügen möchten, führen Sie einfach einen Rechtsklick aus, indem Sie zuerst sicherstellen, dass sich der Cursor genau an der richtigen Stelle befindet. Wenn Sie fertig sind, drücken Sie 'Esc', um den Einfügemodus zu beenden. Drücken Sie schließlich ':wq', um die Änderungen zu speichern und den vi-Editor zu beenden._

_Hinweis: Der Parameter frontEndHost muss nicht mit dem Standardwert aktualisiert werden. Wir haben einen Beispielwert hinzugefügt, der zur Ausführung des Skripts erforderlich ist. Dieser Wert sollte in den weiteren Schritten nicht für den Zugriff auf die WebLogic-/OIM-Konsole verwendet werden._

    ```
    <copy>vi create-domain-inputs.yaml</copy>
    ```
    
    ![Update rcuDatabaseURL parameter](images/15-rcu.png)
    

## Aufgabe 8: Domainerstellungsskript ausführen, um domänenbezogene Kubernetes-Artefakte zu generieren

1.  Führen Sie das Skript zum Erstellen der Domain aus, und geben Sie die Eingabedatei und ein Ausgabeverzeichnis zum Speichern der generierten Artefakte an.

_Hinweis: Dies kann etwa 6-7 Minuten dauern._

    ```
    <copy>./create-domain.sh -i create-domain-inputs.yaml -o output_oimcluster</copy>
    ```
    

## Aufgabe 9: Docker Registry Secret und Kubernetes-Ressource erstellen

1.  Erstellen Sie ein Docker Registry Secret mit dem Namen oig-docker.
    
    _Hinweis: Sie können diesen Befehl unverändert ausführen. Es ist nicht erforderlich, einen der Parameterwerte im folgenden Befehl zu aktualisieren._
    
        	<copy>kubectl create secret docker-registry oig-docker -n oimcluster --docker-username='<user_name>' --docker-password='<password>' --docker-server='<docker_registry_url>' --docker-email='<email_address>'</copy>
        
2.  Erstellen Sie die Kubernetes-Ressource mit dem folgenden Befehl:
    
        	<copy>
        

cd output\_oimcluster/weblogic-domains/oimcluster/ kubectl apply -f domain.yaml \`\`

3.  Führen Sie den folgenden Befehl aus, um den Status der OIG-Pods anzuzeigen:
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
    
    _**Hinweis: Der introspect-domain-job-Pod wird zuerst angezeigt, der für die Erstellung der Admin-Server- und SOA-Server-Pods verantwortlich ist.**_
    
    ![OIG-Pods](images/16-pods.png)
    
4.  Es dauert 8-10 Minuten, bis die Pods für Admin-Server und SOA-Server in den Status 1/1 gelangen. Während die Pods gestartet werden, können Sie den Startstatus in den Podlogs prüfen, indem Sie die folgenden Befehle ausführen:
    
        	<copy>kubectl logs oimcluster-adminserver -n oimcluster</copy>
        
    
        	<copy>kubectl logs oimcluster-soa-server1 -n oimcluster</copy>
        

_**Hinweis: Sie müssen erst mit dem nächsten Schritt fortfahren, wenn sich die Pods für adminserver und soa-server im Status READY 1/1 befinden.**_

5.  Wenn beide Pods aus dem vorherigen Schritt den Status 1/1 READY aufweisen, starten Sie den OIM-Server mit dem folgenden Befehl:
    
        	<copy>kubectl apply -f domain_oim_soa.yaml</copy>
        
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    Es kann etwa 6-8 Minuten dauern, bis sich der OIM-Pod (OIM-server) im Status READY 1/1 befindet. Während der Start des Pods können Sie den Startstatus in den Podlogs prüfen.
    
    ![OIG-Pods](images/17-pods.png)
    
        	<copy>kubectl logs oimcluster-oim-server1 -n oimcluster</copy>
        

## Aufgabe 10: Domain, Pods und Services prüfen

1.  Prüfen Sie, ob die Domain, Serverpods und Services erstellt wurden und sich im Status READY mit dem Status STATUS 1/1 befinden, indem Sie den folgenden Befehl ausführen:
    
        	<copy>kubectl get all,domains -n oimcluster -o wide</copy>
        
    
    Die vom Skript erstellte Standarddomain weist die folgenden Eigenschaften auf:
    
    Ein Administrationsserver namens AdminServer, der auf Port 7001 horcht.
    
    Ein konfiguriertes OIG-Cluster mit dem Namen oig\_cluster der Größe 3.
    
    Ein konfiguriertes SOA-Cluster mit dem Namen soa\_cluster der Größe 3.
    
    Einer startete den OIG Managed Server namens oim\_server1 und horchte auf Port 14000.
    
    Einer startete den SOA Managed Server namens soa\_server1 und horchte auf Port 8001.
    
    Logdateien befinden sich in /u01/domains/oimclusterdomainpv/logs/oimcluster.
    
    ![OIG-Pods](images/18-pods.png)
    
2.  Domain verifizieren
    
        	<copy>kubectl describe domain oimcluster -n oimcluster</copy>
        
    
    Im Abschnitt Status der Ausgabe werden die verfügbaren Server und Cluster aufgeführt.
    
3.  Prüfen Sie die Pods. Notieren Sie die Pod-IP-Adresse des Adminservers und oim\_server1 aus der IP-Spalte.
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    Beispiel:
    
    ![OIG-Pods](images/20-pods.png)
    
4.  Öffnen Sie ein Browserfenster, um über die Admin-Server-Pod-IP mit der folgenden URL auf die WebLogic-Konsole zuzugreifen.
    
        	<copy><ADMIN_IP>:7001/console</copy>
        
    
    Dabei ist _`<ADMIN_IP>`_ die im vorherigen Schritt angegebene Admin-Server-Pod-IP.
    
    ![Administrationskonsole](images/21-vnc.png)
    
    Melden Sie sich mit den folgenden Zugangsdaten bei der Konsole an:
    
    Benutzername:
    
        	<copy>weblogic</copy>
        
    
    Passwort:
    
        	<copy>Welcom@123</copy>
        
    
    Klicken Sie unter _Umgebung_ auf _Server_, und prüfen Sie, ob die SOA- und OIM-Server ausgeführt werden.
    
    ![Administrationskonsole](images/22-vnc.png)
    
    ![Serverstatus](images/29-vnc.png)
    
5.  Öffnen Sie eine Browserregisterkarte, um über die Pod-IP oim\_server1 mit der folgenden URL auf die OIM-Konsole zuzugreifen.
    
        	<copy><OIM_IP>:14000/identity</copy>
        
    
    wobei _`<OIM_IP>`_ die im vorherigen Schritt angegebene Pod-IP oim\_server1 ist.
    
    Melden Sie sich mit den folgenden Zugangsdaten bei der Konsole an:
    
    Benutzername:
    
        	<copy>xelsysadm</copy>
        
    
    Passwort:
    
        	<copy>Welcom@123</copy>
        
    
    ![OIG-Konsole](images/23-vnc.png)
    
    ![OIG-Konsole](images/24-vnc.png)
    
    ![OIG-Konsole](images/25-vnc.png)
    

_Hinweis: Auf OIM- und SOA-Serverkonsolen kann auch über die jeweiligen Cluster-IPs zugegriffen werden_

Sie können jetzt mit der nächsten Übung fortfahren.

## Weitere Informationen

*   [Referenz für Oracle Identity Governance auf Docker und Kubernetes](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/oigdk/overview.html#GUID-1BA2B257-ED5A-4606-B833-C40B2F36F35F)

## Danksagungen

*   **Autor** - Keerti R, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Januar 2022