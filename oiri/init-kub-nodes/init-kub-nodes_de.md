# Kubernetes-Cluster bereitstellen und OIG-Server starten

## Einführung

In dieser Übung prüfen und starten wir alle und stellen Kubernetes-Knoten bereit, die zur erfolgreichen Ausführung dieses Workshops erforderlich sind.

_Geschätzte Laborzeit_: 20 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Kubernetes-Cluster erstellen und initialisieren
*   Starten Sie die OIG 12c-Domain. Verschiedene in OIG erstellte Rollen und Berechtigungen analysieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier, kostenpflichtig oder LiveLabs Oracle Cloud-Account
*   Sie haben abgeschlossen:
    *   Übung: Setup vorbereiten (nur _Free-Tier_ und _bezahlte Mandanten_)
    *   Übung: Umgebungssetup
    *   Übung: Umgebung initialisieren

## Aufgabe 1: Kubernetes-Cluster und das Podnetzwerk-Add-on initialisieren

1.  Öffnen Sie eine Terminal Session.
    
        <copy>sudo swapoff -a</copy>
        
    
        <copy>sudo setenforce 0</copy>
        
    
        <copy>sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux</copy>
        
2.  Stellen Sie das Podnetzwerk bereit, und initialisieren Sie es, und stellen Sie sicher, dass sich das Podnetzwerk nicht mit einem der Hostnetzwerke überschneidet.
    
        <copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        
3.  Aktivieren Sie kubectl für die Arbeit mit Nicht-Root-Benutzern.
    
        <copy>sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config</copy>
        
    
        <copy>sudo chown oracle:oinstall /home/oracle/.kube/config</copy>
        
4.  Starten Sie eine weitere Terminalsession, und planen Sie Pods auf dem Control-Plane-Knoten.
    
        <copy>kubectl taint nodes --all node-role.kubernetes.io/master-</copy>
        
5.  Listen Sie alle Pods in allen Namespaces auf.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
6.  Aktualisieren Sie die Ressourcen im Cluster, und stellen Sie sicher, dass alle Pods den Status "Wird ausgeführt" aufweisen.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    Warten Sie 1-2 Minuten, und listen Sie alle Pods auf, und stellen Sie sicher, dass sie den Status _Wird ausgeführt_ und _READY 1/1_ aufweisen.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Kubernetes-Cluster und das Podnetzwerk-Add-on initialisieren](images/pods.png)
    

## Aufgabe 2: Oracle Identity Governance-(OIG-)Server starten und die Rollen in OIG analysieren

1.  Prüfen Sie, ob der Admin-Server ausgeführt wird. Öffnen Sie ein Browserfenster, und greifen Sie mit der unten genannten URL auf die Weblogic-Konsole zu.
    
        URL  http://oiri.livelabs.oraclevcn.com:7001/console/login/
        
    
    ![Weblogic-Konsolenseite](images/weblogic-console.png)
    
2.  Melden Sie sich mit den WebLogic-Zugangsdaten bei der Konsole an.
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Anmeldung mit den Zugangsdaten der Weblogic-Konsole](images/weblogic-credentials.png)
    
3.  Klicken Sie in der Weblogic-Konsole unter _Umgebung_ auf _Server_. Klicken Sie unter "Zusammenfassung der Server" auf _Steuerung_.
    
    ![Klicken Sie auf Server unter Umgebung](images/weblogic-server.png)
    
    Wählen Sie SOA- und OIM-Server, und klicken Sie auf _Starten_.
    
    ![Klicken Sie auf Control und starten](images/control-server.png) ![Server mit dem Status "Wird ausgeführt"](images/running-server.png)
    
4.  Öffnen Sie eine andere Browserregisterkarte, und greifen Sie mit der folgenden URL auf die _OIG-Identitätskonsole_ zu. Melden Sie sich mit den folgenden Zugangsdaten bei der Identity-Konsole an:
    
        URL       http://oiri.livelabs.oraclevcn.com:14000/identity
        Username  xelsysadm
        Password  Welcome1
        
    
    ![OIG Identity-Konsole - Homepage](images/oig.png)
    
    ![Zugangsdaten für OIG Identity-Konsole - Anmeldung](images/oig-credentials.png)
    
5.  Klicken Sie oben rechts auf _Verwalten_. Klicken Sie anschließend auf _Benutzer_, und beachten Sie, dass etwa 1000 Testbenutzer mit den entsprechenden Rollen und Berechtigungen erstellt wurden. Klicken Sie auf einen beliebigen Benutzer, und klicken Sie auf die Registerkarte _Accounts_, um zu erfahren, dass die Benutzer für die Anwendung _Dokumentzugriff_ bereitgestellt werden.
    
    ![OIG Registerkarte "Identität verwalten"](images/users-oig.png)
    
    ![OIG Registerkarte "Identitätsbenutzer"](images/display-users-oig.png)
    
    ![Registerkarte "Accountdetails" des OIG-Identitätsbenutzers](images/user-details-oig.png)
    
6.  Klicken Sie jetzt auf _Home_. Klicken Sie anschließend auf _Rollen und Zugriffs-Policys_, und wählen Sie _Rollen_ aus. Beachten Sie, dass die Rolle _OrclOIRIRoleEngineer_ erstellt und dem Anwendungsbenutzer zugewiesen wird, damit sich der Benutzer bei der OIRI-Anwendung anmelden kann. Klicken Sie auf die Rolle _OrclOIRIRoleEngineer_. Klicken Sie auf die Registerkarte _Mitglieder_, und beachten Sie, dass diese Rolle dem Benutzer _xelsysadm_ zugewiesen ist.
    
    ![OIG-Identitätsrollen und -Zugriffs-Policys](images/roles-oig.png)
    
    ![OIG Registerkarte "Identitätsrollen"](images/display-roles-oig.png)
    
    ![Registerkarte "Mitgliedsdetails" der OIG-Identitätsrolle](images/members-role-oig.png)
    

Sie können jetzt [mit der nächsten Übung fortfahren](#next).

## Danksagungen

*   **Autor** - Keerti R, Brijith TG, Anuj Tripathi, NATD Solution Engineering
*   **Mitwirkende** - Keerti R, Brijith TG, Anuj Tripathi
*   **Zuletzt aktualisiert am/um** - Indiradarshni B, NATD Solution Engineering, Dezember 2022