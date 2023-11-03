# Oracle SQL Firewall

## Introducción

En este taller se presenta la funcionalidad de Oracle SQL Firewall. Ofrece al usuario la oportunidad de aprender a configurar esas funciones para protegerse contra los riesgos que apuntan a fallos/vulnerabilidades de seguridad en aplicaciones web basadas en datos, incluidos los ataques a bases de datos de inyección SQL.

_Tiempo de laboratorio estimado:_ 30 minutos

_Versión probada en este laboratorio:_ Oracle DB 23.2

### Vista previa de vídeo

Vea una vista previa de "_Using SQL Firewall with Oracle Database 23c Free (junio de 2023)_"[](youtube:7yJv92FvLp4)

### Objetivos

*   Crear una política de firewall SQL para proteger los datos confidenciales
*   Detecte una amenaza interna de acceso a credenciales robadas
*   Mitigue el riesgo de ataques de inyección SQL

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
| 1 | Activar SQL Firewall para proteger la aplicación Glassfish HR | 10 minutos |
| 2 | Detecte una amenaza interna de acceso robado a credenciales con SQL Firewall | 10 minutos |
| 3 | Aplique patrones de acceso y SQL permitidos con SQL Firewall, mitigando el riesgo de ataques de inyección SQL | 10 minutos |
| 4 | Restablecimiento del entorno de prácticas de SQL Firewall | <5 minutos |

## Tarea 1: Activar SQL Firewall para proteger la aplicación Glassfish HR

En este laboratorio, aprenderá cómo el administrador entrena el sistema para conocer las sentencias SQL autorizadas y las rutas de conexión de confianza de la aplicación HR. La política de firewall SQL se genera con listas de permitidos que representan conexiones y sentencias SQL autorizadas y se despliega en el destino.

## Tarea 1a: Configurar entorno de firewall SQL

En este caso, modificaremos la conexión Glassfish por defecto para que tenga como destino Oracle Database 23c, de modo que podamos supervisar y bloquear los comandos SQL.

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/sqlfw</copy>
        
3.  Migre la cadena de conexión de la aplicación Glassfish para dirigirse a la base de datos 23c
    
        <copy>./sqlfw_glassfish_start_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-001.png "Defina HR App con DB23c")
    
    **Nota**: Aquí, conectamos Glassfish a la base de datos **`FREEPDB1`** (DB 23c) en la máquina virtual **`db23c`**
    
4.  A continuación, verifique las funciones de la aplicación como se esperaba
    
    *   Abra un explorador web en la URL _`http://dbsec-lab:8080/hr_prod_pdb1`_ para acceder a **su aplicación Glassfish**
        
        **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
        
    *   Inicie sesión en la aplicación como _`hradmin`_ con la contraseña "_`Oracle123`_"
        
            <copy>hradmin</copy>
            
        
            <copy>Oracle123</copy>
            
        
        ![SQLFW](./images/sqlfw-002.png "Aplicación de RR. HH. - Conexión")
        
        ![SQLFW](./images/sqlfw-003.png "Aplicación de RR. HH. - Conexión")
        
    *   En la esquina superior derecha de la aplicación, haga clic en el enlace **Bienvenido administrador de RR. HH.** y se le enviará a una página con datos de sesión
        
        ![SQLFW](./images/sqlfw-004.png "Aplicación de RR. HH. - Configuración")
        
    *   En la pantalla **Detalles de sesión**, verá cómo se conecta la aplicación a la base de datos. Esta información se obtiene del espacio de nombres **userenv** ejecutando la función `SYS_CONTEXT`.
        
        ![SQLFW](./images/sqlfw-005.png "Aplicación HR - Detalles de sesión")
        
    *   Ahora, debe ver **FREEPDB1** como **`DB_NAME`** y **db23c** como **HOST**
        
        ![SQLFW](./images/sqlfw-006.png "HR App: compruebe la base de datos destino")
        
5.  Crear un administrador (**`dba_tom`**) para gestionar SQL Firewall
    
        <copy>./sqlfw_crea_admin-user.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-007.png "Crear el usuario administrador de SQL Firewall")
    
6.  Activar firewall SQL
    
        <copy>./sqlfw_enable.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-008.png "Activar firewall SQL")
    
    **Nota**: debe ver `ENABLED`
    

## Tarea 1b: Activación de SQL Firewall para conocer el tráfico SQL autorizado del usuario de la aplicación HR

1.  Inicie la captura de carga de trabajo SQL del usuario de la aplicación EMPLOYEESEARCH\_PROD
    
        <copy>./sqlfw_capture_start.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-009.png "Iniciar la captura de carga de trabajo SQL del usuario de la aplicación")
    
2.  Ahora, utiliza tu aplicación Glassfish para generar actividad en tu base de datos:
    
    *   Vuelva a la ventana del explorador web a _`http://dbsec-lab:8080/hr_prod_pdb1`_
        
        **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
        
    *   Haga clic en **Buscar empleados**.
        
        ![SQLFW](./images/sqlfw-010.png "Buscar empleados")
        
    *   Haga clic en \[**Buscar**\]
        
        ![SQLFW](./images/sqlfw-011.png "Buscar empleado")
        
    *   Cambie algunos de los criterios y vuelva a buscar
        
    *   **Repita 2-3 veces** para asegurarse de que tiene suficiente tráfico
        
3.  Vuelva a la sesión de terminal para asegurarse de que las sentencias SQL y las conexiones de la carga de trabajo de la aplicación se capturan correctamente
    
        <copy>./sqlfw_capture_check.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-012.png "Compruebe las sesiones y capture logs")
    
    **Nota:** Aquí, comprobamos la sesión y capturamos logs
    
4.  Si está satisfecho, pare la captura de carga de trabajo SQL
    
        <copy>./sqlfw_capture_stop.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-013.png "Parar la captura de carga de trabajo SQL")
    

## Tarea 1c: Generar y activar reglas de lista de permitidos para usuario de aplicación de RR. HH.

1.  Generar la regla de lista de permitidos
    
        <copy>./sqlfw_allow_list_rule_gen.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-014.png "Generar regla de lista de permitidos")
    
    **Nota:** Aquí, tenemos 4 sentencias
    
2.  Comparar esta lista con los eventos capturados
    
        <copy>./sqlfw_capture_count_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-015.png "Contar los eventos capturados")
    
    **Nota:** El recuento coincide con el recuento de los distintos eventos capturados.
    
3.  Ahora, examine las reglas de lista de permitidos de SQL Firewall para conexiones de base de datos de confianza y sentencias SQL
    
        <copy>./sqlfw_allow_list_rule_exam.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-016.png "Examinar las reglas de lista de permitidos de SQL Firewall")
    
    **Nota:** Aquí, solo se permiten conexiones de la aplicación web (`JDBC ThinClient`) iniciadas por el usuario `oracle` en el servidor `10.0.0.150`
    
4.  Configurar las políticas de auditoría para las infracciones de SQL Firewall
    
        <copy>./sqlfw_setup_audit_policies.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-017.png "Configurar las políticas de auditoría para las infracciones de SQL Firewall")
    
5.  Active la regla allow-list para `EMPLOYEESEARCH_PROD` en el **modo de observación**
    
        <copy>./sqlfw_allow_list_rule_enable_monitor.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-018.png "Activar la regla de lista de permitidos en modo de observación")
    
    **Nota:** Aquí observaremos y no bloquearemos las infracciones de SQL Firewall
    

## Tarea 2: Detectar una amenaza interna de acceso robado a credenciales con SQL Firewall

Supongamos que hay un interno malicioso que tenía acceso a la credencial robada del usuario de HR Apps `EMPLOYEESEARCH_PROD` y que para omitir la autorización de la aplicación HR utiliza SQL Developer para obtener acceso a los datos confidenciales de los empleados.

1.  En primer lugar, vamos a validar que la carga de trabajo SQL de la aplicación normal está permitida en la base de datos
    
    *   Utilice su aplicación Glassfish para generar actividad en su base de datos y realizar sus operaciones normales (coincidencia, registro sin violación):
        
        *   Vuelva a la ventana del explorador web a _`http://dbsec-lab:8080/hr_prod_pdb1`_
            
            **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
            
        *   Haga clic en **Buscar empleados**.
            
            ![SQLFW](./images/sqlfw-010.png "Buscar empleados")
            
        *   Haga clic en \[**Buscar**\]
            
            ![SQLFW](./images/sqlfw-011.png "Buscar empleado")
            
        *   Cambie algunos de los criterios (los mismos que antes) y vuelva a buscar
            
        *   **Repita 2-3 veces** para asegurarse de que tiene suficiente tráfico
            
    *   Ahora, vuelva a la sesión de terminal para comprobar los logs de violaciones y los registros de auditoría
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-019.png "Comprobar logs de infracciones y registros de auditoría")
        
        **Nota:** No se ha encontrado ningún registro porque estas consultas ya se muestran como sentencias SQL permitidas en la base de datos
        
2.  Ahora, detectemos una amenaza interna de acceso a credenciales robadas
    
    *   El usuario interno utiliza SQL\*Plus para obtener acceso a los datos confidenciales de los empleados
        
            <copy>./sqlfw_select_sensitive_data.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-020.png "Seleccionar datos confidenciales de empleado")
        
    *   Vuelva a comprobar los logs de violaciones y los registros de auditoría
        
            <copy>./sqlfw_check_events.sh</copy>
            
        
        ![SQLFW](./images/sqlfw-021.png "Comprobar logs de infracciones y registros de auditoría")
        
        **Nota:** Se emite una violación de contexto de firewall SQL porque SQL\*Plus no está en la lista de permitidos del programa del sistema operativo permitido, lo que llama la atención de los administradores de seguridad
        

## Tarea 3: Aplicar patrones de acceso y SQL permitidos con SQL Firewall para mitigar los riesgos de ataques de inyección SQL

Con el episodio sospechoso de información privilegiada maliciosa, el administrador permite que el firewall SQL en modo de bloqueo no permita ningún intento autorizado por las Naciones Unidas de acceder a información confidencial de los empleados. Descubra cómo SQL Firewall puede aplicar patrones permitidos, incluidas sentencias SQL aprobadas y rutas de conexión a la base de datos, así como alertar sobre posibles ataques de inyección SQL y acceso anómalo a la base de datos de aplicaciones de RR. HH.

Aquí, permitiremos que SQL Firewall bloquee la detección de sentencias/conexiones SQL no autorizadas

1.  Actualizar la aplicación de reglas de lista de permitidos al **modo de bloqueo**
    
        <copy>./sqlfw_allow_list_rule_enable_block.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-022.png "Actualizar la regla de lista de permitidos al modo de bloqueo")
    
    **Nota:** SQL Firewall ahora puede bloquear los intentos de inyección SQL
    
2.  Ahora, un hacker se conecta a la aplicación Glassfish para realizar un ataque de inyección SQL
    
    *   Vuelva a la página web de la aplicación Glassfish, desconéctese y conéctese como _`hradmin`_ con la contraseña "_`Oracle123`_"
        
    *   Haga clic en **Buscar empleados**.
        
        ![SQLFW](./images/sqlfw-010.png "Buscar empleados")
        
    *   Haga clic en \[**Buscar**\]
        
        ![SQLFW](./images/sqlfw-011.png "Buscar empleados")
        
        **Nota**: Todas las filas se devuelven... normal, porque, de nuevo, ha permitido todo.
        
    *   Ahora, marque la **casilla de control "Depurar"** para ver la consulta SQL detrás de este formulario
        
        ![SQLFW](./images/sqlfw-023.png "Ver la consulta SQL ejecutada detrás del formulario")
        
    *   Vuelva a hacer clic en \[**Buscar**\].
        
        ![SQLFW](./images/sqlfw-024.png "Buscar empleados")
        
        **Nota:**
        
        *   Ahora, puede ver la consulta SQL oficial ejecutada por este formulario que muestra los resultados
        *   Esta consulta proporciona la información del número de columnas solicitadas, su nombre, su tipo de dato y su relación
    *   Ahora, en función de esta información, puede crear nuestra consulta de inyección SQL "basada en UNION" para mostrar todos los datos confidenciales que desea extraer directamente del formulario. Aquí, utilizaremos esta consulta para extraer `USER_ID', 'MEMBER_ID', 'PAYMENT_ACCT_NO` y `ROUTING_NUMBER` de la tabla `DEMO_HR_SUPPLEMENTAL_DATA`.
        
            <copy>
            ' UNION SELECT userid, ' ID: '|| member_id, 'SQLi', '1', '1', '1', '1', '1', '1', 0, 0, payment_acct_no, routing_number, sysdate, sysdate, '0', 1, '1', '1', 1 FROM demo_hr_supplemental_data --
            </copy>
            
    *   Copie la consulta de inyección SQL, **péguela directamente en el campo "Posición"** de la pantalla de búsqueda y **marque la casilla de control "Depurar"**.
        
        ![SQLFW](./images/sqlfw-025.png "Copiar/pegar la consulta de inyección SQL")
        
        **Nota:**
        
        *   No olvide "`'`" antes de la palabra clave UNION para cerrar la cláusula SQL "LIKE"
        *   No olvide "`--`" al final para desactivar el resto de la consulta
    *   Haga clic en \[**Buscar**\]
        
        ![SQLFW](./images/sqlfw-026.png "Buscar empleados")
        
        **Nota:**
        
        *   La salida debe devolver un fallo de ORA en estos intentos
        *   Recuerde que esto se debe a que la consulta UNION no se ha agregado a la lista de permitidos en la política de SQL Firewall... tan simple como eso.
3.  Ahora, compruebe los logs de violaciones y los registros de auditoría
    
        <copy>./sqlfw_check_events.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-027.png "Comprobar logs de infracciones y registros de auditoría")
    
    **Nota:** Se produce una infracción SQL que llama la atención de los administradores de seguridad.
    

## Tarea 4: Restablecimiento del entorno de prácticas de SQL Firewall

1.  Una vez que se sienta cómodo con el concepto de firewall SQL, puede restablecer el entorno
    
        <copy>./sqlfw_reset_env.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-050.png "Restablecimiento del entorno de prácticas de SQL Firewall")
    
2.  Migre la cadena de conexión de la aplicación Glassfish para dirigirse a la base de datos por defecto (**pdb1**)
    
        <copy>./sqlfw_glassfish_stop_db23c.sh</copy>
        
    
    ![SQLFW](./images/sqlfw-051.png "Defina HR App con PDB1")
    
    **Nota**: Ahora, conectamos Glassfish a la base de datos **`PDB1`** (DB 19c) en la máquina virtual **`dbsec-lab`**
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

SQL Firewall es una función de seguridad de base de datos incorporada en el núcleo de Oracle Database que inspecciona todas las sentencias SQL entrantes y puede registrar o bloquear sentencias/conexiones SQL que no estén incluidas en la política de listas de permitidos de SQL Firewall. SQL Firewall garantiza que solo se ejecute SQL explícitamente autorizado. Ofrece la mejor protección contra riesgos comunes de la base de datos, como ataques de inyección SQL y cuentas comprometidas.

La inyección SQL es un patrón de ataque de base de datos común para aplicaciones web controladas por datos. Al aprovechar las vulnerabilidades y los fallos de seguridad en las aplicaciones web, los atacantes pueden modificar potencialmente la información de la base de datos, acceder a datos confidenciales, ejecutar tareas de administración en la base de datos, robar credenciales y moverse lateralmente para acceder a otros sistemas confidenciales.

Mientras que otras funciones de seguridad de base de datos embebidas en Oracle Database proporcionan diferentes controles de seguridad para supervisar/evitar dichos ataques de aplicación web, SQL Firewall es el único que inspecciona todas las sentencias SQL entrantes y solo permite SQL autorizado. Registra y bloquea la ejecución de consultas SQL no autorizadas en la base de datos.

SQL Firewall funciona en el núcleo de Oracle Database, en línea con todas las sentencias SQL entrantes independientemente del origen. El firewall SQL puede permitir, registrar y, opcionalmente, bloquear el tráfico SQL cuando detecta una violación de sus reglas.

![SQLFW](./images/sqlfw-concept.png "Concepto de firewall SQL")

Figura 1: Oracle SQL Firewall incorporado en el núcleo de Oracle Database

Las políticas de Oracle SQL Firewall funcionan a nivel de cuenta de base de datos, ya sea de una cuenta de servicio de aplicación o de un usuario directo de base de datos, como un usuario de informes o un administrador de base de datos. La política de firewall SQL para cada cuenta de base de datos consta de listas de permitidos de sentencias SQL autorizadas y rutas de conexión de base de datos de confianza asociadas. El enfoque de lista de permitidos proporciona un mayor nivel de protección contra riesgos como ataques de inyección SQL y cuentas comprometidas. Garantiza que solo se permiten sentencias SQL autorizadas de conexiones de base de datos de confianza para su ejecución dentro de la base de datos Oracle, al tiempo que se alerta/bloquea cualquier intento no autorizado de acceder a los datos confidenciales almacenados en ellos. A diferencia de los mecanismos de protección basados en firmas, el firewall SQL no se puede engañar codificando la sentencia SQL o haciendo referencia a sinónimos o nombres de objetos generados dinámicamente.

Los procedimientos PL/SQL del paquete `SYS.DBMS_SQL_FIREWALL` permiten administrar y gestionar la configuración de SQL Firewall en Oracle Database. Oracle SQL Firewall solo está disponible para Oracle Database Enterprise Edition (versión 23c y posteriores). Oracle SQL Firewall debe tener licencia para su uso. Hay dos rutas a su licencia:

*   Oracle SQL Firewall se incluye con Oracle Database Vault. Database Vault es una opción de costo adicional.
*   Oracle SQL Firewall se incluye con Oracle Audit Vault and Database Firewall (AVDF). AVDF es un producto independiente y requiere una licencia.

### **Ventajas del uso de Oracle SQL Firewall**

*   Proporciona protección en tiempo real contra ataques comunes a la base de datos al restringir el acceso a la base de datos solo a conexiones o sentencias SQL autorizadas.
*   Mitiga los riesgos de ataques de inyección SQL, acceso anómalo y robo/abuso de credenciales.
*   Aplique rutas de conexión de base de datos de confianza.

## Más información

Documentación técnica:

*   [Oracle SQL Firewall 23c](https://docs.oracle.com/en/database/oracle/oracle-database/23/dbseg/using-sql-firewall.html#GUID-F53EAE01-CE78-47F4-80AD-A0091BA3C434)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Angeline Dhanarani
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023