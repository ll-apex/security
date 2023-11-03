# Umgebung initialisieren

## Einführung

In dieser Übung prüfen und starten wir alle Komponenten, die zum erfolgreichen Ausführen dieses Workshops erforderlich sind.

_Geschätzte Zeit:_ 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Workshop-Instanz starten
*   Oracle Database prüfen
*   Kubernetes-Knoten erstellen und initialisieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup

## Aufgabe 1: Umgebungsüberprüfung

1.  Öffnen Sie eine Terminalinstanz, und prüfen Sie, ob die OIG-Datenbank ausgeführt wird.
    
        	<copy>systemctl status oracle-database.service</copy>
        
    
    ![Datenbanksystemservice](images/2-db.png)
    
    Drücken Sie Ctrl+C, um zum Terminal zurückzukehren.
    
2.  Prüfen Sie die Docking- und Helm-Version.
    
        	<copy>docker version</copy>
        
    
        	<copy>helm version</copy>
        
    
    ![Docker- und Helm-Version](images/1-versions.png)
    
3.  Prüfen Sie die OIG-, OAM- und OUD-Docker-Images.
    
        	<copy>docker images | grep oracle</copy>
        
    
    ![Docker-Images](images/3-dockerimages.png)
    

## Aufgabe 2: Umgebungsinitialisierung

1.  Führen Sie die folgenden erforderlichen Einrichtungsschritte aus, um die Umgebung zu initialisieren.
    
        	<copy>
        

Sudo setenforce 0 sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux sudo swapoff -a \`\`

2.  Notieren Sie sich die private IP der Instanz.
    
        	<copy>cat /etc/hosts</copy>
        
    
    Beispiel:
    
    ![/etc/hosts-Datei](images/4-ip.png)
    
3.  Stellen Sie das Podnetzwerk bereit, und initialisieren Sie es, und stellen Sie sicher, dass sich das Podnetzwerk nicht mit einem der Hostnetzwerke überschneidet.
    
        	<copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        

_Hinweis: Der Netzwerk-CIDR-Bereich, der im obigen Befehl für die Podnetzwerkinitialisierung verwendet wird, beträgt 10.244.0.0/16. Dies ist NON-OVERLAPPING für das Hostsubnetz-CIDR von 10.0.0.0/24. Das Hostsubnetz-CIDR bleibt für jedes Deployment gleich._

4.  Aktivieren Sie kubectl für die Arbeit mit Nicht-Root-Benutzern.
    
        	<copy>sudo mkdir -p /home/oracle/.kube
        	sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config
        

sudo chown -R oracle:oinstall /home/oracle/.kube \`\`

5.  Prüfen Sie die Kubernetes-Version, und planen Sie Pods auf dem Knoten des Kontrollbereichs.
    
        	<copy>kubectl version --short</copy>
        
    
    ![Kubectl-Version](images/5-kube.png)
    
        	<copy>kubectl taint nodes --all node-role.kubernetes.io/control-plane-</copy>
        
6.  Listen Sie alle Pods in allen Namespaces auf.
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
7.  Aktualisieren Sie die Ressourcen im Cluster, und stellen Sie sicher, dass alle Pods den Status "Wird ausgeführt" aufweisen.
    
        	<copy>kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    Warten Sie 1-2 Minuten, und listen Sie alle Pods auf, und stellen Sie sicher, dass sie sich im Status "Wird ausgeführt" befinden.
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Kube-System-Pods](images/6-pod.png)
    

Sie können jetzt mit der nächsten Übung fortfahren.

## Danksagungen

*   **Autor** - Keerti R, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Keerti R, NATD Solution Engineering, Januar 2022