# Despliegue del cluster de Kubernetes e inicie el servidor de OIG

## Introducción

En este laboratorio, revisaremos e iniciaremos todos los nodos de Kubernetes necesarios para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear e inicializar cluster de Kubernetes
*   Inicie el dominio OIG 12c. Analizar diferentes roles y derechos creados en OIG

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Inicializar el cluster de Kubernetes y el complemento de red de pod

1.  Abra una sesión de terminal.
    
        <copy>sudo swapoff -a</copy>
        
    
        <copy>sudo setenforce 0</copy>
        
    
        <copy>sudo sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux</copy>
        
2.  Despliegue e inicialice la red de pod y asegúrese de que la red de pod no se superponga con ninguna de las redes de host.
    
        <copy>sudo kubeadm init --pod-network-cidr=10.244.0.0/16</copy>
        
3.  Active kubectl para trabajar con usuarios que no sean root.
    
        <copy>sudo cp -i /etc/kubernetes/admin.conf /home/oracle/.kube/config</copy>
        
    
        <copy>sudo chown oracle:oinstall /home/oracle/.kube/config</copy>
        
4.  Inicie otra sesión de terminal y programe pods en el nodo del plano de control.
    
        <copy>kubectl taint nodes --all node-role.kubernetes.io/master-</copy>
        
5.  Muestre todos los pods en todos los espacios de nombres.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
6.  Actualice los recursos del cluster y asegúrese de que todos los pods tengan el estado "En ejecución".
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml</copy>
        
    
    Espere de 1 a 2 minutos y muestre todos los pods y asegúrese de que tengan el estado _Running_ y _READY 1/1_.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
    ![Inicializar el cluster de Kubernetes y el complemento de red de pod](images/pods.png)
    

## Tarea 2: Inicio del servidor de Oracle Identity Governance (OIG) y análisis de los roles en OIG

1.  Verifique que el servidor de administración se está ejecutando. Abra una ventana del explorador y utilice la siguiente URL para acceder a la consola de Weblogic.
    
        URL  http://oiri.livelabs.oraclevcn.com:7001/console/login/
        
    
    ![Página de la consola de Weblogic](images/weblogic-console.png)
    
2.  Inicie sesión en la consola con las credenciales de WebLogic.
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Inicio de sesión de credenciales de la consola de Weblogic](images/weblogic-credentials.png)
    
3.  En la consola de Weblogic, haga clic en _Servidores_ en _Entorno_. En Resumen de servidores, haga clic en _Control_.
    
    ![Haga clic en Servidores en Entorno](images/weblogic-server.png)
    
    Seleccione el servidor SOA y OIM y haga clic en _Iniciar_.
    
    ![Haga clic en Control y comience](images/control-server.png) ![Servidores en estado En ejecución](images/running-server.png)
    
4.  Abra otro separador del explorador y utilice la siguiente URL para acceder a la _consola de identidad de OIG_. Conéctese a la consola de Identity con las siguientes credenciales:
    
        URL       http://oiri.livelabs.oraclevcn.com:14000/identity
        Username  xelsysadm
        Password  Welcome1
        
    
    ![Página inicial de la consola de identidad de OIG](images/oig.png)
    
    ![Conexión de credenciales de la consola de identidad de OIG](images/oig-credentials.png)
    
5.  Haga clic en _Gestionar_ en la esquina superior derecha. A continuación, haga clic en _Usuarios_ y observe que se han creado unos 1000 usuarios de prueba con los roles y derechos respectivos. Haga clic en cualquier usuario y, en el separador _Cuentas_, observe que los usuarios se aprovisionan en la aplicación _Acceso a documentos_.
    
    ![Separador Gestión de Identidad de OIG](images/users-oig.png)
    
    ![Separador Usuarios de identidad de OIG](images/display-users-oig.png)
    
    ![Separador Detalles de cuenta de usuario de identidad de OIG](images/user-details-oig.png)
    
6.  Ahora, haga clic en _Inicio_. A continuación, haga clic en _Roles y políticas de acceso_ y seleccione _Roles_. Observe que el rol _OrclOIRIRoleEngineer_ se crea y se asigna al usuario de la aplicación para que el usuario pueda conectarse a la aplicación OIRI. Haga clic en el rol _OrclOIRIRoleEngineer_. Haga clic en el separador _Members_ y observe que este rol está asignado al usuario _xelsysadm_.
    
    ![Roles de identidad y políticas de acceso de OIG](images/roles-oig.png)
    
    ![Separador Roles de Identidad de OIG](images/display-roles-oig.png)
    
    ![Separador de detalles de miembro del rol de identidad de OIG](images/members-role-oig.png)
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD, diciembre de 2022