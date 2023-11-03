# Inicializar entorno

## Introducción

En este laboratorio revisaremos e iniciaremos todos los componentes necesarios para ejecutar correctamente este taller.

Tiempo estimado: 10 minutos.

### Objetivos

*   Inicialice el entorno del taller.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Ha finalizado:
    *   Laboratorio: preparación de la configuración
    *   Laboratorio: Configuración del entorno

## Tarea 1: Validar que los procesos necesarios estén activos y en ejecución.

**Nota:** Todas las capturas de pantalla de las tareas de tipo de terminal SSH destacadas en este taller se capturaron mediante el cliente SSH _MobaXterm_, como se describe en este paso. Como resultado, al ejecutar estas tareas desde la sesión de escritorio remoto gráfico, omita los pasos que requieren que se conecte como usuario _oracle_ mediante _sudo su - oracle_, ya que la sesión de escritorio remoto está en el usuario _oracle_.

1.  Ahora, con acceso a la sesión de escritorio remoto, continúe como se indica a continuación para validar el entorno antes de empezar a ejecutar las prácticas posteriores. Los siguientes procesos deben estar activos y en ejecución:
    
    *   listener de la base de datos
    *   Servidores de base de datos (emcdb y cdb1)
    *   Enterprise Manager - Servidor de Gestión (OMS)
    *   Enterprise Manager - Management Agent (emagente)
    *   Mis aplicaciones de RR. HH. en Glassfish
2.  En la ventana del explorador web de la derecha hay un separador precargado con _Enterprise Manager_, conéctese con las credenciales siguientes para validar que está operativo. Si la página de inicio de sesión no aparece en el primer inicio de sesión en el escritorio remoto, actualice para volver a cargarla. Todos los procesos tardan aproximadamente 15 minutos en iniciarse por completo.
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
    ![Conexión a Enterprise Manager](images/em-login.png "Conexión a Enterprise Manager")
    
3.  Abra nuevos separadores del explorador y confirme que la representación de _Mis aplicaciones de RR. HH._ se ha realizado correctamente.
    
    *   PDB1
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb1</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb1</copy>
        
    
    *   PDB2
    
        Prod: <copy>http://dbsec-lab:8080/hr_prod_pdb2</copy>
        
    
        Dev: <copy>http://dbsec-lab:8080/hr_dev_pdb2</copy>
        
    
    Si todos son correctos, el entorno está listo.
    
4.  Si aún no puede obtener todos los _Enterprise Manager_ y todos los enlaces anteriores para que se representen correctamente, abra una sesión de terminal y continúe como se indica a continuación para validar los servicios.
    
    *   Servicios de base de datos (todas las bases de datos y el listener estándar)
        
            <copy>
            sudo systemctl status oracle-database
            </copy>
            
        
        ![Estado de servicio de BD](images/db-service-status.png "Estado de servicio de BD")
        
    *   DBSec-lab Service (Enterprise Manager 13c y My HR Applications on Glassfish)
        
            <copy>
            sudo systemctl status oracle-dbsec-lab
            </copy>
            
        
        ![DBSecLab Estado del servicio](images/dbsec-lab-service-status.png "DBSecLab Estado del servicio")
        
5.  Si ve salidas cuestionables, fallos o componentes inactivos, reinicie los servicios correspondientes según corresponda
    
    *   Base de datos y listener
        
            <copy>
            sudo systemctl restart oracle-database
            </copy>
            
    *   DBSec: servicio de laboratorio
        
            <copy>
            sudo systemctl restart oracle-dbsec-lab
            </copy>
            

Ahora puede **proceder al siguiente laboratorio**.

## Apéndice 1: Gestión de Servicios de Inicio

1.  Servicios de base de datos (todas las bases de datos y el listener estándar)
    
    *   Iniciar
    
        <copy>sudo systemctl start oracle-database</copy>
        
    
    *   Parar
    
        <copy>sudo systemctl stop oracle-database</copy>
        
    
    *   Estado
    
        <copy>sudo systemctl status oracle-database</copy>
        
    
    *   Reiniciar
    
        <copy>sudo systemctl restart oracle-database</copy>
        
2.  DBSec-lab Service (Enterprise Manager 13c y My HR Applications on Glassfish)
    
    *   Iniciar
    
        <copy>sudo systemctl start oracle-dbsec-lab</copy>
        
    
    *   Parar
    
        <copy>sudo systemctl stop oracle-dbsec-lab</copy>
        
    
    *   Estado
    
        <copy>sudo systemctl status oracle-dbsec-lab</copy>
        
    
    *   Reiniciar
    
        <copy>sudo systemctl restart oracle-dbsec-lab</copy>
        

## Apéndice 2: Acceso Web Externo

Si por algún motivo desea iniciar sesión desde una ubicación externa a su sesión de escritorio remoto, como su estación de trabajo/ordenador portátil, consulte los detalles a continuación.

1.  Consola de Enterprise Manager 13c
    
        Username: <copy>sysman</copy>
        
    
        Password: <copy>Oracle123</copy>
        
    
        URL: <copy>http://<Your Instance public_ip>:7803/em</copy>
        
    
    *   _Nota:_ Puede que aparezca un error en el explorador al acceder a la consola web: "_Su conexión no es privada_", como se muestra a continuación. Ignore y agregue la excepción para continuar.
    
    ![Conexión Externa a Enterprise Manager](images/login-em-external-1.png "Conexión Externa a Enterprise Manager") ![Conexión Externa a Enterprise Manager](images/login-em-external-2.png "Conexión Externa a Enterprise Manager")
    
2.  Mis aplicaciones de RR. HH. en Glassfish
    
    *   PDB1
        *   Producto: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb1`
        *   Desarrollo: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb1` (bg: rojo)
    *   PDB2
        *   Producto: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_prod_pdb2` (menú: rojo)
        *   Dev: `http://<YOUR_DBSECLAB-VM_PUBLIC-IP>:8080/hr_dev_pdb2` (bg: rojo y menú: rojo)

## Reconocimientos

*   **Autor**: Rene Fontcha, responsable de plataforma de LiveLabs, NA Technology
*   **Contribuyentes**: Hakim Loumi
*   **Última actualización por/fecha**: Marion Smith, mánager de programas técnicos, abril de 2022