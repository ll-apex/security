# Despliegue de OIRI en el nodo de Kubernetes local

## Introducción

Este laboratorio le guiará por los pasos necesarios para configurar Oracle Identity Role Intelligence, que implica configurar los archivos de configuración, crear la cartera e instalar el gráfico de Helm.

_Tiempo de laboratorio estimado_: 40 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Cargar las imágenes de docker de OIRI
*   Configurar los archivos de configuración de Kubernetes
*   Crear carteras OIRI y ding
*   Crear y predefinir el esquema de base de datos
*   Instalar el gráfico de Helm de OIRI

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Práctica: Despliegue de cluster de Kubernetes e inicio del servidor de OIG

## Tarea 1: Cargar las imágenes de docker de OIRI

El servicio OIRI consta de cuatro imágenes:

*   oiri: Servicio OIRI
*   oiri-cli: interfaz de línea de comandos OIRI
*   oiri-ding: Para la importación de datos
*   oiri-ui: Interfaz de usuario de Identity Role Intelligence

Siga los siguientes pasos para cargar estas imágenes de docker.

1.  Abra una sesión de terminal y cargue las imágenes de docker de OIRI.
    
        <copy>cd /u01/setup/oiri/oiri-12.2.1.4.210423</copy>
        
    
        <copy>docker load --input oiri-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ui-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-cli-12.2.1.4.210423.tar</copy>
        
    
        <copy>docker load --input oiri-ding-12.2.1.4.210423.tar</copy>
        
    
    ![Comandos de terminal para cargar las imágenes de docker de OIRI](images/load-docker-images.png)
    
    _Nota_:
    
    Puede cargar las imágenes de Docker de cualquiera de las siguientes maneras:
    
    *   [Uso de las imágenes de Docker de OIRI desde el archivo zip compartido](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-174D3A93-752E-4E5A-AF3F-0648E87BC0F0)
    *   \[Uso de las imágenes de Docker de Container Registry\] (https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/installing-oracle-identity-role-intelligence.html#GUID-B23F0B59-AF19-4C7F-A268-8B6F3B5FC6B0)
2.  Verifique las imágenes.
    
        <copy>docker images | grep 12.2.1.4.210423</copy>
        
    
    ![Verificar las imágenes](images/verify-docker-images.png)
    

## Tarea 2: Configurar los archivos de configuración

Configure los archivos necesarios para configurar la importación de datos (o la ingestión de datos) y el gráfico de Helm.

1.  Cree los directorios en NFS. NFS es un requisito previo y se utiliza para crear volúmenes persistentes para su uso en los nodos.
    
        <copy>mkdir /nfs/ding</copy>
        
    
        <copy>mkdir /nfs/oiri</copy>
        
    
    Cree el siguiente directorio para generar el archivo values.yaml que utilizará el gráfico de Helm. No es necesario que este directorio esté presente en NFS porque se utiliza sólo para almacenar el archivo values.yaml, que no es necesario compartir en el cluster.
    
        <copy>mkdir /u01/k8s/</copy>
        
    
    Asegúrese de tener permisos de escritura en los directorios creados.
    
        <copy>chmod -R 775 /nfs/ding/</copy>
        
    
        <copy>chmod -R 775 /nfs/oiri/</copy>
        
    
        <copy>chmod -R 775 /u01/k8s/</copy>
        
2.  Anote la IP privada de su instancia como se menciona en el archivo de hosts.
    
        <copy>vi /etc/hosts</copy>
        

Por ejemplo:

    ![Private IP of your instance](images/ip.png)
    

3.  Ejecute el contenedor _oiri-cli_.
    
        <copy>docker run -d --name oiri-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
\--group-add 54321  
oiri-cli:12.2.1.4.210423  
tail -f /dev/null \`\`

    Notice *--group-add 54321* where, 54321 is the `GROUP_ID`.
    `GROUP_ID` is the ID of the group having access to the volumes.
    

4.  Copie el contenido de configuración de Kube del cluster de Kubernetes. Copie el contenido del archivo _/home/oracle/.kube/config_ en un bloc de notas o en un portapapeles. Asegúrese de alejar y copiar todas las líneas del archivo.
    
        <copy>vi /home/oracle/.kube/config</copy>
        
    
    ![Copiar el contenido de configuración de Kube del cluster de Kubernetes](images/config.png)
    
5.  Vaya al contenedor _oiri-cli_.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
6.  Cree el archivo de configuración de Kube. Inserte el contenido copiado del archivo _/home/oracle/.kube/config_ y guarde el archivo.
    
        <copy>vi /app/k8s/config</copy>
        
    
        <copy>chmod 400 /app/k8s/config</copy>
        
    
    ![Crear el archivo de configuración de Kube y pegar el contenido copiado](images/kube-config.png)
    
7.  Verifique la versión de kubectl y el timón. Asegúrese de que los comandos _helm version_ y _kubectl version_ se ejecuten correctamente sin ninguna advertencia ni error y muestre la versión.
    
        <copy>kubectl version</copy>
        
    
        <copy>helm version</copy>
        
    
    ![Verificar la versión de kubectl y timón](images/kube-version.png)
    

_Nota: Si hay un error, podría significar que el contenido del archivo de configuración de Kube no se copia correctamente. Copie y pegue el contenido del archivo de configuración de kube según los pasos anteriores 4-6 y vuelva a intentarlo_

8.  Configurar archivos de configuración. Sustituya el parámetro `<VM IP>` por la dirección IP privada de la instancia indicada en el archivo _/etc/hosts_ del paso 2.
    
        <copy>./setupConfFiles.sh -m prod \
        

\--oigdbhost \--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost \--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://:6443  
\--oigserverurl http://:1400

Por ejemplo:

    ```
    

./setupConfFiles.sh -m prod  
\--oigdbhost 10.0.0.231  
\--oigdbport 1521  
\--oigdbsname oiri.livelabs.oraclevcn.com  
\--oiridbhost 10.0.0.231  
\--oiridbport 1521  
\--oiridbsname oiri.livelabs.oraclevcn.com  
\--sparkmode k8s  
\--dingnamespace ding  
\--dingimage oiri-ding:12.2.1.4.210423  
\--k8scertificatefilename ca.crt  
\--sparkk8smasterurl k8s://https://10.0.0.231:6443  
\--oigserverurl\`http://10.0.0.231:14000

    ![Terminal commands to set up configuration files](images/setup.png)
    

9.  Verifique que se hayan generado los archivos de configuración.
    
        <copy>ls /app/data/conf/</copy>
        
    
        <copy>ls /app/oiri/data/conf</copy>
        
10.  Configure el archivo values.yaml que se utilizará para el gráfico de Helm. Sustituya el parámetro `<VM IP>` por la dirección IP privada de la instancia indicada en el archivo _/etc/hosts_ del paso 2.
    
        <copy>./setupValuesYaml.sh \
        

\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver \--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver \--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingresshostname oiri  
\--sslsecretname "oiri-tls-cert" \`\`\`

Por ejemplo:

    ```
    

./setupValuesYaml.sh  
\--oiriapiimage oiri:12.2.1.4.210423  
\--oirinfsserver 10.0.0.231  
\--oirinfsstoragepath /nfs/oiri  
\--oirinfsstoragecapacity 10Gi  
\--oiriuiimage oiri-ui:12.2.1.4.210423  
\--dingimage oiri-ding:12.2.1.4.210423  
\--dingnfsserver 10.0.0.231  
\--dingnfsstoragepath /nfs/ding  
\--dingnfsstoragecapacity 10Gi  
\--ingresshostname oiri  
\--sslsecretname "oiri-tls-cert" \`\`\`

11.  Verifique que se ha generado values.yaml y salga del contenedor.
    
        <copy>ls /app/k8s/</copy>
        
    
        <copy>exit</copy>
        

## Tarea 3: Actualizar parámetros de entidad para la importación de datos

1.  Ejecute el siguiente comando para poder actualizar los parámetros de entidad para la importación de datos.
    
        <copy>docker run -d --name ding-cli \
        

\-v /nfs/ding/:/app/  
\-v /nfs/oiri/:/app/oiri  
\-v /u01/k8s/:/app/k8s  
oiri-ding:12.2.1.4.210423  
tail -f /dev/null \`\`\`

## Tarea 4: Creación de cartera

1.  Importe el certificado de OIG en el almacén de claves. Para ello, exporte el certificado de OIG para la verificación de firmas.
    
        <copy>cd /u01/oracle/config/domains/oig_domain/config/fmwconfig/</copy>
        
    
        <copy>keytool -export -rfc -alias xell -file xell.pem -keystore default-keystore.jks</copy>
        

Introduzca la contraseña del almacén de claves cuando se le solicite. `Password: <copy>Welcome1</copy>`

![Comandos de terminal para exportar certificado de OIG para verificación de firma](images/keystore.png)

_default-keystore.jks_ se encuentra en _DOMAIN\_HOME/config/fmwconfig_. El certificado que está exportando aquí protege la API de REST de OIG. No es lo mismo que el certificado del servidor de OIG.

2.  Copie el archivo _xell.pem_ exportado del almacén de claves de OIG en el directorio _/nfs/oiri/data/keystore/_.
    
        <copy>sudo cp /u01/oracle/config/domains/oig_domain/config/fmwconfig/xell.pem /nfs/oiri/data/keystore/</copy>
        
    
        <copy>sudo chown opc:users /nfs/oiri/data/keystore/xell.pem</copy>
        
    
        <copy>sudo ls -latr /nfs/oiri/data/keystore</copy>
        
    
    ![Copie el archivo xell.pem exportado del almacén de claves de OIG en /nfs/oiri/data/keystore/](images/opc.png)
    
3.  Genere un almacén de claves dentro del contenedor _oiri-cli_.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>keytool -genkeypair \
        

\-alias oirikey  
\-keypass Welcome1  
\-keyalg RSA  
\-keystore /app/oiri/data/keystore/keystore.jks  
\-storetype pkcs12  
\-storepass Welcome1 \`\`

La salida es:

    ```
    What is your first and last name?
    [Unknown]:
    What is the name of your organizational unit?
    [Unknown]:
    What is the name of your organization?
    [Unknown]:
    What is the name of your City or Locality?
    [Unknown]:
    What is the name of your State or Province?
    [Unknown]:
    What is the two-letter country code for this unit?
    [Unknown]:
    Is CN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=Unknown correct?
    [no]:  yes
    ```
    
    ![Terminal commands to Generate a keystore inside the oiri-cli container](images/keytool.png)
    

4.  Importe el certificado al almacén de datos OIRI.
    
        <copy>keytool -import \
        -alias xell \
        -file /app/oiri/data/keystore/xell.pem \
        -keystore /app/oiri/data/keystore/keystore.jks</copy>
        
    
    Introduzca la contraseña del almacén de claves cuando se le solicite.
    
        Password: <copy>Welcome1</copy>
        
    
    La salida es:
    
        Trust this certificate? [no]: yes
        
    
    ![Importar el certificado al almacén de datos OIRI](images/import-keytool.png)
    
5.  Cree la cartera.
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml wallet create</copy>
        
    
    Introduzca la siguiente información cuando se le solicite:
    
    *   Prefijo UserName de la base de datos OIRI. Por ejemplo: desarrollo, prueba, producción. El nombre del esquema será \_OIRI: **DEV**
    *   Contraseña de base de datos OIRI: **Welcome1**
    *   OIG DB UserName: **DEV\_OIM**
    *   Contraseña de base de datos de OIG: **Welcome1**
    *   Cuenta de servicio de OIG UserName: **xelsysadm**
    *   Contraseña de cuenta de servicio de OIG: **Welcome1**
    *   Contraseña KeyStore de OIRI: **Welcome1**
    *   Alias de clave JWT de OIRI: **oirikey**
    *   Contraseña de clave JWT de OIRI: **Welcome1**
    
    ![Crear la cartera](images/wallet.png)
    
6.  Compruebe que se han creado las carteras OIRI y Ding.
    
        <copy>ls /app/data/wallet</copy>
        
    
        <copy>ls /app/oiri/data/wallet</copy>
        
    
    ![Verificar las carteras OIRI y Ding creadas](images/createdb-wallet.png)
    

## Tarea 5: Crear y predefinir esquema de base de datos OIRI

1.  Cree el esquema de usuario de la base de datos.
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema create /app/data/conf/dbconfig.yaml</copy>
        

Introduzca la contraseña de usuario SYS cuando se le solicite.

    ```
    Password: <copy>Welcome1</copy>
    ```
    
    ![Terminal command to Create the database user schema](images/createdb-wallet.png)
    

2.  Inicia el esquema de base de datos.
    
        <copy>oiri-cli --config=/app/data/conf/config.yaml schema migrate /app/data/conf/dbconfig.yaml</copy>
        
3.  Verifique la cartera.
    
        <copy>./verifyWallet.sh</copy>
        
    
    ![Comando de terminal para verificar la cartera](images/verifywallet.png)
    
    Nota: Si falla la verificación de la cartera, utilice el comando _oiri-cli --config=/app/data/conf/config.yaml wallet update_ para corregir la entrada notificada con un problema
    

## Tarea 6: Instalación del gráfico de Helm de OIRI

1.  Cree los siguientes espacios de nombres.
    
        <copy>kubectl create namespace oiri</copy>
        
    
        <copy>kubectl create namespace ding</copy>
        
    
        <copy>exit</copy>
        
2.  Active SSL desde una máquina host de contenedor de Docker que esté fuera del contenedor oiri-cli.
    
        <copy>cd ~</copy>
        
    
        <copy>openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=oiri"</copy>
        
    
        <copy>kubectl create secret tls oiri-tls-cert --key="tls.key" --cert="tls.crt"</copy>
        
3.  Instale el gráfico de timón.
    
        <copy>docker exec -it oiri-cli /bin/bash</copy>
        
    
        <copy>helm install oiri /helm/oiri -f /app/k8s/values.yaml</copy>
        
    
    ![Comandos de terminal para instalar el gráfico de timón](images/helm.png)
    
4.  Muestre los pods y asegúrese de que se están ejecutando todos los pods. Además, compruebe si los pods bajo el espacio de nombres ding y oiri están en ejecución y en estado listo 1/1.
    
        <copy>kubectl get pods --all-namespaces</copy>
        
    
        <copy>kubectl get pods -n ding</copy>
        
    
        <copy>kubectl get pods -n oiri</copy>
        
    
        <copy>exit</copy>
        
    
    ![Enumerar los pods](images/running-pods.png)
    
    ![Compruebe que los pods bajo el espacio de nombres ding y oiri están en ejecución y el estado READY 1/1](images/ready-pods.png)
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD, diciembre de 2022