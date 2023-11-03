# Tareas posteriores a la actualización

## Introducción

Este laboratorio le guiará por los pasos para reiniciar los servidores y completar el proceso de actualización de un salto.

_Tiempo de laboratorio estimado_: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Reinicie los servidores para completar la actualización
*   Verificar el proceso de actualización

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Laboratorio: Requisitos Antes de la Actualización
    *   Laboratorio: actualización de un salto

## Tarea 1: Inicio del servidor de administración y los servidores gestionados 12c

1.  Navegue al directorio de dominio 12c e inicie los servidores en el orden indicado a continuación
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
2.  Iniciar el Servidor de Administración
    
        <copy>nohup ./startWebLogic.sh &</copy>
        
3.  Una vez iniciado el servidor de administración, acceda a la consola de Weblogic desde el explorador. Haga clic en el marcador _Enlaces de taller_ y haga clic en _WLS12c_ "O" para pegar la siguiente URL en el explorador:
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  En la consola de Weblogic, haga clic en _Servidores_ en _Entorno_ y observe que todos los servidores (OIM,SOA) tienen el estado 'SHUTDOWN'.
    
5.  Abra otro separador en el terminal. Navegue a la ruta de acceso _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_ e inicie el servidor SOA
    
        <copy>nohup ./startManagedWebLogic.sh soa_server1 t3://onehopiam:7005 -Dbpm.enabled=true &</copy>
        
    
    Refresque la consola de Weblogic y observe que el servidor SOA ahora tiene el estado 'RUNNING'. El servidor SOA puede tardar entre 5 y 8 minutos en iniciarse.
    
6.  Abra otro separador en el terminal. Navegue hasta la ruta _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_ e inicie el servidor de OIM.
    
        <copy>nohup ./startManagedWebLogic.sh oim_server1 t3://onehopiam:7005 &</copy>
        
    
    Esta vez, se ejecutará el proceso de inicialización de OIM y, después de la inicialización de datos correcta, el servidor gestionado de OIM se cerrará automáticamente.
    
7.  Después de que el servidor de OIM se cierre automáticamente, verifique que no se esté ejecutando ningún proceso de OIM ejecutando el siguiente comando:
    
        <copy>ps -ef |grep oim_server1</copy>
        
8.  Pare el servidor SOA. Navegue a la ruta de acceso _`/u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin`_ y pare el servidor SOA
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
9.  Parar el Servidor de Administración
    
        <copy>./stopWebLogic.sh</copy>
        

## Tarea 2: Verificación del proceso de actualización

1.  Ejecute la secuencia de comandos _startDomain12c.sh_ para reiniciar el dominio 12c. El servidor de administración tardará entre 3 y 4 minutos en iniciarse. Los servidores SOA y OIM pueden tardar unos 10 minutos en iniciarse.
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./startDomain12c.sh</copy>
        
2.  Acceda a la consola de Weblogic 12c desde el explorador. Haga clic en el marcador _Enlaces de taller_ y haga clic en _WLS12c_ "O" para pegar la siguiente URL en el explorador:
    
        <copy>http://onehopiam:7005/console</copy>
        
    
        Username: <copy>weblogic</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
    
    En la consola de Weblogic, haga clic en _Servidores_ en _Entorno_ y verifique que todos los servidores (OIM,SOA) tengan el estado 'EN EJECUCIÓN'.
    
3.  Acceda a la consola de Identity Self Service. Haga clic en el marcador _Enlaces de taller_ y haga clic en _OIG12c_ "O" para pegar la siguiente URL en el explorador:
    
        <copy>http://onehopiam:14005/identity</copy>
        
    
        Username: <copy>xelsysadm</copy>
        
    
        Password: <copy>Welcom@123</copy>
        
4.  Haga clic en _xelsysadm_ en la esquina superior derecha y haga clic en _About_ (Acerca de) en la lista desplegable. Verifique que la versión de OIM es 12c
    
    ![](images/1-identity.png)
    
5.  Haga clic en _Gestionar_ en la esquina superior derecha. A continuación, haga clic en _Usuarios_ y observe que los tres usuarios _TUSER1, TUSER2, TUSER3_ se migran de 11g a 12c.
    
    ![](images/2-users.png)
    

Se ha completado la actualización de un salto a Oracle Identity Manager 12c.

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, junio de 2021