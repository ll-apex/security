# Auditoría Unificada de Oracle

## Introducción

En este taller se presenta la funcionalidad de Oracle Unified Auditing. Ofrece al usuario la oportunidad de aprender a configurar esta función para auditar la actividad de la base de datos.

_Tiempo de laboratorio estimado:_ 35 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Unified Auditing (mayo de 2022)_"[](youtube:bK26Y0TZANY)

### Objetivos

*   Activar/desactivar la auditoría unificada en la base de datos
    
*   Ver diferentes casos de uso de auditoría
    
    **Nota**:
    
    *   La auditoría en modo mixto es la auditoría por defecto en una base de datos recién instalada. La auditoría en modo mixto activa tanto las funciones tradicionales (es decir, la utilidad de auditoría de versiones anteriores a la versión 12c) como las nuevas funciones de auditoría (auditoría unificada).
    *   El modo mixto tiene como objetivo introducir una auditoría unificada, para que pueda tener una idea de cómo funciona y cuáles son sus matices y beneficios. El modo mixto permite migrar las aplicaciones y los scripts existentes para utilizar la auditoría unificada. Una vez que haya decidido utilizar la auditoría unificada pura, puede volver a enlazar el binario oracle con la opción de auditoría unificada activada y, por lo tanto, activarla como única utilidad de auditoría que ejecuta la base de datos Oracle. Si decide volver al modo mixto, puede hacerlo.
    *   En este entorno, ya hemos migrado esta instancia de Oracle Database al modo de auditoría unificada pura.

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
| 1 | Mostrar la configuración de auditoría actual | 5 minutos |
| 2 | Auditar uso no de aplicación | 10 minutos |
| 3 | Auditar uso de rol de base de datos | <10 minutos |
| 4 | Uso de Pump de Datos de Auditoría | <10 minutos |

## Tarea 1: Mostrar la configuración de auditoría actual

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/unified-auditing</copy>
        
3.  Mostrar la configuración de auditoría
    
        <copy>./ua_current_audit_settings.sh</copy>
        
    *   Ya hemos definido el entorno en modo de auditoría unificada puro, por lo que debe ver que la auditoría unificada está definida en **TRUE**.
        
        ![Auditoría Unificada](./images/ua-001.png "Mostrar la configuración de auditoría")
        
        **Nota**: Esto significa que nuestra base de datos está en modo de auditoría unificada "pura" y que ya no utiliza las capacidades de auditoría tradicionales
        
    *   La 2a consulta muestra cuántas políticas de auditoría unificadas existen y cuántos atributos relacionados con la auditoría están asociados a cada política
        
        ![Auditoría Unificada](./images/ua-002.png "Mostrar la configuración de auditoría")
        
    *   La 3a consulta de esta secuencia de comandos muestra qué políticas de auditoría unificada están **activadas**.
        
        ![Auditoría Unificada](./images/ua-003.png "Mostrar la configuración de auditoría")
        
        **Nota**:
        
        *   El hecho de que la política exista en la consulta anterior no significa que esté activada
        *   El uso de una política de auditoría unificada consta de dos pasos:
            *   crear la política: crear la política de auditoría <policy\_name> ...
            *   active la política: política de auditoría <policy\_name>
    *   La 4a consulta muestra la auditoría basada en el contexto
        
        ![Auditoría Unificada](./images/ua-004.png "Mostrar la configuración de auditoría")
        
        **Nota**:
        
        *   Tenemos una política denominada `TICKETINFO` que captura un atributo denominado `TICKET_ID`
        *   Esta información se podrá ver en la columna `APPLICATION_CONTEXTS` de la vista `UNIFIED_AUDIT_TRAIL`
4.  Mostrar quién tiene los roles `AUDIT_ADMIN` y `AUDIT_VIEWER`
    
        <copy>./ua_who_audit_roles.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-005.png "Mostrar quién tiene los roles AUDIT_ADMIN y AUDIT_VIEWER")
    
5.  Mostrar los registros de auditoría existentes para la base de datos a la que se conecta (por defecto, la secuencia de comandos seleccionará **pdb1** como la base de datos que desea consultar)
    
        <copy>./ua_query_existing_audit_records.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-006.png "Mostrar los registros de auditoría existentes")
    
6.  Por último, muestra algunos de los detalles del paquete `DBMS_AUDIT_MGMT`
    
        <copy>./ua_dbms_audit_mgmt_settings.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-026.png "Muestra algunos de los detalles del paquete DBMS_AUDIT_MGMT")
    
    **Nota**:
    
    *   La función, `DBMS_AUDIT_MGMT.GET_AUDIT_COMMIT_DELAY`, devuelve el tiempo de retraso de confirmación de auditoría como el número de segundos
    *   El tiempo de retraso de confirmación de auditoría es el tiempo máximo que se tarda en confirmar un registro de auditoría en la pista de auditoría de la base de datos
    *   Si se tarda más tiempo en confirmar un registro de auditoría que el definido por el tiempo de retraso de confirmación de auditoría, se escribe una copia del registro de auditoría en la pista de auditoría del sistema operativo.

## Tarea 2: Auditoría del uso no de aplicación

En este laboratorio, auditará quién está utilizando los objetos `EMPLOYEESEARCH_PROD` fuera de la aplicación

1.  Identifique las conexiones en las que confiamos. Generaremos alguna actividad desde la aplicación Glassfish y veremos la información relacionada con la sesión
    
        <copy>./ua_query_employeesearch_usage.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-007.png "Identificar las conexiones en las que confiamos")
    
    **Nota**: Cuando se le solicite, **NO pulse \[return\]** antes de realizar una investigación en Glassfish, como se indica a continuación.
    
2.  En paralelo, utilice la aplicación Glassfish para generar actividad en la base de datos:
    
    *   Abra una ventana del explorador web en _`http://dbsec-lab:8080/hr_prod_pdb1`_
        
        **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
        
    *   Conéctese a la aplicación HR como _`hradmin`_ con la contraseña "_`Oracle123`_"
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![Auditoría Unificada](./images/ua-008.png "Conexión a la Aplicación HR")
        
        ![Auditoría Unificada](./images/ua-009.png "Conexión a la Aplicación HR")
        
    *   Haga clic en **Buscar empleados**.
        
        ![Auditoría Unificada](./images/ua-010.png "Buscar empleados")
        
    *   Haga clic en \[**Buscar**\]
        
        ![Auditoría Unificada](./images/ua-011.png "Buscar empleados")
        
    *   Cambie algunos de los criterios y vuelva a buscar
        
    *   **Repita 2-3 veces** para asegurarse de que tiene suficiente tráfico
        
3.  Vuelva a la sesión de terminal y **pulse \[return\]** cuando esté listo para ver los resultados
    
    ![Auditoría Unificada](./images/ua-012.png "Pulse [return] cuando esté listo para ver los resultados")
    
4.  A continuación, ejecute una consulta para generar tráfico desde **SQL\*Plus** en el sistema operativo host
    
        <copy>./ua_query_employeesearch.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-013.png "Generar tráfico")
    
5.  Ahora, cree la **política de auditoría unificada**
    
        <copy>./ua_create_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-014.png "Creación de la Política de Auditoría Unificada")
    
    **Nota**:
    
    *   La política de auditoría unificada capturará los detalles relacionados con la máquina para crear la cláusula **WHEN**
    *   Aquí hemos creado la política de auditoría `AUDIT_EMPLOYEESEARCH_USAGE` basada en las variables `SYS_CONTEXT` como criterios:
        *   `SESSION_USER = "EMPLOYEESEARCH_PROD"`
        *   **Y** `OS_USER != "oracle"`
        *   **O bien** `MODULE != "JDBC Thin Client"`
        *   **O bien** `HOST != "dbsec-lab.dbsecvcn.oraclevcn.com"`
    *   Esta política de auditoría auditará todas las sesiones que intenten acceder a las tablas `EMPLOYEESEARCH_PROD.DEMO_HR_USERS` y `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` desde una ruta de acceso no segura (es decir, una ruta de acceso distinta de la aplicación web oficial, por ejemplo).
6.  Después de crear la política de auditoría unificada, debe activarla.
    
        <copy>./ua_enable_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-015.png "Activar política de auditoría unificada")
    
7.  Ejecute consultas adicionales para generar tráfico y ver si se generan registros de auditoría
    
    *   Ejecutar
        
            <copy>./ua_query_employeesearch_usage.sh</copy>
            
        
        ![Auditoría Unificada](./images/ua-007.png "Generar tráfico")
        
        **Nota**: Cuando se le solicite, **NO pulse \[return\]** antes de realizar una investigación en Glassfish, como se indica a continuación.
        
    *   Vuelva a la aplicación Glassfish para generar nueva actividad haciendo clic en **Buscar empleados**
        
        ![Auditoría Unificada](./images/ua-010.png "Generar nueva actividad en la aplicación Glassfish")
        
    *   Haga clic en \[**Buscar**\]
        
        ![Auditoría Unificada](./images/ua-011.png "Buscar empleados")
        
    *   Cambie algunos de los criterios y vuelva a buscar
        
    *   **Repita 2-3 veces** para asegurarse de que tiene suficiente tráfico
        
    *   A continuación, vuelva a la sesión de terminal y pulse **\[return\]**.
        
        ![Auditoría Unificada](./images/ua-016.png "Detener la categoría")
        
    *   Visualización de los resultados de la salida de auditoría de la política de auditoría `AUDIT_EMPLOYEESEARCH_USAGE`
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![Auditoría Unificada](./images/ua-016b.png "Ver los resultados de la salida de auditoría")
        
        **Nota**: No se deben generar en función de esta política de auditoría unificada porque **excluimos** los datos de auditoría de la aplicación
        
8.  Ahora, vuelva a consultar la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` mediante una ruta de acceso "no segura" para generar datos de auditoría
    
    *   Ejecutar esta consulta de SQL\*Plus
        
            <copy>./ua_query_employeesearch.sh</copy>
            
        
        ![Auditoría Unificada](./images/ua-013.png "Buscar empleados")
        
    *   Ahora, vea los resultados de la salida de auditoría para la política de auditoría `AUDIT_EMPLOYEESEARCH_USAGE`
        
            <copy>./ua_query_audit_records.sh</copy>
            
        
        ![Auditoría Unificada](./images/ua-017.png "Ver los resultados de la salida de auditoría")
        
        **Nota**:
        
        *   Puede ver que tenemos una entrada que se corresponde con nuestro uso de SQL\*Plus sin capturar consultas de la aplicación Glassfish.
        *   Confiamos en que la aplicación ejecute consultas como `EMPLOYEESEARCH_PROD`, pero no confiamos en nadie más
        *   Queremos auditar a todos los demás
9.  Cuando haya completado esta práctica de laboratorio, puede eliminar la política Unified Audit
    
        <copy>./ua_delete_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-027.png "Eliminación de la política de auditoría unificada")
    

## Tarea 3: Auditoría del uso de roles de la base de datos

Al auditar un rol, Oracle Database audita todos los privilegios del sistema que se otorgan directamente al rol. Puede auditar cualquier rol, incluidos los roles definidos por el usuario. Si crea una política de auditoría unificada común para roles con la opción de auditoría ROLES, debe especificar solo roles comunes en la lista de roles.

Cuando esta política está activada, Oracle Database audita todos los privilegios del sistema que se otorgan de forma habitual y directa al rol común. Los privilegios del sistema que se otorgan localmente al rol común no se auditarán. Para buscar si un rol se ha otorgado normalmente, consulte la vista del diccionario de datos `DBA_ROLES`. Para buscar si los privilegios otorgados al rol se suelen otorgar, consulte la vista `ROLE_SYS_PRIVS`.

1.  Cree el rol `MGR_ROLE` y otorgue el privilegio del sistema `CREATE TABLESPACE`. A continuación, otorgará el rol al usuario de base de datos `DBA_NICOLE`
    
        <copy>./ua_create_role.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-018.png "Cree el rol MGR_ROLE")
    
2.  Cree la política de auditoría `AUD_ROLE_POL` para auditar el uso del rol `MGR_ROLE`
    
        <copy>./ua_create_role_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-019.png "Creación de la Política de Auditoría AUD_ROLE_POL")
    
3.  A continuación, cree el usuario `DBA_JUNIOR` al que se le otorgará el rol `DBA`
    
        <copy>./ua_create_junior_dba.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-020.png "Crear usuario DBA_JUNIOR")
    
4.  Crear la política asociada a la auditoría del uso del rol `DBA`
    
        <copy>./ua_create_dba_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-021.png "Crear la política asociada")
    
5.  Activar las políticas de auditoría para el uso de roles `MGR_ROLE` y `DBA`
    
        <copy>./ua_enable_audit_policies.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-022.png "Activar las políticas de auditoría")
    
6.  Ver las políticas de auditoría que están activadas
    
        <copy>./ua_view_audit_policies.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-023.png "Comprobar las políticas de auditoría")
    
7.  Ejecutar sentencias SQL que se mostrarán en la pista de auditoría unificada
    
        <copy>./ua_generate_audits.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-024.png "Generar auditorías")
    
8.  Ver la salida de pista de auditoría unificada asociada a las dos políticas de auditoría
    
        <copy>./ua_review_generated_audits.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-025.png "Ver la salida de la pista de auditoría unificada")
    
9.  Cuando haya completado esta práctica de laboratorio, puede eliminar las políticas de Unified Audit de uso de roles
    
        <copy>./ua_delete_role_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-025b.png "Eliminar políticas de auditoría unificada de uso de roles")
    

## Tarea 4: Uso de Pump de Datos de Auditoría

En este laboratorio, configurará la pista de auditoría unificada y revisará una auditoría de la exportación de Oracle Data Pump. Esta es una función de auditoría unificada que no está disponible en la auditoría tradicional.

1.  Crear la política de auditoría unificada "`DP_POL`" para auditar las actividades de pump de datos
    
        <copy>./ua_audit_datapump_export.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-028.png "Creación de la Política de Auditoría Unificada DP_POL")
    
2.  Realice dos exportaciones de pump de datos de la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`...
    
        <copy>./ua_datapump_export_hr_table.sh</copy>
        
    
    *   ...como usuario autorizado (`SYSTEM`): **Correcto**... y se ha creado el archivo de exportación `$DBSEC_LABS/unified-auditing/HR_table.dmp`.
        
        ![Auditoría Unificada](./images/ua-029a.png "Exportaciones de pump de datos")
        
    *   ...y como usuario no autorizado (`DBSAT_ADMIN`): **In Failure!**
        
        ![Auditoría Unificada](./images/ua-029b.png "Exportaciones de pump de datos")
        
    
    **Nota**: Solo está disponible la exportación correcta.
    
3.  Revisar la pista de auditoría unificada para la actividad de pump de datos
    
        <copy>./ua_review_datapump_audit_events.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-030.png "Revisión de la Pista de Auditoría Unificada")
    
4.  Cuando haya terminado el ejercicio práctico, puede eliminar la política de auditoría unificada de pump de datos
    
        <copy>./ua_delete_dp_audit_policy.sh</copy>
        
    
    ![Auditoría Unificada](./images/ua-031.png "Eliminación de la política de auditoría unificada de pump de datos")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

En la auditoría unificada, la pista de auditoría unificada captura información de auditoría de una variedad de orígenes.

La auditoría unificada permite capturar registros de auditoría de los siguientes orígenes:

*   Registros de auditoría (incluidos los registros de auditoría SYS) de las políticas de auditoría unificadas y configuración de AUDIT
*   Registros de auditoría detallados del paquete PL/SQL `DBMS_FGA`
*   Registros de auditoría de Oracle Database Real Application Security
*   Registros de auditoría de Oracle Recovery Manager
*   Registros de auditoría de Oracle Database Vault
*   Registros de auditoría de Oracle Label Security
*   Registros de Oracle Data Mining
*   Oracle Data Pump
*   Carga directa de Oracle SQL\*Loader

La pista de auditoría unificada, que reside en una tabla de solo lectura en el esquema AUDSYS del tablespace SYSAUX, hace que esta información esté disponible en un formato uniforme en la vista del diccionario de datos `UNIFIED_AUDIT_TRAIL` y está disponible en entornos de instancia única y Oracle Database Real Application Clusters. Además del usuario SYS, los usuarios a los que se les han otorgado los roles `AUDIT_ADMIN` y `AUDIT_VIEWER` pueden consultar estas vistas. Si los usuarios solo necesitan consultar las vistas pero no crear políticas de auditoría, otorgue el rol `AUDIT_VIEWER`.

Cuando se puede escribir en la base de datos, los registros de auditoría se escriben en la pista de auditoría unificada. Si no se puede escribir en la base de datos, los registros de auditoría se escriben en nuevos archivos del sistema operativo con formato en el directorio `$ORACLE_BASE/audit/$ORACLE_SID`.

### **Ventajas de la Pista de Auditoría Unificada**

*   Una vez activada la auditoría unificada, no depende de los parámetros de inicialización utilizados en versiones anteriores.
*   Los registros de auditoría, incluidos los registros de la pista de auditoría SYS, de todos los componentes auditados de la instalación de Oracle Database se colocan en una ubicación y en un formato, en lugar de tener que buscar en diferentes lugares para buscar pistas de auditoría en diferentes formatos.
*   La gestión y la seguridad de la pista de auditoría también se mejoran al tenerla en una sola pista de auditoría.
*   El rendimiento general de la auditoría mejora considerablemente. Por defecto, los registros de auditoría se escriben automáticamente en una tabla relacional interna del esquema AUDSYS.
*   Puede crear políticas de auditoría con nombre que le permitan auditar los componentes soportados que se muestran al principio de esta sección, así como los usuarios administrativos SYS. Además, puede crear condiciones y exclusiones en sus políticas.
*   Si utiliza un entorno de Oracle Audit Vault and Database Firewall, la pista de auditoría unificada facilita en gran medida la recopilación de datos de auditoría, ya que todos estos datos procederán de una ubicación.

## Más información

Documentación técnica:

*   [Introducción a la auditoría](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/introduction-to-auditing.html)
*   [Supervisión de la actividad de base de datos con auditoría](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/part_6.html)

Vídeo:

*   _Descripción de la auditoría unificada (febrero de 2019)_[](youtube:8spLhyj3iC0)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Angeline Dhanarani
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023