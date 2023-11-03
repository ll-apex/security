# Inicializar entorno

## Introducción

En este laboratorio revisaremos e iniciaremos todos los componentes necesarios para ejecutar correctamente este taller.

_Tiempo estimado:_ 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Iniciar la instancia del taller
*   Verificar Oracle Database
*   Crear e inicializar nodos de Kubernetes

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno

## Tarea 1: Verificación del entorno

1.  Abra una instancia de terminal y verifique que se está ejecutando la base de datos de OIG.
    
        	<copy>systemctl status oracle-database.service</copy>
        
    
    ![Servicio de sistema de base de datos](images/2-db.png)
    
    Pulse Ctrl+C para volver al terminal.
    
2.  Verifique la versión de docker y timón.
    
        	<copy>docker version</copy>
        
    
        	<copy>helm version</copy>
        
    
    ![Versión de Docker y timón](images/1-versions.png)
    
3.  Verifique las imágenes de docker de OIG, OAM y OUD.
    
        	<copy>docker images | grep oracle</copy>
        
    
    ![Imágenes de Docker](images/3-dockerimages.png)
    

## Tarea 2: Inicialización del entorno

1.  Realice los siguientes pasos de configuración de requisitos previos para inicializar el entorno.
    
        	<copy>
        

sudo setenforce 0 sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux intercambio de sudo -a \`\`

2.  Observe la IP privada de la instancia.
    
        	<copy>cat /etc/hosts</copy>
        
    
    Por ejemplo:
    
    ![Archivo /etc/hosts](images/4-ip.png)
    
3.  Despliegue e inicialice la red de pod y asegúrese de que la red de pod no se superponga con ninguna de las redes de host.
    
        	<copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        

_Nota: El rango de CIDR de red utilizado en el comando anterior para la inicialización de red de pod es 10.244.0.0/16, que es NO-OVERLAPPING para el CIDR de subred de host de 10.0.0.0/24. El CIDR de la subred del host seguirá siendo el mismo para cada despliegue._

4.  Active kubectl para trabajar con usuarios que no sean root.
    
        	<copy>sudo mkdir -p /home/oracle/.kube
        	sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config
        

sudo chown -R oracle:oinstall /home/oracle/.kube \`\`

5.  Verifique la versión de Kubernetes y programe pods en el nodo de panel de control.
    
        	<copy>kubectl version --short</copy>
        
    
    ![Versión de Kubectl](images/5-kube.png)
    
        	<copy>kubectl taint nodes --all node-role.kubernetes.io/control-plane-</copy>
        
6.  Muestre todos los pods en todos los espacios de nombres.
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
7.  Actualice los recursos del cluster y asegúrese de que todos los pods tengan el estado "En ejecución".
    
        	<copy>kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    Espere de 1 a 2 minutos y muestre todos los pods y asegúrese de que están en estado Running.
    
        	<copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Pods del sistema Kube](images/6-pod.png)
    

Ahora puede pasar a la siguiente práctica de laboratorio.

## Reconocimientos

*   **Autor**: Keerti R, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, enero de 2022