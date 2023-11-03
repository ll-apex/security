# Verificar entorno e inicializar

## Introducción

En este laboratorio, revisaremos la configuración del entorno e iniciaremos todos los servicios necesarios para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Inicialización del entorno](videohub:1_9mdts598)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Verificar el docker
*   Iniciar servicios de OIG
*   Verificar el estado de todos los servidores
*   Verificar la dirección IP privada de la instancia informática

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento válido de Oracle OCI, con privilegios de administrador de OCI.
    
    _Nota:_ Desde este laboratorio en adelante hasta la finalización del taller, todos los pasos se deben realizar solo dentro de la URL del taller.
    

## Tarea 1: Validar que Docker está activo y en ejecución

1.  Abra una sesión de terminal.
    
    ![Abrir terminal](images/open-terminal.png)
    
    La sesión de terminal ha comenzado.
    
    ![Ventana de terminal](images/terminal-window.png)
    
2.  Compruebe la versión del docker.
    
        <copy>docker -v</copy>
        
    
    ![Comprobar la versión de docker](images/docker-version.png)
    
        Expected output: Docker version 23.0.0, build e92dd87
        
3.  Valide el estado para verificar si el servicio docker está activo/en ejecución
    
        <copy>systemctl status docker</copy>
        
    
    ![Validar el estado de docker](images/docker-info.png)
    
    Introduzca **Ctrl+C** para volver al símbolo del sistema.
    

## Tarea 2: Inicio de los Servicios de Oracle Identity Governance (OIG)

1.  Desplácese al directorio donde se encuentran los archivos de comandos.
    
        <copy>cd /scratch/idmqa/scripts</copy>
        
    
    ![Mover a ubicación de archivos de script](images/script-file.png)
    
2.  Muestre una lista de los archivos del directorio.
    
        <copy>ls</copy>
        
    
    ![Lista de archivos del directorio](images/list-files.png)
    
3.  Inicie la base de datos y todos los servidores manualmente mediante los siguientes scripts.
    
        <copy>./start_db.sh</copy>
        
    
    Espere a que se inicie la base de datos. A continuación, inicie todos los servidores.
    
    ![Se ha iniciado el servidor de bases de datos](images/start-db.png)
    
    ![Se ha iniciado el servidor de bases de datos](images/db-started.png)
    
        <copy>./start_all_servers.sh</copy>
        
    
    ![Todos los servidores están iniciados](images/start-all-servers.png)
    

## Tarea 3: Verificación de la dirección IP privada de la instancia informática

1.  Inicie una ventana del explorador. Conéctese a la consola de OCI mediante la URL mencionada a continuación. Aparece la página de conexión de la cuenta de OCI. Introduzca el nombre de usuario y la contraseña proporcionados durante el registro.
    
        <copy>https://console.us-ashburn-1.oraclecloud.com/</copy>
        
2.  Haga clic en el icono del menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Seleccione _Compute_ en el _menú de navegación_. Seleccione _Instancias_ en la lista de productos.
    
    ![Mover a ubicación de archivos de script](images/oci-console.png) ![Lista de archivos del directorio](images/compute-instance.png)
    
3.  Anote la dirección IP privada de la instancia informática como referencia. Tendremos que utilizarlos en los laboratorios posteriores.
    
    ![Lista de archivos del directorio](images/private-ip.png)
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Edward Lu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023