# Para establecer la conexión con Oracle Database y Oracle Identity Governance

## Introducción

Establecimiento de la conexión a Oracle Database y Oracle Identity Governance

*   Persona: administrador de dominio de identidad

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_61qwyi4k)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Para establecer la conexión con Oracle Database y Oracle Identity Governance

## Tarea 1: Verifique que Docker está activo y en ejecución

1.  Abra una sesión de terminal.
    
2.  Compruebe la versión del docker.
    
        <copy>docker -v</copy>
        
    
        Expected output: Docker version 23.0.0, build e92dd87
        
3.  Valide el estado para verificar si el servicio docker está activo/en ejecución
    
        <copy>systemctl status docker</copy>
        
    
    Introduzca **Ctrl+C** para volver al símbolo del sistema.
    

## Tarea 2: Inicio del servicio de base de datos de Oracle Identity Governance (OIG)

1.  Desplácese al directorio donde se encuentran los archivos de comandos.
    
        <copy>cd /scratch/idmqa/scripts</copy>
        
2.  Muestre una lista de los archivos del directorio.
    
        <copy>ls</copy>
        
3.  Inicie la base de datos y todos los servidores manualmente mediante los siguientes scripts.
    
        <copy>./start_db.sh</copy>
        
    
    Espere a que se inicie la base de datos.
    
4.  Ahora inicie los servicios de OIG con el siguiente comando.
    
        <copy>./start_all_servers.sh</copy>
        

## Tarea 3: Integración con Oracle Identity Governance

1.  En la página inicial del servicio Oracle Access Governance _consulte el laboratorio 6:Tarea 1_, haga clic en el icono del menú de navegación, seleccione **Administración de servicios** y, a continuación, **Sistemas conectados**.
    
    ![Consola de Access Governance - Sistemas Conectados](images/connected-systems.png)
    
2.  Haga clic en **Agregar un sistema conectado**.
    
    ![Agregar - Sistema conectado](images/add-connected-system.png)
    
3.  En el mosaico con la etiqueta **¿Desea conectarse a un sistema de gobernanza de identidades?**, seleccione el botón **Agregar**. ![Consola de Access Governance - Connected Systems-Add](images/connected-system-page.png)
    
4.  Haga clic en **Close** en la ventana emergente de información para ir a la página **Add an Identity Governance System** e iniciar la configuración.
    
    ![Cerrar la ventana emergente](images/pop-up.png)
    
5.  En el paso **Seleccionar sistema**, seleccione el mosaico de **Oracle Identity Governance** para configurar el agente para un sistema conectado de destino de Oracle Identity Governance y, a continuación, haga clic en **Siguiente**.
    
    ![Consola de Access Governance - Sistemas conectados-Siguiente](images/select-oig.png)
    
6.  En el paso **Introducir detalles**, introduzca los siguientes detalles:
    
    *   **Nombre:** oag
    *   **Descripción:** oag
    *   **Haga clic en Next.**
    
    ![Consola de Access Governance - Sistemas conectados-OIG](images/oag-select-system.png)
    
7.  En el paso **Configurar**, introduzca los detalles de conexión del sistema de destino:
    
    **URL de JDBC:** sustituye el marcador de posición de la siguiente URL por la IP privada de la instancia informática. Consulte el _laboratorio 5: Tarea 3_ para obtener la IP privada de la instancia informática.
    
        <copy>jdbc:oracle:thin:@//<--privateipofyourcomputeinstance-->:1521/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM</copy>
        
    
    **Nombre de usuario de base de datos de OIG:**
    
        <copy>DEV_OIM</copy>
        
    
    **Contraseña:**
    
        <copy>Welcome1</copy>
        
    
    **Confirmar contraseña:**
    
        <copy>Welcome1</copy>
        
    
    **URL de servidor de OIG:** sustituya el marcador de posición de la siguiente URL por la IP privada de la instancia informática. Consulte el _laboratorio 3: Tarea 3_ para obtener la IP privada de la instancia informática.
    
        <copy>http://<--privateipofyourcomputeinstance-->:14000</copy>
        
    
    **Nombre de usuario del servidor de OIG:**
    
        <copy>xelsysadm</copy>
        
    
    **Contraseña de usuario del servidor de OIG:**
    
        <copy>Welcome1</copy>
        
    
    **Confirmar Contraseña de Servidor de OIG:**
    
        <copy>Welcome1</copy>
        
    
    ![Configurar Detalles](images/oag-connection-details.png)
    
8.  En el paso Descargar agente, seleccione el _enlace Descargar_ y descargue el archivo zip del agente. El archivo zip está presente en: /home/opc/Downloads
    
    ![Descargar el agente](images/oag-download-link.png)
    
9.  Puede verificar el archivo zip del agente descargado.
    
    ![Navegar al sistema de archivos](images/locate-zip.png)
    
    ![Verifique el archivo zip](images/verify-zip.png)
    

## Tarea 4: Instalación del agente de OAG en la instancia informática y configuración

1.  Abra la sesión de terminal.
    
    ![Abrir sesión de terminal](images/open-terminal-window.png)
    
2.  Mueva el archivo zip descargado (oag.zip) presente en la carpeta /home/opc/Downloads a la carpeta /home/opc/zip\_oag.
    
        <copy>mv /home/opc/Downloads/oag.zip /home/opc/zip_oag</copy>
        
    
    ![Mueva el agente de OAG a zip_oag](images/move-oag-agent.png)
    
    Verifique que el zip del agente (oag.zip) está presente en la carpeta zip\_oag.
    
        <copy>cd /home/opc/zip_oag</copy>
        <copy>ls</copy>
        
    
    ![Verifique el zip del agente oag.zip](images/env_setup.png)
    
3.  Definición de las variables de entorno con el siguiente comando:
    
        <copy>cd ~</copy>
        <copy>source oag_agent.env</copy>
        
    
    ![Inicializar la variable de entorno](images/terminal-oag.png)
    
4.  Instalar el agente
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --agentpackage /home/opc/zip_oag/oag.zip --install</copy>
        
    
    ![Instalar el agente](images/agent-install.png)
    
5.  Iniciar el agente
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --start</copy>
        
    
    ![Iniciar el agente](images/agent-start.png)
    
6.  Verifique el agente
    
        <copy>sh agentManagement.sh --volume /home/opc/vol_oag --status</copy>
        
    
    ![Verifique el agente](images/agent-status.png)
    

## Tarea 5: Conexión a Oracle Database y descarga del agente de base de datos

1.  Navegue a la página Sistemas conectados de la consola de Oracle Access Governance siguiendo estos pasos: en el menú de navegación del icono del menú de navegación de Oracle Access Governance, seleccione Administración de servicios → Sistemas conectados. Haga clic en el botón Agregar un sistema conectado para iniciar el flujo de trabajo.
    
2.  En la página Agregar un sistema conectado, seleccione el botón Agregar en el mosaico ¿Desea conectarse a un sistema de gestión de bases de datos?
    
3.  En el paso Select system del flujo de trabajo, seleccione Database User Management (Oracle DB) y haga clic en Next.
    
4.  En el paso Introducir detalles del flujo de trabajo, escriba los detalles del sistema conectado:
    
         * What do you want to call your database : OAG-DB
         * How do you want to describe this database: OAG-DB
        
    
    Haga clic en Next.
    
5.  En el paso Configurar del flujo de trabajo, introduzca los detalles de configuración necesarios para permitir que Oracle Access Governance se conecte a la base de datos de destino.
    
         * Easy Connect URL for Database: jdbc:oracle:thin:@//<—privateipaddressofcomputeinstance-->/ORCL.NETWORKSPEOSUBN.IDMOCICLOU02PHX.ORACLEVCN.COM
        
         * User Name: sys as sysdba
        
         * Password: Welcome1
        
         * Confirm password: Welcome1
        
6.  Marque el panel de la derecha para ver lo que he seleccionado. Si está satisfecho con los detalles introducidos, seleccione Agregar para crear el sistema conectado.
    
7.  En el paso de finalización del flujo de trabajo, se le pedirá que descargue el agente que utilizará para interactuar entre Oracle Access Governance y Oracle Database. Seleccione el enlace Download para descargar el archivo zip del agente en el entorno en el que se ejecutará el agente.
    

## Tarea 6: Instalación del agente de base de datos en el sistema de destino

1.  Abra el terminal.
    
2.  Cree el volumen.
    
        <copy>mkdir  /home/opc/vol_oag_db</copy>
        
3.  Verifique que el zip del agente (OAG-DB.zip) está presente en las descargas de carpetas.
    
        <copy> cd /home/opc/Downloads
        ls</copy>
        
4.  Definición de las variables de entorno con el siguiente comando:
    
        <copy> cd ~
        source oag_agent.env</copy>
        
5.  Instale el agente con los siguientes comandos:
    
        <copy>curl https://raw.githubusercontent.com/oracle/docker-images/main/OracleIdentityGovernance/samples/scripts/agentManagement.sh -o agentManagement.sh; 
        

\`\`\`\`\` sh agentManagement.sh --volume /home/opc/vol\_oag\_db --agentpackage /home/opc/Downloads/OAG-DB.zip --install \`\` 3. Inicie el agente con el siguiente comando:

      ```
      <copy>sh agentManagement.sh --volume /home/opc/vol_oag_db --start</copy>
      ``` 
    

## Tarea 7: Verificación de la instalación del agente

1.  Conéctese a la consola de Oracle Access Governance, seleccione el icono de menú de navegación para mostrar el menú de navegación.
2.  En Oracle Access Governance Console, seleccione Administración de servicios → Sistemas conectados en el menú de navegación.
3.  En la pantalla Sistemas conectados, el mosaico que muestra el nombre del sistema conectado muestra el estado Esperando la conexión inicial. Haga clic en Manage Connection.
4.  El registro de actividad de la parte inferior de la página mostrará el estado de la operación Validar, Pendiente mientras el agente se activa. Si el agente no aparece, compruebe los logs de instalación y funcionamiento del agente para detectar cualquier incidencia.
5.  Una vez que aparezca el agente, el estado de la operación Validar se mostrará como Correcto.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu