# Oracle Database Vault (DV)

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Database Vault (DV). Ofrece al usuario la oportunidad de aprender a configurar esas funciones para evitar que usuarios con privilegios no autorizados accedan a datos confidenciales.

Tiempo estimado: 45 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Database Vault (mayo de 2022)_"[](youtube:M5Kn-acUHRQ)

### Objetivos

*   Activar Database Vault en el contenedor y la base de datos de conexión `PDB1`
*   Protección de datos confidenciales mediante un dominio de Database Vault
*   Proteja las cuentas de servicio mediante Trusted Path
*   Probar controles de Database Vault con modo de simulación
*   Proteja las bases de datos conectables de los administradores de contenedores

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Ha finalizado:
    *   Laboratorio: Preparar configuración (solo inquilinos pagados)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Activar Database Vault | 5 minutos |
| 2 | Creación de un Dominio Simple | 10 minutos |
| 3 | Creación de una ruta de confianza/autorización multifactor | 10 minutos |
| 4 | Modo de simulación | 10 minutos |
| 5 | Control de operaciones | 5 minutos |
| 6 | Desactivación de Database Vault | <5 minutos |

## Tarea 1: Activación de Database Vault

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/database-vault</copy>
        
3.  Para empezar, active Database Vault en la base de datos de contenedores **cdb1**
    
        <copy>./dv_enable_on_cdb.sh</copy>
        
    
    **Nota**: Para activar DB Vault, se reiniciará la base de datos.
    
    ![Database Vault](./images/dv-001.png "Activar almacén de base de datos")
    
4.  A continuación, actívela en la base de datos conectable. Por ahora, solo tiene que activarlo en **pdb1**
    
        <copy>./dv_enable_on_pdb.sh pdb1</copy>
        
    
    Verá un estado similar al siguiente:
    
    ![Database Vault](./images/dv-002.png "Activar almacén de base de datos")
    
5.  Ahora, Database Vault está activado en la base de datos de contenedores y en pdb1.
    

## Tarea 2: Creación de un Dominio Simple

1.  Abra una ventana del explorador web en _`http://dbsec-lab:8080/hr_prod_pdb1`_ para acceder a la aplicación Glassfish
    
    **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
    
    ![Database Vault](./images/dv-029.png "Aplicación de RR. HH. - Conexión")
    
2.  Inicie sesión en la aplicación como _`hradmin`_ con la contraseña "_`Oracle123`_"
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![Database Vault](./images/dv-030.png "Aplicación de RR. HH. - Conexión")
    
3.  Haga clic en **Buscar empleado**
    
    ![Database Vault](./images/dv-031.png "Aplicación HR - Buscar empleados")
    
4.  Haga clic en \[**Buscar**\]
    
    ![Database Vault](./images/dv-032.png "Aplicación HR - Buscar empleados")
    
5.  Vuelva a la sesión de Terminal y ejecute el comando para ver los detalles de la sesión Glassfish.
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![Database Vault](./images/dv-003.png "HR App: datos de empleados")
    
6.  Ahora, cree el **dominio** `PROTECT_EMPLOYEESEARCH_PROD` para proteger los objetos del esquema `EMPLOYEESEARCH_PROD` de actividades maliciosas
    
        <copy>./dv_create_realm.sh</copy>
        
    
    ![Database Vault](./images/dv-004.png "Cree el Dominio PROTECT_EMPLOYEESEARCH_PROD")
    
7.  Agregar objetos al dominio para proteger (aquí agregará todos los objetos del esquema)
    
        <copy>./dv_add_obj_to_realm.sh</copy>
        
    
    ![Database Vault](./images/dv-005.png "Agregar objetos al dominio para protegerlos")
    
8.  Asegúrese de tener un usuario autorizado en el dominio. En este paso, agregaremos `EMPLOYEESEARCH_PROD` como propietario autorizado de dominio
    
        <copy>./dv_add_auth_to_realm.sh</copy>
        
    
    ![Database Vault](./images/dv-006.png "Agregar EMPLOYEESEARCH_PROD como propietario autorizado de dominio")
    
9.  Vuelva a ejecutar la consulta SQL para mostrar que `SYS` recibe ahora el mensaje de error **privilegios insuficientes**
    
        <copy>./dv_query_employee_data.sh</copy>
        
    
    ![Database Vault](./images/dv-007a.png "Ahora, el usuario SYS recibe el mensaje de error de privilegios insuficientes")
    
10.  Cuando haya terminado esta práctica de laboratorio, puede borrar el dominio
    
        <copy>./dv_drop_realm.sh</copy>
        
    
    ![Database Vault](./images/dv-007b.png "Borrar el dominio")
    

## Tarea 3: Creación de una ruta de confianza/autorización multifactor

1.  Volver a la aplicación Glassfish y volver a hacer clic en \[**Buscar empleado**\]
    
    ![Database Vault](./images/dv-031.png "Aplicación HR - Buscar empleados")
    
2.  Y haga clic en \[**Buscar**\]
    
    ![Database Vault](./images/dv-032.png "Aplicación de RR. HH.: Buscar")
    
3.  Vuelva a la sesión de Terminal y ejecute esta consulta para ver la información de sesión asociada a la aplicación Glassfish
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![Database Vault](./images/dv-019.png "Ver la información de sesión asociada con la aplicación HR")
    
4.  Ahora, consulte la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` con el propietario `EMPLOYEESEARCH_PROD` para demostrar que se puede acceder a ella
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![Database Vault](./images/dv-020.png "Tablas de consulta")
    
5.  Empiece a proteger las credenciales de la aplicación creando una regla de Database Vault
    
        <copy>./dv_create_rule.sh</copy>
        
    
    ![Database Vault](./images/dv-021.png "Creación de una regla de Database Vault")
    
    **Nota:** Autorizamos como aplicación de ruta de confianza solo el acceso desde la aplicación web Glassfish (cliente ligero JDBC) a través del propietario del esquema `EMPLOYEESEARCH_PROD`.
    
6.  Utilizamos la regla de Database Vault agregándola a un **juego de reglas de DVD**
    
    *   Puede tener una o varias reglas en el juego de reglas
    *   Si tiene más de una, puede elegir entre el juego de reglas que evalúa todas las reglas debe ser true o la regla `ANY` debe ser true
    *   Piense en ello como la diferencia entre `IN` y `EXISTS`: `IN` lo incluye todo mientras que `EXISTS` se detiene una vez que identifica una coincidencia de resultados
    
        <copy>./dv_create_rule_set.sh</copy>
        
    
    ![Database Vault](./images/dv-022.png "Creación de un juego de reglas de Database Vault")
    
7.  Crear una regla de comando en "**CONNECT**" para proteger el usuario `EMPLOYEESEARCH_PROD`
    
        <copy>./dv_create_command_rule.sh</copy>
        
    
    ![Database Vault](./images/dv-023.png "Crear una regla de comando en CONNECT")
    
    **Nota**: Solo puede "`CONNECT`" como `EMPLOYEESEARCH_PROD` si coincide con el juego de reglas que hemos creado.
    
8.  Vuelva a la aplicación Glassfish, refresque varias veces y ejecute algunas consultas haciendo clic en \[**Buscar**\] y explore los datos de los empleados.
    
    **Nota**: Como está utilizando la aplicación Glassfish como aplicación Trusted Path, puede acceder a los datos.
    
9.  Vuelva a la sesión de terminal y vuelva a ejecutar nuestra consulta del uso de la aplicación para verificar que sigue funcionando
    
        <copy>./dv_query_employeesearch_usage.sh</copy>
        
    
    ![Database Vault](./images/dv-024.png "Buscar empleados")
    
10.  Ahora, intente consultar la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` con el propietario `EMPLOYEESEARCH_PROD`... **Deberías estar bloqueado**.
    
        <copy>./dv_query_employee_search.sh</copy>
        
    
    ![Database Vault](./images/dv-025.png "Tablas de consulta")
    
    **Nota**: Debido a que está realizando una consulta a través de una aplicación que no sea "Trusted Path", no puede acceder a los datos.
    
11.  Una vez que haya completado correctamente la práctica de laboratorio, puede suprimir la **regla de comando**, el **juego de reglas** y la **regla** de Database Vault
    
        <copy>./dv_del_trusted_path.sh</copy>
        
    
    ![Database Vault](./images/dv-026.png "Supresión de la ruta de confianza")
    

## Tarea 4: Modo de simulación

1.  En primer lugar, consulte el log de simulación para mostrar que no tiene valores actuales
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![Database Vault](./images/dv-008.png "Consultar el log de simulación")
    
2.  A continuación, cree una regla de comando que simule el bloqueo de todas las conexiones a la base de datos. Esta es una manera fácil para nosotros de identificar quién se está conectando y desde dónde se están conectando.
    
        <copy>./dv_command_rule_sim_mode.sh</copy>
        
    
    ![Database Vault](./images/dv-009.png "Crear una regla de comando")
    
3.  Ejecutar un script para crear algunas conexiones de base de datos y generar algunas entradas de log
    
        <copy>./dv_run_queries.sh</copy>
        
    
    ![Database Vault](./images/dv-010.png "Generar tráfico")
    
4.  Ahora, volvemos a consultar el registro de simulación para ver qué nuevas entradas tenemos. Recuerde que hemos creado una regla de comando para simular el bloqueo de conexiones de usuario.
    
        <copy>./dv_query_simulation_logs.sh</copy>
        
    
    ![Database Vault](./images/dv-011.png "Consultar el log de simulación")
    
    El log muestra todos los usuarios que se han conectado y que la regla habría bloqueado. También muestra desde dónde se conectaron y qué cliente utilizaron para conectarse
    
5.  Ejecute este script para obtener una lista de los distintos nombres de usuario presentes en los logs de simulación
    
        <copy>./dv_distinct_users_sim_logs.sh</copy>
        
    
    ![Database Vault](./images/dv-012a.png "Lista de los distintos nombres de usuario presentes en los logs de simulación")
    
6.  Aunque solo utilizamos el modo de simulación en una regla **CONNECT**, podríamos haberlo utilizado en un dominio para mostrar qué violaciones tendríamos
    
7.  Antes de pasar a la siguiente práctica de laboratorio, limpiaremos los logs de simulación y eliminaremos la regla de comando
    
        <copy>./dv_purge_sim_logs.sh</copy>
        
    
    ![Database Vault](./images/dv-012b.png "Depurar los logs de simulación")
    
        <copy>./dv_drop_command_rule.sh</copy>
        
    
    ![Database Vault](./images/dv-012c.png "Borrado de la Regla de Comando")
    

## Tarea 5: Control de operaciones

1.  Comprobar el estado de Database Vault y Operations Control
    
        <copy>./dv_status.sh</copy>
        
    
    ![Database Vault](./images/dv-013.png "Comprobar el estado de Database Vault")
    
    **Nota**: Aún no está configurado.
    
2.  A continuación, ejecutaremos las mismas consultas que la base de datos conectable **pdb1** y **pdb2**...
    
    *   ... como `DBA_DEBRA`
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![Database Vault](./images/dv-014.png "Consultar como DBA DEBRA")
    
    *   ... como `C##SEC_DBA_SAL`
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![Database Vault](./images/dv-015.png "Consultar como SAL de DBA")
    
    **Nota**:
    
    *   Los resultados de la consulta son iguales
    *   El usuario común `C##SEC_DBA_SAL` tiene acceso a los datos de las bases de datos conectables, al igual que el administrador de pdb.
3.  Activar **Operations Control** de Database Vault 19c y volver a ejecutar las consultas
    
    **Nota**: Observe quién puede y quién no puede consultar los datos del esquema `EMPLOYEESEARCH_PROD` ahora... `SAL` ya no debería poder acceder a los datos.
    
        <copy>./dv_enable_ops_control.sh</copy>
        
    
    ![Database Vault](./images/dv-016a.png "Activar control de OPS")
    
        <copy>./dv_status.sh</copy>
        
    
    ![Database Vault](./images/dv-016b.png "Comprobar el estado de Database Vault")
    
        <copy>./dv_query_with_debra.sh</copy>
        
    
    ![Database Vault](./images/dv-017.png "Consultar como DBA DEBRA")
    
        <copy>./dv_query_with_sal.sh</copy>
        
    
    ![Database Vault](./images/dv-018a.png "Consultar como SAL de DBA")
    
4.  Cuando haya completado esta práctica de laboratorio, desactive el control de operaciones
    
        <copy>./dv_disable_ops_control.sh</copy>
        
    
    ![Database Vault](./images/dv-018b.png "Desactivar control de OPS")
    

## Tarea 6: Desactivación de Database Vault

1.  Desactivación de la base de datos conectable **pdb1**
    
        <copy>./dv_disable_on_pdb.sh pdb1</copy>
        
    
    **Nota**: `DV_ENABLE_STATUS` para pdb1 debe ser **FALSO**
    
    ![Database Vault](./images/dv-027.png "Deshabilitar Database Vault")
    
2.  Ahora, desactive Database Vault en la base de datos de contenedores **cdb1**
    
        <copy>./dv_disable_on_cdb.sh</copy>
        
    
    ![Database Vault](./images/dv-028.png "Deshabilitar Database Vault")
    
    **Nota**:
    
    *   Para desactivar DB Vault, se reiniciará la base de datos.
    *   `DV_ENABLE_STATUS` para cdb debe ser **FALSE** (FALSO)
3.  Ahora, Database Vault está desactivado en la base de datos de contenedores y en pdb1.
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Oracle Database Vault proporciona controles para evitar que los usuarios con privilegios no autorizados accedan a datos confidenciales y para evitar cambios no autorizados en la base de datos.

Los controles de seguridad de Oracle Database Vault protegen los datos de las aplicaciones frente a accesos no autorizados y cumplen con los requisitos normativos y de privacidad.

![Database Vault](./images/dv-concept.png "Concepto de almacén de base de datos")

Puede desplegar controles para bloquear el acceso de cuentas con privilegios a los datos de aplicación y controlar las operaciones delicadas dentro de la base de datos mediante la autorización de rutas de confianza.

Mediante el análisis de privilegios y roles, puede aumentar la seguridad de las aplicaciones existentes mediante las mejores prácticas de privilegio mínimo.

Oracle Database Vault protege los entornos de base de datos existentes de forma transparente, eliminando cambios en la aplicación costosos que consumen mucho tiempo.

Oracle Database Vault permite crear un juego de componentes para gestionar la seguridad de la instancia de base de datos.

Estos componentes son:

*   **Dominios**

Un dominio es una zona de protección dentro de la base de datos donde se pueden proteger los esquemas, los objetos y los roles de la base de datos. Por ejemplo, puede proteger un juego de esquemas, objetos y roles relacionados con la contabilidad, las ventas o los recursos humanos. Después de protegerlos en un dominio, puede utilizar el dominio para controlar el uso de los privilegios de sistema y de objeto de cuentas o roles específicos. Esto le permite proporcionar controles de acceso detallados para cualquiera que desee utilizar estos esquemas, objetos y roles.

*   **Reglas de comando**

Una regla de comando es una política de seguridad especial que puede crear para controlar cómo los usuarios pueden ejecutar casi cualquier sentencia SQL, incluidas las sentencias SELECT, ALTER SYSTEM, lenguaje de definición de base de datos (DDL) y lenguaje de manipulación de datos (DML). Las reglas de comando deben funcionar con juegos de reglas para determinar si se permite la sentencia.

*   **Factores**

Un factor es una variable o atributo con nombre, como una ubicación de usuario, una dirección IP de base de datos o un usuario de sesión, que Oracle Database Vault puede reconocer y utilizar como ruta de acceso de confianza. Puede utilizar factores en reglas para controlar actividades como autorizar a las cuentas de base de datos a conectarse a la base de datos o la ejecución de un comando de base de datos específico para restringir la visibilidad y la capacidad de gestión de los datos. Cada factor puede tener una o más identidades. Una identidad es el valor real de un factor. Un factor puede tener varias identidades según el método de recuperación de factores o su lógica de asignación de identidad.

*   **Juegos de Reglas**

Un juego de reglas es una recopilación de una o más reglas que puede asociar a una autorización de dominio, regla de comando, asignación de factor o rol de aplicación segura. El juego de reglas las evalúa como verdaderas o falsas según la evaluación de cada regla que contiene y el tipo de evaluación (Todas Verdaderas o Alguna Verdadera). La regla de un juego de reglas es una expresión PL/SQL que se evalúa como verdadera o falsa. Puede tener la misma regla en varios juegos de reglas.

*   **Roles de aplicación segura**

Un rol de aplicación segura es un rol especial de Oracle Database que se puede activar según la evaluación de un juego de reglas de Oracle Database Vault.

Para aumentar estos componentes, Oracle Database Vault proporciona un juego de interfaces y paquetes PL/SQL. En general, el primer paso es crear un dominio compuesto por los esquemas de base de datos u objetos de base de datos que desea proteger. Puede proteger aún más el dominio mediante la creación de reglas, reglas de comandos, factores, identidades, juegos de reglas y roles de aplicación seguros. Además, puede ejecutar informes sobre las actividades que estos componentes supervisan y protegen.

### **Ventajas del uso de Database Vault**

*   Aborda las regulaciones de cumplimiento para el conocimiento de la seguridad
*   Protege las cuentas de usuarios con privilegios de muchas infracciones de seguridad y robos de datos, tanto externos como internos
*   Le ayuda a diseñar políticas de seguridad flexibles para su base de datos
*   Aborda las preocupaciones de consolidación de bases de datos y entornos en la nube para reducir costos y reducir la exposición de datos de aplicaciones sensibles a aquellos sin una verdadera necesidad de conocimiento
*   Funciona en un entorno multiinquilino, lo que aumenta la seguridad para la consolidación

## Más información

Documentación técnica:

*   [Oracle Database Vault 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dvadm/introduction-to-oracle-database-vault.html#GUID-0C8AF1B2-6CE9-4408-BFB3-7B2C7F9E7284)

Vídeo:

*   _Oracle Database Vault: casos de uso (Part1) (octubre de 2019)_[](youtube:aW9YQT5IRmA)
*   _Oracle Database Vault: casos de uso (Part2) (noviembre de 2019)_[](youtube:hh-cX-ubCkY)
*   _Descripción de Oracle Database Vault (marzo de 2019)_[](youtube:oVidZw7yWIQ)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Richard Evans
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023