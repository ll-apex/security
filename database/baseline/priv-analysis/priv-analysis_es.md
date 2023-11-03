# Análisis de Privilegios de Oracle

## Introducción

En este taller se presenta la funcionalidad de Oracle Privilege Analysis. Ofrece al usuario la oportunidad de aprender a utilizar esta función para conocer siempre el uso de privilegios al que acceden todos los usuarios durante toda la vida útil de la base de datos.

_Tiempo de laboratorio estimado:_ 15 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Privilege Analysis (mayo de 2022)_"[](youtube:OsvxpBIKoOQ)

### Objetivos

*   Capturar la carga de trabajo de una base de datos
*   Generar un informe de análisis de privilegios para conocer todos los privilegios de usuario/sistema utilizados durante esta captura

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Capturar el área de trabajo para analizar | 5 minutos |
| 2 | Analizar la carga de trabajo limitada | 5 minutos |
| 3 | Borrar la captura | <5 minutos |

## Tarea 1: Captura de la carga de trabajo que se va a analizar

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/priv-analysis</copy>
        
3.  Para empezar, asegúrese de que el usuario tiene el rol "`CAPTURE_ADMIN`" y cree la captura Privilege Analysis.
    
        <copy>./pa_create_capture.sh</copy>
        
    
    ![y privilegios](./images/pa-001.png "Crear la captura de Privilege Analysis")
    
4.  A continuación, inicie la captura
    
        <copy>./pa_enable_capture.sh</copy>
        
    
    ![y privilegios](./images/pa-002.png "Iniciar la captura")
    
    **Nota**: Esta acción empezará a recopilar todos los privilegios y/o roles que se están utilizando.
    
5.  Generar alguna carga de trabajo para que tengamos roles y privilegios utilizados y no utilizados
    
        <copy>./pa_generate_workload.sh</copy>
        
    
    ![y privilegios](./images/pa-003.png "Generar alguna carga de trabajo")
    
6.  Podemos desactivar la captura cuando sentimos que tenemos suficientes datos
    
        <copy>./pa_disable_capture.sh</copy>
        
    
    ![y privilegios](./images/pa-004.png "Desactivar la captura")
    

## Tarea 2: Análisis de la carga de trabajo capturada

1.  Generación del Informe
    
        <copy>./pa_generate_report.sh</copy>
        
    
    ![y privilegios](./images/pa-005.png "Generar el informe")
    
    **Nota**:
    
    *   Toma todos los privilegios y roles identificados como utilizados durante la captura y los compara con los roles y privilegios otorgados a cada usuario.
    *   La generación puede tardar unos minutos en depender del volumen que se va a procesar
2.  A continuación, consulte los resultados del informe consultando las vistas asociadas a la salida de captura
    
        <copy>./pa_review_report.sh</copy>
        
    
    ![y privilegios](./images/pa-006.png "Resultados de informes")
    
    **Nota**:
    
    *   Puede ver todos los privilegios (sistema y objetos) utilizados y no utilizados por todos los usuarios activos durante la captura
    *   Este paso es esencial para comprender mejor lo que sucedió en la base de datos durante este período a fin de determinar si los usuarios están utilizando sus propios privilegios correctamente o si necesita revocar algunos no esenciales para evitar cualquier riesgo de abuso, especialmente durante un robo de identidad.
    *   Tenga en cuenta que puede ejecutar esta tarea de análisis de privilegios tantas veces como sea necesario... de hecho, **se recomienda encarecidamente hacerlo con la mayor frecuencia posible** para mantener siempre el control de los derechos de actividad de los usuarios y evitar cualquier intento de elevación de privilegios por parte de posibles atacantes
3.  Ahora, abra la consola de administración de base de datos (OEM Cloud Control) para ver el mismo informe, pero de una mejor forma
    
    *   Abra un explorador web en la URL _`https://dbsec-lab:7803/em`_
        
        **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`https://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:7803/em`_
        
    *   Inicie sesión en la consola de Oracle Enterprise Manager 13c como _`SYSMAN`_ con la contraseña "_`Oracle123`_"
        
            <copy>SYSMAN</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![y privilegios](./images/pa-007.png "OEM - Pantalla de Conexión")
        
    *   Amplíe todas las bases de datos y haga clic en **cdb\_PDB1**
        
        ![y privilegios](./images/pa-008.png "OEM - Visión general de objetivos")
        
    *   En el menú, seleccione **Security** (Seguridad) y **Privileges Analysis** (Análisis de privilegios)
        
        ![y privilegios](./images/pa-009.png "OEM: menú Privilegios Analysis (Análisis de privilegios)")
        
    *   Marque "**Named**" y seleccione "**PA\_ADMIN**" como nombre de credencial
        
        ![y privilegios](./images/pa-010.png "OEM: inicio de sesión de análisis de privilegios")
        
    *   Haga clic en \[**Conectar**\]
        
    *   Seleccione el informe generado durante la captura (aquí "**Toda Captura de Base de Datos**") y haga clic en \[**Ver Informe**\].
        
        ![y privilegios](./images/pa-011.png "OEM - Capturas de análisis de privilegios")
        
    *   Puede ver todos los usuarios que se han conectado a la base de datos durante el período de captura, junto con los privilegios utilizados
        
        ![y privilegios](./images/pa-012.png "OEM - Informe global de análisis de privilegios")
        
    *   Haga clic en el separador "**Utilizado**" para ver todos los privilegios utilizados durante la captura y por quién
        
        ![y privilegios](./images/pa-013.png "OEM - Informe de análisis de privilegios utilizados")
        
        **Nota**: El botón Exportar a hoja de cálculo permite proporcionar esta información a las personas que pueden tomar decisiones sobre la concesión o revocación de derechos.
        
    *   También puede ver los privilegios no utilizados, que pueden ser privilegios que las cuentas no necesitan. Si es así, eliminar estos privilegios innecesarios es una excelente manera de reducir la cantidad de daño que estas cuentas podrían hacer si se vieran comprometidas. Haga clic en el separador "**Sin Utilizar**" para ver todos los privilegios que no se han utilizado durante la captura y por quién
        
        ![y privilegios](./images/pa-014.png "OEM - Informe de análisis de privilegios no utilizado")
        
        **Nota:** Podemos ver claramente si un usuario tiene demasiados derechos o derechos que no necesitan para realizar sus tareas.
        

## Tarea 3: Borrar la captura

1.  Una vez que hemos revisado nuestro informe y nos sentimos cómodos con el análisis de privilegios, podemos borrar la captura que hemos creado
    
        <copy>./pa_drop_capture.sh</copy>
        
    
    ![y privilegios](./images/pa-015.png "Borrar la captura")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

El análisis de privilegios aumenta la seguridad de las aplicaciones y las operaciones de base de datos al ayudarle a implantar las mejores prácticas de privilegios mínimos para los roles y privilegios de base de datos.

Al ejecutarse dentro del núcleo de Oracle Database, el análisis de privilegios ayuda a reducir la superficie de ataque de las cuentas de usuario, herramientas y aplicaciones mediante la identificación de privilegios utilizados y no utilizados para implantar el modelo de privilegios mínimos.

![y privilegios](./images/pa-concept.png "Concepto de análisis de privilegios")

El análisis de privilegios captura dinámicamente los privilegios utilizados por los usuarios y las aplicaciones de la base de datos. El uso del análisis de privilegios puede ayudar a aplicar de forma rápida y eficaz las directrices de privilegios mínimos. En el modelo de privilegios mínimos, los usuarios solo reciben los privilegios y el acceso que necesitan para realizar sus trabajos. Con frecuencia, aunque los usuarios realizan tareas diferentes, a todos se les otorga el mismo juego de privilegios potentes. Sin el análisis de privilegios, averiguar los privilegios que debe tener cada usuario puede ser un trabajo duro y, en muchos casos, los usuarios pueden terminar con algún juego común de privilegios aunque tengan diferentes tareas. Incluso en las organizaciones que gestionan privilegios, los usuarios tienden a acumular privilegios con el tiempo y rara vez pierden privilegios. La separación de tareas divide un solo proceso en tareas separadas para diferentes usuarios. Los privilegios mínimos aplican la separación para que los usuarios solo puedan realizar las tareas necesarias. La aplicación de la separación de funciones es beneficiosa para el control interno, pero también reduce el riesgo de los usuarios maliciosos que roban credenciales privilegiadas.

El análisis de privilegios captura los privilegios que utilizan los usuarios y las aplicaciones de la base de datos en tiempo de ejecución y escribe sus conclusiones en las vistas del diccionario de datos que puede consultar. Si las aplicaciones incluyen derechos del responsable de la definición y procedimientos de derechos del invocador, el análisis de privilegios captura los privilegios necesarios para compilar un procedimiento y ejecutarlo, incluso si el procedimiento se ha compilado antes de crear y activar la captura de privilegios.

Puede crear diferentes tipos de políticas de análisis de privilegios para lograr objetivos específicos:

*   **Captura de uso de privilegios basada en roles**
    
        You must provide a list of roles. If the roles in the list are enabled in the database session, then the used privileges for the session will be captured. You can capture privilege use for the following types of roles: Oracle default roles, user-created roles, Code Based Access Control (CBAC) roles, and secure application roles.
        
*   **Captura de uso de privilegios basada en contexto**
    
        You must specify a Boolean expression only with the `SYS_CONTEXT` function. The used privileges will be captured if the condition evaluates to `TRUE`. This method can be used to capture privileges and roles used by a database user by specifying the user in `SYS_CONTEXT`.
        
*   **Captura de uso de privilegios basados en contexto y rol**
    
        You must provide both a list of roles that are enabled and a `SYS_CONTEXT` Boolean expression for the condition. When any of these roles is enabled in a session and the given context condition is satisfied, then privilege analysis starts capturing the privilege use.
        
*   **Captura de privilegios de toda la base de datos**
    
        If you do not specify any type in your privilege analysis policy, then the used privileges in the database will be captured, except those for the user `SYS`. (This is also referred to as unconditional analysis, because it is turned on without any conditions.)
        

### **Ventajas del Uso del Análisis de Privilegios**

*   Búsqueda de privilegios otorgados innecesariamente
*   Implantación de las mejores prácticas de privilegio mínimo: los privilegios de la cuenta que accede a una base de datos se deben limitar a los privilegios estrictamente necesarios para la aplicación o el usuario
*   Desarrollo de aplicaciones seguras: durante la fase de desarrollo de aplicaciones, algunos administradores pueden otorgar muchos privilegios y roles del sistema potentes a los desarrolladores de aplicaciones
*   Puede crear y utilizar políticas de análisis de privilegios en un entorno multiinquilino
*   Se puede utilizar para capturar los privilegios que se han ejercido en objetos de base de datos precompilados (paquetes, procedimientos, funciones, vistas, disparadores y clases y datos Java PL/SQL)

## Más información

Documentación técnica:

*   [Análisis de privilegios de Oracle 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/performing-privilege-analysis-find-privilege-use.html#GUID-44CB644B-7B59-4B3B-B375-9F9B96F60186)

Vídeo:

*   _Descripción del análisis de privilegios (enero de 2019)_[](youtube:3oRODVtWwbg)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Richard Evans
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023