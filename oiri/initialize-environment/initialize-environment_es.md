# Inicializar entorno

## Introducción

En este laboratorio revisaremos e iniciaremos todos los componentes necesarios para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 20 minutos

### Acerca de Producto/Tecnología

Oracle Identity Role Intelligence (OIRI) es un sistema de gestión de identidad empresarial potente y flexible que gestiona automáticamente los privilegios de acceso del usuario en los recursos de TI de la empresa.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Inicialice el entorno del taller.
*   Verifique el estado de la base de datos.
*   Verifique el estado del dominio 12c.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

## Tarea 1: Validar que los procesos necesarios estén activos y en ejecución.

1.  Ahora, con acceso a la sesión de escritorio remoto, continúe como se indica a continuación para validar el entorno antes de empezar a ejecutar las prácticas posteriores. Los siguientes procesos deben estar activos y en ejecución:
    
    *   listener de la base de datos
    *   Servidor de base de datos
    *   Servidor de administración (el servidor de administración tardará entre 3 y 4 minutos en iniciarse)
2.  En la ventana _Explorador Web_ de la consola Weblogic 12c precargada derecha, haga clic en el campo _Nombre de Usuario_ y seleccione las credenciales guardadas para conectarse. Estas credenciales se han guardado en el _Explorador web_ y se proporcionan a continuación como referencia.
    
        Username  weblogic
        Password  Welcome1
        
    
    ![Página de inicio de sesión de la consola de Weblogic](images/oiri-vnc.png " ")
    
3.  Confirme el inicio de sesión correcto. Tenga en cuenta que se tarda unos 5 minutos después del aprovisionamiento de la instancia para que todos los procesos se inicien por completo.
    
    *   En la consola de Weblogic, haga clic en _Servidores_ en _Entorno_ y verifique que el servidor de administración tenga el estado 'EN EJECUCIÓN'. ![Haga clic en los servidores en Environment](images/oiri-landing.png " ")
    
    Si se realiza correctamente, se muestra la página anterior y, como resultado, su entorno ya está listo.
    
    Ahora puede [proceder al siguiente laboratorio](#next).
    
4.  Si aún no puede iniciar sesión o la página de inicio de sesión no funciona después de volver a cargar desde la URL mencionada a continuación, abra una sesión de terminal y continúe como se indica a continuación para validar los servicios.
    
        ```
        
    
    URL http://oiri.livelabs.oraclevcn.com:7001/console/login/ Contraseña de WebLogic de nombre de usuario Welcome1 \`\`\`
    
    *   Base de datos y listener
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![Comando de ventana de terminal para validar el estado de la base de datos](images/db.png " ")
        
    *   Servidor de administración de WLS y gestor de nodos
        
            <copy>
            sudo systemctl status oiri-weblogic.service
            </copy>
            
        
        ![Comando de ventana de terminal para validar el estado del servidor de administración de WLS](images/oiri-wls-service.png " ")
        
            <copy>
            sudo systemctl status oiri-node.service
            </copy>
            
        
        ![Comando de ventana de terminal para validar el estado del gestor de nodos](images/oiri-node-service.png " ")
        
5.  Si ve salidas cuestionables, fallos o componentes inactivos, reinicie los servicios correspondientes según corresponda
    
    *   Base de datos y listener
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   Servidor de administración de WLS y gestor de nodos
        
            <copy>
            sudo systemctl restart oiri-weblogic.service
            </copy>
            
        
            <copy>
            sudo systemctl restart oiri-node.service
            </copy>
            

Ahora puede [proceder al siguiente laboratorio](#next).

## Apéndice 1: Gestión de Servicios de Inicio

1.  Servicio de base de datos (base de datos y listener).
    
    *   Iniciar
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   Parar
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   Estado
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   Reiniciar
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  Servicio OIRI (servidor de administración de WLS)
    
    *   Iniciar
    
        <copy>sudo systemctl start  oiri-weblogic.service</copy>
        
    
    *   Parar
    
        <copy>sudo systemctl stop oiri-weblogic.service</copy>
        
    
    *   Estado
    
        <copy>sudo systemctl status oiri-weblogic.service</copy>
        
    
    *   Reiniciar
    
        <copy>sudo systemctl restart oiri-weblogic.service</copy>
        
3.  Servicio OIRI (Servicio del Gestor de Nodos)
    
    *   Iniciar
    
        <copy>sudo systemctl start oiri-node.service</copy>
        
    
    *   Parar
    
        <copy>sudo systemctl stop oiri-node.service</copy>
        
    
    *   Estado
    
        <copy>sudo systemctl status oiri-node.service</copy>
        
    
    *   Reiniciar
    
        <copy>sudo systemctl restart oiri-node.service</copy>
        

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD