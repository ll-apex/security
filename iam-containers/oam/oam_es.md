# Desplegar dominio de Oracle Access Manager (OAM)

## Introducción

En este laboratorio se proporciona información sobre los pasos necesarios para desplegar y ejecutar un dominio de OAM en un cluster de Kubernetes con la ayuda de Oracle WebLogic Kubernetes Operator (3.1.0).

_Tiempo estimado:_ 30 minutos

### Acerca de Producto/Tecnología

Oracle Access Management proporciona nuevos servicios innovadores que complementan las capacidades tradicionales de gestión de acceso. No solo proporciona SSO web con MFA, autorización general y gestión de sesiones, sino que también proporciona funciones estándar de federación SAML y OAuth para permitir un acceso seguro a aplicaciones móviles y en la nube externas. Se puede integrar fácilmente con Oracle Identity Cloud Service para admitir las funciones de gestión de acceso híbrido, que pueden ayudar a los clientes a proteger a la perfección aplicaciones y cargas de trabajo locales y en la nube. El operador de Kubernetes de Oracle WebLogic Server soporta el despliegue de Oracle Access Management (OAM).

Los dominios de OAM se admiten mediante el modelo "domain on a persistente volume", donde el directorio raíz del dominio se encuentra en un volumen persistente (PV).

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Desplegar OAM en el entorno de kubernetes

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Laboratorio: Despliegue del dominio de Oracle Identity Governance (OIG)

## Tarea 1: Configurar el repositorio de código para desplegar dominios de OAM

Los componentes que se muestran a continuación ya se han instalado durante el despliegue de OIG (en el directorio /u01/k8siam) y, por lo tanto, no es necesario volver a configurarlos. Podemos copiar directamente los scripts de despliegue de OAM en la ubicación de ejemplos del operador de Kubernetes.

*   Imagen de Docker de operador de Kubernetes de Oracle WebLogic Server
*   WebLogic Código de origen de operador de Kubernetes
*   Scripts de despliegue de OAM desde el repositorio de Kubernetes de FMW

1.  Copie los scripts de despliegue de OAM en la ubicación de ejemplos del operador de Kubernetes de Oracle WebLogic Server.
    
        	<copy>cd /u01/k8siam</copy>
        
    
        	<copy>cp -rf /u01/k8siam/fmw-kubernetes/OracleAccessManagement/kubernetes/create-access-domain  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/</copy>
        

## Tarea 2: Instalación del operador de Kubernetes de Oracle WebLogic Server

1.  Ejecute el siguiente comando para crear un espacio de nombres para el operador:
    
        	<copy>kubectl create namespace opns</copy>
        
2.  Cree una cuenta de servicio para el operador en el espacio de nombres del operador ejecutando el siguiente comando:
    
        	<copy>kubectl create serviceaccount -n opns op-sa</copy>
        
3.  Ejecute el siguiente comando helm para instalar e iniciar el operador:
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm install weblogic-kubernetes-operator kubernetes/charts/weblogic-operator \
        	--namespace opns --set image=weblogic-kubernetes-operator:3.1.0 \
        	--set serviceAccount=op-sa --set "domainNamespaces={}" --set "javaLoggingLevel=FINE" --wait</copy>
        
    
    ![Instalar pod de operador](images/1-helm.png)
    
4.  Verifique que el pod y los servicios del operador se están ejecutando
    
        	<copy>kubectl get all -n opns</copy>
        
    
    ![Pod de operador](images/2-kube.png)
    

## Tarea 3: Creación de esquema de RCU

1.  Cree un espacio de nombres para el dominio:
    
        	<copy>kubectl create namespace accessns</copy>
        
2.  Cree un pod de ayuda para ejecutar RCU:
    
        	<copy>kubectl run helperoam --image oracle/oam:12.2.1.4.0 -n accessns -- sleep infinity</copy>
        
3.  Compruebe que el pod se está ejecutando:
    
        	<copy>kubectl get pods -n accessns</copy>
        
4.  Inicie un shell bash en el pod de ayuda:
    
        	<copy>kubectl exec -it helperoam -n accessns -- /bin/bash</copy>
        
5.  Sustituya _`<PRIVATE_IP>`_ por la IP privada de la instancia (notada en el laboratorio 1) y ejecute los siguientes comandos para definir el entorno:
    
        	<copy>export CONNECTION_STRING=<PRIVATE_IP>:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt</copy>
        
    
    Por ejemplo:
    
        	export CONNECTION_STRING=11.0.2.64:1521/orcl.livelabs.oraclevcn.com
        	export RCUPREFIX=OAMK8S
        	echo -e Welcom#123"\n"Welcom#123 > /tmp/pwd.txt
        	cat /tmp/pwd.txt
        
6.  Cree los esquemas de RCU en la base de datos. Esto puede tardar entre 3 y 4 minutos.
    
        	<copy>/u01/oracle/oracle_common/bin/rcu -silent -createRepository -databaseType ORACLE -connectString \
        	$CONNECTION_STRING -dbUser sys -dbRole sysdba -useSamePasswordForAllSchemaUsers true \
        	-selectDependentsForComponents true -schemaPrefix $RCUPREFIX -component MDS -component IAU \
        	-component IAU_APPEND -component IAU_VIEWER -component OPSS -component WLS -component STB -component OAM -f < /tmp/pwd.txt</copy>
        
    
    ![Crear esquema de RCU](images/3-rcu.png)
    
7.  Salir del shell bash auxiliar
    
        	<copy>exit</copy>
        

## Tarea 4: Preparación del entorno para la creación de dominios

1.  Configurar el operador para el espacio de nombres de dominio
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator</copy>
        
    
        	<copy>helm upgrade --reuse-values --namespace opns --set "domainNamespaces={accessns}" --wait weblogic-kubernetes-operator kubernetes/charts/weblogic-operator</copy>
        
2.  Crear secretos de Kubernetes para el dominio
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-credentials</copy>
        
    
        	<copy>./create-weblogic-credentials.sh -u weblogic -p Welcom@123 -n accessns -d accessinfra -s accessinfra-domain-credentials</copy>
        
3.  Verifique que se haya creado el secreto de dominio
    
        	<copy>kubectl get secret accessinfra-domain-credentials -o yaml -n accessns</copy>
        
4.  Creación de secretos de Kubernetes para la RCU
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-rcu-credentials</copy>
        
    
        	<copy>./create-rcu-credentials.sh -u OAMK8S -p Welcom#123 -a sys -q Welcom#123 -d accessinfra -n accessns -s accessinfra-rcu-credentials</copy>
        
5.  Verifique que se haya creado el secreto de RCU.
    
        	<copy>kubectl get secret accessinfra-rcu-credentials -o yaml -n accessns</copy>
        

## Tarea 5: Creación de un volumen persistente y una reclamación de volumen persistente de Kubernetes

1.  Cree el directorio necesario para el volumen persistente.
    
        	<copy>mkdir -p /u01/domains/accessdomainpv</copy>
        
    
        	<copy>chmod 777 /u01/domains/accessdomainpv</copy>
        
2.  Realice una copia de seguridad del archivo create-pv-pvc-inputs.yaml y ejecute el script para crear los archivos de configuración PV y PVC:
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-weblogic-domain-pv-pvc</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp create-pv-pvc-inputs.yaml create-pv-pvc-inputs.yaml_OIG</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-pv-pvc-inputs.yaml .</copy>
        
    
        	<copy>./create-pv-pvc.sh -i create-pv-pvc-inputs.yaml -o output_access</copy>
        
3.  Ejecute lo siguiente para mostrar que se han creado los archivos:
    
        	<copy>ls output_access/pv-pvcs</copy>
        
4.  Ejecute el siguiente comando de kubectl para crear PV y PVC en el espacio de nombres de dominio:
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pv.yaml -n accessns</copy>
        
    
        	<copy>kubectl create -f output_access/pv-pvcs/accessinfra-domain-pvc.yaml -n accessns</copy>
        
5.  Verificar pv y pvc creados
    
        	<copy>kubectl describe pv accessinfra-domain-pv</copy>
        
    
        	<copy>kubectl describe pvc accessinfra-domain-pvc -n accessns</copy>
        
    
    ![Verificar volumen persistente](images/4-pv.png)
    

## Tarea 6: Creación de Espacios de Nombres de Dominio de OAM

1.  Preparación de la secuencia de comandos de creación de dominio
    
        	<copy>cd /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv</copy>
        
    
        	<copy>cp create-domain-inputs.yaml create-domain-inputs.yaml.orig</copy>
        
    
        	<copy>mkdir output_access</copy>
        
    
        	<copy>cp /u01/sampleFilesOAM/create-domain-inputs.yaml .</copy>
        
2.  Edite el archivo _create-domain-inputs.yaml_ para actualizar la IP privada de la instancia en el parámetro _rcuDatabaseURL_. El parámetro _rcuDatabaseURL_ se encuentra al final del archivo _create-domain-inputs.yaml_. Navegue hasta el final del archivo y actualice el parámetro a la IP privada de la instancia.
    
        	<copy>vi create-domain-inputs.yaml</copy>
        
    
    ![Actualizar el parámetro rcuDatabaseURL](images/13-rcu.png)
    

## Tarea 7: Ejecutar el script de creación de dominio para generar artefactos de kubernetes relacionados con el dominio

1.  Ejecute el script de creación de dominio especificando el archivo de entrada y un directorio de salida para almacenar los artefactos generados:
    
        	<copy>./create-domain.sh -i create-domain-inputs.yaml -o output_access</copy>
        

## Tarea 8: Creación del recurso de Kubernetes

1.  Cree el recurso de Kubernetes con el siguiente comando:
    
        	<copy>cd  /u01/k8siam/weblogic-kubernetes-operator/kubernetes/samples/scripts/create-access-domain/domain-home-on-pv/output_access/weblogic-domains/accessinfra</copy>
        
    
        	<copy>kubectl apply -f domain.yaml</copy>
        

## Tarea 9: Verificar el dominio

1.  Ejecute el siguiente comando para ver el estado de los pods de OAM.
    
        	<copy>kubectl get pods -n accessns</copy>
        
    
    ![Pods de OAM](images/6-pods.png)
    
2.  Tardará entre 10 y 15 minutos antes de que todos los servicios enumerados anteriormente se muestren. Cuando un pod tiene un estado de 0/1, se inicia el pod, pero se está iniciando el servidor OAM asociado. Mientras se inician los pods, puede comprobar el estado de inicio en los logs de pod ejecutando el siguiente comando:
    
        	<copy>kubectl logs accessinfra-adminserver -n accessns</copy>
        
    
        	<copy>kubectl logs accessinfra-oam-server1 -n accessns</copy>
        
    
    El dominio por defecto creado por la secuencia de comandos tiene las siguientes características:
    
    Un servidor de administración denominado AdminServer que recibe en el puerto 9001.
    
    Cluster de OAM configurado denominado oam\_cluster de tamaño 3.
    
    Cluster de Policy Manager configurado denominado policy\_cluster de tamaño 3.
    
    Un servidor gestionado de OAM iniciado, denominado recepción oam\_server1 en el puerto 14100.
    
    Un servidor gestionado de Policy Manager iniciado denominado oam-policy-mgr1, que recibe en el puerto 15100.
    
    Los archivos log se encuentran en /u01/domains/accessdomainpv/logs/accessinfra
    
3.  Describa el dominio:
    
        	<copy>kubectl describe domain accessinfra -n accessns</copy>
        
4.  Verifique que el dominio, los servidores, los pods y los servicios se han creado y que están en estado READY (Listo) con un estado de 1/1
    
        	<copy>kubectl get all,domains -n accessns</copy>
        
    
    ![Pods de OAM](images/7-pods.png)
    
5.  Verifique los pods. Copiar la dirección IP del pod del servidor de administración
    
        	<copy>kubectl get pods -n accessns -owide</copy>
        
    
    Por ejemplo:
    
    ![Pods de OAM](images/12-pods.png)
    
6.  Abra una ventana del explorador para acceder a la consola de WebLogic mediante la IP del pod del servidor de administración mediante la siguiente URL.
    
        	<copy><ADMIN_IP>:9001/console</copy>
        
    
    donde _`<ADMIN_IP>`_ es la IP del pod del servidor de administración
    
    ![Consola de administración](images/8-vnc.png)
    
    Inicie sesión en la consola con las siguientes credenciales:
    
    Nombre de usuario:
    
        	<copy>weblogic</copy>
        
    
    Contraseña:
    
        	<copy>Welcom@123</copy>
        
    
    Haga clic en _Servidores_ en _Entorno_ y verifique que el servidor de OAM se está ejecutando.
    
    ![Consola de administración](images/11-vnc.png)
    

_Nota: El servidor gestionado de OAM se puede mostrar en estado de advertencia debido a un tamaño de pila limitado. El tamaño de pila se puede ampliar actualizando los archivos yaml de dominio generados._

8.  Abra un separador del explorador para acceder a la consola de OAM mediante la IP del pod del servidor de administración mediante la siguiente URL.
    
        	<copy><ADMIN_IP>:9001/oamconsole</copy>
        
    
    donde _`<ADMIN_IP>`_ es la IP del pod del servidor de administración
    
    ![Consola de OAM](images/9-vnc.png)
    
    Inicie sesión en la consola con las siguientes credenciales:
    
    Nombre de usuario:
    
        	<copy>weblogic</copy>
        
    
    Contraseña:
    
        	<copy>Welcom@123</copy>
        
    
    ![Consola de OAM](images/10-vnc.png)
    

Ahora puede pasar a la siguiente práctica de laboratorio.

## Más información

*   [Referencia para Oracle Access Management en Docker y Kubernetes](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, enero de 2022