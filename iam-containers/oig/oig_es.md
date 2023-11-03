# Desplegar dominio de Oracle Identity Governance (OIG)

## Introducción

En este laboratorio se proporciona información sobre los pasos necesarios para desplegar y ejecutar el dominio de OIG en un cluster de kubernetes con la ayuda del operador de Kubernetes de Oracle WebLogic (3.1.0)

_Tiempo estimado:_ 30 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Despliegue del dominio de Oracle Identity Governance (OIG)](videohub:1_furulwif)

### Acerca de Producto/Tecnología

Oracle Identity Governance (OIG) es un sistema de gestión de identidad empresarial potente y flexible que gestiona automáticamente los privilegios de acceso del usuario en los recursos de TI de la empresa. El operador de Kubernetes de Oracle WebLogic soporta el despliegue de Oracle Identity Governance (OIG). Los dominios de OIG se admiten mediante el modelo de "dominio en un volumen persistente", donde el directorio raíz del dominio se encuentra en un volumen persistente (PV).

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Despliegue de OIG en el entorno de kubernetes

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Instalación y ejecución de la imagen de Docker de operador de Kubernetes de Oracle WebLogic Server

La imagen de Docker de operador de Kubernetes de Oracle WebLogic Server se debe instalar en el nodo maestro y en cada uno de los nodos de trabajador del cluster de Kubernetes. También puede colocar la imagen en un registro de Docker al que pueda acceder el cluster.

1.  Extraiga la imagen de operador de Kubernetes de Oracle WebLogic Server ejecutando el siguiente comando en el nodo maestro.
    
        	<copy>docker pull ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0</copy>
        
2.  Ejecute el comando docker tag de la siguiente forma:
    
        	<copy>docker tag ghcr.io/oracle/weblogic-kubernetes-operator:3.1.0 weblogic-kubernetes-operator:3.1.0</copy>
        
3.  Verifique las imágenes para el operador de kubernetes de WebLogic.
    
        	<copy>docker images | grep operator</copy>
        

## Tarea 2: Configuración del repositorio de código para desplegar dominios de Oracle Identity Governance

1.  Crear un directorio de trabajo y descargar el código fuente de Oracle WebLogic Kubernetes Operator 3.1.0 del proyecto github de operador
    

mkdir -p /u01/k8siam cd /u01/k8siam clon de git https://github.com/oracle/weblogic-kubernetes-operator.git --branch release/3.1.0 \`\`

2.  Clonar los scripts de despliegue de Oracle Identity Governance desde el repositorio de OIG y copiarlos en la ubicación de ejemplos de operadores WebLogic
    
        	<copy>git clone https://github.com/oracle/fmw-kubernetes.git</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleIdentityGovernance/kubernetes/create-oim-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## Tarea 3: Instalación del operador de Kubernetes de Oracle WebLogic

1.  Ejecute el siguiente comando para crear un espacio de nombres para el operador
    
        	<copy>kubectl create namespace operator</copy>
        
2.  Crear una cuenta de servicio para el operador en el espacio de nombres del operador
    
        	<copy>kubectl create serviceaccount -n operator operator-serviceaccount</copy>
        
3.  Ejecute el siguiente comando helm para instalar e iniciar el operador:
    
        	<copy>
        

cd weblogic-kubernetes-operator/ helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator  
\--namespace operator  
\--set image=weblogic-kubernetes-operator:3.1.0  
\--set serviceAccount=operator-serviceaccount  
\--set "domainNamespaces={}" \`\`

    ![Install operator pod](images/7-helm.png)
    

4.  Verifique la instalación del timón para el operador
    
        	<copy>helm ls -n operator</copy>
        
5.  Verifique que el pod del operador se está ejecutando con el estado 1/1 READY
    
        	<copy>kubectl get all -n operator</copy>
        
    
    ![Estado de pod de operador](images/8-operator.png)
    

## Tarea 4: Creación de esquema de RCU

1.  Ejecute el siguiente comando para crear un espacio de nombres para el dominio:
    
        	<copy>kubectl create namespace oimcluster</copy>
        
2.  Ejecute el siguiente comando para crear un pod de ayuda:
    
        	<copy>kubectl run helper --image oracle/oig:12.2.1.4.0 -n oimcluster -- sleep infinity</copy>
        
3.  Verificar el pod de ayuda
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
4.  Inicie un shell bash en el pod de ayuda. Esto te llevará a un shell bash en el pod rcu en ejecución:
    
        	<copy>kubectl exec -it helper -n oimcluster -- /bin/bash</copy>
        
5.  En el shell bash de ayuda, ejecute los siguientes comandos para definir el entorno. Sustituya el parámetro _`<PRIVATE_IP>`_ copiado del laboratorio anterior 'Initialize Environment', Tarea 2, Paso 2 (IP privada de instancia).
    
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
        
    
    Por ejemplo:
    
        	export DB_HOST=11.0.2.64
        

    export DB_PORT=1521
    export DB_SERVICE=orcl.livelabs.oraclevcn.com
    export RCUPREFIX=OIGK8S
    export RCU_SCHEMA_PWD=Welcom#123
    echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
    cat /tmp/pwd.txt
    export RCU_SCHEMA_PWD=Welcom#123
    ```
    

6.  En el shell bash de ayuda, ejecute los siguientes comandos para crear los esquemas de RCU en la base de datos:
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$DB_HOST:$DB_PORT/$DB_SERVICE -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component OIM -component MDS -component SOAINFRA -component OPSS \
        	-f < /tmp/pwd.txt</copy>
        
    
    _**Nota: Los esquemas de RCU pueden tardar entre 5 y 8 minutos en crearse.**_
    
    ![Crear Esquemas de RCU](images/9-rcu.png)
    
7.  Salir del shell bash auxiliar
    
        	<copy>exit</copy>
        

## Tarea 5: Preparar el entorno para la creación de dominios

1.  Configurar el operador de Kubernetes de Oracle WebLogic para gestionar el dominio en el espacio de nombres de dominio
    
        	<copy>helm upgrade --reuse-values --namespace operator --set "domainNamespaces={oimcluster}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  Cree un secreto de Kubernetes para el dominio mediante el script create-weblogic-credentials en el mismo espacio de nombres de Kubernetes que el dominio:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials ./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n oimcluster -d oimcluster -s oimcluster-domain-credentials \`\`

    where:
    
    -u weblogic is the WebLogic username
    
    -p <pwd> is the password for the WebLogic user
    
    -n <domain_namespace> is the domain namespace
    
    -d <domain_uid> is the domain UID to be created. The default is domain1 if not specified
    
    -s <kubernetes_domain_secret> is the name you want to create for the secret for this namespace. The default is to use the domainUID if not specified
    

3.  Verifique que se haya creado el secreto de dominio
    
        	<copy>kubectl get secret oimcluster-domain-credentials -o yaml -n oimcluster</copy>
        
    
    ![Secreto de dominio](images/10-secret.png)
    
4.  Cree un secreto de Kubernetes para RCU en el mismo espacio de nombres de Kubernetes que el dominio mediante el script create-weblogic-credentials.sh:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials ./create-rcu-credentials.sh -u OIGK8S -p Welcom#123 -a sys -q Welcom#123 -d oimcluster -n oimcluster -s oimcluster-rcu-credentials \`\`

    where:
    
    -u <rcu_prefix> is the name of the RCU schema prefix created previously
    
    -p <rcu_schema_pwd> is the password for the RCU schema prefix
    
    -q <sys_db_pwd> is the sys database password
    
    -d <domain_uid> is the domain_uid that you created earlier
    
    -n <domain_namespace> is the domain namespace
    
    -s <kubernetes_rcu_secret> is the name of the rcu secret to create
    

5.  Verifique que se haya creado el secreto de RCU.
    
        	<copy>kubectl get secret oimcluster-rcu-credentials -o yaml -n oimcluster</copy>
        
    
    ![Secreto de RCU](images/11-secret.png)
    

## Tarea 6: Creación de un volumen persistente y una reclamación de volumen persistente de Kubernetes

1.  Cree el directorio necesario para el volumen persistente.
    
        	<copy>
        

mkdir -p /u01/domains/oimclusterdomainpv chmod 777 /u01/domains/oimclusterdomainpv \`\`

2.  Realice una copia de seguridad del archivo create-pv-pvc-inputs.yaml y ejecute el script create-pv-pvc.sh para crear los archivos de configuración PV y PVC:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-pv-pvc-inputs.yaml . ./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output\_oimcluster \`\`

    ![Create persistent volume](images/12-pv.png)
    

3.  Ejecute lo siguiente para mostrar que se han creado los archivos:
    
        	<copy>ls output_oimcluster/pv-pvcs</copy>
        
4.  Ejecute el siguiente comando de kubectl para crear PV y PVC en el espacio de nombres de dominio:
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pv.yaml -n oimcluster</copy>
        
    
        	<copy>kubectl create -f output_oimcluster/pv-pvcs/oimcluster-oim-pvc.yaml -n oimcluster</copy>
        
5.  Verificar pv y pvc creados
    
        	<copy>kubectl describe pv oimcluster-oim-pv -n oimcluster</copy>
        
    
    ![Describir el volumen persistente](images/13-pv.png)
    
        	<copy>kubectl describe pvc oimcluster-oim-pvc -n oimcluster</copy>
        
    
    ![Describir el volumen persistente](images/14-pv.png)
    

## Tarea 7: Creación de dominios de OIG

1.  Realice una copia del archivo create-domain-inputs.yaml:
    
        	<copy>
        

cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-oim-domain/domain-home-on-pv cp create-domain-inputs.yaml create-domain-inputs.yaml.orig mkdir output\_oimcluster cp /u01/sampleFilesOIG/create-domain-inputs.yaml . \`\`

3.  Edite el parámetro _rcuDatabaseURL_ para incluir la IP privada de la instancia (notada en el laboratorio anterior "Initialize Environment", tarea 2, paso 2). También puede obtener la IP privada proporcionando el comando _cat /etc/hosts_. El parámetro _rcuDatabaseURL_ se encuentra al final del archivo _create-domain-inputs.yaml_. Navegue hasta el final del archivo y actualice el parámetro a la IP privada de la instancia.

_Nota: Para editar un archivo, utilice el comando 'vi ', después del cual puede usar las teclas de flecha para desplazarse a la ubicación donde se deben realizar cambios en el archivo (como acceso directo, puede usar 'Mayús + G' para ir directamente al final del archivo). Pulse 'i' en el teclado para cambiar al modo de inserción. Puede escribir los cambios necesarios O bien, si desea pegar contenido copiado, simplemente haga un clic con el botón derecho asegurándose de que el cursor esté en el lugar exacto. Cuando haya terminado, pulse 'Esc' para salir del modo Insertar. Por último, pulse ':wq' para guardar los cambios y salir del editor vi._

_Nota: No es necesario actualizar el parámetro frontEndHost a partir de su valor por defecto. Hemos agregado un valor de ejemplo necesario para ejecutar el script y no se debe utilizar para acceder a la consola de weblogic/oim en los pasos posteriores._

    ```
    <copy>vi create-domain-inputs.yaml</copy>
    ```
    
    ![Update rcuDatabaseURL parameter](images/15-rcu.png)
    

## Tarea 8: Ejecutar el script de creación de dominio para generar artefactos de kubernetes relacionados con el dominio

1.  Ejecute el script de creación de dominio especificando el archivo de entrada y un directorio de salida para almacenar los artefactos generados.

_Nota: Esto puede tardar entre 6 y 7 minutos._

    ```
    <copy>./create-domain.sh -i create-domain-inputs.yaml -o output_oimcluster</copy>
    ```
    

## Tarea 9: Crear recurso de Kubernetes y secreto de Docker Registry

1.  Cree un secreto de Docker Registry con el nombre oig-docker.
    
    _Nota: Puede ejecutar este comando tal cual. No es necesario actualizar ninguno de los valores de parámetro en el siguiente comando._
    
        	<copy>kubectl create secret docker-registry oig-docker -n oimcluster --docker-username='<user_name>' --docker-password='<password>' --docker-server='<docker_registry_url>' --docker-email='<email_address>'</copy>
        
2.  Cree el recurso de Kubernetes con el siguiente comando:
    
        	<copy>
        

cd output\_oimcluster/weblogic-domains/oimcluster/ kubectl apply -f domain.yaml \`\`

3.  Ejecute el siguiente comando para ver el estado de los pods de OIG:
    
        	<copy>kubectl get pods -n oimcluster</copy>
        
    
    _**Nota: El pod introspect-domain-job se mostrará primero, que será responsable de crear los pods adminserver y soa-server.**_
    
    ![Pods de OIG](images/16-pods.png)
    
4.  Tardará entre 8 y 10 minutos antes de que los pods para adminserver y soa-server pasen al estado 1/1. Mientras se inician los pods, puede comprobar el estado de inicio en los logs de pod ejecutando los siguientes comandos:
    
        	<copy>kubectl logs oimcluster-adminserver -n oimcluster</copy>
        
    
        	<copy>kubectl logs oimcluster-soa-server1 -n oimcluster</copy>
        

_**Nota: Solo debe continuar con el siguiente paso una vez que los pods para adminserver y soa-server estén en estado READY 1/1.**_

5.  Una vez que ambos pods del paso anterior estén en estado 1/1 READY, inicie el servidor OIM con el siguiente comando:
    
        	<copy>kubectl apply -f domain_oim_soa.yaml</copy>
        
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    El pod de OIM (oim-server) puede tardar entre 6 y 8 minutos en estar en estado READY 1/1. Mientras se inicia el pod, puede comprobar el estado de inicio en los logs de pod.
    
    ![Pods de OIG](images/17-pods.png)
    
        	<copy>kubectl logs oimcluster-oim-server1 -n oimcluster</copy>
        

## Tarea 10: Verificación del dominio, los pods y los servicios

1.  Verifique que el dominio, los servidores, los pods y los servicios se hayan creado y estén en estado READY (Listo) con un estado 1/1, ejecutando el siguiente comando:
    
        	<copy>kubectl get all,domains -n oimcluster -o wide</copy>
        
    
    El dominio por defecto creado por la secuencia de comandos tiene las siguientes características:
    
    Un servidor de administración denominado AdminServer que recibe en el puerto 7001.
    
    Un cluster de OIG configurado denominado oig\_cluster de tamaño 3.
    
    Cluster de SOA configurado denominado soa\_cluster de tamaño 3.
    
    Un servidor gestionado de OIG iniciado, denominado oim\_server1, que recibe en el puerto 14000.
    
    Un servidor gestionado de SOA iniciado, denominado soa\_server1, que recibe en el puerto 8001.
    
    Los archivos log se encuentran en /u01/domains/oimclusterdomainpv/logs/oimcluster.
    
    ![Pods de OIG](images/18-pods.png)
    
2.  Verificación del dominio
    
        	<copy>kubectl describe domain oimcluster -n oimcluster</copy>
        
    
    En la sección Status de la salida, se muestran los servidores y clusters disponibles.
    
3.  Verifique los pods. Observe la dirección IP del pod del servidor de administración y oim\_server1 de la columna IP.
    
        	<copy>kubectl get pods -n oimcluster -o wide</copy>
        
    
    Por ejemplo:
    
    ![Pods de OIG](images/20-pods.png)
    
4.  Abra una ventana del explorador para acceder a la consola de WebLogic mediante la IP del pod del servidor de administración mediante la siguiente URL.
    
        	<copy><ADMIN_IP>:7001/console</copy>
        
    
    donde _`<ADMIN_IP>`_ es la IP del pod del servidor de administración anotada en el paso anterior.
    
    ![Consola de administración](images/21-vnc.png)
    
    Inicie sesión en la consola con las siguientes credenciales:
    
    Nombre de usuario:
    
        	<copy>weblogic</copy>
        
    
    Contraseña:
    
        	<copy>Welcom@123</copy>
        
    
    Haga clic en _Servidores_ en _Entorno_ y verifique que los servidores SOA y OIM se están ejecutando.
    
    ![Consola de administración](images/22-vnc.png)
    
    ![Estado del Servidor](images/29-vnc.png)
    
5.  Abra un separador del explorador para acceder a la consola de OIM mediante la IP del pod oim\_server1 mediante la siguiente URL.
    
        	<copy><OIM_IP>:14000/identity</copy>
        
    
    donde _`<OIM_IP>`_ es la IP del pod oim\_server1 anotada en el paso anterior.
    
    Inicie sesión en la consola con las siguientes credenciales:
    
    Nombre de usuario:
    
        	<copy>xelsysadm</copy>
        
    
    Contraseña:
    
        	<copy>Welcom@123</copy>
        
    
    ![Consola de OIG](images/23-vnc.png)
    
    ![Consola de OIG](images/24-vnc.png)
    
    ![Consola de OIG](images/25-vnc.png)
    

_Nota: También se puede acceder a las consolas de servidor OIM y SOA mediante sus respectivas IP de cluster_

Ahora puede pasar a la siguiente práctica de laboratorio.

## Más información

*   [Referencia para Oracle Identity Governance en Docker y Kubernetes](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/oigdk/overview.html#GUID-1BA2B257-ED5A-4606-B833-C40B2F36F35F)

## Reconocimientos

*   **Autor**: Keerti R, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, enero de 2022