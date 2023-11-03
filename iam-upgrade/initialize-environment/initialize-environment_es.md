# Inicialización del Entorno

## Introducción

En este laboratorio, revisaremos y configuraremos todos los componentes necesarios para actualizar correctamente IAM 11.1.2.3 a la versión 12.2.1.4.

_Tiempo de laboratorio estimado_: 30 minutos

### Objetivos

*   Inicialice el entorno de taller base de IAM 11.1.2.3.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno

## Tarea 1: Validar que los procesos necesarios estén activos y en ejecución

1.  Ahora, con acceso a la sesión de escritorio remoto, continúe como se indica a continuación para validar el entorno antes de empezar a ejecutar las prácticas posteriores. Los siguientes procesos deben estar activos y en ejecución:
    
    *   listener de la base de datos
        *   LISTENER
    *   Instancia de servidor de base de datos
        *   orcl
    
    ![Página de llegada de escritorio remoto](./images/login.png " ")
    
2.  Ejecute lo siguiente desde la sesión _Terminal_ para validar que los procesos esperados están activos.
    
    *   Servicio de Base de Datos
        
            <copy>
            systemctl status oracle-database.service
            </copy>
            
        
        ![comprobación de estado de servicio de base de datos terminal](./images/db-service-status.png " ")
        
    *   Servicio IAM
        
            <copy>
            systemctl status oracle-iam.service
            </copy>
            
        
        ![comprobación de estado de servicio iam de terminal](./images/iam-service-status.png " ")
        
    
    Si todos los procesos esperados se muestran en la salida como se ha visto anteriormente, el entorno está listo para la siguiente tarea.
    
3.  Pruebe la instalación base accediendo a la _consola de administración de Identity Manager_, que se ha iniciado previamente en la ventana _Chrome_ de la sesión de escritorio remoto, con las credenciales siguientes:
    
        URL: <copy>http://wsidmhost.idm.oracle.com:7778/oim</copy>
        
    
        User: <copy>xelsysadm</copy>
        
    
        Password: <copy>IAMUpgrade12c##</copy>
        
    
    ![Conexión a la consola de administración de Identity Manager](./images/login1.png " ")
    
    ![Página inicial de la consola de administración de Identity Manager](./images/oim-landing.png " ")
    

## Tarea 2: Revisar ReadMe.txt y binarios

1.  Revise los detalles del entorno para obtener más información sobre la configuración. Vaya al explorador de archivos como se muestra a continuación y abra _ReadMe.txt_.
    
    ![revisión readme.txt](./images/review.png " ")
    
2.  Para su comodidad, todos los binarios de software necesarios a lo largo del taller se han almacenado en la instancia. Consulte los detalles a continuación para revisarlos
    
    *   Revisar binarios 12c
    
        <copy>ls -ltrh /home/oracle/Downloads/12cbits </copy>
        
    
    *   Revisar binarios de 11g
    
        <copy>ls -ltrh /home/oracle/Downloads/11gbits </copy>
        
    
    ![estructura de carpeta de binarios](./images/review2.png " ")
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Apéndice 1: Gestión de Servicios de Inicio

1.  Servicio de base de datos (Database and Standard Listener).
    
    *   Iniciar
    
        <copy>
        sudo systemctl start oracle-database
        </copy>
        
    
    *   Parar
    
        <copy>
        sudo systemctl stop oracle-database
        </copy>
        
    
    *   Estado
    
        <copy>
        systemctl status oracle-database
        </copy>
        
    
    *   Reiniciar
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
2.  Servicio IAM
    
    *   Iniciar
    
        <copy>
        sudo systemctl start oracle-iam.service
        </copy>
        
    
    *   Parar
    
        <copy>
        sudo systemctl stop oracle-iam.service
        </copy>
        
    
    *   Estado
    
        <copy>
        systemctl status oracle-iam.service
        </copy>
        
    
    *   Reiniciar
    
        <copy>
        sudo systemctl restart oracle-iam.service
        </copy>
        

## Más información

Utilice estos enlaces para obtener más información sobre Oracle Identity and Access Management:

*   [Oracle Identity Management 12.2.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html).

## Reconocimientos

*   **Autor**: Anbu Anbarasu, director, COE de Cloud Platform
*   **Contribuyentes** - Eric Pollard - Ingeniería de mantenimiento, Ajith Puthan - Soporte de IAM, Rene Fontcha
*   **Última actualización por/Fecha**: Sahaana Manavalan, LiveLabs Desarrollador, NA Technology, mayo de 2022