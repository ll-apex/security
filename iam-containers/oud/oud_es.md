# Desplegar Oracle Unified Directory (OUD)

## Introducción

En este laboratorio se proporciona información sobre los pasos necesarios para desplegar y ejecutar Oracle Unified Directory 12c PS4 (12.2.1.4.0) en un entorno de Kubernetes.

_Tiempo estimado:_ 20 minutos

### Acerca de <Producto/Tecnología>

Oracle Unified Directory proporciona una completa solución de directorio para una gestión de identidad sólida. Oracle Unified Directory es una solución de directorio todo en uno con capacidades de almacenamiento, proxy, sincronización y virtualización. Al unificar el enfoque, proporciona todos los servicios necesarios para entornos empresariales de alto rendimiento y de nivel de operador. Oracle Unified Directory garantiza escalabilidad a miles de millones de entradas, facilidad de instalación, despliegues flexibles, capacidad de gestión empresarial y supervisión eficaz.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear una instancia de OUD en el entorno de kubernetes

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Laboratorio: Despliegue del dominio de Oracle Identity Governance (OIG)
    *   Laboratorio: Despliegue del dominio de Oracle Access Manager (OAM)

## Tarea 1: Preparar el entorno para la creación de contenedores

1.  Comprobación de que el cluster de Kubernetes está listo
    
        	<copy>kubectl get nodes,pods -n kube-system</copy>
        
    
    ![Cluster de Kubernetes](images/1-kube.png)
    
2.  Comprobación de la Imagen de Oracle Unified Directory
    
        	<copy>docker images | grep oud</copy>
        
3.  Crear espacio de nombres de Kubernetes
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp oudns.yaml oudns.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oudns.yaml .</copy>
        
    
        	<copy>kubectl apply -f oudns.yaml</copy>
        
4.  Confirme que se ha creado el espacio de nombres:
    
        	<copy>kubectl get namespaces </copy>
        
5.  Crear secretos para ID de usuario y contraseñas
    
        	<copy>cp secrets.yaml secrets.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/secrets.yaml .</copy>
        
    
    Este archivo incluye el secreto actualizado, el espacio de nombres y otros parámetros con valores especificados en formato Base64
    
    secreto - oudsecret
    
    espacio de nombres - myoudns
    
    %rootUserDN% - Valor codificado Base64 de 'cn=Directory Manager' para el parámetro rootUserDN
    
    %rootUserPassword% - Valor codificado Base64 del parámetro 'Welcom@123' rootUserPassword
    
    %adminUID% - Valor codificado Base64 de 'admin' para el parámetro adminUID
    
    %adminPassword% - Valor codificado Base64 de 'Welcom@123' para el parámetro adminPassword
    
    %bindDN1% - Valor codificado Base64 del parámetro 'cn=Directory Manager bindDN1
    
    %bindPassword1% - Valor codificado Base64 de 'Welcom@123 para el parámetro bindPassword1
    
    %bindDN2% - Valor codificado Base64 de 'cn=Directory Manager para el parámetro bindDN2
    
    %bindPassword2% - Valor codificado Base64 de 'Welcom@123' para el parámetro bindPassword2
    
6.  Aplique el archivo:
    
        	<copy>kubectl apply -f secrets.yaml</copy>
        
7.  Verifique que el secreto se haya creado:
    
        	<copy>kubectl --namespace myoudns get secret</copy>
        
8.  Preparar un directorio de host para utilizarlo para PersistentVolume basado en sistema de archivos
    
        	<copy>mkdir -p /u01/domains/oud_user_projects</copy>
        
    
        	<copy>chmod 777 /u01/domains/oud_user_projects</copy>
        
    
        	<copy>cp persistent-volume.yaml persistent-volume.yaml_orig</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/persistent-volume.yaml .</copy>
        
9.  Aplique el archivo:
    
        	<copy>kubectl apply -f persistent-volume.yaml</copy>
        
10.  Verifique PersistentVolume y PersistentVolumeClaim.
    
        <copy>kubectl --namespace myoudns describe persistentvolume oudpv</copy>
        
    
        <copy>kubectl --namespace myoudns describe pvc oudpvc</copy>
        
    
    ![Verificar volumen persistente](images/2-pv.png)
    

## Tarea 2: Servidor de Directorios (instanceType=Directorio)

Como parte de este laboratorio, crearemos un POD (oudpod1) que consta de un único contenedor basado en una imagen PS4 (12.2.1.4.0) de Oracle Unified Directory 12c.

1.  Para crear el POD, realice una copia del archivo oud-dir-pod.yaml actualizada con los valores adecuados.
    
        	<copy>cd /u01/k8siam/fmw-kubernetes/OracleUnifiedDirectory/kubernetes/samples/</copy>
        
    
        	<copy>cp /u01/sampleFilesOUD/oud-dir-pod.yaml .</copy>
        
    
    Este archivo de ejemplo incluye los siguientes parámetros actualizados:
    
    Espacio de nombres - myoudns
    
    Etiqueta de imagen de Oracle - oracle/oud:12.2.1.4.0
    
    Nombre secreto - oudsecret
    
    Nombre PV: oudpv
    
    Nombre PVC - oudpvc
    
    No de entradas de muestra - 15
    
2.  Aplique el archivo.
    
        	<copy>kubectl apply -f oud-dir-pod.yaml</copy>
        
3.  Compruebe el estado del pod creado:
    
        	<copy>kubectl get pods -n myoudns</copy>
        
    
    ![pods de OUD](images/3-pods.png)
    
    Para almacenar los registros del contenedor durante la inicialización, utilice el siguiente comando:
    
        	<copy>kubectl --namespace myoudns logs -f -c oudds1 oudpod1</copy>
        
    
    Pulse Ctrl+C para salir de los logs de contenedor
    
4.  Para validar que la instancia del servidor de directorios de Oracle Unified Directory se está ejecutando, conéctese al contenedor:
    
        	<copy>kubectl --namespace myoudns exec -it -c oudds1 oudpod1 /bin/bash</copy>
        
5.  En el contenedor, ejecute ldapsearch para devolver entradas del servidor de directorios:
    
        	<copy>cd /u01/oracle/user_projects/oudpod1/OUD/bin</copy>
        
    
        	<copy>./ldapsearch -h localhost -p 1389 -D "cn=Directory Manager" -w Welcom@123 -b "" -s sub "(objectclass=*)" dn</copy>
        
    
    ![Entradas de Usuario](images/4-oud.png)
    
        	<copy>exit</copy>
        

## Más información

*   [Referencia para Oracle Unified Directory en Docker y Kubernetes](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/oamkd/overview.html#GUID-38F207C8-E648-4A79-8205-942DAD5F674A)

## Reconocimientos

*   **Autor**: Keerti R, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, enero de 2022