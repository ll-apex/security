# Instalar y configurar el agente de Oracle Access Governance

## Introducción

Se instalará y configurará el agente de OAG.

_Tiempo de laboratorio estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Instalación de Oracle Access Governance Agent](videohub:1_u4xrvpak)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Descargue el agente para realizar la integración con OIG
*   Instale y configure el **agente de OAG**.

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento válido de Oracle OCI, con privilegios de administrador de OCI.

## Tarea 1: Integración con Oracle Identity Governance

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
    

## Tarea 2: Instalación del agente de OAG en la instancia informática y configuración

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