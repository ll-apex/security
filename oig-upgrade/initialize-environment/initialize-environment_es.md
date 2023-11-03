# Inicializar entorno

## Introducción

En este laboratorio revisaremos e iniciaremos todos los componentes necesarios para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 20 minutos

### Acerca de Producto/Tecnología

Oracle Identity Governance (OIG) es un sistema de gestión de identidad empresarial potente y flexible que gestiona automáticamente los privilegios de acceso del usuario en los recursos de TI de la empresa.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Inicialice el entorno del taller.
*   Verifique el estado de la base de datos.
*   Verifique el estado del dominio 11g disponible para la actualización.
*   Verifique el estado del dominio 12c.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

## Tarea 1: Validar que los procesos necesarios estén activos y en ejecución.

1.  Ahora, con acceso a la sesión de escritorio remoto, continúe como se indica a continuación para validar el entorno antes de empezar a ejecutar las prácticas posteriores. Los siguientes procesos deben estar activos y en ejecución:
    
    *   listener de la base de datos
    *   Servidor de base de datos
    *   Servidor de administración (el servidor de administración tardará entre 3 y 4 minutos en iniciarse)
    *   Servidor OIM y SOA (los servidores SOA y OIM tardarán entre 10 y 12 minutos en iniciarse)
2.  En la ventana _Firefox_ de la consola Weblogic 11g precargada derecha, si no, refresque la página o espere entre 2 y 3 minutos para iniciar el servidor de administración y, a continuación, haga clic en el campo _Nombre de usuario_ y seleccione las credenciales guardadas para conectarse. Estas credenciales se han guardado en _Firefox_ y se proporcionan a continuación como referencia.
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/oig-vnc.png " ")
    
3.  El servidor de administración tardará entre 3 y 4 minutos en iniciarse. Los servidores SOA y OIM pueden tardar entre 10 y 12 minutos en iniciarse.
    
    *   En la consola de Weblogic 11g, haga clic en _Servidores_ en _Entorno_ y verifique que todos los servidores (OIM,SOA) tengan el estado 'EN EJECUCIÓN'. ![](images/oig-vnc2.png " ")
        
    *   En la ventana _Firefox_ del 3er tabulador precargado derecho se encuentra la consola de Weblogic 12c.
        
            Username: <copy>weblogic</copy>
            
        
            Password: <copy>Welcom@123</copy>
            
    
    ![](images/oig-vnc3.png " ")
    
    *   Haga clic en _Servidores_ en _Entorno_ y verifique que todos los servidores (OIM,SOA) tengan el estado 'EN EJECUCIÓN'. ![](images/oig-vnc4.png " ")
    
    Si se realiza correctamente, la página anterior se muestra con la ejecución de todo el servidor, su entorno ya está listo.
    
4.  Si aún no puede iniciar sesión o la página de inicio de sesión no funciona después de volver a cargarla desde la carpeta de marcadores _Enlaces del Taller_, abra una sesión de terminal y continúe como se indica a continuación para validar los servicios.
    
    *   Base de datos y listener
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![](images/1-database.png " ")
        
    *   Servidor de Administración de WLS, Servidor de OIM
        
            <copy>
            sudo systemctl status oig-11g.service
            </copy>
            
        
        ![](images/oig-11gservice.png " ")
        
            <copy>
            sudo systemctl status oig-12c.service
            </copy>
            
        
        ![](images/oig-12cservice.png " ")
        

## Tarea 2: Verificación del dominio 11g y 12c

1.  Acceda a la consola de Identity Self Service. Haga clic en el marcador _Enlaces de taller_ y haga clic en _OIG11g_ "O" para pegar la siguiente URL en el explorador:
    
        <copy>http://onehopiam:14000/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/5-identity-console.png)
    
2.  Haga clic en _xelsysadm_ en la esquina superior derecha y haga clic en _About_ (Acerca de) en la lista desplegable. Verifique que la versión de OIM es 11g
    
    ![](images/6-identity-console.png)
    
3.  Haga clic en _Gestionar_ en la esquina superior derecha. A continuación, haga clic en _Usuarios_ y observe que se han creado 3 usuarios de prueba (_TUSER1, TUSER2, TUSER3_)
    
    ![](images/7-users.png)
    
    ![](images/8-users.png)
    

4.  Acceda a la consola de Identity Self Service OIG-12c. Haga clic en el marcador _Enlaces de taller_ y haga clic en _OIG12c_ "O" para pegar la siguiente URL en el explorador:
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    ![](images/11-oim12c.png)
    
5.  Haga clic en _xelsysadm_ en la esquina superior derecha y haga clic en _About_ (Acerca de) en la lista desplegable. Verifique que la versión de OIM es 12c
    
    ![](images/12-oim12c.png)
    
6.  Haga clic en _Gestionar_ en la esquina superior derecha. A continuación, haga clic en _Usuarios_ y observe que no se han creado nuevos usuarios.
    
    ![](images/13-oim12c.png)
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Apéndice 1: Gestión de Servicios de Inicio

1.  Servicio de base de datos (base de datos y listener).
    
        Start: <copy>sudo systemctl start oracle-database</copy>
        
    
        Stop: <copy>sudo systemctl stop oracle-database</copy>
        
    
        Status: <copy>sudo systemctl status oracle-database</copy>
        
    
        Restart: <copy>sudo systemctl restart oracle-database</copy>
        
2.  Servicio OIG - versión 11g (servidor OIG del servidor de administración WLS)
    
        Start: <copy>sudo systemctl start oig-11g.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-11g.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-11g.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-11g.service</copy>
        
3.  Servicio OIG - versión 12c (servidor OIG del servidor de administración WLS)
    
        Start: <copy>sudo systemctl start oig-12c.service</copy>
        
    
        Stop: <copy>sudo systemctl stop oig-12c.service</copy>
        
    
        Status: <copy>sudo systemctl status oig-12c.service</copy>
        
    
        Restart: <copy>sudo systemctl restart oig-12c.service</copy>
        

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/fecha**: Ashish Kumar, ingeniería de soluciones NATD, junio de 2021